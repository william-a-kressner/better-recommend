<template>
  <div class="home">
    <h1>Welcome to BetterRecommend!</h1>
    <button v-if="!isSignedIn" @click="signin">Sign in</button>
    <button v-else @click="displayDashboard">Go to Dashboard</button>
  </div>
</template>

<script>

export default {
  name: 'Home',
  methods: {
    signin(){
      this.$router.push('/signin')
    },
    displayDashboard() {
      this.$router.push('/dashboard')
    }
  },
  computed: {
    isSignedIn() {
      return localStorage.accessToken === '' ? false : true
    }
  }
}
</script>
