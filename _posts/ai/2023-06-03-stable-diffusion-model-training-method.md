---
layout: post
title: Stable Diffusion Embeddings / LoRa / Hypernetwork
category: AI
tags: [ai, stable-diffusion]
---

## Embeddings 文本嵌入

### 作用及原理

瞭解能對AI索引信息產生影響的文本嵌入（Embeddings）模型的基本概念，並深入探討其幾種常見應用。
1. 詞嵌入模型的基本概念
2. 使用Embeddings描繪對象或概念
3. 利用負面詞嵌入消除錯手

Checkpoint: 大字典

Embeddings: 小書籤

C站: Textual Inversion

### 調用

後綴名: pt

目錄: \sd-webui-aki-v4\sd-webui-aki-v4\embeddings

用咒語召喚，不需要特地調用

### 應用

- [CharTurner - Character Turnaround helper for 1.5 AND 2.1! - CharTurner V2 - For 1.5 \| Stable Diffusion Textual Inversion \| Civitai](https://civitai.com/models/3036/charturner-character-turnaround-helper-for-15-and-21)
- [EasyNegative - EasyNegative \| Stable Diffusion Textual Inversion \| Civitai](https://civitai.com/models/7808/easynegative) → 基於counterfeit訓練，作者測試對大多數二次元模型都有效
- [Deep Negative V1.x - V1 75T \| Stable Diffusion Textual Inversion \| Civitai](https://civitai.com/models/4629?modelVersionId=5637) → 針對真人模型訓練

## LoRa 低秩模型

### 作用及原理

Low-Rank Adaption Model

瞭解用於描繪特定對象內容的低秩模型（LoRa）的基本概念，並簡單實踐其基本應用流程。
1. 低秩模型的基本概念
2. LoRa的作用與應用簡析

LoRa: 夾在裏面的彩頁，直接在紙上寫明白主體是什麼以及特點，能更加全面準確

## 調用

目錄: \sd-webui-aki-v4\models\Lora

調用: `<lora:lora文件名>` or 特定的觸發提示詞（依作者建議而定）

權重: 0.5-0.8 or 依作者建議而定

## 應用

1. 人物角色形象
2. 畫風或風格
3. 概念
4. 服飾
5. 物體/特定元素

## Hypernetwork 超網絡

瞭解用於塑造特定畫風的超網絡（Hypernetwork）模型的基本概念，並簡單實踐其基本應用流程。
1. 超網絡的基本概念
2. Hypernetwork的作用與應用簡析

與LoRa相似，但更適用於描繪圖畫的整體畫風

路徑: \sd-webui-aki-v4\models\hypernetworks

調用: 設置 → 附加網絡 → 將超網絡添加到提示詞 → 選擇對應的超網絡

應用：
- Q版: [[Toru8P] Waven Chibi Style - wavenchibi v1.0b \| Stable Diffusion Hypernetwork \| Civitai](https://civitai.com/models/4379/toru8p-waven-chibi-style)
- 雕塑: [Dantion Marble Statues Hypernetwork - 1.0 \| Stable Diffusion Hypernetwork \| Civitai](https://civitai.com/models/3810/dantion-marble-statues-hypernetwork)
- 像素: [[LuisaP] Pixel art Hypernetwork - v1\| Stable Diffusion Hypernetwork \| Civitai](https://civitai.com/models/3962/luisap-pixel-art-hypernetwork)
- 抽象: [[LuisaP]Abstract Painting - abstract version \| Stable Diffusion Hypernetwork \| Civitai](https://civitai.com/models/4853?modelVersionId=5579)

---