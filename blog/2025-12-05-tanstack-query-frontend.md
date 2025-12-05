---
slug: tanstack-query-frontend
title: Tanstack Query for Frontend
authors: [MrBoss]
# tags: [facebook, hello, docusaurus]
---

Frontend လောကမှာ လက်ရှိ Hot အဖြစ်ဆုံးနဲ့ လုပ်ငန်းခွင် (Working Level) မှာ မဖြစ်မနေ သိထားရမယ့် Library တစ်ခုကတော့ **TanStack Query (အရင်က React Query)** ဖြစ်ပါတယ်။

Redux သုံးဖူးတဲ့သူတွေအနေနဲ့ _"Global State Management အတွက် Redux ရှိပြီးသား၊ ဒါကြီးက ဘာထပ်လုပ်တာလဲ?"_ လို့ မေးစရာရှိပါတယ်။
အဖြေကတော့ - **"Redux က Client State (Theme, Modal open/close) အတွက်ကောင်းပြီး၊ TanStack Query က Server State (API Data fetching/caching) အတွက် အကောင်းဆုံးပါ"**။

Basic ကနေ Advanced အထိ လက်တွေ့လုပ်ငန်းခွင်သုံး ပုံစံတွေကို ရှင်းပြပေးသွားပါမယ်။

---

### ၁။ ဘာကြောင့် TanStack Query ကို သုံးသင့်တာလဲ? (The Pain Point)

ပုံမှန် `useEffect` နဲ့ Data fetch ကြည့်ရအောင်။

**The Old Way (Without TanStack Query):**

1.  `useState` (loading, data, error) သုံးခုဆောက်ရမယ်။
2.  `useEffect` ထဲမှာ `fetch` ခေါ်မယ်။
3.  Data မရခင် `loading` ပြ၊ ရရင် `data` ထည့်၊ error တက်ရင် `error` ပြ။
4.  တခြား page ကူးသွားပြီး ပြန်လာရင် Data က ပျောက်သွားလို့ **ထပ် fetch** ရပြန်ရော။ (Loading ထပ်စောင့်ရ)။

**The TanStack Way:**
ဒီပြဿနာတွေကို TanStack Query က ဖြေရှင်းပေးပါတယ်။

- **Caching:** Data ကို သိမ်းထားပေးလို့ Page အကူးအပြောင်းမှာ Loading ထပ်စောင့်စရာ မလိုတော့ဘူး။
- **Auto Refetch:** Browser tab ပြောင်းပြီး ပြန်လာတာနဲ့ Data အသစ်ရှိမရှိ Auto စစ်ပေးတယ်။
- **Boilerplate Code:** ကုဒ်လိုင်းအများကြီး ရေးစရာမလိုတော့ဘူး။

---

### ၂။ Basic Setup (စတင်အသုံးပြုခြင်း)

ပထမဆုံး `npm i @tanstack/react-query` နဲ့ install လုပ်ပါ။
Project ရဲ့ အပေါ်ဆုံး (Root) မှာ `QueryClientProvider` နဲ့ အုပ်ပေးရပါမယ်။

```javascript
// main.jsx or App.jsx
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient();

function App() {
  return (
    // App တစ်ခုလုံးကို အုပ်ပေးလိုက်ပါ
    <QueryClientProvider client={queryClient}>
      <YourComponent />
    </QueryClientProvider>
  );
}
```

---

### ၃။ Data Fetching (useQuery) - Working Level

Data ယူတဲ့နေရာမှာ `useQuery` ကို သုံးပါတယ်။ ဒီနေရာမှာ အရေးအကြီးဆုံးက **Unique Key** ပါ။

```javascript
import { useQuery } from "@tanstack/react-query";
import axios from "axios";

function UserProfile() {
  // useQuery အသုံးပြုပုံ
  const { data, isLoading, isError, error } = useQuery({
    queryKey: ["user", 1], // Key က Unique ဖြစ်ရမယ်။ ['user', userId] ပုံစံပေးလေ့ရှိတယ်။
    queryFn: async () => {
      const res = await axios.get("https://api.example.com/user/1");
      return res.data;
    },
    staleTime: 1000 * 60 * 5, // 5 မိနစ်အတွင်း Data အသစ်ထပ်မယူဘူး (Cache သုံးမယ်)
  });

  if (isLoading) return <div>Loading...</div>;
  if (isError) return <div>Error: {error.message}</div>;

  return <div>Name: {data.name}</div>;
}
```

**Working Level Tip:** `queryKey` ကို Array `['users', userId, { filter: 'active' }]` ပုံစံမျိုး သုံးတာက လုပ်ငန်းခွင် Standard ပါ။ Key ထဲက တစ်ခုခုပြောင်းတာနဲ့ Auto refetch လုပ်ပေးပါတယ်။

---

