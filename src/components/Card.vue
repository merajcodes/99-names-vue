<template>
  <div class="py-2 cursor-pointer" 
  :class="[cardClass, { 'flash-card': item.flash }]" 
  @click="handleClick">
  	<div v-if="!item.matched" class="text-4xl text-center my-4 text-olive-700">{{ item.id }}</div>
  	<transition	name="reveal">
  		<div v-if="item.matched" class="mt-2">  			
		    <h2 class="text-xl text-center text-amber-400">{{ item.arabic_name }}</h2>
		    <!-- <h2 class="text-center text-olive-500"id>{{ item.english_name }}</h2> -->
		    <h2 class="text-center text-olive-500 text-xs">{{ item.english_meaning }}</h2>
  		</div>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'Card',
  props: {
    item: Object,
    isTrainingMode: Boolean
  },
  computed: {
    cardClass() {
      return this.item.revealedManually
        ? 'bg-red-950'
        : 'bg-olive-800'
    }
  },
  methods: {
    handleClick() {
      if (this.isTrainingMode && !this.item.trainingEligible) return
      this.$emit('reveal', this.item.id)
    }
  }
}
</script>

<style scoped>
.reveal-enter-active,
.reveal-leave-active {
  transition: opacity 0.4s ease, transform 0.4s ease;
}

.reveal-enter-from,
.reveal-leave-to {
  opacity: 0;
  transform: translateY(8px);
}

.reveal-enter-to,
.reveal-leave-from {
  opacity: 1;
  transform: translateY(0);
}

.flash-card {
  animation: flashCard 0.5s ease;
}

@keyframes flashCard {
  0% {
    transform: scale(1);
    box-shadow: 0 0 0 rgba(251, 191, 36, 0);
  }
  50% {
    transform: scale(1.05);
    box-shadow: 0 0 12px rgba(251, 191, 36, 0.8);
  }
  100% {
    transform: scale(1);
    box-shadow: 0 0 0 rgba(251, 191, 36, 0);
  }
}
</style>