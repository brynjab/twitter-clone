<template>
  <div class="home">
    <TweetInput/>
    <div v-for="tweet in user.tweets" v-bind:key="tweet.id" class="tweet-post">
      <div>
        <img :src="user.profilePhotoURL" alt="" class="profile-image">
      </div>
      <div class="tweet-content">
        <span class="user-name">{{user.userName}}</span>
        <span>@{{user.twitterHandle}}</span><br>
        <span>{{tweet.text}}</span>
        <div class="tweet-image">
          <img :src="tweet.image" alt="">
        </div>
        <div class="tweet-interactions">
          <div>
            <p class="material-symbols-outlined">mode_comment</p>
            <p class="numbers">{{tweet.replies.length}}</p>
          </div>
          <div>
            <p class="material-symbols-outlined">autorenew</p>
            <p class="numbers">{{tweet.retweets}}</p>
          </div>
          <div>
            <p class="material-symbols-outlined">favorite</p>
            <p class="numbers">{{tweet.likes}}</p>
          </div>
          <div>
            <p class="material-symbols-outlined">file_upload</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>




<script>
import axios from 'axios'
import TweetInput from '../components/TweetInput.vue'

export default {
  name: 'Home',
  components: {
    TweetInput
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
  min-width: 60px;
  height: 60px;
  object-fit: contain;
  border-radius: 50%;
  margin-right: 10px;
}
.tweet-post {
  padding: 20px;
  display: flex;
  border: solid 1px #e5e5e5;
}
.tweet-input {
  margin-top: 10px;
  border: 0px;
}
.tweet-image {
  padding-top: 20px;
  padding-bottom: 20px;
}
.tweet-image, img {
  max-width: 100%;
}
.user-name {
  font-weight: bold;
  margin-right: 10px;
}
.material-symbols-outlined {
  margin-right: 10px;
  font-variation-settings:
  'FILL' 0,
  'wght' 300,
  'GRAD' 0,
  'opsz' 48
}
.tweet-interactions{
  display: flex;
  justify-content: space-between;
}
.numbers {
  float: right;
}
</style>