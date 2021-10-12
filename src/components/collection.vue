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
        <b-card-body v-if="deletedEtude">

        <b-alert show variant="info"
          >
          Etude <b>{{ deletedEtude }}</b> removed
          <div class="">
            <b-button variant="info" @click="cleareDeletedEtude">Ok</b-button>
          </div>
        </b-alert>

        </b-card-body>

        <b-card-body v-else>
          <b-card-text>Min tempo: <code>{{ etude.tempo_min }}</code></b-card-text>
          <b-card-text>Current tempo: <code>{{ etude.tempo_current }}</code></b-card-text>
          <b-card-text>Max tempo: <code>{{ etude.tempo_max }}</code></b-card-text>
          <b-card-text>Notes:
            <div class="note-list mb-2 mt-2">
              <b-button variant="outline-primary"
                        class="mb-1 mr-1"
                        size="sm"
                        v-for="note in etude.notes"
                        :key="note.num"
                        >
                {{ note.name }}
              </b-button>
            </div>
          </b-card-text>

          <div class="edit-buttons">
            <b-button variant="light">
              <router-link :to="{path: '/etude/update',
                                 query: {
                                   updateQuery: {
                                     minTempo: etude.tempo_min,
                                     maxTempo: etude.tempo_max,
                                     currentTempo: etude.tempo_current,
                                     name: etude.name,
                                     notes: etude.notes
                                   }
                                  }
                                }"
              >Edit
              </router-link>
            </b-button>
            <b-button variant="danger" @click="deleteEtude(etude.etude_id, etude.name)">Delete</b-button>
          </div>

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
      deletedEtude: ''
    }
  },
  created() {
    this.getEtudeCollection();
  },
  methods: {
    async getEtudeCollection() {
      let resp = await fetch(environment.url + 'etude/full');
      this.etudeList = await resp.json();
    },
    async deleteEtude(id, name) {
      let resp = await fetch(environment.url + 'etude/delete/' + id, {
                              method: 'DELETE'
                            });
      let data = await resp.json();
      if (data.length) {
        this.deletedEtude = name;
      }
    },
    cleareDeletedEtude() {
      this.deletedEtude = '';
      this.getEtudeCollection();
    },

    openEtudeInfo(index) {
      this.$root.$emit('bv::toggle::collapse', 'etude-' + index);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.edit-buttons {
  display: flex;
    width: 100%;
    justify-content: space-between;
}

</style>
