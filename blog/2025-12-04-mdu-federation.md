---
slug: module-federation-micro-frontend
title: Module Federation in Micro Frontend
authors: [MrBoss]
# tags: [facebook, hello, docusaurus]
---

# 🚀 Module Federation (Webpack) ကို အသေးစိတ် ရှင်းပြခြင်း

မင်္ဂလာပါ ခင်ဗျာ။ Micro Frontend တွေ တည်ဆောက်တဲ့နေရာမှာ အခုလက်ရှိ ခေတ်အမီဆုံးနဲ့ အစွမ်းထက်ဆုံး နည်းလမ်းဖြစ်တဲ့ **Webpack** ရဲ့ **Module Federation** အကြောင်းကို ဆက်ပြီး အသေးစိတ် ရှင်းပြပေးပါ့မယ်။

---

## 🚀 Module Federation ဆိုတာ ဘာလဲ?

**Module Federation** ဆိုတာ **Webpack 5** မှာ အသစ်ပါဝင်လာတဲ့ Feature တစ်ခုဖြစ်ပြီး၊ **သီးခြား Build လုပ်ထားတဲ့ Javascript Application တွေကို Runtime (Application လည်ပတ်နေချိန်) မှာ တစ်ခုနဲ့တစ်ခု Load လုပ်ပြီး မျှဝေသုံးစွဲနိုင်အောင် လုပ်ပေးတဲ့ ဗိသုကာပုံစံ (Architecture Pattern)** တစ်ခုပဲ ဖြစ်ပါတယ်။

ရိုးရိုးရှင်းရှင်း ပြောရရင် -

- သင့်မှာ **App A (Host)** နဲ့ **App B (Remote)** ဆိုတဲ့ သီးခြား Application ၂ ခုရှိမယ်။
- App B ထဲက Component တွေ၊ Service တွေကို **App A ကနေ လိုအပ်တဲ့အချိန်မှာ အလွယ်တကူ Dynamic Load လုပ်ပြီး ယူသုံးလို့ရ**တယ်။

ဒါဟာ Micro Frontend ရဲ့ အဓိက စိန်ခေါ်မှုဖြစ်တဲ့ "သီးခြားတည်ဆောက်ထားတဲ့ App တွေကို Browser ထဲမှာ တစ်သားတည်းကျအောင် ဘယ်လို ပေါင်းစပ်မလဲ" ဆိုတာကို ဖြေရှင်းပေးလိုက်တာ ဖြစ်ပါတယ်။

---

## 🔑 အဓိက သဘောတရားများ (Core Concepts)

Module Federation မှာ နားလည်ထားရမယ့် အဓိက အခန်းကဏ္ဍ (Roles) ၃ ခုရှိပါတယ်။

### ၁။ Host Application (အိမ်ရှင် App)

- ဒါဟာ User က ပထမဆုံး ဝင်ရောက်တဲ့ Application ဖြစ်ပြီး ကျန်တဲ့ **Remote Application တွေကို ယူသုံးမယ့် App** ဖြစ်ပါတယ်။
- သူ့ရဲ့ Webpack Configuration မှာ Remote Application တွေရဲ့ နေရာ (Location) ကို သတ်မှတ်ပေးရပါတယ်။

### ၂။ Remote Application (အဝေးမှ App)

- ဒါဟာ သူ့ရဲ့ Component တွေ၊ Module တွေ၊ Library တွေ ကို Host Application က ယူသုံးနိုင်အောင် **Export (ထုတ်ပေး) မယ့် App** ဖြစ်ပါတယ်။
- သူ့ရဲ့ Webpack Configuration မှာ Exposed လုပ်မယ့် Component တွေကို သတ်မှတ်ပေးရပါတယ်။

### ၃။ Shared Dependencies (မျှဝေသုံးစွဲမှု)

- Host နဲ့ Remote App တွေဟာ တူညီတဲ့ Library တွေ (ဥပမာ: React, React-DOM, Redux) ကို သုံးလေ့ရှိပါတယ်။
- Module Federation ရဲ့ အစွမ်းထက်ဆုံး အချက်ကတော့ ဒီလို တူညီတဲ့ Library တွေကို **App အားလုံးကနေ တစ်ကြိမ်တည်းသာ Load လုပ်ပြီး မျှဝေသုံးစွဲနိုင်အောင်** လုပ်ပေးတာပါပဲ။
- ဒါကြောင့် **Performance** ကို သိသိသာသာ တိုးတက်စေပြီး တူညီတဲ့ Library တွေ ထပ်ခါထပ်ခါ Load ဖြစ်တာမျိုးကို တားဆီးပေးပါတယ်။

---

