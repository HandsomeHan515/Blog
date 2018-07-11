```code
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>

<body>
  <table id="app">
    <tr v-for="(run, index) in settings.runs">
      <td>
        <input type="text" :name="'run'+index" v-model="settings.runs[index]" />
      </td>
      <td>
        <button @click.prevent="removeRun(index)">X</button>
      </td>
      <td>
        {{run}}
      </td>
    </tr>
  </table>
  <script>
    new Vue({
      el: '#app',
      data: {
        settings: {
          runs: [1, 2, 3]
        }
      },
      methods: {
        removeRun: function (i) {
          console.log("Remove", i);
          this.settings.runs.splice(i, 1);
        }
      }
    });
  </script>
</body>

</html>
```
