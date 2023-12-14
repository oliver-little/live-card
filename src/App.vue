<script>
import TextMorph from './components/TextMorph.vue';

export default {
  components: {TextMorph},
  data() {
    return {
      values: ["abc", "def", "ghi"],
      valuesIndex: 0,
    }
  }
}
</script>

<template>
  <main>
    <button @click="valuesIndex = valuesIndex + 1">Next</button>
    <TextMorph :inputText="values[valuesIndex]"/>
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
