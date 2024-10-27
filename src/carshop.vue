<script setup>

import { ref } from 'vue'

const brandFilter = ref('all')
const yearFilter = ref('all')
const priceFilter = ref('all')

const cars = ref([
  {
    id: 1,
    brand: 'mercedes',
    model: 'S-Class',
    year: '2024',
    description: 'The epitome of luxury with cutting-edge technology and supreme comfort',
    image: '/api/placeholder/400/250',
    price: 110000,
    specs: ['4.0L V8', 'AWD', '496 HP']
  },
  {
    id: 2,
    brand: 'bmw',
    model: '7 Series',
    year: '2023',
    description: 'A masterpiece of automotive engineering and luxury',
    image: '/api/placeholder/400/250',
    price: 95000,
    specs: ['3.0L I6', 'AWD', '375 HP']
  },
  {
    id: 3,
    brand: 'audi',
    model: 'A8',
    year: '2024',
    description: 'Where innovation meets sophistication',
    image: '/api/placeholder/400/250',
    price: 88000,
    specs: ['3.0L V6', 'Quattro', '335 HP']
  },
  {
    id: 4,
    brand: 'mercedes',
    model: 'E-Class',
    year: '2023',
    description: 'The perfect blend of comfort and performance',
    image: '/api/placeholder/400/250',
    price: 65000,
    specs: ['2.0L I4', 'RWD', '255 HP']
  },
  {
    id: 5,
    brand: 'bmw',
    model: '5 Series',
    year: '2022',
    description: 'Dynamic performance meets business class',
    image: '/api/placeholder/400/250',
    price: 55000,
    specs: ['2.0L I4', 'RWD', '248 HP']
  },
  {
    id: 6,
    brand: 'audi',
    model: 'A6',
    year: '2024',
    description: 'Progressive technology with timeless elegance',
    image: '/api/placeholder/400/250',
    price: 58000,
    specs: ['2.0L I4', 'Quattro', '261 HP']
  }
])

const filteredCars = computed(() => {
  return cars.value.filter(car => {
    const brandMatch = brandFilter.value === 'all' || car.brand === brandFilter.value
    const yearMatch = yearFilter.value === 'all' || car.year === yearFilter.value
    let priceMatch = true

    if (priceFilter.value === 'under-50k') {
      priceMatch = car.price < 50000
    } else if (priceFilter.value === '50k-100k') {
      priceMatch = car.price >= 50000 && car.price <= 100000
    } else if (priceFilter.value === 'over-100k') {
      priceMatch = car.price > 100000
    }

    return brandMatch && yearMatch && priceMatch
  })
})

const formatPrice = (price) => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
    maximumFractionDigits: 0
  }).format(price)
}
</script>

<template>
  <div class="min-h-screen bg-gradient-to-br from-gray-900 to-gray-800">
    <!-- Header -->
    <header class="fixed w-full z-50 bg-opacity-95 bg-[#ff5555] backdrop-blur-md shadow-lg">
      <h1 class="text-2xl md:text-3xl font-extralight tracking-wider text-white text-center py-6">
        PREMIUM AUTO GALLERY
      </h1>
    </header>

    <!-- Filter Bar -->
    <div class="fixed w-full z-40 top-[76px] bg-black bg-opacity-30 backdrop-blur-md">
      <div class="container mx-auto px-4 py-4 flex flex-wrap justify-center gap-4">
        <select
          v-model="brandFilter"
          class="px-6 py-3 rounded-full bg-white bg-opacity-5 border border-[#ff5555] border-opacity-30 
                 text-white text-sm cursor-pointer hover:bg-[#ff5555] hover:bg-opacity-10 
                 transition-all duration-300 outline-none min-w-[150px]"
        >
          <option value="all">All Brands</option>
          <option value="mercedes">Mercedes</option>
          <option value="bmw">BMW</option>
          <option value="audi">Audi</option>
        </select>

        <select
          v-model="yearFilter"
          class="px-6 py-3 rounded-full bg-white bg-opacity-5 border border-[#ff5555] border-opacity-30 
                 text-white text-sm cursor-pointer hover:bg-[#ff5555] hover:bg-opacity-10 
                 transition-all duration-300 outline-none min-w-[150px]"
        >
          <option value="all">All Years</option>
          <option value="2024">2024</option>
          <option value="2023">2023</option>
          <option value="2022">2022</option>
        </select>

        <select
          v-model="priceFilter"
          class="px-6 py-3 rounded-full bg-white bg-opacity-5 border border-[#ff5555] border-opacity-30 
                 text-white text-sm cursor-pointer hover:bg-[#ff5555] hover:bg-opacity-10 
                 transition-all duration-300 outline-none min-w-[150px]"
        >
          <option value="all">Price Range</option>
          <option value="under-50k">Under $50,000</option>
          <option value="50k-100k">$50,000 - $100,000</option>
          <option value="over-100k">Over $100,000</option>
        </select>
      </div>
    </div>

    <!-- Gallery Grid -->
    <div class="container mx-auto px-4 pt-48 pb-8">
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <TransitionGroup name="car-fade">
          <div
            v-for="car in filteredCars"
            :key="car.id"
            class="group bg-white bg-opacity-5 backdrop-blur-md rounded-3xl overflow-hidden 
                   border border-white border-opacity-10 transition-all duration-500 
                   hover:transform hover:scale-[1.02] hover:-translate-y-2 
                   hover:bg-opacity-10 hover:shadow-xl hover:shadow-[#ff5555]/10"
          >
            <!-- Price Tag -->
            <div class="absolute top-4 right-4 bg-[#ff5555] bg-opacity-90 px-4 py-2 
                      rounded-full text-white font-medium backdrop-blur-sm z-10">
              {{ formatPrice(car.price) }}
            </div>

            <!-- Car Image -->
            <div class="relative overflow-hidden">
              <img
                :src="car.image"
                :alt="`${car.brand} ${car.model}`"
                class="w-full h-64 object-cover transition-transform duration-500 
                       group-hover:scale-105"
              >
            </div>

            <!-- Car Info -->
            <div class="p-6">
              <h2 class="text-xl font-medium text-white mb-3">
                {{ car.brand.toUpperCase() }} {{ car.model }}
              </h2>
              <p class="text-gray-400 text-sm leading-relaxed mb-4">
                {{ car.description }}
              </p>

              <!-- Specs -->
              <div class="flex flex-wrap gap-3">
                <span
                  v-for="(spec, index) in car.specs"
                  :key="index"
                  class="px-4 py-2 rounded-full text-sm bg-[#ff5555] bg-opacity-10 
                         text-white"
                >
                  {{ spec }}
                </span>
              </div>
            </div>
          </div>
        </TransitionGroup>
      </div>
    </div>
  </div>
</template>

<style scoped>
.car-fade-move,
.car-fade-enter-active,
.car-fade-leave-active {
  transition: all 0.5s cubic-bezier(0.4, 0, 0.2, 1);
}

.car-fade-enter-from,
.car-fade-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

.car-fade-leave-active {
  position: absolute;
}
</style>