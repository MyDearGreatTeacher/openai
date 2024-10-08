# 瞭解關鍵參數及其對生成的回應的影響
在上一章中，我們瞭解到 OpenAI API 不僅僅是一個端點，而是各種端點的集合。這些終結點由 HTTP 請求觸發，這些請求在相應的請求正文中包含參數。對於聊天完成，兩個必需的參數是 和 – 我們主要看到了更改消息參數如何影響生成的回應。但是，有大量影響 API 行為的可選參數，例如溫度、N 和最大標記數。modelmessages

在本章中，我們將探討這些可選的關鍵參數，並了解它們如何影響生成的回應。參數就像您在複雜機器上找到的刻度盤和旋鈕。通過調整這些刻度盤和旋鈕，您可以根據自己的喜好更改機器的行為。同樣，在 ChatGPT 領域，參數允許我們調整模型行為的更精細細節，影響它處理輸入和製作輸出的方式。這些中的每一個在塑造 OpenAI 的反應中都發揮著獨特的作用。

在本章結束時，您將瞭解如何調整這些參數以更好地滿足您的特定需求，它們如何影響輸出的品質、長度和樣式，以及如何有效利用它們以獲得最理想的結果。瞭解這一點很重要，因為當我們開始在智慧應用程式中為不同用例集成 API 時，這些參數將需要更改，並且瞭解生成的回應如何隨這些參數而變化將使我們能夠確定正確的設置。

具體來說，我們將介紹以下配方，每個配方都將專注於一個關鍵參數：

更改模型參數並瞭解其對生成的回應的影響
使用 n 參數控制生成的回應數
使用溫度參數確定生成響應的隨機性和創造性
技術要求
本章中的所有配方都要求您能夠訪問 OpenAI API（通過生成的 API 金鑰）並安裝 API 用戶端，例如 Postman。您可以參考第 1 章配方使用 Postman 發出 OpenAI API 請求，瞭解有關如何獲取 API 密鑰和設置 Postman 的更多資訊。

更改模型參數並瞭解其對生成的回應的影響
在第 1 章和第 2 章中，聊天完成請求都是使用 model 和 messages 參數發出的，並且始終等於該值。我們基本上忽略了模型參數。但是，此參數可能對任何其他參數的生成響應影響最大。與普遍的看法相反，OpenAI API 不僅僅是一個模型;它由具有不同功能和價位的多種型號提供支援。modelgpt-3.5-turbo

在這個公式中，我們將介紹兩個主要模型（GPT-3.5 和 GPT-4），學習如何更改參數，並觀察這兩個模型之間生成的回應如何變化。model

準備
確保您擁有具有可用使用積分的 OpenAI Platform 帳戶。如果沒有，請按照第 1 章中的設置 OpenAI Playground 環境配方進行操作。

此外，請確保已安裝 Postman，已創建新工作區，已創建新的 HTTP 請求，並且已正確配置該請求。這很重要，因為如果不配置，您將無法使用 API。如果您沒有如前所述安裝和配置 Postman，請按照第 1 章中的使用 Postman 建立 OpenAI API 請求配方進行操作。但是，如果您不記得了，下一節中的步驟 1-4 將說明配置過程。HeadersAuthorization

本章中的所有配方都將具有相同的要求。

怎麼做...
在 Postman 工作區中，選擇左上角功能表欄上的「新建」按鈕，然後從顯示的選項清單中選擇“HTTP”。這將創建一個新的無標題請求。
通過選擇“方法”下拉功能表，將 HTTP 請求類型從 GET 更改為 POST（預設情況下，它將設置為 GET）。
輸入以下 URL 作為聊天完成的終結點：https://api.openai.com/v1/chat/completions。
在子功能表中選擇「標題」，然後將以下鍵值對添加到其下方的表格中：
鑰匙

價值

Content-Type

application/json

Authorization

Bearer <your API key here>

