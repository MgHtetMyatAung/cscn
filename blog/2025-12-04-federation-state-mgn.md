---
slug: state-management-micro-frontend
title: State Management in Micro Frontend
authors: [MrBoss]
# tags: [facebook, hello, docusaurus]
---

# Micro Frontend များကြား State Management (Data မျှဝေမှု) ကို ဆွေးနွေးခြင်း

Micro Frontend တွေရဲ့ အဓိက စိန်ခေါ်မှုတစ်ခုကတော့ သီးခြားရပ်တည်နေတဲ့ Application အပိုင်းအစလေးတွေကြားမှာ **State (Application ရဲ့ အခြေအနေ၊ Data)** တွေကို ဘယ်လို သတင်းအချက်အလက် ပေးပို့ ဆက်သွယ်မလဲ၊ ဘယ်လို မျှဝေသုံးစွဲမလဲဆိုတာပဲ ဖြစ်ပါတယ်။

ဒါကို စီမံခန့်ခွဲတဲ့ နည်းလမ်းတွေ အများကြီးရှိပါတယ်။ အသုံးအများဆုံးနဲ့ ထိရောက်တဲ့ နည်းလမ်းတွေကို အောက်မှာ ရှင်းပြပေးပါ့မယ်။

---

## 🎯 ၁။ Browser ၏ Native Mechanism များ အသုံးပြုခြင်း

Application အပိုင်းအစတွေကို ပေါင်းစပ်တဲ့အခါ အလွယ်ကူဆုံးနဲ့ Browser မှာ မူလပါဝင်တဲ့ နည်းလမ်းတွေကို အသုံးပြုနိုင်ပါတယ်။

### A. Custom Events (သို့) Pub/Sub Pattern

- **သဘောတရား:** ဒါဟာ Publish/Subscribe (ထုတ်ပြန်ခြင်း/စာရင်းသွင်းခြင်း) ပုံစံကို အခြေခံပါတယ်။ Micro Frontend တစ်ခု (**Publisher**) က Event တစ်ခု (ဥပမာ: "ကုန်ပစ္စည်း ခြင်းတောင်းထဲ ထည့်လိုက်ပြီ") ကို ထုတ်ပြန်လိုက်ရင် ကျန်တဲ့ Micro Frontend တွေ (**Subscribers**) က အဲဒီ Event ကို နားထောင်ပြီး လိုအပ်သလို အလုပ်လုပ်ပါတယ်။
- **ဘယ်လို အကောင်အထည်ဖော်မလဲ:**
  - **Browser ၏ CustomEvent:** Browser ရဲ့ built-in ဖြစ်တဲ့ `dispatchEvent()` နဲ့ `addEventListener()` ကို သုံးပြီး Global `window` object ပေါ်မှာ Event တွေကို ပေးပို့ ဆက်သွယ်နိုင်ပါတယ်။
  - **Library များ:** `mitt` (သို့) `EventEmitter` လိုမျိုး Pub/Sub Utility Library သေးသေးလေးတွေကို သုံးပြီး ပိုမို စနစ်တကျ စီမံခွဲနိုင်ပါတယ်။
- **အားသာချက်:** ရိုးရှင်းတယ်၊ Framework တွေပေါ် မမူတည်ဘူး (Framework-Agnostic)။

### B. Local Storage / Session Storage

- **သဘောတရား:** App တွေကြား မျှဝေချင်တဲ့ Data (ဥပမာ: User ရဲ့ Authentication Token သို့မဟုတ် Theming Preference) တွေကို Browser ရဲ့ Storage ထဲမှာ သိမ်းဆည်းထားပြီး လိုအပ်တဲ့ App ကနေ ပြန်ဖတ်ယူသုံးစွဲတာ ဖြစ်ပါတယ်။
- **အားနည်းချက်:** Storage မှာ သိမ်းထားတဲ့ Data တွေ ပြောင်းလဲသွားရင် ကျန်တဲ့ App တွေကို အကြောင်းကြားဖို့ သီးခြား Event တွေ ထပ်ထည့်ပေးရပါတယ်။ Real-time Data တွေအတွက် မသင့်တော်ပါ။

---

## 🔗 ၂။ Shared Library (သို့) Container App ကို အသုံးပြုခြင်း

Micro Frontend တွေမှာ အတွေ့ရများတဲ့ ပုံစံပါ။ Data တွေကို နေရာတစ်ခုတည်းမှာ စုစည်းပြီး စီမံခန့်ခွဲစေပါတယ်။

### A. Shared State Management Library

