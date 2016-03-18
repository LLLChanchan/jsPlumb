# jsPlumb 

---

## 一、默认属性

- `Anchor`：锚点(连接点位置)，可以设置在任何没有锚点的目标上(`endPoint`)
- `Anchors`：设置在connect的源和目标点的连接点位置，默认是 `BottomCenter`
- `Connector`：连接线（比如：`["Bezier", {curviness: 63}]`为贝塞尔曲线）
- `ConnectionsDetachable`：是否在连接点可以用鼠标拖动[`true/false`]
- `Container`：容器
- `DoNotThrowErrors`：设置当锚点(`Anchor`)、端点(`endPoint`)和连接器(`Connector`)不存在的时候是否抛出异常
- `ConnectionOverlays`：默认覆盖附着在每个连接器
- `DragOptions`：为 被 `jsPlumb.draggable` 设置了拖拽的元素拖拽时设置的css样式.eg:
		hoverClass: "dropHover",//释放时指定鼠标停留在该元素上使用的css class;
		activeClass: "dragActive"//可拖动到的元素使用的css class
- `Endpoint`: 端点的形状定义，比如圆：`[ "Dot", { radius:5 } ]`；正方形：`Rectangle`
- `Endpoints`：设置了连接器的源和目标端点的形状，eg圆： `[ [ "Dot", { radius:7 } ], [ "Dot", { radius:11 } ] ]`
- `EndpointOverlays`:默认覆盖附着在每个端点
- `EndpointStyle`：端点的默认样式
- `EndpointStyles`：设置了连接器的源和目标端点的样式
- `EndpointHoverStyle`：端点的hover状态样式
- `EndpointHoverStyles` ：设置了连接器的源和目标端点端点的hover状态样式
- `HoverPaintStyle` ：
- `LogEnabled`：jsPlumb内部日志是否开启。
- `Overlays`：默认覆盖连接器和端点样式，装饰连接器,如标签、箭头等
- `MaxConnections` ：设置连接点最多可以连接几条线
- `PaintStyle` ：设置连接点的样式
- `connectorStyle`：设置连接线样式
- `ReattachConnections` ：
- `RenderMode` ：默认渲染模式
- `Scope`：连接点的标识符，只有标识符相同的连接点才能连接

## 二、锚点(Anchor)

### 1 . 默认锚位置:

- Top (TopCenter)
- TopRight
- Right (RightMiddle)
- BottomRight
- Bottom (BottomCenter)
- BottomLeft
- Left (LeftMiddle)
- TopLeft
- Center

eg：

	//定义了一个在底部中间的锚点位置
	jsPlumb.connect({...., anchor:"Bottom", ... });

### 2 . 基于数组的语法 [x,y,dx,dy] 

- x-相对该锚点在x轴坐标比例（最大1）
- y-相对该锚点y轴坐标比例（最大1）
- dx-控制锚的方向
- dy-同上

eg：

	//定义了一个在底部中间的锚点位置
	jsPlumb.connect({...., anchor:[ 0.5, 1, 0, 1 ], ... });

如果锚点位置无法满足你的需求，还可以设置锚点的偏移量[x,y,dx,dy,offSetX,offSetY] ,下面设置了Y轴偏移50px，锚点Y坐标会+50px:

	jsPlumb.connect({...., anchor:[ 0.5, 1, 0, 1, 0, 50 ], ... }); 

### 3 . 动态锚

- 数组定义

	没有特殊的语法来创建一个动态锚;可以提供一个静态数组锚规格,如:
	
		var dynamicAnchors = [ [ 0.2, 0, 0, -1 ],  [ 1, 0.2, 1, 0 ], [ 0.8, 1, 0, 1 ], [ 0, 0.8, -1, 0 ] ];
		jsPlumb.connect({...., anchor:dynamicAnchors, ... });
	
	或者组合：
	
		var dynamicAnchors = [ [ 0.2, 0, 0, -1 ],  [ 1, 0.2, 1, 0 ],  "Top", "Bottom" ];
		jsPlumb.connect({...., anchor:dynamicAnchors, ... });
	
	这样锚点会根据位置自动调整到最合适的位置(定义的数组里几个点中)

