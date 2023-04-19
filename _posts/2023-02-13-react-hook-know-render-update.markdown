---
layout: post
title:  "React-16 render: How to know it is render or update(re-render)"
date:   2023-02-14 11:00:00 +0600
img: post/react-hooks.jpg
categories: react
comments: true
---

One common question arise when debugging simpler functional components with state. Is this dom change is due to the first time render or re-renders(update).
Use effect hooks in your advantage. Keep the second argument of the useEffect hook undefined (don't provide it), and in hook body check for the state value. Is it initial state or not.

```
useEffect(() => {
        if (!rows.length) {
            console.log("render");
        } else {
            console.log("update");
        }
    });
```
