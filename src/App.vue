<template>
  <div id="app">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <app-header></app-header>
    <friendly-main></friendly-main>
  </div>
</template>

<script>
import Header from './components/Shared/Header';
import FriendlyMain from './components/Pages/FriendlyMain';

export default {
  name: 'App',
  components: {
    appHeader: Header,
    friendlyMain: FriendlyMain,

  },
};
</script>

<style>
#app {
  font-family: "Merriweather", "Georgia", serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

html,body {
  background: #EFF3F4;
}
</style>
