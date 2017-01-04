
### Easily prettify dates and datetimes with str()


```python
from datetime import date, datetime 
```


```python
today=date.today()
right_now=datetime.now()
(today, right_now)
```




    (datetime.date(2017, 1, 4), datetime.datetime(2017, 1, 4, 15, 59, 25, 191057))




```python
(str(today), str(right_now))
```




    ('2017-01-04', '2017-01-04 15:59:25.191057')


