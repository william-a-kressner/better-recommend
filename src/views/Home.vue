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
  data() {
    return {
      isSignedIn: false
    }
  },
  methods: {
    signin(){
      this.$router.push('/signin')
    },
    displayDashboard() {
      this.$router.push('/dashboard')
    }
  },
  async mounted() {
    const headers = new Headers()
    headers.append('Authorization', `Bearer ${localStorage.accessToken}`)
    const response = await fetch('https://api.spotify.com/v1/me/', {
        method: 'get',
        headers,
    })
    if (response.status === 200) this.isSignedIn = true
  }
}
</script>
