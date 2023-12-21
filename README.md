Development Status :: 5 - Production/Stable


# Google <a href="https://bard.google.com/"><img src="https://camo.githubusercontent.com/adb54264fe2ad5067d07d0752fc32600b4e6250073b01ce8c386575b431e3f06/68747470733a2f2f7777772e677374617469632e636f6d2f6c616d64612f696d616765732f66617669636f6e5f76315f31353031363063646466663766323934636533302e737667" height="20px"></a> Bard API


<p align="left">
<a href="https://github.com/dsdanielpark/Bard-API"><img alt="PyPI package" src="https://img.shields.io/badge/pypi-BardAPI-black"></a>
<a href="https://pepy.tech/project/bardapi"><img alt="Downloads" src="https://pepy.tech/badge/bardapi"></a>
<!-- <a><img alt="commit update" src="https://img.shields.io/github/last-commit/dsdanielpark/Bard-API?color=black"></a> -->
<a href="https://github.com/psf/black"><img alt="Code style: black" src="https://img.shields.io/badge/code%20style-black-000000.svg"></a>
<a href="https://github.com/dsdanielpark/Bard-API"><img src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fdsdanielpark%2FBARD_API&count_bg=%23000000&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=BardAPI&edge_flat=false"/></a>
<a href="https://github.com/dsdanielpark/Bard-API/stargazers"><img src="https://img.shields.io/github/stars/dsdanielpark/Bard-API?style=social"></a>
<a href="https://pypi.org/project/bardapi/"><img alt="PyPI" src="https://img.shields.io/pypi/v/bardapi"></a>
<a href="https://www.buymeacoffee.com/parkminwoo"><img src="https://cdn.buymeacoffee.com/buttons/v2/arial-orange.png" height="20px"></a>
  
</p>


