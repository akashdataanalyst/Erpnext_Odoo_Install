# Oddo-With-Docker

# Oddo-With-Docker

🧱 Step 1: Server ready karo (AWS EC2 ya local)
Ubuntu 22.04 use karo
Ports open:
22 (SSH)
80 (HTTP)
443 (HTTPS)
8069 (Odoo)

🐳 Step 2: Docker install karo
sudo apt update
sudo apt install docker.io docker-compose -y


Docker start:

sudo systemctl enable docker
sudo systemctl start docker

Check:

docker --version
docker-compose --version


📁 Step 3: Project folder banao

mkdir odoo-docker
cd odoo-docker

⚙️ Step 4: docker-compose.yml file banao

nano docker-compose.yml

Paste this 👇

version: '3.1'

services:
  web:
    image: odoo:17
    depends_on:
      - db
    ports:
      - "8069:8069"
    environment:
      - HOST=db
      - USER=odoo
      - PASSWORD=odoo
    volumes:
      - odoo-web-data:/var/lib/odoo

  db:
    image: postgres:15
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=odoo
      - POSTGRES_PASSWORD=odoo
    volumes:
      - odoo-db-data:/var/lib/postgresql/data

volumes:
  odoo-web-data:
  odoo-db-data:


▶️ Step 5: Containers run karo
docker-compose up -d

Check:

docker ps
🌐 Step 6: Browser me open karo
http://your-server-ip:8069

👉 Odoo setup page open ho jayega 🎉


sudo docker-compose up -d
sudo docker compose up -d



Option 2 (Permanent Fix – Best Practice)

User ko docker group me add karo:

sudo usermod -aG docker ubuntu

👉 Ab logout/login karo (IMPORTANT)

exit

Phir dubara SSH se login karo.

🧪 Check karo
docker ps

👉 Agar bina sudo chal gaya → ✅ perfect

⚡ Alternative (fast reload without logout)

Agar logout nahi karna:

newgrp docker
🚀 Ab next step

Phir run karo:

docker compose up -d
🧠 Extra Important (Tumhari case me)

Tumne ye command use ki:

docker-compose up -d

👉 But system me new version hai:

docker compose up -d
