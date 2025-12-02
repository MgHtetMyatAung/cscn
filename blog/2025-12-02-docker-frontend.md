---
slug: docker-and-frontend
title: Docker And Frontend
authors: [MrBoss]
# tags: [facebook, hello, docusaurus]
---

# 🐳 Docker And Frontend Development (Basic)

မင်္ဂလာပါ! ကျွန်တော်က မစ္စတာကန်ပါ။ Frontend Developer တစ်ယောက်အနေနဲ့ Docker ကို ဘာကြောင့် သုံးသင့်လဲ၊ ဘယ်လို အကျိုးကျေးဇူးတွေ ရနိုင်လဲဆိုတာကို အခြေခံကနေ မြန်မာလို ရှင်းပြပေးပါမယ်။

---

## 🧐 What is Docker?

Docker ဆိုတာက သင့်ရဲ့ Application ကို (Frontend Code, Backend Code, Database စတာတွေကို) **Container** ဆိုတဲ့ သေတ္တာငယ်လေးတွေထဲ ထုပ်ပိုးပြီး အလွယ်တကူ Run ဖို့ ကူညီပေးတဲ့ Platform တစ်ခုပါ။

**သဘောတရား:** သင့်ရဲ့ ကွန်ပျူတာမှာပဲဖြစ်ဖြစ်၊ သူငယ်ချင်းရဲ့ ကွန်ပျူတာမှာပဲဖြစ်ဖြစ်၊ Cloud Server မှာပဲဖြစ်ဖြစ် — ဘယ်နေရာမှာ Run run — အလုပ်လုပ်ပုံ၊ လိုအပ်တဲ့ Software တွေ၊ Environment တွေအားလုံး အမြဲတမ်း **တစ်ထပ်တည်း တူညီနေစေဖို့** လုပ်ပေးပါတယ်။

**Frontend အတွက် ဘာလို့ အရေးကြီးလဲ:** Frontend Application တစ်ခု Run ဖို့အတွက် Node.js Version၊ npm Package တွေ လိုအပ်ပါတယ်။ သင့်သူငယ်ချင်းက Node Version တစ်မျိုးသုံးပြီး၊ သင်က တစ်မျိုးသုံးရင် **"Works on my machine"** (ငါ့ဆီမှာတော့ အလုပ်လုပ်တယ်) ဆိုတဲ့ ပြဿနာမျိုးတွေ ကြုံရတတ်ပါတယ်။ Docker က အဲဒီပြဿနာကို ဖြေရှင်းပေးပါတယ်။

---

## 🌟 Benefits for Frontend Devs

### ၁။ Environment Consistency

- Team ထဲက လူတိုင်း Node.js, npm/yarn တို့ရဲ့ Version တူ ကိုပဲ သုံးနေစေဖို့ သေချာစေပါတယ်။
- သင့် Code ကို Build (သို့) Run လုပ်တဲ့အခါတိုင်း ရလဒ်က အမြဲတမ်း တူနေပါမယ်။

### ၂။ Dependency Management

- သင့်ရဲ့ Frontend Project မှာ လိုအပ်တဲ့ Node.js လိုမျိုး Software တွေကို သင့်ရဲ့ ကွန်ပျူတာမှာ တိုက်ရိုက် Install လုပ်စရာ မလိုတော့ပါဘူး။ အားလုံး Container ထဲမှာ ရှိနေမှာပါ။

### ၃။ Simple Setup

- Project အသစ်တစ်ခု စဝင်တဲ့ Developer အနေနဲ့ Docker သုံးထားရင် မိနစ်ပိုင်းအတွင်းမှာပဲ Project ကို အလုပ်လုပ်ဖို့ Ready ဖြစ်အောင် Setup လုပ်နိုင်ပါတယ်။

### ၄။ Production Deployment

- သင့်ရဲ့ Build လုပ်ပြီးသား Static HTML/CSS/JS ဖိုင်တွေကို Nginx လို Web Server တစ်ခုနဲ့ Container ထဲမှာ ထုပ်ပိုးပြီး Production Server ကို လွယ်လွယ်ကူကူ Deploy လုပ်နိုင်ပါတယ်။

---

## 🛠️ Basic Terminologies

| အသုံးအနှုန်း (Term) | မြန်မာလို ရှင်းလင်းချက် (Explanation)                                                                                                                                               |
| :------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Dockerfile**      | Container တစ်ခုကို ဘယ်လို တည်ဆောက်ရမယ်ဆိုတဲ့ အဆင့်ဆင့် ညွှန်ကြားချက်စာအုပ်။ ဥပမာ - Node.js ဘယ် Version သုံးရမယ်၊ Code တွေ ဘယ်ကို ကူးထည့်ရမယ်၊ ဘယ် Command Run ရမယ်ဆိုတာတွေ ရေးရတယ်။ |
| **Image**           | Dockerfile ကို Compile လုပ်လိုက်ရင် ရလာတဲ့ Template/ပုံစံခွက်။ ဒီဟာကို သုံးပြီး Container တွေကို ဖန်တီးနိုင်တယ်။                                                                    |
| **Container**       | Image ကို Run လိုက်လို့ အလုပ်လုပ်နေတဲ့ သေတ္တာငယ်။ သင့်ရဲ့ Frontend Application (ဥပမာ- React App) ကို အထဲမှာ Run နေတာကို ဆိုလိုတယ်။                                                  |
| **Docker Compose**  | Frontend, Backend, Database စတဲ့ Container အများအပြား ကို တစ်ပြိုင်နက်တည်း အလွယ်တကူ စီမံခန့်ခွဲပြီး Run ဖို့ ကူညီပေးတဲ့ Tool တစ်ခု။                                                 |

