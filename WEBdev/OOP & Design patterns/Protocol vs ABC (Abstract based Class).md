## Interface

### What?
Interface là một class để khai báo cấu trúc. Class hỗ trợ Interface phải implement các property và method trong Interface.
Các method trong interface phải có cùng name, input, output

![](InterfaceVSAbstract.PNG )

### When?

Ví dụ dưới đây, khi mà ta muốn inject một Object vào một function thay vì tự khởi tạo một Object trong function.
Ta dùng Interface như dưới đây để đảm bảo Object đưa vào có method charge với các input đó

```python 
from typing import Protocol

from pay.card import CreditCard
from pay.order import Order


class PaymentProcessor(Protocol):
    def charge(self, card: CreditCard, amount: int) -> None:
        """Charge the card."""

def pay_order(
    order: Order, payment_processor: PaymentProcessor, card: CreditCard
) -> None:
    if order.total == 0:
        raise ValueError("Can't pay an order with total 0.")
    payment_processor.charge(card, amount=order.total)
    order.pay()
```

### Why

SOLID
Dễ test code hơn (dùng mock)
