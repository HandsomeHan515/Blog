```
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script>
  const get = url => {
    return new Promise((resolve, reject) => {
      $.ajax({
        url,
        dataType: "json",
        success: res => {
          resolve(res);
        },
        error: err => {
          reject(err);
        }
      });
    });
  }

  let p1 = get('data/1.txt');
  let p2 = get('data/2.txt');

  Promise.all([p1, p2])
    .then(res => {
      const [p1, p2] = res;
      console.log(res, p1, p2);
    })
    .catch(err => {
      console.log(err);
    })

  Promise.race([p2, p1])
    .then(res => {
      console.log('d', res);
    });
</script>
```
