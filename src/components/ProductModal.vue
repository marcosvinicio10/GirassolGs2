<template>
  <!-- Backdrop -->
  <div 
    v-if="isOpen"
    @click="$emit('close-modal')"
    class="fixed inset-0 bg-black/70 backdrop-blur-sm z-50 transition-opacity duration-300"
  ></div>

  <!-- Modal -->
  <div 
    v-if="isOpen && product"
    class="fixed inset-0 z-50 flex items-center justify-center p-4"
  >
    <div 
      @click.stop
      class="bg-white rounded-3xl shadow-2xl max-w-4xl w-full max-h-[90vh] overflow-hidden transform transition-all duration-300"
    >
      <!-- Header -->
      <div class="flex items-center justify-between p-6 border-b border-gray-200 bg-gradient-to-r from-primary-500 to-primary-600 text-white">
        <h2 class="text-xl font-bold flex items-center space-x-2">
          <i class="fas fa-info-circle"></i>
          <span>Detalhes do Produto</span>
        </h2>
        <button 
          @click="$emit('close-modal')"
          class="w-8 h-8 rounded-full bg-white/20 hover:bg-white/30 transition-colors duration-300 flex items-center justify-center"
        >
          <i class="fas fa-times"></i>
        </button>
      </div>

      <!-- Content -->
      <div class="flex flex-col lg:flex-row">
        <!-- Product Image -->
        <div class="lg:w-1/2 p-6">
          <div class="relative">
            <img 
              :src="product.image" 
              :alt="product.name"
              class="w-full h-64 lg:h-80 object-cover rounded-2xl shadow-medium"
            >
            
            <!-- Category Badge -->
            <div class="absolute top-4 left-4">
              <span class="category-badge">
                {{ getCategoryName(product.category) }}
              </span>
            </div>
            
            <!-- Favorite Button -->
            <button 
              @click="toggleFavorite"
              class="absolute top-4 right-4 w-10 h-10 bg-white/90 backdrop-blur-sm rounded-full flex items-center justify-center hover:bg-white transition-colors duration-300 shadow-medium"
              :class="{ 'text-red-500': isFavorite }"
            >
              <i class="fas fa-heart" :class="{ 'fas': isFavorite, 'far': !isFavorite }"></i>
            </button>
          </div>
        </div>

        <!-- Product Info -->
        <div class="lg:w-1/2 p-6 flex flex-col justify-between">
          <div>
            <!-- Brand -->
            <div class="flex items-center space-x-2 mb-3">
              <div class="w-8 h-8 bg-gradient-to-br from-primary-400 to-primary-600 rounded-full flex items-center justify-center">
                <i class="fas fa-sun text-white text-sm"></i>
              </div>
              <span class="text-sm font-medium text-primary-600">{{ product.brand }}</span>
            </div>
            
            <!-- Product Name -->
            <h1 class="text-2xl lg:text-3xl font-bold text-gray-900 mb-4">
              {{ product.name }}
            </h1>
            
            <!-- Description -->
            <p class="text-gray-600 mb-6 leading-relaxed">
              {{ product.description }}
            </p>
            
            <!-- Features -->
            <div class="space-y-3 mb-6">
              <div class="flex items-center space-x-3">
                <div class="w-8 h-8 bg-green-100 rounded-full flex items-center justify-center">
                  <i class="fas fa-clock text-green-600 text-sm"></i>
                </div>
                <span class="text-gray-700">
                  <strong>Tempo de produção:</strong> {{ product.productionTime }}
                </span>
              </div>
              
              <div class="flex items-center space-x-3">
                <div class="w-8 h-8 bg-blue-100 rounded-full flex items-center justify-center">
                  <i class="fas fa-shipping-fast text-blue-600 text-sm"></i>
                </div>
                <span class="text-gray-700">
                  <strong>Entrega:</strong> Grátis para toda região
                </span>
              </div>
              
              <div class="flex items-center space-x-3">
                <div class="w-8 h-8 bg-purple-100 rounded-full flex items-center justify-center">
                  <i class="fas fa-heart text-purple-600 text-sm"></i>
                </div>
                <span class="text-gray-700">
                  <strong>Qualidade:</strong> Produto único e especial
                </span>
              </div>
            </div>
          </div>

          <!-- Price and Actions -->
          <div class="border-t border-gray-200 pt-6">
            <!-- Price -->
            <div class="flex items-baseline space-x-2 mb-6">
              <span class="text-2xl font-bold text-gray-500">R$</span>
              <span class="text-4xl font-bold text-gray-900">{{ formatPrice(product.price) }}</span>
            </div>
            
            <!-- Action Buttons -->
            <div class="space-y-3">
              <button 
                @click="addToCart"
                class="w-full bg-gradient-to-r from-primary-500 to-primary-600 text-white font-semibold py-4 px-6 rounded-xl shadow-medium hover:shadow-heavy hover:scale-105 transition-all duration-300 flex items-center justify-center space-x-2"
              >
                <i class="fas fa-shopping-cart"></i>
                <span>Adicionar ao Carrinho</span>
              </button>
              
              <a 
                :href="whatsappUrl"
                target="_blank"
                class="w-full bg-gradient-to-r from-green-500 to-green-600 text-white font-semibold py-4 px-6 rounded-xl shadow-medium hover:shadow-heavy hover:scale-105 transition-all duration-300 flex items-center justify-center space-x-2"
              >
                <i class="fab fa-whatsapp"></i>
                <span>Falar no WhatsApp</span>
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'

