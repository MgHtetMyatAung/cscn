---
slug: module-federation-micro-frontend-vite
title: Module Federation in Micro Frontend with Vite
authors: [MrBoss]
# tags: [facebook, hello, docusaurus]
---

ဒီနေ့မှာတော့ **Vite** နဲ့ **Module Federation** ကိုသုံးပြီး Micro Frontend အသေးစားလေးတစ်ခု လက်တွေ့ တည်ဆောက်ပြပါမယ်။

Backend မှာ Microservices ချိတ်သလိုပဲ၊ Frontend မှာ App နှစ်ခု (Host နဲ့ Remote) ကို ဘယ်လိုချိတ်ဆက်မလဲဆိုတာ အဆင့်ဆင့် လိုက်လုပ်ကြည့်ရအောင်။

---

### လက်တွေ့စမ်းသပ်မည့် ပုံစံ

ကျွန်တော်တို့ Project (၂) ခု ဆောက်ပါမယ်။

1.  **Remote App (Producer):** Button Component လေးတစ်ခု ထုတ်ပေးမယ့် App (Port: 5001)
2.  **Host App (Consumer):** ဟိုဘက်က Button ကို လှမ်းယူသုံးမယ့် App (Port: 5000)

ဒီ Tutorial မှာ **`@originjs/vite-plugin-federation`** ဆိုတဲ့ plugin ကို အသုံးပြုသွားပါမယ်။

---

### အဆင့် (၁) - Project အသစ် (၂) ခု တည်ဆောက်ခြင်း

Terminal မှာ အောက်ပါအတိုင်း Project (၂) ခု ဆောက်လိုက်ပါ။

```bash
# 1. Host App ဆောက်မယ်
npm create vite@latest host-app -- --template react
cd host-app
npm install
npm install @originjs/vite-plugin-federation --save-dev

# 2. Remote App ဆောက်မယ် (နောက် Terminal တစ်ခုနဲ့ လုပ်ပါ)
npm create vite@latest remote-app -- --template react
cd remote-app
npm install
npm install @originjs/vite-plugin-federation --save-dev
```

---

### အဆင့် (၂) - Remote App (Button ထုတ်ပေးမည့်သူ) ကို ပြင်ဆင်ခြင်း

`remote-app` folder ထဲမှာ အလုပ်လုပ်ပါမယ်။

**၁. Component တစ်ခု အရင်ဆောက်ပါ**
`src/components/Button.jsx` ဆိုပြီး ဖိုင်အသစ်ဆောက်ပါ။

```javascript
// src/components/Button.jsx
import React from "react";

export const Button = () => {
  return (
    <button
      style={{ backgroundColor: "blue", color: "white", padding: "10px" }}
    >
      Remote Button (From App 2)
    </button>
  );
};

export default Button;
```

**၂. Vite Config မှာ Expose လုပ်ပေးပါ**
`vite.config.js` ဖိုင်ကို ဖွင့်ပြီး အောက်ပါအတိုင်း ပြင်ရေးပါ။

```javascript
// remote-app/vite.config.js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import federation from "@originjs/vite-plugin-federation";

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: "remote_app", // Remote App နာမည်
      filename: "remoteEntry.js", // Entry file နာမည် (Host က ဒါကို လှမ်းခေါ်ရမှာ)
      exposes: {
        "./Button": "./src/components/Button", // ဘယ် Component ကို ထုတ်ပေးမလဲ
      },
      shared: ["react", "react-dom"], // React version မကွဲအောင် share သုံးမယ်
    }),
  ],
  build: {
    target: "esnext", // Modern browser တွေအတွက်
  },
});
```

---

### အဆင့် (၃) - Host App (Button လှမ်းယူမည့်သူ) ကို ပြင်ဆင်ခြင်း

အခု `host-app` folder ဘက်ကို ပြောင်းပြီး အလုပ်လုပ်ပါမယ်။

**၁. Vite Config မှာ Remote ကို လှမ်းချိတ်ပါ**
`vite.config.js` ကို ဖွင့်ပြီး ပြင်ဆင်ပါ။

```javascript
// host-app/vite.config.js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import federation from "@originjs/vite-plugin-federation";

export default defineConfig({
  plugins: [
    react(),
    federation({
      name: "host_app",
      remotes: {
        // Remote App ရဲ့ URL နဲ့ entry file ကို လှမ်းချိတ်တာပါ
        remoteApp: "http://localhost:5001/assets/remoteEntry.js",
      },
      shared: ["react", "react-dom"],
    }),
  ],
  build: {
    target: "esnext",
  },
});
```

**၂. Remote Component ကို ခေါ်သုံးပါ**
`src/App.jsx` မှာ ဟိုဘက်က Button ကို လှမ်းခေါ်သုံးပါမယ်။

```javascript
// host-app/src/App.jsx
import React, { useState } from "react";
import "./App.css";

// Remote App ဆီက Button ကို Import လုပ်ခြင်း
// 'remoteApp' ဆိုတဲ့နာမည်က vite.config.js ထဲက remotes နာမည်နဲ့ တူရပါမယ်
import RemoteButton from "remoteApp/Button";

function App() {
  return (
    <div className="App">
      <h1>Host Application</h1>
      <div className="card">
        <p>အောက်က Button က Remote App ဆီက လာတာပါ</p>

        {/* Remote Component ကို သုံးလိုက်ပြီ */}
        <RemoteButton />
      </div>
    </div>
  );
}

export default App;
```

---

### အဆင့် (၄) - Project တွေကို Run ကြည့်မယ်

ဒီအပိုင်းက အရေးအကြီးဆုံးပါ။ Module Federation အလုပ်လုပ်ဖို့ **Build လုပ်ပြီး Preview** နဲ့ Run မှ ပိုသေချာပါတယ်။ (Dev server မှာ တခါတလေ Hot Reload ပြဿနာရှိတတ်လို့ပါ)။

**၁. Remote App ကို Run ပါ (Port 5001)**
`remote-app` ရဲ့ `package.json` မှာ script ကို ဒီလိုအရင်ပြင်လိုက်ရင် ပိုလွယ်ပါတယ်။ `"preview": "vite preview --port 5001 --strictPort",`

```bash
# remote-app folder ထဲမှာ
npm run build
npm run preview
```

(အခုဆိုရင် `http://localhost:5001/assets/remoteEntry.js` ဆိုတဲ့ file ရှိနေပါပြီ)

**၂. Host App ကို Run ပါ (Port 5000)**
`host-app` ရဲ့ `package.json` မှာ script ကို ဒီလိုပြင်ပါ။ `"preview": "vite preview --port 5000 --strictPort",`

```bash
# host-app folder ထဲမှာ
npm run build
npm run preview
```

---

### ရလဒ် (Result)

Browser မှာ `http://localhost:5000` (Host App) ကို ဝင်ကြည့်လိုက်ပါ။
Host App ရဲ့ ခေါင်းစဉ်အောက်မှာ Remote App က ထုတ်ပေးလိုက်တဲ့ **"အပြာရောင် Button"** လေးကို မြင်ရပြီဆိုရင် မိတ်ဆွေဟာ Micro Frontend Architecture ကို အောင်မြင်စွာ တည်ဆောက်နိုင်သွားပါပြီ။

**Mr. Kan's Tip:**

> တကယ့်လုပ်ငန်းခွင်မှာတော့ `localhost` နေရာမှာ Domain အစစ်တွေ (ဥပမာ - `header.example.com`) ဖြစ်သွားမှာပါ။ Concept ကတော့ ဒီအတိုင်း အတူတူပါပဲ။
