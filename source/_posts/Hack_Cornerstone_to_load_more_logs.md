title: Hack Cornerstone to load more logs
date: 2017-11-02 17:51:44
categories: Development
tags: [Mac]

---

Cornerstone 每次只能 load 50 条 logs，所以想看 log 的时候，需要频繁点击 load 按钮，非常麻烦。

用 Hopper Disassembler 分析了一下它的代码，发现下面几处被 hardcode 成了 50：

* `[V4LogTableViewController init]`

* `[V4LogTableViewController controller]`

* `[V4Log init]`

* `[V4Log log]`

将这些函数里面的 `mov edx, 0x32` 改成 `mov edx, 0xff`，这样每次就可以 load 255 条 logs 了。
