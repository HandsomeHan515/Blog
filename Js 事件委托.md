```html
<ul id="todo-app">
  <li class="item">Walk the dog</li>
  <li class="item">Pay bills</li>
  <li class="item">Make dinner</li>
  <li class="item">Code for an hour</li>
</ul>
```

```js
document.getElementById('todo-app').addEventListener('click', function(e) {
  var event = e || window.event;
  var target = event.target || event.srcElement;

  if (target.nodeName == 'LI') {
    console.log('the content is ' + target.innerHTML);
  }
})
```
