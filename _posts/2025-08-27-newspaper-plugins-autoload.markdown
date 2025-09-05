---
layout: post
title:  "অনলাইন নিউজপেপার প্রজেক্টে ডাইনামিক প্লাগিন লোড করা, প্লাগিন রেজিষ্ট্রি ব্যাবহার করে | nextjs+ts+laravel "
date:   2025-08-27 00:00:00 +0600
img: post/newspaper-plugins-autoload.jpg
categories: Newsmil CMS: Online Newspaper
comments: false
---


#  অনলাইন নিউজপেপার প্রজেক্টে ডাইনামিক প্লাগিন লোড করা, প্লাগিন রেজিষ্ট্রি ব্যাবহার করে | nextjs+ts+laravel 

সম্প্রতি আমাদের অনলাইন নিউজপেপার প্রজেক্টে প্লাগিন অটোলোড ও ফাইল ওভারলোডিংয়ের জন্য ওয়েবপ্যাককের aliasing বাদ দিতে হয়েছে। এ ব্যাপারে পূর্ববর্তী পোষ্টে ডিটেল আছে। এখানে আমরা প্লাগিন গুলোর ইনডেক্স ফাইল অটোলেোড করছি next.config.js ফাইল এ

---

## CommonJS থেকে ESM এ next.config.js

1. **`require` → `import`**
   CommonJS-এ:

   ```js
   const fs = require("fs");
   const path = require("path");
   ```

   ESM-এ:

   ```js
   import fs from "fs";
   import path from "path";
   import { fileURLToPath } from "url";
   ```

---

2. **`__dirname` ও `__filename` এর পরিবর্তন**
   CommonJS-এ এগুলো ডিফল্টভাবে পাওয়া যায়।
   কিন্তু ESM-এ এগুলো তৈরি করতে হয় এভাবে:

   ```js
   const __filename = fileURLToPath(import.meta.url);
   const __dirname = path.dirname(__filename);
   ```

---

3. **প্লাগিন Dynamic Import**
   CommonJS-এ ছিল:

   ```js
   require(path.join(pluginsDir, plugin_folder));
   ```

   ESM-এ করতে হয়েছে:

   ```js
   await import(path.join(pluginsDir, plugin_folder));
   ```

---

4. **`module.exports` → `export default`**
   আগে:

   ```js
   module.exports = nextConfig;
   ```

   এখন:

   ```js
   export default nextConfig;
   ```

---

## ফাইনাল কোড স্নিপেট (ESM)

```js
import fs from "fs";
import path from "path";
import { fileURLToPath } from "url";

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

function loadPlugins() {
  const pluginsDir = path.join(__dirname, "plugins");

  if (fs.existsSync(pluginsDir)) {
    const pluginFolders = fs.readdirSync(pluginsDir);
    pluginFolders.forEach(async (plugin_folder) => {
      try {
        await import(path.join(pluginsDir, plugin_folder));
      } catch (e) {
        console.error(`failed to load plugin ${plugin_folder}`, e);
      }
    });
  }
}
loadPlugins();

const nextConfig = {
  images: {
    remotePatterns: [
      { protocol: "https", hostname: "**" },
      { protocol: "http", hostname: "**" },
    ],
    domains: ["*"],
  },
};

export default nextConfig;
```

---


