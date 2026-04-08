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



# Note pwd .env  FILE Main Sites ka naam bhee add karna hai
# bench new-site calcoerp.com --mariadb-user-host-login-scope='%' --admin-password=admin --db-root-                username=root --db-root-password=admin --install-app erpnext --set-default
# FRAPPE_SITE_NAME_HEADER: calcoerp.com

docker compose -f pwd.yml up -d
docker compose -f pwd.yml logs -f create-site

http://YOUR_EC2_PUBLIC_IP:8080
# Local ke liye
http://localhost:8080/

Done

# Docker Ke container Check ke liye 
docker ps

# frappe_docker-backend-1
   frappe_docker-db-1
    frappe_docker-frontend-1
👉 Hume backend container use karna hai.

# Container Ke undar jane ke liye 
docker exec -it frappe_docker-backend-1 bash

# Install app Check karne ke liye 
bench list-apps
# output
# frappe  16.14.0 UNVERSIONED
# erpnext 16.13.0 UNVERSIONED

# Agar tumhe kisi specific site (jaise calcoerp.com) ke apps dekhne hain:
bench --site calcoerp.com list-apps

# app Folder bhee use kar sakte hai 
cd frappe-bench/apps
ls

# Agar baar-baar container ke andar nahi jana: not compulsory
docker exec -it frappe_docker-backend-1 bench list-apps

# Ya specific site ke liye: not compulsory
docker exec -it frappe_docker-backend-1 bench --site calcoerp.com list-apps

# app Create calco_erp

🔹 Step 1: Check karo app already bana hai ya nahi
docker compose -f pwd.yml exec backend ls apps

👉 Agar output mein calco_erp nahi hai → create karna padega

🔹 Step 2: App create karo (agar nahi hai)
 # Container ke undar ho toh 
 bench new-app calco_erp
 # bahar ho toh
docker compose -f pwd.yml exec backend bench new-app calco_erp
🔹 Step 3: Site pe install karo
 # Container ke undar ho toh
 bench --site calcoerp.com install-app calco_erp
 # bahar ho toh
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


# Note Custom App ko bahar nikala container se tabhi deply hota hai aws pr 
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

