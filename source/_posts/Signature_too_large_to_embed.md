title: How to solve codesign issue "signature too large to embed"
date: 2017-08-22 17:30:52
categories: Development
tags: [Xcode]

---

最近在更换了苹果开发者证书后，用 *codesign* 签名时会报错：*signature too large to embed*。大意就是签名数据太大，超出了限制。

**根本原因**

通过查看 *codesign* 的[源代码] [1]发现，它会对签名数据的大小进行限制，默认限制是 9000。

`state.mCMSSize = 9000;`

可以通过 `--signature-size` 这个[私有参数] [2]来手动控制该限制的值，而不使用这个默认值。该参数并不是 *codesign* 的公开参数。

**具体用法**

`codesign --signature-size 10000 -s "XXX"`

网上看到有人说利用 `--timestamp=none` 可以解决该问题，但其实是治标不治本。`--timestamp=none` 本身会减少签名数据的大小，所以才看似有效果。

<!--more-->

-----

**查看源代码的小窍门**

* 查看：https://opensource.apple.com/source/libsecurity_codesigning/
* 下载：https://opensource.apple.com/tarballs/libsecurity_codesigning/

区别就是链接里面的 *source* 和 *tarballs*。

-----

[1]: https://opensource.apple.com/source/libsecurity_codesigning/libsecurity_codesigning-55037.15/lib/CodeSigner.cpp.auto.html
[2]: https://opensource.apple.com/source/security_systemkeychain/security_systemkeychain-55202/src/codesign.cpp.auto.html
