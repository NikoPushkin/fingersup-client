<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <Base msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Base from '@/components/base.vue'

export default {
  name: 'Home',
  components: {
    Base
  }
}
</script>
