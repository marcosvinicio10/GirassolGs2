<template>
        <header
          class="fixed top-2 sm:top-4 left-1/2 transform -translate-x-1/2 z-50 transition-all duration-300 w-full max-w-6xl mx-2 sm:mx-4 hidden md:block"
          :class="headerClasses"
        >
    <div class="px-4 sm:px-6 lg:px-8">
      <div class="flex items-center justify-between h-14 lg:h-16">
        <!-- Logo -->
        <div class="flex items-center space-x-2">
          <div class="w-10 h-10 lg:w-12 lg:h-12 bg-gradient-to-br from-primary-500 to-primary-600 rounded-xl flex items-center justify-center shadow-medium">
            <i class="fas fa-sun text-white text-lg lg:text-xl"></i>
          </div>
          <div class="hidden sm:block">
            <h1 class="text-xl lg:text-2xl font-bold text-gradient">
              Girassol GS
            </h1>
            <p class="text-xs text-gray-600 hidden lg:block">
              Flores que transformam momentos
            </p>
          </div>
        </div>

        <!-- Navigation -->
        <nav class="hidden md:flex items-center space-x-6">
          <router-link 
            to="/"
            class="nav-link"
            :class="{ 'text-primary-600': $route.name === 'Home' }"
          >
            Início
          </router-link>
          <button 
            @click="$emit('scroll-to-section', 'products')"
            class="nav-link"
          >
            Produtos
          </button>
          <button 
            @click="$emit('scroll-to-section', 'about')"
            class="nav-link"
          >
            Sobre
          </button>
          <button 
            @click="$emit('scroll-to-section', 'contact')"
            class="nav-link"
          >
            Contato
          </button>
        </nav>

        <!-- Cart Button -->
        <div class="flex items-center space-x-4">
          <!-- WhatsApp Button (Mobile) -->
          <a 
            href="https://wa.me/5511999999999?text=Olá! Gostaria de saber mais sobre os produtos da Girassol GS"
            target="_blank"
            class="md:hidden bg-green-500 hover:bg-green-600 text-white p-2 rounded-lg transition-colors duration-300"
          >
            <i class="fab fa-whatsapp text-lg"></i>
          </a>

          <!-- Cart Button -->
          <router-link 
            to="/carrinho"
            class="relative bg-primary-500 hover:bg-primary-600 text-white rounded-xl p-3 transition-all duration-300 group shadow-medium hover:shadow-heavy"
          >
            <i class="fas fa-shopping-cart text-lg"></i>
            <span 
              v-if="cartItems.length > 0"
              class="absolute -top-2 -right-2 bg-red-500 text-white text-xs font-bold rounded-full w-6 h-6 flex items-center justify-center animate-pulse-slow"
            >
              {{ cartItems.length }}
            </span>
          </router-link>

          <!-- Mobile Menu Button -->
          <button 
            @click="toggleMobileMenu"
            class="md:hidden text-gray-700 p-2 rounded-lg hover:bg-gray-100 transition-colors duration-300"
          >
            <i class="fas fa-bars text-lg"></i>
          </button>
        </div>
      </div>

      <!-- Mobile Menu -->
      <div 
        v-if="isMobileMenuOpen"
        class="md:hidden absolute top-full left-0 right-0 mt-2 bg-white/95 backdrop-blur-md border border-white/20 shadow-heavy rounded-xl"
      >
        <div class="px-4 py-6 space-y-4">
          <button 
            @click="navigateAndClose('home')"
            class="block w-full text-left text-gray-800 hover:text-primary-500 font-medium py-2 transition-colors duration-300"
          >
            Início
          </button>
          <button 
            @click="navigateAndClose('products')"
            class="block w-full text-left text-gray-800 hover:text-primary-500 font-medium py-2 transition-colors duration-300"
          >
            Produtos
          </button>
          <button 
            @click="navigateAndClose('about')"
            class="block w-full text-left text-gray-800 hover:text-primary-500 font-medium py-2 transition-colors duration-300"
          >
            Sobre
          </button>
          <button 
            @click="navigateAndClose('contact')"
            class="block w-full text-left text-gray-800 hover:text-primary-500 font-medium py-2 transition-colors duration-300"
          >
            Contato
          </button>
          
          <!-- WhatsApp Button (Mobile Menu) -->
          <a 
            href="https://wa.me/5511999999999?text=Olá! Gostaria de saber mais sobre os produtos da Girassol GS"
            target="_blank"
            class="flex items-center space-x-2 bg-green-500 hover:bg-green-600 text-white px-4 py-3 rounded-lg transition-colors duration-300 mt-4"
          >
            <i class="fab fa-whatsapp"></i>
            <span>Falar no WhatsApp</span>
          </a>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import { ref, computed, onMounted, onUnmounted } from 'vue'

export default {
  name: 'Header',
  props: {
    cartItems: {
      type: Array,
      default: () => []
    }
  },
  emits: ['toggle-cart', 'scroll-to-section'],
  setup(props, { emit }) {
    const isScrolled = ref(false)
    const isMobileMenuOpen = ref(false)

    const headerClasses = computed(() => {
      return {
        'bg-white/95 backdrop-blur-md shadow-heavy border border-white/20 rounded-2xl': true,
        'text-gray-900': true
      }
    })

    const handleScroll = () => {
      isScrolled.value = window.scrollY > 50
    }

    const toggleMobileMenu = () => {
      isMobileMenuOpen.value = !isMobileMenuOpen.value
    }

    const navigateAndClose = (section) => {
      emit('scroll-to-section', section)
      isMobileMenuOpen.value = false
    }

    onMounted(() => {
      window.addEventListener('scroll', handleScroll)
    })

    onUnmounted(() => {
      window.removeEventListener('scroll', handleScroll)
    })

    return {
      isScrolled,
      isMobileMenuOpen,
      headerClasses,
      toggleMobileMenu,
      navigateAndClose
    }
  }
}
</script>

<style scoped>
.nav-link {
  @apply text-gray-700 hover:text-primary-600 font-medium transition-colors duration-300 relative;
}

.nav-link::after {
  content: '';
  position: absolute;
  bottom: -4px;
  left: 0;
  width: 0;
  height: 2px;
  background: linear-gradient(90deg, #FFD700, #FFA500);
  transition: width 0.3s ease;
}

.nav-link:hover::after {
  width: 100%;
}

/* Active nav link */
.nav-link.router-link-active {
  @apply text-primary-600;
}

.nav-link.router-link-active::after {
  width: 100%;
}
</style>
