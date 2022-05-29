<template>
  <div>
    <div class="cover">
        <img src="../assets/f1-cover.jpg" alt="" class="cover-image">
        <img :src="user.profilePhotoURL" alt="" class="profile-image">
    </div>
    <div class="user-info">
      <h4>{{user.userName}}</h4>
      <p>@{{user.twitterHandle}}</p>
      <div class="flex">
        <div>
          <p class="material-symbols-outlined">pin_drop</p>
          <p class="float-right">{{user.location}}</p>
        </div>
        <div>
          <p class="material-symbols-outlined">calendar_month </p>
          <p class="float-right">Joined August 2009</p>
        </div>
      </div>
      <div>
        <span class="number">{{user.following}}</span>
        <span class="mr-15">Following</span>
        <span class="number">{{user.followers | abbr }}</span>
        <span>Followers</span>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'ProfileBanner',
    data: () => {
    return{
      user: []
    }
    
  },
  filters: {
        abbr: function(num) {
            if (String(num).length < 7) {
                return (num/1000).toFixed(1) + 'K';
            } else {
                return (num/1000000).toFixed(1) + 'M';
            }
        }
    },
  mounted() {
    axios.get('https://localhost:7109/api/users/f1').then(response => {
      this.user = response.data
      console.log(response.data)
    })
  }
}
</script>

<style scoped>
.cover {
  position: relative;
  top: 0;
  left: 0;
  margin-bottom: 55px;
}
.cover-image {
  position: relative;
  top: 0;
  left: 0;
  width: 100%;
  object-fit: cover;
}
.profile-image {
  position: absolute;
  bottom: -50px;
  left: 20px;
  min-width: 60px;
  height: 100px;
  object-fit: contain;
  border-radius: 50%;
  margin-right: 10px;
  border: 1px solid #e5e5e5
}
.user-info {
    padding-left: 20px;
    margin-bottom: 10px;
}
.material-symbols-outlined {
    font-variation-settings:
    'FILL' 0,
    'wght' 300,
    'GRAD' 0,
    'opsz' 48
}
.flex {
    display: flex;
}
.float-right {
    float: right;
    margin-right: 15px;
}
.number {
    font-weight: bold;
    margin-right: 3px;
}
.mr-15 {
    margin-right: 15px;
}
</style>