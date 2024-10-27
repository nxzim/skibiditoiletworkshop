<script setup>
import { ref } from 'vue'

const selectedCar = ref(null)
const showModal = ref(false)
const isPlaying = ref(false)

const images = [
  {
    id: 1,
    main: "https://pictures.porsche.com/rtt/iris?COSY-EU-100-1711coMvsi60AAt5FwcmBEgA4qP8iBUDxPE3Cb9pNXkBuNYdMGF4tl3U0%25z8rMHIspbWvanYb%255y%25oq%25vSTmjMXD4qAZeoNBPUSfUx4RmHlCgI7Zl2dioCxkF%25vUqCNwuWXsOw3meV6iTCj%25zhp4NdTxf4KxJhvauDFmOcuLAlWRDdV2M",
    hover: "https://www.cnet.com/a/img/resize/5b84fb196e6ff6db4148ccbc844a1d8c5f62ec3e/hub/2022/10/02/d3866672-8254-46d2-9844-87aef1c0fe1e/2023-porsche-911-gt3-rs-001.jpg?auto=webp&width=768",
    title: "Porsche 911 GT3 RS",
    description: "The Porsche 911 GT3 RS is the most focused, track-ready variant of the 911. With its naturally aspirated 4.0L flat-six engine producing 518 hp, extensive use of carbon fiber, and advanced aerodynamics including a massive rear wing, it represents the pinnacle of Porsche's track-focused engineering.",
    specs: {
      engine: "4.0L Naturally Aspirated Flat-Six",
      power: "518 hp",
      acceleration: "0-60 mph in 3.0 seconds",
      topSpeed: "184 mph"
    },
    engineSound: "/sounds/engine-rev-continuous-sound-effect-for-editing-128-ytshorts.savetube.me.mp3"
  },
  {
    id: 2,
    main: "https://toyotagazooracing.com/-/media/TMC/tgr/global/contents/pressrelease/images/2024/0712-01/0712-01.jpg",
    hover: "https://toyotagazooracing.com/-/media/TMC/tgr/global/contents/pressrelease/images/2024/0712-01/0712-02.jpg",
    title: "Toyota GR86",
    description: "The Toyota GR86 is a nimble and lightweight sports car designed for enthusiasts who appreciate precision handling and balanced performance. With its naturally aspirated flat-four engine and rear-wheel-drive layout, the GR86 offers an engaging driving experience, perfect for both spirited driving and daily use. Refined for the new generation, the GR86 provides a responsive and connected feel, appealing to purists who love the thrill of a true sports coupe.",
    specs: {
      engine: "2.4L Naturally Aspirated Flat-Four",
      power: "228 hp",
      acceleration: "0-60 mph in 6.1 seconds",
      topSpeed: "140 mph"
    },
    engineSound: "https://example.com/toyota-gr86.mp3"
  },
  {
    id: 3,
    main: "https://www.telegraph.co.uk/content/dam/luxury/2020/11/19/Nismo-1_trans_NvBQzQNjv4BqZgEkZX3M936N5BQK4Va8RWtT0gK_6EfZT336f62EI5U.jpg?imwidth=680",
    hover: "https://hips.hearstapps.com/hmg-prod/images/2024-nissan-gt-r-121-1673621285.jpg?crop=0.922xw:1.00xh;0.0501xw,0&resize=980:*",
    title: "Nissan GT-R R35",
    description: "The Nissan GT-R R35 is a legendary sports car that combines brutal power with advanced technology. Known for its outstanding performance and precision, it delivers a supercar experience at a competitive price. With a twin-turbo V6 engine and an advanced all-wheel-drive system, the GT-R R35 excels on both the track and the road, making it an iconic choice for enthusiasts.",
    specs: {
      engine: "3.8L Twin-Turbo V6",
      power: "565 hp",
      acceleration: "0-60 mph in 2.9 seconds",
      topSpeed: "196 mph"
    },
    engineSound: "https://example.com/nissan-gtr.mp3"
  }
]

const openModal = (car) => {
  selectedCar.value = car
  showModal.value = true
  isPlaying.value = false
}

const closeModal = () => {
  showModal.value = false
  selectedCar.value = null
  isPlaying.value = false
  stopSound()
}

const audio = ref(null)

const playEngineSound = () => {
  if (!audio.value) {
    audio.value = new Audio(selectedCar.value.engineSound)
  }
  
  if (!isPlaying.value) {
    audio.value.play()
    isPlaying.value = true
  } else {
    stopSound()
  }
}

const stopSound = () => {
  if (audio.value) {
    audio.value.pause()
    audio.value.currentTime = 0
    isPlaying.value = false
  }
}
</script>

