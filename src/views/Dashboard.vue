<template>
    <h1>Welcome, {{ user.name }}</h1>
</template>

<script>
export default {
    data() {
        return {
            user: { 
                name: ''
            }
        }
    },
    async mounted() {
        if (!localStorage.accessToken) {
            localStorage.accessToken = window.location.toString().split('access_token=')[1].split('&')[0]
            const expireTime = Number(window.location.toString().split('expires_in=')[1])
            console.log(`Access token set to exprie in ${expireTime} seconds`)
            setTimeout(() => localStorage.accessToken = '', expireTime * 1000)
            this.$router.push('/dashboard')
        }
        const headers = new Headers()
        headers.append('Authorization', `Bearer ${localStorage.accessToken}`)
        const response = await fetch('https://api.spotify.com/v1/me', {
            method: 'get',
            headers,
        })
        const data = await response.json()
        this.user.name = data.display_name
    }
}
</script>