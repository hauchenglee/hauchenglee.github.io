---
layout: post
title: Stable Diffusion Generate Image
category: AI
tags: [ai, stable-diffusion]
---

### 圖生圖三個基本步驟

步驟：
1. 導入圖片
2. 書寫提示詞
3. 參數調整

### 參數技術性分析

- 重繪幅度（Denoising）：0.6~0.8
- 尺寸：與原圖相同

### 隨機種子的含義探究

- 骰子：設置-1，每次隨機生成全新圖像
- 循環按鈕：設置上一張生成圖的種子值
- 反推提示詞：
  - CLIP
  - DeepBooru (v)

### 圖生圖拓展應用


---