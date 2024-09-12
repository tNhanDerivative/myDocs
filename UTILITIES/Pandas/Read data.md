
```
date,amount,company
2020-01-01,89.92,Raymond Architural Design
2020-01-01,48.74,Jason Lighting
2020-01-01,39.08,Raymond Architural Design
2020-01-01,48.36,Raymond Architural Design
2020-01-01,77.36,Raymond Architural Design
2020-01-01,73.46,Raymond Architural Design
2020-01-01,57.16,Raymond Architural Design
```
# how to specify data type

You can use the following basic syntax to specify the dtype of each column

```python
df = pd.read_csv('my_data.csv',
				  dtype = {'amount': float, 'company': str}, parse_dates=['date'])
```

# Date time
You should add `parse_dates=['column name']`  or  `parse_dates=True` when reading data.

```pandas
df = pd.read_csv('my_data.csv',
				  dtype = {'column1': str, 'column2': float,}, parse_dates=['column3'])
```

But there are always weird formats which need to be defined manually. In such a case you can also add a date parser function

- Giả sử column 'datetime' có type là string

```python
from datetime import datetime
dateparse = lambda x: datetime.strptime(x, '%Y-%m-%d %H:%M:%S')

df = pd.read_csv(infile, parse_dates=['datetime'], date_parser=dateparse)
```

- Suppose you want to combine multiple columns into a single datetime column:

```python
dateparse = lambda x: datetime.strptime(x, '%Y-%m-%d %H:%M:%S')

df = pd.read_csv(infile, parse_dates={'datetime': ['date', 'time']}, date_parser=dateparse)
```

you can find directives (i.e. the letters to be used for different formats) for `strptime` and `strftime` [in this page](https://docs.python.org/2/library/datetime.html#strftime-and-strptime-behavior).

- Now you want to seperate to multiple column to easily use
 ```python
df = pd.read_csv(infile, parse_dates=['date'], 
						data['year'] = data['date'].dt.year.astype(str)
						data['month'] = data['date'].dt.month.astype(str) )
```