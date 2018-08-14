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

[1.3、组件](#1.3)

[1.4、定位元素](#1.4)

[1.5、布局元素](#1.5)

[1.6、输入元素](#1.6)

[2、动态元素](#2)

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

<h3 id=1.2>1.2基本元素</h3>

Item（基础元素对象）

Rectangle（矩形框）

Text（文本）

Image（图像）

MouseArea（鼠标区域）

<h4>基本元素对象</h4>

- Item是所有可视化元素的基础对象

| 分组 | 属性 |
|--|--|
Geometry（几何属性）| x，y定义与左上角的位置，width、height定义元素显示范围，z重叠的位置
Layout handling（布局操作）|anchors(锚定，包括左(left)、右(right)、上(top)、下(bottom)、<br>中(center)、全(fill)、水平与垂直居中(vertical center，horizontal	center))
Key	handling（按键操作）|附加属性key（按键）和keyNavigation（按键定位）属 性来控制按键操作，处理输入焦点（focus）可用操作。
Transformation（转 换）| 缩放（scale）、rotate(旋转),x、y、z属性转换（transform）,旋转基点（transformOrigin）
Visual(可视化)| 不透明度（opacity），visible（是否可见），clip（裁剪）用来限制元素边界的绘制，smooth（平滑）用来提高渲染质量
State	definition（状态定义）|states（状态列表属性）提供了元素当前所支持的状态列表

item相当于html的div元素。常用来作为其他元素的容器

<h4>矩形框元素（Rectangle Element）</h4>

基础元素的扩展，支持边界的定义，border.color（边界颜色），border.width（边界宽度）

使用radius（半径）属性来创建一个圆角矩形。

支持渐变色的实现

```qml
gradient:	Gradient	{										
GradientStop	{	position:	0.0;	color:	"lightsteelblue"	}					
GradientStop	{	position:	1.0;	color:	"slategray"	}				
}	
```

- 位置标记了y轴上的位置（0	=	顶，1	=	底）。GradientStop（倾斜点）的颜色标记了颜色 的位置。

<h4>文本元素（Text Element）</h4>

用于显示文本，有text和font（字体属性组）两个属性
- horizontalAlignment与verticalAlignment属性来设置它的对齐效果
- 使用style和styleColor属性来配置文字的外框效 果，浮雕效果或者凹陷效果
- elide属性允许你设置 文本左边，右边或者中间的省略位置
- 希望使用文字换行的方式显示所有的文本，你可以使用wrapMode属性

<h4>图像元素（image Element）</h4>

- 支持显示不同格式的图像
- source属性提供了图像文件的链接信息
- fillMode（文 件模式）属性能够控制元素对象的大小调整行为。

<h4>鼠标区域元素（MouseArea	Element）</h4>

- 非可视化元素对象
- 用于与不同元素的交互
- 捕捉鼠标事件
- 这是QtQuick中非常重要的概念，输入处理与可视化显示分开。这样你的交互区域 可以比你显示的区域大很多。

<h3 id=1.3>1.3组件</h3>

- 组件是一个可以重复使用的元素
- 创建组件有多种方法，其一是一个文件就是一个组件
- 如文件名为Button.qml，导入该文件，你的代码可以使用Button组件了

<h3 id=1.4>1.4定位元素</h3>

- Column（列）元素将它的子对象通过顶部对齐的列方式进行排列。spacing属性用 来设置每个元素之间的间隔大小。
- Row（行）元素将它的子对象从左到右，或者从右到左依次排列，排列方式取决于 layoutDirection属性。spacing属性用来设置每个元素之间的间隔大小。
- Grid（栅格）元素通过设置rows（行数）和columns（列数）将子对象排列在一个 栅格中。
- 如果没有设置其中的任意一个，会自动计算
- layoutDirection（布局方向）用来控制子元素的加入顺序。spacing属性用来控制所 有元素之间的间隔。
- Flow（流），从左到右或右到左自动布局

<h3 id=1.5>1.5布局元素</h3>

- 使用anchors（锚）对元素进行布局
- anchoring（锚定）是基础元素对象的基 本属性，可以<b>被所有的可视化</b>QML元素使用
- top顶，bottom底，left左，right右，horizontalCenter水平 中，verticalCenter垂直中
- 以及对于 horizontalCenter（水平中）与verticalCenter（垂直中）与baseline（文本基线）中 被称作偏移值

<h3 id=1.6>1.6输入元素</h3>

- 文本输入（TextInput）
- 只允许输入一行文本，不过素支持使用正则表达式验证器来限制输入和输入掩码的模式设置

- 焦点区域（FocusScope）
- 如果焦点区域接收到焦点，它的最后一个使用 focus:true的子元素接收焦点，它将会把焦点传递给最后申请焦点的子元素。

- 文本编辑（TextEdit）
- 它支持多行文本编辑。
- 它不再支持文本输入的限制，但是提供了已绘制文本的大小查询 （paintedHeight，paintedWidth）

- 按键元素（Key	Element）
- 基于某个按键的点击来执行代码
- 例如使用up，down按键来移 动一个方块，left，right按键来旋转一个元素，
- plus，minus按键来缩放一个元素

<h2 id=2>2、动态元素</h2>

<h3>动画（Animations）</h3>

- 动画用于属性的改变，一个动画定义属性值的变化的曲线，即将一个属性值变化从一值过度到另一值
- 动画控制了属性的改变，而每个元素都有大量属性任意使用
- 代码实例

```qml
import	QtQuick	2.0
Image	{			
  source:	"assets/background.png"
    Image	{				
      x:	40;	
      y:	80				
      source:	"assets/rocket.png"
						
      NumberAnimation	on	x	{					
          to:	240						
          duration:	4000				
          loops:	Animation.Infinite			
      }								
      RotationAnimation	on	rotation	{				
          to:	360								
          duration:	4000						
          loops:	Animation.Infinite	
      }			
   }
}

```

<h4>动画元素</h4>

- 基本通用
- PropertyAnimation（属性动画）-	使用属性值改变播放的动画
- NumberAnimation（数字动画）-	使用数字改变播放的动画
- ColorAnimation（颜色动画）-	使用颜色改变播放的动画
- RotationAnimation（旋转动画）-	使用旋转改变播放的动画

- 特殊情形的动画
- PauseAnimation（停止动画）-	运行暂停一个动画
- SequentialAnimation（顺序动画）-	允许动画有序播放
- ParallelAnimation（并行动画）-	允许动画同时播放
- AnchorAnimation（锚定动画）-	使用锚定改变播放的动画
- ParentAnimation（父元素动画）-	使用父对象改变播放的动画
- SmotthedAnimation（平滑动画）-	跟踪一个平滑值播放的动画
- SpringAnimation（弹簧动画）-	跟踪一个弹簧变换的值播放的动画
- PathAnimation（路径动画）-	跟踪一个元素对象的路径的动画
- Vector3dAnimation（3D容器动画）-	使用QVector3d值改变播放的动画

- 动作元素
- PropertyAction（属性动作）-	在播放动画时改变属性
- ScriptAction（脚本动作）-	在播放动画时运行脚本

<h4>应用动画（Applying Animations）</h4>

应用的方式：
- 属性动画，在元素完整加载后自动运行
- 属性动作，当属性值改变时自动运行
- 独立运行动画，使用start()函数明确指定运行或running属性被设置为true（比如通过属性绑定）
- 每个动画都有start()，stop()，resume()，restart()函数
- 另一个启动/停止一个动画的方法是绑定一个动画的running属性

<h4>缓冲曲线（Easing	Curves）</h4>

<h4>动画分组（Grouped Animations）</h4>

- 有两种分组方法：平行与连续
- SequentialAnimation(连续动画)、ParallelAnimation（平行动画）
- 平行动画例子：

```qml
//	parallelanimation.qml import
QtQuick	2.0
BrightSquare	{		
	id:	root		
	width:	300		
	height:	200		
	property int duration:	3000
	ClickableImageV3	{	
		id:rocket		
		x:20;	y:120	
		source:	"assets/rocket2.png"	
		onClicked:anim.restart()	
}
ParallelAnimation{	
	id:	anim		
	NumberAnimation	{	
		target:	rocket		
		properties:"y"	
		to:	20		
		duration:root.duration	
	}				
	NumberAnimation	{	
		target:	rocket		
		properties:"x"		
		to:160		
		duration:root.duration	
	}				
	} 
}

```

- 连续动画的例子：
