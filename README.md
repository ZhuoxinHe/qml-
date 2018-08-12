# qml-笔记
## 初始qt5
Qt Quick是Qt5界面开发技术的统称，是以下几种技术的集合：

QML - 界面标记语言

JavaScript - 动态脚本语言

Qt C++ - 跨平台C++封装库

<h1>目录</h1>

[1、qml快速入门](#1)

[1.1、语法](#1.1)

[1.2、基本元素](#1.2)

<h2 id=1>qml快速入门<h2>

<h3 id=1.1>语法</h3>

qml是一种描述用户界面的声明式语言
```qml
  // rectangle.qml
import QtQuick 2.0
// The root element is the Rectangle
Rectangle {
  // name this element root
  id: root
  // properties: <name>: <value>
  width: 120; height: 240
  // color property
  color: "#D8D8D8"
  // Declare a nested element (child of root)
Image {
  id: rocket
  // reference the parent
  x: (parent.width - width)/2; y: 40
  source: 'assets/rocket.png'
}
// Another child of root
Text {
  // un-named element
  // reference element by id
  y: rocket.y + rocket.height + 20
  // reference root element
  width: root.width
  horizontalAlignment: Text.AlignHCenter
  text: 'Rocket'
}
}
```
- import声明用于导入指定的模块版本，一般导入QtQuick2.0作为初始元素引用。
- //为单行注释，/**/为多行注释
- 每个QML文件必须有一个根元素，这里是Rectangle
- 每个元素使用他的类型声明然后用{}进行内容包含
- 元素可以拥有属性，赋值格式为name:value
- 任何在QML的元素都可以使用它们的id进行访问
- 元素可以嵌套
<h4>属性</h4>
  代码例子：

```qml

Text {
  // (1) identifier
  id: thisLabel
  // (2) set x- and y-position
  x: 24; y: 16
  // (3) bind height to 2 * width
  height: 2 * width
  // (4) custom property
  property int times: 24
  // (5) property alias
  property alias anotherTimes: thisLabel.times
  // (6) set text appended by value
  text: "Greetings " + times
  // (7) font is a grouped property
  font.family: "Ubuntu"
  font.pixelSize: 24
  // (8) KeyNavigation is an attached property
  KeyNavigation.tab: otherLabel
  // (9) signal handler for property changes
  onHeightChanged: console.log('height:', height)
  // focus is neeed to receive key events
  focus: true
  // change color based on focus value
  color: focus?"red":"black"
}

```
  属性的特点：
- id是一个特殊的属性值，用来引用元素。它不是一个字符串，而是一个标识符，且一个id在一个QML文档是唯一的，不能设为以其他值和无法查询。
- 一个属性可以设置一个值，这个只依赖于它的类型，如果没有对属性赋值将初始化一个默认值，该默认值可以查看特定的元素文档获得。
- 一个元素可以依赖一个或多个其他属性，这被称为元素的属性绑定。依赖的值改变时，它的值也会改变。
- 如果需要添加自定义的属性需要使用property修饰符，跟上类型，名字和可选择的初始化值。如果没有初始值，会给定一个系统初始值
- alias关键字--另一种声明属性的方法，允许转发一个属性或者一个属性对象到另个作用域。
- text属性依赖于timers属性，在text里，int整型数据会自动转为string数据，文本会根据timers属性的每次刷新而改变。
- 按组分配的属性：font{family:"Ubuntu"; pixelSize: 24 }
- 元素自身附加属性，如键盘输入
- 对于每个元素你都可以提供一个信号操作

<h4>脚本</h4>
  QML与javascript兼容
  
  ```qml
Text {
  id: label
  x: 24; y: 24
  // custom counter property for space presses
  property int spacePresses: 0
  text: "Space pressed: " + spacePresses + " times"
  // (1) handler for text changes
  onTextChanged: console.log("text changed to:", text)
  // need focus to receive key events
  focus: true
  // (2) handler with some JS
  Keys.onSpacePressed: {
    increment()
   }
   // clear the text on escape
  Keys.onEscapePressed: {
    label.text = ''
  }
  // (3) a JS function
  function increment() {
    spacePresses = spacePresses + 1
  }
}
```
注意事项：
- QML的：（属性绑定）与JavaScript的=（赋值）是不同的。
- 定是一个协议，并且存在于整个生命周期。然而JavaScript赋值（=）只会产生一次效果。
- 一个新的绑定生效或者使用JavaScript赋值给属性时，绑定的生命周期就会结束。

<h3 id=1.2>基本元素</h3>

Item（基础元素对象）

Rectangle（矩形框）

Text（文本）

Image（图像）

MouseArea（鼠标区域）

<h4>基本元素对象</h4>

- Item是所有可视化元素的基础对象

| 分组 | 属性 |
|--|--|

