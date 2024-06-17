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


## 部分作品展示（由于设备性能问题，暂时不追求极致的画面质量）

> **1**   checkpoint:基于国风武侠/Chosen Chinese styl 大模型   Trigger Words:CHINESE STYLE   lora:CG古风

![img](/img/AI_post/Snipaste_2024-06-17_14-39-53.jpg)         ![img](/img/AI_post/Snipaste_2024-06-17_14-52-03.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_14-53-28.jpg)         ![img](/img/AI_post/Snipaste_2024-06-17_14-54-30.jpg)

```
prompt:-Hanama wine, 1boy, male focus, black hair, solo, sitting, long sleeves, water, robe,
smoking, short hair, ripples, cigarette, glasses, chinese clothes<lora:Hanama wine-000018:0.8>,


Negative prompt: EasyNegative,badhandsv5-neg,Subtitles,word,
```




> **2**  checkpoint:XXMixDream, SDXL模拟器, 手绘/彩绘/手工画/游戏原画   lora:鎏金暗香, 凤凰_1.0  奇妙的光影 | wonderful light and shadow

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

![img](/img/AI_post/checkpoint_ghost.jpg)


![img](/img/AI_post/Snipaste_2024-06-17_15-07-56.jpg)

```
prompt:
Tea room\(cement wall,Dark toned lighting,night,bamboo curtain,depth of field,pool,interior design building,
smooth panel,houzz,elegant interior,residential design,beautiful rendering of the Tang Dynasty,dim toner light,Photorealism\),Background\(snow,mountains,forest\),
masterpiece,best quality,unreal engine 5 rendering,movie light,movie lens,movie special 
effects,detailed details,HDR,UHD,8K,CG wallpaper


negative:
blurry,low quality,bad anatomy,sketches,lowres,normal quality,worstquality,signature,watermark,cropped,bad proportions,out of focus, (worst quality, low quality:1.4),
monochrome,zombie,(interlocked fingers),(worst quality, low quality:2),monochrome,zombie,overexposure,watermark,text,bad anatomy,bad hand,extra hands,extra fingers,
(extra digit and hands and fingers and legs and arms:1.4),(deformed fingers:1.2),(long fingers:1.2),(bad-artist-anime),extra legs,lowres, bad anatomy, bad hands, text,
error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality,normal quality, jpeg artifacts, signature, watermark, username


Sampler
DPM++ 2M Karras
Steps
30
CFG
7


```


> **3**    checkpoint:星语 || 绘卷, MIRACLE-造梦空间 大师胶片电影风格模拟,   lora:AiARTiST UNIT XL 基础单元 CADS2兼容版  奇妙的光影 | wonderful light and shadow

![img](/img/AI_post/Snipaste_2024-06-17_15-32-49.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-35-21.jpg)

```
prompt:
Masterpiece,(Best Quality),universe,starry sky,an astronaut sitting on the surface of the moon,(from_below:1.2),
cinematic_angle,broken spacecraft on the ground,from behind,(starry night:1.2),(galaxy:1.2),
(look to the sky:1.1),looking_up,the sky occupies most of the picture,


Sampler
DPM++ 2M Karras
CFG
7

```


> **4**  checkpoint:EBMD丨INDOOR RENDERING室内渲染 XL, EBMD丨Vacation Villa XL

![img](/img/AI_post/Snipaste_2024-06-17_15-07-06.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-14-43.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-23-47.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-24-43.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-25-37.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-26-04.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-26-21.jpg)



> **5**  AiARTiST 专用ACG Playground SDXL, 国风仙丹-神话|Chinese Mythology-SDXL

![img](/img/AI_post/Snipaste_2024-06-17_15-36-58.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-44-45.jpg)

```
best quality,tortoise,no humans,scenery,fantasy,cloud,architecture,water,tree,east asian architecture,chinese mythology,in the style of green and cyan,full of imagination,unmatched composition,ultra-detailed,super high quality,(masterpiece, top quality, official art, beautiful and aesthetic:1.2),
extreme detailed,highest detailed,blue and green theme,



(low quality, worst quality:1.4),cgi,text,signature,watermark,extra limbs,Bad quality,nsfw,nsfw,


Sampler
DPM++ 2M Karras
Steps
30
CFG
7

```

---

## 2024大模型 lora微模型推荐

> **1**  GhostMix鬼混  二次元  画风加强  动漫画风  checkpoint

1.推荐一定要做高清修复! 高清修复: 2倍, 重绘幅度:0.4-0.5 或 1.5倍, 重绘幅度:0.5-0.65

2.如果想要复现,确保CLIP值要对! CLIP1和CLIP2要对!建议把图下下来然后放到PNG信息里面去查设置)

3.之前用的大多数Prompts在新版本也可以生成相似的结果

4.用 ng_deepnegative_v1_75t和easynegative,建议别用BadHandV4,V5（会非常影响画风）

5.采样方法建议 DPM++系列 , 步数20-30, CFG:3-7(7最好，3有时候会有非常大的惊喜)

![img](/img/AI_post/Snipaste_2024-06-17_15-06-02.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-10-38.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-13-13.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-13-34.jpg)


> **2**   lora:全息故障|Holographic_Fault, NORFLEET光影2.5D 融合, 赛博朋克写实

创意概述 ：赛博色彩/故障效果/全息点，如像素错误、干扰线、色彩失真、闪烁和几何畸变等。

生成建议 :  底模为Lyriel_v1.5，可适当使用高权重，画面色彩鲜艳更佳，机械、装甲、未来科技效果更好。

            用来做颜色干扰和衣服颗粒

![img](/img/AI_post/Snipaste_2024-06-17_15-26-52.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-28-45.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-30-25.jpg)

![img](/img/AI_post/Snipaste_2024-06-17_15-31-05.jpg)


> **3**  胶片风格 很炸裂的效果

![img](/img/AI_post/Snipaste_2024-06-17_15-49-48.jpg)


  








