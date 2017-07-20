### 开始之前

```
<canvas id='canvas'>你的浏览器不支持Canvas</ canvas>
```
> 获取canvas容器(JavaScript中的方式)


```
var cvs = documents.getElementById('canvas')
```
> 创建画布(JavaScript中的方式)


```
var ctx = cvs.getContent('2d')
```

---

### 在React中创建画布


```
<canvas ref='canvas'>你的浏览器不支持Canvas</ canvas>

componentDidMount() {
    this.cvs = findDOMNode(this.refs.canvas) 
    this.ctx = this.cvs.getContext('2d')
}
```

---

### Canvas中常用的属性和方法

#### 绘制路径

```
//新建路径,begainPath()是绘制新图形的开始
ctx.beginPath()
//路径的起点
ctx.moveTo()
//线的终点
ctx.lineTo()
//闭合路径,不是必须的，如果线的终点和起点一样，会自动闭合
ctx.closePath()
//通过线条绘制轮廓(边框)
ctx.stroke()
//通过路径填充区域(实心)
ctx.fill()
```
> fill()和stroke()表示绘图结束，如果要继续绘制，需要重新新建路径beginPath()；如果lineTo()最后路径没有封闭，fill()函数会自动封闭路径，但是stroke()则不会
#### 绘制矩形
---
```
//绘制矩形
rect(x, y, width, height) 
//填充矩形(x, y是横纵坐标，原点在Canvas的左上角)
ctx.fillRect(x, y, width, height)
//边框矩形(默认1px,黑色h)
ctx.strokeRect(x, y, width, height)
//清除制定的矩形，变成透明
ctx.clearRect(x, y, width, height)
```
---
#### 绘制弧线(圆形)

```
//x, y圆心,r半径，start和end是开始和结束角，false表示顺时针(默认)，true表示逆时针
ctx.arc(x, y, r, start, end, true/false)
//根据给定的点画圆弧，再以直线链接两个点
ctx.arcTo( x1, y1, x2, y2, radius) 
```
---
#### 样式
##### 颜色

```
ctx.fillStyle = 'red' 
ctx.strokeStyle = 'red'//
ctx.globalAlpha = 0.5 //透明度
```
---
##### 线段端点连接处


```
ctx.lineWidth = 4
//设置和返回线条结束时端点样式round/butt(默认)/square
ctx.lineCap = 'round'
//两条线相交时，所创建的拐角类型bevel/round/miter(默认)
ctx.lineJoin = 'round'
```
---
##### 文字


```
ctx.font = '20px 宋体'
ctx.textAlign = 'center'
ctx.textBaseline = 'middle'
ctx.direction = 'inherit'
```
---
### 其他的一些常用属性
```
clip()  从原始画布剪切任意形状和尺寸的区域

quadraticCurveTo()  创建二次贝塞尔曲线

bezierCurveTo()   创建三次贝塞尔曲线

isPointInPath( x, y )  检测指定的点是否在当前路径中，在则返回true。

shadowColor  设置或返回用于阴影的颜色

shadowBlur   设置或返回用于阴影的模糊级别

shadowOffsetX  设置或返回阴影与形状的水平距离

shadowOffsetY  设置或返回阴影与形状的垂直距离

miterLimit  设置或返回最大斜接长度

createLinearGradient( x0, y0, x1, y1 )  创建线性渐变

createPattern( image/canvas/video, repeat )  在指定的方向内重复绘制指定的元素

createRadialGradient( x0, y0, r0, x1, y1, r1 ) 创建径向渐变

addColorStop( stop, color )  规定渐变对象中的颜色和停止位置

measureText( text )  返回包含指定文本宽度的对象（属性width获取宽度）

drawImage( image/canvas, x, y )、drawImage( image/canvas, x, y, width, height )、drawImage( image/canvas, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight )  在画布上绘制图像、画布或视频

createImageData( width, height )、createImageData(imageData)  绘制ImageData对象

getImageData( x, y, width, height )  返回ImageData对象，该对象为画布上指定的矩形复制像素数据。

putImageData( ImageData, x, y )、putImageData( imageData, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight )  把图像数据放回画布上。

width  返回ImageData对象的宽度

height  返回ImageData对象的高度

data  返回一个对象，包含指定的ImageData对象的图像数据

globalCompositeOperation  设置或返回新图像如何绘制到已有的图像上。

scale( x, y )  缩放当前绘图

translate( x, y )  重新设置画布上的(0,0)位置

rotate( angle )  选择当前绘图，单位为“弧度”，角度转弧度公式（ degrees*Math.PI/180）

transform( m11, m12, m21, m22, dx, dy )  替换绘图的当前转换矩阵

setTransform()  将当前转换重置为单元矩阵，然后运行transform()

save()  保存当前环境的状态

restore()  恢复之前保存过的路径状态和属性

getContext('2d')  获取2d对象

toDataURL()  将canvas转换成图片，返回地址
```
