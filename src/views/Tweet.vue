<template>
  <div>
    <div>
      <router-link to="/" style="text-decoration: none; color: inherit;">
        <div class="go-back">
          <span class="material-symbols-outlined">arrow_back</span> 
          <span>Tweet</span>
        </div>
      </router-link>
    </div>
    <div class="tweet-post" v-if="tweet.user">
      <div>
        <img :src="tweet.user.profilePhotoURL" alt="" class="profile-image">
      </div>
      <div class="tweet-content">
        <span class="user-name">{{tweet.user.userName}}</span>
        <span>@{{tweet.user.twitterHandle}}</span>
        <button v-if="tweet.user.id === 1" class="btn btn-delete material-symbols-outlined" @click="deleteTweet(tweet.id)">delete</button><br>
        <span>{{tweet.text}}</span>
        <div class="tweet-image">
          <img :src="tweet.image" alt="">
        </div>
        <div class="tweet-interactions">
          <div >
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
    <div class="reply-box">
        <div class="input-group mt-2 mb-2">
          <img :src="user.profilePhotoURL" alt="" class="profile-image" @click="toProfile()">
          <input type="text" class="form-control tweet-input"  placeholder="Tweet your reply" v-model="replyText">
          <span class="input-group-btn">
            <button class="btn btn-primary reply-button" type="button" @click="addReply()">Reply</button>
          </span>
        </div>
        <div>
          <span class="material-symbols-outlined blue" @click="showInput = !showInput">image</span>
          <span class="material-symbols-outlined blue">gif_box</span>
          <span class="material-symbols-outlined blue">sentiment_satisfied</span>
        </div>
        <div v-if="showInput">
          <input type="text" placeholder="image link" v-model="replyImage" class="image-input">
        </div>
    </div>
    <div v-for="reply in tweet.replies" v-bind:key="reply.id" >
      <div class="tweet-post">
        <div>
          <img :src="user.profilePhotoURL" alt="" class="profile-image">
        </div>
        <div class="tweet-content">
          <span class="user-name">{{user.userName}}</span>
          <span>@{{user.twitterHandle}}</span><br>
          <span>{{reply.text}}</span>
          <div v-if="reply.image === !null" class="tweet-image">
            <img :src="reply.image" alt="">
          </div>
          <div class="tweet-interactions">
          <div>
            <p class="material-symbols-outlined">mode_comment</p>
            <p class="numbers">{{reply.replies.length}}</p>
          </div>
          <div>
            <p class="material-symbols-outlined">autorenew</p>
            <p class="numbers">{{reply.retweets | abbr }}</p>
          </div>
          <div v-if="tweet.isLiked == false" @click="likeTweet(tweet.id, tweet.likes)">
            <i class="bi bi-heart"></i>
            <p class="numbers">{{reply.likes | abbr }}</p>
          </div>
          <div v-else @click="dislikeTweet(tweet.id, tweet.likes)">
            <i class="bi bi-heart-fill"></i>
            <p class="numbers">{{reply.likes | abbr }}</p>
          </div>
          <div>
            <p class="material-symbols-outlined">file_upload</p>
          </div>
        </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data: () => {
    return{
      user: [],
      tweet: [],
      replyText: undefined,
      replyImage: undefined,
      showInput: false
    }
  },
  methods: {
    fetchTweet(){
      axios.get(`https://localhost:7109/api/twitter/${this.$route.params.id}`).then(response => {
      this.tweet = response.data
      console.log(response.data)
      }).catch(error => console.log(error))
    },
      addReply(){
      const replyToAdd = { text: "Replying to @" + this.tweet.user.twitterHandle + '\n' + this.replyText, image: this.replyImage, likes: 0, retweets: 0, userId: this.user.id}
      axios.post(`https://localhost:7109/api/twitter/${this.$route.params.id}`, replyToAdd).then( () => this.fetchTweet())
      this.replyText = ""
      this.replyImage = ""
      console.log("reply added" + replyToAdd)
    },
    toProfile(){
      this.$router.push("/profile")
    },
    likeTweet(tweetId, counter){
      axios.put(`https://localhost:7109/api/twitter/${tweetId}`, { likes: parseInt(counter) + 1, isLiked: true }).then( () => this.fetchTweet())
    },
    dislikeTweet(tweetId, counter){
      axios.put(`https://localhost:7109/api/twitter/${tweetId}`, {likes: parseInt(counter) - 1, isLiked: false }).then( () => this.fetchTweet())
    }
  },
  mounted() {
    axios.get('https://localhost:7109/api/users/f1').then(response => {
      this.user = response.data
    }).catch(error => console.log(error)),
    this.fetchTweet()
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
}
</script>

<style>
.profile-image {
  min-width: 60px;
  height: 60px;
  object-fit: contain;
  border-radius: 50%;
  margin-right: 10px;
}
.profile-image:hover {
  cursor: pointer;
}
.reply-box{
    padding-left: 20px;
}
.tweet-post {
  padding: 20px;
  display: flex;
}
.blue {
    color: #1D9BF0;
}
.tweet-input {
  margin-top: 10px;
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
.reply-button {
  background-color: #1D9BF0;
  margin-top: 10px;
  margin-right: 25px;
  border-radius: 20px;
  padding-right: 30px;
  padding-left: 30px;
}
.tweet-image {
  padding-top: 20px;
  padding-bottom: 20px;
}
.tweet-image, img {
  max-width: 100%;
  border-radius: 15px;
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
.tweet-content {
  width: 100%;
  white-space: pre-line;
}
.tweet-content i {
  margin-right: 10px;
}
.tweet-interactions{
  margin-top: 10px;
  display: flex;
  justify-content: space-between;
}
.numbers {
  float: right;
}
.btn-delete {
  float: right;
  font-size: 20px;
}
.go-back{
    display: flex;
    margin: 10px;
    text-decoration: none;
}
</style>