> 通过Cookie值返回[Google Bard](https://bard.google.com/)响应的Python包。

![](./assets/bard_api.gif)


**请谨慎使用此包，并负责任地使用。这个Python包是_非官方的_。**

我参考了这个GitHub仓库([github.com/acheong08/Bard](https://github.com/acheong08/Bard))，在那里对Bard的推理过程进行了逆向工程。使用`__Secure-1PSID`，您可以向Google Bard提问并获取答案。请注意，bardapi不是免费服务，而是一种工具，用于由于Google Bard API的开发和发布延迟而帮助开发人员测试某些功能。它具有轻量级结构，可以轻松适应官方API的出现。因此，我强烈不建议将其用于任何其他目的。如果您可以访问官方的[PaLM-2 API](https://blog.google/technology/ai/google-palm-2-ai-large-language-model/)，请使用相应的官方代码替换提供的响应。

<br>

- [Google  Bard API](#google--bard-api)
  - [什么是Google Bard？](#什么是google-bard)
  - [安装](#安装)
  - [身份验证](#身份验证)
  - [用法](#用法)
  - [更多信息](#更多信息)
    - [在代理后面](#在代理后面)
    - [可重用的会话对象](#可重用的会话对象)
    - [自动Cookie Bard](#自动cookie-bard)
    - [Bard `ask_about_image` 方法](#bard-ask_about_image-方法)
    - [从Bard获取文本到语音（TTS）](#从bard获取文本到语音tts)
  - [更多功能](#更多功能)
  - [在使用Bard API之前](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_FAQ.md#before-using-the-bard-api)



<br>

## 什么是Google [Gemini](https://deepmind.google/technologies/gemini/#introduction)?
[Gemini](https://deepmind.google/technologies/gemini/#introduction)是由[Google DeepMind](https://deepmind.google/)开发的先进的多模态AI模型，能够理解和整合各种信息类型，如文本、代码、音频、图像和视频。

### 在Bard API中访问Gemini Pro
Bard API从[Google Bard的官方网站](https://bard.google.com/chat)获取响应，允许您获得与网站相同的响应。因此，如果Gemini的答案在网上可用，您也可以通过Bard API访问Gemini。然而，重要的是要注意，响应也可能来自其他模型，不仅仅是Gemini Pro或Ultra。
- 通过Bard API访问Gemini Pro不一致，仅限英语。
- 没有官方的Bard API或Gemini的提前访问/等待列表，尽管[PaLM2 API](https://github.com/dsdanielpark/Bard-API#google-palm-api)可用。
  - Google的PaLM2 API与Bard的某些方面不同。
  - 有人推测，在经过对Gemini的专业审查后，明年的Bard Advanced系列可能会提供官方API。
- Gemini和之前的生成式AI模型的响应是随机提供的。
- Bard API具有不完善的扩展功能（例如`ask_about_image`），有时展示Gemini的能力。
- 此行为可能因地区、语言或Google账户而异。
- 在[FAQ](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_FAQ.md)中查看更多信息。

有关Gemini的更多信息：
- [介绍Gemini：我们最大、最有能力的AI模型](https://blog.google/technology/ai/google-gemini-ai/)
- [它是如何制造的：多模提示](https://developers.googleblog.com/2023/12/how-its-made-gemini-multimodal-prompting.html)
- [YouTube演示](https://www.youtube.com/watch?v=UIZAiXYceBI)



<br>

## 什么是Google Bard？
Bard是由Google开发的会话式生成式人工智能聊天机器人，最初基于LaMDA系列的LLMs（大型语言模型），后来是PaLM LLM。请查看Bard的[更新](https://bard.google.com/updates)的官方文件，包括[Bard的可用区域和语言](https://support.google.com/bard/answer/13575153?hl=en)。

## 安装
```
$ pip install bardapi
```
```
$ pip install git+https://github.com/dsdanielpark/Bard-API.git
```
由于某些与64位Windows（OS）不兼容的依赖包，我们发布了Bard的轻量级alpha版本，该版本仅对简单请求返回响应。这个版本是pypi `0.1.18`版本的继续，该版本保持了轻量级和简单的功能。有关更多详细信息，请参阅[alpha-release github分支](https://github.com/dsdanielpark/Bard-API/tree/alpha-release)。
```
$ pip install bardapi==0.1.23a
```

<br>

## 身份验证
> **警告** 不要公开`__Secure-1PSID` 
1. 访问https://bard.google.com/
2. F12打开控制台
3. 会话：应用程序 → Cookies → 复制`__Secure-1PSID` cookie的值。

请注意，尽管我将`__Secure-1PSID`值方便地称为API密钥，但这并不是官方提供的API密钥。Cookie值会频繁更改。如果发生错误，请再次验证该值。输入无效的Cookie值会导致大多数错误。

<br>

如果需要设置多个Cookie值

- [Bard Cookies](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#multi-cookie-bard) - 在确认在某些国家可靠地接收响应需要多个Cookie值之后，我将部署它以进行测试。请调试并创建拉取请求


<br>

## 用法 
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1zzzlTIh0kt2MdjLzvXRby1rWbHzmog8t?usp=sharing) 


简单用法

```python
from bardapi import Bard

token = 'xxxxxxx'
bard = Bard(token=token)
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```
或者您可以使用这个
```python
from bardapi import Bard
import os
os.environ['_BARD_API_KEY']="xxxxxxx"

Bard().get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```

获取响应字典
```python
import bardapi
import os

# 将__Secure-1PSID值设置为密钥
token = 'xxxxxxx'

# 设置您的输入文本
input_text = "나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘"

# 发送API请求并获取响应。
response = bardapi.core.Bard(token).get_answer(input_text)
```



解决类似Google Colab和容器中的延迟响应引起的错误。如果尽管按照正确的步骤操作仍然发生错误，请使用timeout参数。
```python
from bardapi import Bard
import os
os.environ['_BARD_API_KEY']="xxxxxxx"

bard = Bard(timeout=30) # 以秒为单位设置超时
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```

<br>

## 更多信息
### 在代理后面
如果您在代理后面工作，请使用以下方法。
```python
from bardapi import Bard
import os

# 将'http://proxy.example.com:8080'更改为您的http代理
# 超时时间（以秒为单位）
proxies = {
    'http': 'http://proxy.example.com:8080',
    'https': 'https://proxy.example.com:8080'
}

bard = Bard(token='xxxxxxx', proxies=proxies, timeout=30)
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```

### 使用旋转代理

如果您想**避免被阻止的请求**和封禁，那么使用[Crawlbase的Smart Proxy](https://crawlbase.com/docs/smart-proxy/?utm_source=github_ad&utm_medium=social&utm_campaign=bard_api)。它在到达目标网站之前将您的连接请求转发到代理池中的**随机旋转的IP地址**。AI和ML的结合使其更有效地**避免CAPTCHA和封禁**。

```python
from bardapi import Bard
import requests

# 在crawlbase https://crawlbase.com/docs/smart-proxy/get/获取您的代理url
proxy_url = "http://xxxxxxxxxxxxxx:@smartproxy.crawlbase.com:8012" 
proxies = {"http": proxy_url, "https": proxy_url}

bard = Bard(token='xxxxxxx', proxies=proxies, timeout=30)
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```

### 可重用的会话对象
你可以使用可重用的会话对象继续对话。
```python
from bardapi import Bard
import os
import requests
os.environ['_BARD_API_KEY'] = 'xxxxxxx'
# token='xxxxxxx'

session = requests.Session()
session.headers = {
            "Host": "bard.google.com",
            "X-Same-Domain": "1",
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.114 Safari/537.36",
            "Content-Type": "application/x-www-form-urlencoded;charset=UTF-8",
            "Origin": "https://bard.google.com",
            "Referer": "https://bard.google.com/",
        }
session.cookies.set("__Secure-1PSID", os.getenv("_BARD_API_KEY")) 
# session.cookies.set("__Secure-1PSID", token) 

bard = Bard(token=token, session=session, timeout=30)
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']

# 不设置新会话继续对话
bard.get_answer("我的最后一个提示是什么？")['content']
```

### 自动Cookie Bard
使用 [browser_cookie3](https://github.com/borisbabic/browser_cookie3) 从所有浏览器中提取 `__Secure-1PSID` cookie，然后我们可以在不传递令牌的情况下使用 API。然而，仍然存在不完整的依赖包和各种变量，因此请在以下 [GitHub问题](https://github.com/borisbabic/browser_cookie3/issues) 寻求帮助，或调整您浏览器的版本。
- 在浏览器中访问 https://bard.google.com/ 并在启用聊天的状态下执行以下命令。有关其工作原理的详细信息，请参阅 browser_cookie3。如果出现任何问题，请重新启动浏览器或重新登录到您的 Google 帐户。
*建议保持浏览器打开。*
```python
from bardapi import Bard

bard = Bard(token_from_browser=True)
res = bard.get_answer("你喜欢饼干吗？")
print(res['content'])
```

### Bard `ask_about_image` 方法 
*可能不起作用，因为它仅适用于特定帐户、区域和其他限制。*
作为实验性功能，可以使用图像提出问题。但是，此功能仅适用于在 Bard 的 Web UI 中具有图像上传功能的帐户。

```python
from bardapi import Bard

bard = Bard(token='xxxxxxx')
image = open('image.jpg', 'rb').read() # 支持 (jpeg, png, webp)。
bard_answer = bard.ask_about_image('图像中是什么？', image)
print(bard_answer['content'])
```

### [Bard 文本到语音（TTS）](https://cloud.google.com/text-to-speech?hl=ko)
商业用户和高流量可能会根据 Google 的政策受到帐户限制。请为任何其他目的使用 [Official Google Cloud API](https://cloud.google.com/text-to-speech)。用户对所有代码负有唯一的责任，并有必要咨询 Google 的官方服务和政策。此外，此存储库中的代码是根据 MIT 许可提供的，并且放弃任何责任，包括明示或暗示的法律责任。
```python
from bardapi import Bard

bard = Bard(token='xxxxxxx')
audio = bard.speech('你好，我是巴德！我今天可以帮你什么？')
with open("speech.ogg", "wb") as f:
  f.write(bytes(audio['audio']))
```

<br>

## [更多功能](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md)
从版本 `0.1.18` 开始，GitHub 版本的 BardAPI 将与 PyPI 版本同步并同时发布。然而，正在进行 QA 的版本仍可以从 GitHub 存储库中使用。<br>
```
$ pip install git+https://github.com/dsdanielpark/Bard-API.git
```

- [自动Cookie Bard](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#auto-cookie-bard)
- [从 Bard 进行 TTS](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#text-to-speechtts)
- [多语言 Bard API](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#multi-language-bard-api)
- [获取图像链接](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#get-image-links)
- [ChatBard](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#chatbard)
- [导出对话](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#export-conversation)
- [将代码导出到 Repl.it](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#export-code-to-replit)
- [执行从 Bard 收到的 Python 代码](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#chatbard)
- [异步使用 Bard](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#using-bard-asynchronously)
- [Bard Cookies](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#bard-which-can-get-cookies)
- [修复会话 ID（修复上下文）](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#fix-conversation-id-fix-context)
- [max_token-max_sentence](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#max_token-max_sentence)
- [转到其他编程语言的翻译](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#translation-to-another-programming-language)


<br>



##  [Amazing Bard Prompts](https://github.com/dsdanielpark/amazing-bard-prompts) 就是你需要的全部！
- 适用于 Google Bard 的有用提示

<br>

## [The Python package hf-transllm](https://github.com/dsdanielpark/hf-transllm)
如果您想要舒适地使用 `以 Apache 许可发布（允许免费商业使用）` 的 `各种语言` 中的开源 LLM 模型，可以尝试使用 [hf-transllm](https://github.com/dsdanielpark/hf-transllm) 包。hf-transllm 还支持在 hugging face 存储库中存储的各种 LLM 的多语言推断。

### [hf-transllm](https://github.com/dsdanielpark/hf-transllm) 的示例代码
<details>
<summary>如果由于政策限制不再提供 Google 包，这里是一个使用开源语言模型（LLM）的简单示例代码，支持英语和多种语言。</summary>

<br>

### 用法
对于由 Hugging Face 提供的解码器模型，您可以通过遵循简单的方法或覆盖推断方法来轻松使用它们。您可以在 [此链接](https://huggingface.co/models) 上探索各种开源语言模型。通过 [Open LLM Leader Board Report repository](https://github.com/dsdanielpark/Open-LLM-Leaderboard-Report) 的排名信息，您可以找到关于好模型的信息。

#### 对于使用 `非英语` 语言的 LLM
```python
from transllm import LLMtranslator

open_llama3b_kor = LLMtranslator('openlm-research/open_llama_3b', target_lang='ko', translator='google') # 韩语

trnaslated_answer = open_llama3b_kor.generate("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")
print(trnaslated_answer)
```

#### 对于使用 `英语` 语言的 LLM
参考 https://github.com/openlm-research/open_llama 或像这样使用：
```python
from transllm import LLMtranslator

open_llama3b = LLMtranslator('openlm-research/open_llama_3b') 

answer = open_llama3b.generate("Tell me about the Korean girl group Newjeans.")
print(answer)
```

</details>



<br>

# Google PaLM API
您可以探索有关 Google 各种生成式 AI 模型的信息。尽管 palm2 API 似乎正在准备中，但您可以在演示页面上检查与 palm2 相关的演示。

## PaLM API
在 https://makersuite.google.com/app/prompts/new_text 上尝试演示。
```
你是谁？
>> 我由 PaLM 2 提供支持，PaLM 2 是来自 Google AI 的大型语言模型。
```

Google 生成式 AI
- 官方页面: https://blog.google/technology/ai/google-palm-2-ai-large-language-model/
- GitHub: https://github.com/GoogleCloudPlatform/generative-ai
- 尝试演示: https://makersuite.google.com/app/prompts/new_text.
- 官方库: https://makersuite.google.com/app/library
- 获取 API 密钥: https://makersuite.google.com/app/apikey
- 快速入门教程: https://developers.generativeai.google/tutorials/text_quickstart

### 快速开始
```
$ pip install -q google-generativeai
```

```python
import pprint
import google.generativeai as palm

palm.configure(api_key='YOUR_API_KEY')

models = [m for m in palm.list_models() if 'generateText' in m.supported_generation_methods]
model = models[0].name
print(model)

prompt = "你是谁？"

completion = palm.generate_text(
    model=model,
    prompt=prompt,
    temperature=0,
    # 响应的最大长度
    max_output_tokens=800,
)
print(completion.result)
```

<br>

<br>

## 赞助商

<a href="https://crawlbase.com/?utm_source=github_ad&utm_medium=social&utm_campaign=bard_api"><img src="./assets/sponsor_ad.png"></a>
 
 **使用数据抓取训练您的 AI 模型。** 

- 简单易用的 **API 来爬取和抓取** 数百万个网站
- 使用 crawlbase 进行高效的 [数据提取](https://crawlbase.com/generative-ai-data?utm_source=github_ad&utm_medium=social&utm_campaign=bard_api) 用于您的 **LLMs**
- 平均 **成功率：98%**
- 正常运行时间保证：**99.9%**
- [简单文档](https://crawlbase.com/docs?utm_source=github_ad&utm_medium=social&utm_campaign=bard_api)，几分钟即可入门
- **异步** 爬行 **API**，如果您需要大量数据
- **GDPR** 和 **CCPA** 合规

被 **70k+** 开发者使用。


## [常见问题](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_FAQ.md)
在创建新问题之前，请查看 FAQ 和已有问题，以防有类似问题。重复的问题将保留为未解决的问题。过多的请求可能会触发临时账户封锁（HTTP 429）。请保持适当的间隔，使用 sleep 等功能以避免速率限制。政策可能因国家和语言而异，因此所有用户可能会通过 API 遇到临时或永久的错误。

## 脚本
在 [脚本文件夹](./scripts/) 中，我发布了一个脚本，帮助您比较 [OpenAI-ChatGPT](./scripts/openai_api.ipynb)、[Microsoft-EdgeGPT](./scripts/microsoft_api.ipynb) 和 [Google-Bard](./scripts/google_api.ipynb)。希望它们能帮助更多的开发者。

## 贡献者
我要对所有贡献者表示最诚挚的感谢。

<a href="https://github.com/dsdanielpark/Bard_API/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=dsdanielpark/Bard_API" />
</a>


<br>

## 许可证
[MIT](https://opensource.org/license/mit/) <br>
我们不承担法律责任；有关更多信息，请参阅自述文件底部。我们只是希望您给我和 [他们](https://github.com/acheong08/Bard) 一颗星。

```
The MIT License (MIT)

版权所有（c）2023 Minwoo Park

特此免费授予获得此软件和相关文档文件（以下简称“软件”）的任何人，无限制地处理此软件的权利，包括但不限于使用、复制、修改、合并、发布、分发、再许可和/或出售此软件的副本，以及允许提供此软件的人员这样做，但须遵守以下条件：

上述版权声明和本许可声明应包含在所有
软件副本或重要部分。

本软件按“原样”提供，不提供任何形式的明示或
默示担保，包括但不限于对适销性的担保，
适用于特定用途和非侵权的默示担保。在任何情况下
作者或版权所有者对任何索赔、损害或其他
责任，无论是在合同、侵权或其他方面的行为，均不承担
由于使用、不良或其他方式产生的，
或与软件或使用或其他软件的交互有关。
  
```

## 服务政策变更：Bard 和 Google 的动态 
Bard 的服务状态和 Google 的 API 接口不断变化。*目前回复的数量受到限制，但某些用户，*例如使用 VPN 或代理服务器的用户，报告了稍高的消息容量。在应对这些动态服务政策时，适应能力至关重要。请注意，此软件包中使用的 cookie 值不是官方 API 值。
            
## 错误和问题
对于新功能或错误的任何报告都表示衷心感谢。非常感谢您对代码的宝贵反馈。

## 联系方式
- 核心维护者：[Daniel Park, South Korea](https://github.com/DSDanielPark) <br>
- 电子邮件：parkminwoo1991@gmail.com <br>

## 参考 
[1] https://github.com/acheong08/Bard <br>
            
> **警告** 重要通知
  用户对使用 BardAPI 软件包所涉及的所有法律责任负有全部责任。此 Python 软件包仅为开发人员提供轻松访问 Google Bard 的工具。用户完全负责管理数据并适当使用该软件包。有关更多信息，请参阅 Google Bard 官方文档。
    
> **警告** 注意
此 Python 软件包不是官方的 Google 软件包或 API 服务。它与 Google 无关，使用 Google 帐户 cookie，这意味着过多或商业使用可能会导致对您的 Google 帐户的限制。该软件包旨在支持由于官方 Google 软件包的延迟而对功能进行测试的开发人员。然而，请注意不要滥用。请小心，并参考自述文件获取更多信息。
  
<br><br>
  
 *版权所有（c）2023 MinWoo Park，韩国*<br>