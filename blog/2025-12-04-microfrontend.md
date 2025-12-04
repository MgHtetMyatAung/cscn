---
slug: micro-frontend
title: Understanding in Micro Frontend
authors: [MrBoss]
# tags: [facebook, hello, docusaurus]
---

လွန်ခဲ့တဲ့ရက်ပိုင်းက Docker အကြောင်း ဆွေးနွေးခဲ့ကြပြီးပြီဆိုတော့ ဒီနေ့မှာတော့ Frontendလောကမှာ ခေတ်စားလာနေပြီး Enterprise Level Project တွေမှာ မဖြစ်မနေ သိထားသင့်တဲ့ **"Micro Frontend"** အကြောင်းကို ရှင်းပြပေးသွားမှာ ဖြစ်ပါတယ်။

Monolithic (အလုံးကြီးတစ်လုံးတည်း) ကနေ Microservices ဘက်ကို Backend သမားတွေ ကူးပြောင်းသွားသလိုပဲ၊ Frontend မှာလည်း ဒီ Concept ကို အသုံးပြုလာကြတာပါ။

---

### ၁။ Micro Frontend ဆိုတာ ဘာလဲ? (Simple Concept)

ရိုးရိုးရှင်းရှင်း ပြောရရင် Web Application အကြီးကြီးတစ်ခုကို **"သေးငယ်တဲ့ အစိတ်အပိုင်း (App) လေးတွေအဖြစ် ခွဲထုတ်ပြီး ပြန်လည်ပေါင်းစပ်တည်ဆောက်ခြင်း"** ဖြစ်ပါတယ်။

ပုံမှန် Traditional (Monolithic) နည်းလမ်းမှာ Website တစ်ခုလုံးကို React (သို့) Vue project တစ်ခုတည်းအောက်မှာ အကုန်ရေးကြပါတယ်။ Project ကြီးလာတာနဲ့အမျှ Build time ကြာလာမယ်၊ Team တွေ အများကြီးဝင်လုပ်တဲ့အခါ Code Conflict တွေဖြစ်မယ်၊ Maintenance လုပ်ရခက်လာပါမယ်။

Micro Frontend မှာတော့ -

- Header ကို Team A က တာဝန်ယူမယ် (React နဲ့ ရေးချင်ရေးမယ်)
- Product List ကို Team B က တာဝန်ယူမယ် (Vue နဲ့ ရေးချင်ရေးမယ်)
- Checkout page ကို Team C က တာဝန်ယူမယ် (Angular နဲ့ ရေးချင်ရေးမယ်)

နောက်ဆုံးမှ ဒီအပိုင်းတွေအားလုံးကို **Container App (Host App)** တစ်ခုထဲမှာ ပြန်ပေါင်းစည်းပြီး User ဘက်ကကြည့်ရင် Website တစ်ခုတည်းလိုပဲ မြင်ရအောင် လုပ်ဆောင်တာ ဖြစ်ပါတယ်။

[Image of micro frontend architecture diagram]

---

### ၂။ ဘာကြောင့် Micro Frontend ကို သုံးသင့်တာလဲ? (Pros)

လုပ်ငန်းခွင် (Working Level) မှာ ဒီအချက်တွေကြောင့် သုံးကြတာများပါတယ်။

1.  **Independent Deployments:** Header ပိုင်းပဲ ပြင်စရာရှိရင် Header app လေးကိုပဲ Deploy လုပ်လိုက်ရုံပါပဲ။ Website တစ်ခုလုံးကို ပြန် Build/Deploy လုပ်စရာ မလိုတော့ပါဘူး။
2.  **Team Autonomy:** Team တွေက ကိုယ်ပိုင်ဆုံးဖြတ်ချက်နဲ့ ကိုယ်ကြိုက်တဲ့ Framework ကို သုံးလို့ရပါတယ်။ (ဥပမာ - Legacy Code တွေက jQuery နဲ့ကျန်ခဲ့လည်း အသစ်တွေကို React နဲ့ သီးခြားခွဲရေးလို့ ရသွားပါတယ်)။
3.  **Faster Build Time:** Codebase သေးသွားတဲ့အတွက် Development လုပ်ရတာရော Build လုပ်ရတာပါ ပိုမြန်ဆန်လာပါတယ်။
4.  **Incremental Upgrade:** Website အဟောင်းကြီးကို အသစ်ပြန်ရေးချင်တဲ့အခါ တစ်ခါတည်း အကုန်ဖြိုဖျက်စရာမလိုဘဲ တစ်စစီ (Feature by feature) ခွဲထုတ်ပြီး အသစ်ပြန်ရေးလို့ ရပါတယ်။

---

### ၃။ ဘယ်လိုနည်းလမ်းတွေနဲ့ Implement လုပ်ကြလဲ?

Micro Frontend ကို အကောင်အထည်ဖော်ဖို့ နည်းလမ်းအမျိုးမျိုးရှိပေမယ့် လက်ရှိ Industry မှာ အသုံးအများဆုံး (၃) ခုကို ပြောပြပေးပါမယ်။

