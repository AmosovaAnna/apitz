<template>
  <div class="home">
    <div class="description">
      Фильтровать можно, нажимая на заголовки таблицы<br>
      Настройки слева от кнопки удаления в строке таблицы<br>
      Добавление новой строки находится в конце таблицы<br>
      Заполнение добавленной строки можно сделать в настройках
    </div>
    <table class="table" v-show="json">
      <tr class="table__head">
        <td class="hover" @click="sortByUserId">userId</td>
        <td class="hover" @click="sortByTitle">title</td>
        <td class="hover" @click="sortByBody">body</td>
        <td></td>
        <td class="hover" @click="sortByReverse"><i class="fi-ss-apps-sort"></i></td>
      </tr>
      <tr v-for="item in json" v-bind:key="item.id">
        <td>{{ item.userId }}</td>
        <td>{{ item.title }}</td>
        <td>{{ item.body }}</td>
        <td class="hover"><router-link :to="'/change/' + item.id"><i class="fi-ss-settings"></i></router-link></td>
        <td class="hover" @click="json.splice(json.indexOf(item), 1)"><i class="fi-ss-trash"></i></td>
      </tr>
    </table>
    <div class="table__footer" @click="json.push({userId: '', id: json.length + 1, title: '', body: ''})">Добавить</div>
    <div v-show="!json" ref="loadOrError">загрузка...</div>
  </div>
</template>

<style scoped lang="scss">
  .home{
    backdrop-filter: blur(5px);
    width: 70%;
    margin: auto;
    background: #d9dde88f;
    min-height: 100vh;
  }
  .table {
    border-collapse: collapse;
    td {
      padding: 12px;
      a {
        color: black;
      }
    }
    tr {
      border-top: 1px solid #4b6268;
    }
    td.hover,
    td.hover a {
      color: #446f77;
    }
    td.hover:hover {
      cursor: pointer;
      background: #ececec;
    }
    &__head {
      background: #ffffff7d;
      color: #4b6268;
      font-weight: 500;
      td.hover:hover {
        cursor: pointer;
        background: #ececec;
      }
    }
    &__footer {
      padding: 20px;
      border-top: 1px solid #4b6268;
      background: #ffffff7d;
      color: #4b6268;
      font-weight: 500;
      cursor: pointer;
    }
  }
  .description {
    padding: 25px;
    background: #ece8e8bf;
    font-size: 16px;
    font-weight: 200;
  }
</style>

<script>
export default {
  name: 'Home',
  data: function () {
    return {
      result: JSON.parse(localStorage.getItem('json'))
    }
  },
  computed: {
    json: function () {
      localStorage.setItem('json', JSON.stringify(this.result))
      return this.result
    }
  },
  methods: {
    sortByUserId: function () {
      function sorting (d1, d2) {
        return (d1.userId > d2.userId) ? 1 : -1
      }
      return this.json.sort(sorting)
    },
    sortByTitle: function () {
      function sorting (d1, d2) {
        return (d1.title.toLowerCase() > d2.title.toLowerCase()) ? 1 : -1
      }
      return this.json.sort(sorting)
    },
    sortByBody: function () {
      function sorting (d1, d2) {
        return (d1.body.toLowerCase() > d2.body.toLowerCase()) ? 1 : -1
      }
      return this.json.sort(sorting)
    },
    sortByReverse: function () {
      return this.json.reverse()
    }
  },
  beforeMount: function () {
    if (localStorage.getItem('json') == null) {
      (async () => {
        await fetch('http://jsonplaceholder.typicode.com/posts')
          .then(response => response.json())
          .then(result => {
            localStorage.setItem('json', JSON.stringify(result))
          })
          .catch(error => {
            this.$refs.loadOrError.innerText = error
          })
      })()
    }
  }
}
</script>
