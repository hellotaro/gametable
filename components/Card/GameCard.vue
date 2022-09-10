<template>
<div class="card">
    <a href="#!">
        <img class="rounded-t-lg max-h-300" :src="img_src" alt="" />
    </a>
    <div class="p-6">
        <h5 class="mb-2 text-xl font-medium text-gray-900">{{title}}</h5>
        <p class="mb-4 text-base text-gray-700">{{content}}</p>
        <NuxtLink class="card-button" :to="link">{{link}}</NuxtLink>
    </div>
</div>
</template>

<style lang="postcss" scoped>
.card {
    @apply m-1 w-1/4 rounded-lg bg-white shadow-lg;
}
.card-button {
    @apply inline-block rounded bg-blue-600 px-6 py-2.5 text-xs font-medium uppercase leading-tight text-white shadow-md transition duration-150 ease-in-out;
    &:hover {
        @apply bg-blue-700;
    }
}
</style>

<script>

export default {
    name: "GameCard",
    data() {
        return {
            data: "mycard",
        }
    },
    props: ["title", "img_src", "content", "link"],
}
</script>