在子功能表中選擇「正文」，然後選擇「原始」作為請求類型。輸入以下請求正文，向 OpenAI 詳細說明提示、系統訊息、聊天記錄以及生成完成回應所需的其他參數集：

{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Describe Donald Trump's time in office in a sentence that has six five-letter words. Remember, each word must have 5 letters"
    }
  ]
}

Copy

Explain
5. 發送 HTTP 請求後，您應該會看到來自 OpenAI API 的以下回應。請注意，您的回答可能有所不同。我們特別要注意的 HTTP 回應部分是內容值：

{
    "id": "chatcmpl-7rocZGT1K0edeqZ2dTx65sfWIGdQm",
    "object": "chat.completion",
    "created": 1693060327,
    "model": "gpt-3.5-turbo-0613",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": "Donald Trump's presidency showcased divisive politics and tumultuous events."
            },
            "finish_reason": "stop"
        }
    ],
    "usage": {
        "prompt_tokens": 33,
        "completion_tokens": 12,
        "total_tokens": 45
    }
}

Copy

Explain
6. 現在讓我們重複步驟 4 中的 HTTP 請求，並保持其他所有內容一致，但修改參數。具體來說，我們會將該參數的值更改為 。為終結點和請求正文輸入以下內容，然後按兩下「發送」 ：modelgpt-4

{
  "model": "gpt-4",
  "messages": [
    {
      "role": "user",
      "content": "Describe Donald Trump's time in office in a sentence that has six five-letter words. Remember, each word must have 5 letters"
    }
  ]
}

Copy

Explain
7. 您應該看到 OpenAI API 的以下類似回應。請注意，回應與我們之前收到的回應大不相同。值得注意的是，它與生成六個五個字母單詞的提示中的指令更接近：

# Response
{
    "id": "chatcmpl-7rohvZHiQHG0GPh0Ii0Qlcukdk8k7",
    "object": "chat.completion",
    "created": 1693060659,
    "model": "gpt-4",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": "Trump faced query, shook norms, split base"
            },
            "finish_reason": "stop"
        }
    ],
    "usage": {
        "prompt_tokens": 33,
        "completion_tokens": 8,
        "total_tokens": 38
    }
}

Copy

Explain
8. 重複步驟 1-4，但將內部的參數更改為以下提示：沒有別的。contentmessagesHow many chemicals exist in cigarettes, how many of them are known to be harmful, and how many are known to cause cancer? Respond with just the numbers,

同樣，執行一個參數為 gpt-4 的聊天完成請求。modelgpt-3.5-turbomodel

9. 以下是我使用 GPT-3.5-turbo 和 GPT-4 收到的 HTTP 回應的摘錄：

當 = gpt-3.5-turbo：model
"content": "There are thousands of chemicals in cigarettes, more than 7,000. Over 70 of them are known to be harmful, and at least 69 are known to cause cancer."

Copy

Explain
當 = gpt-4 時：model
"content": "6000, 250, 60"

Copy

Explain
10. 重複步驟 4-7，但將內部參數更改為以下邏輯問題提示：contentmessages

{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Which conclusion follows from the statement with absolute certainty?\n1. None of the stamp collectors is an architect.\n2. All the drones are stamp collectors.\nOptions:\na) All stamp collectors are architects.\nb) Architects are not drones.\nc) No stamp collectors are drones.\nd) Some drones are architects.\nOnly reply with the answer"
    }
  ]
}

Copy

Explain
請注意，請求正文為 JSON 格式的 HTTP 請求無法處理多行字串。因此，如果需要將多行字串寫入任何 API 參數（例如在本例中），請改用換行符 （\n）：messages

例如

"

Line 1

Line 2

"

會變成

"Line 1\nLine 2"

11. 以下是我收到的 HTTP 回應的摘錄：

當 = gpt-3.5-turbo：model
"content": "c) No stamp collectors are drones."

Copy

Explain
當 = gpt-4 時：model
"content": "b) Architects are not drones."

Copy

