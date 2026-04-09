🔷 STEP 1: VS Code install + extensions

VS Code open karo aur ye extensions install karo:

✅ Required Extensions:
Docker
Dev Containers (by Microsoft)


🔷 STEP 2: Docker container run hona chahiye
Terminal me check karo:

docker ps

👉 Tumhe ye container dikhna chahiye:

frappe_docker-backend-1

Agar nahi dikh raha:

docker compose -f pwd.yml up -d
🔷 STEP 3: VS Code ko container se connect karo

ubuntu mai Code . 
VS Code open karo
Press:
Ctrl + Shift + P
Type karo:
Dev Containers: Attach to Running Container
Select karo:
frappe_docker-backend-1

⏳ 5–10 sec lagega connect hone me

🔷 STEP 4: App folder open karo

Container connect hone ke baad:

VS Code me “Open Folder”
Path daalo:
/home/frappe/frappe-bench/apps/calco_erp

👉 Ab tumhara pura app VS Code me open ho gaya 🎯

🔷 STEP 5: Live editing test karo
Example:
File open karo:
calco_erp/hooks.py
Ya koi JS / Python file edit karo
Save karo (Ctrl + S)
🔷 STEP 6: Browser me check karo

Open:

http://localhost:8080

👉 Hard refresh:

Ctrl + Shift + R
🔷 STEP 7: Agar change nahi dikha toh

Container terminal (VS Code ke andar) me run karo:

bench clear-cache
🔷 STEP 8: Kab restart karna padega?

Agar ye change kiya hai:

Change	Action
JS / UI	सिर्फ refresh
Python logic	restart
hooks.py	restart
Restart command:
docker compose -f pwd.yml restart backend
🔥 BONUS (IMPORTANT)
Developer mode ON karo

Container terminal me:

bench --site calcoerp.com set-config developer_mode 1
🔷 FINAL FLOW (YAAD RAKHO)
VS Code (container se connected)
↓
Edit files
↓
Save
↓
Browser refresh
↓
Live changes 🚀
❗ Common mistakes (avoid karo)

❌ Windows folder edit karna (container se alag)
❌ export-fixtures bhool jana
❌ module galat select karna

✅ Ab tum kya karo
VS Code connect karo container se
calco_erp open karo
Ek file edit karo
Browser me check karo

Agar kahin atak jao toh screenshot bhej dena
👉 main turant bata dunga kya issue hai (live debugging 👍)
Dev Containers (important)




git add .
git commit -m "Initial commit"
git branch -M main
git push -u origin main
