<template>
  <div id="app">
    <main-header></main-header>
    <router-view/>
  </div>
</template>

<script>
import Header from '@/components/Header.vue';

export default {
  name: 'app',

  components: {
    'main-header': Header,
  },
};
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #2c3e50;
}

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