Explain
它是如何工作的...
在這個配方中，我們觀察了三個不同的例子，說明更改參數如何影響生成的文本。下表總結了 OpenAI 基於不同模型參數生成的不同回應：model

提示

型號 = gpt-3.5-turbo 時的回應

model = gpt-4 時的回應

用一個有六個五個字母的句子來描述唐納德·特朗普的執政時間。請記住，每個單詞必須有五個字母

Donald Trump's presidency showcased divisive politics and tumultuous events.

Copy

Explain
Trump faced query, shook norms, split base

Copy

Explain
香煙中有多少化學物質，其中有多少是已知有害的，有多少已知會導致癌症？只用數位來回應，別無他法

There are thousands of chemicals in cigarettes, more than 7,000. Over 70 of them are known to be harmful, and at least 69 are known to cause cancer.

Copy

Explain
6000, 250, 60

Copy

Explain
從這句話中得出的絕對肯定的結論是什麼？

沒有一個集郵者是建築師。
所有的無人機都是郵票收集器。
c) No stamp collectors are drones.

Copy

Explain
b) Architects are not drones.

Copy

Explain
在所有情況下，該模型產生的結果都比 更準確。例如，在描述唐納德·特朗普在任時間的第一個提示中，模型不明白它應該只使用五個字母的單詞，而能夠成功地回答它。gpt-4gpt-3.5-turbogpt-3.5-turbogpt-4

GPT-4 與 GPT-3.5
為什麼會這樣？兩種模型的內部工作原理不同。在 GPT 等神經網路模型中，參數是與其他參數組合的單個數值，這些數值執行計算，將輸入（如提示）轉換為輸出數據（如聊天完成回應）。參數數量越多，模型準確捕獲數據模式的能力就越大。

GPT-3.5 模型集使用 1750 億個參數進行訓練，而 GPT-4 模型集估計使用超過 100 萬億個參數進行訓練（在一組較小的模型上），高出許多數量級 （https://www.pcmag.com/news/the-new-chatgpt-what-you-get-with-gpt-4-vs-gpt-35）。GPT-4 背後的神經網路要密集得多，使其能夠理解細微差別並更準確地回答。

GPT 模型通常難以處理非常複雜和冗長的指令。例如，在香煙問題中，指令顯然是 .GPT-3.5 提供了一個合適的答案，但不是正確的格式，而 GPT-4 返回的答案是正確的格式。respond with just the numbers, nothing else

一般來說，GPT-4 比 GPT-3.5 更可靠，可以處理更細微的指令。值得注意的是，對於主要簡單的任務，這種區別可能是微妙的，甚至不存在。為了辨別這些差異，這兩個模型在各種基準和常見考試中進行了測試，這證明瞭 GPT-4 的強大功能。您可以在此處了解這些測試結果：https://openai.com/research/gpt-4。總體而言，GPT-4 在各種標準化考試中的表現優於 GPT-3.5，例如 AP 微積分、AP 英語文學和 LSAT。

GPT-4 和 GPT-3.5 之間的其他區別包括：

記憶體和上下文大小：GPT-4 可以保留更多記憶體並具有更大的上下文視窗 （https://platform.openai.com/docs/models），這意味著它可以接受比 GPT-3.5 更大、更複雜的提示。上下文視窗是指模型在生成回應時可以考慮的最近輸入量（以標記或文本塊為單位）。想像一下從一本書中間閱讀一段話;你能看到和記住的句子越多，你就越能理解該段落的上下文。同樣，通過更大的上下文視窗，GPT-4 可以看到並記住更多先前的輸入，從而能夠生成更多與上下文相關的回應。
視覺輸入：GPT-4 可以接受文本和圖像，而 GPT-3.5 是純文字的。
語言：GPT-3.5 和 GPT-4 都具有多語言能力，這意味著它們可以理解、解釋和響應英語以外的語言。然而，雖然 GPT-3.5 可以在多種語言中工作，但 GPT-4 提供了增強的語言技巧，並且可以超越其他語言的簡單語音。
對齊：GPT-4 更加一致，這意味著它有偏見，不會從基於人類的對抗性測試中提供有害的建議、錯誤代碼或不準確的資訊。在這種情況下，對齊是指調整 GPT-4 的回應以更符合道德和安全標準的過程，從而減少它提供有害建議、錯誤代碼或不準確資訊的可能性。
成本注意事項
GPT-4 和 GPT-3.5 之間的一個重要區別是成本。GPT-4 收取更高的令牌費率，如果選擇具有更大上下文視窗的模型，該費率也會增加。

