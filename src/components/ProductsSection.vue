<template>
  <section id="products" class="py-16 lg:py-24 bg-gradient-to-b from-gray-50 to-white">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <!-- Section Header -->
      <div class="text-center mb-12 lg:mb-16">
        <h2 class="text-3xl sm:text-4xl lg:text-5xl font-bold text-gray-900 mb-4">
          Nossos <span class="text-gradient">Produtos</span>
        </h2>
        <p class="text-lg sm:text-xl text-gray-600 max-w-2xl mx-auto mb-8">
          {{ getSectionDescription() }}
        </p>
        
        <!-- Category Filter -->
        <div class="flex flex-wrap justify-center gap-3 mb-8">
          <button 
            v-for="category in categories"
            :key="category.value"
            @click="$emit('select-category', category.value)"
            class="category-filter-btn"
            :class="{ 'active': activeCategory === category.value }"
          >
            <i :class="category.icon" class="mr-2"></i>
            {{ category.label }}
            <span class="ml-2 bg-white/20 px-2 py-1 rounded-full text-xs">
              {{ getCategoryCount(category.value) }}
            </span>
          </button>
        </div>
      </div>

      <!-- Products Grid (Desktop) -->
      <div class="hidden md:grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 lg:gap-8">
        <ProductCard
          v-for="product in products"
          :key="product.id"
          :product="product"
          @add-to-cart="$emit('add-to-cart', $event)"
          @view-product="$emit('view-product', $event)"
        />
      </div>

      <!-- Products Carousel (Mobile) -->
      <div class="md:hidden">
        <div class="relative bg-gradient-to-r from-primary-50 to-secondary-50 rounded-3xl p-4 shadow-medium">
          <!-- Carousel Header -->
          <div class="flex items-center justify-between mb-4">
            <div class="flex items-center space-x-2">
              <div class="w-8 h-8 bg-gradient-to-br from-primary-500 to-primary-600 rounded-full flex items-center justify-center">
                <i class="fas fa-shopping-bag text-white text-sm"></i>
              </div>
              <h3 class="text-lg font-bold text-gray-900">Nossos Produtos</h3>
            </div>
            <div class="text-sm text-gray-600">
              {{ currentSlide + 1 }} / {{ totalSlides }}
            </div>
          </div>

          <!-- Carousel Container -->
          <div class="relative overflow-hidden rounded-2xl">
            <div 
              class="flex transition-transform duration-500 ease-out"
              :style="{ transform: `translateX(-${currentSlide * 100}%)` }"
              @touchstart="handleTouchStart"
              @touchmove="handleTouchMove"
              @touchend="handleTouchEnd"
            >
              <div 
                v-for="(chunk, index) in productChunks"
                :key="index"
                class="w-full flex-shrink-0 px-1"
              >
                <div class="grid grid-cols-2 gap-3">
                  <ProductCard
                    v-for="product in chunk"
                    :key="product.id"
                    :product="product"
                    @add-to-cart="$emit('add-to-cart', $event)"
                    @view-product="$emit('view-product', $event)"
                  />
                </div>
              </div>
            </div>

            <!-- Navigation Arrows -->
            <button
              v-if="currentSlide > 0"
              @click="previousSlide"
              class="absolute left-2 top-1/2 transform -translate-y-1/2 bg-white/95 backdrop-blur-md rounded-full p-3 shadow-heavy hover:bg-white hover:scale-110 transition-all duration-300 z-10 border border-white/20"
            >
              <i class="fas fa-chevron-left text-primary-600 text-lg"></i>
            </button>

            <button
              v-if="currentSlide < totalSlides - 1"
              @click="nextSlide"
              class="absolute right-2 top-1/2 transform -translate-y-1/2 bg-white/95 backdrop-blur-md rounded-full p-3 shadow-heavy hover:bg-white hover:scale-110 transition-all duration-300 z-10 border border-white/20"
            >
              <i class="fas fa-chevron-right text-primary-600 text-lg"></i>
            </button>
          </div>

          <!-- Enhanced Dots Indicator -->
          <div class="flex justify-center space-x-3 mt-6">
            <button
              v-for="(slide, index) in totalSlides"
              :key="index"
              @click="goToSlide(index)"
              class="w-3 h-3 rounded-full transition-all duration-300 border-2"
              :class="index === currentSlide 
                ? 'bg-primary-500 border-primary-500 scale-125' 
                : 'bg-white border-gray-300 hover:border-primary-300 hover:scale-110'"
            ></button>
          </div>

          <!-- Swipe Hint -->
          <div class="text-center mt-4">
            <p class="text-xs text-gray-500 flex items-center justify-center space-x-1">
              <i class="fas fa-hand-pointer text-primary-500"></i>
              <span>Deslize para ver mais produtos</span>
            </p>
          </div>
        </div>
      </div>

      <!-- Empty State -->
      <div v-if="products.length === 0" class="text-center py-16">
        <div class="w-24 h-24 bg-gray-100 rounded-full flex items-center justify-center mx-auto mb-6">
          <i class="fas fa-search text-gray-400 text-3xl"></i>
        </div>
        <h3 class="text-xl font-semibold text-gray-900 mb-2">Nenhum produto encontrado</h3>
        <p class="text-gray-600 mb-6">Tente selecionar uma categoria diferente</p>
        <button 
          @click="$emit('select-category', 'all')"
          class="btn-primary"
        >
          Ver Todos os Produtos
        </button>
      </div>

      <!-- Load More Button (if needed) -->
      <div v-if="products.length > 0 && activeCategory === 'all'" class="text-center mt-12">
        <button 
          @click="loadMoreProducts"
          class="btn-secondary"
          :disabled="isLoading"
        >
          <i class="fas fa-plus mr-2" v-if="!isLoading"></i>
          <i class="fas fa-spinner fa-spin mr-2" v-if="isLoading"></i>
          {{ isLoading ? 'Carregando...' : 'Carregar Mais' }}
        </button>
      </div>

      <!-- WhatsApp CTA -->
      <div class="mt-16 bg-gradient-to-r from-green-500 to-green-600 rounded-3xl p-8 lg:p-12 text-center text-white">
        <div class="max-w-3xl mx-auto">
          <div class="w-20 h-20 bg-white/20 rounded-full flex items-center justify-center mx-auto mb-6">
            <i class="fab fa-whatsapp text-3xl"></i>
          </div>
          <h3 class="text-2xl lg:text-3xl font-bold mb-4">
            Não encontrou o que procura?
          </h3>
          <p class="text-lg mb-6 opacity-90">
            Entre em contato conosco pelo WhatsApp e vamos criar algo especial para você!
          </p>
          <a 
            href="https://wa.me/5511999999999?text=Olá! Gostaria de saber mais sobre produtos personalizados da Girassol GS"
            target="_blank"
            class="inline-flex items-center space-x-3 bg-white text-green-600 font-semibold py-4 px-8 rounded-xl shadow-heavy hover:shadow-xl hover:scale-105 transition-all duration-300"
          >
            <i class="fab fa-whatsapp text-xl"></i>
            <span>Falar no WhatsApp</span>
          </a>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { ref, computed } from 'vue'
