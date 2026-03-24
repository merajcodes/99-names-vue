<template>
  <div class="bg-olive-800 py-2">
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
    item: {
      type: Object,
      required: true
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
</style>