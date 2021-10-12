<template>
  <div class="main">
    <img alt="Vue logo" src="../assets/logo.png">
    <Base msg="Welcome to Your Vue.js App"/>
    <div class="about">
      <div class="">
        <h1>Welcome please to the FingersUp app</h1>
      </div>
      <div class="">
        <h5>FingersUp provides simple and useful interface to manage etudes you play on music instruments</h5>
      </div>
      <div class="">
        <h5>So you can start from
          <router-link to="/etude/create">
            creating
          </router-link>
          new etudes you want to use or look see our
          <router-link to="/etude/collection">collection</router-link>
        </h5>
      </div>
    </div>
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
<style scoped>
.main {
  display: grid;
  width: 100%;
  align-content: center;
  justify-items: center;
}
.about {
  display: grid;
  width: 57%;
  text-align: center;
}

</style>