---

## 📝 Sample Dockerfile for Frontend

အောက်ပါ နမူနာ Dockerfile က သင့်ရဲ့ React/Vue/Angular Frontend Project တစ်ခုကို ဘယ်လို Build လုပ်ပြီး၊ Nginx Web Server နဲ့ ဘယ်လို Run မလဲဆိုတာ ပြထားပါတယ်။

```dockerfile
# 1. Build Stage: Project ကို Build လုပ်ဖို့ Node Environment ကို သုံးမယ်
FROM node:18-alpine AS build

# /app ကို Working Directory အဖြစ် သတ်မှတ်မယ်
WORKDIR /app

# package.json နဲ့ package-lock.json တွေကို ကူးထည့်မယ်
COPY package.json package-lock.json ./

# Dependency တွေကို Install လုပ်မယ်
RUN npm install

# ကျန်တဲ့ Code တွေအားလုံးကို ကူးထည့်မယ်
COPY . .

# Frontend Project ကို Build လုပ်မယ် (build ဖိုင်တွေထွက်လာဖို့)
RUN npm run build

# 2. Production Stage: Web Server (Nginx) နဲ့ Run ဖို့
# အရွယ်အစား သေးငယ်ပြီး မြန်ဆန်တဲ့ Nginx ကို သုံးမယ်
FROM nginx:alpine

# Build Stage ကနေ ထွက်လာတဲ့ build ဖိုင်တွေကို Nginx ရဲ့ default path ထဲ ကူးထည့်မယ်
COPY --from=build /app/build /usr/share/nginx/html

# Container က port 80 ကို အပြင်ကို ထုတ်ပေးမယ်
EXPOSE 80

# Container စRun ရင် Nginx ကို စတင်ဖို့ Command ပေးမယ်
CMD ["nginx", "-g", "daemon off;"]
```

### 💻 Docker Commands များ (Common Docker Commands)

Project ကို စီမံခန့်ခွဲရာမှာ အသုံးများတဲ့ အခြေခံ Command တွေကို သိထားဖို့ လိုအပ်ပါတယ်:

- **Build an Image:**

  ```bash
  docker build -t your-image-name .
  ```

  (လက်ရှိ Directory မှာရှိတဲ့ `Dockerfile` ကိုသုံးပြီး Image တည်ဆောက်ခြင်း။ `-t` က Image ကို နာမည်ပေးဖို့အတွက်ဖြစ်ပါတယ်။)

- **Run a Container:**

  ```bash
  docker run -d -p 8080:80 your-image-name
  ```

  (တည်ဆောက်ထားတဲ့ Image ကို Container အဖြစ် Run ခြင်း။ `-d` က background မှာ Run ဖို့၊ `-p` က Host Machine ရဲ့ Port `8080` ကို Container ရဲ့ Port `80` နဲ့ ချိတ်ဆက်ဖို့ ဖြစ်ပါတယ်။)

- **List Containers:**

  ```bash
  docker ps
  ```

  (Run နေတဲ့ Container တွေကို ကြည့်ရှုခြင်း။)

- **List Images:**

  ```bash
  docker images
  ```

  (သင့်စက်ထဲမှာ ရှိနေတဲ့ Images တွေကို ကြည့်ရှုခြင်း။)

- **Stop a Container:**

  ```bash
  docker stop [CONTAINER ID or NAME]
  ```

- **Remove a Container/Image:**
  ```bash
  docker rm [CONTAINER ID or NAME]
  docker rmi [IMAGE ID or NAME]
  ```

---

### 🚀 Docker Compose (Multi-Container Management)

**Docker Compose** ကို အသုံးပြုခြင်းဟာ Frontend Developer တွေအတွက် အလွန်အရေးကြီးပါတယ်။ အဘယ်ကြောင့်ဆိုသော်-

- Frontend Project တစ်ခုဟာ **Backend API** (Node/Python/Go) နဲ့ **Database** (Mongo/Postgres) တို့ကို မှီခိုနေရတတ်ပါတယ်။
- Docker Compose က သင့်ရဲ့ Frontend Container, Backend Container, နဲ့ Database Container တွေကို **YAML** (Yet Another Markup Language) File တစ်ခုထဲမှာ အတူတကွ **Define** လုပ်ပြီး **တစ်ပြိုင်တည်း Run** ပေးနိုင်ပါတယ်။

#### 📝 Docker Compose File နမူနာ:

```yaml
version: "3.8"
services:
  # 1. Frontend Service
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "3000:80" # Host:3000 ကို Container:80 နဲ့ ချိတ်ဆက်
    depends_on:
      - backend

  # 2. Backend Service
  backend:
    image: node:18-alpine
    working_dir: /app
    command: npm run start
    # ... (other backend settings)
```

ဒီဖိုင်ကိုသုံးပြီး docker compose up ဆိုတဲ့ Command တစ်ခုတည်းနဲ့ Frontend, Backend နှစ်ခုလုံးကို တပြိုင်တည်း Run နိုင်ပါတယ်။
