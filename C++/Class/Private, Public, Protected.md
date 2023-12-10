
## Private member variable
Ví dụ: 
Biến status của đèn chỉ dc thay khi đèn được bật, tắt. 
Do đó ko thể cho phép access thay đổi giá trị của biến này bên ngoài class
## Private member Function
- Function may change or read provide access to Private member variable 
- But haven't check business logic or validate access
=> So it means to use inside another function which check business logic

## Protected
Often use in parent class. Protected member similiar with Private but can be inherit.