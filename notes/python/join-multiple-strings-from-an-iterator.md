
## Join multiple strings from an iterator


```python
# Example data
text_items = ['This is some text.', 
              'This is also some text.', 
              'Hey, more text.', 
              'Wait, this is text too, isn\'t it?']
```


```python
' '.join([text for text in text_items])
```




    "This is some text. This is also some text. Hey, more text. Wait, this is text too, isn't it?"


