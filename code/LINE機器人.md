# LINE機器人
- LINE 機器人
- 開發原理
- 設定 Messaging API
- 取得密鑰、存取令牌並加入機器人

其中有兩個最主要部分：
- Messaging API :負責作為中介在LINE 與我們撰寫的程式之間傳遞訊息。
- 後端程式：
  - 使用replit 平台建構後端伺服器，負責接收MessagingAPI 收到的使用者訊息（股票代號），進行處理後傳到OpenAI API ， 最後將分析報告傳回Messaging API 。

## 教科書2
- [最強 AI 投資分析](https://www.tenlong.com.tw/products/9789863127727?list_name=srh)
- [程式碼](https://www.flag.com.tw/bk/t/f3933)
- https://replit.com/@kknono668
```
▌第 6 章 股票分析機器人：部署至 LINE 及 Discord 上
6-1 建構股票分析機器人
股票分析機器人
6-2 部署 LINE 機器人
開發原理
設定 Messaging API
取得密鑰、存取令牌並加入機器人
Replit 專案：LINE 股票分析機器人
測試 LINE 股票分析機器人
程式碼詳解：LINE 機器人
6-3 部署 Discord 機器人
開發原理
建立 Discord 開發者應用程式
取得 TOKEN (授權令牌)
將 Discord 機器人加入伺服器
複製 Replit 專案：Discord 股票分析機器人
程式碼詳解：Discord 股票分析機器人
```
#### 1.LINE 開發者網站建立 MyStockGPT
- [LINE 開發者網站](https://developers.line.biz/zh-hant/)
- 步驟1:建立provider（開發單位）
  - provider 代表單一開發單位， 你可以視需要為組織內個別的開發人員或是部門、專案小組建立provider，並沒有嚴格的規則。
- 步驟2:建立Messaging API channel:
  - Messaging API channel 就是你商業ID 下的—個LINE 官方帳號，作為中介在LINE 與外部程式之間互相傳送聊天訊息的通道
- 步驟3:Create a new channel
  - Channel type ==>  Messaging API
    - Don't leave this empty
  - Provider ==>  MyStockGPT
    -  Don't leave this empty
  - Company or owner's country or region :Taiwan
  - Channel name ==>  MyStockGPT
    - Note: The channel name can't be changed for seven days.
    - Don't leave this empty
    - Don't use special characters (4-byte Unicode)
    - Enter no more than 20 characters
  - Channel description ==> Stock Bot股票分析機器人
    - Don't leave this empty
    - Don't use special characters (4-byte Unicode)
    - Enter no more than 500 characters
  - Stock Bot股票分析機器人 

#### 取得密鑰、存取令牌並加入機器人
- 後端程式與LINE API 溝通時並不是直接使用你的帳號和密碼登入，而是需要Channel 的Channel secret(密鑰）及Channel access token（存取令牌）。
- 密鑰是用來讓通道表明自己的身分，讓後端程式可以確認收到的請求是由通道發送過來，以便篩除非通道的請求，避免程式處理非LINE 傳來的訊息；
- 存取令牌則是讓我們的程式向LINE 驗證身分，確認可以回覆訊息至該通道。
- Basic settings ==> 取得Channel secret 密鑰
  - 請往下滾動找到Channelsecret 段落即會看到密鑰
  - Channel secret ==>  b571719116356b6a40fa6a8aeed73b6f
- Messaging API ==> 取得Channel access token 存取令牌
  - Channel access token ==>Channel access token (long-lived)
  - ye2mnXxK97So440wBtVow5PoKZMs2j2AfwKJUD3kdwYw3MsPTGLwHab9xEsKSqQCn8fcHb165zudNnMhunI6Kb5LkRK4GXszkcWlWb4doiXkKV2Vvg1nu6Q3vE4ZkklW60NP3z4XgQO64WVYmofZNgdB04t89/1O/w1cDnyilFU= 
- LIFF
- Security
- Statistics
- Roles

#### 停止自動回覆功能：
  - 新建立的通道預設會啟用自動回應功能，你可以設定在使用者輸入特定關鍵字時自動回覆預先設定好的訊息。
  - 不過我們要建立的是由後端程式回覆訊息的聊天機器人，所以要關閉此功能， 否則通道就不會將訊息轉送到後端程式。

#### 在LINE 中將聊天機器人加入成為好友：


## 2.在Replit建立專案 LINE 股票分析機器人
- 設定secrets
  - Channel secret
  - Channel access token
  - api_key 20240522  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
- 測試 LINE 股票分析機器人
- 程式碼詳解：LINE 機器人


## 3.串接後端程式與通道
- 步驟1:回到LINE 通道的頁面，並點擊Messaging API
- 步驟2:
- 步驟3:
- 步驟4:
- 步驟5:
- 步驟6:

- 通常設定完webhook 網址後，會需要一小段時間才會生效，如果你按了Verify 後出現錯誤，請稍後再試。如果等候很長一段時間測試還是錯誤， 請確認環境變數設定正確、webhook 設定的網址有" https/／＂開頭，以及有開啟Use webhook 功能。
