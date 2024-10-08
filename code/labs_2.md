## 範例學習1:
- 範例學習1
  - Google Colab的設定
  - 測試 OpenAI API
    - 使用 Playground 熟悉 API
    - 使用 Python requests 模組呼叫 API
    - 使用 curl 工具快速測試 API
  - API 參數解析

## 範例學習1
### Google Colab設定與隱藏金鑰的方法 [Google Colab](https://colab.research.google.com/)
- Colab 提供有 Secret, 可以依帳戶儲存機密資料, 並設定是否允許必記本存取。
- 機密資料不會隨筆記本分享出去, 只有帳戶擁有者才能讀取。
```python
from google.colab import userdata
userdata.get('A888168GPT')
```
- 程式中就可以不用出現金鑰了
![Colab_1.JPG](../pics/Colab_1.jpg)


![Colab_2.JPG](../pics/Colab_2.jpg)



## 使用官方 openai 套件
  - OpenAI 官方提供有 openai 套件, 可以簡化直接使用 requests 模組的複雜度。
  - 安裝與使用 openai 套件 ==>  `!pip install openai`
```python
!pip install openai
!pip install rich
```
```python
import openai
from google.colab import userdata
OPENAI_API_KEY= userdata.get('A888168GPT')
client = openai.OpenAI(api_key=OPENAI_API_KEY)
```
```
reply = client.chat.completions.create(
    model = "gpt-3.5-turbo",
    # model = "gpt-4",
    messages = [
        {"role":"user", "content": "你好"}
    ]
)
```
- 檢視傳回物件 ==> print(reply)
```python
from rich import print as pprint
pprint(reply)
```
```
print(reply.choices[0].message.content)
```

### 傳遞多筆訊息
```
reply = client.chat.completions.create(
    model = "gpt-3.5-turbo",
    messages = [
        {"role":"system",
         "content":"你是條住在深海、只會台灣中文的魚"},
        {"role":"user", "content": "你住的地方很亮嗎？"}
    ]
)

print(reply.choices[0].message.content)
```
![Colab_3.JPG](../pics/Colab_3.jpg)
![Colab_4.JPG](../pics/Colab_4.jpg)
![Colab_5.JPG](../pics/Colab_5.jpg)
### 使用 Python requests 模組呼叫 API
- Python 提供有 requests 模組, 可以快速發送 HTTP 請求
- 如果是要在本機上執行, 必須安裝 requests 套件 ==> !pip install requests
```python
import requests
import os

response = requests.post(
    'https://api.openai.com/v1/chat/completions',
    headers = {
        'Content-Type': 'application/json',
        'Authorization': f'Bearer {userdata.get("A888168GPT")}'
    },
    json = {
        'model': 'gpt-3.5-turbo',
        "messages": [{"role": "user", "content": "你好"}]}
)
```
### 使用 curl 測試 API

```
import os
os.environ['OPENAI_API_KEY'] = userdata.get("OPENAI_API_KEY")

# Commented out IPython magic to ensure Python compatibility.
%%bash
curl -s https://api.openai.com/v1/chat/completions \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $OPENAI_API_KEY" \
-d '{
      "model": "gpt-3.5-turbo",
      "messages": [
        {
         "role": "user", "content": "你好"
        }
       ]
     }'
```
#### 2-4 加入組織成員

## 使用 Python requests 模組呼叫 API
## 使用 curl 工具快速測試 API
```
client = openai.OpenAI(
    api_key=userdata.get('OPENAI_API_KEY'),
    organization='你的組織識別碼'
)

reply = client.chat.completions.create(
    model = "gpt-3.5-turbo",
    # model = "gpt-4",
    messages = [
        {"role":"user", "content": "你好"}
    ]
)

print(reply.choices[0].message.content)
```
```
import requests
import os

response = requests.post(
    'https://api.openai.com/v1/chat/completions',
    headers = {
        'Content-Type': 'application/json',
        'Authorization': f'Bearer {userdata.get("OPENAI_API_KEY")}',
        'OpenAI-Organization': '你的組織識別碼'
    },
    json = {
        'model': 'gpt-3.5-turbo',
        "messages": [{"role": "user", "content": "你好"}]}
)

print(response.json()['choices'][0]['message']['content'])
```

```
# Commented out IPython magic to ensure Python compatibility.
# %%bash
# curl -s https://api.openai.com/v1/chat/completions \
# -H 'Content-Type: application/json' \
# -H "Authorization: Bearer $OPENAI_API_KEY" \
# -H "OpenAI-Organization: 請換成你的組織識別碼" \
# -d '{
#       "model": "gpt-3.5-turbo",
#       "messages": [
#         {
#           "role": "user", "content": "你好"
#         }
#       ]
#     }'
```


