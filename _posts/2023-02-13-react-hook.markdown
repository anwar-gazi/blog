---
layout: post
title:  "React-16 hooks for functional components and network request within it"
date:   2023-02-13 11:00:00 +0600
img: post/react-hooks.jpg
categories: react
comments: true
---

React effect hooks are run with useEffect for functional components. We know that. When no second argument is provided, the effect hook runs at every render and re-render(update).
Note the difference between reacts "render" and browser dom render. As react maintains it's own dom (virt. dom), it renders the virtual dom at first, then mounts the component.
The element is still not visible, only the virtual dom is rendered, remember that. Now after the component is mounted, it gets displayed or rendered to build browser dom.
When the component state gets update, the virtual dom also gets updated or "re-rendered".
For useEffect with no second argument, hook runs at render and at update. When your hook code has a network request, it fires two times. This network request promise fulfillment is also called two times.
This can easily get out of hand casing a performance issue.

If you want to run network request indefinitely at every render and update: write the useEffect hook with the network request and without the second argument for the hook, and for the network promise fulfillment, use setState.
