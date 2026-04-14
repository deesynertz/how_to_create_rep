# Deployment Guide: Deesynertz Website (VPS + Cloudflare + GitHub Actions)

This guide provides a step-by-step walkthrough to deploy your Angular application to a new VPS using GitHub Actions for CI/CD, with Cloudflare managing your domain.

## Prerequisites

1.  **VPS**: A clean Linux VPS (Ubuntu 22.04 or 24.04 recommended).
2.  **Cloudflare**: Your domain added to Cloudflare.
3.  **GitHub Repository**: `axetrixhub/deesynertz-website`.

---

## Step 1: VPS Preparation

Connect to your VPS via SSH and run the following commands:

### 1.1 Install Docker & Docker Compose
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Install Docker Compose (if not included with Docker)
sudo apt install -y docker-compose-plugin
```

### 1.2 Set Up Project Directory
```bash
sudo mkdir -p /app/deesynertz-website
sudo chown -R $USER:$USER /app/deesynertz-website
```

### 1.3 Allow Traffic (Firewall)
If you are using `ufw`:
```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

---

## Step 2: Configure SSH for GitHub Actions

You need to allow GitHub Actions to securely log into your VPS.

### 2.1 Generate SSH Key Pair (on your local machine)
```bash
ssh-keygen -t ed25519 -C "github-actions-deploy" -f ./id_rsa_deploy
```
*   **Private Key**: `./id_rsa_deploy` (Keep this secret!)
*   **Public Key**: `./id_rsa_deploy.pub`

### 2.2 Add Public Key to VPS
Copy the content of `id_rsa_deploy.pub` and add it to `~/.ssh/authorized_keys` on your VPS.

### 2.3 Add Secrets to GitHub
Go to **Settings > Secrets and variables > Actions** in your GitHub repository and add:

| Name | Value |
| :--- | :--- |
| `SERVER_IP` | Your VPS IP Address |
| `SERVER_USER` | Your VPS username (e.g., `root` or `ubuntu`) |
| `SERVER_SSH_KEY` | Content of your **private** key (`id_rsa_deploy`) |

---

## Step 3: Deployment Configuration

The following files are now fully automated:

### 3.1 GitHub Action Workflow
File: `.github/workflows/deploy.yml`
*   **Build**: Builds the production image and pushes to GHCR.
*   **Sync**: **Automatically** copies your `docker-compose.prod.yml` to the server using SCP.
*   **Deploy**: Pulls the new image and restarts the container.

### 3.2 Docker Compose Production
File: `docker-compose.prod.yml`
*   Uses the pre-built image from GHCR.
*   Maps port **80** on the VPS.

---

## Step 4: Validate Before Pushing (Testing)

To ensure your configuration works **before** you deploy, you can run this command in your local terminal:

```bash
# Verify the docker-compose file syntax is valid
docker compose -f docker-compose.prod.yml config
```
*   If it prints out a YAML configuration without errors, it is syntactically correct.
*   The GitHub Action itself acts as a "smoke test"—if any part of the build or deployment fails, you will get a notification, and the server will continue running the previous version without downtime.

---

## Step 5: Cloudflare DNS Configuration

1.  Log in to **Cloudflare**.
2.  Select your domain.
3.  Go to **DNS > Records**.
4.  Add an **A Record**:
    *   **Type**: `A`
    *   **Name**: `@` (or `www`)
    *   **IPv4 address**: Your VPS IP Address.
    *   **Proxy status**: `Proxied` (for DDOS protection and SSL).
5.  Go to **SSL/TLS > Overview**:
    *   Set encryption mode to **Flexible** (since we are only serving port 80 for now).

---

## Step 6: Verify Deployment

1.  **Trigger the Action**: Push a change to the `main` branch.
2.  **Monitor Progress**: Check the **Actions** tab in GitHub.
3.  **Visit Site**: Once the action completes, visit your domain in the browser.

> [!TIP]
> To view logs on the server if things go wrong:
> `docker compose -f /app/deesynertz-website/docker-compose.prod.yml logs -f`
