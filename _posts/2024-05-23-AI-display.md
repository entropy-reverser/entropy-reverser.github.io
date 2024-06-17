---
layout:     post
title:      "基于SD扩散模型的生成作品展示"
subtitle:   "使用SD Web UI工作流，基于多种checkpoint大模型配合lora controlnet等微调模型"
date:       2024-05-27 12:00:00
author:     "逆熵人"
header-img: "img/bg-me-2022.jpg"
catalog: true
tags:
    - AI
    - Stable Diffusion
    - 大模型
    - checkpoint 
    - lora
    - controlnet
---


## 前言

> High-Resolution Image Synthesis with Latent Diffusion Models

[stable-diffusion](https://github.com/CompVis/stable-diffusion)  是一种潜在的文本到图像扩散模型，能够在给定任何文本输入的情况下生成照片般逼真的图像
Stable Diffusion 是基于latent-diffusion 并与 Stability AI and Runway合作实现的.

### 扩散模型如何应用在图像中（Diffusion Models）
扩散模型包括两个过程：前向过程（forward process）和反向过程（reverse process），
其中前向过程又称为扩散过程（diffusion process）。
扩散过程是指的对数据逐渐增加高斯噪音直至数据变成随机噪音的过程。

### 什么是 latent-diffusion模型？
diffusion 与 latent diffusion的区别，可以理解为 diffusion直接在原图进行图片的去噪处理，而 latend diffusion 是图像经过VAE编码器压缩的图像，
进行diffusion处理，然后再通过解码器，对压缩后的latent 编码还原为图像。

SD主体结构主要包括三个模型：**autoencoder** （variantional auto-encoder）：encoder将图像压缩到latent空间，而decoder将latent解码为图像；
**CLIP text encoder**：提取输入text的text embeddings，通过cross attention方式送入扩散模型的UNet中作为condition；
**UNet**：扩散模型的主体，用来实现文本引导下的latent生成



![img](/img/AI_post/latent.png)

---


## 作品展示（由于设备性能问题，暂时不追求极致的画面质量）

> **1**   checkpoint:基于国风武侠/Chosen Chinese styl 大模型   Trigger Words:CHINESE STYLE   lora:CG古风

![img](/img/AI_post/Snipaste_2024-06-17_14-39-53.jpg)         ![img](/img/AI_post/Snipaste_2024-06-17_14-52-03.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_14-53-28.jpg)         ![img](/img/AI_post/Snipaste_2024-06-17_14-54-30.jpg)

```
prompt:-Hanama wine, 1boy, male focus, black hair, solo, sitting, long sleeves, water, robe,
smoking, short hair, ripples, cigarette, glasses, chinese clothes<lora:Hanama wine-000018:0.8>,


Negative prompt: EasyNegative,badhandsv5-neg,Subtitles,word,
```




> **2**  checkpoint:XXMixDream, SDXL模拟器, 手绘/彩绘/手工画/游戏原画   lora:鎏金暗香, 凤凰_1.0

![img](/img/AI_post/Snipaste_2024-06-17_15-01-44.jpg)      
```
prompt:
(fèng huáng,wings,phoenix):1.3,sky,cloud,front view,(look at viewer,outdoor,full body):1.7\),
Background\((full moon,flower,daisy,birds,lily,lotus,grassland):1.5,sky,forest,lake\),
masterpiece,best quality,unreal engine 5 rendering,movie light,movie lens,movie special effects,detailed details,HDR,UHD,8K,CG wallpaper


negative:
blurry,low quality,bad anatomy,sketches,lowres,normal quality,worstquality,signature,
watermark,cropped,bad proportions,out of focus, (worst quality, low quality:1.4),
monochrome,zombie,(interlocked fingers),(worst quality, low quality:2),monochrome,
zombie,overexposure,watermark,text,bad anatomy,bad hand,extra hands,extra fingers,(extra digit and hands and fingers and legs and arms:1.4),(deformed fingers:1.2),(long fingers:1.2),(bad-artist-anime),extra legs,lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, 
normal quality, jpeg artifacts, signature, watermark, username

Sampler
DPM++ 2M Karras
Steps
30
CFG
7

```



