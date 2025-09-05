---
layout: post
title:  "Javascript singleton constructor: importing module is skipping import cache"
date:   2025-09-05 00:00:00 +0600
img: post/singleton-js.jpg
categories: newsmil
comments: false
---


browser bundle এ module একবার evaluate হয়ে cache হয়ে যায়। 
আলাদা bundle chunk-এ পড়ে গেলে বা tree-shaking/config এর কারণে একই মডিউল **বারবার evaluate হতে পারে**

---

## সম্ভাব্য কারণ

### 1. **Relative import path mismatch**

এক জায়গায় এভাবে:

```js
import { registry } from "./registry.js";
```

আর অন্য জায়গায়:

```js
import { registry } from "../some/../registry.js";
```

→ Next.js আলাদা module মনে করবে, তাই দুবার load হবে।

✅ সমাধান: সব জায়গায় একদম **একই import path** ব্যবহার করুন।
যদি দরকার হয় alias (`@/registry`) সেট করুন।

---

### 2. **Hot reload / Fast Refresh (Next.js Dev mode)**

Dev mode এ React Fast Refresh এর কারণে কিছু module re-evaluate হতে পারে।
Production build (`next build && next start`) এ সাধারণত এই সমস্যা থাকে না।

---

### 3. **Dynamic import / async chunk split**

যদি আপনি কোথাও করেন:

```js
await import("./registry.js");
```

এবং অন্য কোথাও static import করেন:

```js
import { registry } from "./registry.js";
```

→ আলাদা chunks এ split হয়ে multiple evaluate হতে পারে।

---

### 4. **Server + Client দুটোতেই evaluate হচ্ছে, কিন্তু console এ দেখাচ্ছে client থেকে**

Next.js App Router এ অনেক code server-client boundary mix করে। একি registry ফাইল যদি `"use client"` component এ import করা হয়, আবার server code এও import করা হয় → দুবার evaluate হয় (server copy + client copy)।

---

## Debug করার উপায়

console.log দিন:

```js
console.log("Registry module evaluated", Date.now());

class Registry {
  constructor() {
    console.log("Registry constructor called", Date.now());
    this.overrides = new Map();
  }
  register(key, fn) {
    this.overrides.set(key, fn);
  }
  resolve(key) {
    return this.overrides.get(key);
  }
}

export const registry = new Registry();
```

* **"Registry module evaluated"** কয়বার আসে?
* কয়েকবার আসলে তবে import path mismatch / multiple chunk issue।
* শুধু constructor বারবার আসলে, Fast Refresh / HMR issue হতে পারে।


