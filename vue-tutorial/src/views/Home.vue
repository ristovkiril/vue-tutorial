<template>
  <div class="home">
    <h1>Hello World</h1>
    <div>
      <router-link v-for="user in users" :to="{name: 'UserProfile', params: {userId: user.id}}" :key="user.id">
        {{user.username}}<br>
      </router-link>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import {users} from '@/assets/users';

export default {
  name: 'Home',
  data() {
    return {
      users: users
    }
  }
}
</script>
