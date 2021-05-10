<template>
    <h1 v-if="!isSignedIn">Signing in...</h1>
    <div id="spotify-page"></div>
</template>
<script>
export default {
    data() {
        return {
            isSignedIn: false,
            access_token: ''
        }
    },
    mounted() {
        console.log(this.$route.query)
        window.location.href = 'https://accounts.spotify.com/authorize?client_id=3b71bf200a4c43c89a9472fa3b6aafaa&response_type=token&scope=user-read-currently-playing&redirect_uri=http://localhost:8080/dashboard'
    },
}
</script>
<style lang="scss" scoped>

</style>