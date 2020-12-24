<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">
        Admin {{userId}}
      </div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>

      <CreateTwoot @postTwoot="postTwoot" />

    </div>

  </div>
  <div class="user-profile__twoots-wrapper">
    <TwootItem v-for="twoot in user.twoots"
               :key="twoot.id"
               :username="user.username"
               :twoot="twoot"
               @favourite="toggleFavorite"/>
  </div>
</template>

<script>
import TwootItem from "@/components/TwootItem";
import CreateTwoot from "@/components/CreateTwoot";
import { useRoute } from "vue-router";
import {users} from '@/assets/users'

export default {
  name: "UserProfile",
  components: {CreateTwoot, TwootItem},
  data() {
    const router = useRoute();
    return {
      followers: 0,
      userId: router.params.userId,
      user: {}
    }
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount){
      //Do here main logic
      if (oldFollowerCount < newFollowerCount){
        console.log(`${this.user.username} has gained a follower!`)
      } else {
        console.log(`${this.user.username} is unfollowed`)
      }
    },
    newTwootContent(newContent, oldContent){
      if (newContent.length > 180){
        this.newTwootContent = oldContent.substring(0, 180);
      }
    }
  },
  methods: {
    followUser() {
      this.followers++
    },
    unFollowUser() {
      if (this.followers > 0)
        this.followers--
    },
    toggleFavorite(id){
      alert(`Favorite Tweet ${id}`)
    },
    postTwoot(newTwootContent) {
      if (newTwootContent){
       this.user.twoots.unshift(
           {
             id: this.user.twoots.length + 1,
             content: newTwootContent,
             date: new Date()
           });
        this.newTwootContent = "";
        users[this.user.id].twoots = this.user.twoots;
      }
    }
  },
  mounted() {
    this.followUser();
    const router = useRoute();
    const userId = router.params.userId;
    this.userId = userId;
    this.user = users[userId-1] || users[0];

  }
}
</script>

<style lang="scss" scoped>
  .user-profile{
    display: grid;
    grid-template-columns: 1fr 3fr;
    grid-gap: 50px;
    padding: 50px 5%;

    .user-profile__user-panel {
      display: flex;
      flex-direction: column;
      padding: 20px;
      background-color: white;
      border-radius: 5px;
      border: 1px solid #DFE3E8;

      h1 {
        margin: 0;
      }
      .user-profile__admin-badge {
        background: rebeccapurple;
        color: white;
        border-radius: 5px;
        margin-right: auto;
        padding: 0 10px;
        font-weight: bold;
      }



    }

    .user-profile__twoots-wrapper {
      display: grid;
      grid-gap: 10px;
    }

  }

</style>