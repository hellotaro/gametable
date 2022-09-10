<template>
<div class="card">
    <a href="#!">
        <img class="image-avator" :src="img_src" alt="" />
    </a>
    <div class="p-6">
        <h5 class="mb-2 text-xl font-medium text-gray-900">{{name}}</h5>
    </div>
</div>
</template>

<style lang="postcss" scoped>
.card {
    @apply m-1 max-w-sm rounded-lg bg-white shadow-lg;
}
.image-avator {
    width: 50%;
    height: 50%;
}
</style>

<script>

export default {
    name: "AvatorCard",
    props: ["name", "img_src", ],
}
</script>
