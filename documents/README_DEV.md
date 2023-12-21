Development Status :: 5 - Production/Stable


# GitHub 安装要求以下功能。
从版本 `0.1.18` 开始，BardAPI 的 GitHub 版本将与 PyPI 版本同步，并同时发布。
```python
pip install git+https://github.com/dsdanielpark/Bard-API.git
```

# 目录
- [GitHub 安装要求以下功能。](#github-installation-required-for-the-following-features)
- [目录](#contents)
    - [`core.py - Bard` 类的结构](#structure-of-corepy---bard-class)
    - [自动 Cookie Bard](#auto-cookie-bard)
    - [文本转语音(TTS)](#text-to-speechtts)
    - [多语言 Bard](#multi-language-bard)
    - [Bard `ask_about_image` 方法](#bard-ask_about_image-method)
    - [获取图片链接](#get-image-links)
    - [ChatBard](#chatbard)
    - [导出对话](#export-conversation)
    - [将代码导出到 Repl.it](#export-code-to-replit)
    - [执行从 Bard 接收的 Python 代码](#executing-python-code-received-as-a-response-from-bard)
    - [异步使用 Bard](#using-bard-asynchronously)
    - [多 Cookie Bard](#multi-cookie-bard)
    - [修复对话 ID（修复上下文）](#fix-conversation-id-fix-context)
    - [转换到另一种编程语言](#translation-to-another-programming-language)
    - [max\_token, max\_sentence](#max_token-max_sentence)
    - [具有更多功能的 ChatBard](#chatbard-with-more-features)
      - [用法](#usage)
      - [功能](#features)
      - [处理 API 错误](#handle-api-errors)
      - [输入验证](#input-validation)
      - [聊天记录](#chat-history)
      - [多语言支持](#multilingual-support)
      - [用户体验改进](#improved-user-experience)
      - [与其他 API 集成](#integration-with-other-apis)


<br>


### `core.py - Bard` 类的结构

<details>
<summary> 查看类结构...</summary>
    
```python
class Bard:
    """
    用于与 Bard API 交互的 Bard 类。
    """

    def __init__(
        self,
        token: Optional[str] = None,
        timeout: int = 20,
        proxies: Optional[dict] = None,
        session: Optional[requests.Session] = None,
        conversation_id: Optional[str] = None,
        google_translator_api_key: Optional[str] = None,
        language: Optional[str] = None,
        run_code: bool = False,
        token_from_browser: bool = False,
    ):
        """
        初始化 Bard 实例。
        ...
        """

    def get_answer(self, input_text: str) -> dict:
        """
        为给定的输入文本从 Bard API 获取答案。
        ...
        """

    def speech(self, input_text: str, lang: str = "en-US") -> dict:
        """
        从 Bard API 获取给定输入文本的语音音频。
        ...
        """

    def export_conversation(self, bard_answer, title: str = "") -> dict:
        """
        为来自 Bard 的特定答案获取共享 URL。
        ...
        """

    def ask_about_image(self, input_text: str, image: bytes, lang: Optional[str] = None) -> dict:
        """
        发送带有问题的图像给 Bard 并获取答案。
        ...
        """

    def export_replit(
        self, code: str, langcode: Optional[str] = None, filename: Optional[str] = None, **kwargs
    ) -> dict:
        """
        从代码获取导出到 repl.it 的 URL。
        ...
        """

    def _get_snim0e(self) -> str:
        """
        从 Bard API 响应中获取 SNlM0e 值。
        ...
        """

    def _get_session(self, session: Optional[requests.Session]) -> requests.Session:
        """
        获取请求的 Session 对象。
        """

    def _get_token(self, token: str, token_from_browser: bool) -> str:
        """
        从提供的令牌或浏览器 cookie 获取 Bard API 令牌。
        """ 
```
</details>

<br>

### 自动 Cookie Bard
使用 [browser_cookie3](https://github.com/borisbabic/browser_cookie3) 我们从所有浏览器中提取 `__Secure-1PSID`` cookie，然后我们可以在不传递令牌的情况下使用 API。但是，仍然存在不完整的依赖包和各种变量，请在以下 [GitHub 问题](https://github.com/borisbabic/browser_cookie3/issues) 中寻求帮助或调整您浏览器的版本。

```python
from bardapi import Bard

bard = Bard(token_from_browser=True)
bard_answer = bard.get_answer("你喜欢饼干吗？")['content']
print(bard_answer)
```

<br>

### 文本转语音(TTS)
```python
from bardapi import Bard

bard = Bard(token= xxxxxxxxx )
audio = bard.speech("你好，我是巴德！我能帮助你什么？", lang='zh-CN') # 获取字节音频对象。

# 保存音频对象。
with open('bard_response.mp3', 'wb') as f:
    f.write(audio['audio'])
```

<br>

### 多语言 Bard
对于商业用途，请勿出于非商业目的使用 bardapi 中包含的非官方 Google 翻译包。而是，请访问官方的 Google Cloud 翻译网站。请负责任地使用它，对您的行为承担全部责任，因为 bardapi 包不承担任何隐含或明示的责任。
> 官方 Google 翻译 API
- 支持语言：https://cloud.google.com/translate/docs/languages?hl=ko
> 非官方 Google 翻译用于非盈利目的（如功能测试）
- 支持语言：https://github.com/nidhaloff/deep-translator/blob/master/deep_translator/constants.py
```python
from bardapi import Bard

bard = Bard(token=token, language='chinese (simplified)')
res = bard.get_answer("今天首尔的天气怎么样？")
print(res['content

'])
```
或者
```python
from bardapi import Bard
import os
os.environ["_BARD_API_LANG"] = 'chinese (simplified)'
os.environ["_BARD_API_KEY"] = 'xxxxxxxxx'

res = Bard().get_answer("今天首尔的天气怎么样？")
print(res['content'])
```

<br>

### Bard `ask_about_image` 方法 
 *它可能不起作用，因为它仅适用于某些帐户、地区和其他限制。*
作为一项实验性功能，可以使用图像提出问题。但是，此功能仅适用于在 Bard 的 Web UI 中具有图像上传功能的帐户。

```python
from bardapi import Bard

bard = Bard(token='xxxxxxxx')
image = open('image.jpg', 'rb').read() # （jpeg、png、webp）都受支持。
bard_answer = bard.ask_about_image('图中是什么？', image)
print(bard_answer['content'])
```

<br>

### 获取图片链接
```python
from bardapi import Bard
bard = Bard(token='xxxxxxxx')
res = bard.get_answer("给我找一张斯坦福大学正门的图片。")
res['links'] # 获取图片链接（列表）
res['images'] # 获取图片（列表）
```

<br>
    
### ChatBard
已修复 ChatBard 中的一些错误。但是，建议不要通过输入传递令牌的值。在 [ChatBard with more features](https://github.com/dsdanielpark/Bard-API/blob/main/README_DEV.md#chatbard-with-more-features) 中查看详细信息。

```python
from bardapi import ChatBard
    
chat = ChatBard(token="xxxxxxxx", language='en')
chat.start()
```
或者
```python
from bardapi import ChatBard
import os
os.environ["_BARD_API_KEY"]='xxxxxxxx'   # 必需的
os.environ["_BARD_API_LANG"]="Arabic"     # 可选，默认为英语
os.environ["_BARD_API_TIMEOUT"]=30        # 可选，会话超时
 
chat = ChatBard()
chat.start()
```
或者
```python
from bardapi import Bard, SESSION_HEADERS
import os
import requests

token='xxxxxxxx'
session = requests.Session()
session.headers = SESSION_HEADERS
session.cookies.set("__Secure-1PSID", token) 
proxies = {
    'http': 'http://proxy.example.com:8080',
    'https': 'https://proxy.example.com:8080'
}
    
ChatBard(token=token, session=session, proxies=proxies, timeout=40, language="chinese (simplified)").start()
```

<br>    

### 导出对话
 *它可能不起作用，因为它仅适用于某些帐户、地区和其他限制。*
Bard UI 提供了一种方便的方式通过生成 URL 来共享来自 Bard 的特定答案。此功能使用户能够轻松创建和共享单个答案的 URL。

```python
from bardapi import Bard
bard = Bard(token='xxxxxxxx')
bard_answer = bard.get_answer('你好吗？')
url = bard.export_conversation(bard_answer, title='示例共享对话')
print(url['url'])

```

<br>

### 导出代码到 [Repl.it](https://replit.com/)
```python
from bardapi import Bard

bard = Bard(token='xxxxxxxx')

bard_answer = bard.get_answer("用 Python 编写代码打印 hello world")
# {'code': 'print("Hello World")', 'program_lang': 'python'}
url = bard.export_replit(
    code=bard_answer['code'],
    program_lang=bard_answer['program_lang'],
)
print(url['url']) # https://replit.com/external/v1/claims/xxx/claim
```

<br>

### 执行从 Bard 接收的 Python 代码
```python
from bardapi import Bard
    
bard = Bard(token="xxxxxxxx", run_code=True)
bard.get_answer("用 Python 代码绘制饼图，数据为{'blue':25, 'red':30, 'green':30, 'purple':15}")
```
![](assets/bardapi_run_code.png)
    
<br>

### 异步使用 Bard 
在实现 ChatBots 或类似功能时，使用异步实现将更有效。
BardAsync 不使用 requests 库，而是使用 httpx 库和 http2 协议。

BardAsync 存在于 translate_to.core_async.BardAsync
```python
from bardapi import BardAsync 
    
bard = BardAsync(token="xxxxxxxx")
await bard.get_answer("Metaverse 是什么？")
```
或者
```python
import asyncio
from bardapi import BardAsync
    
bard = BardAsync(token="xxxxxxxx")
asyncio.run(bard.get_answer("Metaverse 是什么？"))
```
<br>
    

### 多 Cookie Bard
```python
from bardapi import BardCookies

cookie_dict = {
    "__Secure-1PSID": "xxxxxxxx",
    "__Secure-1PSIDTS": "xxxxxxxx",
    "__Secure-1PSIDCC": "xxxxxxxx",
    # 您想传递的任何 cookie 值给 session 对象。
}

bard = BardCookies(cookie_dict=cookie_dict)
print(bard.get_answer("こんにちは")['content'])
```

带有可重用会话的 Bard，其中包含多个 cookie 值
```python
import requests
from bardapi import Bard, SESSION_HEADERS

session = requests.Session()
session.cookies.set("__Secure-1PSID", "bard __Secure-1PSID token")
session.cookies.set("__Secure-1PSIDCC", "bard __Secure-1PSIDCC token")
session.cookies.set("__Secure-1PSIDTS", "bard __Secure-1PSIDTS token")
session.headers = SESSION_HEADERS

bard = Bard(session=session)
bard.get_answer("今天广州的天气怎么样？")
```


带有自动从浏览器获取 Cookie 的多 Cookie Bard
```python
from bardapi import Bard

bard = BardCookies(token_from_browser=True)
bard.get_answer("今天首尔的天气怎么样？")
```

<br>

### 修复对话 ID（修复上下文）
BART 返回多个响应作为候选答案。每个响应都被分配一个 conversation_id。在使用可重用会话时，您可以观察到您的提示已存储。但是，如果您希望获得一致的答案，您可以将所需的 conversation_id 作为返回的候选答案之一的参数提供。

- 仅传递 `session`：保留您的提示。
- 传递 `session` 和 `conversation_id`：保留您的提示并允许您接收具有一致参数的答案。

```python
from bardapi import Bard, SESSION_HEADERS
import os
import requests

# 设置令牌
token= 'xxxxxxxx'

# 设置会话
session = requests.Session()
session.headers = SESSION_HEADERS
session.cookies.set("__Secure-1PSID", token) 

# 给定会话和对话 ID。（手动检查）
bard = Bard(token=token, session=session, conversation_id="c_1f04f704a788e6e4", timeout=30)
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```

<br>

### 转换到另一种编程语言
请在 [此文件夹](https://github.com/dsdanielpark/Bard-API/tree/main/translate_to) 中检查翻译结果。
- 复制 [Core.py 的代码](https://github.com/dsdanielpark/Bard-API/blob/17d5e948d4afc535317de3964232ab82fe223521/bardapi/core.py)。
- 要求 ChatGPT 进行翻译，例如“翻译为 Swift”。
- 要求 ChatGPT 优化代码或提供任何所需的指示，直到您满意。<br>

![](./assets/translate.png)


<br>

### max_token, max_sentence
Bard 不支持温度或超参数调整，但是可以使用简单的算法实现限制输出标记数或输出句子数的外观，如下所示：
```python
from bardapi import Bard, max_token, max_sentence

token = 'xxxxxxx'
bard = Bard(token=token)

# max_token==30
max_token(bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content'], 30) 
# max_sentence==2
max_sentence(bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content'], 2)
```

<br>


### 具有更多功能的 ChatBard
ChatBard 是由 Bard API 提供支持的聊天机器人类。它允许用户与聊天机器人进行互动并从 Bard API 检索响应。

#### 用法

```python
from bardapi import ChatBard

chat = ChatBard()
chat.start()
```

#### 功能
自定义用户提示
用户可以通过向 start() 方法提供自定义提示消息来自定义在输入之前显示给用户的提示。
```python
chat.start(prompt="输入您的消息：")
```

#### 处理 API 错误
已实现对 API 请求的错误处理。如果在与 Bard API 通信时发生错误，将向用户显示适当的错误消息。

#### 输入验证
在将输入发送到 API 之前，对用户输入进行了验证。对输入进行了空值和长度验证。如果输入无效，将提示用户提供有效的输入。

#### 聊天历史
在对话期间存储聊天历史，包括用户输入和相应的聊天机器人响应。可以调用 display_chat_history() 方法来打印聊天历史。
```python
chat.display_chat_history()
```

#### 多语言支持
用户可以通过在初始化 ChatBard 对象时指定 language 参数来选择与聊天机器人互动的不同语言。默认语言是英语。
```python
chat = ChatBard(language="Chinese")
```

#### 用户体验改进
聊天机器人响应使用颜色和格式进行显示，以增强用户体验。用户输入以绿色显示，聊天机器人响应以蓝色显示。

#### 与其他 API 集成
ChatBard 类专注于与 Bard API 的交互。如果要集成其他 API，可以通过在 ChatBard 类中添加适当的方法并在 ChatBard 类内进行必要的 API 调用来扩展功能。
            
            