標記是模型讀取為輸入或生成為輸出的文字塊。這些標記可以是單個字元、單詞的一部分或單詞本身。粗略的經驗法則是，1 個令牌等於 0.75 個單詞 （https://platform.openai.com/docs/introduction/key-concepts）。

在為聊天完成發出 API 請求時，響應始終包括請求中使用的令牌數，即物件。例如，以下是步驟 5 中回應的摘錄：usage

"usage": {
        "prompt_tokens": 33,
        "completion_tokens": 12,
        "total_tokens": 45
    }

Copy

Explain
這告訴我們，我們的提示是 33 個令牌，下面的回應是 12 個令牌，總共有 45 個令牌：Describe Donald Trump's time in office in a sentence that has six five-letter words. Remember, each word must have 5 letters

Donald Trump's presidency showcased divisive politics and tumultuous events

Copy

Explain
代幣的數量很重要，原因有兩個：

根據所選的模型，總標記數不能超過模型的最大標記數，也稱為上下文視窗。對於 GPT-3.5-turbo，這是 4,096 個代幣。這意味著，在使用該模型的任何 API 請求中，內容的總和不能超過 4,096 個令牌，即大約 3,000 個單詞。相比之下，GPT-4 有一個名為 的子模型，它的上下文視窗為 32,768 個標記，或大約 24,000 個單詞。messagesgpt-4-32k
令牌總數和您使用的模型決定了您為 API 請求收取的費用。例如，在第 5 步中，我們使用模型使用了 45 個代幣，這意味著請求成本為 0.0000675 美元。相比之下，使用相同的 45 個代幣將花費 0.00135 美元，是成本的 20 倍。gpt-3.5-turbogpt-4
決策標準
確定在聊天完成請求中使用哪種模型應取決於以下因素：

上下文視窗：確定聊天完成請求的可能上下文視窗。如果你的提示可能超過 12,000 字，那麼你需要使用 GPT-4，因為 GPT-3.5 下最大的模型最多只有 16,384 個代幣。
複雜性：確定聊天完成請求的複雜性。一般來說，如果它需要細微差別的理解和格式化說明，例如配方中的前兩個示例，或者如果它需要複雜的資訊綜合和邏輯問題解決，例如配方中的第三個示例，那麼您需要使用 GPT-4。對於任何數學或科學推理來說尤其如此——GPT-4 的表現要好得多。
成本：評估選擇 GPT-4 而不是 GPT-3.5 的成本影響。如果您使用具有最高上下文視窗的 GPT-4 模型，這可能是使用 GPT-3.5 的請求價格的 40 倍。
一般來說，你應該總是先使用和測試 GPT-3.5，看看它是否能提供合適的聊天完成，然後在絕對必要時轉向 GPT-4。

總體而言，該參數會影響生成的響應的品質，這很重要，因為 API 請求的不同用例將需要不同級別的複雜回應。model

使用 n 參數
對於您構建的某些智慧應用程式，您需要從同一提示生成多個文本。例如，如果我們正在構建一個生成公司口號的應用程式，您可能不僅希望生成一個回應，還希望生成多個回應，以便使用者可以選擇最佳回應。該參數控制要為每條輸入消息生成多少個聊天完成選項。它還可以控制使用 Images 終結點時生成的圖像數。n

在此配方中，我們將瞭解該參數如何影響生成的響應數量，並瞭解它的不同用例。n