<template>
  <div class="min-h-screen bg-[#DBE2EF]">
    <!-- Header -->
    <header class="fixed w-full z-50 bg-[#DBE2EF] bg-opacity-95 backdrop-blur-md shadow-md">
      <h1 class="text-5xl md:text-5xl font-bold font-sans tracking-wider text-[#112D4E] text-center py-6">
        GALLERIA
      </h1>
    </header>

    <!-- Gallery Container -->
    <div class="container mx-auto px-4 pt-32">
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <!-- Gallery Items -->
        <div v-for="image in images" 
             :key="image.id" 
             class="gallery-item group">
          <div class="bg-[#F9F7F7]/80 rounded-lg p-4 transition-all duration-300 hover:shadow-lg hover:shadow-[#3F72AF]/30 backdrop-blur-sm cursor-pointer"
               @click="openModal(image)">
            <!-- Main Image -->
            <img :src="image.main" 
                 class="w-full h-auto object-cover rounded-lg mb-0 transition-all duration-300"
                 :alt="image.title">
            
            <!-- Hover Image -->
            <div class="hover-image-container h-0 overflow-hidden transition-all duration-300 group-hover:h-auto">
              <img :src="image.hover" 
                   class="w-full h-auto object-cover rounded-lg mt-4 transition-all duration-300"
                   :alt="`${image.title} - Alternate View`">
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Modal -->
    <div v-if="showModal" 
         class="fixed inset-0 bg-[#112D4E]/40 backdrop-blur-sm flex items-center justify-center z-50 p-4"
         @click="closeModal">
      <div class="bg-[#F9F7F7] rounded-lg max-w-2xl w-full p-6 text-[#112D4E] shadow-xl transform transition-all"
           @click.stop>
        <!-- Modal Header -->
        <div class="flex justify-between items-center mb-4">
          <div class="flex items-center gap-4">
            <h2 class="text-2xl font-bold text-[#3F72AF]">{{ selectedCar?.title }}</h2>
            <!-- Engine Rev Button -->
            <button 
              @click="playEngineSound"
              class="flex items-center gap-2 px-4 py-2 bg-[#3F72AF] hover:bg-[#112D4E] text-[#F9F7F7] rounded-lg transition-colors duration-300"
            >
              <svg 
                xmlns="http://www.w3.org/2000/svg" 
                :class="{'animate-pulse': isPlaying}"
                class="w-5 h-5" 
                fill="none" 
                viewBox="0 0 24 24" 
                stroke="currentColor"
              >
                <path 
                  v-if="!isPlaying"
                  stroke-linecap="round" 
                  stroke-linejoin="round" 
                  stroke-width="2" 
                  d="M15.536 8.464a5 5 0 010 7.072M17.95 6.05a8 8 0 010 11.9M4 10v4a1 1 0 001 1h3l3.5 3.5a1 1 0 001.5 0V5.5a1 1 0 00-1.5 0L8 9H5a1 1 0 00-1 1z"
                />
                <path 
                  v-else
                  stroke-linecap="round" 
                  stroke-linejoin="round" 
                  stroke-width="2" 
                  d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"
                />
              </svg>
              {{ isPlaying ? 'Stop Rev' : 'Rev Engine' }}
            </button>
          </div>
          <button @click="closeModal" 
                  class="text-[#DBE2EF] hover:text-[#3F72AF] transition-colors">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"/>
            </svg>
          </button>
        </div>

        <!-- Modal Content -->
        <div class="space-y-4">
          <img :src="selectedCar?.main" 
               class="w-full h-auto rounded-lg mb-4" 
               :alt="selectedCar?.title">
          
          <p class="text-[#112D4E]">{{ selectedCar?.description }}</p>

          <!-- Specifications -->
          <div class="mt-4">
            <h3 class="text-xl font-semibold text-[#3F72AF] mb-2">Specifications</h3>
            <div class="grid grid-cols-2 gap-4">
              <div v-for="(value, key) in selectedCar?.specs" 
                   :key="key" 
                   class="bg-[#DBE2EF] p-3 rounded-lg">
                <span class="text-[#112D4E]/70 text-sm">{{ key.charAt(0).toUpperCase() + key.slice(1) }}</span>
                <p class="text-[#112D4E] font-semibold">{{ value }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.gallery-item {
  perspective: 1000px;
}

.hover-image-container {
  transition: height 0.3s ease-in-out;
}

.gallery-item:hover .hover-image-container {
  height: auto;
}

/* Modal Animation */
.modal-enter-active,
.modal-leave-active {
  transition: all 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
  transform: scale(0.9);
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: .5; }
}

.animate-pulse {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}
</style>