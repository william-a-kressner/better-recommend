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
    mounted() {
        this.accessToken = window.location.toString().split("access_token=")[1].split("&")[0]
        console.log(this.accessToken)
    }
}
</script>