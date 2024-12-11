<template>
  <form class="calculator">
    <div class="calculator__item">
      <label for="windSpeed">Wind level:</label>
      <input
        class="calculator__input"
        type="range"
        id="windSpeed"
        v-model="windSpeed"
        min="1"
        max="10"
      />
      <span>{{ windSlogan }}</span>
    </div>

    <div class="calculator__item">
      <label for="distractionLevel">Distraction level:</label>
      <input
        class="calculator__input"
        type="range"
        id="distractionLevel"
        v-model="distractionLevel"
        min="1"
        max="10"
      />
      <span>{{ distractionScenario }}</span>
    </div>

    <div class="calculator__item">
      <label for="terrainDifficulty">Terrain difficulty level:</label>
      <input
          class="calculator__input"
          type="range"
          id="terrainDifficulty"
          v-model="terrainDifficulty"
          min="1"
          max="10"
      />
      <canvas class="calculator__chart" ref="terrainCanvas" width="300" height="150"></canvas>
    </div>

    <div class="calculator__item">
      <label for="hatSecure">Hat secure level:</label>
      <input
        class="calculator__input"
        type="range"
        id="hatSecure"
        v-model="hatSecure"
        min="1"
        max="10"
        value="1"
      />
      <img :src="hatImage" :alt="'Hat security level ' + hatSecure" width="100" />
    </div>

    <button class="calculator__button" type="button" @click="calculateProbability">
      Calculate
    </button>

    <dialog class="calculator__dialog" ref="resultDialog">
      <div class="calculator__result">
        <span v-if="hatSecure === '7'">Chance of losing your hat:<br> 0%, because you're a wizard!</span>
        <span v-else v-html="resultText"></span>
        <button class="calculator__button-reset" type="button" @click="resetCalculator">
          Calculate again
        </button>
      </div>
    </dialog>
  </form>
</template>

<script>
export default {
  data() {
    return {
      windSpeed: 1,
      distractionLevel: 1,
      terrainDifficulty: 1,
      hatSecure: 1,
      resultText: '',
    };
  },
  watch: {
    terrainDifficulty(newVal) {
      this.updateCanvas(newVal);
    },
  },
  mounted() {
    this.updateCanvas(this.terrainDifficulty);
  },
  computed: {
    windSlogan() {
      const slogans = [
        "🍃 Calm breeze",
        "💨 Gentle puff",
        "🌬️ Light gust",
        "🌄 Refreshing zephyr",
        "🪁 Not bad for kite flying",
        "🤠 Hold onto your hat!",
        "🌪️ Blustery winds incoming!",
        "⛈️ Storm warning!",
        "🌊 Near hurricane!",
        "🌀🎩💨 Hats don't stand a chance!",
      ];
      return slogans[this.windSpeed - 1];
    },
    distractionScenario() {
      const scenarios = [
        "🔍 Focus is sharp",
        "🔈 Mild background noise",
        "👋🏽 Someone waved at you",
        "☎️ Phone buzzing",
        "🐶 Dog barking",
        "🎶 Music blasting",
        "😂 Friend telling a joke",
        "🚸 Kids running around",
        "☕ Dropping your coffee",
        "🎪 Full circus act!",
      ];
      return scenarios[this.distractionLevel - 1];
    },
    hatImage() {
      return `Images/secure-hat-${this.hatSecure}.jpeg`;
    },
  },
  methods: {
    updateCanvas(terrainValue) {
      const canvas = this.$refs.terrainCanvas;
      const ctx = canvas.getContext('2d');
      const width = canvas.width;
      const height = canvas.height;
      const baseHeight = height * 0.6;
      const roughness = terrainValue * 10;

      ctx.clearRect(0, 0, width, height);

      ctx.fillStyle = '#4c566a'; // Grey color for the terrain
      ctx.beginPath();
      ctx.moveTo(0, height);

      for (let x = 0; x <= width; x += width / 20) {
        const randomOffset = (Math.random() - 0.5) * roughness;
        const y = Math.max(0, Math.min(baseHeight + randomOffset, height));
        ctx.lineTo(x, y);
      }

      ctx.lineTo(width, height);
      ctx.closePath();
      ctx.fill();
    },
    calculateProbability() {
      let probability;
      probability = Math.min(
          100,
          Math.max(
              0,
              5 + // Baseline risk
              this.windSpeed ** 2.25 * 0.9 + // Dominant wind effect
              this.terrainDifficulty * 0.4 + // Minor terrain effect
              this.distractionLevel * 0.6 // Minor distraction effect
          ) * (1 - this.hatSecure / 20) // Hat security reduces risk
      );
      this.resultText = `Chance of losing your hat:<br> ${probability.toFixed(1)}%`;

      this.$refs.resultDialog.showModal();
    },
    resetCalculator() {
      this.$refs.resultDialog.close();
    },
  },
};
</script>

<style scoped>
/* Add styles for this component or use CSS import */
</style>
