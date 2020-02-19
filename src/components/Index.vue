<template>
  <v-card dark class="mx-auto" max-width="600">
    <v-toolbar flat dense>
      <v-toolbar-title>
        <span class="subheading">VUETRONOME</span>
      </v-toolbar-title>
    </v-toolbar>
    <v-card-text>
      <v-row class="mb-4" justify="space-between">
        <v-col class="text-left">
          <span class="display-3 font-weight-light" v-text="metronome.bpm"></span>
          <span class="subheading font-weight-light mr-1">BPM</span>
          <v-fade-transition>
            <v-avatar v-if="metronome.isPlaying" :color="color" :style="{
                animationDuration: animationDuration
              }" class="mb-1 v-avatar--metronome" size="12"></v-avatar>
          </v-fade-transition>
        </v-col>
        <v-col class="text-right">
          <v-btn :color="color" dark depressed fab @click="toggle">
            <v-icon large>
              {{ metronome.isPlaying ? 'mdi-pause' : 'mdi-play' }}
            </v-icon>
          </v-btn>
        </v-col>
      </v-row>

      <v-slider v-model="metronome.bpm" :color="color" track-color="grey" always-dirty min="40" max="218">
        <template v-slot:prepend>
          <v-icon :color="color" @click="decrement">
            mdi-minus
          </v-icon>
        </template>

        <template v-slot:append>
          <v-icon :color="color" @click="increment">
            mdi-plus
          </v-icon>
        </template>
      </v-slider>
    </v-card-text>
  </v-card>
</template>

<script>
import beat from '../audio/beat.wav'

  export default {
    data: () => ({
      bpm: 40,
      interval: null,
      metronome: {
        clickAudio: new Audio(beat),
        timer: null,
        isPlaying: false,
        toggle: function () {
          if (this.isPlaying === true) {
            let t1 = performance.now()
            this.clickAudio.play()
            let interval = 60000 / this.bpm

            this.timer = setTimeout(() => {
              console.log(performance.now() - t1)
              this.toggle()
            }, interval)
          } else {
            clearInterval(this.timer)
            this.timer = null
          }
        },
      }
    }),

    computed: {
      color() {
        if (this.metronome.bpm < 100) return 'indigo'
        if (this.metronome.bpm < 125) return 'teal'
        if (this.metronome.bpm < 140) return 'green'
        if (this.metronome.bpm < 175) return 'orange'
        return 'red'
      },
      animationDuration() {
        return `${60 / this.metronome.bpm}s`
      },
    },

    methods: {
      decrement() {
        this.metronome.bpm--
      },
      increment() {
        this.metronome.bpm++
      },
      toggle() {
        this.metronome.isPlaying = !this.metronome.isPlaying
        this.metronome.toggle()
      },
    },
  }
</script>

<style>
  @keyframes metronome-example {
    from {
      transform: scale(.5);
    }

    to {
      transform: scale(1);
    }
  }

  .v-avatar--metronome {
    animation-name: metronome-example;
    animation-iteration-count: infinite;
    animation-direction: alternate;
  }
</style>