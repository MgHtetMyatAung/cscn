---
slug: docker-in-frontend
title: Docker in Frontend
authors: [MrBoss]
# tags: [facebook, hello, docusaurus]
---

# 🐳 Docker And Frontend Development (Basic)

ဒီနေ့မှာတော့ Frontend Developer တွေအတွက် တကယ့်လက်တွေ့လုပ်ငန်းခွင် (Working Level) မှာ မရှိမဖြစ်သိထားသင့်တဲ့ **"Docker in Frontend"** အကြောင်းကို အခြေခံကနေစပြီး ရှင်းပြပေးသွားမှာ ဖြစ်ပါတယ်။

---

### ၁။ Frontend Developer တစ်ယောက်အနေနဲ့ Docker ကို ဘာလို့ သိထားသင့်တာလဲ?

Frontend Developer တော်တော်များများက _"Docker က Backend သမားတွေ၊ DevOps သမားတွေနဲ့ပဲ ဆိုင်တယ်"_ လို့ ထင်တတ်ကြပါတယ်။ တကယ်တော့ မဟုတ်ပါဘူး။

လုပ်ငန်းခွင်မှာ အများဆုံးကြုံရတဲ့ ပြဿနာက **"It works on my machine"** (ငါ့စက်မှာတော့ run လို့ရတယ်၊ ဟိုဘက်စက်ရောက်မှ error တက်နေတယ်) ဆိုတဲ့ ပြဿနာပါ။

- Node Version မတူတာတွေ၊
- OS (Windows/Mac/Linux) မတူလို့ npm install လုပ်ရင် error တက်တာတွေ၊
- Dependencies အနိမ့်အမြင့် ပြဿနာတွေ၊

ဒါတွေကို ဖြေရှင်းဖို့ Docker က ကူညီပေးပါတယ်။ Docker သုံးလိုက်ရင် သင့်ရဲ့ Code က သင့်စက်မှာ ဘယ်လို Run လဲ၊ Server ပေါ်မှာလည်း အဲ့ဒီအတိုင်း တစ်ထပ်တည်း Run မှာဖြစ်ပါတယ်။

---

### ၂။ Docker ရဲ့ အဓိက Concept များ (လွယ်ကူစွာ မှတ်သားရန်)

Docker ကို နားလည်ဖို့ ဒီ (၂) ခုကို အရင်ကွဲကွဲပြားပြား သိထားရပါမယ်။

- **Docker Image (Blueprint/Recipe):** ဒါက ဟင်းချက်နည်း (Recipe) စာအုပ်နဲ့ တူပါတယ်။ Code တွေ၊ Library တွေ၊ Environment Setting တွေ အားလုံးကို "Image" တစ်ခုအနေနဲ့ ထုပ်ပိုးထားတာပါ။ ပြင်လို့မရပါဘူး (Read-only)။
- **Docker Container (Running App):** ဒါကတော့ ဟင်းချက်နည်းအတိုင်း ချက်ထားတဲ့ "ဟင်းပွဲ" အစစ်ပါ။ Image ကို Run လိုက်တဲ့အခါ Container ဖြစ်လာပါတယ်။ Container တစ်ခုနဲ့တစ်ခု သီးခြားစီ (Isolated) Run နေကြတာပါ။

![Docusaurus logo](/img/docker.png)

---

### ၃။ Frontend Project (React/Vue) တစ်ခုကို Dockerize လုပ်ကြည့်ရအောင်

သာမန် Frontend Project တစ်ခုအတွက် `Dockerfile` ရေးနည်းကို အဆင့်ဆင့် ကြည့်ကြမယ်။ Project ရဲ့ Root Folder ထဲမှာ `Dockerfile` (extension မပါပါ) ဆိုတဲ့ ဖိုင်တစ်ခု ဆောက်လိုက်ပါ။

#### **Step 1: Dockerfile ရေးသားခြင်း**