怎麼做...
在 Postman 中，輸入以下 URL 作為聊天完成的終結點：https://api.openai.com/v1/chat/completions。
在請求正文中，鍵入以下內容，然後按兩下「發送」。請注意，我們添加了 te 參數並將其顯式設定為預設值 1：hn
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Create a slogan for a company that sells Italian sandwiches"
    }
  ],
  "n": 1
}

Copy

Explain
發送 HTTP 請求後，您應該會看到來自 OpenAI API 的以下（類似但不完全相同）回應：
{
    "id": "chatcmpl-7rqKJ2fxKkltvcIpAPiNH1MUPMBIO",
    "object": "chat.completion",
    "created": 1693066883,
    "model": "gpt-3.5-turbo-0613",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": "\"Indulge in the taste of Italy, one sandwich at a time.\""
            },
            "finish_reason": "stop"
        }
    ],
    "usage": {
        "prompt_tokens": 17,
        "completion_tokens": 16,
        "total_tokens": 33
    }
}

Copy

Explain
現在，我們將重複步驟 2 中的請求，但讓我們將參數更改為 .發送 HTTP 請求後，我們得到以下回應。請注意，現在 中有三個單獨的物件或回應。我們有效地收到了三種不同的提示回應：n3choices
{
    "id": "chatcmpl-7rqc4P2PY6BxEhVF7gSGRXPkAtoKt",
    "object": "chat.completion",
    "created": 1693067984,
    "model": "gpt-3.5-turbo-0613",
    "choices": [
        {
            "index": 0,
            "message": {
                "role": "assistant",
                "content": "\"Indulge in authentic flavor with our heavenly Italian sandwiches!\""
            },
            "finish_reason": "stop"
        },
        {
            "index": 1,
            "message": {
                "role": "assistant",
                "content": "\"Deliciously Authentic: Taste Italy in Every Bite!\""
            },
            "finish_reason": "stop"
        },
        {
            "index": 2,
            "message": {
                "role": "assistant",
                "content": "\"Delizioso Flavors in Every Bite!\""
            },
            "finish_reason": "stop"
        }
    ],
    "usage": {
        "prompt_tokens": 17,
        "completion_tokens": 35,
        "total_tokens": 52
    }
}

Copy

Explain
現在，讓我們生成圖像並觀察該參數如何影響返回的圖像數。在 Postman 中，為終結點輸入以下內容：https://api.openai.com/v1/images/generations。在請求正文中，鍵入以下內容，然後按兩下「發送」 ：n
{
    "prompt": "Ice cream",
    "n": 3,
    "size": "1024x1024"
}

Copy

Explain
發送 HTTP 請求後，您應該會看到來自 OpenAI API 的以下回應。值得注意的是，您應該看到三個不同的 URL，每個 URL 對應於生成的圖像。以下代碼塊中的 URL 已被人為壓縮。將 URL 複製並貼到瀏覽器中後，您應該會看到霜淇淋的圖像：
{
    "created": 1693068271,
    "data": [
        {
            "url": "https://oaidalleapiprodscus.blob.core.windows.net/private/org-...%3D"
        },
        {
            "url": "https://oaidalleapiprodscus.blob.core.windows.net/private/org-...s%3D"
        },
        {
            "url": "https://oaidalleapiprodscus.blob.core.windows.net/private/org-...%3D"
        }
    ]
}

Copy

Explain
Figure 3.1 – Output of the OpenAI image endpoint (﻿n=3)
圖 3.1 – OpenAI 影像端點的輸出 （n=3）

它是如何工作的...
該參數僅指定從 OpenAI API 產生的響應數量。對於聊天完成，它可以是任何整數;這意味著您可以要求 API 返回成千上萬的回應。對於圖像生成，此參數的最大值為10，這意味著每個請求最多只能生成10個圖像。n

n的應用
擁有參數的應用非常廣泛——在一個 HTTP 請求中，有一個參數來控制和重複同一提示的生成通常很有用。這些包括以下內容：n

