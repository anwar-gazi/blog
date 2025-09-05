---
layout: post
title:  "Node.js প্রজেক্টে "type": "module" ব্যবহার করলে আসলে কী হয়?"
date:   2025-09-05 00:00:00 +0600
img: post/type-module.jpg
categories: Newsmil CMS: Online Newspaper
comments: false
---


TLDR;
package.json ফাইলে `"type": "module"` দিলে প্রজেক্টের `.js` ফাইলগুলোকে ডিফল্টভাবে ECMAScript Module (ESM) হিসেবে ধরা হয়। কোডবেসকে করে আরও স্ট্যান্ডার্ড, ক্লিন আর মেইন্টেইনেবল।

🔹 **Module Syntax**

* CommonJS (CJS)-এর `require()` এবং `module.exports` এর পরিবর্তে `import` এবং `export` — এটা আধুনিক স্ট্যান্ডার্ড।

🔹 **File Extensions**

* `.js` ফাইলকেই সরাসরি ESM হিসেবে ব্যবহার করা যায়। `.mjs`, `.cjs` এসব এক্সটেনশান নিষ্প্রয়োজন।

🔹 **Scoped Variables**

* (ESM) টপ-লেভেলে ডিফাইন করা ভ্যারিয়েবল শুধু সেই মডিউলের মধ্যে থাকে, গ্লোবাল হয় না।

🔹 **\_\_dirname এবং \_\_filename**

* নেই। `import.meta.url` ব্যবহার করা যায়।

🔹 **JSON Modules**

* সরাসরি ইম্পোর্ট।

  ```js
  import data from './data.json';
  ```


---