#### **A. Iframe (အရိုးရှင်းဆုံးနည်းလမ်း)**

Feature တစ်ခုချင်းစီကို `iframe` နဲ့ လှမ်းချိတ်တာပါ။

- **ကောင်းချက်:** လွယ်တယ်၊ Isolation (သီးခြားကွဲထွက်မှု) အပြည့်ရှိတယ်။
- **ဆိုးချက်:** Performance မကောင်းဘူး၊ SEO ပြဿနာရှိတယ်၊ Responsive ဖြစ်ဖို့ ခက်ခဲတယ်။ (Modern Web App တွေမှာ သိပ်မသုံးကြတော့ပါ)

#### **B. Web Components (Custom Elements)**

Browser ရဲ့ Native Feature ဖြစ်တဲ့ Web Components ကို သုံးပြီး `<product-list></product-list>` ဆိုပြီး HTML tag အနေနဲ့ ခေါ်သုံးတာပါ။ Framework မရွေးဘူး၊ သုံးရလွယ်ကူပါတယ်။

#### **C. Module Federation (The Modern Standard)**

လက်ရှိမှာ အအောင်မြင်ဆုံးနဲ့ Developer တွေ အကြိုက်ဆုံးနည်းလမ်းပါ။ **Webpack 5** မှာ ပါလာတဲ့ Feature တစ်ခုဖြစ်ပါတယ်။

- JavaScript file တွေကို Runtime (Browser ပေါ်ရောက်မှ) ကျမှ Dynamic load လုပ်ပြီး ပေါင်းစည်းပေးတာပါ။
- Library တွေ (ဥပမာ React, Lodash) ကို အတူတူ Share သုံးလို့ရတဲ့အတွက် File size ကိုလည်း မကြီးစေပါဘူး။

---

### ၄။ လက်တွေ့မှာ ဘယ်အချိန်သုံးသင့်လဲ? (When to use)

"Micro Frontend က ကောင်းတယ်ဆိုတိုင်း နေရာတိုင်းမှာ သုံးရမယ်လို့ မဆိုလိုပါဘူး။"

- **မသုံးသင့်ပါ:** Project က သေးငယ်မယ်၊ Developer ၃-၄ ယောက်လောက်နဲ့ပဲ လုပ်နေမယ်ဆိုရင် Micro Frontend က မလိုအပ်တဲ့ Complexity တွေကိုပဲ တိုးလာစေပါလိမ့်မယ်။ ရိုးရိုး Monolithic ကပဲ ပိုကောင်းပါတယ်။
- **သုံးသင့်ပါ:**
  - Developer အယောက် ၂၀-၃၀ ကျော်ပြီး Team တွေ အများကြီးခွဲထားရတဲ့အခါ။
  - Domain တွေ ရှုပ်ထွေးပြီး ကြီးမားတဲ့ E-commerce, Dashboard အကြီးကြီးတွေ လုပ်တဲ့အခါ။
  - Legacy Project ကြီးကို တဖြည်းဖြည်းချင်း အသစ်ပြောင်းချင်တဲ့အခါ။

---

### ၅။ ဥပမာ - Module Federation (Concept Code)

Host App (Main Container) ရဲ့ `webpack.config.js` မှာ ဒီလိုမျိုး သတ်မှတ်လေ့ရှိပါတယ်။

```javascript
// Host App (Container)
new ModuleFederationPlugin({
  name: "container",
  remotes: {
    // အခြား App တွေကို လှမ်းချိတ်ခြင်း
    header: "header@http://localhost:3001/remoteEntry.js",
    products: "products@http://localhost:3002/remoteEntry.js",
  },
});
```

ပြီးရင် React ကုဒ်ထဲမှာ ဒီလို လှမ်းခေါ်သုံးလို့ ရသွားပါပြီ။

```javascript
import React, { Suspense } from "react";

// Remote App ကို လှမ်းခေါ်ခြင်း
const RemoteHeader = React.lazy(() => import("header/Header"));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading Header...</div>}>
        <RemoteHeader />
      </Suspense>
      <h1>Main Container App</h1>
    </div>
  );
}
```

---

**နိဂုံးချုပ်**

Micro Frontend ဆိုတာ Frontend Architecture ရဲ့ အဆင့်မြင့်ပုံစံတစ်ခု ဖြစ်ပါတယ်။ ရှုပ်ထွေးမှုကို ဖြေရှင်းဖို့ ပေါ်လာတာဖြစ်ပေမယ့်၊ မလိုအပ်ဘဲ သုံးမိရင် ကိုယ့် Project ကို ပိုရှုပ်ထွေးသွားစေနိုင်တာကို သတိပြုရပါမယ်။ ဒါပေမယ့် Senior Developer တစ်ယောက်အနေနဲ့ ဒီ Architecture ကို သေချာပေါက် နားလည်ထားသင့်ပါတယ်။
