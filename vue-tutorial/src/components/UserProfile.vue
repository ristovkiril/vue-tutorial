<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">
        Admin
      </div>
      <div class="user-profile__follower-count">
        <strong>Followers: </strong> {{ followers }}
      </div>
        <form :class="{'.dangerError': newTwootCharacterCount > 180}" class="user-profile__create-twoot" @submit.prevent="postTwoot">
          <label class="" for="newTwoot"><strong>New Twoot</strong> ({{newTwootCharacterCount}}/180)</label>
          <textarea id="newTwoot" rows="4"  v-model="newTwootContent"></textarea>


          <div class="user-profile__create-twoot-type">
            <label for="newTwootType"><strong>Type: </strong></label>
            <select id="newTwootType" v-model="selectedTwootType">
              <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                {{ option.name }}
              </option>
            </select>
          </div>

          <button>Post</button>

        </form>
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

export default {
  name: "UserProfile",
  components: {TwootItem},
  data() {
    return {
      newTwootContent: '',
      selectedTwootType: 'instant',
      twootTypes: [
        { value: 'draft', name: "Draft"},
        { value: 'instant', name: "Instant Twoot"}
      ],
      followers: 0,
      user: {
        id: 1,
        username: "ristov.kiril",
        firstName: "Kiril",
        lastName: "Ristov",
        email: "ristov.kiril@yahoo.com",
        isAdmin: true,
        twoots: [
          { id: 1,  content: "Twooter is Amizing!", date: new Date() },
          { id: 2,  content: "Dont forget to Subscribe!", date: new Date() },
          { id: 3, content: "How are you today?", date: new Date() },
          { id: 4, content: "Happy new Year", date: new Date() },
          { id: 6, content: "2021 sucks!", date: new Date() },
          { id: 7, content: "2021 sucks!", date: new Date() },
          { id: 8, content: "2021 sucks!", date: new Date() },
          { id: 9, content: "2021 sucks!", date: new Date() },
          { id: 10, content: "2021 sucks!", date: new Date() }
        ]
      }
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
  computed: {
    newTwootCharacterCount() {
      return this.newTwootContent.length;
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
    postTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== 'draft'){
       this.user.twoots.unshift(
           {
             id: this.user.twoots.length + 1,
             content: this.newTwootContent,
             date: new Date()
           }
       )
        this.newTwootContent = "";
      }
    }
  },
  mounted() {
    this.followUser();
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

      .user-profile__create-twoot{
        padding-top: 20px;
        display: flex;
        flex-direction: column;

        &.dangerError {
          color: red;
        }

      }

    }

    .user-profile__twoots-wrapper {
      display: grid;
      grid-gap: 10px;
    }

  }

</style>