### ၄။ Data Update/Create (useMutation) - Intermediate

Data ယူတာမဟုတ်ဘဲ Server ပေါ်မှာ Data သွားပြင်တာ (POST, PUT, DELETE) ဆိုရင် `useMutation` ကို သုံးပါတယ်။
ဒီနေရာမှာ အရေးကြီးဆုံးက **Data ပြင်ပြီးရင် အဟောင်းတွေကို Invalidate (အသုံးမဝင်တော့ဘူးလို့ သတ်မှတ်)** လုပ်ဖို့ပါပဲ။

```javascript
import { useMutation, useQueryClient } from "@tanstack/react-query";

function AddUser() {
  const queryClient = useQueryClient();

  const mutation = useMutation({
    mutationFn: (newUser) => {
      return axios.post("/users", newUser);
    },
    // Server မှာ Data ထည့်ပြီးတာနဲ့ ချက်ချင်းအလုပ်လုပ်မည့် Function
    onSuccess: () => {
      // 'users' key နဲ့ cache လုပ်ထားသမျှကို ဖျက်ပြီး အသစ်ပြန်ဆွဲခိုင်းတာ
      // ဒါမှ List ထဲမှာ User အသစ် ချက်ချင်းပေါ်လာမှာ
      queryClient.invalidateQueries({ queryKey: ["users"] });
      alert("User added successfully!");
    },
  });

  return (
    <button
      onClick={() => mutation.mutate({ name: "Mr. Kan" })}
      disabled={mutation.isPending}
    >
      {mutation.isPending ? "Adding..." : "Add User"}
    </button>
  );
}
```

---

### ၅။ Advanced Concepts (Must Know)

လုပ်ငန်းခွင်မှာ Senior တစ်ယောက်အနေနဲ့ ဒီ Concept (၂) ခုကို ကွဲကွဲပြားပြား သိထားရပါမယ်။

#### **A. Stale Time vs GC Time (Cache Time)**

Developer တော်တော်များများ ရောထွေးတတ်ပါတယ်။

- **`staleTime` (Freshness):** "Data က ဘယ်လောက်ကြာကြာ လတ်ဆတ်နေသေးလဲ?"

  - Default က `0` ပါ။ ဆိုလိုတာက Fetch ပြီးတာနဲ့ ချက်ချင်း "ရိုးသွားပြီ (Stale)" လို့သတ်မှတ်ပြီး နောက်တစ်ခါ လိုရင် Server ကနေ ပြန်ဆွဲပါတယ်။
  - `staleTime: 5000` (5 စက္ကန့်) ထားလိုက်ရင်၊ ၅ စက္ကန့်အတွင်း ဒီ Component ကို ပြန်လာရင် API မခေါ်တော့ဘဲ Cache ထဲက Data ကိုပဲ ချက်ချင်းပြပါမယ်။

- **`gcTime` (Garbage Collection):** "Data ကို Memory ထဲမှာ ဘယ်လောက်ကြာကြာ သိမ်းထားမလဲ?"

  - Component unmount ဖြစ်သွားရင် (Page ပြောင်းသွားရင်) Data က Cache (Memory) ထဲမှာ ကျန်နေပါတယ်။
  - Default က 5 မိနစ်ပါ။ ၅ မိနစ်ကျော်လို့ ပြန်မလာရင် အဲ့ဒီ Data ကို Memory ထဲက ဖျက်လိုက်ပါမယ်။

#### **B. Window Focus Refetching**

User က တခြား Tab ကို သွားဖတ်ပြီး ကိုယ့် Web App ဆီ ပြန်လာတဲ့အချိန်မှာ Data ကို Auto update လုပ်ပေးတာပါ။ Default က `true` ပါ။ Real-time သဘောဆန်တဲ့ App တွေမှာ အရမ်းအသုံးဝင်ပါတယ်။

---

### ၆။ Mr. Kan's Pro Tips (Folder Structure)

Project ကြီးလာရင် `useQuery` တွေကို Component ထဲမှာ တိုက်ရိုက်မရေးပါနဲ့။ **Custom Hooks** အနေနဲ့ ခွဲထုတ်ပါ။

`src/hooks/useTodos.js`

```javascript
export const useTodos = () => {
  return useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
    staleTime: 1000 * 60, // 1 minute
  });
};
```

Component ထဲမှာတော့ ဒီလိုပဲ သန့်သန့်လေး ခေါ်သုံးပါ။
`const { data } = useTodos();`

---

**နိဂုံးချုပ်**
TanStack Query ဟာ Frontend Development မှာ Server State Management အတွက် မရှိမဖြစ် လက်နက်တစ်ခု ဖြစ်ပါတယ်။ Code ကို သန့်ရှင်းစေတယ်၊ User Experience (Loading မကြာတာ) ကို ကောင်းမွန်စေပါတယ်။
