---
layout: post
title: Clean Code 無暇的程式碼- Ch1 無暇的程式碼
category: it
tags: [design-pattern]
---

[Clean Code 無暇的程式碼：敏捷軟體開發技巧守則](https://www.books.com.tw/products/0010579897){:target="_blank"}

作者： Robert C. Martin

---

## 我的前言

期待已久的書，一直很想閱讀，最近買了這本書，終於有機會有時間拜讀了。

在寫項目的時候，常常覺得自己寫的程序不夠乾淨、整潔、清爽，有很多冗贅的代碼。一來是時間上時程內的產出，當然最主要是自己本身實力不足、經驗不夠。
許多時候為了快速開發，選擇便宜行事的方式，代碼質量就被剝奪到一旁，除了版本迭代過多以外，也造成越來越難改動。很多次都想要整體重構，但是一想想所需要花費的時間、人力、精力，
每次都打退堂鼓。

關於重構項目，曾經我試著重構項目中的一個功能，雖然確實在一部分內成功砍掉1000行代碼，但是隨之而來的是架構大幅更動，並且前後端都要重新設計，調用新的API。
工程浩大，後來就放棄這件事了。倒是在重構中的技巧有運用到後續的項目，倒也有收穫。

於是，寫出一個漂亮、乾淨整齊，並且高可用性、相容性、擴充性，充分運用設計模式，以及好的閱讀體驗的代碼，一直是我心中的痛。當然我也知道這需要時間與經驗，
就像一個月後看一個月前自己的代碼，都會不忍直視：誰寫的！然後翻翻Git提交記錄，再捂臉自己。太羞恥了，都快看不懂自己在寫啥了！

我同意一句話：代碼其實從寫完就持續在腐敗，所以要不停的維護。

這個系列的文章就是看完【Clean Code 無暇的程式碼：敏捷軟體開發技巧守則】後的點點滴滴、筆記與心得。期望看完這本書後能寫出Clean Code【無暇的程式碼】，更能讓我編程的思維提升。

## 筆記心得

讓開發速度變快的唯一方式，就是隨時保持代碼整齊乾淨。

設計原則：
- 单一职责原则（Single Responsibility Principle）
- 开放封闭原则（Open Close Principle）
- 里氏替换原则（Liskov Substitution Principle）
- 依赖倒置原则（Dependence Inversion Principle）
- 接口隔离原则（Interface Segregation Principle）
- 迪米特原则（Law of Demeter）

Ref:
- [设计模式6大原则 - 掘金](https://juejin.im/post/5a52144d6fb9a01c9b65c651){:target="_blank"}
- [设计模式六大原则](http://www.uml.org.cn/sjms/201211023.asp){:target="_blank"}

---
