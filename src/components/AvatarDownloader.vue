<script setup lang="ts">
function download() {
    const canvas:any =  document.querySelector(".generator__canvas");
    (document.querySelector("#downloader") as any).download = "Lego Avatar.png";
    (document.querySelector("#downloader") as any).href = canvas.toDataURL("image/png")
}
</script>

<template>
  <a href="#" id="downloader" @click="download">
    <slot />
  </a>
</template>