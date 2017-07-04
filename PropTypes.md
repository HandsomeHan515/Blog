# React => PropTypes
## 声明prop为指定的JS基本类型
> 默认情况下是可传可不穿的

```
optionalArray: React.PropTypes.array
optionalBool: React.PropTypes.bool
optionalFunc: React.PropTypes.func
optionalNum: React.PropTypes.number
optionalObject: React.PropTypes.object
optionalString: React.PropTypes.string

```
> 所有可被渲染的对象：数字、字符串、DOM元素或者包含这些类型的数组

``` 
optionalNode: React.PropTypes.node
```
> React元素
```
optionalElement: React.PropTypes.element
```
> 用JS的instanceof操作符声明prop为类的实例
```
optionalMessage: React.PropTypes.instanceof(Message)
```
> 用enum来限制prop只能接受指定的值
``` 
optionalEnum: React.PropTypes.oneOf(['NEWS, Photos'])
```
> 指定多个对象类型中的一个
```
optionalUnion: React.PropTyepes.oneOfType([
    React.PropTypes.string,
    React.PropTypes.number,
    React.PropTypes.instanceOf(Message)
])
```
> 指定类型组成的数组
```
optionalArrayOf: React.PropTypes.arrayOf(React.PropTypes.number)
```
> 指定类型的属性组成的对象
```
optionalObjectOf: React.PropTypes.objectOf(React.PropTypes.number)
```

**任意类型加上isRequired使prop不为空**
**不可空的任意类型**
```
requiredAny: React.PropTypes.any.isRequired
```
> 特定形状参数的对象
```
optionalObjectWithShape: React.PropTypes.shape({
    color: React.PropTypes.string,
    fontSize: React.PropTypes.number
})
```