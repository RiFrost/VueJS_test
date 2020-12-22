<template>
  <div id="app">
    <nav>
      <div class="navigation__logo">
        Twotter
      </div>
      <div class="navigation__user">
        {{ user.username }}
      </div>
    </nav>
    <UserProfile/>
  </div>
</template>

<script>
import UserProfile from './components/UserProfile'

export default {
  name: 'App',
  components: { UserProfile },
  data() {
    return {
      user: {
        username: 'MusterMax'
      }
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
}
</style>
