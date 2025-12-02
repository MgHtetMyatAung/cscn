---
slug: stylex-open-source-by-meta
title: StyleX Introduction, Open Sourced by Meta
authors: [MrBoss]
# tags: [facebook, hello, docusaurus]
---

ကျွန်တော်ကတော့ Senior Frontend Developer တစ်ယောက်အနေနဲ့ ဒီနေ့မှာ Meta (Facebook) ကနေ မကြာသေးခင်ကမှ Open Source ပေးလိုက်တဲ့ **StyleX** အကြောင်းကို ဆွေးနွေးပေးချင်ပါတယ်။

ကျွန်တော်တို့ Large Scale Project တွေဖြစ်တဲ့ E-commerce, Banking, Dashboard တွေရေးတဲ့အခါ CSS နဲ့ပတ်သက်ပြီး ခေါင်းကိုက်ရတဲ့ ပြဿနာတွေ (Naming Conflicts, File Size ကြီးလာတာတွေ) ကို StyleX က ဘယ်လိုဖြေရှင်းပေးလဲဆိုတာ လေ့လာကြည့်ရအောင်။

---

## 1. What is StyleX ?

StyleX ဆိုတာ Meta (Facebook, Instagram, WhatsApp) မှာ လက်ရှိသုံးနေတဲ့ **Styling System** တစ်ခုပါ။

ရိုးရိုးရှင်းရှင်းပြောရရင်တော့ **CSS-in-JS** (JavaScript ထဲမှာ CSS ရေးသလိုမျိုး) ဖြစ်ပေမယ့်၊ Browser ပေါ်ရောက်တဲ့အခါ JavaScript အနေနဲ့ Run တာမဟုတ်ဘဲ **Static CSS** (ပုံမှန် `.css` file) အနေနဲ့ **Build Time** မှာတင် ပြောင်းပေးလိုက်တဲ့ Compiler တစ်ခုဖြစ်ပါတယ်။

**StyleX ရဲ့ အဓိက Concept ကတော့:**

> Development လုပ်တုန်းမှာ Developer Experience (DX) ကောင်းအောင် JS နဲ့ရေးမယ်၊ User ဆီရောက်တဲ့အခါ Performance ကောင်းအောင် Optimized CSS အနေနဲ့ ထုတ်ပေးမယ်ဆိုတဲ့ သဘောတရားပါပဲ။

---

## 2. Why use StyleX ?

ကျွန်တော်တို့ Project ကြီးလာတာနဲ့အမျှ CSS တွေရှုပ်ပွလာတတ်ပါတယ်။ StyleX က ဒီအချက်တွေကို ဖြေရှင်းပေးပါတယ်-

### 🚀 Performance (Atomic CSS)

StyleX က Style တွေကို အသေးစိတ်ဖြိုခွဲလိုက်ပါတယ် (**Atomic CSS**). ဥပမာ - `margin: 10px` ကို Component ပေါင်း ၅၀ မှာ သုံးထားရင်တောင် StyleX က CSS file ထဲမှာ Class တစ်ခုတည်းအဖြစ်ပဲ ထုတ်ပေးမှာပါ။ ဒါကြောင့် Project ကြီးလာလည်း CSS file size က အရမ်းကြီးမလာတော့ပါဘူး။

> **Fact:** Meta ဟာ StyleX ကိုသုံးလိုက်လို့ သူတို့ရဲ့ CSS size **80% လောက် ကျသွားတယ်**လို့ ဆိုပါတယ်။

### 🛡️ No Conflict (Collision-Free)

Class name ပေးရတာ ခေါင်းစားဖူးလား? StyleX မှာ Class name တွေကို သူက **Auto generate** လုပ်ပေးတာမို့ ကိုယ်ပေးလိုက်တဲ့ class name တူသွားပြီး တခြားနေရာမှာ Layout ပျက်သွားတာမျိုး (**Specificity Wars**) လုံးဝ မဖြစ်တော့ပါဘူး။

### 🧠 Predictability (ခန့်မှန်းရလွယ်ကူခြင်း)

ပုံမှန် CSS မှာ Class တွေကို အစီအစဉ်ကျော်ရေးမိရင် ဘယ်ဟာက အလုပ်လုပ်မလဲဆိုတာ ရှုပ်ထွေးတတ်ပါတယ်။ StyleX မှာတော့ **"Last Style Wins"** ပါ။ နောက်ဆုံးမှ ထည့်လိုက်တဲ့ Style က အမြဲနိုင်ပါတယ်။ Logic က ရိုးရှင်းပါတယ်။

### 💎 Type-Safe

TypeScript နဲ့ တွဲရေးတဲ့အခါ ကိုယ်က Style property နာမည်မှားရေးမိရင် IDE က ချက်ချင်း **အနီရောင် ပြပါလိမ့်မယ်။**

---

## 3. How to use StyleX ?

React Native ရေးဖူးတဲ့သူတွေဆိုရင် StyleX က အရမ်းရင်းနှီးပြီးသား ဖြစ်နေမှာပါ။

### အဆင့် (၁) - Define Styles (`stylex.create`)

Style တွေကို Object အနေနဲ့ အရင်ကြေညာပါမယ်။ `10px` လို့ String နဲ့ မရေးဘဲ **Number** နဲ့ ရေးရတာ သတိထားပါ။

```javascript
import * as stylex from "@stylexjs/stylex";

const styles = stylex.create({
  base: {
    fontSize: 16,
    lineHeight: 1.5,
    color: "grey", // Type-safe ဖြစ်ပါတယ်
  },
  highlighted: {
    color: "blue",
    fontWeight: "bold",
  },
});
```

### အဆင့် (၂) - Apply Styles (`stylex.props`)

HTML element တွေမှာ `stylex.props` ကိုသုံးပြီး ပြန်ခေါ်သုံးပါမယ်။ Condition တွေနဲ့ တွဲသုံးရတာ အရမ်းမိုက်ပါတယ်။

```javascript
function MyButton({ isActive }) {
  return (
    // isActive က true ဖြစ်ရင် 'highlighted' style က 'base' ကို override လုပ်သွားပါမယ်
    <div {...stylex.props(styles.base, isActive && styles.highlighted)}>
      Click Me
    </div>
  );
}
```

### Behind the Scenes (ဘာတွေဖြစ်သွားလဲ):

Browser မှာ Run တဲ့အခါ အပေါ်က Code ကနေ အောက်ကလို Class name တွေအဖြစ် အလိုအလျောက် ပြောင်းသွားပါမယ်။

```html
<div class="x1e2nbdu x1343 hji">Click Me</div>
```

## 📝 Summary for Myanmar Devs 🇲🇲

အနှစ်ချုပ်ပြောရရင် -

- အကယ်၍ သင်က Personal Blog လေးတွေ၊ Small Project လေးတွေပဲ ရေးမယ်ဆိုရင် **Tailwind CSS** က အဆင်ပြေဆုံးပါပဲ။
- ဒါပေမဲ့ **Large Scale Application** တွေ (ဥပမာ - Banking App, Super App) ရေးနေရတယ်၊ Team ကလည်း အကြီးကြီးဖြစ်ပြီး CSS file တွေ ရှုပ်ပွနေတာကို ရှင်းချင်တယ်ဆိုရင်တော့ **StyleX** က အကောင်းဆုံး ရွေးချယ်မှုတစ်ခု ဖြစ်လာမှာပါ။

**Happy Coding !💻**
