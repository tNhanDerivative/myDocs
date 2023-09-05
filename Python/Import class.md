
```python
from iot.device import Device, StatusEnum  
from iot.command import Command
  
class SmartSpeaker(Device):  
  
	def __init__(self, device_id: str) -> None:  
		self.__device_id = device_id  
		self.__status = StatusEnum.OFF

	def send_command(self, command: Command) -> None:  
		print(f"Smart Speaker {self.__device_id} handling command {command.cmd_Enum}.")
```

Khi truyền một Class Command vào hàm `send_command` thì không cần phải khai báo kiểu dữ liệu của property cmd_Enum là CommandEnum

Nhưng khi sử dụng giá trị của kiểu dữ liệu StatusEnum để gán cho `__status` thì phải khai báo

Lý giải bằng scope
