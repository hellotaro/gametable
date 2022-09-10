<template>
    <div>
        <div class="trump-card" :style="{backgroundColor: card.Color}">
            <p class="card-text">{{getSuiteText(card.Suite)}}{{card.Number}}</p>
        </div>
        <p v-if="card.Score">({{card.Score}})</p>
    </div>
</template>

<style lang="postcss" scoped>
.trump-card {
    @apply block rounded bg-gray-300 w-16 m-1;
    line-height: 120px;
    text-align: center;
}
.card-text {
    font-size: 26px;
}
</style>

<script>

export default {
    name: "TrumpCard",
    props: ["card", ],
    methods: {
        getSuiteText(suite) {
            if(suite == 0) { return "♠" }
            if(suite == 1) { return "♣" }
            if(suite == 2) { return "♡" }
            if(suite == 3) { return "♢" }
            return suite;
        },
    },
}
</script>
