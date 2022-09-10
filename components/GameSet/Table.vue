<template>
<div>
    <h2>Pod: {{table.Pod}}</h2>
    <div class="flex justify-center">
        <GameSetTrumpCard v-for="(card,cidx) in table.Cards" :key="'card-'+cidx" :card="card"/>
    </div>
</div>
</template>

<script>
export default {
    name: "GameTable",
    props: ["table", ],
}
</script>
