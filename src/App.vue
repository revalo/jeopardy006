<template>
  <div id="app">
    <div class="nav">
      <router-link to="/">Home</router-link>
      <router-link to="/editor">Editor</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
body {
  font-family: 'Roboto';
}
</style>
