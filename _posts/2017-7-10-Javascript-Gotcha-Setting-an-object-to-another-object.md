---
layout: post
title: Javascript Gotcha: Setting an object to another object (object cloning)
comments: true
---

If you have an object, say the following:
```
var a = {
  property: "value"
}
```

And you want to assign this object to another variable so you can manipulate it without affecting the first object, you would think you could do the following:
```
var b = a;
```

This appears to work fine until you start manipulating `b` and find that `a` gets manipulated as well. Unlike variables, integers, etc in javascript, assigning objects to objects creates a reference, not an actual copy. So to create the expected experience, you can do the following:
```
var b = JSON.parse(JSON.stringify(a));
```

You can now manipulate `b` while leaving `a` untouched.
