# Developing_Apps_with_GPT-4_and_ChatGPT
- [GITHUB](https://github.com/malywut/gpt_examples/tree/main)


## Chap2_01_HelloWorld
```python
from dotenv import load_dotenv

load_dotenv()
import os
from openai import OpenAI

client = OpenAI()

# Make sure the environment variable OPENAI_API_KEY is set.

# Call the openai ChatCompletion endpoint, with th ChatGPT model
response = client.chat.completions.create(model="gpt-3.5-turbo",
messages=[
      {"role": "user", "content": "Hello World!"}
  ])

# Extract the response
print(response.choices[0].message.content)
```


## Chap2_02_ChatCompletion
```python
from dotenv import load_dotenv

load_dotenv()
from openai import OpenAI

client = OpenAI()

# For GPT 3.5 Turbo, the endpoint is ChatCompletion
response = client.chat.completions.create(model="gpt-3.5-turbo",
# Conversation as a list of messages.
messages=[
    {"role": "system", "content": "You are a helpful teacher."},
    {
        "role": "user",
        "content": "Are there other measures than time complexity for an \
        algorithm?",
    },
    {
        "role": "assistant",
        "content": "Yes, there are other measures besides time complexity \
        for an algorithm, such as space complexity.",
    },
    {"role": "user", "content": "What is it?"},
])

print(response.choices[0].message.content)
```

## Chap2_02_ChatCompletionFunctions
```python
from dotenv import load_dotenv

load_dotenv()
from openai import OpenAI

client = OpenAI()
import json


def find_product(sql_query):
    # Execute query here
    results = [
        {"name": "pen", "color": "blue", "price": 1.99},
        {"name": "pen", "color": "red", "price": 1.78},
    ]
    return results


function_find_product = {
        "name": "find_product",
        "description": "Get a list of products from a sql query",
        "parameters": {
            "type": "object",
            "properties": {
                "sql_query": {
                    "type": "string",
                    "description": "A SQL query",
                }
            },
            "required": ["sql_query"],
        },
    }



def run(user_question):
    # Send the question and available functions to GPT
    messages = [{"role": "user", "content": user_question}]

    response = client.chat.completions.create(model="gpt-3.5-turbo-0613", messages=messages, tools=[{"type": "function", "function": function_find_product }])
    response_message = response.choices[0].message

    # Append the assistant's response to the messages
    messages.append(response_message)
    

    # Call the function and add the results to the messages
    function_name = response_message.tool_calls[0].function.name
    if function_name == "find_product":
        function_args = json.loads(
            response_message.tool_calls[0].function.arguments
        )
        products = find_product(function_args.get("sql_query"))
    else:
        # Handle error
        products = []
    # Append the function's response to the messages
    messages.append(
        {
            "role": "tool",
            "content": json.dumps(products),
            "tool_call_id": response_message.tool_calls[0].id,
        }
    )
    # Get a new response from GPT so it can format the function's response into natural language
    second_response = client.chat.completions.create(model="gpt-3.5-turbo-0613",
    messages=messages)
    return second_response.choices[0].message.content


print(run("I need the top 2 products where the price is less than 2.00"))
```

## Chap2_02_JSON
```python
from openai import OpenAI
from dotenv import load_dotenv

load_dotenv()

client = OpenAI()

# Make sure the environment variable OPENAI_API_KEY is set.

# Call the openai ChatCompletion endpoint, with th ChatGPT model
response = client.chat.completions.create(
    model="gpt-3.5-turbo-1106",
    response_format={"type": "json_object"},
    messages=[{"role": "system",
               "content": "Convert the user's query in a JSON object"},
              {"role": "user",
               "content": "I am looking for blue or red shoes, leather, size 7."}])

# Extract the response
print(response.choices[0].message.content)
```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## Chap3_07_Streaming
```python
stream = client.chat.completions.create(
model="gpt-4",
messages=[{
    "role": "user",
    "content": "Write a 10 lines story for my 5 year old."}],
stream=True,
)

for chunk in stream:
    if chunk.choices[0].delta.content is not None:
        print(chunk.choices[0].delta.content, end="")
```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```
## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```
## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```
## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```
## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```

## 
```python

```
