---
title: 用 edge-tts 把文字变成语音（含可用语音库与交互脚本）
date: 2024-12-15 17:51:09
tags:
  - TTS
  - Python
  - edge-tts
categories:
  - 工具与效率  
---

>
> 在打服务外包的时候，看到不少 edge-tts 的教程，但要么只给了最基础指令，要么语音清单/参数不全，实际动手总要四处拼资料。  
> 这篇文章尝试**一次性把常用指令、参数说明、可用语音名称、以及可交互的示例脚本**汇总在一起：复制即可用、需要时再按需扩展，减少你搭建 TTS 工作流的来回查找成本。

> 目标：快速把任意文本合成自然语音，支持多语言、男女声、语速、音量、音调与部分情绪风格。


## 1⃣️ edge-tts 是什么

`edge-tts` 是基于微软 Edge/SSML 的文本转语音 Python 库与命令行工具。它的特点：

- 支持多语言和地区方言（普通话、粤语、英语、法语、阿拉伯语等）
- 可调参数：语言/声音、性别、语速 `rate`、音量 `volume`、音调 `pitch`
- 部分声音支持情绪/风格（如 激情、活泼、温柔等）
- 纯本地调用云端服务，无需自行搭建推理引擎

项目地址：<https://github.com/rany2/edge-tts>

---

## 2⃣️ 安装

```bash
pip install edge-tts
```

如需命令行使用，安装上面这一条即可。

------

## 3⃣️ 最小可用示例

```python
import asyncio
import edge_tts

async def run():
    tts = edge_tts.Communicate("你好，edge tts！", "zh-CN-XiaoxiaoNeural")
    await tts.save("hello.mp3")

asyncio.run(run())
```

------

## 4⃣️ 命令行（无需写代码）

```bash
edge-tts --voice zh-CN-XiaoxiaoNeural --text "你好，这是命令行合成" --write-media out.mp3
```

列出可用声音：

```bash
edge-tts --list-voices
```

------

## 5⃣️ 交互式多语言脚本（含风格/情绪选择）

下面是按需求重构的**标准语音库**与交互脚本。直接复制运行即可（运行前先 `pip install edge-tts`）。脚本支持：选择语言/方言、性别、风格，并调节 `pitch / rate / volume`，自动命名输出文件。

```python
import asyncio
import edge_tts
import os

# 用户输入
input_text = input("请输入您想要生成的文本：")
selected_voice = input("请输入您要使用的语音（例如 zh-CN-XiaoxiaoNeural）：")
pitch = input("请输入音调（例如 +0Hz、+20Hz、-10Hz）：")
volume = input("请输入音量（例如 +0%、+20%、-10%）：")
rate = input("请输入语速（例如 +0%、+15%、-10%）：")

# 输出文件夹路径（可自行修改）
output_dir = r"E:\codecx\audio_example"
os.makedirs(output_dir, exist_ok=True)

# 自动生成输出文件名
output_file = os.path.join(output_dir, f"{selected_voice}_p{pitch}_v{volume}_r{rate}.mp3")

# 异步生成语音文件
async def amain():
    communicator = edge_tts.Communicate(
        input_text, selected_voice, pitch=pitch, rate=rate, volume=volume
    )
    await communicator.save(output_file)

if __name__ == "__main__":
    asyncio.run(amain())
    print(f"语音已生成：{output_file}")
```

其中selected_voice为表格文档第二列内容（【金山文档 | WPS云文档】 Language Name
https://www.kdocs.cn/l/co8YliHtxxvF），部分如图：

![1157X743/屏幕截图_2025-10-22_221413.png](https://tc.z.wiki/autoupload/f/zpduUOTToOUwtaMnTYFBl8kJiVcdDX0rG09M-a0gARI/20251022/UYCX/1157X743/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE_2025-10-22_221413.png)





将上面的 `output_dir` 改成你自己的存放目录即可。

------

## 6⃣️ 参数说明

- `--voice` / `selected_voice`：选择具体说话人，例如 `zh-CN-XiaoxiaoNeural`
- `--rate` / `rate`：语速，`+10%` 更快，`-20%` 更慢
- `--volume` / `volume`：音量，`+0%` 到 `+100%` 或 `-xx%`
- `--pitch` / `pitch`：音调，`+20Hz` 更尖，`-20Hz` 更低
- 风格：部分声音支持情绪/风格（如 激情、活泼、温柔、稚嫩等），此能力与具体 voice 绑定

------

## 7⃣️ 语音清单

你可以直接用命令列出所有官方可用 voice：

```bash
edge-tts --list-voices > voices.json
```

也可以把整理好的 Excel 表作为速查表。

------

## 8⃣️ 常见问题

- 报错 `SSLError`：检查网络或代理，或稍后再试
- 输出空文件：确认 `await tts.save("xx.mp3")` 写入路径存在
- 部分风格不生效：该 voice 不支持对应风格，换支持的 voice
- 想生成 WAV：把输出文件后缀改为 `.wav` 即可

------

## 9⃣️ 参考

https://github.com/rany2/edge-tts

https://blog.csdn.net/weixin_62828995/article/details/136722656

https://blog.csdn.net/2303_80346267/article/details/145040588

