```
let setInterval = (callback, time) => {
  setTimeout(() => {
    if (callback) {
      callback();
    }
    return setInterval(callback, time);
  }, time);
}

setInterval(function () {
  console.log(123);
}, 2000)
```
