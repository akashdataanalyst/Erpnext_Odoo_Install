git remote set-url origin https://github.com/akashdataanalyst/Aws_deploy.git
git add .
git commit -m "Initial commit"
git branch -M main
git push -u origin main

# install app ko uninstall ke liye 
bench --site calcoerp.com uninstall-app maintenance_management

# install app delete karne ke liye 
rm -rf apps/maintenance_management

# Contaniner mai app add karna leke aana , migrate karna 

# cd /home/frappe/frappe-bench/apps

bench get-app https://github.com/akashdataanalyst/maintenance.git

# install 
# cd /home/frappe/frappe-bench
bench --site calcoerp.com install-app maintenance_management

# migrate
bench --site calcoerp.com  migrate


# Deployment 2

## 1. App Mapping

Do apps hain:

- `maintenance_management`
  Directory: `C:\Users\akash\OneDrive\Desktop\erpnext-maintenance`
- `calco_erp`
  Directory: `C:\Users\akash\OneDrive\Desktop\erpnext-maintenance\calco_erp`

Simple rule:

- root folder push karoge = `maintenance_management`
- `calco_erp` folder push karoge = `calco_erp`

## 2. Local Push: `maintenance_management`

```powershell
cd C:\Users\akash\OneDrive\Desktop\erpnext-maintenance
git status
git add .
git commit -m "Update maintenance_management"
git push -u origin main
```

## 3. Local Push: `calco_erp`

```powershell
cd C:\Users\akash\OneDrive\Desktop\erpnext-maintenance\calco_erp
git status
git add .
git commit -m "Update calco_erp"
git push -u origin main
```

## 4. AWS Par Dono Apps Install

```bash
cd /home/frappe/frappe-bench
bench get-app https://github.com/<username>/maintenance_management.git
bench get-app https://github.com/<username>/calco_erp.git
bench --site <site-name> install-app maintenance_management
bench --site <site-name> install-app calco_erp
bench --site <site-name> migrate
```
# main 

docker compose restart 
## 5. Docker Me Kya Karna Hai

Docker me best rule:

- `git push` local machine se karo
- running container ke andar normal `git push` mat karo
- app ko image build ke time lao ya copy karo
- `bench migrate` backend container ke andar chalao

Current project ke Dockerfile me app image build ke time aa raha hai.
Matlab current pattern me running container ke andar manual clone karna primary method nahi hai.

Clone / get-app kahan hota hai:

- `bench get-app` hamesha bench root se chalao: `/home/frappe/frappe-bench`
- app folders automatically `apps/` ke andar aate hain
- final path usually ye hoga:
  - `/home/frappe/frappe-bench/apps/maintenance_management`
  - `/home/frappe/frappe-bench/apps/calco_erp`

`git push` kahan se hota hai:

- `git push` local machine se hota hai, AWS server se nahi
- pehle apne system se GitHub par push karo
- phir AWS/container/server par `bench get-app` ya `git pull` karo

Container ke andar ya bahar:

- agar Frappe Docker/container ke andar chal raha hai, to `bench`, `git pull`, `migrate` wahi environment me chalao jahan bench installed hai
- agar normal VM/server install hai, to host server par `/home/frappe/frappe-bench` ke andar chalao

## 6. Docker Me Fresh Install

Fresh install ka simple Docker flow:

1. Local se GitHub par code push karo
2. Server par latest code lao ya build context update karo
3. Docker image rebuild karo
4. Containers start/update karo
5. Backend container me `install-app` aur `migrate` chalao

Example:

```bash
docker compose build
docker compose up -d
docker compose exec backend bench --site <site-name> install-app maintenance_management
docker compose exec backend bench --site <site-name> install-app calco_erp
docker compose exec backend bench --site <site-name> migrate
```

## 7. Docker Me Update Karna Ho

```bash
docker compose build
docker compose up -d
docker compose exec backend bench --site <site-name> migrate
```

Iska matlab:

1. Latest code ke saath image rebuild hoti hai
2. Containers updated image se restart hote hain
3. Backend container ke andar `migrate` database changes apply karta hai

Kab use karna hai:

- jab app pehle se install hai aur sirf code update hua hai

Fresh install me `install-app` bhi chalega, update case me usually sirf `migrate` chalega.

## 8. Agar Container Ke Andar Manual Clone Karna Hi Ho

Usually manual clone yahan hota hai:

```bash
cd /home/frappe/frappe-bench/apps
git clone https://github.com/<username>/maintenance_management.git
git clone https://github.com/<username>/calco_erp.git
```

Uske baad:

```bash
docker compose exec backend bench --site <site-name> install-app maintenance_management
docker compose exec backend bench --site <site-name> install-app calco_erp
docker compose exec backend bench --site <site-name> migrate
```

Lekin Docker me ye method long-term best nahi hai, kyunki container recreate hone par manual changes lose ho sakte hain.

Best method:

- app image build me include karo
- phir container recreate karo
- phir backend container me migrate chalao

## 9. Important Notes

- Dono apps ke liye alag GitHub repo rakhna best hai.
- Ek Git URL se Bench normally ek hi app uthata hai.
- Same site par dono features chahiye to dono `install-app` karne padenge.
- `calco_erp/calco_erp` normal structure hai. Ye do apps nahi hain.

## 10. Final Order

1. Root folder se `maintenance_management` push karo.
2. `calco_erp` folder se `calco_erp` push karo.
3. Server par Docker image rebuild karo.
4. Containers up karo.
5. Backend container me `install-app` ya `migrate` chalao.

