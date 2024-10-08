# OpenAI API功能(CAPABILITIES)
- 文本生成(Text generation)
- Function calling
- Embeddings
- Fine-tuning
- Image generation
- Vision
- Text-to-speech
- 語音轉文本(Speech-to-text)
- Moderation

## 文本生成
- OpenAI 的文本生成模型（通常稱為生成式預訓練轉換器或大型語言模型）已經過訓練，可以理解自然語言、代碼和圖像。
- 模型提供文本輸出以回應其輸入。
- 這些模型的文本輸入也稱為“提示”。
- 設計提示本質上是你如何“程式設計”大型語言模型模型，通常是通過提供說明或一些如何成功完成任務的示例。
- 使用 OpenAI 的文字產生模型，您可以構建應用程式以：
  - 檔草稿(Draft documents)
  - 編寫計算機代碼(Write computer code)
  - 回答有關知識庫的問題(Answer questions about a knowledge base)
  - 分析文本(Analyze texts)
  - 為軟體提供自然語言介面(Give software a natural language interface)
  - 一系列學科的導師(Tutor in a range of subjects)
  - 翻譯語言(Translate languages)
  - 模擬遊戲角色(Simulate characters for games)

#### code_1:Chat Completions API
```python
response1 = client.chat.completions.create(
  model="gpt-3.5-turbo",
  messages=[
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Who won the world series in 2020?"},
    {"role": "assistant", "content": "The Los Angeles Dodgers won the World Series in 2020."},
    {"role": "user", "content": "Where was it played?"}
  ]
)
print(response1.choices[0].message.content)
```
- 主要輸入是 messages 參數。
- messages必須是message objects的陣列，其中每個物件都有一個角色（“系統”、“使用者”或“助手”）和內容。
- Conversations可以短至一條消息，也可以是多次來回切換。
- 通常，Conversations的格式首先使用系統消息(system message)，然後是交替的`使用者消息(user messages)`和`助手消息(assistant messages)`。
- `系統消息(system message)`有助於設置助手的行為。例如，您可以修改助手的個性，或提供有關其在整個對話中應如何表現的具體說明。
- 但是，請注意，`系統消息(system message)`是可選的，沒有系統消息的模型的行為可能類似於使用通用消息，例如“你是一個有用的助手”。
- `使用者消息(user messages)`提供請求或評論，供助手回應。
- `助手消息(assistant messages)`存儲以前的助理回復，但也可以由您編寫，以提供所需行為的示例。
- Chat Completions response format

#### code_2:設定JSON mode回傳
```python
response2 = client.chat.completions.create(
  model="gpt-3.5-turbo-0125",
  response_format={ "type": "json_object" },
  messages=[
    {"role": "system", "content": "You are a helpful assistant designed to output JSON."},
    {"role": "user", "content": "Who won the world series in 2020?"}
  ]
)

print(response2.choices[0].message.content)
```

## Function calling
## 文字嵌入| (word) Embeddings 
- 什麼是Embeddings嵌入？
  - OpenAI 的文字嵌入測量文字字串的相關性。
  - 文字(word) ==> 向量(vector)
- 用途:
  - 嵌入通常用於：
    - 搜尋（其中結果按與查詢字串的相關性進行排名）
    - 聚類分析（其中文字字串按相似性分組）
    - 建議（建議包含相關文字字串的專案）
    - 異常檢測（識別相關性不高的異常值）
    - 多樣性測量（分析相似性分佈）
    - 分類（其中文字字串按其最相似的標籤進行分類）
- 嵌入是浮點數的向量（清單）。
- 兩個向量之間的距離衡量它們的相關性:小距離表示高相關性，大距離表示低相關性。
- ==> 使用embeddings API endpoint 
```python
response = client.embeddings.create(
    input="Your text string goes here",
    model="text-embedding-3-small"
)

print(response.data[0].embedding)
```
- 嵌入模型(Embedding models)
  - OpenAI 提供了兩個強大的第三代嵌入模型:
    - text-embedding-3-small
    - text-embedding-3-large
    - text-embedding-ada-002  

#### 範例練習
- 資料集(DataSet) :[亞馬遜食品評論數據集(Amazon Fine Food Reviews)](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews)
  - 截至 2012 年 10 月，該DataSet共包含亞馬遜使用者留下的 568,454 條食品評論。
  - 我們將使用 1,000 條最新評論的子集進行說明。
  - 評論是用英文寫的，往往是正面或負面的。
  - 每條評論有五個欄位:
    - ProductId
    - UserId
    - 分數Score
    - 評論標題（摘要）review title (Summary)
    - 評論正文（文本）review body (Text) 


## Fine-tuning
- 為應用程式自定義模型。
- 通過微調，您可以通過提供以下功能從 API 提供的模型中獲得更多收益：
  - 比提示更高質量的結果
  - 能夠對比提示更多的示例進行訓練
  - 由於較短的提示而節省了代幣
  - 更低的延遲請求