- 默认定义

	jsPlumb提供了一个动态锚 `AutoDefault` 选择从 前 , 右 , 底 和 左 :
	
		jsPlumb.connect({...., anchor:"AutoDefault", ... });

### 4 . 多边形锚

- Circle(圆)
- Ellipse(椭圆)
- Triangle(三角形)
- Diamond(菱形)
- Rectangle(矩形)
- Square(正方形)

#### (1) 单个多边形

eg：

	jsPlumb.addEndpoint("someElement", {
		endpoint:"Dot",
		anchor:[ "Perimeter", { shape:"Circle" } ]
	});

如果锚点的宽高一样，该锚点位置为动态圆周。宽高不同为椭圆，类似正方形和矩形。

默认情况下，锚点个数为60，我们还可以手动指定：

eg（指定150个动态锚点）：

	jsPlumb.addEndpoint("someDiv", {
	    endpoint:"Dot",
	    anchor:[ "Perimeter", { shape:"Square", anchorCount:150 }]
	});

#### (2) 组合锚点（三角形与菱形）：

	jsPlumb.connect({
	    source:"someDiv",
	    target:"someOtherDiv",
	    endpoint:"Dot",
	    anchors:[
	        [ "Perimeter", { shape:"Triangle" } ],
	        [ "Perimeter", { shape:"Diamond" } ]
	    ]
	});

#### (3) 自定义角度多边形锚点

	jsPlumb.connect({
	    source:"someDiv",
	    target:"someOtherDiv",
	    endpoint:"Dot",
	    anchors:[
	        [ "Perimeter", { shape:"Triangle", rotation:25 } ],
	        [ "Perimeter", { shape:"Triangle", rotation:-335 } ]
	    ]
	});  

上面定义了两个三角形旋转不同角度得到的组合图形（旋转适用带角度的多边形）。

### 5. CSS类和锚点

#### (1)介绍

锚点的不同位置可以有多种css样式，那就要有不同的css类提供支持。

被写入到锚点的CSS类和元素与jsPlumb实例相关联的前缀默认的前缀:

	jsplumb-endpoint-anchor-

