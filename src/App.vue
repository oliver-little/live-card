<script>
import TextMorph from './components/TextMorph.vue';

export default {
  components: {TextMorph},
  data() {
    return {
      timerEnabled: true,
      timerCount: 1,
      timerGap: 1000,
      morphSpeed: 500,
      kanjiValues: ["", "千", "千と", "千と千", "千と千尋", "千と千尋の", "千と千尋の神", "千と千尋の神隠", "千と千尋の神隠し"],
      kanjiValuesLoop: ["千と千尋の神隠し", "千と千尋の神隠し.", "千と千尋の神隠し..", "千と千尋の神隠し..."],
      kanjiValuesIndex: 0,
      started: false,
      inKanjiLoop: false,
      fullStop: false,
      lastTimeout: null,
    }
  },
  computed: {
    displayValue() {
      if (!this.started) {
        return this.fullStop ? "." : "";
      }
      if (this.inKanjiLoop) {
        return this.kanjiValuesLoop[this.kanjiValuesIndex];
      }
      else {
        return this.kanjiValues[this.kanjiValuesIndex];
      }
    }
  },
  methods: {
    start() {
      this.started = true;
      this.timerGap = 250;
      this.morphSpeed = 125;
      this.timerEnabled = true;
      this.timerCount++;
    },
    nextState() {
      // Start state
      if (!this.started) {
        this.fullStop = !this.fullStop;
      }
      // Kanji loop state - end of loop
      else if (this.inKanjiLoop && this.kanjiValuesLoop.length == this.kanjiValuesIndex + 1) {
        this.kanjiValuesIndex = 0;
      }
      // Kanji loop start
      else if (this.kanjiValues.length == this.kanjiValuesIndex + 1) {
        this.timerGap = 1000;
        this.morphSpeed = 250;
        this.inKanjiLoop = true;
        this.kanjiValuesIndex = 0;
      }
      // Otherwise increment
      else {
        this.kanjiValuesIndex++;
      }
    }
  },
  watch: {
    timerCount: {
        handler(value) {
          if (value > 0 && this.timerEnabled) {
            if (this.lastTimeout) {
              clearTimeout(this.lastTimeout);
            }
            this.lastTimeout = setTimeout(() => {
              this.timerCount++;
            }, this.timerGap);
          }

          this.nextState();
        },
        immediate: true
    }
  }
}
</script>

<template>
  <main>
    <button @click="start()">Start</button>
    <TextMorph :morphSpeed="morphSpeed / 1000" :inputText="displayValue"/>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
