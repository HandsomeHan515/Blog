```
function Switch(type) {
  this.type = type;
  this.arr = [];
}

Switch.prototype.match = function (key, callback) {
  if (key == this.type) {
    this.arr.push(key);
    if (callback) {
      callback(key);
    }
  }
  return this;
}

Switch.prototype.others = function (callback) {
  if (this.arr.indexOf(this.type) < 0) {
    if (callback) {
      callback(this.type);
    }
  }
  return this;
}

new Switch(1)
  .match(1, function (res) {
    console.log(res);
  })
  .match(2, function (res) {
    console.log(res);
  })
  .others(function (res) {
    console.log('default', res);
  })
```
