<template>
  <v-app>
    <v-container fluid>
      <v-row justify="center">
        <v-col cols="6" class="text-center mt-12">
          <h4 class="display-3">River Project</h4>
        </v-col>
      </v-row>
      
      <v-row justify="center">
        <v-col cols="6" class="text-center">
          <a class="subtitle mr-4" href="https://tomrocksmaths.files.wordpress.com/2019/06/crawfordtj-thesis.pdf">Based on works by Tom Crawford</a>
          <a class="subtitle ml-4" href="https://github.com/moses-lee/river-project">github</a>
        </v-col>
      </v-row>

      <v-row justify="center">
        <v-col cols="12">
          <InputFields @clicked="showValues"></InputFields>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
import InputFields from "./components/InputFields";

export default {
  name: "App",

  components: {
    InputFields
  }
};
</script>
