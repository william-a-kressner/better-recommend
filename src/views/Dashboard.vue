<template>
    <div v-if="Object.keys(currentSong).length !== 0">
        <h1>You are listening to {{ currentSong.name }} by {{ currentSong.artist }} off the album {{ currentSong.albumName }}</h1>
        <img id="album-cover" :src="currentSong.albumArtUrl" alt="Album art" crossorigin="anonymous">
    </div>
    <div v-else>
        <h1>You aren't listening to anything! Fire up Spotify to see your song here.</h1>
    </div>
</template>

<script>
import ColorThief from 'colorthief'
export default {
    data() {
        return {
            currentSong: {},
            accessToken: ''
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
        this.$router.push('/dashboard')
        this.getCurrentSong()
        setInterval(this.getCurrentSong, 5000)
    },
    methods: {
        async getCurrentSong(){
            const headers = new Headers()
            headers.append('Authorization', `Bearer ${localStorage.accessToken}`)
            const response = await fetch('https://api.spotify.com/v1/me/player/currently-playing', {
                method: 'get',
                headers,
            })
            this.updateCurrentSong(response)
        },
        async updateCurrentSong(response) {
            if (response.status === 204) {
                this.currentSong = {}
            } else {
                const data = await response.json()
                const oldUrl = this.currentSong?.albumArtUrl
                if (this.currentSong?.name !== data.item.name) {
                    this.currentSong = {
                        name: data.item.name,
                        albumName: data.item.album.name,
                        albumArtUrl: data.item.album.images[0].url,
                        artist: data.item.artists[0].name
                    }
                    if (oldUrl !== this.currentSong.albumArtUrl) this.getColors()
                }                
            }
        },
        async getColors() {
            const colorThief = new ColorThief()
            const albumArtImg = document.getElementById('album-cover')
            const colorArr = await colorThief.getPalette(albumArtImg)    
            console.log(colorArr)
        }
    }
}
</script>