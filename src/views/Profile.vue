<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col-2">
          <img :src="user.profilePhotoURL" alt="" class="profile-image">
        </div>
        <div class="col-10">
            <input type="text" placeholder="What's happening?" class="tweet-input">
        </div>
      </div>
    </div>
    <div v-for="tweet in user.tweets" v-bind:key="tweet.id">
      
    {{tweet}}<br>
      <div>
        {{tweet.text}}
      </div>
      <div>
          <img :src="tweet.image" alt=""  class="tweet-image">
      </div>
      

    </div>



  </div>
</template>




<script>
import axios from 'axios'

export default {
  name: 'Home',
  components: {
  },
  data: () => {
    return{
      user: []
    }
    
  },
  methods: {

  }, 
  mounted() {
    axios.get('https://localhost:7109/api/twitter/f1').then(response => {
      this.user = response.data
    })
  }
}
</script>

<style scoped>
.profile-image {
  width: 60px;
  height: 60px;
  object-fit: contain;
  border-radius: 50%;
}
.tweet-input {
    margin-top: 10px;
    border: 0px;
}
.tweet-image {
    max-width: 100%;
}
</style>