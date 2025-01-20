# 🍓 Raspberry Pi Server Setup with 🐳 Docker and 🛳️ Portainer

## **📝 Overview**
This guide outlines a lean and minimal process to set up a Raspberry Pi server with 🌐 Nginx, 🐘 PHP, 🐳 Docker, and 🛳️ Portainer, ensuring 🔒 security and simplicity.

---

## **1️⃣ Prepare the System**
1. 🔄 Update and upgrade the system:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. 🛠️ Install essential tools:
   ```bash
   sudo apt install curl wget ufw -y
   ```

---

## **2️⃣ Set Up Web Environment**
1. 🌐 Install Nginx:
   ```bash
   sudo apt install nginx -y
   ```
2. 🐘 Install PHP for dynamic content:
   ```bash
   sudo apt install php-fpm -y
   ```
3. ✅ Test Nginx:
   - Visit your server’s 📡 IP in a browser to see the default Nginx page.
4. 🛠️ Configure PHP with Nginx:
   - ✍️ Edit the default site configuration:
     ```bash
     sudo nano /etc/nginx/sites-available/default
     ```
   - Add this line in the `location` block:
     ```nginx
     location ~ \.php$ {
         include snippets/fastcgi-php.conf;
         fastcgi_pass unix:/run/php/php7.4-fpm.sock; # Adjust version as needed
     }
     ```
   - 🔄 Restart Nginx:
     ```bash
     sudo systemctl restart nginx
     ```

---

## **3️⃣ Install 🐳 Docker**
1. 📥 Install Docker using the official script:
   ```bash
   curl -fsSL https://get.docker.com | sudo bash
   ```
2. 👥 Add your user to the `docker` group:
   ```bash
   sudo usermod -aG docker $USER
   ```
3. 🔄 Restart your session or system to apply changes.
4. ✅ Verify Docker is working:
   ```bash
   docker ps
   ```

---

## **4️⃣ Deploy 🛳️ Portainer**
1. 📥 Pull and run Portainer:
   ```bash
   docker run -d -p 9000:9000 --name=portainer --restart=always \
   -v /var/run/docker.sock:/var/run/docker.sock \
   -v portainer_data:/data portainer/portainer-ce:latest
   ```
2. ✅ Verify Portainer is running:
   ```bash
   docker ps
   ```
3. 🌐 Access Portainer:
   - Open your browser and go to:
     ```
     http://<server-ip>:9000
     ```

---

## **5️⃣ 🔒 Security Updates**
1. 🔥 Set up a firewall with `ufw`:
   ```bash
   sudo ufw allow 22
   sudo ufw allow 80
   sudo ufw allow 9000
   sudo ufw enable
   ```
2. 🔐 Consider securing Portainer with Nginx reverse proxy and SSL:
   - Use Let's Encrypt to obtain a certificate.
   - Configure Nginx to proxy traffic to Portainer securely.

---

## **6️⃣ Ensure 🛳️ Portainer Auto-Starts**
1. 🔄 Confirm the restart policy for Portainer:
   ```bash
   docker inspect portainer --format='{{.HostConfig.RestartPolicy.Name}}'
   ```
   - If not `always`, update it:
     ```bash
     docker update --restart=always portainer
     ```
2. 🔁 Reboot the system and verify:
   ```bash
   sudo reboot
   ```
   - After reboot, check:
     ```bash
     docker ps
     ```

---

## **🐞 Key Debugging Tips**
- **If 🐳 Docker commands show permission errors**:
  - Ensure your user is in the `docker` group and restart the session.
- **If 🛳️ Portainer isn’t accessible**:
  - Check if the container is running (`docker ps`).
  - Verify the firewall allows port 9000.
  - Ensure no other services are using port 9000.

