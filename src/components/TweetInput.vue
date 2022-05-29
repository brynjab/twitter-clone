<template>
  <div class="tweet-box">
    <div>
      <img :src="user.profilePhotoURL" alt="" class="profile-image" @click="toProfile()">
    </div>
    <div>
          <div class="input-box">
            <input type="text" placeholder="What's happening?" class="tweet-input" v-model="tweetText">
            
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
    <div>

            <button class="btn btn-primary" @click="addTweet()">Tweet</button>
    </div>
  </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'TweetInput',
        data: () => {
            return {
                user: [],
                tweetImage: undefined,
                tweetText: undefined,
                showInput: false
            }
        },
        methods: {
            addTweet(){
                const tweetToAdd = { text: this.tweetText, image: this.tweetImage, likes: 0, retweets: 0, userId: this.user.id}
                axios.post('https://localhost:7109/api/twitter', tweetToAdd)
                this.tweetText = ""
                this.tweetImage = ""
                console.log(tweetToAdd)
            },
            toProfile(){
                this.$router.push("/profile")
            }
        },
        mounted() {
            axios.get('https://localhost:7109/api/twitter/f1').then(response => {
                this.user = response.data
            }).catch(error => console.log(error))
            
        }
    }
</script>

<style scoped>
.profile-image {
    width: 60px;
    height: 60px;
    object-fit: contain;
    border-radius: 50%;
    margin-right: 10px;
}
.tweet-box{
    display: flex;
    padding-left: 20px;
    border: solid 1px #e5e5e5;
}
.tweet-input {
    margin-top: 10px;
    border: 0px;
}
.tweet-input:focus {
    outline: none;
}
.input-box {
    height: 60px;
    font-size: 20px;
}
.material-symbols-outlined {
    color: #1D9BF0;
    padding-right: 10px;
    font-variation-settings:
    'FILL' 0,
    'wght' 300,
    'GRAD' 0,
    'opsz' 48
}
.image-input {
    border: 0px;
    border-bottom: solid 1px #1D9BF0;
}
.image-input:focus {
    outline: none;
}
.btn-primary {
  background-color: #1D9BF0;
  border-radius: 20px;
  padding-right: 30px;
  padding-left: 30px;
}
</style>