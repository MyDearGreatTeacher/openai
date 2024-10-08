## [labs_4](./code/labs_4.md)
- [Prompt examples的各種範例](https://platform.openai.com/docs/examples)


## 英文文法修正(Grammar correction)
```python
response = client.chat.completions.create(
  model="gpt-3.5-turbo",
  messages=[
    {
      "role": "system",
      "content": "You will be provided with statements, and your task is to convert them to standard English."
    },
    {
      "role": "user",
      "content": "She no went to the market."
    }
  ],
  temperature=0.7,
  max_tokens=64,
  top_p=1
)
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
