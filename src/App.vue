<template>
  <h1 class="text-xl font-bold text-center text-amber-100 my-2">
    اسماء الحسنی
  </h1>
  <div class="flex justify-center items-center">    
    <p class="text-olive-500 mx-2 text-4xl">{{ matchedCount }} / 99</p>

    <input 
      v-model="guess" 
      placeholder="type a name" 
      @input="checkGuess"
      ref="guessInput"
      type="text"
      class="border border-olive-600 text-white my-2 mx-auto w-96 p-2 rounded-md focus:outline-none">

    <div class="flex space-x-2 text-white px-2">
      <button  @click="toggleTrainingMode" class="rounded-md bg-olive-700 px-4 py-1 text-olive-300">Train</button>
      <button @click="revealAll" class="rounded-md bg-olive-700 px-4 py-1 text-olive-300">Reveal</button>
    </div>

  </div>
  
  <div class="grid grid-cols-11 gap-2 px-2">    
    <Card
        v-for="item in names"
        :key="item.id"
        :item="item"
        :isTrainingMode="isTrainingMode"
        @reveal="revealCard"
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
      isTrainingMode: false,
      names: rawNames.map(item => ({
        ...item,
        matched: false,
        revealedManually: false,
        trainingEligible: false,
        flash: false
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
        const isMatch = item.allowed_spellings.some(spelling =>
          this.normalizeText(spelling) === normalizedGuess
        )

        if (isMatch) {
          foundMatch = true

          if (item.matched) {
            this.flashCard(item)
          } else {
            item.matched = true
            item.revealedManually = false
            item.trainingEligible = false
          }
        }
      })

      if (foundMatch) {
        this.guess = ''
      }
    },
    revealCard(id) {
      const item = this.names.find(n => n.id === id)
      if (item && !item.matched) {
        item.matched = true
        item.revealedManually = true
        item.trainingEligible = true
      }
    },
    revealAll() {
      this.names.forEach(item => {
        if (!item.matched) {
          item.matched = true
          item.revealedManually = true
          item.trainingEligible = true
        }
      })
    },
    toggleTrainingMode() {
      this.isTrainingMode = !this.isTrainingMode

      if (this.isTrainingMode) {
        // reset only training items for practice
        this.names.forEach(item => {
          if (item.trainingEligible) {
            item.matched = false
            item.revealedManually = false
          }
        })
      }
    },
    flashCard(item) {
      item.flash = false

      this.$nextTick(() => {
        item.flash = true

        setTimeout(() => {
          item.flash = false
        }, 500)
      })
    },
    handleKeydown(e) {
      // "/" key (Shift+/ also triggers same key)
      if (e.key === '/') {
        e.preventDefault() // prevents typing "/" in other places
        this.$refs.guessInput.focus()
      }
    }
  },
  mounted() {
    window.addEventListener('keydown', this.handleKeydown)
  },
  beforeUnmount() {
    window.removeEventListener('keydown', this.handleKeydown)
  },
}
</script>