<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <h1 class="text-h3 text-center my-6">
        Algoritmos de Planificación del CPU
      </h1>
      <h2>
        No Expulsivos
      </h2>
      <v-btn
        v-for="item in nonExpulsive"
        :key="item.link"
        class="my-4"
        block
        color="primary"
        dark
        nuxt
        :to="item.link"
        :disabled="item.disabled"
      >
        {{ item.text }}
      </v-btn>
      <h2>
        Expulsivos
      </h2>
      <v-btn
        v-for="item in expulsive"
        :key="item.link"
        class="my-4"
        outline
        block
        color="primary"
        dark
        nuxt
        :to="item.link"
        :disabled="item.disabled"
      >
        {{ item.text }}
      </v-btn>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data () {
    return {
      nonExpulsive: [
        { text: 'Primero en llegar primero en ejecutar (FCFS)', link: '/non-expulsive/fcfs', disabled: false },
        { text: 'Primero el trabajo mas corto (SJF)', link: '/non-expulsive/sjf', disabled: true },
        { text: 'Selección aleatoria', link: '/non-expulsive/random', disabled: true }
      ],
      expulsive: [
        { text: 'Turno rotatorio (round robin)', link: '/roundrobin', disabled: true },
        { text: 'Primero el menor tiempo restante (SRTF)', link: '/srtf', disabled: true },
        { text: 'Planificación basada en prioridades', link: '/priorities', disabled: true },
        { text: 'Colas Multinivel', link: '/multilevel', disabled: true }
      ]
    }
  }
}
</script>
