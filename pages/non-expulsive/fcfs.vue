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
      <v-row v-if="ready.length > 0">
        <v-col
          v-for="p in ready"
          :key="p.id"
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
                P{{ p.id }}
              </v-card>
            </template>
            <span>Burst Time: {{ p.burstTime }}</span>
            <span>Wait Time: {{ p.waitTime }}</span>
          </v-tooltip>
        </v-col>
      </v-row>
      <h2 class="mt-6">
        CPU
      </h2>
      <v-row v-if="cpu.length > 0">
        <v-col
          v-for="p in cpu"
          :key="p.id"
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
                P{{ p.id }}
              </v-card>
            </template>
            <span>Burst Time: {{ p.burstTime }}</span>
            <span>Wait Time: {{ p.waitTime }}</span>
          </v-tooltip>
        </v-col>
      </v-row>
      <h2 class="mt-6">
        Terminados
      </h2>
      <v-row v-if="finished.length > 0">
        <v-col
          v-for="p in finished"
          :key="p.id"
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
                P{{ p.id }}
              </v-card>
            </template>
            <span>Burst Time: {{ p.burstTime }}</span>
            <span>Wait Time: {{ p.waitTime }}</span>
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
      step: 0,
      ready: [],
      cpu: [],
      cpuIdle: 0,
      finished: [],
      n: 0
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
        // Iniciar Timer
        this.timer = setInterval(() => {
          // Agregar tiempo de espera
          this.ready = this.ready.map((p) => {
            p.waitTime += 1
            return p
          })
          // Evaluar si incluir un nuevo proceso a listo
          const randomInt = Math.floor(Math.random() * 10) + 1
          if (randomInt > 6) {
            this.n += 1
            this.ready.push({ id: this.n, burstTime: Math.floor(Math.random() * 6) + 1, waitTime: 0 })
          }
          // Evaluar si el CPU está vacío y pasar proceso FCFS
          // De lo contrario quemar tiempo en el proceso
          if (this.cpu.length === 0 && this.ready.length > 0) {
            this.cpu.push(this.ready.shift())
          } else if (this.cpu.length > 0 && this.cpu[0].burstTime === 0) {
            this.finished.push(this.cpu.pop())
          } else if (this.cpu.length > 0) {
            this.cpu[0].burstTime -= 1
          } else {
            this.cpuIdle += 1
          }
          // Aumentar el step cada ciclo
          this.step += 1
        }, 1000)
      } else {
        clearInterval(this.timer)
        this.timer = null
      }
    }
  },
  methods: {
    reset () {
      this.step = 0
      this.n = 0
      this.ready = []
      this.cpu = []
      this.finished = []
      this.cpuIdle = 0
    }
  }
}
</script>

<style>

</style>
