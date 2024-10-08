## OpenAI programming

## Google Colab 實戰 1 [Google Colab](https://colab.research.google.com/)
- 安裝openai ==> !pip install openai

```python
# 載入使用的套件
from openai import OpenAI, OpenAIError # OpenAI 官方套件

# 檢視你的 Key ==> https://platform.openai.com/api-keys
import getpass # 保密輸入套件

# 需要上傳config.py
# API_KEY ="XXXXXXXXXXXXXXXX4SoyDAIa"

import config

# 建立 OpenAI 物件
api_key = config.API_KEY
#api_key = getpass.getpass("請輸入金鑰：")

client = OpenAI(api_key = api_key)
```
```
# 建構模型並交談
reply = client.chat.completions.create(
    model = "gpt-3.5-turbo",
    # model = "gpt-4",
    messages = [
 # 設定 AI 角色
       # {"role":"system", "content":"你是隻住在外太空的猴子"},
        {"role":"user", "content": "你住的地方很亮嗎？"}
    ]
)

```
```python
# 檢視傳回物件

print(reply)
```
`輸出結果`
```
ChatCompletion(
id='chatcmpl-9MriqVT8F3EOWF0LX6OqHY4XeyCAZ',
choices=[
   Choice(finish_reason='stop', index=0, logprobs=None,
          message=ChatCompletionMessage(content='我是一个语言模型AI，没有实际的住所。不过通常来说，人们喜欢住在明亮的地方，这样可以让居住环境更加舒适和宜居。明亮的居所通常会让人感到更加开朗和愉快。你觉得你住的地方很亮吗？',
         role='assistant', function_call=None, tool_calls=None))],

created=1715236752,
model='gpt-3.5-turbo-0125',
object='chat.completion',
system_fingerprint=None,
usage=CompletionUsage(completion_tokens=108, prompt_tokens=21, total_tokens=129)
)
```

```python
## 檢視訊息內容
print(reply.choices[0].message.content)
```
`輸出結果`
```
我是一个语言模型AI，没有实际的住所。不过通常来说，人们喜欢住在明亮的地方，这样可以让居住环境更加舒适和宜居。
明亮的居所通常会让人感到更加开朗和愉快。你觉得你住的地方很亮吗？
```

### [OpenAI Models](https://platform.openai.com/docs/models)
## [使用OpenAI Playground 學習](https://platform.openai.com/examples)

## Google Colab 實戰 2: 看圖說故事 [Google Colab](https://colab.research.google.com/)
- https://platform.openai.com/docs/guides/vision
```python
response = client.chat.completions.create(
  model="gpt-4o",
  messages=[
    {
      "role": "user",
      "content": [
#        {"type": "text", "text": "What’s in this image?"},
         {"type": "text", "text": "描述一下這張圖片?"},
        {
          "type": "image_url",
          "image_url": {
            "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg",
          },
        },
      ],
    }
  ],
  max_tokens=300,
)

print(response.choices[0])
```
```
這張圖片展示了一條木製的步道，直直地延伸到景象的深處，消失在遠方的樹木和灌木叢中。步道兩旁是茂密的綠色草地，顯示了生機盎然的自然景觀。天空晴朗，有一些散布的白色和淡灰色的雲，天色呈現出漸變的藍色，與下方的綠色草地形成了強烈的對比。整個景象給人一種寧靜、和諧且充滿自然美的感覺。
```

### Google Colab 實戰 3: text-generation 
- https://platform.openai.com/docs/guides/text-generation
```python
response2 = client.chat.completions.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Who won the world series in 2020?"},
    {"role": "assistant", "content": "The Los Angeles Dodgers won the World Series in 2020."},
    {"role": "user", "content": "Where was it played?"}
  ]
)
```
```
print(response2.choices[0].message.content)
```
### Google Colab 實戰 4: 文生圖 
- https://platform.openai.com/docs/guides/images/usage
```python
response3 = client.images.generate(
  model="dall-e-3",
  prompt="a white siamese cat",
  size="1024x1024",
  quality="standard",
  n=1,
)

image_url = response3.data[0].url
image_url
```

- `提示詞`:An expressive oil painting of a chocolate chip cookie being dipped in a glass of milk, depicted as an explosion of flavors.
- `ChatGPT| DALLE-e-3回應`: [官方網址](https://openai.com/index/dall-e-3/)

### 更多OpenAI 實戰 :請參閱 底下參考書籍
- [OpenAI Cookbook](https://cookbook.openai.com/)
- [Building AI Applications with ChatGPT APIs](https://www.packtpub.com/product/building-ai-applications-with-chatgpt-apis/9781805127567)
  - [GITHUB](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-APIs/tree/main)
```
Part 1:Getting Started with OpenAI APIs
Chapter 1: Beginning with the ChatGPT API for NLP Tasks
Chapter 2: Building a ChatGPT Clone

Part 2: Building Web Applications with the ChatGPT API
Chapter 3: Creating and Deploying an AI Code Bug Fixing SaaS Application Using Flask
Chapter 4: Integrating the Code Bug Fixer Application with a Payment Service
Chapter 5: Quiz Generation App with ChatGPT and Django

Part 3: The ChatGPT, DALL-E, and Whisper APIs for Desktop Apps Development
Chapter 6: Language Translation Desktop App with the ChatGPT API and Microsoft Word
Chapter 7: Building an Outlook Email Reply Generator
Chapter 8: Essay Generation Tool with PyQt and the ChatGPT API
Chapter 9: Integrating ChatGPT and DALL-E API: Build End-to-End PowerPoint Presentation Generator
Chapter 10: Speech Recognition and Text-to-Speech with the Whisper API

Part 4:Advanced Concepts for Powering ChatGPT Apps
Chapter 11: Choosing the Right ChatGPT API Model
Chapter 12: Fine-Tuning ChatGPT to Create Unique API Models
```