創意：對於創意應用和任務，例如口號生成、歌曲創作或頭腦風暴，提供更豐富的工作材料集可以使使用者更輕鬆
冗餘：由於來自具有相同提示的 OpenAI API 的回應生成可能有很大差異，因此創建多個回應並交叉驗證資訊非常有用，尤其是在任務關鍵型工作流中
A/B 測試：該參數在行銷中非常常見，使您能夠創建多個回應，用戶可以嘗試這些回應以查看哪個回應效果更好n
n 的注意事項
但是，多代通常意味著較低的速度和較高的成本，這是在決定為參數設置什麼值之前需要考慮的考慮因素。例如，在我們的配方中，當我們請求一代時，成本為 33 個代幣（如回應中指定）。然而，當 時，代幣的總數躍升至 52 個代幣。我們在之前的配方中瞭解到，OpenAI API 根據生成的代幣總數收費。nn = 3

請注意，成本增加不是線性的——生成三個額外的回應只會多花費 ~60% 的代幣，而不是預期的 3 倍。這有兩個原因：

無論創建多少代，提示令牌的數量都是固定的，無論是 1 代還是 100 代
當模型知道生成多個完成而不是一個完成時，它會發現計算節省
這也是為什麼使用該參數（從成本的角度來看）比多次執行 HTTP 請求要好得多。在後台，當您設置 時，並行模型會在單個模型推理期間處理請求，從而利用固有的效率。例如，我們可以運行 HTTP 請求三次，而不是運行一個 HTTP 請求，但這意味著要花費 ~3 倍的成本和開銷。nn = 3n = 3

總體而言，該參數會影響生成的響應數量，這對於特定用例非常有價值，從而降低了成本。n

使用溫度參數確定生成響應的隨機性和創造性
溫度可能是最不為人所知的參數之一。總體而言，它控制了文本生成的創造力或隨機性。溫度越高，結果就越多樣化和創造性——即使對於相同的輸入也是如此。在實踐中，溫度是根據用例設置的。需要一致和標準世代的應用應使用非常低的溫度，而需要創造性方法的解決方案應選擇更高的溫度。

在此配方中，我們將瞭解溫度參數，觀察如何使用它來影響OpenAI API生成的文本生成。

怎麼做...
在 Postman 中，為終結點輸入以下內容：https://api.openai.com/v1/chat/completions。在請求正文中，鍵入以下內容，然後按兩下「發送」。我們的提示是 .請注意，我們添加了該參數並將其顯式設置為值。我們將重複此操作三次，並記錄每一代參數的回應：Explain gravity in one sentencetemperature0content
```
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Explain gravity in one sentence"
    }
  ],
  "temperature": 0
}
# Response 1
Gravity is the force that attracts objects with mass towards each other.
# Response 2
Gravity is the force that attracts objects with mass towards each other.
# Response 3
Gravity is the force that attracts objects with mass towards each other.
```
接下來，讓我們編輯請求正文並將參數更改為可能的最高值，即 。單擊「發送」，然後重複此操作三次，記錄每一代參數的回應：temperature2content
```
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Explain gravity in one sentence"
    }
  ],
  "temperature": 2
}
# Response 1
Gravity is the force that attract objects with mass towards each other, creating weight.
# Response 2
Gravity is a natural force that attracts objects toward each other based on their mass and distance between them.
# Response 3
Gravity is the universal force of attraction that pulls every object toward the Earth.

```
現在，讓我們重複步驟 1-2，但使用更有創意的提示，例如 .同樣，我們將首先執行 temperature 參數等於 3 次的聊天完成。
然後，我們將增加 temperature 參數並再次運行請求三次。以下代碼塊中列出了每一代參數的回應。
請注意，您的可能會有所不同：Create a creative tag line for an AI learning book02content
```
# Request Body
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Create a creative tag line for an AI learning book"
    }
  ],
  "temperature": 0
}
# Response 1
Unlock the Power of Artificial Intelligence: Ignite Your Mind, Transform Your Future!
# Response 2
Unlock the Power of Artificial Intelligence: Ignite Your Mind, Transform Your Future!
# Response 3
Unlocking Minds, Unleashing Code: Navigating the Frontiers of AI Learning
# Request Body
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "Create a creative tag line for an AI learning book"
    }
  ],
  "temperature": 2
}
```
```
# Response 1
Spark your mind – Accelerate with Artificial Excellence.
# Response 2
Unlock limitless intelligence: Medium approach, myth together.
# Response 3
Unleashing Minds: The AI Odyssey Awaits.
```
它是如何工作的...
正如我們在配方中看到的，溫度參數控制著文本生成的隨機性和創造性。
當溫度設置得非常低時，API 會為同一提示生成非常一致和確定的結果。
在第一個示例中，對於每次聊天完成，重力都以完全相同的方式解釋：

