<template>
  <v-container>
    <v-row>
      <v-col cols="12" align="center">
        <h1>Marvel APP</h1>
      </v-col>
    </v-row>

    <v-row>
      <v-container>
        <v-row v-for="hero in heroes.results" v-bind:key="hero.id">
          <v-col cols="3" align="center">
            <v-card>
              <v-card-title primary-title>
                {{ hero.name }}
              </v-card-title>
              <v-card-text>
                <v-img v-bind:src="hero.thumbnail.path + '.' + hero.thumbnail.extension" width="350px">

                </v-img>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
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
        image: "",
      },
      heroes: [],
    }
  },
  methods: {
    loadHeroes() {
      const url = "https://gateway.marvel.com:443/v1/public/characters?limit=20&offset=0&ts=1&apikey=21a4da889edbe77bc6cb0a69e352ec87&hash=84fbb30897280ab8b56fcdd59fb4ffe2"
      this.$axios.get(url).then(response => {
        this.heroes = response.data.data
        console.log(this.heroes);
      }).catch(error => {
        console.log(error);
      })
    }
  }
}
</script>
