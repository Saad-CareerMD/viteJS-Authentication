<template>
  <div>
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="feed">Feed</router-link> |
      <!-- <span v-if="isLoggedIn"> 
        <router-link to="/feed"> Feed </router-link> |
      </span> -->
      <span v-if="isLoggedIn">
        <button @click="signOut">Logout</button>
      </span>
      <span v-else>
        <router-link to="/register"> Register </router-link> |
        <router-link to="/sign-in"> Login </router-link>
      </span>
    </nav>
  </div>
  <router-view/>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<script lang="ts" setup>
import { ref, watchEffect } from 'vue' // used for conditional rendering
import {getAuth, onAuthStateChanged} from 'firebase/auth'
import { useRouter } from 'vue-router'
const router = useRouter()
const isLoggedIn = ref(true)
  // runs after firebase is initialized
  const auth = getAuth();
  onAuthStateChanged(auth, function(user) {
      if (user) {
        isLoggedIn.value = true // if we have a user
      } else {
        isLoggedIn.value = false // if we do not
      }
  })
  const signOut = () => {
    const auth = getAuth();
    auth.signOut()
    router.push('/')
  }
</script>