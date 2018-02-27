```
function Target(type) {
  this.type = type;
  this.arr = [];
}

Target.prototype = {
  match: function(type, callback) {
    this.arr.push(type);

    this.target = type;
    if (this.type === this.target) {
        callback(this.target);
    }
    return this;
},

others: function(target) {
 // console.log('arr: %o', this.arr, this.type, this.arr.indexOf(this.type));
  if (this.arr.indexOf(this.type) == -1) {
      target();
  }
  return null;
}
}

function target(type) {
  return new Target(type);
}


target(1)
.match(1, console.log)
.match(2, console.log)
.match(4, console.log)
.others(() => { 
  console.log(11);
});
```
