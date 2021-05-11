<template>
    <h1>You are listening to {{ currentSong.name }} by {{ currentSong.artist }} off the album {{ currentSong.albumName }}</h1>
    <img id="album-cover" :src="currentSong.albumArtUrl" alt="Album art">
</template>

<script>
import ColorThief from 'colorthief'
export default {
    data() {
        return {
            currentSong: { 
                name: '',
                albumName: '',
                albumArtUrl: '',
                artist: ''
            },
            accessToken: ''
        }
    },
    async mounted() {
        this.accessToken = window.location.toString().split("access_token=")[1].split("&")[0]
        this.getCurrentSong()
        setInterval(this.getCurrentSong, 5000)

    },
    methods: {
        async getCurrentSong(){
            const headers = new Headers()
            headers.append('Authorization', `Bearer ${this.accessToken}`)
            const response = await fetch('https://api.spotify.com/v1/me/player/currently-playing', {
                method: 'get',
                headers,
            })
            const data = await response.json()
            if (this.currentSong.name !== data.item.name) {
                this.currentSong = {
                    name: data.item.name,
                    albumName: data.item.album.name,
                    albumArtUrl: data.item.album.images[0].url,
                    artist: data.item.artists[0].name
                }
                this.getColors()
            }
        },
        getColors() {
            const colorThief = new ColorThief()
            // const albumArtImg = document.getElementById('album-cover').getAttribute('src')
            console.log(this.currentSong.albumArtUrl)            
            colorThief.getColor(this.currentSong.albumArtUrl).then(color => { console.log(color)}).then(err => { console.log(err)})            
        }
    }
}
</script>