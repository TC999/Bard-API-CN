Development Status :: 5 - Production/Stable

# 常见问题解答

## 在使用Bard API之前
- Google Bard可能根据账户、国家、地区、IP等因素返回不同的响应，遵循Google的政策。这意味着即使功能良好，特别是`ask_about_image`方法，也可能遇到响应错误，这是由于与Google政策相关的各种原因引起的，而不是包错误。这无法在包级别解决（例如，[CAPTCHA](https://en.wikipedia.org/wiki/CAPTCHA)或[HTTP 429错误](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429)）。
- 此服务的API令牌（__Secure-1PSID cookie值）是非官方的。此外，公开它可能会使其他人轻松使用Bard服务并使用您的Google ID，因此请勿公开。
- 此服务每单位时间具有非常有限且可变的调用限制，超过速率限制会暂时阻止获取正常响应结果。
- 在请求中多次发送相同的问题也可能会暂时阻止获取正常响应结果。
- 一些地区可能需要除__Secure-1PSID之外的其他cookie值，请参阅问题页面。
- __Secure-1PSID cookie值可能会经常更改。注销，重新启动Web浏览器，并输入新的__Secure-1PSID cookie值。
- 将此包用于实际应用程序是非常不适当的。由于速率限制和可变的API政策，它只会在短时间内起作用。
- 如果请求之间的时间间隔非常短，Google API进程可能会将其解释为执行大量请求，并可能不返回正常响应。
- 所有这些政策都可能会发生变化，并且界面也是可变的。
- Bard API的方法名称不遵循一般LLM的典型推断格式的原因是为了防止与官方API混淆。此Python包仅用作从Bard网站获取响应的非官方API，请不要误解。
- Bard的官方API格式可能如下。
  <details>
  <summary>点击查看文本生成AI模型API代码模板</summary>
  
  ```python
  bard.configure(api_key='YOUR_API_KEY')
  
  models = [m for m in palm.list_models() if 'generateText' in m.supported_generation_methods]
  model = models[0].name
  print(model)
  
  prompt = "Who are you?"
  
  completion = bard.generate_text(
      model=model,
      prompt=prompt,
      temperature=0,
      # 响应的最大长度
      max_output_tokens=800,
  )
  ```
  </details>



<br>

## 使用Bard API解决与Google账户相关的问题

<br>

### 1. 响应错误
```python
Response Error: b')]}\\'\\n\\n38\\n[[\"wrb.fr\",null,null,null,null,[8]]]\\n54\\n[[\"di\",59],
[\"af.httprm\",59,\"4239016509367430469\",0]]\\n25\\n[[\"e\",4,null,null,129]]\\n'. 
由于流量或cookie值错误而暂时不可用。请仔细检查cookie值并验证您的网络环境。
```

您遇到的错误并非来自包本身，而是与您的Google账户有关，可能是由于各种因素，如您的国家或地区设置，导致您无法从Bard中获得准确的结果。因此，解决这个问题超出了包的范围，但请考虑检查以下内容：

1. 首先，仔细阅读自述文件，并查看是否涉及包的临时异常响应情况。稍后再试一次或几个小时后再试：[在使用Bard API之前](https://github.com/dsdanielpark/Bard-API/blob/main/README.md#before-using-the-bard-api)
2. 绕过代理：[在代理后面](https://github.com/dsdanielpark/Bard-API#behind-a-proxy)
3. 尝试使用来自不同地区的帐户或更改您的Google帐户的语言设置。
4. 重新启动浏览器以刷新cookie值并使用新的__Secure-1PSID。
5. 尝试使用[多cookie Bard](https://github.com/dsdanielpark/Bard-API/blob/main/documents/README_DEV.md#multi-cookie-bard)传递三个或更多cookie。如果不起作用，请考虑传递几乎所有cookie值。

如果需要进一步协助，请告诉我。

<br>

### 2. 响应状态为429

```
Traceback (most recent call last):
  File "/Users/arielolin/IAPP-server/server.py", line 20, in <module>
    bard = Bard(token=BARD_TOKEN, session=bard_session)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages/bardapi/core.py", line 78, in __init__
    self.SNlM0e = self._get_snim0e()
                  ^^^^^^^^^^^^^^^^^^
  File "/Library/Frameworks/Python.framework/Versions/3.11/lib/python3.11/site-packages/bardapi/core.py", line 149, in _get_snim0e
    raise Exception(
Exception: Response status code is not 200. Response Status is 429
```

这两者都不是与包相关的问题，而是无法解决的问题。建议尽量绕过它们。这是Google API的问题，尽管可能需要花费很长时间来开发绕过它的软件，即使假设可以开发，也不值得开发，因为很容易被阻止。没有进一步更新的计划。防止验证码或HTTP 429错误的包级解决方案除了努力绕过自

述和代理以及在请求之间创建时间间隔以避免速率限制之外，没有。

- `CAPTCHA`：旨在区分人类和机器输入的程序或系统，通常作为[阻止](https://www.google.com/search?sca_esv=573532060&sxsrf=AM9HkKmd5Faz1q0x4sLsgIG3VgVR9V18iA:1697335053753&q=thwarting&si=ALGXSlbSiMNWMsv5Y0U_0sBS8EWzwSlNZdPczeDdDqrhgxYO86hMDzIqBVTJp6ZKxKdXeVsCSihVIJAH_MROqwPM7RtQB0OoEA%3D%3D&expnd=1)垃圾邮件和从网站中自动提取数据的方式。
- `HTTP 429`：太多请求的响应状态代码表示用户在一定时间内发送了太多请求（“速率限制”）

<br>

### 3. 异常：未找到SNlM0e值。仔细检查__Secure-1PSID值或将其作为token='xxxxx'传递。#155, #99
- https://github.com/dsdanielpark/Bard-API/issues/155
- https://github.com/dsdanielpark/Bard-API/issues/99

<br>

### 4. 响应错误：b')]}\\'\\n\\n38\\n[[\"wrb.fr\",null,null,null,null,[8]]]\\n54\\n[[\"di\",59],[\"af.httprm\",59,\"4239016509367430469\",0]]\\n25\\n[[\"e\",4,null,null,129]]\\n'. \n由于流量或cookie值错误而暂时不可用。请仔细检查cookie值并验证您的网络环境。#128
- https://github.com/dsdanielpark/Bard-API/issues/128

<br>

### 5. 使用代理
如果您在您的地区无法收到正常响应，请尝试通过[Crawlbase](https://crawlbase.com/)的匿名[智能代理服务](https://crawlbase.com/docs/smart-proxy/get/)进行请求。 （但仍需注意Google的速率限制，因此调整请求之间的时间并避免请求重复响应。）

```
$ pip install bardapi
```

```python
from bardapi import Bard
import requests

# 在crawlbase上获取您的代理URL https://crawlbase.com/docs/smart-proxy/get/
proxy_url = "http://xxxxxxxxxxxxxx:@smartproxy.crawlbase.com:8012" 
proxies = {"http": proxy_url, "https": proxy_url}

bard = Bard(token='xxxxxxx', proxies=proxies, timeout=30)
bard.get_answer("나와 내 동년배들이 좋아하는 뉴진스에 대해서 알려줘")['content']
```