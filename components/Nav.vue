<template>
<nav class="nav-bar">
    <div class="nav-container container-fluid">
        <div class="collapse navbar-collapse flex-grow items-center" id="navbarSupportedContent">
            <a class="text-xl text-black" href="/">GameCenter</a>
            <ul class="flex flex-col pl-0 mr-auto navbar-nav list-style-none">
                <li class="nav-item px-2">
                    <a class="nav-link active" aria-current="page" href="/account">Account</a>
                </li>
                <li class="nav-item px-2">
                    <a class="nav-link active" aria-current="page" href="/poker">Poker</a>
                </li>
                <li class="nav-item px-2">
                    <a class="nav-link active" aria-current="page" href="/chinchiro">Chinchiro</a>
                </li>
            </ul>
        </div>
    </div>
</nav>
</template>

<style lang="postcss" scoped>
.nav-bar {
    @apply relative w-full flex flex-wrap items-center justify-between py-4 bg-gray-100 text-gray-500 shadow-lg;
}
.nav-container {
    @apply w-full flex flex-wrap items-center justify-between px-6;
}
</style>

<script>
export default {
    name: "Nav",
}
</script>
