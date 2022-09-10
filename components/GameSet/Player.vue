<template>
<div>
    <h2>{{player.Name}}({{player.State}})</h2>
    <h4>Bet:{{player.Bet}}/Fund:{{player.Fund}}</h4>
    <h4>{{player.Strategy}}</h4>
    <h4 v-if="player.Info != ''">{{player.Info}}</h4>
    <div class="flex justify-center">
        <GameSetTrumpCard v-for="(card,cidx) in player.Hand.Cards" :key="'card-'+cidx" :card="card"/>
    </div>
    <div v-if="is_me" class="flex justify-center">
        <button type="button" class="card-button" v-on:click="clickGameCheckBtn">Check</button>
        <button type="button" class="card-button" v-on:click="clickGameHoldBtn">Hold</button>
        <button type="button" class="card-button" v-on:click="clickGameRaiseBtn">Raise</button>
    </div>
    <div v-if="is_me" class="flex justify-center">
        <input v-model="raised_value" placeholder="raise fee" />
    </div>
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
export default {
    name: "GamePlayer",
    props: ["player", "actionHandler","is_me"],
    data() {
        return {
            raised_value: "",
        }
    },
    methods: {
        clickGameCheckBtn() {
            const meta = {
                ActionType: "action",
                Value: "check",
            }
            this.actionHandler(meta)
        },
        clickGameHoldBtn() {
            const meta = {
                ActionType: "action",
                Value: "hold",
            }
            this.actionHandler(meta)
        },
        clickGameRaiseBtn() {
            const meta = {
                ActionType: "action",
                Value: "raise",
                Meta: this.raised_value
            }
            this.actionHandler(meta)
        },
    },
}
</script>
