---
layout: post
title: "中文领域语音技术栈高性价比开源方案"
category: daily
---


前前后后调研了差不多一年的中文语音技术栈。

最终在 2025 年 6 月 30 日，敲定了，在中文领域，语音技术的高性价比开源方案。

语音识别 ASR：whisper-large-v3，如果在垂直领域识别效果不好，则微调一下。

语音生成 TTS：Index-TTS，哔哩哔哩开源，如果怕有些文本模型不认识，可以在模型前面再封装一层未登录词管理，将不认识的词转换成模型认识的词。（注意要用这个加速版：https://github.com/Ksuriuri/index-tts-vllm）

资源占用情况：
whisper-large-v3 跑起来只需要不到 4GB 的显存。index-tts 只需要 1GB 显存。whisper-large-v3 微调会吃力一些，大概占用 150GB 显存。

模型性能：
在 RTX4060 显卡上，一段 10 秒的语音转换成文字只需要 500ms 左右，50个字的短句，生成时间只需要一秒左右。

在一块消费级显卡上，用 whisper-large-v3 和 index-tts，基本上能取得性能和成本的平衡。
