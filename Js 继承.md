```
function Dog(name) {
  this.name = name;
  this.species = '犬科';
}

var dogA = new Dog('wu');
console.log(dogA.name);
var dogB = new Dog('xu');
dogA.species = '猫科';
console.log(dogB.species);
function Dog(name) {
  this.name = name;
}

Dog.prototype.species = '犬科';

var dogA = new Dog('wu');
var dogB = new Dog('xu');

console.log(dogB.species, dogA.species);

// 构造函数绑定
function Animal() {
  this.species = '动物';
}

function Cat(name, color) {
  Animal.apply(this, arguments);
  this.name = name;
  this.color = color;
}

var cat = new Cat('wu', 'yellow');
console.log(cat.species);

// prototype 模式
function Animal() {
  this.species = '动物';
}

function Cat(name, color) {
  this.name = name;
  this.color = color;
}

Cat.prototype = new Animal();

Cat.prototype.constructor = Cat;

var cat1 = new Cat('xu', 'red');
console.log(cat1.species);

// 对象的继承(object)
function object(o) {
  function F() {}
  F.prototype = o;
  return new F();
}	

// 对象的继承(浅拷贝)	
function extendCopy(p) {
  var c = {}
  for (var i in p) {
    c[i] = p[i]
  }
  c.uber = p;
  return c;
}

//深拷贝
function deepCopy(p, c) {
  var c = c || {};
  for (var i in p) {
    if (typeof p[i] === 'object') {
      c[i] = (p[i].constructor === Array) ? [] : {};
      deepCopy(p[i], c[i]);
    } else {
       c[i] = p[i];
    }
  }
  return c;
}
```
