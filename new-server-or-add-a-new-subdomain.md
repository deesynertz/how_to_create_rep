# **clean production-ready Markdown guide** 

you can reuse every time you set up a new server or add a new subdomain (like `intelligent.deesynertz.co.tz`). It is structured as a repeatable playbook.

---

# 🚀 Deesynertz Server Setup Guide (Production Standard)

This guide explains how to set up a new service on a VPS using:

* Docker (apps)
* Nginx (routing layer)
* Subdomains (multi-service architecture)

---

# 🧱 1. Server Preparation

## 1.1 SSH into server

```bash
ssh root@YOUR_SERVER_IP
```

---

## 1.2 Update system

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 1.3 Install required tools

```bash
sudo apt install -y docker.io docker-compose nginx
```

Enable services:

```bash
sudo systemctl enable docker
sudo systemctl enable nginx
```

---

# 🐳 2. Deploy Application (Docker Layer)

## 2.1 Pull image

```bash
docker pull YOUR_IMAGE
```

---

## 2.2 Run container (IMPORTANT: never use port 80)

### Example:

```bash
docker run -d \
  --name intelligent-app \
  -p 3001:80 \
  YOUR_IMAGE
```

### Rules:

* Each app must use a unique port:

  * main site → 3000
  * intelligent → 3001
  * api → 3002

---

## 2.3 Verify running container

```bash
docker ps
```

Expected:

```
0.0.0.0:3001->80/tcp
```

---

# 🌐 3. DNS Configuration

In your domain provider:

## Add A Record:

```
Type: A
Name: intelligent
Value: YOUR_SERVER_IP
```

Result:

```
intelligent.deesynertz.co.tz → VPS IP
```

---

# ⚙️ 4. Nginx Reverse Proxy Setup

## 4.1 Create config file

```bash
sudo nano /etc/nginx/sites-available/intelligent.deesynertz.co.tz
```

---

## 4.2 Add server block

```nginx
server {
    listen 80;
    server_name intelligent.deesynertz.co.tz;

    location / {
        proxy_pass http://127.0.0.1:3001;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

---

## 4.3 Enable site

```bash
sudo ln -s /etc/nginx/sites-available/intelligent.deesynertz.co.tz /etc/nginx/sites-enabled/
```

---

## 4.4 Test configuration

```bash
sudo nginx -t
```

---

## 4.5 Reload Nginx

```bash
sudo systemctl reload nginx
```

---

# 🔐 5. SSL (HTTPS) Setup (Recommended)

Install certbot:

```bash
sudo apt install certbot python3-certbot-nginx -y
```

Enable HTTPS:

```bash
sudo certbot --nginx -d intelligent.deesynertz.co.tz
```

Auto renewal:

```bash
sudo systemctl enable certbot.timer
```

---

# 🧠 6. Final Architecture

```text
Internet
   ↓
Nginx (80 / 443)
   ↓
┌──────────────────────────────┐
│ intelligent.deesynertz.co.tz │ → localhost:3001
│ deesynertz.co.tz             │ → localhost:3000
│ api.deesynertz.co.tz        │ → localhost:3002
└──────────────────────────────┘
   ↓
Docker Containers
```

---

# 🔁 7. Repeatable Pattern (IMPORTANT)

For every new service:

### Step A — Pick port

```
3000 → main
3001 → intelligent
3002 → api
```

### Step B — Run Docker

```
docker run -p PORT:80
```

### Step C — Create Nginx file

```
/etc/nginx/sites-available/SUBDOMAIN
```

### Step D — Enable + reload

```
ln -s → nginx -t → reload
```

---

# ⚠️ Common Mistakes

* ❌ Using port 80 in Docker
* ❌ Forgetting DNS A record
* ❌ Not reloading Nginx
* ❌ Wrong proxy_pass port
* ❌ Missing SSL setup

---

# 🚀 If you want next upgrade

I can extend this into a full **DevOps production system**, including:

* GitHub Actions auto deploy
* Zero downtime deployments
* Docker Compose orchestration
* Central logging
* Multi-server scaling

Just say:
👉 “make this enterprise ready”