- **သဘောတရား:** Micro Frontend အားလုံးကနေ တပြိုင်နက် ဝင်ရောက်ကြည့်ရှု၊ ပြောင်းလဲနိုင်တဲ့ Global State တစ်ခုကို သီးခြား Library တစ်ခုအနေနဲ့ တည်ဆောက်ပြီး **Module Federation** (သို့) အခြား နည်းလမ်းတွေနဲ့ **Shared Dependency** အနေနဲ့ မျှဝေသုံးစွဲတာ ဖြစ်ပါတယ်။
- **ဥပမာ:** Redux (သို့) Zustand လိုမျိုး Global State Library တစ်ခုကို "**state-store**" ဆိုတဲ့ Package အနေနဲ့ ဖန်တီးပြီး၊ လိုအပ်တဲ့ App တွေက ဒီ Package ကို ယူသုံးတာပါ။
- **အားသာချက်:** Data စီးဆင်းမှု (Data Flow) ကို နားလည်လွယ်တဲ့ စံနှုန်း (Standard) တစ်ခုနဲ့ စီမံခန့်ခွဲနိုင်တယ်။

### B. Parent/Container App မှ စီမံခြင်း

- **သဘောတရား:** Micro Frontend တွေကို စုစည်းထားတဲ့ **Container App (Host App)** က Global State ကို ကိုင်တွယ်ထားပြီး၊ အဲဒီ Data တွေကို Child Micro Frontend တွေဆီကို **Props** (သို့) Custom API တွေကနေတဆင့် ပေးပို့တာ ဖြစ်ပါတယ်။
- **အားသာချက်:** Data တွေရဲ့ Source of Truth (အမှန်တကယ် စစ်မှန်တဲ့ Data ရှိရာနေရာ) ကို ရှင်းရှင်းလင်းလင်း သိနိုင်တယ်။
- **အားနည်းချက်:** Container App နဲ့ Child App တွေကြားမှာ Coupling (တွယ်ငြိမှု) များလာနိုင်ပြီး၊ Child App တွေကို သီးခြား ပြန်လည်အသုံးပြုဖို့ ခက်ခဲလာနိုင်တယ်။

---

## 🌐 ၃။ URL (Routing) မှတစ်ဆင့် Data ပေးပို့ခြင်း

Data ပမာဏ နည်းပါးပြီး Application ၏ လက်ရှိ အခြေအနေကို ဖော်ပြတဲ့ Data များအတွက် အသုံးဝင်ပါတယ်။

### URL Parameters/Query Strings

- **သဘောတရား:** Micro Frontend တစ်ခုကနေ နောက်တစ်ခုကို ခေါ်တဲ့အခါ URL (Web Link) ရဲ့ နောက်မှာ Data တွေကို Query String အနေနဲ့ တွဲပါသွားအောင် ပေးပို့တာ ဖြစ်ပါတယ်။
- **ဥပမာ:** Product List App ကနေ Product Detail App ကို သွားတဲ့အခါ `.../product/detail?**productId=123**` လို့ ပို့လိုက်ရင် Detail App က `productId=123` ကို ဖတ်ပြီး Data ဆွဲယူပြသတာမျိုးပါ။
- **အားသာချက်:** လွယ်ကူတယ်၊ Page Refresh လုပ်ရင်လည်း State မပျောက်ဘူး (Bookmark လုပ်ထားလို့ရတယ်)။
- **အားနည်းချက်:** Sensitive Data တွေနဲ့ Data ပမာဏ များရင် မသင့်တော်ပါ။

---

## 💡 အကောင်းဆုံး လမ်းညွှန်ချက်များ (Best Practices)

Micro Frontend များအတွက် State Management ကို အသုံးပြုရာမှာ အောက်ပါ အချက်တွေကို လိုက်နာသင့်ပါတယ်။

1.  **Distributed State ကို အဓိကထားပါ:** Micro Frontend တစ်ခုချင်းစီဟာ သူ့ရဲ့အတွင်းပိုင်း State (**Self-Contained State**) ကို ကိုယ်တိုင် ကိုင်တွယ်ဖို့ တတ်နိုင်သမျှ ကြိုးစားပါ။
2.  **Global State ကို အတတ်နိုင်ဆုံး လျှော့ချပါ:** မရှိမဖြစ် လိုအပ်တဲ့ Data (ဥပမာ: User Login Status) တွေကိုသာ Global (မျှဝေသုံးစွဲဖို့) ထားပါ။ တခြား App တွေနဲ့ သိပ်မဆိုင်တဲ့ Data တွေကို Local မှာပဲ ထားပါ။
3.  **Communication နည်းလမ်းကို စံသတ်မှတ်ပါ:** Pub/Sub (Custom Events) ကို အသုံးပြုမယ်ဆိုရင် ဘယ်လို Key နာမည်တွေ (ဥပမာ: `cart-updated`) ကို သုံးမလဲ၊ ဘယ်လို Payload (ပါသွားမယ့် Data) ပုံစံကို သုံးမလဲဆိုတာကို အဖွဲ့တွေကြား သဘောတူညီမှု (Agreement) ရယူထားရပါမယ်။

---

Micro Frontend တွေရဲ့ State Management အတွက် အဲဒီနည်းလမ်းတွေဟာ အဓိကကျပါတယ်။
