---
title: "Building My Own Private DNS with AdGuard Home"
date: 2025-05-02 00:00:00 +0600
categories: [Homelab, Networking]
tags: [adguard, dns, proxmox, ubuntu, lxc, self-hosting]
summary: "Learn how to set up your own private DNS server using AdGuard Home, hosted on an LXC container in Proxmox. A step-by-step guide for self-hosting enthusiasts."
image:
  path: /assets/img/covers/2025-05-02-adguard-home-dns.jpg
  alt: "Screenshot of AdGuard Home dashboard"
  lqip: data:image/webp;base64,UklGRhYAAABXRUJQVlA4TAYAAAAvAAAAHwAAHwAAQUxQSAwAAAABAAAAAA==
readingTime: 7
author: afrin
---

> *“Why rely on someone else's DNS when you can break your own?”* — Me, before setting up AdGuard Home.

## 🧠 Why Bother with a Private DNS?

In the vast realm of IT, DNS (Domain Name System) is like the unsung hero—translating human-friendly domain names into IP addresses. It's the phonebook of the internet. But trusting third-party DNS providers? That's like letting someone else decide who you can and can't call.

So, I thought, *"Why not build my own DNS server?"* Enter **AdGuard Home**—a network-wide software for blocking ads and tracking. It's like Pi-hole's cousin but with a slicker interface and some extra features.

## 🛠️ Setting Up AdGuard Home: My Way or the Highway

### 1. **Creating the LXC Container**

Using Proxmox, I spun up a bare-metal LXC container. For the OS, I chose **Ubuntu 24.04.2 LTS**—because staying updated is the name of the game.

**Container Specs:**

- **RAM:** 1GB
- **CPU Cores:** Unlimited (but at least 1 is recommended)
- **Swap Memory:** 512MB
- **Disk Size:** 8GB

After the OS installation, it's always a good idea to update the system:

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. **Installing AdGuard Home**

For easier installation you can use [Proxmox Helper Script](https://community-scripts.github.io/ProxmoxVE/scripts?id=adguard). But, I opted for the manual route—for the thrill and the learning experience.

#### Steps:

1. Navigate to the [AdGuard Home GitHub Repository](https://github.com/AdguardTeam/AdGuardHome).
2. Scroll down to the "Getting Started" section.
3. Use the automated installation script:

```bash
curl -s -S -L https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v
```

This script downloads and installs AdGuard Home. Once done, the web interface is accessible at:

```
http://<your-server-ip>:3000
```

Replace `<your-server-ip>` with the actual IP address of your LXC container.

### 🛑 Avoiding Port 53 Conflicts

Ubuntu’s `systemd-resolved` may already be using port 53, which AdGuard needs. To fix this:

1. Open the resolved configuration file:

```bash
sudo nano /etc/systemd/resolved.conf
```

2. Uncomment and set the following:

```ini
DNSStubListener=no
```

3. Restart the `systemd-resolved` service:

```bash
sudo systemctl restart systemd-resolved
```

4. Verify if anything else is using port 53:

```bash
sudo lsof -i :53
```

### 3. **Initial Configuration**

Upon accessing the web interface, you'll be greeted with a setup wizard. It will guide you through:

- Setting up admin credentials.
- Choosing the interfaces AdGuard Home should listen to.
- Configuring DNS settings.

The process is straightforward, and the UI is intuitive.

### 🧩 Troubleshooting Tips

#### Issue: AdGuard Home won’t start
**Fix:** Port 53 might already be in use. Run:
```bash
sudo lsof -i :53
```
Then stop any conflicting service like `systemd-resolved`.

#### Issue: Can’t access the web interface
**Fix:** Double-check the container’s IP and ensure port 3000 is open.

#### Issue: DNS not resolving
**Fix:** Check if AdGuard Home is running and correctly bound to the expected interfaces under **Settings > DNS settings**.

---

### 🔐 Optional: Enable Encryption with Let's Encrypt SSL (DNS-over-HTTPS)

If you want to securely serve DNS traffic over HTTPS (DoH) or TLS, you’ll need a valid SSL certificate. Here’s how I used `certbot` to get a free Let's Encrypt certificate and integrate it with AdGuard Home.

#### 📋 Prerequisites
- A domain name (e.g., `dns.yourdomain.com`)
- A DNS provider with proper DNS records pointing to your server.
- Your LXC container must be reachable from the internet on ports 80/443 during certificate issuance.

#### 🧰 Step-by-Step: Installing certbot and Issuing a Certificate

1. **Install certbot**
```bash
sudo apt update
sudo apt install certbot
```

2. **Issue a Certificate**
Replace `dns.yourdomain.com` with your actual domain:
```bash
sudo certbot certonly --standalone -d dns.yourdomain.com
```
This will automatically handle the certificate issuance process.

3. **Locate the Certificate**
Certbot stores certificates in `/etc/letsencrypt/live/<your-domain>/`. Replace `<your-domain>` with your actual domain name.

4. **Configure AdGuard Home to Use SSL**
Create a directory for your AdGuard certs (if not already):
```bash
sudo mkdir -p /opt/adguardhome/ssl
```
Copy the certificate and key to the AdGuard directory:
```bash
sudo cp /etc/letsencrypt/live/dns.yourdomain.com/fullchain.pem /opt/adguardhome/ssl/cert.pem
sudo cp /etc/letsencrypt/live/dns.yourdomain.com/privkey.pem /opt/adguardhome/ssl/private.key
```

5. **Update AdGuard Home Settings**
1. Open the AdGuard Home web interface.
2. Go to **Settings → Encryption**.
3. Enter your domain (e.g., `dns.yourdomain.com`).
4. Set paths:
   - **Private Key:** `/opt/adguardhome/ssl/private.key`
   - **Certificate:** `/opt/adguardhome/ssl/cert.pem`
5. Save and restart AdGuard Home if needed.

#### 🔁 Automating Renewal
Certbot automatically sets up a cron job for renewal. To manually test it:
```bash
sudo certbot renew --dry-run
```

---

### 🧷 Bonus: Restrict External Access (Optional)

If you're only using DoH locally or in your private network, consider firewall rules to restrict access to ports 443/853 from outside.

### 🐾 Meet Bandittoh: The Debugging Cat

No setup is complete without unexpected hiccups. Thankfully, my cat, Bandittoh, was there to supervise. Every time I made a mistake, he'd let out a disapproving meow. I swear he's more of a sysadmin than I am.

And that's it! Your AdGuard DNS installation is now complete. Enjoy your private DNS server, the satisfaction of self-hosting, and maybe even a debugging cat of your own.
