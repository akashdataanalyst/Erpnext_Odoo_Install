# Erpnext-with-Docker


sudo apt update && sudo apt upgrade -y
curl -fsSL https://get.docker.com | sudo sh
# Note ubuntu user name hai  "akash" 
sudo usermod -aG docker ubuntu
newgrp docker

sudo apt install git -y
git clone https://github.com/frappe/frappe_docker
cd frappe_docker


cp example.env .env
nano .env

# Add 
# SITES=calcoerp.com
# DB_ROOT_PASSWORD=admin
# ADMIN_PASSWORD=admin

# Note example.env  FILE Main Sites ka naam bhee add karna hai

docker compose -f pwd.yml up -d

# FRAPPE_SITE_NAME_HEADER: calcoerp.com
# Note pwd .env  FILE Main Sites ka naam bhee add karna hai

docker compose -f pwd.yml logs -f create-site

http://YOUR_EC2_PUBLIC_IP:8080
# Local ke liye
http://localhost:8080/

Done

# app Create calco_erp

🔹 Step 1: Check karo app already bana hai ya nahi
docker compose -f pwd.yml exec backend ls apps

👉 Agar output mein calco_erp nahi hai → create karna padega

🔹 Step 2: App create karo (agar nahi hai)
docker compose -f pwd.yml exec backend bench new-app calco_erp
🔹 Step 3: Site pe install karo
docker compose -f pwd.yml exec backend bench --site calcoerp.com install-app calco_erp
🔹 Step 4: Confirm
docker compose -f pwd.yml exec backend bench --site calcoerp.com list-apps

👉 Expected:

frappe
erpnext
calco_erp ✅
⚠️ Important (real-world issue jo log miss karte hain)
🔸 Agar error aaye: ModuleNotFoundError

Run:

docker compose -f pwd.yml restart backend
🔸 Agar migration error aaye
docker compose -f pwd.yml exec backend bench --site calcoerp.com migrate

🔹 1. Actual container name check karo
     docker ps

    Tumhe output milega something like:

    frappe_docker-backend-1
    frappe_docker-frontend-1
    frappe_docker-db-1
 👉 Yaha backend ka exact naam copy karo

🔹 2. Correct command use karo

    Example (agar naam hai frappe_docker-backend-1):

  docker cp frappe_docker-backend-1:/home/frappe/frappe-bench/apps/calco_erp ./


extra : 
1 . docker compose down -v  # Down karne ke liye
2 .docker compose -f pwd.yml up -d # Dubara Start Ke liye