export default {
  name: 'ProductModal',
  props: {
    isOpen: {
      type: Boolean,
      default: false
    },
    product: {
      type: Object,
      default: null
    }
  },
  emits: ['close-modal', 'add-to-cart'],
  setup(props, { emit }) {
    const isFavorite = ref(false)

    const getCategoryName = (category) => {
      const categories = {
        'buques': 'Buquê',
        'caixas': 'Caixa',
        'unidades': 'Unidade'
      }
      return categories[category] || category
    }

    const formatPrice = (price) => {
      return price.toFixed(2).replace('.', ',')
    }

    const whatsappUrl = computed(() => {
      if (!props.product) return ''
      
      const message = `🌻 *Olá! Gostaria de saber mais sobre este produto da Girassol GS:*\n\n` +
        `*${props.product.name}*\n` +
        `Preço: R$ ${formatPrice(props.product.price)}\n` +
        `Categoria: ${getCategoryName(props.product.category)}\n\n` +
        `Gostaria de fazer um pedido ou tirar algumas dúvidas. Obrigado! 🌻`
      
      return `https://wa.me/5511999999999?text=${encodeURIComponent(message)}`
    })

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

    const addToCart = () => {
      emit('add-to-cart', props.product)
      emit('close-modal')
    }

    // Load favorite status from localStorage
    onMounted(() => {
      if (props.product) {
        const favorites = JSON.parse(localStorage.getItem('girassol-favorites') || '[]')
        isFavorite.value = favorites.includes(props.product.id)
      }
    })

    return {
      isFavorite,
      getCategoryName,
      formatPrice,
      whatsappUrl,
      toggleFavorite,
      addToCart
    }
  }
}
</script>

<style scoped>
.category-badge {
  @apply bg-white/90 backdrop-blur-sm text-gray-700 text-sm font-medium px-3 py-1 rounded-full shadow-medium;
}

/* Modal animation */
.modal-enter-active, .modal-leave-active {
  transition: all 0.3s ease;
}

.modal-enter-from, .modal-leave-to {
  opacity: 0;
  transform: scale(0.9);
}

/* Favorite animation */
.fa-heart {
  transition: all 0.3s ease;
}

.fa-heart:hover {
  transform: scale(1.2);
}

/* Responsive adjustments */
@media (max-width: 1024px) {
  .modal {
    margin: 1rem;
    max-height: calc(100vh - 2rem);
  }
}
</style>
