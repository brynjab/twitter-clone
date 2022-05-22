<template>
  <div id="app">
    <Navbar/>
    <div class="container-fluid">
      <div class="row">
        <div class="col-1 col-sm-3"></div>
        <div class="col-10 col-sm-7">
          <router-view />
        </div>
        <div class="col-1 col-sm-2"></div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from './components/Navbar.vue'

export default {
  name: 'app',
  components: {
    Navbar
  }
}

</script>

<style scoped>

</style>