- OpenAI 的文本生成模型已經在大量文本上進行了預訓練。
- 為了有效地使用這些模型，我們在提示中包括說明，有時還包括幾個示例。
- 使用演示來展示如何執行任務通常稱為「小樣本學習」。
- 微調通過訓練比提示符中容納的更多的示例來改進小樣本學習，讓您在大量任務上取得更好的結果。
- 微調模型后，無需在提示中提供太多示例。這樣可以節省成本並實現更低延遲的請求。
- 微調涉及以下步驟：
  - 準備和上傳訓練數據
  - 訓練新的微調模型
  - 評估結果，並根據需要返回步驟
  - 使用微調的模型

## 圖像生成(Image generation) 
- [使用Copilot| Designer產生DALL-E 3圖片(20240526)](Image_Generation_20240526.md)
- 使用 DALL-E API
  - DALL-E 2
  - DALL-E 3 
- Images API I 提供了三種與影像互動的方法：
  - 1.根據文字提示從頭開始創建影像（DALL-E 3 和 DALL-E 2）
  - 2.圖像編輯:使用文本提示來編輯原來的圖像之某區域來生成創建新圖像（只能使用 DALL-E 2）
  - 3.建立現有圖像的變體 （只能使用 DALL-E 2）
- [圖像生成 AI 的原理與應用 系列](https://ithelp.ithome.com.tw/users/20162522/ironman/6400)
- [[Day 29] 淺談 DALL·E 2 的原理](https://ithelp.ithome.com.tw/articles/10336415)
- https://zhuanlan.zhihu.com/p/662501902
#### 範例練習1:根據文字提示從頭開始創建影像
- 使用DALL-E 3，圖像的大小可以為three different image sizes: 1024x1024、1024x1792 或 1792x1024 像素(pixels)
- 圖像品質設定 ==>  Standard vs HD Quality
  - 預設情況下，圖像是以`standard`品質生成的
  - `(new)`使用DALL-E 3 設定quality="hd" 來加強圖片細節
  - Square, standard quality images are the fastest to generate.
- 生成圖像張數:
  - 使用DALL-E 2 ==> 一次最多生成 10 張圖像(n 參數)
  - 使用DALL-E 3 ==> 一次最多生成 1 張圖像(n 參數) 
- 風格 ==> natural look vs vivid look
  - DALL-E 3 introduces two new styles: natural and vivid
- 推薦閱讀 [OpenAI Cookbook|What's new with DALL·E 3?](https://cookbook.openai.com/articles/what_is_new_with_dalle_3)
- 論文
  - DALLE-1 [Zero-Shot Text-to-Image Generation(202102)](https://arxiv.org/pdf/2102.12092)
  - DALLE-2 [Hierarchical Text-Conditional Image Generation with CLIP Latents(202204)](https://arxiv.org/abs/2204.06125)
    - 其他測試論文
      - [A very preliminary analysis of DALL-E 2(202204)](https://arxiv.org/abs/2204.13807)
      - [DALLE-2 is Seeing Double: Flaws in Word-to-Concept Mapping in Text2Image Models(202210)](https://arxiv.org/abs/2210.10606)  
  - DALLE-3 [Improving Image Generation with Better Captions](https://cdn.openai.com/papers/dall-e-3.pdf)
```python
response = client.images.generate(
  model="dall-e-3",
  prompt="a white siamese cat",
  size="1024x1024",
  quality="standard",
  n=1,
)

image_url = response.data[0].url
```
#### 範例練習2: 圖像編輯|Edits| 修復`inpainting` (只能使用 DALL·E 2)
- 圖像編輯端點允許您通過上傳圖像和MASK來編輯或擴展圖像，以指示應替換哪些區域。
- MASK的透明區域指示應編輯圖像的位置，提示應描述完整的新圖像，而不僅僅是擦除的區域。
```python
response = client.images.edit((
  model="dall-e-2",
  image=open("sunlit_lounge.png", "rb"),
  mask=open("mask.png", "rb"),
  prompt="A sunlit indoor lounge area with a pool containing a flamingo",
  n=1,
  size="1024x1024"
)

image_url = response.data[0].url
```
- 上傳的圖片和MASK必須都是大小小於 4MB 的方形 PNG 圖片，並且彼此之間的尺寸必須相同。
- 生成輸出時不使用MASK的非透明區域，因此它們不一定需要像上面的示例那樣與原始圖像匹配。

#### 範例練習3: Variations|建立現有圖像的變體 (只能使用 DALL·E 2 )
- 輸入圖像必須是大小小於 4MB 的正方形 PNG 圖像。
-  image variations endpoint 
```python
response = client.images.create_variation(
  model="dall-e-2",
  image=open("corgi_and_cat_paw.png", "rb"),
  n=1,
  size="1024x1024"
)

image_url = response.data[0].url
```
## Vision
```python
response = client.chat.completions.create(
  model="gpt-4o",
  messages=[
    {
      "role": "user",
      "content": [
        {"type": "text", "text": "What’s in this image?"},
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
- Multiple image inputs
```python
response = client.chat.completions.create(
  model="gpt-4o",
  messages=[
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "What are in these images? Is there any difference between them?",
        },
        {
          "type": "image_url",
          "image_url": {
            "url": "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Gfp-wisconsin-madison-the-nature-boardwalk.jpg/2560px-Gfp-wisconsin-madison-the-nature-boardwalk.jpg",
          },
        },
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
## Voice- generation 輸入文字生成語言(Text-to-speech)
- 文字轉語音:將文本轉換為逼真的語音音訊
- speech endpoint 提供基於 TTS（text-to-speech|文字到語音轉換）模型的語音終結點。
- 內建有 6 種內置聲音(voices)
  - alloy
  - echo
  - fable
  - onyx
  - nova
  - shimmer 
- 應用：
  - 敘述一篇書面博客文章
  - 製作多種語言的語音音訊
  - 使用流媒體提供即時音訊輸出
- 端點接受三個關鍵輸入：
  - 模型(model)
  - text:應轉換為音訊的文本
  - voice:用於音訊生成的語音 
- 輸出
  - 支援的輸出格式(Supported output formats)
  - 預設輸出格式為“mp3”，但其他格式也可用:
  - Opus：用於互聯網流媒體和通信，低延遲。
  - AAC：用於數位音訊壓縮，YouTube、Android、iOS 首選。
  - FLAC：用於無損音訊壓縮，深受音訊愛好者的青睞。
  - WAV：未壓縮的WAV音訊，適用於低延遲應用，避免解碼開銷。
  - PCM：類似於 WAV，但包含 24kHz（16 位有符號，低端）的原始樣本，不帶標頭。 
- 支援的語言
  - TTS 模型在語言支援方面通常遵循 Whisper 模型。
  - Whisper 支援許多語言
  - 當前語音已針對英語進行了優化，性能良好
```python
from pathlib import Path

speech_file_path = Path(__file__).parent / "speech.mp3"
response = client.audio.speech.create(
  model="tts-1",
  voice="alloy",
  input="Today is a wonderful day to build something people love!"
)

response.stream_to_file(speech_file_path)
```
- 流式傳輸即時音訊(Streaming real time audio)
  - Speech API  支援使用`塊傳輸編碼(chunk transfer encoding)`的即時音訊流。
  - 音訊可以在生成完整檔並使其可存取前播放。
```python
response = client.audio.speech.create(
    model="tts-1",
    voice="alloy",
    input="Hello world! This is a streaming test.",
)

response.stream_to_file("output.mp3")
```
## 語音辨識|語音轉文本|輸入語音檔案辨識成文字 (Speech-to-text)
- Audio API提供兩個語音轉文本endpoints:
  - transcriptions
  - translations 
- 使用最先進的開源 large-v2 Whisper 模型。
- 應用：
  - 1.將音訊轉錄為任何語言的音訊Transcribe audio into whatever language the audio is in.
  - 2.將音訊翻譯並轉錄為英語Translate and transcribe the audio into english
- 檔案上傳目前限制為 25 MB
- 支援以下輸入檔案類型：mp3, mp4, mpeg, mpga, m4a, wav, and webm
#### 改編  Transcriptions API 
- 轉錄 API 將要轉錄的音訊檔和音訊轉錄所需的輸出檔案格式作為輸入。
- 目前支援多種輸入和輸出檔案格式。

```python
audio_file= open("/path/to/file/audio.mp3", "rb")
transcription = client.audio.transcriptions.create(
  model="whisper-1", 
  file=audio_file
)
print(transcription.text)
```

```python
audio_file = open("/path/to/file/speech.mp3", "rb")
transcription = client.audio.transcriptions.create(
  model="whisper-1", 
  file=audio_file, 
  response_format="text"
)
print(transcription.text)
```
#### 翻譯(Translations)
- translations API 將任何受支援語言的音訊檔作為輸入，將音訊轉錄為英語。
- 與Transcriptions 終結點不同，因為輸出不是原始輸入語言，而是翻譯成英語文本。
- 目前僅支援輸出成英文
```python
audio_file= open("/path/to/file/german.mp3", "rb")
translation = client.audio.translations.create(
  model="whisper-1", 
  file=audio_file
)

print(translation.text)
```

```python
from pydub import AudioSegment

song = AudioSegment.from_mp3("good_morning.mp3")

# PyDub handles time in milliseconds
ten_minutes = 10 * 60 * 1000

first_10_minutes = song[:ten_minutes]

first_10_minutes.export("good_morning_10.mp3", format="mp3")
```
#### 增加可告度的做法(Improving reliability)
- using the optional prompt parameter 
```python
audio_file = open("/path/to/file/speech.mp3", "rb")
transcription = client.audio.transcriptions.create(
  model="whisper-1", 
  file=audio_file, 
  response_format="text",
  prompt="ZyntriQix, Digique Plus, CynapseFive, VortiQore V8, EchoNix Array, OrbitalLink Seven, DigiFractal Matrix, PULSE, RAPT, B.R.I.C.K., Q.U.A.R.T.Z., F.L.I.N.T."
)
print(transcription.text)
```
- Post-processing with GPT-4
## Moderation