eg：

	var ep = jsPlumb.addEndpoint("someDiv", {
	  anchor:[0.5, 0, 0, -1, 0, 0, "top" ]
	};

jsPlumb将会分配这个类给创建的 `endpoint` 和元素 `someDiv`：

	jsplumb-endpoint-anchor-top


#### (2)示例

一个使用动态锚的例子:

	var ep = jsPlumb.addEndpoint("someDiv", {
	  anchor:[
	    [ 0.5, 0, 0, -1, 0, 0, "top" ],
	    [ 1, 0.5, 1, 0, 0, 0, "right" ]
	    [ 0.5, 1, 0, 1, 0, 0, "bottom" ]
	    [ 0, 0.5, -1, 0, 0, 0, "left" ]
	  ]
	});

这里的类分配给端点和元素循环这些值作为锚位置的变化:

	jsplumb-endpoint-anchor-top
	jsplumb-endpoint-anchor-right
	jsplumb-endpoint-anchor-left
	jsplumb-endpoint-anchor-bottom

如果您提供多个类名,jsPlumb不会预先考虑类中的每个词的前缀:

	var ep = jsPlumb.addEndpoint("someDiv", {
	  anchor:[ 0.5, 0, 0, -1, 0, 0, "foo bar" ]
	});

会导致2个类被添加到端点和元素:

	jsplumb-endpoint-anchor-foo 和 bar

#### (3)改变锚类前缀

前缀 `endpointAnchorClass` 用于锚类存储为jsPlumb的成员，这个前缀是可更改的:

	jsPlumb.endpointAnchorClass = "anchor_";

或者
	
	var jp = jsPlumb.getInstance();
	jp.endpointAnchorClass = "anchor_";

## 三、连接线(器)(Connectors)

### 1. 简介
jsPlumb提供了四种连接线：

- `straight`(直线)
- `Bezier`(贝塞尔曲线)
- `flowchart`(流程图)
- `state machine`

### straight

在两个端点之间画一条直线。 它支持两个构造函数参数:

- `stub`：可选的,默认值为0。此参数的任何正值将导致在与连接线的两端产生一段不可改变方向的线段
- `gap`：可选，默认为0像素。在连接线的一端和连接的元素之间指定一个间隙。

### Bezier

贝塞尔提供了一个立方的贝塞尔曲线。 它支持一个构造函数参数:

- `curviness`：参数可选,默认为150。 定义了曲线的弯曲程度。

### flowchart 

垂直或水平的连接线，提供了四个参数：

- `stub`：这是最小长度，以像素为单位，最初的存根，源自一个端点。这是一个可选的参数，并且可以是一个整数，它指定了连接器的每个末端的存根，或是一个整数数组，指定[源目标]端点的连接。默认值为30像素的整数 
- `alwaysRespectStubs` ：可选，默认为false。
- `gap`：可选，默认为0像素。在连接线的一端和连接的元素之间指定一个间隙。
- `midpoint`：可选，默认为0.5。这是一个流程图中最长的部分将被绘制的2个元素之间的距离。
- `cornerRadius`：默认为0。此参数的正值将改变弯角的度数。

### state machine

略微弯曲的线（实际上是二次Bezier曲线），类似于状态机的连接器，支持的构造函数参数：

- `margin`：可选；默认为5。定义连接线开始/结束的元素的距离。
- `curviness`：可选的,默认为10，定义了曲线的弯曲程度。
- `proximityLimit` ： 可选,默认为80。 连接线的两端之间的最小距离 它描绘为一条直线而非二次贝塞尔曲线。

## 四、端点(Endpoints)

### 简介

端点是连接里的一个端点外观和行为表现的集合，jsPlumb实现了四个端点：

- Dot(点)
- Rectangle(矩形)
- Blank(空)
- image(图像)

### 创建

有不同的方式创建 `endpoint`：

#### (1)connect

并通过一个元素id或DOM元素作为源/目标,创建并分配一个新的端点

eg：

	jpInstance.connect({
        source: "state1",
        target: "state2",
        scope: "state3"
    });

#### (2)addEndpoint
创建一个新的端点
	
	jpInstance.addEndpoint("myDivId", EndpointConfig)

#### (3)makeSource()

	jpInstance.makeSource(...)

### 类型

#### (1)Dot
就是在屏幕上画一个点，它支持三个构造函数参数:

- `radius`：可选，默认为10像素。 定义点的半径
- `cssClass` ：可选，端点元素的CSS类。 
- `hoverClass` 可选的，元素或连线的hover属性样式类

#### (2)Rectangle

绘制一个矩形。 支持构造函数参数有:

- `width`：可选的，默认为20像素。定义矩形的宽度。
- `height`：可选的，默认为20像素。定义矩形的高。
- `cssClass` ：可选的，端点元素的CSS类。
- `hoverClass` ：可选的，元素或连线的hover属性样式类

#### (3)Image

从一个指定的URL加载图像，这个端点支持三种构造函数参数:

- `src`：图片的url
- `cssClass` ：可选的，端点元素的CSS类。
- `hoverClass` ：可选的，元素或连线的hover属性样式类

## 五、覆盖(Overlays)

### 简介

jsPlumb带有五个类型的覆盖图:
- Arrow(箭头) ：一个可配置的箭头，在某些点上涂上了一个可配置的箭头。你可以控制箭头的长度和宽度，“折返”点一点尾巴分折回来，和方向（允许值为1和1；1是默认的，意味着在连接点方向）
- Label(标签)：
- PlainArrow(平原箭头)：
- Diamond(钻石)：
- Custom(自定义)：
