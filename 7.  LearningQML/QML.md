QML is a declarative programming language, we define the efect we want to achive
We do that by defining components items nested within each other.
Each of these components has a specific place in the user-visible scene and will be located exactly where we specify it.

## id
Each item in QML hierarchy may have unique `id`
Object can access other Object by this `id`
```qml
OWCTooltip {

	visible: maHistory.containsMouse
	text: qsTranslate(STR.tagTranslate,
					  STR.TOOLTIP_HISTORY)
	x: -(width / 2 - iHistory.width / 2) / pageHeader.scale
	y: -height - 5
	belowItem: true
}

MouseArea {
	id: maHistory

	anchors.fill: parent
	hoverEnabled: true
	cursorShape: Qt.PointingHandCursor
	onClicked: {
	...
	}
}
```
## Item
An Item is the most basic visual type in QML and is often used as a container for other types. 
[Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)
The root type of every of our component is an Item with the `id` _container_. 
FeatureHome.qml
```qml
Item {
    id: iFeatureHome

    signal signalShowNotification(var shouldShow)
    signal actionClicked(var model, var item)
    ...
}
```
`Item` inherit from `QtObject` which inherit from `QObject` that mean it has slots and signal

>In practice, this means that every item in QML has properties, slots and signals. The whole point of programming in QML lies precisely in defining those three things: properties, slot, signals. 

We define the position and size of an object by setting its width and height, which are properties.

### property
we can also declare our own properties using keyword property  with type and default value
```
property int leftMargin: 12
```

## Visual type (Cell Component)
Items, Rectangle ...

