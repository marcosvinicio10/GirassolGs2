<template>
  <div class="product-page min-h-screen bg-gray-50">
    <!-- Loading State -->
    <div v-if="loading" class="flex items-center justify-center min-h-screen">
      <div class="text-center">
        <div class="w-16 h-16 border-4 border-primary-500 border-t-transparent rounded-full animate-spin mx-auto mb-4"></div>
        <p class="text-gray-600">Carregando produto...</p>
      </div>
    </div>

    <!-- Product Not Found -->
    <div v-else-if="!product" class="flex items-center justify-center min-h-screen">
      <div class="text-center">
        <div class="w-24 h-24 bg-gray-100 rounded-full flex items-center justify-center mx-auto mb-6">
          <i class="fas fa-exclamation-triangle text-gray-400 text-3xl"></i>
        </div>
        <h2 class="text-2xl font-bold text-gray-900 mb-4">Produto não encontrado</h2>
        <p class="text-gray-600 mb-8">O produto que você está procurando não existe.</p>
        <router-link 
          to="/"
          class="btn-primary inline-flex items-center space-x-2"
        >
          <i class="fas fa-home"></i>
          <span>Voltar ao Início</span>
        </router-link>
      </div>
    </div>

    <!-- Product Content -->
    <div v-else>
      <!-- Header -->
      <div class="bg-white shadow-sm border-b">
        <div class="container mx-auto px-4 py-4">
          <div class="flex items-center justify-between">
            <div class="flex items-center space-x-3">
              <button 
                @click="$router.go(-1)"
                class="w-10 h-10 rounded-full bg-gray-100 hover:bg-gray-200 transition-colors duration-300 flex items-center justify-center"
              >
                <i class="fas fa-arrow-left text-gray-600"></i>
              </button>
              <div>
                <h1 class="text-lg font-semibold text-gray-900">{{ product.name }}</h1>
                <p class="text-sm text-gray-500">{{ getCategoryName(product.category) }}</p>
              </div>
            </div>
            
            <div class="flex items-center space-x-2">
              <button 
                @click="toggleFavorite"
                class="w-10 h-10 rounded-full bg-gray-100 hover:bg-gray-200 transition-colors duration-300 flex items-center justify-center"
                :class="{ 'text-red-500 bg-red-50': isFavorite }"
              >
                <i class="fas fa-heart" :class="{ 'fas': isFavorite, 'far': !isFavorite }"></i>
              </button>
              
              <button 
                @click="goToCart"
                class="relative w-10 h-10 rounded-full bg-primary-500 hover:bg-primary-600 transition-colors duration-300 flex items-center justify-center"
              >
                <i class="fas fa-shopping-cart text-white"></i>
                <span 
                  v-if="cartCount > 0"
                  class="absolute -top-2 -right-2 bg-red-500 text-white text-xs font-bold rounded-full w-5 h-5 flex items-center justify-center"
                >
                  {{ cartCount }}
                </span>
              </button>
            </div>
          </div>
        </div>
      </div>

      <!-- Product Details -->
      <div class="container mx-auto px-4 py-8">
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 lg:gap-12">
          <!-- Product Image -->
          <div class="space-y-4">
            <div class="relative">
              <img 
                :src="product.image" 
                :alt="product.name"
                class="w-full h-64 sm:h-80 lg:h-96 object-cover rounded-2xl shadow-medium"
                @error="handleImageError"
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
                class="absolute top-4 right-4 w-12 h-12 bg-white/90 backdrop-blur-sm rounded-full flex items-center justify-center hover:bg-white transition-colors duration-300 shadow-medium"
                :class="{ 'text-red-500': isFavorite }"
              >
                <i class="fas fa-heart text-lg" :class="{ 'fas': isFavorite, 'far': !isFavorite }"></i>
              </button>
            </div>
          </div>

          <!-- Product Info -->
          <div class="space-y-6">
            <!-- Brand -->
            <div class="flex items-center space-x-3">
              <div class="w-10 h-10 bg-gradient-to-br from-primary-400 to-primary-600 rounded-full flex items-center justify-center">
                <i class="fas fa-sun text-white text-sm"></i>
              </div>
              <span class="text-lg font-medium text-primary-600">{{ product.brand }}</span>
            </div>
            
            <!-- Product Name -->
            <h1 class="text-3xl lg:text-4xl font-bold text-gray-900">
              {{ product.name }}
            </h1>
            
            <!-- Description -->
            <p class="text-lg text-gray-600 leading-relaxed">
              {{ product.description }}
            </p>
            
            <!-- Features -->
            <div class="space-y-4">
              <div class="flex items-center space-x-4">
                <div class="w-12 h-12 bg-green-100 rounded-full flex items-center justify-center">
                  <i class="fas fa-clock text-green-600"></i>
                </div>
                <div>
                  <p class="font-semibold text-gray-900">Tempo de produção</p>
                  <p class="text-gray-600">{{ product.productionTime }}</p>
                </div>
              </div>
              
              <div class="flex items-center space-x-4">
                <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center">
                  <i class="fas fa-shipping-fast text-blue-600"></i>
                </div>
                <div>
                  <p class="font-semibold text-gray-900">Entrega</p>
                  <p class="text-gray-600">Grátis para toda região</p>
                </div>
              </div>
              
              <div class="flex items-center space-x-4">
                <div class="w-12 h-12 bg-purple-100 rounded-full flex items-center justify-center">
                  <i class="fas fa-heart text-purple-600"></i>
                </div>
                <div>
                  <p class="font-semibold text-gray-900">Qualidade</p>
                  <p class="text-gray-600">Produto único e especial</p>
                </div>
              </div>
            </div>

            <!-- Price -->
            <div class="border-t border-gray-200 pt-6">
              <div class="flex items-baseline space-x-2 mb-6">
                <span class="text-2xl font-bold text-gray-500">R$</span>
                <span class="text-5xl font-bold text-gray-900">{{ formatPrice(product.price) }}</span>
              </div>
              
              <!-- Action Buttons -->
              <div class="space-y-4">
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

        <!-- Related Products -->
        <div class="mt-16">
          <h2 class="text-2xl lg:text-3xl font-bold text-gray-900 mb-8 text-center">
            Produtos <span class="text-gradient">Relacionados</span>
          </h2>
          
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
            <div 
              v-for="relatedProduct in relatedProducts"
              :key="relatedProduct.id"
              @click="goToProduct(relatedProduct.id)"
              class="related-product bg-white rounded-2xl p-4 shadow-light hover:shadow-medium transition-all duration-300 cursor-pointer"
            >
              <div class="w-full h-48 rounded-xl overflow-hidden mb-4">
                <img 
                  :src="relatedProduct.image" 
                  :alt="relatedProduct.name"
                  class="w-full h-full object-cover"
                  @error="handleImageError"
                >
              </div>
              <h3 class="font-semibold text-gray-900 mb-2 line-clamp-2">{{ relatedProduct.name }}</h3>
              <div class="flex items-center justify-between">
                <span class="text-lg font-bold text-primary-600">R$ {{ formatPrice(relatedProduct.price) }}</span>
                <span class="text-xs text-gray-500">{{ getCategoryName(relatedProduct.category) }}</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import productsData from '../products.json'

