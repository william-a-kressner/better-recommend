<template>
    <h1>Welcome, {{ user.name }}</h1>
</template>

<script>
export default {
    data() {
        return {
            user: { 
                name: ''
            },
            accessToken: ''
        }
    },
    async mounted() {
        this.accessToken = window.location.toString().split("access_token=")[1].split("&")[0]
        const headers = new Headers()
        headers.append('Authorization', `Bearer ${this.accessToken}`)
        const response = await fetch('https://api.spotify.com/v1/me', {
            method: 'get',
            headers,
        })
        const data = await response.json()
        this.user.name = data.display_name
    }
}
</script>