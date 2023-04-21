<script setup lang="ts">
defineProps({
  color: {
    color: String,
    default: "blue",
    validation: () => [
      "blue",
      "blue-transparent",
      "red",
      "red-transparent",
    ]
  }
})
</script>

<template>
  <button 
    v-if="color === 'red'"
    :color="color"
    class="button button-red"
    >
    <slot />
  </button>

  <button 
    v-else-if="color === 'red-transparent'"
    :color="color"
    class="button button-red_transparent"
    >
    <slot />
  </button>

  <button 
    v-else-if="color === 'blue-transparent'"
    :color="color"
    class="button button-blue_transparent"
    >
    <slot />
  </button>

  <button 
    v-else
    :color="color"
    class="button button-blue"
    >
    <slot />
  </button>
</template>

<style>
.button {
  width: 200px;
  height: 45px;
  background-color: var(--button-blue);
  transition: 0.25s all;
  border: none;
  border-radius: 8px;
  opacity: .75;
  cursor: pointer;

  text-align: center;
  font-size: 16px;
  color: var(--button-text-color);
}

.button:hover, .button:active {
  opacity: 1;
}

.button-blue {
  background-color: var(--button-blue);
}

.button-blue_transparent {
  border: 2px solid var(--button-blue);
  background-color: #00000000;

  color: var(--button-text-transparent);
}

.button-blue_transparent:hover, .button-blue_transparent:active {
  background-color: var(--button-blue-transparent-hover);
}

.button-red {
  background-color: var(--button-red);
}

.button-red_transparent {
  border: 2px solid var(--button-red);
  background-color: #00000000;

  color: var(--button-text-transparent);
}

.button-red_transparent:hover, .button-red_transparent:active {
  background-color: var(--button-red-transparent-hover);
}

</style>