<template>
  <div class="home">
    <div class="tweet-box">
      <div class="input-group mt-2 mb-2">
        <img :src="user.profilePhotoURL" alt="" class="profile-image" @click="toProfile()">
        <input type="text" class="form-control tweet-input"  placeholder="What's happening?" v-model="tweetText">
        <span class="input-group-btn">
          <button class="btn btn-primary tweet-button" type="button" @click="addTweet()">Tweet</button>
        </span>
      </div>
      <div>
        <span class="material-symbols-outlined" @click="showInput = !showInput">image</span>
        <span class="material-symbols-outlined">gif_box</span>
        <span class="material-symbols-outlined">bar_chart</span>
        <span class="material-symbols-outlined">sentiment_satisfied</span>
        <span class="material-symbols-outlined">calendar_month </span>
        <span class="material-symbols-outlined">pin_drop</span>
      </div>
      <div v-if="showInput">
        <input type="text" placeholder="image link" v-model="tweetImage" class="image-input">
      </div>
    </div>
    <div v-for="tweet in tweets.slice().reverse()" v-bind:key="tweet.id"  class="tweet-post">
      <div>
        <img :src="tweet.user.profilePhotoURL" alt="" class="profile-image" @click="toProfile()">
      </div>
      <div class="tweet-content" >
        <span class="user-name" @click="toTweet(tweet.id)">{{tweet.user.userName}}</span>
        <span>@{{tweet.user.twitterHandle}}</span>
        <button v-if="tweet.user.id === 1" class="btn btn-delete material-symbols-outlined" @click="deleteTweet(tweet.id)">delete</button><br>
        <span class="tweet-text" @click="toTweet(tweet.id)">{{tweet.text}}</span>
        <div class="tweet-image">
          <img :src="tweet.image" alt="" @click="toTweet(tweet.id)">
        </div>
        <div class="tweet-interactions">
          <div @click="toTweet(tweet.id)">
            <p class="material-symbols-outlined">mode_comment</p>
            <p class="numbers">{{tweet.replies.length}}</p>
          </div>
          <div>
            <p class="material-symbols-outlined">autorenew</p>
            <p class="numbers">{{tweet.retweets | abbr }}</p>
          </div>
          <div v-if="tweet.isLiked == false" @click="likeTweet(tweet.id, tweet.likes)">
            <i class="bi bi-heart"></i>
            <p class="numbers">{{tweet.likes | abbr }}</p>
          </div>
          <div v-else @click="dislikeTweet(tweet.id, tweet.likes)">
            <i class="bi bi-heart-fill"></i>
            <p class="numbers">{{tweet.likes | abbr }}</p>
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

export default {
  name: 'Home',
  components: {
    
  },
  data: () => {
    return{
      user: [],
      tweetImage: undefined,
      tweetText: undefined,
      showInput: false,
      tweets: [],
      likes: []
    }
  },
  methods: {
    fetchTweet(){
    axios.get('https://localhost:7109/api/twitter').then(response => {
      this.tweets = response.data
      console.log(response.data)
    }).catch(error => console.log(error))
    },
    addTweet(){
      const tweetToAdd = { text: this.tweetText, image: this.tweetImage, likes: 0, retweets: 0, userId: this.user.id}
      axios.post('https://localhost:7109/api/twitter', tweetToAdd).then( () => this.fetchTweet())
      this.tweetText = "";
      this.tweetImage = "";
      this.showInput = false;
      console.log("tweet added" + tweetToAdd)
    },
    toProfile(){
      this.$router.push("/profile")
    },
    deleteTweet(tweetId){
      if(confirm('are you sure?'))axios.delete(`https://localhost:7109/api/twitter/${tweetId}`).then( () => this.fetchTweet())
    },
    toTweet(id){
      this.$router.push(`/tweet/${id}`)
    },
    likeTweet(tweetId, counter){
      axios.put(`https://localhost:7109/api/twitter/${tweetId}`, { likes: parseInt(counter) + 1, isLiked: true }).then( () => this.fetchTweet())
    },
    dislikeTweet(tweetId, counter){
      axios.put(`https://localhost:7109/api/twitter/${tweetId}`, {likes: parseInt(counter) - 1, isLiked: false }).then( () => this.fetchTweet())
    }
  },  
  filters: {
    abbr: function(num) {
      if (String(num).length < 4) {
        return num;
      } else if(String(num).length >= 4 && String(num).length < 7) {
        return (num/1000).toFixed(1) + 'K';
      } else {
        return (num/1000000).toFixed(1) + 'M';
      }
    }
  },  
  mounted() {
    this.fetchTweet(),
    axios.get('https://localhost:7109/api/users/f1').then(response => {
      this.user = response.data
    }).catch(error => console.log(error))
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
.profile-image:hover{
  cursor: pointer;
}
.tweet-box{
    padding-left: 20px;
    border: solid 1px #e5e5e5;
}
.tweet-post {
  padding: 20px;
  display: flex;
  border: solid 1px #e5e5e5;
}
.tweet-input {
  border: 0px;
}
.tweet-input:focus {
    outline: none;
}
.image-input {
    border: 0px;
    border-bottom: solid 1px #1D9BF0;
}
.image-input:focus {
    outline: none;
}
.tweet-button {
  background-color: #1D9BF0;
  margin-top: 10px;
  margin-right: 20px;
  border-radius: 20px;
  padding-right: 30px;
  padding-left: 30px;
}
.tweet-image {
  padding-top: 20px;
  padding-bottom: 20px;
}
.tweet-image:hover{
  cursor: pointer;
}
.tweet-image, img {
  max-width: 100%;
  border-radius: 15px;
}
.user-name {
  font-weight: bold;
  margin-right: 10px;
}
.user-name:hover {
  cursor: pointer;
}
.material-symbols-outlined {
  margin-right: 10px;
  font-variation-settings:
  'FILL' 0,
  'wght' 300,
  'GRAD' 0,
  'opsz' 48
}
.tweet-content {
  width: 100%;
  white-space: pre-line;
}
.tweet-content i {
  margin-right: 10px;
}
.tweet-interactions{
  display: flex;
  justify-content: space-between;
}
.tweet-interactions:hover {
  cursor: pointer;
}
.numbers {
  float: right;
}
.btn-delete {
  float: right;
  font-size: 20px;
}
.tweet-text:hover {
  cursor: pointer;
}
</style>