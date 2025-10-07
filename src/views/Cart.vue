<template>
  <div class="cart-page min-h-screen bg-gray-50">
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
            <h1 class="text-2xl font-bold text-gray-900">Carrinho</h1>
          </div>
          
          <div class="flex items-center space-x-2">
            <span class="text-sm text-gray-500">{{ cartItems.length }} itens</span>
            <div class="w-8 h-8 bg-primary-500 rounded-full flex items-center justify-center">
              <i class="fas fa-shopping-cart text-white text-sm"></i>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Cart Content -->
    <div class="container mx-auto px-4 py-8">
      <div v-if="cartItems.length === 0" class="text-center py-16">
        <!-- Empty Cart -->
        <div class="max-w-md mx-auto">
          <div class="w-32 h-32 bg-gray-100 rounded-full flex items-center justify-center mx-auto mb-8">
            <i class="fas fa-shopping-cart text-gray-400 text-4xl"></i>
          </div>
          <h2 class="text-2xl font-bold text-gray-900 mb-4">Seu carrinho está vazio</h2>
          <p class="text-gray-600 mb-8">
            Que tal adicionar alguns produtos incríveis da Girassol GS?
          </p>
          <router-link 
            to="/"
            class="btn-primary inline-flex items-center space-x-2"
          >
            <i class="fas fa-home"></i>
            <span>Continuar Comprando</span>
          </router-link>
        </div>
      </div>

      <div v-else class="grid grid-cols-1 lg:grid-cols-3 gap-8">
        <!-- Cart Items -->
        <div class="lg:col-span-2 space-y-4">
          <div 
            v-for="item in cartItems"
            :key="item.id"
            class="cart-item bg-white rounded-2xl p-4 sm:p-6 shadow-light hover:shadow-medium transition-all duration-300"
          >
            <div class="flex flex-col sm:flex-row space-y-4 sm:space-y-0 sm:space-x-4">
              <!-- Product Image -->
              <div class="w-full sm:w-24 h-48 sm:h-24 rounded-xl overflow-hidden flex-shrink-0">
                <img 
                  :src="item.image" 
                  :alt="item.name"
                  class="w-full h-full object-cover"
                  @error="handleImageError"
                >
              </div>
              
              <!-- Product Info -->
              <div class="flex-1 min-w-0">
                <div class="flex flex-col sm:flex-row sm:items-start sm:justify-between">
                  <div class="flex-1">
                    <h3 class="text-lg font-semibold text-gray-900 mb-2 line-clamp-2">
                      {{ item.name }}
                    </h3>
                    <p class="text-sm text-gray-600 mb-2">
                      {{ getCategoryName(item.category) }}
                    </p>
                    <p class="text-sm text-gray-500 mb-4 line-clamp-2">
                      {{ item.description }}
                    </p>
                  </div>
                  
                  <!-- Price -->
                  <div class="text-right">
                    <div class="flex items-baseline space-x-1 mb-2">
                      <span class="text-sm font-medium text-gray-500">R$</span>
                      <span class="text-xl font-bold text-gray-900">{{ formatPrice(item.price) }}</span>
                    </div>
                    <div class="text-sm text-gray-600">
                      Total: <span class="font-semibold">R$ {{ formatPrice(item.price * item.quantity) }}</span>
                    </div>
                  </div>
                </div>
                
                <!-- Quantity Controls -->
                <div class="flex items-center justify-between mt-4 pt-4 border-t border-gray-100">
                  <div class="flex items-center space-x-3">
                    <span class="text-sm font-medium text-gray-700">Quantidade:</span>
                    <div class="flex items-center space-x-2">
                      <button 
                        @click="updateQuantity(item.id, item.quantity - 1)"
                        class="w-8 h-8 rounded-full bg-gray-200 hover:bg-gray-300 transition-colors duration-300 flex items-center justify-center"
                      >
                        <i class="fas fa-minus text-xs"></i>
                      </button>
                      
                      <span class="w-12 text-center font-semibold text-lg">{{ item.quantity }}</span>
                      
                      <button 
                        @click="updateQuantity(item.id, item.quantity + 1)"
                        class="w-8 h-8 rounded-full bg-gray-200 hover:bg-gray-300 transition-colors duration-300 flex items-center justify-center"
                      >
                        <i class="fas fa-plus text-xs"></i>
                      </button>
                    </div>
                  </div>
                  
                  <!-- Remove Button -->
                  <button 
                    @click="removeItem(item.id)"
                    class="text-red-500 hover:text-red-700 text-sm font-medium transition-colors duration-300 flex items-center space-x-1"
                  >
                    <i class="fas fa-trash text-xs"></i>
                    <span class="hidden sm:inline">Remover</span>
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Order Summary -->
        <div class="lg:col-span-1">
          <div class="sticky top-8">
            <div class="bg-white rounded-2xl p-6 shadow-medium">
              <h3 class="text-xl font-bold text-gray-900 mb-6">Resumo do Pedido</h3>
              
              <!-- Summary Details -->
              <div class="space-y-4 mb-6">
                <div class="flex justify-between text-sm">
                  <span class="text-gray-600">Subtotal ({{ totalItems }} itens)</span>
                  <span class="font-semibold">R$ {{ formatPrice(subtotal) }}</span>
                </div>
                
                <div class="flex justify-between text-sm">
                  <span class="text-gray-600">Entrega</span>
                  <span class="font-semibold text-green-600">Grátis</span>
                </div>
                
                <div class="border-t border-gray-200 pt-4">
                  <div class="flex justify-between">
                    <span class="text-lg font-bold text-gray-900">Total</span>
                    <span class="text-2xl font-bold text-gray-900">R$ {{ formatPrice(subtotal) }}</span>
                  </div>
                </div>
              </div>

              <!-- Action Buttons -->
              <div class="space-y-3">
                <button 
                  @click="proceedToCheckout"
                  class="w-full bg-gradient-to-r from-green-500 to-green-600 text-white font-semibold py-4 px-6 rounded-xl shadow-medium hover:shadow-heavy hover:scale-105 transition-all duration-300 flex items-center justify-center space-x-2"
                >
                  <i class="fab fa-whatsapp text-lg"></i>
                  <span>Finalizar no WhatsApp</span>
                </button>
                
                <button 
                  @click="clearCart"
                  class="w-full bg-gray-200 hover:bg-gray-300 text-gray-700 font-medium py-3 px-6 rounded-xl transition-colors duration-300"
                >
                  Limpar Carrinho
                </button>
                
                <router-link 
                  to="/"
                  class="w-full bg-primary-500 hover:bg-primary-600 text-white font-medium py-3 px-6 rounded-xl transition-colors duration-300 flex items-center justify-center space-x-2"
                >
                  <i class="fas fa-plus"></i>
                  <span>Continuar Comprando</span>
                </router-link>
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

