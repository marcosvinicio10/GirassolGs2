<template>
  <div class="product-card group bg-white rounded-2xl shadow-light hover:shadow-heavy transition-all duration-500 overflow-hidden border border-gray-100">
    <!-- Product Image -->
    <router-link :to="`/produto/${product.id}`" class="relative overflow-hidden cursor-pointer block">
      <img 
        :src="product.image" 
        :alt="product.name"
        class="w-full h-64 object-cover transition-transform duration-500 group-hover:scale-110"
        loading="lazy"
        @error="handleImageError"
      >
      
      <!-- Overlay -->
      <div class="absolute inset-0 bg-gradient-to-t from-black/60 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
      
      <!-- Quick Actions -->
      <div class="absolute top-4 right-4 flex flex-col space-y-2 opacity-0 group-hover:opacity-100 transition-all duration-300 transform translate-x-4 group-hover:translate-x-0">
        <router-link 
          :to="`/produto/${product.id}`"
          class="w-10 h-10 bg-white/90 backdrop-blur-sm rounded-full flex items-center justify-center hover:bg-white transition-colors duration-300 shadow-medium"
          title="Ver detalhes"
        >
          <i class="fas fa-eye text-gray-700 text-sm"></i>
        </router-link>
        
        <button 
          @click="toggleFavorite"
          class="w-10 h-10 bg-white/90 backdrop-blur-sm rounded-full flex items-center justify-center hover:bg-white transition-colors duration-300 shadow-medium"
          :class="{ 'text-red-500': isFavorite }"
          title="Favoritar"
        >
          <i class="fas fa-heart text-sm" :class="{ 'fas': isFavorite, 'far': !isFavorite }"></i>
        </button>
      </div>
      
      <!-- Category Badge -->
      <div class="absolute top-4 left-4">
        <span class="category-badge">
          {{ getCategoryName(product.category) }}
        </span>
      </div>
      
      <!-- Production Time -->
      <div class="absolute bottom-4 left-4 right-4 text-white transform translate-y-4 group-hover:translate-y-0 transition-transform duration-300">
        <div class="flex items-center space-x-2 text-sm">
          <i class="fas fa-clock"></i>
          <span>{{ product.productionTime }}</span>
        </div>
      </div>
    </router-link>

    <!-- Product Info -->
    <div class="p-4 sm:p-6">
      <!-- Brand -->
      <div class="flex items-center space-x-2 mb-2">
        <div class="w-6 h-6 bg-gradient-to-br from-primary-400 to-primary-600 rounded-full flex items-center justify-center">
          <i class="fas fa-sun text-white text-xs"></i>
        </div>
        <span class="text-sm font-medium text-primary-600">{{ product.brand }}</span>
      </div>
      
      <!-- Product Name -->
      <h3 class="text-base sm:text-lg font-semibold text-gray-900 mb-2 line-clamp-2 group-hover:text-primary-600 transition-colors duration-300">
        {{ product.name }}
      </h3>
      
      <!-- Description -->
      <p class="text-gray-600 text-xs sm:text-sm mb-3 sm:mb-4 line-clamp-2">
        {{ product.description }}
      </p>
      
      <!-- Price -->
      <div class="flex items-center justify-between mb-3 sm:mb-4">
        <div class="flex items-baseline space-x-1">
          <span class="text-lg sm:text-2xl font-bold text-gray-900">R$</span>
          <span class="text-xl sm:text-3xl font-bold text-gray-900">{{ formatPrice(product.price) }}</span>
        </div>
        
        <!-- Rating (placeholder) -->
        <div class="flex items-center space-x-1">
          <div class="flex space-x-1">
            <i class="fas fa-star text-yellow-400 text-sm" v-for="i in 5" :key="i"></i>
          </div>
          <span class="text-sm text-gray-500">(4.9)</span>
        </div>
      </div>
      
      <!-- Add to Cart Button -->
      <button 
        @click="$emit('add-to-cart', product)"
        class="w-full bg-gradient-to-r from-primary-500 to-primary-600 text-white font-semibold py-3 px-4 rounded-xl shadow-medium hover:shadow-heavy hover:scale-105 transition-all duration-300 relative overflow-hidden group/btn"
      >
        <span class="relative z-10 flex items-center justify-center space-x-2">
          <i class="fas fa-shopping-cart"></i>
          <span>Adicionar ao Carrinho</span>
        </span>
        
        <!-- Shimmer effect -->
        <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/20 to-transparent transform -skew-x-12 -translate-x-full group-hover/btn:translate-x-full transition-transform duration-700"></div>
      </button>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  name: 'ProductCard',
  props: {
    product: {
      type: Object,
      required: true
    }
  },
  emits: ['add-to-cart', 'view-product'],
  setup(props) {
    const isFavorite = ref(false)

    const getCategoryName = (category) => {
      const categories = {
        'buques': 'Buquês',
        'caixas': 'Caixas',
        'unidades': 'Unidades'
      }
      return categories[category] || category
    }

    const formatPrice = (price) => {
      return price.toFixed(2).replace('.', ',')
    }

    const toggleFavorite = () => {
      isFavorite.value = !isFavorite.value
      
      // Save to localStorage
      const favorites = JSON.parse(localStorage.getItem('girassol-favorites') || '[]')
      if (isFavorite.value) {
        if (!favorites.includes(props.product.id)) {
          favorites.push(props.product.id)
        }
      } else {
        const index = favorites.indexOf(props.product.id)
        if (index > -1) {
          favorites.splice(index, 1)
        }
      }
      localStorage.setItem('girassol-favorites', JSON.stringify(favorites))
    }

    const handleImageError = (event) => {
      // Só aplica fallback se não for a imagem de fallback
      if (!event.target.src.includes('girassol.png')) {
        event.target.src = '/assets/girassol.png'
        event.target.alt = 'Imagem não disponível'
      }
    }

    // Load favorite status from localStorage
    onMounted(() => {
      const favorites = JSON.parse(localStorage.getItem('girassol-favorites') || '[]')
      isFavorite.value = favorites.includes(props.product.id)
    })

    return {
      isFavorite,
      getCategoryName,
      formatPrice,
      toggleFavorite,
      handleImageError
    }
  }
}
</script>

<style scoped>
.product-card {
  @apply transition-all duration-500 hover:-translate-y-2;
}

.category-badge {
  @apply bg-white/90 backdrop-blur-sm text-gray-700 text-xs font-medium px-3 py-1 rounded-full shadow-medium;
}

.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Enhanced hover effects */
.product-card:hover {
  transform: translateY(-8px) scale(1.02);
}

/* Button shimmer effect */
.group\/btn:hover .absolute {
  animation: shimmer 0.7s ease-out;
}

@keyframes shimmer {
  0% {
    transform: translateX(-100%) skewX(-12deg);
  }
  100% {
    transform: translateX(200%) skewX(-12deg);
  }
}

/* Favorite animation */
.fa-heart {
  transition: all 0.3s ease;
}

.fa-heart:hover {
  transform: scale(1.2);
}

/* Category badge colors */
.category-badge {
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.7));
  backdrop-filter: blur(10px);
}
</style>
