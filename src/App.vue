<script setup lang="ts">
import AvatarGenerator from "./components/AvatarGenerator.vue"
</script>

<template>
    <h1 class="site-title">Создай свою лего аватарку!</h1>
    <AvatarGenerator />
</template>

<style>
.site-title {
    font-size: 36px;
    font-weight: 800;
    text-align: center;
    padding: 12px 0;
}

.avatar-generator {
    width: 100%;
    height: 100%;
}

@media screen and (max-width: 620px) {
    .site-title {
        font-size: 24px;
        padding: 8px 0;
    }
}
</style>