export default {
  name: 'Product',
  props: {
    id: {
      type: String,
      required: true
    }
  },
  setup(props) {
    const router = useRouter()
    const loading = ref(true)
    const product = ref(null)
    const isFavorite = ref(false)
    const cartCount = ref(0)

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
      if (!product.value) return ''
      
      const message = `🌻 *Olá! Gostaria de saber mais sobre este produto da Girassol GS:*\n\n` +
        `*${product.value.name}*\n` +
        `Preço: R$ ${formatPrice(product.value.price)}\n` +
        `Categoria: ${getCategoryName(product.value.category)}\n\n` +
        `Gostaria de fazer um pedido ou tirar algumas dúvidas. Obrigado! 🌻`
      
      return `https://wa.me/5511999999999?text=${encodeURIComponent(message)}`
    })

    const relatedProducts = computed(() => {
      if (!product.value) return []
      
      return productsData.products
        .filter(p => p.category === product.value.category && p.id !== product.value.id)
        .slice(0, 4)
    })

    const toggleFavorite = () => {
      isFavorite.value = !isFavorite.value
      
      const favorites = JSON.parse(localStorage.getItem('girassol-favorites') || '[]')
      if (isFavorite.value) {
        if (!favorites.includes(product.value.id)) {
          favorites.push(product.value.id)
        }
      } else {
        const index = favorites.indexOf(product.value.id)
        if (index > -1) {
          favorites.splice(index, 1)
        }
      }
      localStorage.setItem('girassol-favorites', JSON.stringify(favorites))
    }

    const addToCart = () => {
      const existingCart = JSON.parse(localStorage.getItem('girassol-cart') || '[]')
      const existingItem = existingCart.find(item => item.id === product.value.id)
      
      if (existingItem) {
        existingItem.quantity += 1
      } else {
        existingCart.push({
          ...product.value,
          quantity: 1
        })
      }
      
      localStorage.setItem('girassol-cart', JSON.stringify(existingCart))
      updateCartCount()
      showCartFeedback()
    }

    const goToCart = () => {
      router.push('/carrinho')
    }

    const goToProduct = (productId) => {
      router.push({ name: 'Product', params: { id: productId } })
    }

    const handleImageError = (event) => {
      // Só aplica fallback se não for a imagem de fallback
      if (!event.target.src.includes('girassol.png')) {
        event.target.src = '/assets/girassol.png'
        event.target.alt = 'Imagem não disponível'
      }
    }

    const updateCartCount = () => {
      const cart = JSON.parse(localStorage.getItem('girassol-cart') || '[]')
      cartCount.value = cart.reduce((total, item) => total + item.quantity, 0)
    }

    const showCartFeedback = () => {
      const notification = document.createElement('div')
      notification.className = 'fixed top-20 right-4 bg-green-500 text-white px-4 py-2 rounded-lg shadow-lg z-50 animate-slide-in'
      notification.textContent = 'Produto adicionado ao carrinho!'
      
      document.body.appendChild(notification)
      
      setTimeout(() => {
        notification.remove()
      }, 3000)
    }

    onMounted(() => {
      // Find product by ID
      const foundProduct = productsData.products.find(p => p.id === parseInt(props.id))
      
      if (foundProduct) {
        product.value = foundProduct
        
        // Load favorite status
        const favorites = JSON.parse(localStorage.getItem('girassol-favorites') || '[]')
        isFavorite.value = favorites.includes(foundProduct.id)
      }
      
      updateCartCount()
      loading.value = false
    })

    return {
      loading,
      product,
      isFavorite,
      cartCount,
      getCategoryName,
      formatPrice,
      whatsappUrl,
      relatedProducts,
      toggleFavorite,
      addToCart,
      goToCart,
      goToProduct,
      handleImageError
    }
  }
}
</script>

<style scoped>
.category-badge {
  @apply bg-white/90 backdrop-blur-sm text-gray-700 text-sm font-medium px-3 py-1 rounded-full shadow-medium;
}

.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.btn-primary {
  @apply bg-gradient-to-r from-primary-500 to-primary-600 text-white font-semibold py-3 px-6 rounded-xl shadow-medium hover:shadow-heavy hover:scale-105 transition-all duration-300;
}

.text-gradient {
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 50%, #FF8C00 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.related-product:hover {
  transform: translateY(-4px);
}

/* Mobile optimizations */
@media (max-width: 640px) {
  .container {
    padding-left: 1rem;
    padding-right: 1rem;
  }
  
  .grid {
    gap: 1rem;
  }
}
</style>
