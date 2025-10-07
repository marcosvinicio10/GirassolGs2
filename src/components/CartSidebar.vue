<template>
  <!-- Backdrop -->
  <div 
    v-if="isOpen"
    @click="$emit('close-cart')"
    class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 transition-opacity duration-300"
  ></div>

  <!-- Sidebar -->
  <div 
    class="fixed top-0 right-0 h-full w-full max-w-md bg-white shadow-2xl z-50 transform transition-transform duration-300 ease-in-out"
    :class="{ 'translate-x-0': isOpen, 'translate-x-full': !isOpen }"
  >
    <!-- Header -->
    <div class="flex items-center justify-between p-6 border-b border-gray-200 bg-gradient-to-r from-primary-500 to-primary-600 text-white">
      <h2 class="text-xl font-bold flex items-center space-x-2">
        <i class="fas fa-shopping-cart"></i>
        <span>Carrinho</span>
        <span v-if="cartItems.length > 0" class="bg-white/20 px-2 py-1 rounded-full text-sm">
          {{ cartItems.length }}
        </span>
      </h2>
      <button 
        @click="$emit('close-cart')"
        class="w-8 h-8 rounded-full bg-white/20 hover:bg-white/30 transition-colors duration-300 flex items-center justify-center"
      >
        <i class="fas fa-times"></i>
      </button>
    </div>

    <!-- Cart Content -->
    <div class="flex flex-col h-full">
      <!-- Cart Items -->
      <div class="flex-1 overflow-y-auto p-6">
        <div v-if="cartItems.length === 0" class="text-center py-12">
          <div class="w-20 h-20 bg-gray-100 rounded-full flex items-center justify-center mx-auto mb-4">
            <i class="fas fa-shopping-cart text-gray-400 text-2xl"></i>
          </div>
          <h3 class="text-lg font-semibold text-gray-900 mb-2">Seu carrinho está vazio</h3>
          <p class="text-gray-600 mb-6">Adicione alguns produtos para começar</p>
          <button 
            @click="$emit('close-cart')"
            class="btn-primary"
          >
            Continuar Comprando
          </button>
        </div>

        <div v-else class="space-y-4">
          <div 
            v-for="item in cartItems"
            :key="item.id"
            class="cart-item bg-gray-50 rounded-xl p-4 border border-gray-200"
          >
            <!-- Product Image -->
            <div class="flex space-x-4">
              <div class="w-16 h-16 rounded-lg overflow-hidden flex-shrink-0">
                <img 
                  :src="item.image" 
                  :alt="item.name"
                  class="w-full h-full object-cover"
                >
              </div>
              
              <!-- Product Info -->
              <div class="flex-1 min-w-0">
                <h3 class="text-sm font-semibold text-gray-900 mb-1 line-clamp-2">
                  {{ item.name }}
                </h3>
                <p class="text-sm text-gray-600 mb-2">
                  {{ getCategoryName(item.category) }}
                </p>
                
                <!-- Price and Quantity -->
                <div class="flex items-center justify-between">
                  <div class="flex items-baseline space-x-1">
                    <span class="text-sm font-medium text-gray-500">R$</span>
                    <span class="text-lg font-bold text-gray-900">{{ formatPrice(item.price) }}</span>
                  </div>
                  
                  <!-- Quantity Controls -->
                  <div class="flex items-center space-x-2">
                    <button 
                      @click="$emit('update-quantity', item.id, item.quantity - 1)"
                      class="w-8 h-8 rounded-full bg-gray-200 hover:bg-gray-300 transition-colors duration-300 flex items-center justify-center"
                    >
                      <i class="fas fa-minus text-xs"></i>
                    </button>
                    
                    <span class="w-8 text-center font-semibold">{{ item.quantity }}</span>
                    
                    <button 
                      @click="$emit('update-quantity', item.id, item.quantity + 1)"
                      class="w-8 h-8 rounded-full bg-gray-200 hover:bg-gray-300 transition-colors duration-300 flex items-center justify-center"
                    >
                      <i class="fas fa-plus text-xs"></i>
                    </button>
                  </div>
                </div>
                
                <!-- Remove Button -->
                <button 
                  @click="$emit('remove-item', item.id)"
                  class="mt-2 text-red-500 hover:text-red-700 text-sm font-medium transition-colors duration-300 flex items-center space-x-1"
                >
                  <i class="fas fa-trash text-xs"></i>
                  <span>Remover</span>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Cart Summary -->
      <div v-if="cartItems.length > 0" class="border-t border-gray-200 p-6 bg-gray-50">
        <!-- Summary -->
        <div class="space-y-3 mb-6">
          <div class="flex justify-between text-sm">
            <span class="text-gray-600">Subtotal</span>
            <span class="font-semibold">R$ {{ formatPrice(subtotal) }}</span>
          </div>
          
          <div class="flex justify-between text-sm">
            <span class="text-gray-600">Entrega</span>
            <span class="font-semibold text-green-600">Grátis</span>
          </div>
          
          <div class="border-t border-gray-300 pt-3">
            <div class="flex justify-between">
              <span class="text-lg font-bold text-gray-900">Total</span>
              <span class="text-xl font-bold text-gray-900">R$ {{ formatPrice(subtotal) }}</span>
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
            @click="$emit('clear-cart')"
            class="w-full bg-gray-200 hover:bg-gray-300 text-gray-700 font-medium py-3 px-6 rounded-xl transition-colors duration-300"
          >
            Limpar Carrinho
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed } from 'vue'

export default {
  name: 'CartSidebar',
  props: {
    isOpen: {
      type: Boolean,
      default: false
    },
    cartItems: {
      type: Array,
      default: () => []
    }
  },
  emits: ['close-cart', 'update-quantity', 'remove-item', 'clear-cart'],
  setup(props) {
    const subtotal = computed(() => {
      return props.cartItems.reduce((total, item) => {
        return total + (item.price * item.quantity)
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

    const proceedToCheckout = () => {
      if (props.cartItems.length === 0) return

      // Create WhatsApp message
      let message = '🌻 *Olá! Gostaria de finalizar meu pedido na Girassol GS:*\n\n'
      
      props.cartItems.forEach((item, index) => {
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

    return {
      subtotal,
      getCategoryName,
      formatPrice,
      proceedToCheckout
    }
  }
}
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.cart-item {
  transition: all 0.3s ease;
}

.cart-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

/* Custom scrollbar */
.overflow-y-auto::-webkit-scrollbar {
  width: 4px;
}

.overflow-y-auto::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 2px;
}

.overflow-y-auto::-webkit-scrollbar-thumb {
  background: linear-gradient(135deg, #FFD700, #FFA500);
  border-radius: 2px;
}

.overflow-y-auto::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(135deg, #FFA500, #FF8C00);
}

/* Button styles */
.btn-primary {
  @apply bg-gradient-to-r from-primary-500 to-primary-600 text-white font-semibold py-3 px-6 rounded-xl shadow-medium hover:shadow-heavy hover:scale-105 transition-all duration-300;
}

/* Animation for cart items */
.cart-item {
  animation: slideIn 0.3s ease-out;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateX(20px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}
</style>
