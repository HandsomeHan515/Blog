```
let timer = (callback) => {
  setTimeout(() => {
    if (callback) {
      callback();
    }
    return timer(callback);
  }, 1000);
}

timer(function () {
  console.log(123);
})
```
