<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <h1 class="text-h3 text-center my-6">
        Primero trabajo mas corto (SJF)
      </h1>
      <p class="text-body-1 text-justify">
        Este algoritmo emplea una cola de "listos" donde llegan los procesos que están listos para ejecutar. Los procesos se organizan por tiempo de procesamiento restante de menor a mayor (menor sale primero).
      </p>
      <p class="text-body-1 text-justify">
        No hay expulsión, esto da a entender que una vez que un proceso esté ejecutandose en el procesador, se debe esperar a que se ejecute en su totalidad para poder pasar otro proceso a ejecución.
      </p>
      <p class="text-body-1 text-justify">
        En la siguiente simulación se tiene una probabilidad del 20% en cada paso para que aparezca un proceso nuevo en la cola de listos y cada proceso tiene un tiempo en procesador (burst time) entre uno y diez dado por un generador de números pseudoaleatorios con distribución aproximadamente uniforme, también hay una probabilidad de 10% para que un proceso ejecute una operación de entrada o salida mientras está en el cpu por un tiempo de entre uno y cinco pasos.
      </p>
      <p class="text-body-1">
        Tw = Tiempo de espera <br>
        Te = Tiempo de ejecución <br>
        Tp = Tiempo de procesamiento (burst time) <br>
        Tb = Tiempo de bloqueo (io burst time)
      </p>
      <v-text-field
        v-model="stepTime"
        :disabled="play"
        class="mt-6"
        label="Tiempo x paso en milisegundos"
        type="number"
        suffix="milisegundos (ms)"
      />
      <p class="text-body-1 font-weight-bold">
        Paso: {{ step }}
      </p>
      <Queue :process-list="ready" title="Listos" />
      <Queue :process-list="blocked" title="Bloqueados" />
      <Queue :process-list="cpu" title="CPU" />
      <Queue :process-list="finished" title="Terminados" />
      <v-row class="mt-6" no-gutters>
        <h2>Estadísticas</h2>
        <v-col cols="12">
          <p>Uso del CPU (C): {{ (cpuUsage * 100).toFixed(2) }}%</p>
        </v-col>
        <v-col cols="12">
          <p>Tiempo de espera (Tw) promedio: {{ averageWaitTime.toFixed(2) }} pasos</p>
        </v-col>
        <v-col cols="12">
          <p>Tiempo de bloqueo (Tb) promedio: {{ averageBlockTime.toFixed(2) }} pasos</p>
        </v-col>
        <v-col cols="12">
          <p>Tiempo de ejecución (Te) promedio: {{ averageExecutionTime.toFixed(2) }} pasos</p>
        </v-col>
        <v-col cols="12">
          <p>Procesos completados por paso (P): {{ finishedJobsRate.toFixed(4) }}</p>
        </v-col>
        <v-col cols="12">
          <p>Arribo de nuevos procesos por paso: {{ arrivalPerStep.toFixed(2) }} procesos</p>
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
  name: 'SJF',
  data () {
    return {
      play: false,
      timer: null,
      step: 0,
      ready: [],
      cpu: [],
      cpuIdle: 0,
      finished: [],
      blocked: [],
      n: 0,
      stepTime: 500
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
    },
    cpuUsage () {
      return (this.step - this.cpuIdle) / this.step || 0
    },
    averageWaitTime () {
      return this.finished.reduce((prev, curr) => prev + curr.waitTime, 0) / this.finished.length || 0
    },
    averageExecutionTime () {
      return this.finished.reduce((prev, curr) => prev + curr.waitTime + curr.cpuTime + curr.ioTime, 0) / this.finished.length || 0
    },
    finishedJobsRate () {
      return this.averageWaitTime ? 1 / this.averageWaitTime : 0
    },
    arrivalPerStep () {
      return this.n / this.step || 0
    },
    averageLongTermQueue () {
      return this.arrivalPerStep * this.averageWaitTime
    },
    averageBlockTime () {
      return this.finished.reduce((prev, curr) => prev + curr.ioTime, 0) / this.finished.length || 0
    }
  },
  watch: {
    play () {
      if (this.play) {
        // Iniciar Timer
        this.timer = setInterval(this.stepHandler, this.stepTime)
      } else {
        // Limpiar Timer
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
      this.blocked = []
      this.cpuIdle = 0
    },
    stepHandler () {
      // Agregar tiempo de espera
      this.ready = this.ready.map((p) => {
        p.waitTime += 1
        return p
      })
      // Evaluar si incluir un nuevo proceso a listo
      const shouldCreateNewProcess = Math.floor(Math.random() * 100) + 1 >= 80
      if (shouldCreateNewProcess) {
        this.n += 1
        this.ready.push({ id: this.n, burstTime: Math.floor(Math.random() * 10) + 1, waitTime: 0, cpuTime: 0, ioTime: 0 })
      }

      // Bajar tiempo bloqueado y mover a listo en caso de terminar
      this.blocked = this.blocked
        .map((p) => {
          p.ioBurstTime -= 1
          p.ioTime += 1
          return p
        })
        .filter((p) => {
          if (p.ioBurstTime === 0) {
            this.ready.push(p)
            return false
          } else {
            return true
          }
        })

      // Ordenar por burst time
      this.ready.sort((a, b) => a.burstTime - b.burstTime)

      // Evaluar si el CPU está vacío y pasar proceso FCFS
      // De lo contrario quemar tiempo en el proceso
      if (this.cpu.length === 0 && this.ready.length > 0) {
        this.cpu.push(this.ready.shift())
      } else if (this.cpu.length > 0 && this.cpu[0].burstTime === 0) {
        this.finished.push(this.cpu.pop())
      } else if (this.cpu.length > 0) {
        const shouldDoIO = Math.floor(Math.random() * 100) + 1 >= 90
        if (shouldDoIO) {
          this.cpu[0].ioBurstTime = Math.floor(Math.random() * 5) + 1
          this.blocked.push(this.cpu.pop())
        } else {
          this.cpu[0].burstTime -= 1
          this.cpu[0].cpuTime += 1
        }
      } else {
        this.cpuIdle += 1
      }
      // Aumentar el step cada ciclo
      this.step += 1
      if (this.step > 999) {
        this.play = false
      }
    }
  }
}
</script>

<style>

</style>
