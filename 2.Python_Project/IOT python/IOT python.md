
https://www.youtube.com/watch?v=xvb5hGLoK0A&list=PLC0nd42SBTaNuP4iB4L6SJlMaHE71FG6N&index=32

## Enum

[[Data types]]

## Sensor data object

```python
import random
import json

class Stack(object):
    __slots__ = ["data"]
    def __init__(self):
        self.data = []


class Sensor(object):
    """  Data Layers  """
    def __init__(self):
        self.stack = Stack()

    def getTemperature(self):
        return random.randint(0,77)

    def getHumidity(self):
        return  random.randint(0,99)

    def getPayload(self):
        """ Return Object and not the Data  """
        self._Payload = {}
        self._Payload["Temperature"]  =self.getTemperature()
        self._Payload["humidity"] = self.getHumidity()
        self.stack.data.append(self._Payload)
        return self.stack


class BusinessLogic(object):
    def __init__(self):
        self.sensor = Sensor()
        self._obj = self.sensor.getPayload().data.pop()
    def get_data(self):
        """ Business Logic Goes Here  """
        if self._obj["Temperature"] >0:
            return self._obj
        else:
            return None


class UI(object):
    def __init__(self):
        self.bi = BusinessLogic()
    def getTemperature(self):
        data = self.bi.get_data()
        return data["Temperature"]
    def getHumidity(self):
        data = self.bi.get_data()
        return data["humidity"]


if __name__ == "__main__":
    obj = UI()
    tem = obj.getTemperature()
    print(tem)
```
### Slot

Slot: https://www.geeksforgeeks.org/python-use-of-__slots__/
Slot video: https://www.youtube.com/watch?v=Iwf17zsDAnY&t=221s

Stack.data: list
`self.sensor.getPayload().data.pop()` : https://www.programiz.com/python-programming/methods/list/pop




