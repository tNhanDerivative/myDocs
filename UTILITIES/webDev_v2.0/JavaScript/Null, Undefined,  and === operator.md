
- [[#Định nghĩa|Định nghĩa]]
- [[#Trường hợp xuất hiện|Trường hợp xuất hiện]]

  
## Định nghĩa
Null: có nghĩa là biến có giá trị rỗng
undefined: có nghĩa là biến có giá trị không xác định

**Differentiating using isNaN():** You can refer to [NaN](https://www.geeksforgeeks.org/number-isnan-javascript/) article for better understanding.

```javascript
isNaN(2 +  null)      // false
isNaN(2 + undefined) // true
```


## Trường hợp xuất hiện
Undefined: 
- Khi biến được khởi tạo nhưng chưa gán giá trị
- Khi biến dc gán giá trị không tồn tại
```javascript
var temp=['a','b','c'];
if(temp[3] === undefined)
console.log("true");
else
console.log("false");
```

Null: được gán