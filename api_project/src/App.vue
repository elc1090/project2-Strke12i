<script setup>
import MyBand from './components/MyBand.vue';
import NavBar from './components/NavBar.vue';
import Band from "./components/Band.vue"
</script>

<template>
    <header class="w-full">
        <div class="items-center justify-center">
            <NavBar @inputValue="inputTextEmit" :findArtist="buscarArtista" />
            <MyBand v-if="data" :data="data" />
            <div class="flex justify-center items-center mb-4">
                <div class="grid grid-cols-1 grid-rows-10 gap-4 w-3/4 md:grid-cols-2">
                <Band v-if="bands" v-for="band in bands" :data="band"/>
            </div>
            </div>
            
        </div>
    </header>
</template>

<script>
var localStorage = window.localStorage

export default {
    async created() {
        var token = localStorage.getItem("spotifyToken");
        const clientId = process.env.VUE_APP_CLIENT_ID
        const clientSecret = process.env.VUE_APP_CLIENT_SECRET;
        console.log(clientId);
        console.log(clientSecret);
        const getToken = async () => {
            const response = await fetch("https://accounts.spotify.com/api/token", {
                method: "POST",
                headers: {
                    "Content-Type": "application/x-www-form-urlencoded",
                    "Authorization": `Basic ${btoa(`${clientId}:${clientSecret}`)}`
                },
                body: "grant_type=client_credentials"
            });
            const data = await response.json();
            return data.access_token;
        };
        const isTokenExpired = (token) => {
            const date = new Date(parseInt(token.split(".")[1]));
            if (Date.now() - date.getTime() > 3600000) {
                return true;
            }
            return false;
        };
        if (!token || isTokenExpired(token)) {
            token = await getToken();
            if(token){
                const date_token = token + "." + String(Date.now());
                localStorage.setItem("spotifyToken", date_token);
            }
           
        }
        console.log(localStorage.getItem("spotifyToken"));
    },
    components: { MyBand, NavBar },
    data() {
        return {
            inputText: '',
            data: null,
            bands: null,
        }
    },
    methods: {
        inputTextEmit(value) {
            this.inputText = value
        },
        async buscarArtista() {
            const getArtist = async (token) => {
                const response = await fetch(`https://api.spotify.com/v1/search?type=artist&q=${this.inputText}`, {
                    method: 'GET',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${token}`,
                        "Accept": "application/json",
                    }
                });

                const data = await response.json();
                return data.artists.items[0];
            }

            const getRelatedArtists = async (artistId, token) => {
                const response = await fetch(`https://api.spotify.com/v1/artists/${artistId}/related-artists`, {
                    method: 'GET',
                    headers: {
                        'Content-Type': 'application/json',
                        'Authorization': `Bearer ${token}`,
                        "Accept": "application/json",
                    }
                });

                const data = await response.json();
                return data.artists;
            };

             const token = localStorage.getItem("spotifyToken").split(".")[0];
            this.data = await getArtist(token);

            if(this.data){
                this.bands = await getRelatedArtists(this.data.id,token)
            } 

    }
    }
}
</script>