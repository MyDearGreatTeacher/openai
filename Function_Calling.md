# Function_Calling
- `提問詞`:如何使用Function Calling機制和Plug-ins呢？
- 使用Function Calling機制和Plug-ins的基本步驟如下：
  - 1.定義函式: 首先，您需要定義一個或多個函式，這些函式將被ChatGPT API呼叫。
    - 每個函式都應該有一個名稱、描述和參數列表。
  - 2.建立JSON模板: 接著，建立一個JSON模板，其中包含了函式的名稱和所需的參數。
    - 這個模板將指導ChatGPT API生成正確格式的輸出，以便呼叫您的函式。
  - 3.發送API請求: 使用ChatGPT API時，在您的API請求中包含上述JSON模板。
    - 這樣，當API處理您的請求時，它會根據模板生成結構化的輸出。
  - 4.處理API回應: API回應將包含一個或多個函式呼叫的結果。
    - 您需要在您的應用程式中處理這些結果，並根據需要進行適當的操作。
  - 5.整合Plug-ins: 如果您使用的是Plug-ins，則可以將它們整合到您的應用程式中，以擴充ChatGPT的功能。
    - Plug-ins可以是外部API、數據庫查詢或任何其他工具。
  - 這些步驟可以幫助您有效地使用Function Calling機制和Plug-ins來擴充ChatGPT的功能，並創建更強大的應用程式。
  - 如果您需要更具體的例子或進一步的指導，我可以提供相關資訊。
- `提問詞`: 你能給我一個實際的例子嗎？
- 當然可以。以下是一個使用Function Calling機制和Plug-ins的實際例子：
- 假設我們想要創建一個功能，讓用戶可以查詢波士頓目前的天氣。
- 我們可以定義一個函式get_current_weather，它需要兩個參數：地點（location）和溫度單位（unit）。
- 這個函式將會呼叫一個外部天氣API來獲取資訊。
- 定義函式:JSON
```JSON
{
  "name": "get_current_weather",
  "description": "Get the current weather in a given location",
  "parameters": {
    "location": {
      "type": "string",
      "description": "The city and state, e.g., Boston, MA"
    },
    "unit": {
      "type": "string",
      "enum": ["celsius", "fahrenheit"],
      "description": "The temperature unit"
    }
  }
}
```
- 建立JSON模板:
```JSON
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {"role": "user", "content": "What is the weather like in Boston?"}
  ],
  "functions": [
    {
      "name": "get_current_weather",
      "parameters": {
        "location": "Boston, MA",
        "unit": "celsius"
      }
    }
  ]
}
```
- 發送API請求: 當用戶問：“波士頓目前天氣如何？”，API會根據上述模板生成一個JSON輸出，這個輸出包含了呼叫get_current_weather函式所需的所有資訊。
- 處理API回應: 天氣API回應後，我們會得到一個包含波士頓當前天氣的JSON物件，例如：
```JSON
{
  "temperature": 22,
  "unit": "celsius",
  "description": "Sunny"
}
```
- 接著，我們可以將這個結果餵回給ChatGPT API，讓它以自然語言形式回覆用戶：“波士頓現在的天氣是攝氏22度，天氣晴朗。”
- 這個例子展示了如何通過Function Calling機制和Plug-ins來整合外部API，並以結構化的方式與ChatGPT API互動。
- 這種方法可以提高應用程式的靈活性和準確性1。

