<template>
  <form :class="{'.dangerError': state.newTwootContent.length > 180}" class="user-profile__create-twoot" @submit.prevent="postTwoot">
    <label class="" for="newTwoot"><strong>New Twoot</strong> ({{ state.newTwootContent.length }}/180)</label>
    <textarea id="newTwoot" rows="4"  v-model="state.newTwootContent"></textarea>


    <div class="user-profile__create-twoot-type">
      <label for="newTwootType"><strong>Type: </strong></label>
      <select id="newTwootType" v-model="state.selectedTwootType">
        <option :value="option.value" v-for="(option, index) in state.twootTypes" :key="index">
          {{ option.name }}
        </option>
      </select>
    </div>

    <button>Post</button>

  </form>

</template>

<script>
import {reactive, computed, watchEffect} from 'vue';
export default {
  name: "CreateTwoot",
  setup(props, ctx) {
    const state = reactive({
      newTwootContent: '',
      selectedTwootType: 'instant',
      twootTypes: [
        { value: 'draft', name: "Draft"},
        { value: 'instant', name: "Instant Twoot"}
      ],
    });

    const newTwootCharacterCount = computed(() => state.newTwootContent.length);


    function postTwoot() {
      if (state.selectedTwootType === 'instant') {
        ctx.emit("postTwoot", state.newTwootContent);
        state.newTwootContent = "";
      }
    }

    watchEffect(() => {
      if (state.newTwootContent.length > 180){
        state.newTwootContent = state.newTwootContent.substring(0, 180);
      }

    })


    return {
      state,
      newTwootCharacterCount,
      postTwoot
    }
  }

}
</script>

<style lang="scss" scoped>
.user-profile__create-twoot{
  padding-top: 20px;
  display: flex;
  flex-direction: column;

  &.dangerError {
     color: red;
   }

}
</style>