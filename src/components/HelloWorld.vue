<template>
  <div>
    <v-card class="mx-auto mt-8" max-width="400">
      <v-card-text>
        <v-row align="center">
          <v-col cols="12"
            ><v-slider
              v-model="rnn_steps"
              label="Length RNN:"
              :thumb-size="24"
              thumb-label="always"
            ></v-slider>
          </v-col>
        </v-row>
        <v-row align="center">
          <v-col cols="12"
            ><v-slider
              v-model="rnn_temperature"
              label="Chaos:"
              :thumb-size="24"
              thumb-label="always"
              min="1"
              max="30"
            ></v-slider>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions>
        <v-btn id="btn" @click="playNewMedlody()">{{ text }}</v-btn>
        <v-btn id="btn" @click="playVAE()">{{ textVae }}</v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import * as mm from "@magenta/music/es6";

const ORIGINAL_TWINKLE_TWINKLE: mm.INoteSequence = {
  notes: [
    { pitch: 60, quantizedStartStep: 0, quantizedEndStep: 2, program: 0 },
    { pitch: 60, quantizedStartStep: 2, quantizedEndStep: 4, program: 0 },
    { pitch: 67, quantizedStartStep: 4, quantizedEndStep: 6, program: 0 },
    { pitch: 67, quantizedStartStep: 6, quantizedEndStep: 8, program: 0 },
    {
      pitch: 69,
      quantizedStartStep: 8,
      quantizedEndStep: 10,
      program: 0,
    },
    {
      pitch: 69,
      quantizedStartStep: 10,
      quantizedEndStep: 12,
      program: 0,
    },
    {
      pitch: 67,
      quantizedStartStep: 12,
      quantizedEndStep: 16,
      program: 0,
    },
    {
      pitch: 65,
      quantizedStartStep: 16,
      quantizedEndStep: 18,
      program: 0,
    },
    {
      pitch: 65,
      quantizedStartStep: 18,
      quantizedEndStep: 20,
      program: 0,
    },
    {
      pitch: 64,
      quantizedStartStep: 20,
      quantizedEndStep: 22,
      program: 0,
    },
    {
      pitch: 64,
      quantizedStartStep: 22,
      quantizedEndStep: 24,
      program: 0,
    },
    {
      pitch: 62,
      quantizedStartStep: 24,
      quantizedEndStep: 26,
      program: 0,
    },
    {
      pitch: 62,
      quantizedStartStep: 26,
      quantizedEndStep: 28,
      program: 0,
    },
    {
      pitch: 60,
      quantizedStartStep: 28,
      quantizedEndStep: 32,
      program: 0,
    },
    {
      pitch: 67,
      quantizedStartStep: 32,
      quantizedEndStep: 34,
      program: 0,
    },
    {
      pitch: 67,
      quantizedStartStep: 34,
      quantizedEndStep: 36,
      program: 0,
    },
    {
      pitch: 65,
      quantizedStartStep: 36,
      quantizedEndStep: 38,
      program: 0,
    },
    {
      pitch: 65,
      quantizedStartStep: 38,
      quantizedEndStep: 40,
      program: 0,
    },
    {
      pitch: 64,
      quantizedStartStep: 40,
      quantizedEndStep: 42,
      program: 0,
    },
    {
      pitch: 64,
      quantizedStartStep: 42,
      quantizedEndStep: 44,
      program: 0,
    },
    {
      pitch: 62,
      quantizedStartStep: 44,
      quantizedEndStep: 48,
      program: 0,
    },
    {
      pitch: 67,
      quantizedStartStep: 48,
      quantizedEndStep: 50,
      program: 0,
    },
    {
      pitch: 67,
      quantizedStartStep: 50,
      quantizedEndStep: 52,
      program: 0,
    },
    {
      pitch: 65,
      quantizedStartStep: 52,
      quantizedEndStep: 54,
      program: 0,
    },
    {
      pitch: 65,
      quantizedStartStep: 54,
      quantizedEndStep: 56,
      program: 0,
    },
    {
      pitch: 64,
      quantizedStartStep: 56,
      quantizedEndStep: 58,
      program: 0,
    },
    {
      pitch: 64,
      quantizedStartStep: 58,
      quantizedEndStep: 60,
      program: 0,
    },
    {
      pitch: 62,
      quantizedStartStep: 60,
      quantizedEndStep: 64,
      program: 0,
    },
    {
      pitch: 60,
      quantizedStartStep: 64,
      quantizedEndStep: 66,
      program: 0,
    },
    {
      pitch: 60,
      quantizedStartStep: 66,
      quantizedEndStep: 68,
      program: 0,
    },
    {
      pitch: 67,
      quantizedStartStep: 68,
      quantizedEndStep: 70,
      program: 0,
    },
    {
      pitch: 67,
      quantizedStartStep: 70,
      quantizedEndStep: 72,
      program: 0,
    },
    {
      pitch: 69,
      quantizedStartStep: 72,
      quantizedEndStep: 74,
      program: 0,
    },
    {
      pitch: 69,
      quantizedStartStep: 74,
      quantizedEndStep: 76,
      program: 0,
    },
    {
      pitch: 67,
      quantizedStartStep: 76,
      quantizedEndStep: 80,
      program: 0,
    },
    {
      pitch: 65,
      quantizedStartStep: 80,
      quantizedEndStep: 82,
      program: 0,
    },
    {
      pitch: 65,
      quantizedStartStep: 82,
      quantizedEndStep: 84,
      program: 0,
    },
    {
      pitch: 64,
      quantizedStartStep: 84,
      quantizedEndStep: 86,
      program: 0,
    },
    {
      pitch: 64,
      quantizedStartStep: 86,
      quantizedEndStep: 88,
      program: 0,
    },
    {
      pitch: 62,
      quantizedStartStep: 88,
      quantizedEndStep: 90,
      program: 0,
    },
    {
      pitch: 62,
      quantizedStartStep: 90,
      quantizedEndStep: 92,
      program: 0,
    },
    {
      pitch: 60,
      quantizedStartStep: 92,
      quantizedEndStep: 96,
      program: 0,
    },
  ],
  tempos: [{ time: 0, qpm: 60 }],
  quantizationInfo: { stepsPerQuarter: 4 },
  totalQuantizedSteps: 96,
};
const music_rnn = new mm.MusicRNN(
  "https://storage.googleapis.com/magentadata/js/checkpoints/music_rnn/melody_rnn"
);
const music_vae = new mm.MusicVAE(
  "https://storage.googleapis.com/magentadata/js/checkpoints/music_vae/trio_4bar"
);
const Player = new mm.Player();

export default Vue.extend({
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      text: "Play RNN",
      textVae: "Play VAE",
      rnn_steps: 20,
      rnn_temperature: 1,
    };
  },
  methods: {
    playNewMedlody() {
      if (Player.isPlaying()) {
        this.text = "Play";
        Player.stop();
        return;
      } else {
        this.text = "Stop";
        // The model expects a quantized sequence, and ours was unquantized:
        const qns = mm.sequences.quantizeNoteSequence(
          ORIGINAL_TWINKLE_TWINKLE,
          4
        );
        music_rnn
          .continueSequence(
            ORIGINAL_TWINKLE_TWINKLE,
            this.rnn_steps,
            this.rnn_temperature
          )
          .then((sample) => Player.start(sample))
          .then(() => (this.text = "Play"));
      }
    },
    playVAE() {
      if (Player.isPlaying()) {
        this.textVae = "Play Vae"
        Player.stop()
        return
      } else {
        this.textVae = "Stop";
        music_vae
          .sample(1, this.rnn_temperature/10)
          .then((sample) => Player.start(sample[0]))
          .then(() => this.textVae = "Play Vae")
      }
    },
  },
  mounted() {
    music_rnn.initialize();
    music_vae.initialize();
  },
});
</script>

<style scoped>
</style>
