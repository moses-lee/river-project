<template>
  <v-container fluid>
    <v-row justify="center">
      <v-col cols="6" class="text-center mt-12">
        <h4 class="display-3">River Project</h4>
      </v-col>
    </v-row>

    <v-row justify="center">
      <v-col cols="6" class="text-center mb-6">
        <a class="subtitle" href="https://github.com/moses-lee/river-project">github</a>
      </v-col>
    </v-row>

    <v-row justify="center" class="mt-6">
      <v-col cols="2">
        <v-text-field v-model="name" label="River Name" placeholder="eg. Rhine River" outlined></v-text-field>
      </v-col>
    </v-row>

    <v-row justify="center">
      <v-col cols="3">
        <v-text-field
          v-model="volume_flux"
          label="River Volume"
          placeholder="eg. 100"
          outlined
          suffix="m^3/s"
        ></v-text-field>
      </v-col>
      <v-col cols="3">
        <v-text-field
          v-model="latitude"
          label="River Mouth Latitude"
          placeholder="eg. -37"
          outlined
          suffix="degrees"
        ></v-text-field>
      </v-col>
    </v-row>

    <v-row justify="center">
      <v-col cols="3">
        <v-text-field
          v-model="river_density"
          label="River Density"
          placeholder="eg. 1"
          outlined
          suffix="g/cm^3"
        ></v-text-field>
      </v-col>
      <v-col cols="3">
        <v-text-field
          v-model="ocean_density"
          label="Ocean Density"
          placeholder="eg. 1.2"
          outlined
          suffix="g/cm^3"
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="6" class="text-center">
        <v-btn x-large color="cyan" @click="onCalculate" dark>Calculate</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "InputFields",
  data: () => ({
    ROTATION_RATE: 7.29 * 10 ** -5,
    GRAVITY: 9.81,
    name: null,
    latitude: null,
    ocean_density: null,
    river_density: null,
    volume_flux: null
  }),
  methods: {
    get_width() {
      return (
        ((8 * this._density_diff() * this.volume_flux) /
          this._rotation_rate() ** 3) **
        (1 / 4)
      );
    },
    get_depth() {
      return (
        ((2 * this._rotation_rate() * this.volume_flux) /
          this._density_diff()) **
        (1 / 2)
      );
    },
    get_velocity() {
      return (
        (3 / 2 ** (9 / 4)) *
        (this._rotation_rate() * this.volume_flux * this._density_diff()) **
          (1 / 4)
      );
    },
    _rotation_rate() {
      return (
        2 *
        this.ROTATION_RATE *
        Math.abs(Math.sin(this.latitude * (Math.PI / 180)))
      );
    },
    _density_diff() {
      // can assume river water density is same as freshwater or 1 gram / cm^3
      return (
        (this.GRAVITY * (this.ocean_density - this.river_density)) /
        this.river_density
      );
    },
    onCalculate() {
      this.$emit("clicked", "someValue");
    }
  }
};
</script>
