<template>
  <div class="tweet-box">
    <div>
      <img :src="user.profilePhotoURL" alt="" class="profile-image">
    </div>
    <div>
          <div class="input-box">
            <input type="text" placeholder="What's happening?" class="tweet-input" >
          </div>
          <div>
            <span class="material-symbols-outlined">image</span>
            <span class="material-symbols-outlined">gif_box</span>
            <span class="material-symbols-outlined">bar_chart</span>
            <span class="material-symbols-outlined">sentiment_satisfied</span>
            <span class="material-symbols-outlined">calendar_month </span>
            <span class="material-symbols-outlined">pin_drop</span>
          </div>
    </div>
  </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: 'TweetInput',
        data: () => {
            return {
                user: []
            }
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
    margin-right: 10px;
}
.tweet-box{
    display: flex;
    border: solid 1px #e5e5e5;
    padding-left: 20px;
}
.tweet-input {
    margin-top: 10px;
    border: 0px;
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
</style>