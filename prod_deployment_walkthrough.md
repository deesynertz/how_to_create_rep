# Production Deployment Walkthrough

This guide outlines the steps to deploy the Platform on your VPS example (`192.168.100.0`) using host-installed PostgreSQL and Dockerized microservices.

## 1. VPS Preparation & PostgreSQL Installation

Since you want to run PostgreSQL directly on the host, follow these steps to install and secure it.

### Install PostgreSQL
SSH into your VPS and run:
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib -y
sudo systemctl enable postgresql
sudo systemctl start postgresql
```

### Configure User & Databases
Switch to the postgres user and create the necessary databases:
```bash
sudo -u postgres psql
```
Inside the SQL prompt, run:
```sql
-- Create Users
CREATE USER admin WITH PASSWORD 'YOUR_SECURE_PASSWORD';

-- Create Databases
CREATE DATABASE main;
CREATE DATABASE intelligence;

-- INTO DATABASE
\c main | ntelligence

-- Grant Privileges
GRANT ALL PRIVILEGES ON DATABASE main TO admin;
GRANT ALL PRIVILEGES ON DATABASE ntelligence TO admin;

\q
```

### Allow Network Connections
By default, Postgres only listens on `localhost`. To allow Docker containers to connect, update the configuration:

1. Edit `/etc/postgresql/16/main/postgresql.conf`:
   ```bash
   # Set listen_addresses to '*' or your bridge gateway (usually 172.17.0.1)
   listen_addresses = '*'
   ```
2. Edit `/etc/postgresql/16/main/pg_hba.conf` to allow the Docker subnet:
   ```text
   # Allow all connections from the Docker bridge network
   host    all     all     172.17.0.0/16     scram-sha-256
   ```
3. Restart Postgres:
   ```bash
   sudo systemctl restart postgresql
   ```

---

## 2. Platform Infrastructure Configuration

We will now create the production Docker Compose file and environment configuration.

### Create `.env.prod`
Create a file named `.env.prod` on your VPS (or in your CD pipeline).
> [!IMPORTANT]
> Change the `DB_HOST` to the IP address of your Docker Bridge (usually `172.17.0.1`).

```env
NODE_ENV=production
GATEWAY_PORT=8105

# Database (Main)
MAIN_DB_USER=admin
MAIN_DB_PASSWORD=YOUR_SECURE_PASSWORD
MAIN_DB_NAME=main
DATABASE_URL=postgres://admin:YOUR_SECURE_PASSWORD@172.17.0.1:5432/main

# Database (Intelligence)
INTEL_DB_USER=admin
INTEL_DB_PASSWORD=YOUR_SECURE_PASSWORD
INTEL_DB_NAME=intelligence
INTEL_DATABASE_URL=postgres://admin:YOUR_SECURE_PASSWORD@172.17.0.1:5432/intelligence

# API/AI Engine Secrets
DEESYNERTZ_API_KEY=your-prod-api-key
JWT_SECRET=your-prod-jwt-secret
MOCK_AI=false
```

---

## 3. Reverse Proxy Setup (Nginx Host)

Since you already have domain like `deesynertz.co.tz` running on the host, you need to add a new server block for the subdomain.

### Create Nginx Config
Create `/etc/nginx/sites-available/intelligent.deesynertz.co.tz`:
```nginx
server {
    listen 80;
    server_name intelligent.deesynertz.co.tz;

    location / {
        proxy_pass http://localhost:8105; # Matches GATEWAY_PORT in .env.prod
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Enable & Apply SSL
```bash
sudo ln -s /etc/nginx/sites-available/intelligent.deesynertz.co.tz /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

# Install SSL via Certbot
sudo certbot --nginx -d intelligent.deesynertz.co.tz
```

---

## 4. Deployment Steps

On your VPS, you only need the `docker-compose.prod.yml` and `.env.prod` files.

1. **Login to GHCR**:
   ```bash
   echo $CR_PAT | docker login ghcr.io -u YOUR_GITHUB_USERNAME --password-stdin
   ```
2. **Pull & Start**:
   ```bash
   docker compose -f infrastructure/docker-compose.prod.yml --env-file .env.prod up -d
   ```

---

## Next Steps
1. Review the generated [docker-compose.prod.yml](file:///Users/deesynertz/Projects/Deesynertz/freelancer/DEESYNERT-GROUP/deesynertz-platform-dev/infrastructure/docker-compose.prod.yml).
2. Ensure your Github Actions are pushing images to `ghcr.io/axetrixhub/deesynertz-api-engine:main`, etc.
3. Configure the firewall (`ufw`) on your VPS to allow port 5432 **only** from the Docker network.
