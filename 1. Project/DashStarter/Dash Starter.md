
## Data loader
```python
import pandas as pd

class DataField:
    AMOUNT = "amount"
    CATEGORY = "category"
    DATE = "date"
    MONTH = "month"
    YEAR = "year"

def load_transaction_data(path: str) -> pd.DataFrame:
    # load the data from the CSV file
    data = pd.read_csv(
        path,
        dtype={
            DataField.AMOUNT: float,
            DataField.CATEGORY: str,
            DataField.DATE: str,
        },
        parse_dates=[DataField.DATE],
    )
    data[DataField.YEAR] = data[DataField.DATE].dt.year.astype(str)
    data[DataField.MONTH] = data[DataField.DATE].dt.month.astype(str)
    return data
```

1. Tạo DataField thay vì dùng giá trị string 
- Để ta có thể sử dụng auto complete, 
- Ta có thể nhấn `Ctr + click` để tìm ra giá trị
- Sử dụng ở nhiều file khác nhau ko lo bị sai chính tả

2. Đọc giải thích hàm read_csv ở đây: [[Read data]]

## Dropdown

### Year dropdown

```python
all_years: list[str] = data[DataField.YEAR].tolist()
unique_years = sorted(set(all_years), key=int)
```

1. type hint 
`all_years: list[str] =['a', 'b', 'c']` 
Xem thêm về  type hint: (https://viblo.asia/p/gioi-thieu-ve-type-hints-trong-python-gGJ596JrKX2)

2. Remove duplicate trong list dùng hàm `set()`
3.  Đọc về hàm sorted ở đây (https://www.w3schools.com/python/ref_func_sorted.asp)