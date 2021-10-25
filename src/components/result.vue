<template>
  <b-card-body>
    <b-alert show variant="info"
      >
      Etude <b>{{ etudeName }}</b> {{ action }}
      <div class="">
        <b-button variant="info">
          <router-link :to='redirectPath'
                       @click.native="$router.go()"
                       class="sub-link">
            Ok
          </router-link>
        </b-button>
      </div>
    </b-alert>
  </b-card-body>

</template>

<script>

export default {
  name: 'Result',
  props: {
    etudeName: {
      type: String,
      default: ''
    },
    action: {
      type: String,
      default: ''
    },
    redirectPath: {
      type: String,
      default: '/etude/collection'
    }
  },
  data() {
    return {
    }
  },
  methods: {
    update() {
      this.$forceUpdate()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.sub-link {
  color: white;
  text-decoration: none;
  width: 100%;
  height: 100%
}
</style>
