<template>
  <b-container class="etude-container" style="max-width: 400px">
    <b-card no-body class="mb-1" v-bind:key="etude.name" v-for="etude, index in etudeList">
      <b-card-header header-tag="header" class="p-1" role="tab">
        <b-button block
                  @click="openEtudeInfo(index)"
                  variant="outline-primary"
                  >
                  {{ etude.name }}
        </b-button>
      </b-card-header>

      <b-collapse :id="'etude-' + index" accordion="my-accordion" role="tabpanel">
        <b-card-body>
          <b-card-text>Min tempo: <code>{{ etude.tempo_min }}</code></b-card-text>
          <b-card-text>Current tempo: <code>{{ etude.tempo_current }}</code></b-card-text>
          <b-card-text>Max tempo: <code>{{ etude.tempo_max }}</code></b-card-text>
          <b-card-text>Notes: <code>{{ etude.notes }}</code></b-card-text>
        </b-card-body>
      </b-collapse>

    </b-card>
  </b-container>

</template>

<script>
import { environment } from '../environment';

export default {
  name: 'Collection',
  data() {
    return {
      etudeList: [],
    }
  },
  created() {
    this.getEtudeCollection();
  },
  methods: {
    async getEtudeCollection() {
      let resp = await fetch(environment.url + 'etude/full');
      this.etudeList = await resp.json();
      console.log(this.etudeList);
    },

    openEtudeInfo(index) {
      this.$root.$emit('bv::toggle::collapse', 'etude-' + index);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
