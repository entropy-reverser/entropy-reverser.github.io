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
