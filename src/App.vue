<template>
  <v-app>
    <InputFields @clicked="showValues"></InputFields>

  </v-app>
</template>

<script>
import InputFields from './components/InputFields';


export default {
  name: 'App',

  components: {
    InputFields,
  },
  methods: {
    showValues(value) {
      document.write(value);
    }
  }
};
</script>
