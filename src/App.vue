<template>
  <div id="app">
    <Panels uri="ws://localhost:4444/graph"/>
  </div>
</template>

<script>
import Panels from './components/panels.vue'

export default {
  name: 'app',
  components: {
    Panels
  }
}
</script>
<style>
body {
  background-color: transparent;
}
html {
  background-image: url('./assets/bg-poly-slate-1.png');
  background-repeat:  no-repeat;
  background-position:  center center;
  background-attachment: fixed;
  background-size: cover;
}
</style>