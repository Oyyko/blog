---
title: In archlinux, chrome cannot be displayed normally under AMD.
date: 2022-05-27
tags: [bash]
---
Solution:
```
rm -rf .config/google-chrome/Default/GPUCache
```