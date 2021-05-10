<template>
  <div class="home">
    <h1>Welcome to BetterRecommend!</h1>
    <button @click="signin">Sign in</button>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data() {
    return {
        isSignedIn: false,
      }
  },
  methods: {
    signin(){
      this.$router.push('/signin')
    }
  }
}
</script>
