<template>
  <div id="app">
    <Index />
  </div>
</template>

<script>
import Index from './components/index.vue'

export default {
  name: 'App',
  components: { 
    Index
  }
}
</script>

<style>
.father {
  width: 1200px;
  margin: 0 auto;
}
.content {
  text-align: center;
}
</style>
