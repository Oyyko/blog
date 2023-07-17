---
title: Texmacs/Mogan Note 2
date: 2023-07-13
tags: [note, code, mogan, texmacs]
---
# 23_15
```scheme
(get-init-tree "text-at-halign")
```
这个与缓存有关。缓存位置在`~/.Texmacs/`。如果不更改缓存，更改源代码不会生效。

# 23_7
```scheme
(tm-define (graphics-release-left x y)
  ;;(display* "Graphics] Release-left " x ", " y "\n")
  (if (inside-graphical-text?)
      (with-innermost t graphical-text-context?
        (let* ((ps (select-first (s2f x) (s2f y)))
               (p (and ps (car ps))))
          (if (and p (list-starts? p (tree->path t)))
              (go-to p)
              (tree-go-to t :start))))
      (edit_left-button (car (graphics-mode)) x y)))
```

```scheme
(tm-define (get-keyboard-modifiers)
  the-keyboard-modifiers)

(tm-define (set-keyboard-modifiers mods)
  (set! the-keyboard-modifiers mods))
```

结论：https://github.com/XmacsLabs/mogan/pull/796


# 问题
1. .ts文件怎么使用
2. C++代码中有很多字符串类型的东西可以用枚举类型来替代，可以重构一下？
3. 绘图模式有没有给座标系标注1、2、3的模式