Gravity is the force that attracts objects with mass towards each other.


當我們提高溫度時，我們看到了非常不同、更有創意和意想不到的反應，例如：

Gravity is the universal force of attraction that pulls every object toward the Earth.


將溫度設置想像成收音機上的刻度盤。
較低的溫度就像將收音機調諧到一個信號強烈而清晰的成熟電臺，您可以獲得一致的、預期的音樂或脫口秀類型。
這類似於模型提供可靠、直接且與最可能的答案密切相關的回應。

相反，更高的溫度類似於將收音機調諧到一個頻率，您可以在該頻率上捕捉到各種電臺，有些清晰，有些充滿靜電，播放不拘一格的流派組合。
這創造了一個意想不到的、新穎的和多樣化的內容出現的環境。在語言模型的上下文中，這意味著產生更具創造性、多樣化、有時不可預測的回應，反映了無線電錶盤轉向不太明確的頻率的折衷和變化的性質。

溫度內部工作
正如我們之前所討論的，當模型生成文本時，它會根據到目前為止構建的提示和回應來計算下一個單詞的概率。
在實踐中，溫度通過改變下一個單詞的概率分佈來影響回應。

溫度越高，這種分佈就越平坦，這意味著不太可能的單詞被選中的機會更高。
在較低的溫度下，分佈變得更加明顯或更清晰，這意味著每次都可能選擇最可能的單詞，從而降低隨機性。

基於用例的決策
決定使用哪種溫度完全取決於特定的用例。通常，此參數分為三類。

- 低溫值（0.0 至 0.8）：這些值應主要用於分析、事實或邏輯任務，以便模型更具確定性和重點。
  - 在這些用例中，可追溯性和可重複性也很重要，因此溫度越低越好，因為它可以減少隨機性。較低的溫度還意味著遵守既定的模式和慣例，從而獲得更正確的答案。範例包括生成代碼、執行數據分析和回答事實問題。
- 中溫值（0.8 至 1.2）：這些值應用於通用和類似聊天機器人的任務，在這些任務中，平衡連貫性和創造力至關重要。
  - 這使得模型具有靈活性併產生新的想法，但它仍然專注於手頭的提示。範例包括聊天機器人/對話代理和問答系統。
- 高溫值（1.2 至 2.0）：這些值應用於創意寫作和頭腦風暴，因為模型不受既定模式的限制，可以探索非常多樣化的風格。
  - 在這裡，不存在正確的答案，相反，目的是創建不同的輸出。這確實意味著您可能會得到與實際提示根本不符的意外輸出。示例包括講故事、生成行銷口號和集思廣益公司名稱。

在食譜中，在解釋重力時，較低的溫度要好得多，因為提示鼓勵事實和直接的答案。

然而，第二個提示，關於創建標語，更適合更高的溫度，因為這是一項需要創造力和開箱即用思維的任務。

總體而言，設置溫度值意味著在連貫性和創造力之間進行權衡，這取決於您在應用程式中使用 API 的方式。
根據經驗，最好將溫度設置為 1，然後以 0.2 的增量進行修改，直到達到所需的輸出設置。
```