export default {
  name: 'Cart',
  setup() {
    const cartItems = ref([])

    const subtotal = computed(() => {
      return cartItems.value.reduce((total, item) => {
        return total + (item.price * item.quantity)
      }, 0)
    })

    const totalItems = computed(() => {
      return cartItems.value.reduce((total, item) => {
        return total + item.quantity
      }, 0)
    })

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

    const updateQuantity = (productId, newQuantity) => {
      const item = cartItems.value.find(item => item.id === productId)
      if (item) {
        if (newQuantity <= 0) {
          removeItem(productId)
        } else {
          item.quantity = newQuantity
          localStorage.setItem('girassol-cart', JSON.stringify(cartItems.value))
        }
      }
    }

    const removeItem = (productId) => {
      cartItems.value = cartItems.value.filter(item => item.id !== productId)
      localStorage.setItem('girassol-cart', JSON.stringify(cartItems.value))
    }

    const clearCart = () => {
      cartItems.value = []
      localStorage.removeItem('girassol-cart')
    }

    const proceedToCheckout = () => {
      if (cartItems.value.length === 0) return

      // Create WhatsApp message
      let message = '🌻 *Olá! Gostaria de finalizar meu pedido na Girassol GS:*\n\n'
      
      cartItems.value.forEach((item, index) => {
        message += `${index + 1}. *${item.name}*\n`
        message += `   Quantidade: ${item.quantity}\n`
        message += `   Preço unitário: R$ ${formatPrice(item.price)}\n`
        message += `   Subtotal: R$ ${formatPrice(item.price * item.quantity)}\n\n`
      })
      
      message += `💰 *Total do pedido: R$ ${formatPrice(subtotal.value)}*\n\n`
      message += 'Por favor, confirme a disponibilidade e me informe sobre a entrega. Obrigado! 🌻'

      // Open WhatsApp
      const whatsappUrl = `https://wa.me/5511999999999?text=${encodeURIComponent(message)}`
      window.open(whatsappUrl, '_blank')
    }

    const handleImageError = (event) => {
      // Só aplica fallback se não for a imagem de fallback
      if (!event.target.src.includes('girassol.png')) {
        event.target.src = '/assets/girassol.png'
        event.target.alt = 'Imagem não disponível'
      }
    }

    // Load cart from localStorage on mount
    onMounted(() => {
      const savedCart = localStorage.getItem('girassol-cart')
      if (savedCart) {
        cartItems.value = JSON.parse(savedCart)
      }
    })

    return {
      cartItems,
      subtotal,
      totalItems,
      getCategoryName,
      formatPrice,
      updateQuantity,
      removeItem,
      clearCart,
      proceedToCheckout,
      handleImageError
    }
  }
}
</script>

<style scoped>
.cart-item {
  transition: all 0.3s ease;
}

.cart-item:hover {
  transform: translateY(-2px);
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

/* Mobile optimizations */
@media (max-width: 640px) {
  .cart-item {
    padding: 1rem;
  }
  
  .grid {
    gap: 1rem;
  }
}
</style>