import ProductCard from './ProductCard.vue'

export default {
  name: 'ProductsSection',
  components: {
    ProductCard
  },
  props: {
    products: {
      type: Array,
      required: true
    },
    activeCategory: {
      type: String,
      default: 'all'
    }
  },
  emits: ['select-category', 'add-to-cart', 'view-product'],
  setup(props) {
    const isLoading = ref(false)
    const displayedProducts = ref(12) // Show 12 products initially
    const currentSlide = ref(0)

    const categories = [
      { value: 'all', label: 'Todos', icon: 'fas fa-th-large' },
      { value: 'buques', label: 'Buquês', icon: 'fas fa-seedling' },
      { value: 'caixas', label: 'Caixas', icon: 'fas fa-gift' },
      { value: 'unidades', label: 'Unidades', icon: 'fas fa-leaf' }
    ]

    // Carousel logic
    const productsPerSlide = 4 // 2x2 grid per slide
    const productChunks = computed(() => {
      const chunks = []
      for (let i = 0; i < props.products.length; i += productsPerSlide) {
        chunks.push(props.products.slice(i, i + productsPerSlide))
      }
      return chunks
    })

    const totalSlides = computed(() => productChunks.value.length)

    const nextSlide = () => {
      if (currentSlide.value < totalSlides.value - 1) {
        currentSlide.value++
      }
    }

    const previousSlide = () => {
      if (currentSlide.value > 0) {
        currentSlide.value--
      }
    }

    const goToSlide = (index) => {
      currentSlide.value = index
    }

    // Touch/swipe support
    const touchStartX = ref(0)
    const touchEndX = ref(0)

    const handleTouchStart = (e) => {
      touchStartX.value = e.touches[0].clientX
    }

    const handleTouchMove = (e) => {
      touchEndX.value = e.touches[0].clientX
    }

    const handleTouchEnd = () => {
      const swipeThreshold = 50
      const diff = touchStartX.value - touchEndX.value

      if (Math.abs(diff) > swipeThreshold) {
        if (diff > 0) {
          // Swipe left - next slide
          nextSlide()
        } else {
          // Swipe right - previous slide
          previousSlide()
        }
      }
    }

    const getSectionDescription = () => {
      const descriptions = {
        'all': 'Explore nossa coleção completa de produtos únicos e especiais',
        'buques': 'Buquês únicos e elegantes para todas as ocasiões especiais',
        'caixas': 'Caixas de bombom e presentes doces para demonstrar seu carinho',
        'unidades': 'Flores individuais perfeitas para decorar ou compor arranjos'
      }
      return descriptions[props.activeCategory] || descriptions['all']
    }

    const getCategoryCount = (category) => {
      if (category === 'all') {
        return props.products.length
      }
      // Count products from the original products array, not the filtered one
      const allProducts = JSON.parse(localStorage.getItem('girassol-all-products') || '[]')
      if (allProducts.length === 0) {
        // Fallback to props.products if localStorage is empty
        return props.products.filter(product => product.category === category).length
      }
      return allProducts.filter(product => product.category === category).length
    }

    const loadMoreProducts = async () => {
      isLoading.value = true
      
      // Simulate loading delay
      await new Promise(resolve => setTimeout(resolve, 1000))
      
      displayedProducts.value += 12
      isLoading.value = false
    }

    return {
      isLoading,
      displayedProducts,
      categories,
      getSectionDescription,
      getCategoryCount,
      loadMoreProducts,
      currentSlide,
      productChunks,
      totalSlides,
      nextSlide,
      previousSlide,
      goToSlide,
      handleTouchStart,
      handleTouchMove,
      handleTouchEnd
    }
  }
}
</script>

