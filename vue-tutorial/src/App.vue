<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <UserProfile />
  </div>
</template>

<script>
import UserProfile from "@/components/UserProfile";

export default {
  name: 'App',
  components: {UserProfile}
}
// so this pristapuvame do se so ni e u export default
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button{
  margin: 3px;
}
img{
  display: block;
  margin: auto;
}
</style>
