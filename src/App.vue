<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
html,body {
  margin: 0;
  padding: 0;
}
body {
  background-image: url(assets/background.jpg);
  background-attachment: fixed;
  background-repeat: no-repeat;
  background-size: cover;
  * {
    font-family: system-ui;
    text-align: center;
    box-sizing: border-box;
    transition: .25s;
    outline: none;
    text-decoration: none;
  }
}
</style>
