# 第 4 章 讓 AI 計算技術指標及資料視覺化
- 4-1 技術指標公式太複雜？讓 AI 自動化計算
  - 讓 AI 自動生成技術指標程式碼
  - 資料處理自動化
- 4-2 資料視覺化
  - 繪製股價圖：matplotlib
  - 繪製 K 線圖：mplfinance
- 4-3 進階互動式圖表：plotly
  - 互動式 K 線圖
- 4-4 建構 Dash 應用程式
  - 運行 Dash 應用程式
  - 程式碼詳解：Dash 應用程式
  - [Dash Documentation & User Guide | Plotly](https://dash.plotly.com/)
  - [Dash Userguide](https://plot.ly/dash)
  - [](https://github.com/amadoukane96/dash-docs)
  - https://github.com/amadoukane96/dash-sample-apps
  - https://github.com/plotly/dash-sample-apps

## Web專案

![DataVisualization_1.JPG](../../pics/DataVisualization_1.JPG)

## 程式碼分析
![DataVisualization_0.JPG](../../pics/DataVisualization_0.JPG)


####
```python
import datetime as dt

import dash
import dash_bootstrap_components as dbc
from dash import dcc, html
from dash.dependencies import Input, Output
from my_commands.ai_calculate import download_stock_data
from my_commands.plot_k import create_stock_figure

app = dash.Dash(__name__, external_stylesheets=[dbc.themes.BOOTSTRAP])

app.layout = dbc.Container([
    # 上方的輸入框
    dbc.Row(
        [
            dbc.Col([
                html.Label("輸入股票代號:"),
                dcc.Input(id="stock-id-input", type="text", value="2330"),
            ],
                    width=2),
            dbc.Col([
                html.Label("開始日期:"),
                dcc.DatePickerSingle(
                    id="start-date-input",
                    date=dt.date.today() - dt.timedelta(days=365)),
            ],
                    width=2),
            dbc.Col([
                html.Label("結束日期:"),
                dcc.DatePickerSingle(id="end-date-input",
                                     date=dt.date.today()),
            ],
                    width=2),
            dbc.Col([
                html.Label("輸入技術指標:"),
                dcc.Input(id="indicator-input", type="text", value="MACD"),
            ],
                    width=2),
            dbc.Col(
                [
                    html.Br(),  # 空行
                    html.Button("確認", id="update-button")
                ],
                width=2)
        ],
        className="mb-3"),

    # 輸出圖形
    dbc.Row([dbc.Col([dcc.Graph(id="stock-graph")], width=12)])
])


@app.callback(Output("stock-graph", "figure"),
              [Input("update-button", "n_clicks")], [
                  dash.dependencies.State("stock-id-input", "value"),
                  dash.dependencies.State("start-date-input", "date"),
                  dash.dependencies.State("end-date-input", "date"),
                  dash.dependencies.State("indicator-input", "value")
              ])
def update_graph(n_clicks, stock_id, start_date, end_date, indicator):
  df = download_stock_data(stock_id, start_date, end_date, indicator)
  fig = create_stock_figure(stock_id, df)
  return fig


if __name__ == "__main__":
  app.run_server(host='0.0.0.0', port=8080)
```

```python
import datetime as dt

import openai  # 串接 OpenAI API
import yfinance as yf


# GPT 3.5 模型
def get_reply(messages):
  try:
    response = openai.ChatCompletion.create(model="gpt-3.5-turbo",
                                            messages=messages)
    reply = response["choices"][0]["message"]["content"]
  except openai.OpenAIError as err:
    reply = f"發生 {err.error.type} 錯誤\n{err.error.message}"
  return reply


# 設定 AI 角色, 使其依據使用者需求進行 df 處理
def ai_helper(df, user_msg):

  msg = [{
      "role":
      "system",
      "content":
      f"As a professional code generation robot, \n\
      I require your assistance in generating Python code \n\
      based on specific user requirements. To proceed, \n\
      I will provide you with a dataframe (df) that follows the \n\
      format {df.columns}. Your task is to carefully analyze the \n\
      user's requirements and generate the Python code \n\
      accordingly.Please note that your response should solely \n\
      consist of the code itself,\n\
      and no additional information should be included."
  }, {
      "role":
      "user",
      "content":
      f"The user requirement:{user_msg} \n\
    Your task is to create a function named 'calculate(df)' \n\
    that takes a dataframe as input. The function should process \n\
    the dataframe and return only the processed dataframe. \n\
    Please ensure that your response includes the Python code \n\
    for the 'calculate(df)' function \n\
    and does not include any other content."
  }]

  reply_data = get_reply(msg)
  cleaned_code = reply_data.replace("```", "")
  cleaned_code = cleaned_code.replace("python", "")
  cleaned_code = cleaned_code.replace("Python", "")
  
  return cleaned_code


# AI 進行技術指標計算及資料處理
def download_stock_data(stock_id, start=None, end=None, indicator='MACD'):
  stock_id = f"{stock_id}.tw"
  if not end:
    end = dt.date.today()
  if not start:
    start = end - dt.timedelta(days=365)
  # 從 yf 下載資料
  df = yf.download(stock_id, start=start, end=end).reset_index()

  # AI 計算技術指標
  code_str = ai_helper(df, f"計算{indicator}")
  print(code_str)

  # 將 exec 生成的 calculate 設為局部變數
  local_vars = {}
  exec(code_str, globals(), local_vars)
  calculate = local_vars['calculate']

  df = calculate(df)

  # 資料處理
  bk_df = df.reset_index()
  bk_df.index = bk_df["Date"].dt.strftime('%Y-%m-%d')

  return bk_df
```

```python
import pandas as pd
import plotly.graph_objects as go
from my_commands.ai_calculate import download_stock_data


# 繪製圖表函式
def create_stock_figure(stock_id, bk_df):

    # 創建 K 線圖
    fig = go.Figure(data=[go.Candlestick(x=bk_df.index,
                        open=bk_df['Open'],
                        high=bk_df['High'],
                        low=bk_df['Low'],
                        close=bk_df['Close'],
                        increasing_line_color='red',
                        decreasing_line_color='green',
                        name = "K 線")])

    # 交易量
    fig.add_trace(go.Bar(x=bk_df.index, y=bk_df['Volume'],
                         marker={'color': 'green'}, yaxis='y2',
                           name = "交易量"))

    # 找出需要繪製的欄位
    columns = bk_df.columns
    exclude_columns = ['index','Date', 'Open', 'High',
                        'Low', 'Close', 'Adj Close', 'Volume']
    remain_columns = [col for col in columns if
                       col not in exclude_columns]
    min_close = bk_df['Close'].min() - bk_df['Close'].std()
    max_close = bk_df['Close'].max() + bk_df['Close'].std()
    # 繪製技術指標
    for i in remain_columns:
      if min_close <= bk_df[i].mean() <= max_close:
        fig.add_trace(go.Scatter(x=bk_df.index, y=bk_df[i],
                                  mode='lines', name=i))
      else:
        fig.add_trace(go.Scatter(x=bk_df.index, y=bk_df[i],
                                  mode='lines', yaxis='y3', name=i))

    # 加入懸停十字軸
    fig.update_xaxes(showspikes=True, spikecolor="gray",
                    spikemode="toaxis")
    fig.update_yaxes(showspikes=True, spikecolor="gray",
                    spikemode="across")
    # 更新畫布大小並增加範圍選擇
    fig.update_layout(
        height=800,
        width=1200,
        yaxis={'domain': [0.35, 1]},
        yaxis2={'domain': [0.15, 0.3]},
        # 若要重疊 y1 和 y3, 可以改成
        # yaxis3=dict(overlaying='y', side='right')
        yaxis3={'domain': [0, 0.15]},
        title=f"{stock_id}",
        xaxis={
            # 範圍選擇格
            'rangeselector': {
                'buttons': [
                    {'count': 1, 'label': '1M',
                      'step': 'month', 'stepmode': 'backward'},
                    {'count': 6, 'label': '6M',
                      'step': 'month', 'stepmode': 'backward'},
                    {'count': 1, 'label': '1Y',
                      'step': 'year', 'stepmode': 'backward'},
                    {'step': 'all'}
                ]
            },
            # 範圍滑動條
            'rangeslider': {
                'visible': True,
                'thickness': 0.01,  # 滑動條的高度
                'bgcolor': "#E4E4E4"  # 背景色
            },
            'type': 'date'
        }
    )

    # 移除非交易日空值
    # 生成該日期範圍內的所有日期
    all_dates = pd.date_range(start=bk_df.index.min(),
                               end=bk_df.index.max())
    # 找出不在資料中的日期
    breaks = all_dates[~all_dates.isin(bk_df.index)]
    dt_breaks = breaks.tolist() # 轉換成列表格式
    fig.update_xaxes(rangebreaks=[{'values': dt_breaks}])

    return fig
```
