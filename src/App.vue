<template>
  <v-app>
    <v-content>
      <Metronome/>
    </v-content>
  </v-app>
</template>

<script>
import Metronome from './components/Metronome';

export default {
  name: 'App',

  components: {
    Metronome,
  },

  data: () => ({
    //
  }),
};
</script>

