<template>
  <!-- Mobile Bottom Navigation -->
  <div class="fixed bottom-0 left-0 right-0 z-50 md:hidden">
    <div class="bg-white/95 backdrop-blur-md border-t border-gray-200 shadow-lg">
      <div class="flex items-center justify-around py-3">
        <!-- Home -->
        <router-link 
          to="/"
          class="flex flex-col items-center space-y-1 p-3 rounded-xl transition-all duration-300 min-w-0 flex-1"
          :class="{ 'text-primary-600 bg-primary-50': $route.name === 'Home', 'text-gray-600 hover:text-primary-600 hover:bg-primary-50': $route.name !== 'Home' }"
        >
          <i class="fas fa-home text-lg"></i>
          <span class="text-xs font-medium">Início</span>
        </router-link>

        <!-- Categories -->
        <button 
          @click="scrollToProducts"
          class="flex flex-col items-center space-y-1 p-3 rounded-xl transition-all duration-300 text-gray-600 hover:text-primary-600 hover:bg-primary-50 min-w-0 flex-1"
        >
          <i class="fas fa-shopping-bag text-lg"></i>
          <span class="text-xs font-medium">Produtos</span>
        </button>

        <!-- Cart -->
        <router-link 
          to="/carrinho"
          class="relative flex flex-col items-center space-y-1 p-3 rounded-xl transition-all duration-300 min-w-0 flex-1"
          :class="{ 'text-primary-600 bg-primary-50': $route.name === 'Cart', 'text-gray-600 hover:text-primary-600 hover:bg-primary-50': $route.name !== 'Cart' }"
        >
          <i class="fas fa-shopping-cart text-lg"></i>
          <span class="text-xs font-medium">Carrinho</span>
          <span 
            v-if="cartCount > 0"
            class="absolute -top-1 -right-1 bg-red-500 text-white text-xs font-bold rounded-full w-5 h-5 flex items-center justify-center animate-pulse"
          >
            {{ cartCount }}
          </span>
        </router-link>

        <!-- WhatsApp -->
        <a 
          href="https://wa.me/5511999999999?text=Olá! Gostaria de saber mais sobre os produtos da Girassol GS"
          target="_blank"
          class="flex flex-col items-center space-y-1 p-3 rounded-xl transition-all duration-300 text-green-600 hover:text-green-700 hover:bg-green-50 min-w-0 flex-1"
        >
          <i class="fab fa-whatsapp text-lg"></i>
          <span class="text-xs font-medium">WhatsApp</span>
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'

export default {
  name: 'MobileNavigation',
  setup() {
    const cartCount = ref(0)

    const scrollToProducts = () => {
      if (window.location.pathname === '/') {
        const element = document.getElementById('products')
        if (element) {
          element.scrollIntoView({ behavior: 'smooth' })
        }
      } else {
        // Navigate to home and then scroll
        window.location.href = '/#products'
      }
    }

    const updateCartCount = () => {
      const cart = JSON.parse(localStorage.getItem('girassol-cart') || '[]')
      cartCount.value = cart.reduce((total, item) => total + item.quantity, 0)
    }

    onMounted(() => {
      updateCartCount()
      
      // Listen for cart updates
      window.addEventListener('storage', updateCartCount)
      
      // Update cart count periodically
      setInterval(updateCartCount, 1000)
    })

    return {
      cartCount,
      scrollToProducts
    }
  }
}
</script>

<style scoped>
/* Mobile navigation styles */
.router-link-active {
  @apply text-primary-600;
}

/* Touch improvements */
@media (hover: none) and (pointer: coarse) {
  .flex {
    min-height: 44px;
  }
}
</style>
