<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
  yo_soy_dani: String,
})

const count = ref(20)
</script>

<template>
  <h1>{{ msg }}</h1>
  <h2>{{ yo_soy_dani }}</h2>

  <div class="card">
    <button type="button" @click="count=count*3">count is {{ count }}</button>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Learn more about IDE Support for Vue in the
    <a
      href="https://vuejs.org/guide/scaling-up/tooling.html#ide-support"
      target="_blank"
      >Vue Docs Scaling up Guide</a
    >.
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
