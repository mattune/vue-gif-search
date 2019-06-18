<template>
  <div id="app">
    <GIFkeyboard/>
  </div>
</template>

<script>
import GIFkeyboard from './components/GIFkeyboard.vue'

export default {
  name: 'app',
  components: {
    GIFkeyboard
  }
}
</script>

<style lang="scss" scoped>
#app {
  padding: 100px;

  textarea {
    border: 1px solid #000;
    display: block;
    margin-bottom: 20px;
    padding: 15px;
    width: 500px;
    height: 300px;
  }

  button {
    border: 1px solid #000;
    border-radius: 10px;
    padding: 10px;
    display: block;
  }
}


#sampleTA {
  width: 200px;
  height: 200px;
  border: 1px solid #000;
}
</style>
