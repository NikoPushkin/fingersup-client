<template>
  <div class="main">
    <div class="about">
      <div class="">
        <h1>Welcome please to the FingersUp app</h1>
      </div>
      <div class="">
        <h5>This web application provides simple and useful interface to manage etudes you play on music instruments</h5>
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
