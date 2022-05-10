<template>
      <nav class="navbar fixed-bottom navbar-light bg-light ">
      <div class="container-fluid">
          <div class="twitter-icon">
            <font-awesome-icon icon="fa-brands fa-twitter" />
          </div>
          <div class="nav-item home">
            <span class="material-symbols-outlined">home</span>
            <div class="text"><p>Home</p></div>
          </div>
          <div class="nav-item explore">
            <span class="material-symbols-outlined">tag</span>
            <div class="text">
              <p>Explore</p></div>
            </div>
          <div class="nav-item notifications">
            <span class="material-symbols-outlined">notifications</span>
            <div class="text"><p>Notifications</p></div>
          </div>
          <div class="nav-item messages">
            <span class="material-symbols-outlined">mail</span>
            <div class="text"><p>Messages</p></div>
          </div>
          <div class="nav-item bookmarks">
            <span class="material-symbols-outlined">bookmark</span>
            <div class="text"><p>Bookmarks</p></div>
          </div>
          <div class="nav-item lists">
            <span class="material-symbols-outlined">article</span>
            <div class="text"><p>Lists</p></div>
          </div>
          <div class="nav-item profile">
            <span class="material-symbols-outlined">person</span>
            <div class="text"><p>Profile</p></div>
          </div>
          <div class="nav-item more">
            <span class="material-symbols-outlined">pending</span>
            <div class="text"><p>More</p></div>
          </div>
          <div class="nav-item tweet">
            <button class="btn btn-primary">Tweet</button>
          </div>
      </div>
    </nav>
</template>

<script>
export default {

}
</script>

<style scoped>
  @media(min-width:576px) {
    nav.navbar {
      position: fixed;
      left: 0;
      width: 20%;
      bottom: 0;
      top: 0;
      overflow-y: auto;
      overflow-x: hidden;
      display: block;
      border-right: 1px solid #cecece;
    }
    nav.navbar>.container-fluid {
      flex-direction: column;
      padding: 0;
    }
  }
  @media(max-width:576px) {
    button.btn {
      display: none;
    }
    .twitter-icon, .bookmarks, .lists, .profile, .more  {
      display: none;
    }
    .nav-item.home {
      margin-left: 15%;
    }
  }

  @media(max-width:1281px) {
    .text {
      display: none;
    }
    button.btn {
    padding-right: 10px;
    padding-left: 10px;
    }
  }


  .twitter-icon {
    color: #1D9BF0;
    font-size: 30px;
  }

  .material-symbols-outlined {
    font-variation-settings:
      'FILL'0,
      'wght'500,
      'GRAD'0,
      'opsz'48
  }

  .btn {
    background-color: #1D9BF0;
    border-radius: 20px;
    padding-right: 30px;
    padding-left: 30px;
  }
  .nav-item{
    margin-top: 20px;
  }

  p {
    text-align: left;
  }

  .text {
    width: 100px;
    padding-left: 10px;
    float: right;
  }
</style>