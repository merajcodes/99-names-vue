<template>
  <h1 class="text-xl font-bold text-center text-amber-400">
    اسماء الحسنی
  </h1>
  <div class="flex justify-center items-center">    
    <input 
      v-model="guess" 
      placeholder="type a name" 
      @input="checkGuess"
      type="text" 
      name="" 
      class="border border-olive-600 text-white my-2 mx-auto w-96 px-2">
      <p class="text-olive-500 mx-2">{{ matchedCount }} / 99</p>
  </div>
  <div class="grid grid-cols-11 gap-2">    
    <Card
        v-for="item in names"
        :key="item.id"
        :item="item"
      />
  </div>
</template>

<script>
import Card from './components/Card.vue'
import rawNames from './assets/99_names.json'

export default {
  name: 'App',
  components: {
    Card
  },
  data() {
    return {
      guess:'',
      names: rawNames.map(item => ({
        ...item,
        matched: false
      }))
    }
  },
  computed: {
    matchedCount() {
      return this.names.filter(item => item.matched).length
    }
  },
  methods: {
    normalizeText(value) {
      return value
        .toLowerCase()
        .trim()
        .replace(/\s+/g, ' ')
    },
    checkGuess() {
      const normalizedGuess = this.normalizeText(this.guess)

      if (!normalizedGuess) return

      let foundMatch = false

      this.names.forEach(item => {
        const isMatch = item.allowed_spellings.some(spelling => {
          return this.normalizeText(spelling) === normalizedGuess
        })

        if (isMatch) {
          item.matched = true
          foundMatch = true
        }
      })

      if (foundMatch) {
        this.guess = ''
      }
    }
  }
}
</script>