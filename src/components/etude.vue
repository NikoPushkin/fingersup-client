<template>
  <b-container class="etude-container" style="max-width: 400px">
    <b-card>
      <b-row class="mb-2">
        <b-col class="text-left" sm="12"><p class="text-item">Etude {{ msg }}</p></b-col>
      </b-row>

      <!-- NAME input -->
      <b-row class="justify-content-md-center mb-4">
        <b-col sm="12">
          <b-input-group>
            <b-input-group-text style="width: 80px;"  slot="prepend">Title</b-input-group-text>
            <b-form-input id="name-input" :state="nameState()" v-model="name" trim type="text" placeholder="Am scale"></b-form-input>
          </b-input-group>
          <b-form-invalid-feedback id="name-input">
            Enter at least 3 letters
          </b-form-invalid-feedback>
        </b-col>
      </b-row>

      <b-row class="mb-2">
        <b-col sm="12" class="text-left">
          <p class="text-item">Tempo settings</p>
        </b-col>
      </b-row>
      <!-- MIN TEMPO INPUT -->
      <b-row class="justify-content-md-center mb-3">
        <b-col sm="12">
          <b-input-group>
            <b-input-group-text style="width: 80px;" slot="prepend">Min</b-input-group-text>
            <b-input-group-text slot="append">{{ minTempo }}</b-input-group-text>
            <b-form-input min="10" max="300" type="range" v-model="minTempo"></b-form-input>
          </b-input-group>
        </b-col>
      </b-row>

      <!-- CURRENT TEMPO INPUT -->
      <b-row class="justify-content-md-center mb-3">
        <b-col sm="12">
          <b-input-group>
            <b-input-group-text style="width:80px" slot="prepend">Current</b-input-group-text>
            <b-input-group-text slot="append">{{ currentTempo }}</b-input-group-text>
            <b-form-input min="10" max="300" type="range" v-model="currentTempo"></b-form-input>
          </b-input-group>
        </b-col>
      </b-row>

      <!-- GOAL TEMPO INPUT -->
      <b-row class="justify-content-md-center mb-4">
        <b-col sm="12">
          <b-input-group>
            <b-input-group-text style="width: 80px;" slot="prepend">Goal</b-input-group-text>
            <b-input-group-text slot="append">{{ maxTempo }}</b-input-group-text>
            <b-form-input min="10" max="300" type="range" v-model="maxTempo"></b-form-input>
          </b-input-group>
        </b-col>
      </b-row>

      <!-- NOTES -->
      <b-row>
        <b-col sm="12" class="text-left">
          <p class="text-item">Select notes</p>
        </b-col>
      </b-row>
      <div class="note-list mb-2 mt-2">
        <b-button variant="outline-primary" class="mb-1 mr-1" size="sm" v-for="note in notes" :key="note.num" @click="note.num = selectedNotes.length, addNote(note)">
          {{ note.name }}
        </b-button>
      </div>
      <b-row>
        <b-col sm="12" class="text-left">
          <p class="text-item">Selected notes</p>
        </b-col>
      </b-row>
      <div class="added-note-list mt-2">
        <b-button v-for="note, index in selectedNotes" :key="note.name + index" size="sm" variant="primary" class="mr-1 mb-1" @click="deleteNote(note)">
          {{ note.name }}
        </b-button>
      </div>


      <b-row class="justify-content-md-center mt-3 mb-3">
        <b-col sm="6">
          <b-button style="width: 100%" :disabled="generalState() == false || nameState() == false" variant="outline-primary">Create</b-button>
        </b-col>
        <b-col sm="6">
          <button style="width: 100%"
              class="btn btn-info"
              @click="openInfo"
              >Info
          </button>
        </b-col>
      </b-row>


      <div class="collapse mt-2" id="collapseeExample">
        <div class="card card-body important-text-left">
          <p :key="field" v-for='(description, field) in info'>
            <b>{{ field }}</b>: {{ description }}
          </p>
        </div>
      </div>


    </b-card>
  </b-container>
</template>

<script>
export default {
  name: 'Etude',
  props: {
    msg: String
  },
  data() {
    return {
      minTempo: 50,
      maxTempo: 100,
      currentTempo: 80,
      name: '',
      notes: [],
      selectedNotes: [],
      info: {'minimum tempo': 'the most convenience tempo to play etude for you',
             'current tempo': 'int - tempo you currently practice',
             'goal tempo': 'tempo you want to reach'}
    }
  },
  created() {
    this.getNotes()
  },
  methods: {
    openInfo() {
      let elem = document.getElementById('collapseeExample');

      if (elem.style.display == "block") {
        elem.style.display = "none"
      } else {
        elem.style.display = "block"
      }

    },
    generalState() {
      return this.selectedNotes.length > 2 ? true : false
    },

    nameState() {
      return this.name.length > 2 && this.name.length < 20 ? true : false
    },

    async getNotes() {
      var resp = await fetch('http://localhost:5004/note/get')
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        data.forEach(note => {
          note.flat = false;
          note.sharp = false;
          this.notes.push(note)
        })

        this.addAccidental(data, '#')
        this.addAccidental(data, 'b')
      });
      console.log(resp);
    },

    addAccidental(notes, symbol) {
      let noAccidentalNotes = { '#': ['E', 'B'],
                                'b': ['F', 'C'] };

      notes.map(note => {
        if (!noAccidentalNotes[symbol].includes(note.name)) {
          let newNote;
          if (symbol == '#') {
            newNote = { 'name': note.name + symbol,
                        'note_id': note.note_id,
                        'sharp': true,
                        'flat': false };
          } else if (symbol == 'b') {
            newNote = { 'name': note.name + symbol,
                        'note_id': note.note_id,
                        'sharp': false,
                        'flat': true };
          }
          this.notes.push(newNote);
        }
      })
    },

    deleteNote(note) {
      this.selectedNotes = this.selectedNotes.filter((item) => {return item !== note})
    },
    addNote(note) {
      let newNote = {name: note.name,
                     'note_id': note.note_id,
                     num: this.selectedNotes.length,
                     flat: note.flat,
                     sharp: note.sharp}
      this.selectedNotes.push(newNote)
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.text-item {
  font-size: 1.8vh;
  margin: 0 !important;
  color: #2c3e50;
  letter-spacing: 0.1vh
}
.note-list {
  display: flex;
  max-width: 100%;
  flex-wrap: wrap;
  justify-content: space-between;
}

.important-text-left {
  text-align: left !important
}


</style>
