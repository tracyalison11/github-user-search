<template>
  <div id="app">
    <UserSearch/>
  </div>
</template>

<script>
import UserSearch from './components/UserSearch.vue'

export default {
  name: 'app',
  components: {
    UserSearch
  }
}
</script>

<style>
  @import './styles/styles.css';
</style>
