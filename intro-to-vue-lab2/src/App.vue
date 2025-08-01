<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import { useMessageStore } from '@/stores/message'
import { storeToRefs } from 'pinia'
const store = useMessageStore()
const { message } = storeToRefs(store)

</script>

<template>
  <div id="layout">
  <header>
    <div id="flashMessage" v-if="message">
      <h4>{{ message }}</h4>
    </div>
    <div class="wrapper">
      <nav>
        <RouterLink to="/">Home</RouterLink> |
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header>

  <RouterView />
</div>

</template>

<style scoped>
#layout {
  font-family: Arial, Helvetica, sans-serif;
  -webkit-font-smoothing:antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}


nav{
padding: 30px;

}
nav a {
 font-weight: bold;
   color: #2c3e50;

}


  nav {
    text-align: center;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }

nav a.router-link-exact-active {
    color: #42b983;
}
@keyframes yellofade {
  from {
    background-color: yellow;
  }
  to {
    background-color: transparent;
  }
}

#flashMessage {
  animation: yellofade 3s ease-in-out;
}


</style>
