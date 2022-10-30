<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
// import HelloWorld from './components/HelloWorld.vue'
import Footer from './components/Footer.vue'
import Header from './components/Header.vue'
// import Intro from './components/Intro.vue'
import Halls from './components/Halls/Halls.vue'
import SearchBar from './components/SearchBar.vue'

</script>

<template>
  <main id="scrollspyHeading1">
  <Header bride="SG" groom="SD" />
    <!-- <Intro /> -->
    <div class="album py-5 bg-light">
      <div class="container">
        <Halls />

        <button class="btn btn-danger btn-sm" id="btn-back-to-top" >
          <a href="#scrollspyHeading1" style="text-decoration-line:none; color:white">
            TOP
          </a>
        </button>
      </div>
    </div>
    <Footer />

  </main>

</template>

<style>
.bd-placeholder-img {
  font-size: 1.125rem;
  text-anchor: middle;
  -webkit-user-select: none;
  -moz-user-select: none;
  user-select: none;
}

@media (min-width: 100%) {
  .bd-placeholder-img-lg {
    font-size: 3.5rem;
  }
}

#btn-back-to-top {
  position: fixed;
  bottom: 20px;
  right: 20px;
  /* display: none; */
}
</style>
