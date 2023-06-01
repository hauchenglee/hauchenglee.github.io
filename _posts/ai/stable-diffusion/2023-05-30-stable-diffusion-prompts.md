---
layout: post
title: Stable-diffusion Prompts
category: blog
tags: [ai, stable-diffusion]
---

## 提示詞基本概念

提示詞: 告訴AI我們要畫什麼

## 提示詞分類和書寫方式

規則: 
1. 英文書寫
2. 以詞組為單位

內容型提示詞: 
- 人物及主體
  - 服飾穿搭: white dress
  - 髮型髮色: blonde hair, long hair
  - 五官特點: small, big mouth
  - 面部表情: smilling
  - 肢體動作: stretching arms
- 場景
  - 室內室外: indoor, outdoor
  - 大場景: forest, city, street
  - 小細節: tree, bush, white flower
- 環境光照: 
  - 白天黑夜: day, night
  - 特定時段: morning, sunset
  - 光環境: sunlight, bright, dark
  - 天空: blue sky, starry sky
- 畫面視角
  - 距離: close-up, distant
  - 人物比例: full body, upper body
  - 觀察視角: from above, view of back
  - 鏡頭類型: wide angle, Sony A7 III

標準化提示詞: 
- 畫質: 
  - 通用畫質: 
    - best quality,ultra detailed,masterpiece,hires,8k
    - 最高的质量，超级细节，杰作，高分辨率，8k（分辨率）
  - 特定高分辨率類型: 
    - extremely detailed CG unity 8k wallpaper
    - 超级细节的Unity CG壁纸
    - unreal engine rendered
    - 虛幻引擎渲染
- 畫風: 
  - 插画风: illustation,painting,paintbrush
  - 二次元: anime,comic,game CG
  - 写实系: phototealistic,realistic,photograoh

## 權重

括號加數字：
- `(white flower: 1.5)`
- `(white flower: 0.8)`

套括號：
- 圓括號 `(((white flower)))` → 1.1 * 1.1 * 1.1
- 大括號 `{{{white flower}}}` → 1.05 * 1.05 * 1.05
- 方括號 `[[[white flower]]]` → 0.9 * 0.9 * 0.9

權重安全值：1 +- 0.5

進階語法：
- 混合: `white | yellow flower` 生成黃色和白色混合的花
- 遷移: `[white|red|blue] flower` 先生成白花，再生成紅花，再生成藍花
- 迭代: `(white flower:bush:0.8)` 進程到達80% (0.8) 之前生成白花，80%之後再生成灌木

## 出圖參數詳解

採樣迭代步數（steps）: 10~40, avg: 20

採樣方法（sampler）: 算法
- euler a: 適合插畫，出圖樸素
- euler: 適合插畫，出圖樸素
- 選 + 號

寬度/高度 → 小尺寸 + 高清修復

提示詞相關性（CFG Scale）: 
- 數值越高，AI忠實反映提示詞程度就越高
- avg: 7~12

隨機種子（seed）

## 總結

1. 翻譯
2. 藉助工具
3. 抄作業

## Reference

教程：
- [课程学习资料链接：Stable Diffusion 零基础入门课学习资料汇总 - 幕布](https://mubu.com/doc/_2As4DSE4m)

提示詞：
- [全网最全的AI绘画提示词网站，看这一篇就够了！_办公软件_什么值得买](https://post.smzdm.com/p/all0mv3p/)
- [AI绘画提示词生成器 - 一个工具箱 - 好用的在线工具都在这里！](http://www.atoolbox.net/Tool.php?Id=1101)
- [AI词汇加速器 AcceleratorI Prompt](https://ai.dawnmark.cn/)

參考：
- [Discover and Generate AI Art \| OpenArt](https://openart.ai/)
- [Arthub.ai: Discover, Upload and Share AI Generated Art](https://arthub.ai/)

---