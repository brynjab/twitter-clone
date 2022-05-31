<template>
  <div id="app">
    <Navbar/>
    <div class="container-fluid">
      <div class="row">
        <div class="col-1 col-sm-3"></div>
        <div class="col-10 col-sm-8">
          <div class="max-width">
            <router-view />
          </div>
        </div>
        <div class="col-1 col-sm-1">
        </div>
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
.max-width{
  max-width: 800px;
}
</style>