## ⚙️ Webpack Configuration (ဘယ်လို အလုပ်လုပ်သလဲ)

Module Federation ကို အသုံးပြုဖို့အတွက် Webpack ရဲ့ `ModuleFederationPlugin` ကို အသုံးပြုရပါတယ်။

### 📝 Remote App (Component ထုတ်ပေးမယ့်သူ) အတွက် Config ပုံစံ:

```javascript
// remote-app/webpack.config.js
plugins: [
  new ModuleFederationPlugin({
    name: "remoteApp", // Remote App ရဲ့ နာမည်
    filename: "remoteEntry.js", // Host က ယူသုံးဖို့ Entry file
    exposes: {
      // ဘယ် Module တွေကို ထုတ်ပေးမလဲ
      "./ProductCard": "./src/components/ProductCard.js",
      "./utils": "./src/utils/utility-functions.js",
    },
    shared: ["react", "react-dom"], // မျှဝေသုံးစွဲမယ့် Library တွေ
  }),
];
```

### 📝 Host App (Component ယူသုံးမယ့်သူ) အတွက် Config ပုံစံ:

```javascript
// host-app/webpack.config.js
plugins: [
  new ModuleFederationPlugin({
    name: "hostApp",
    remotes: {
      // ဘယ် Remote App ကို ယူသုံးမလဲ
      remoteApp: "remoteApp@http://localhost:3001/remoteEntry.js", // Name@URL
    },
    shared: ["react", "react-dom"], // Host ကလည်း မျှဝေဖို့ သတ်မှတ်ရမယ်
  }),
];
```

### 🎯 Host App မှာ ဘယ်လို ခေါ်သုံးမလဲ?

Configuration လုပ်ပြီးတာနဲ့ Host App မှာ Remote App က ထုတ်ပေးထားတဲ့ Component တွေကို Dynamic Import (Async) ပုံစံနဲ့ အောက်ပါအတိုင်း အသုံးပြုနိုင်ပါတယ်။

```javascript
// host-app/src/App.js
import React from "react";

// Remote App ထဲက ProductCard ကို ခေါ်ယူသုံးစွဲခြင်း
const RemoteProductCard = React.lazy(() => import("remoteApp/ProductCard"));

function App() {
  return (
    <div>
      <h1>Host Application Content</h1>
      <React.Suspense fallback={<div>Loading...</div>}>
        <RemoteProductCard productId={123} />
      </React.Suspense>
    </div>
  );
}
```

### ⭐️ Module Federation ရဲ့ အားသာချက်များ

- **True Runtime Integration:** Build Time မှာ မဟုတ်ဘဲ App လည်ပတ်နေချိန်မှာ Component တွေကို တိုက်ရိုက် Load လုပ်တာဖြစ်လို့ ပိုမိုပြောင်းလွယ်ပြင်လွယ် ရှိပါတယ်။
- **Efficient Dependency Sharing:** Shared Dependencies ကြောင့် Component တွေကို Bundle Size သေးငယ်စွာ ထိန်းသိမ်းနိုင်ပြီး၊ User ရဲ့ Browser ကနေ Library တွေကို **တစ်ကြိမ်တည်းသာ Download** လုပ်စေပါတယ်။
- **Cross-Framework Potential:** နည်းပညာ မတူညီတဲ့ App တွေကြားမှာ (ဥပမာ: React App ကနေ Vue Component ကို) ပေါင်းစပ်သုံးစွဲဖို့ လမ်းဖွင့်ပေးပါတယ်။ (သို့သော် နည်းပညာချင်း တူညီမှ ပိုမိုလွယ်ကူပြီး အကောင်းဆုံး အလုပ်လုပ်ပါတယ်)။
- **Decoupling:** App တွေကို လုံးဝ သီးခြားခွဲထားတာကြောင့် အဖွဲ့တွေရဲ့ အလုပ်လုပ်ပုံကို ပိုမို လျင်မြန်စေပါတယ်။

---

### အနှစ်ချုပ်အားဖြင့်

**Module Federation** ဟာ Micro Frontend အတွက် Component တွေကို **Build-Time မှာ မဟုတ်ဘဲ Run-Time မှာ မျှဝေသုံးစွဲနိုင်အောင် လုပ်ပေးတဲ့ အဆင့်မြင့်တဲ့ စနစ်**တစ်ခုပဲ ဖြစ်ပါတယ်။ ဒါဟာ ခေတ်မီတဲ့ Web Application ကြီးတွေအတွက် **Performance** နဲ့ **Scalability** ကို တိုးတက်စေဖို့ အကောင်းဆုံး ရွေးချယ်မှုတစ်ခု ဖြစ်ပါတယ်ခင်ဗျာ။
