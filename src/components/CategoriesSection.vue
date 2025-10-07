<template>
  <section class="py-16 lg:py-24 bg-gradient-to-b from-white to-gray-50">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <!-- Section Header -->
      <div class="text-center mb-12 lg:mb-16">
        <h2 class="text-3xl sm:text-4xl lg:text-5xl font-bold text-gray-900 mb-4">
          Nosso <span class="text-gradient">Catálogo</span>
        </h2>
        <p class="text-lg sm:text-xl text-gray-600 max-w-2xl mx-auto">
          Explore nossa coleção cuidadosamente selecionada de produtos únicos
        </p>
      </div>

      <!-- Categories Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 sm:gap-6 lg:gap-8 max-w-5xl mx-auto">
        <!-- Buquês Category -->
        <div 
          @click="$emit('select-category', 'buques')"
          class="category-card card-hero-style group cursor-pointer"
          :class="{ 'ring-4 ring-primary-500 ring-opacity-50': activeCategory === 'buques' }"
        >
          <div class="text-center p-4 sm:p-6 lg:p-8">
            <div class="w-16 h-16 sm:w-20 sm:h-20 lg:w-24 lg:h-24 bg-gradient-to-br from-primary-400 to-primary-600 rounded-2xl sm:rounded-3xl flex items-center justify-center mx-auto mb-4 sm:mb-6 shadow-heavy group-hover:shadow-xl transition-all duration-300 group-hover:scale-110">
              <i class="fas fa-seedling text-white text-2xl sm:text-3xl lg:text-4xl"></i>
            </div>
            <h3 class="text-lg sm:text-xl lg:text-2xl font-bold text-gray-900 mb-2 sm:mb-3">Buquês</h3>
            <p class="text-gray-600 mb-3 sm:mb-4 leading-relaxed text-sm sm:text-base">
              Arranjos únicos e elegantes para todas as ocasiões especiais
            </p>
            <div class="inline-flex items-center text-primary-600 font-semibold group-hover:text-primary-700 transition-colors duration-300 text-sm sm:text-base">
              <span>20 produtos</span>
              <i class="fas fa-arrow-right ml-2 transform group-hover:translate-x-1 transition-transform duration-300"></i>
            </div>
          </div>
        </div>

        <!-- Caixas Category -->
        <div 
          @click="$emit('select-category', 'caixas')"
          class="category-card card-hero-style group cursor-pointer"
          :class="{ 'ring-4 ring-primary-500 ring-opacity-50': activeCategory === 'caixas' }"
        >
          <div class="text-center p-4 sm:p-6 lg:p-8">
            <div class="w-16 h-16 sm:w-20 sm:h-20 lg:w-24 lg:h-24 bg-gradient-to-br from-secondary-400 to-secondary-600 rounded-2xl sm:rounded-3xl flex items-center justify-center mx-auto mb-4 sm:mb-6 shadow-heavy group-hover:shadow-xl transition-all duration-300 group-hover:scale-110">
              <i class="fas fa-gift text-white text-2xl sm:text-3xl lg:text-4xl"></i>
            </div>
            <h3 class="text-lg sm:text-xl lg:text-2xl font-bold text-gray-900 mb-2 sm:mb-3">Caixas de Bombom</h3>
            <p class="text-gray-600 mb-3 sm:mb-4 leading-relaxed text-sm sm:text-base">
              Presentes doces e especiais para demonstrar seu carinho
            </p>
            <div class="inline-flex items-center text-secondary-600 font-semibold group-hover:text-secondary-700 transition-colors duration-300 text-sm sm:text-base">
              <span>8 produtos</span>
              <i class="fas fa-arrow-right ml-2 transform group-hover:translate-x-1 transition-transform duration-300"></i>
            </div>
          </div>
        </div>

        <!-- Unidades Category -->
        <div 
          @click="$emit('select-category', 'unidades')"
          class="category-card card-hero-style group cursor-pointer"
          :class="{ 'ring-4 ring-primary-500 ring-opacity-50': activeCategory === 'unidades' }"
        >
          <div class="text-center p-4 sm:p-6 lg:p-8">
            <div class="w-16 h-16 sm:w-20 sm:h-20 lg:w-24 lg:h-24 bg-gradient-to-br from-accent-coral to-pink-500 rounded-2xl sm:rounded-3xl flex items-center justify-center mx-auto mb-4 sm:mb-6 shadow-heavy group-hover:shadow-xl transition-all duration-300 group-hover:scale-110">
              <i class="fas fa-leaf text-white text-2xl sm:text-3xl lg:text-4xl"></i>
            </div>
            <h3 class="text-lg sm:text-xl lg:text-2xl font-bold text-gray-900 mb-2 sm:mb-3">Flores Unitárias</h3>
            <p class="text-gray-600 mb-3 sm:mb-4 leading-relaxed text-sm sm:text-base">
              Flores individuais perfeitas para decorar ou compor arranjos
            </p>
            <div class="inline-flex items-center text-accent-coral font-semibold group-hover:text-pink-600 transition-colors duration-300 text-sm sm:text-base">
              <span>6 produtos</span>
              <i class="fas fa-arrow-right ml-2 transform group-hover:translate-x-1 transition-transform duration-300"></i>
            </div>
          </div>
        </div>
      </div>

      <!-- All Products Button -->
      <div class="text-center mt-8 sm:mt-12">
        <button 
          @click="$emit('select-category', 'all')"
          class="inline-flex items-center space-x-2 bg-gradient-to-r from-gray-800 to-gray-900 text-white font-semibold py-3 px-6 sm:px-8 rounded-xl shadow-medium hover:shadow-heavy hover:scale-105 transition-all duration-300 text-sm sm:text-base"
          :class="{ 'ring-4 ring-gray-500 ring-opacity-50': activeCategory === 'all' }"
        >
          <i class="fas fa-th-large"></i>
          <span>Ver Todos os Produtos</span>
        </button>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'CategoriesSection',
  props: {
    activeCategory: {
      type: String,
      default: 'all'
    }
  },
  emits: ['select-category']
}
</script>

<style scoped>
.category-card {
  @apply bg-white rounded-3xl p-6 shadow-light hover:shadow-heavy transition-all duration-500 border border-gray-100;
}

.category-card:hover {
  transform: translateY(-8px) scale(1.02);
}

/* Custom gradient text */
.text-gradient {
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 50%, #FF8C00 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Enhanced hover effects */
.category-card:hover .w-16 {
  transform: scale(1.1) rotate(5deg);
}

/* Ring animation for active category */
.ring-4 {
  animation: ring-pulse 2s infinite;
}

@keyframes ring-pulse {
  0%, 100% {
    ring-opacity: 50;
  }
  50% {
    ring-opacity: 80;
  }
}
</style>
