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
          required
          suffix="m^3/s"
        ></v-text-field>
      </v-col>
      <v-col cols="3">
        <v-text-field
          v-model="latitude"
          label="River Mouth Latitude"
          placeholder="eg. -37"
          outlined
          required
          suffix="degrees"
        ></v-text-field>
      </v-col>
    </v-row>

    <v-row justify="center">
      <v-col cols="3">
        <v-text-field
          v-model="river_density"
          label="River Density"
          placeholder="eg. 1 (most rivers can be assumed to have density of 1 g/cm^3"
          outlined
          required
          suffix="g/cm^3"
        ></v-text-field>
      </v-col>
      <v-col cols="3">
        <v-text-field
          v-model="ocean_density"
          label="Ocean Density"
          placeholder="eg. 1.02"
          required
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

    <v-dialog v-model="showDataDialog" width="500px">
      <v-card>
        <v-card-title class="py-4">
          <h4>{{name}} Data</h4>
        </v-card-title>
        <v-card-text>
          <v-row>
            <v-col cols="6" class="text-center subtitle-1">
              <span>Width:</span>
            </v-col>
            <v-col cols="6" class="text-center subtitle-1">
              <span>{{payload.width}} meters</span>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="6" class="text-center subtitle-1">
              <span>Depth:</span>
            </v-col>
            <v-col cols="6" class="text-center subtitle-1">
              <span>{{payload.depth}} meters</span>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="6" class="text-center subtitle-1">
              <span>Velocity:</span>
            </v-col>
            <v-col cols="6" class="text-center subtitle-1">
              <span>{{payload.velocity}} meters per second</span>
            </v-col>
          </v-row>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="showDataDialog = false">Ok</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  name: "InputFields",
  data: () => ({
    GRAVITY: 9.81,
    name: null,
    latitude: null,
    ocean_density: null,
    river_density: null,
    volume_flux: null,
    showDataDialog: false,
    payload: {
      width: null,
      depth: null,
      velocity: null
    }
  }),
  methods: {
    get_width() {
      return (
        (8 * this._density_diff() * this.volume_flux) /
          Math.pow(Math.pow(this._rotation_rate(), 3)),
        1 / 4
      );
    },
    get_depth() {
      return (
        (2 * this._rotation_rate() * this.volume_flux) /
          Math.pow(this._density_diff()),
        1 / 2
      );
    },
    get_velocity() {
      return (
        (3 / Math.pow(2, 9 / 4)) *
        Math.pow(
          this._rotation_rate() * this.volume_flux * this._density_diff(),
          1 / 4
        )
      );
    },
    _rotation_rate() {
      let rotation_rate = 7.29 * Math.pow(10, -5);
      return (
        2 * rotation_rate * Math.abs(Math.sin(this.latitude * (Math.PI / 180)))
      );
    },
    _density_diff() {
      // can assume river water density is same as freshwater or 1 gram / cm^3
      return (
        (this.GRAVITY * (this.ocean_density - this.river_density)) /
        this.river_density
      );
    },
    round(num) {
      return Math.round(num * 100) / 100
    },
    onCalculate() {
      this.payload.width = this.round(this.get_width())
      this.payload.depth = this.round(this.get_depth())
      this.payload.velocity = this.round(this.get_velocity())

      this.showDataDialog = true;
    }
  }
};
</script>
