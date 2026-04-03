# Erpnext-with-Docker


sudo apt update && sudo apt upgrade -y
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker ubuntu
newgrp docker

sudo apt install git -y
git clone https://github.com/frappe/frappe_docker
cd frappe_docker

docker compose -f pwd.yml up -d

docker compose -f pwd.yml logs -f create-site

http://YOUR_EC2_PUBLIC_IP:8080

