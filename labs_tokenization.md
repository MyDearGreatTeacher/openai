## token 與 tokenization
## 參考資料
- [Day10-Tokenizer 入門](https://ithelp.ithome.com.tw/articles/10298516)
- [Tokenizer](https://platform.openai.com/tokenizer)


### 使用 tiktoken 套件計算精確 token 數
- 安裝好用套件 ==>`!pip install tiktoken`
```python

import tiktoken

encoder = tiktoken.encoding_for_model('gpt-3.5-turbo')
print(encoder.name)
encoder = tiktoken.encoding_for_model('gpt-4')
print(encoder.name)

tokens = encoder.encode("你好")
print(tokens)
print(encoder.decode(tokens))

"""### ChatML 標記語言"""

print(encoder.encode("user"))
print(encoder.encode("assistant"))
print(encoder.encode("system"))
print(encoder.encode("\n"))

"""計算 message 總 tokens 數"""

def tokens_in_messages(messages, model="gpt-3.5-turbo",
                       show=False):
    totals = 0
    for message in messages:
        for k in message:
            if k == "content":
                totals += 4 # <|im_start|>user\n{內容}<|im_end|>
                totals += len(encoder.encode(message[k]))
    totals += 3 # <|im_start|>assistant<|message|>
    return totals

print(tokens_in_messages([
        {"role":"user", "content": "你好"}
    ]))

```
