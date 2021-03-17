<template>
  <div class="change">
    <h1 class="change__head">{{ $route.params.element }}</h1>
    <label class="change__label">
      <div class="change__placeholder">userId</div>
      <input class="change__form" v-model="userId" type="text">
    </label>
    <label class="change__label">
      <div class="change__placeholder">title</div>
      <input class="change__form" v-model="title" type="text">
    </label>
    <label class="change__label">
      <div class="change__placeholder">body</div>
      <textarea class="change__form" v-model="body" type="text"></textarea>
    </label>
    <div class="change__btn-save" @click="saveChanges">Сохранить изменения</div>
    <div class="change__btn-remove" @click="removeChanges">Отменить изменения</div>
    <div class="change__btn-back"><router-link to="/">Обратно</router-link></div>
  </div>
</template>
<style scoped lang="scss">
  .change{
    backdrop-filter: blur(5px);
    width: 70%;
    margin: auto;
    background: #d9dde88f;
    display: flex;
    flex-direction: column;
    padding: 20px;
    min-height: 100vh;
    &__head {
      margin: 0;
      padding: 10px;
      font-size: 44px;
      color: #446f77;
    }
    &__placeholder {
      text-align: left;
      font-size: 15px;
      color: #dce5e4;
      text-shadow: 0 0 3px #3e565a;
    }
    &__form {
      width: 100%;
      background: #f9fdff4a;
      border: none;
      padding: 15px;
      font-size: 18px;
      text-align: left;
    }
    &__btn-save,
    &__btn-remove,
    &__btn-back {
      padding: 10px;
      background: #3f5b5f;
      color: white;
      cursor: pointer;
      margin: 10px 0;
      a {
        color: white;
      }
    }
    textarea {
      resize: vertical;
      min-height: 200px;
    }
  }
</style>
<script>

export default {
  name: 'Change',
  data: function () {
    return {
      result: JSON.parse(localStorage.getItem('json')),
      userId: JSON.parse(localStorage.getItem('json'))[this.$route.params.element - 1].userId,
      title: JSON.parse(localStorage.getItem('json'))[this.$route.params.element - 1].title,
      body: JSON.parse(localStorage.getItem('json'))[this.$route.params.element - 1].body
    }
  },
  methods: {
    saveChanges: function () {
      for (const key in this.result[this.$route.params.element - 1]) {
        if (key !== 'id') {
          this.result[this.$route.params.element - 1][key] = this[key]
          localStorage.setItem('json', JSON.stringify(this.result))
        }
      }
    },
    removeChanges: function () {
      for (const key in this.result[this.$route.params.element - 1]) {
        if (key !== 'id') {
          this[key] = this.result[this.$route.params.element - 1][key]
        }
      }
    }
  }
}
</script>