```dockerfile
# 1. Base Image ကို ရွေးမယ် (Nodejs environment လိုချင်လို့ node image ကိုယူမယ်)
# working level မှာ version အတိအကျ သတ်မှတ်တာ ပိုကောင်းပါတယ် (ဥပမာ - node:18-alpine)
FROM node:18-alpine

# 2. Container ထဲမှာ အလုပ်လုပ်မယ့် Folder (Directory) ကို သတ်မှတ်မယ်
WORKDIR /app

# 3. Dependencies တွေကို အရင် copy ကူးမယ် (Caching မိအောင်လို့ပါ)
COPY package.json package-lock.json ./

# 4. Library တွေ သွင်းမယ်
RUN npm install

# 5. ကျန်တဲ့ Code တွေ အကုန်လုံးကို ကူးထည့်မယ်
COPY . .

# 6. Port ကို ဖွင့်ပေးမယ် (React/Vite ပေါ်မူတည်ပြီး 3000 or 5173 ပြောင်းနိုင်ပါတယ်)
EXPOSE 3000

# 7. Container စ run တာနဲ့ လုပ်ဆောင်မယ့် Command
CMD ["npm", "run", "dev"]
```

> **Mr. Kan's Note:** `alpine` version ကို သုံးတာက Image size ကို သေးစေပြီး ပေါ့ပါးစေလို့ လုပ်ငန်းခွင်မှာ အသုံးများပါတယ်။

---

### ၄။ မရှိမဖြစ် သိထားရမယ့် Basic Commands များ

Terminal (သို့) CMD မှာ အောက်ပါ Command တွေကို ရိုက်ပြီး စမ်းသပ်နိုင်ပါတယ်။

**၁. Image ဆောက်မယ် (Build)**
ပထမဆုံး ရေးထားတဲ့ Dockerfile ကို Image အဖြစ် ပြောင်းပါမယ်။

```bash
# -t ဆိုတာ tag (နာမည်ပေးတာပါ), (.) ဆိုတာ လက်ရှိ folder ကို ပြောတာပါ
docker build -t my-frontend-app .
```

**၂. App ကို Run မယ် (Run)**
Image ရပြီဆိုရင် Container အနေနဲ့ run ပါမယ်။

```bash
# -p 3000:3000 ဆိုတာ ကိုယ့်စက်က port 3000 နဲ့ container ထဲက port 3000 ကို ချိတ်တာပါ
docker run -p 3000:3000 my-frontend-app
```

ဒါဆိုရင် Browser မှာ `localhost:3000` နဲ့ ဝင်ကြည့်လို့ ရပါပြီ။

**၃. Run နေတဲ့ Container တွေကို ကြည့်မယ်**

```bash
docker ps
```

**၄. Container ကို ရပ်မယ်**

```bash
docker stop <CONTAINER_ID>
```

---

### ၅။ Senior Level Tips (လုပ်ငန်းခွင် အတွေ့အကြုံ)

လုပ်ငန်းခွင်မှာ "Working Level" အနေနဲ့ ဒီအချက်တွေကိုပါ ထပ်ဖြည့်သိထားရင် ပိုကောင်းပါတယ်။

1.  **`.dockerignore` ဖိုင်:** Git မှာ `.gitignore` ရှိသလို Docker မှာလည်း ရှိပါတယ်။ `node_modules` folder ကြီးတစ်ခုလုံးကို Docker image ထဲ copy မကူးထည့်မိအောင် `.dockerignore` ဖိုင်ထဲမှာ `node_modules` လို့ ထည့်ရေးထားသင့်ပါတယ်။ ဒါမှ Image build တာ ပိုမြန်ပါလိမ့်မယ်။

2.  **Multi-stage Build (Production အတွက်):**
    အပေါ်က `CMD ["npm", "run", "dev"]` က Development အတွက်ပဲ ကောင်းပါတယ်။ Production (User တွေသုံးဖို့) အတွက်ဆိုရင်တော့ Nginx နဲ့ တွဲသုံးတာက Standard ပါ။

    - Stage 1: Node နဲ့ `npm run build` လုပ်ပြီး HTML/CSS/JS file တွေ ထုတ်မယ်။
    - Stage 2: ရလာတဲ့ file တွေကို Nginx image ထဲ ကူးထည့်ပြီးစာ Run မယ်။ (ဒါက File size ကို အများကြီး သေးသွားစေပါတယ်)။

---

**နိဂုံးချုပ်**
Docker ဟာ Frontend Developer တွေအတွက် အစပိုင်းမှာ နည်းနည်းရှုပ်တယ်လို့ ထင်ရပေမယ့်၊ တကယ်တမ်း သုံးတတ်သွားရင် Environment setting ချိန်ရတဲ့ ခေါင်းကိုက်မှုတွေကို ၉၀% လောက် လျှော့ချပေးနိုင်ပါတယ်။

ဒီ guide လေးကို ဖတ်ပြီး လက်တွေ့ စမ်းလုပ်ကြည့်ဖို့ တိုက်တွန်းချင်ပါတယ်။
