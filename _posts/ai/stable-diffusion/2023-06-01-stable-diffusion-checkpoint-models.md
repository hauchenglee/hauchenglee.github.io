---
layout: post
title: Stable Diffusion Checkpoint Models
category: blog
tags: [ai, stable-diffusion]
---

## 模型的概念及原理

模型：提供給AI圖片以及AI針對他們做學習的這個過程，並打包成文件

變分自解碼器 (VAE):
- 負責將加躁后的潛空間數據轉化為正常的圖像，也就是調色濾鏡，影響畫面的色彩質感
- 目前多數比較新的模型，其實都已經把VAE整合進大模型文件裏了，但也不排除少數沒有的（例如: FaceBombMix）
- 可以調整成跟checkpoint一樣的文件名，在啟動時就可以自動加載VAE

## 文件構成和加載位置

路徑：
- models: \sd-webui-aki-v4\models\Stable-diffusion
- VAE: \sd-webui-aki-v4\models\VAE

文件名後綴：
- ckpt 3-7GB
- safetensor 1-2GB

## 模型下載渠道分析

- [Hugging Face – The AI community building the future.](https://huggingface.co/)
- [Civitai \| Stable Diffusion models, embeddings, LoRAs and more](https://civitai.com/)

## 模型分類

### 二次元模型

模型：
- Anything V5: 廣大二次元
- Counterfeit V2.5: 精緻
- Dreamlike Diffusion: 歐美風

其他也推薦的：
- 深淵橘
- DreamShaper
- Meina Mix
- Cetus Mix
- Pastel Mix
- DalcefoPainting

關鍵詞：
- illustration
- painting
- sketch
- drawing
- comic
- anime
- catoon

### 寫實模型

模型：
- Deliberate
- Realistic Version
- L.O.F.I

關鍵詞：
- photography
- photo
- realistic
- photorealistic
- RAW photo

### 2.5D模型

模型：
- Never Ending Dream (NED)
- Protogen (Realistic)
- 国风3 (GuoFeng3) 

關鍵詞：
- 3D, render
- chibi
- digital art
- concept art
- realistic

### 其他模型

- Cheese Daddy's Landscapes mix: 魔幻感場景
- dvArch - Multi-Prompt Architecture Tuned Model: 現代感建築
- Graphic design_2.0: 高級感平面設計

---