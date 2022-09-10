<template>
<div>
    <button @click="postChinchiroActionNext">Chinchiro</button>
</div>
</template>

<style lang="postcss" scoped>
.card-button {
    @apply inline-block rounded bg-blue-600 px-6 py-2.5 text-xs font-medium uppercase leading-tight text-white shadow-md transition duration-150 ease-in-out;
    &:hover {
        @apply bg-blue-700;
    }
}
</style>

<script>
import axios from "axios"

export default {
    name: "ChinchiroGame",
    data() {
        return {
            name: "",
            password: "",
            fund: "",
        }
    },
    mounted() {
        
    },
    methods: {
        postChinchiroActionNext() {
            let meta = {}
            meta = {
                ActionType: "action",
                Value: "roll",
            }
            this.postChinchiroAction(meta).then((res) => {
                console.log(res)
            })
        },
        async postChinchiroAction(actionMeta) {
            const pokerGame = axios.post("/api/game/chinchiro/action", actionMeta)
            return pokerGame
        },
    },
}
</script>
