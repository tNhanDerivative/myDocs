
[Youtube video](https://www.youtube.com/watch?v=IodTf-Yw4yc&t=368s)
# Ideas
Ta muốn tách operation, algorithm thành 1 class riêng tách ra khỏi cả concrete class và parent class (class hierarchy)
Ví dụ: Ta có một class hierarchy cho Shape
	- Để Render ta có thể sử dụng Vulcan hoặc OpenGL tùy Client App
	- Để Serialize ta có thể dùng Json hoặc XML
> Serialize: là quá trình chuyển đổi một object thành một chuỗi byte để nó có thể được lưu trữ trong disk hoặc truyền qua network
De-serialize: sử dụng chuỗi byte để reconstruct cái object ban đầu.

Nếu đưa Render và Serialize vào class Shape
	- Shape sẽ phải biết (dependent) vào Vulcan, OpenGL để Render
	- Operation ảnh hưởng đến signature của class Shape như có 2 render method dù 1 cái ko dùng

![[Shape-render.svg]]

```cpp
class Shape{
public:
	virtual ~Shape() = default;
	virtual float Size() const =0;
	virtual float Area() const =0;
	virtual void Accept(ShapeVisitor* sh)
}

class ShapeVisitor{
public:
	virtual ~ShapeVisitor() = default;
	virtual void Visit(Circle* c) =0;
	virtual void Visit(Square* s) =0;
}

class VulcanShapeVisitor: public ShapeVisitor{
public:
	virtual void Visit(Circle* c) override;
	virtual void Visit(Square* s) override;
	
}
```

Shape chỉ biết (dependent) vào `ShapeVisitor`
`ShapeVisitor` và children class biết (dependent) tất cả các `Shape` và children của nó
Visitor sẽ sử dụng type của Shape để function overload để dispatch đúng method 

