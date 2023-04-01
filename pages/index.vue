<template>
  <v-container>
    <v-row>
      <v-col cols="12" align="center">
        <h1>Marvel APP</h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="12" align="center">
        <v-btn color="red darken-4" dark @click="loadLessHeroes()" :disabled="!btnLess" :loading="loading">Cargar menos personajes</v-btn>
        <v-btn color="red darken-4" dark @click="loadMoreHeroes()" :loading="loading">Cargar más personajes</v-btn>
      </v-col>
    </v-row>

    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-title class="text-h5 red darken-4" text-xs-center>
          {{ myHero.name }}
          <v-spacer></v-spacer>
          <v-icon @click="dialog = false">mdi-close</v-icon>
        </v-card-title>

        <v-card-text>
          <v-img :src="myHero.imageUrl" width="450px" height="350px"></v-img>
          <p>
            {{ myHero.description }}
          </p>
          <span class="title">Cantidad de Comics: </span>
          {{ myHero.comicsQuant }}
          <br>
          <span class="title">Cantidad de Series: </span>
          {{ myHero.seriesQuant }}
          <br>
          <span class="title">Cantidad de Historias: </span>
          {{ myHero.storiesQuant }}
          <br>
          <span class="title">Cantidad de Eventos: </span>
          {{ myHero.eventsQuant }}
          <br>

          <v-expansion-panels popout>
            <v-expansion-panel>
              <v-expansion-panel-header class="red darken-4">
                Series de {{ myHero.name }}
              </v-expansion-panel-header>
              <v-expansion-panel-content elevation="10" v-for="(serie, i) in myHero.series" :key="i">
                {{ serie.name }}
              </v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="white" text @click="dialog = false">
            Cerrar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-row>
      <v-col cols="3" align="center" v-for="hero in heroes.results" v-bind:key="hero.id">
        <v-card>
          <v-card-title primary-title>
            {{ hero.name }}
          </v-card-title>
          <v-card-text>
            <v-img v-bind:src="hero.thumbnail.path + '.' + hero.thumbnail.extension" width="350px" height="350px">

            </v-img>
          </v-card-text>
          <v-card-actions>
            <v-btn color="red darken-4" dark @click="showDescription(hero)">Ver más</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>

  </v-container>
</template>

<script>
export default {
  layout: "blank",
  beforeMount() {
    this.loadHeroes()
  },
  data() {
    return {
      myHero: {
        name: "",
        imageUrl: "",
        description: "",
        comicsQuant: 0,
        seriesQuant: 0,
        storiesQuant: 0,
        eventsQuant: 0,
        series: []
      },
      heroes: [],
      dialog: false,
      limit: 20,
      offset: 0,
      btnLess: false,
      loading: false,
    }
  },
  methods: {
    loadHeroes() {
      const limit = this.limit.toString()
      const offset = this.offset.toString()
      const url = `https://gateway.marvel.com:443/v1/public/characters?limit=${limit}&offset=${offset}&ts=1&apikey=21a4da889edbe77bc6cb0a69e352ec87&hash=84fbb30897280ab8b56fcdd59fb4ffe2`
      this.loading = true
      this.$axios.get(url).then(response => {
        this.heroes = response.data.data
      }).catch(error => {
        console.log(error);
      }).finally(()=>{
        this.loading = false
      })
    },

    loadMoreHeroes(){
      if (this.offset == 20) {
        this.btnLess = false
      }
      this.offset += 20
      this.loadHeroes()
      this.btnLess = true
    },

    loadLessHeroes(){
      if (this.offset == 20) {
        this.offset -= 20
        this.loadHeroes()
        this.btnLess = false
      } else {
        this.offset -= 20
        this.btnLess = true
        this.loadHeroes()
      }
    },

    showDescription(hero) {
      if (hero.description == "") {
        this.myHero.description = "No tiene descripción"

      } else {
        this.myHero.description = `${hero.description}`
      }
      this.dialog = true
      this.myHero.name = hero.name
      this.myHero.imageUrl = `${hero.thumbnail.path}.${hero.thumbnail.extension}`
      this.myHero.comicsQuant = hero.comics.available
      this.myHero.seriesQuant = hero.series.available
      this.myHero.storiesQuant = hero.stories.available
      this.myHero.eventsQuant = hero.events.available
      this.myHero.series = hero.series.items
      this.myHero.series = this.myHero.series.slice(0, 3)
    }
  }
}
</script>

<style scoped>
.title {
  font-weight: bold;
  font-size: large;
  margin-left: 35px;
}
</style>