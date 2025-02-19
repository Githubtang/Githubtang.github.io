---
title: "使用 MLstudio 运行大模型服务"
description: "本地使用模型：deepseek-r1-distill-llama-8b"
date: 2025-02-19
draft: false
tags: ["MLstudio", "大模型", "深度学习"]
---

## 介绍

MLstudio 是一个强大的工具，用于运行和管理大语言模型。在本文中，我们将介绍如何在本地环境中使用 MLstudio 部署和运行一个预训练的模型：`deepseek-r1-distill-llama-8b`😈

## 环境准备

在开始之前，请确保你已经安装了以下工具：
- MLstudio 客户端

## 下载和部署模型

1. **安装 MLstudio 客户端**  
   通过以下命令安装 MLstudio 客户端：
   ```bash
   pip install mlstudio
    ```
   或直接去官网下载 [mlstudio地址](https://lmstudio.ai/)
2. **安装 Deepseek 模型**

    将模型文件放在lmstudio模型目录下
    {{< figure
    src="下载Deepseek.png"
    alt="#"
    caption="Photo by [Jr Korpa](https://unsplash.com/@jrkorpa) on [Unsplash](https://unsplash.com/)"
    >}}