<style scoped>
.category-filter-btn {
  @apply flex items-center px-4 py-2 rounded-full font-medium transition-all duration-300 border-2 border-transparent;
  @apply bg-white text-gray-700 hover:bg-gray-50 hover:border-primary-200;
}

.category-filter-btn.active {
  @apply bg-gradient-to-r from-primary-500 to-primary-600 text-white border-primary-500 shadow-medium;
}

.category-filter-btn:hover {
  transform: translateY(-2px);
}

/* Custom gradient text */
.text-gradient {
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 50%, #FF8C00 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Button styles */
.btn-primary {
  @apply bg-gradient-to-r from-primary-500 to-primary-600 text-white font-semibold py-3 px-8 rounded-xl shadow-medium hover:shadow-heavy hover:scale-105 transition-all duration-300;
}

.btn-secondary {
  @apply bg-gradient-to-r from-secondary-500 to-secondary-600 text-white font-semibold py-3 px-8 rounded-xl shadow-medium hover:shadow-heavy hover:scale-105 transition-all duration-300;
}

/* Loading animation */
.fa-spinner {
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

/* Responsive grid adjustments */
@media (max-width: 640px) {
  .grid {
    grid-template-columns: 1fr;
    gap: 1rem;
  }
}

@media (min-width: 641px) and (max-width: 1024px) {
  .grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 1.5rem;
  }
}

@media (min-width: 1025px) {
  .grid {
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
  }
}

@media (min-width: 1280px) {
  .grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
</style>
