OPTION 1 (RECOMMENDED) — Keep Docker as main, Nginx for routing

👉 Best production setup

You do this:

Internet
   ↓
Nginx (port 80 + 443)
   ↓
Docker containers (apps)

You do this:

Internet
   ↓
Nginx (port 80 + 443)
   ↓
Docker containers (apps)

BUT RIGHT NOW:
👉 Docker is directly exposing port 80 — this must change.

🛠️ STEP 1 — Check your running container

Run:

docker ps

Look for something like:

0.0.0.0:80->3000/tcp

or:

0.0.0.0:80->80/tcp
🛠️ STEP 2 — Fix Docker port mapping (VERY IMPORTANT)

You must move Docker OFF port 80.

Example fix:

If your container currently runs like this:

docker run -p 80:3000 my-app

👉 Change it to:

docker run -p 3000:3000 my-app

or:

docker run -p 8080:3000 my-app
🛠️ STEP 3 — Restart container
docker stop <container_id>
docker rm <container_id>
docker run -d -p 3000:3000 my-app
🟢 STEP 4 — Now start Nginx

Once port 80 is free:

sudo systemctl start nginx
sudo systemctl enable nginx
🧩 STEP 5 — Configure Nginx as reverse proxy

Now Nginx becomes the ENTRY POINT:



🟢 OPTION A (QUICK FIX — KEEP CURRENT SETUP)

👉 Do nothing to Docker
👉 Do NOT install Nginx for now
👉 Just add subdomain using Docker or another port

But this limits flexibility.

🟢 OPTION B (PRODUCTION BEST PRACTICE — RECOMMENDED)

We restructure properly:

Internet → Nginx (80/443)
            ↓
        Docker apps (3000, 3001, etc.)

Right now you are missing this layer.

🚨 STEP 1 — Fix Docker (IMPORTANT)

You MUST free port 80 from Docker.

Stop container:
docker stop deesynertz-website-prod
Remove it:
docker rm deesynertz-website-prod
Re-run it on internal port (NOT 80):

You need to know your app internal port (likely 3000 or 80 inside container).

Try this common safe setup:

docker run -d \
  --name deesynertz-website-prod \
  -p 3000:80 \
  ghcr.io/axetrixhub/deesynertz-website:main

Now:

Docker internal: 80
Server exposed: 3000
🧪 STEP 2 — Confirm Docker is fixed
docker ps

You should now see:

0.0.0.0:3000->80/tcp

NOT 80 anymore.

🟢 STEP 3 — Install Nginx

Now install safely:

sudo apt update
sudo apt install nginx -y

Start it:

sudo systemctl start nginx
sudo systemctl enable nginx
🧩 STEP 4 — Configure Nginx for your site

Create config:

sudo nano /etc/nginx/sites-available/deesynertz.co.tz

Paste:

server {
    listen 80;
    server_name deesynertz.co.tz www.deesynertz.co.tz;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}

Enable it:

sudo ln -s /etc/nginx/sites-available/deesynertz.co.tz /etc/nginx/sites-enabled/

Test:
sudo nginx -t

Reload:

sudo systemctl reload nginx
🌐 RESULT AFTER FIX
User → Nginx (80)
         ↓
     Docker (3000)
         ↓
  Your React/Website
🚀 WHY THIS IS IMPORTANT

Right now your system is:

❌ Docker = web server
❌ Nginx = not running

After fix:

✅ Nginx = traffic router
✅ Docker = app runtime
✅ Clean scaling for subdomains
