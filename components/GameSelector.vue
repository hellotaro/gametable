<template>
<div>
    <div class="flex justify-center">
        <CardGameCard v-for="card in games" :key="card.title" :title="card.title" :img_src="card.img_src" :content="card.content" :link="card.link" />
    </div>
    <div class="flex justify-center">
        <CardAvatorCard v-for="avator in avators" :key="avator.name" :name="avator.name" :img_src="avator.img_src" />
    </div>
</div>
</template>

<script>
import axios from "axios"

export default {
    name: "PokerGame",
    data() {
        return {
            games: [],
            avators: [{
                name: "Mike",
                img_src: "https://mdbootstrap.com/img/new/standard/nature/184.jpg",
            },{
                name: "Jane",
                img_src: "https://mdbootstrap.com/img/new/standard/nature/184.jpg",
            },],
        }
    },
    mounted() {
        this.getPlayers().then((res) => {console.log(res)})
        this.setGames().then((res) =>{})
    },
    methods: {
        async setGames() {
            this.games = []
            let games = await this.getGames()
            games = games.data
            console.log(games)
            for(let idx=0;idx<games.length;idx++) {
                this.games.push({
                    title: games[idx].name,
                    img_src: games[idx].img_url,
                    content: games[idx].content,
                    link: games[idx].link,
                })
            }
        },
        async getPlayers() {
            const players = axios.post("/api/players")
            return players
        },
        async getGames() {
            const games = axios.post("/api/games")
            return games
        },
    },
}
</script>
