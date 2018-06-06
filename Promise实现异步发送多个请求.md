```js
var urls = ["1.txt", "2.txt"];
Promise.all(urls.map(url =>
    fetch(url).then(resp => resp.text())
)).then(texts => {
    console.log(texts);
});
```
