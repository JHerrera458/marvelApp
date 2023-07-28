<!-- eslint-disable space-before-function-paren -->
<template>
  <v-container>
    <v-row>
      <v-spacer />
      <v-col cols="3">
        <v-text-field
          v-model="heroName"
          label="Search hero by name"
          append-icon="mdi-magnify"
          prepend-icon="mdi-close"
          @click:append="loadHeroes(heroName)"
          @click:prepend="eraseSearch()"
        />
      </v-col>
      <v-spacer />
    </v-row>
    <v-row>
      <v-col cols="12" align="center">
        <v-btn
          color="red darken-4"
          dark
          :disabled="!btnLess"
          :loading="loading"
          width="150"
          @click="loadLessHeroes()"
        >
          Previous Page
        </v-btn>
        <v-btn color="red darken-4" dark :loading="loading" width="150" @click="loadMoreHeroes()">
          Load Heroes
        </v-btn>
      </v-col>
    </v-row>

    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-title class="text-h5 red darken-4" text-xs-center>
          {{ myHero.name }}
          <v-spacer />
          <v-icon @click="dialog = false">
            mdi-close
          </v-icon>
        </v-card-title>

        <v-card-text>
          <v-img :src="myHero.imageUrl" width="450px" height="350px" />
          <p>
            {{ myHero.description }}
          </p>
          <span class="title">Comics Quantity: </span>
          {{ myHero.comicsQuant }}
          <br>
          <span class="title">Series Quantity: </span>
          {{ myHero.seriesQuant }}
          <br>
          <span class="title">Histories Quantity: </span>
          {{ myHero.storiesQuant }}
          <br>
          <span class="title">Events Quantity: </span>
          {{ myHero.eventsQuant }}
          <br>

          <v-expansion-panels popout>
            <v-expansion-panel>
              <v-expansion-panel-header class="red darken-4">
                <span style="font-weight: bold"> {{ myHero.name }} </span> Series
              </v-expansion-panel-header>
              <v-expansion-panel-content v-for="(serie, i) in myHero.series" :key="i" elevation="10">
                {{ serie.name }}
              </v-expansion-panel-content>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-card-text>

        <v-divider />

        <v-card-actions>
          <v-spacer />
          <v-btn color="white" text @click="dialog = false">
            Cerrar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-row>
      <v-col v-for="hero in heroes.results" :key="hero.id" cols="3" align="center">
        <v-card>
          <v-card-title primary-title>
            {{ hero.name }}
          </v-card-title>
          <v-card-text>
            <v-img :src="hero.thumbnail.path + '.' + hero.thumbnail.extension" width="350px" height="350px" />
          </v-card-text>
          <v-card-actions>
            <v-btn color="red darken-4" dark @click="showDescription(hero)">
              Details...
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  layout: 'blank',
  data () {
    return {
      myHero: {
        name: '',
        imageUrl: '',
        description: '',
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
      heroName: ''
    }
  },
  beforeMount () {
    this.loadHeroes(this.heroName)
  },
  methods: {
    loadHeroes (heroName) {
      const limit = this.limit.toString()
      const offset = this.offset.toString()
      let url = ''
      if (heroName === '') {
        url = `https://gateway.marvel.com:443/v1/public/characters?limit=${limit}&offset=${offset}&ts=1&apikey=21a4da889edbe77bc6cb0a69e352ec87&hash=84fbb30897280ab8b56fcdd59fb4ffe2`
      } else {
        url = `https://gateway.marvel.com:443/v1/public/characters?nameStartsWith=${heroName}&limit=100&offset=0&ts=1&apikey=21a4da889edbe77bc6cb0a69e352ec87&hash=84fbb30897280ab8b56fcdd59fb4ffe2`
      }
      this.loading = true
      this.$axios.get(url).then((response) => {
        this.heroes = response.data.data
      }).catch(() => {
        alert('Hubo un error cargando los personajes')
      }).finally(() => {
        this.loading = false
      })
    },

    loadMoreHeroes () {
      if (this.offset === 20) {
        this.btnLess = false
      }
      this.offset += 20
      this.loadHeroes(this.heroName)
      this.btnLess = true
    },

    loadLessHeroes () {
      if (this.offset === 20) {
        this.offset -= 20
        this.loadHeroes(this.heroName)
        this.btnLess = false
      } else {
        this.offset -= 20
        this.btnLess = true
        this.loadHeroes(this.heroName)
      }
    },

    eraseSearch () {
      this.heroName = ''

      this.limit = 20
      this.offset = 0
      this.loadHeroes(this.heroName)
      this.btnLess = false
    },

    showDescription (hero) {
      if (hero.description === '') {
        this.myHero.description = 'No tiene descripción'
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
