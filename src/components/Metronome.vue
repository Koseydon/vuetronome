<template>
  <v-card  :dark="isDarkTheme" class="mx-auto" max-width="600">
    <v-toolbar flat dense>
      <v-toolbar-title>
        <span class="subheading">VUETRONOME</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn small :dark="isDarkTheme" depressed fab @click="toggleDark">
        <v-icon small>
          {{ isDark ? 'mdi-white-balance-sunny' : 'mdi-moon-waning-crescent' }}
        </v-icon>
      </v-btn>
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
      <v-slider v-model="metronome.bpm" :color="color" track-color="grey" always-dirty label="Tempo" min="40" max="218">
        <template v-slot:prepend>
          <v-icon :color="color" @click="bpmDecrement">
            mdi-minus
          </v-icon>
        </template>
        <template v-slot:append>
          <v-icon :color="color" @click="bpmIncrement">
            mdi-plus
          </v-icon>
        </template>
      </v-slider>
      <v-slider v-model="metronome.volumeRange" :color="color" track-color="grey" always-dirty label="Volume" min="0"
        max="100">
        <template v-slot:prepend>
          <v-icon :color="color" @click="bpmDecrement">
            mdi-volume-low
          </v-icon>
        </template>
        <template v-slot:append>
          <v-icon :color="color" @click="bpmIncrement">
            mdi-volume-high
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
      interval: null,
      isDark: false,
      metronome: {
        volumeRange: 100,
        bpm: 120,
        clickAudio: new Audio(beat),
        timer: null,
        isPlaying: false,
        toggle: function () {
          if (this.isPlaying === true) {
            let t1 = performance.now()
            this.clickAudio.play()
            this.clickAudio.volume = this.volumeRange / 100
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
      isDarkTheme(){
        if (this.isDark) return true
        return false
      },
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
      volumeDecrement() {
        this.metronome.volumeRange--
      },
      bolumeIncrement() {
        this.metronome.volumeRange++
      },
      bpmDecrement() {
        this.metronome.bpm--
      },
      bpmIncrement() {
        this.metronome.bpm++
      },
      toggle() {
        this.metronome.isPlaying = !this.metronome.isPlaying
        this.metronome.toggle()
      },
      toggleDark() {
        this.isDark = !this.isDark
      }
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