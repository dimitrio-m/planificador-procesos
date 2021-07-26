<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <h1 class="text-h3 text-center my-6">
        Primero en llegar primero en ejecutar (FCFS)
      </h1>
      <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Illo, impedit. Eveniet a neque quas consequatur dolores! Modi itaque velit ea rerum autem fugit ducimus nemo omnis eos, dolor laborum tempora.</p>
      <p>Paso: {{ step }}</p>
      <h2 class="mt-6">
        Listos
      </h2>
      <v-row>
        <v-col
          v-for="n in 4"
          :key="n"
          cols="2"
        >
          <v-tooltip bottom color="blue-grey darken-1">
            <template #activator="{ on, attrs }">
              <v-card
                class="pa-2"
                tile
                outlined
                v-bind="attrs"
                v-on="on"
              >
                P{{ n }}
              </v-card>
            </template>
            <span>Bottom tooltip</span>
          </v-tooltip>
        </v-col>
      </v-row>
      <h2 class="mt-6">
        CPU
      </h2>
      <v-row>
        <v-col
          v-for="n in 1"
          :key="n"
          cols="2"
        >
          <v-tooltip bottom>
            <template #activator="{ on, attrs }">
              <v-card
                class="pa-2"
                tile
                outlined
                color="blue"
                v-bind="attrs"
                v-on="on"
              >
                P{{ n }}
              </v-card>
            </template>
            <span>Bottom tooltip</span>
          </v-tooltip>
        </v-col>
      </v-row>
      <h2 class="mt-6">
        Terminados
      </h2>
      <v-row>
        <v-col
          v-for="n in 4"
          :key="n"
          cols="2"
        >
          <v-tooltip bottom>
            <template #activator="{ on, attrs }">
              <v-card
                class="pa-2"
                tile
                outlined
                v-bind="attrs"
                v-on="on"
              >
                P{{ n }}
              </v-card>
            </template>
            <span>Bottom tooltip</span>
          </v-tooltip>
        </v-col>
      </v-row>
      <v-row>
        <v-btn class="mt-6" outlined to="/">
          <v-icon left>
            mdi-arrow-left
          </v-icon>
          Regresar
        </v-btn>
        <v-spacer />
        <v-btn v-show="!play && step > 0" class="mt-6 mr-2" color="warning" @click="reset">
          <v-icon left>
            mdi-stop
          </v-icon>
          Reiniciar
        </v-btn>
        <v-btn class="mt-6" :color="playButtonColor" @click="play = !play">
          <v-icon left>
            {{ playButtonIcon }}
          </v-icon>
          {{ playButtonText }}
        </v-btn>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data () {
    return {
      play: false,
      timer: null,
      step: 0
    }
  },
  computed: {
    playButtonIcon () {
      return this.play ? 'mdi-pause' : 'mdi-play'
    },
    playButtonText () {
      return this.play ? 'Pausar' : 'Iniciar'
    },
    playButtonColor () {
      return this.play ? 'error' : 'success'
    }
  },
  watch: {
    play () {
      if (this.play) {
        this.timer = setInterval(() => { this.step += 1 }, 1000)
      } else {
        clearInterval(this.timer)
        this.timer = null
      }
    }
  },
  methods: {
    reset () {
      this.step = 0
    }
  }
}
</script>

<style>

</style>
