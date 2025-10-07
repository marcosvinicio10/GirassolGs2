<template>
  <div id="app" class="min-h-screen bg-white">
    <!-- Header -->
    <Header 
      :cart-items="cartItems" 
      @toggle-cart="toggleCart"
      @scroll-to-section="scrollToSection"
    />
    
    <!-- Router View -->
    <router-view />
    
    <!-- Cart Sidebar -->
    <CartSidebar 
      :is-open="isCartOpen"
      :cart-items="cartItems"
      @close-cart="toggleCart"
      @update-quantity="updateQuantity"
      @remove-item="removeItem"
      @clear-cart="clearCart"
    />
    
    <!-- Product Modal -->
    <ProductModal 
      :is-open="isProductModalOpen"
      :product="selectedProduct"
      @close-modal="closeProductModal"
      @add-to-cart="addToCart"
    />
    
    <!-- Footer -->
    <Footer />
    
    <!-- Mobile Navigation -->
    <MobileNavigation />
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import Header from './components/Header.vue'
import CartSidebar from './components/CartSidebar.vue'
import ProductModal from './components/ProductModal.vue'
import Footer from './components/Footer.vue'
import MobileNavigation from './components/MobileNavigation.vue'

export default {
  name: 'App',
  components: {
    Header,
    CartSidebar,
    ProductModal,
    Footer,
    MobileNavigation
  },
  setup() {
    // Reactive data
    const cartItems = ref([])
    const isCartOpen = ref(false)
    const isProductModalOpen = ref(false)
    const selectedProduct = ref(null)

    // Methods
    const scrollToSection = (sectionId) => {
      const element = document.getElementById(sectionId)
      if (element) {
        element.scrollIntoView({ behavior: 'smooth' })
      }
    }

    const addToCart = (product) => {
      const existingItem = cartItems.value.find(item => item.id === product.id)
      
      if (existingItem) {
        existingItem.quantity += 1
      } else {
        cartItems.value.push({
          ...product,
          quantity: 1
        })
      }
      
      // Save to localStorage
      localStorage.setItem('girassol-cart', JSON.stringify(cartItems.value))
      
      // Show success feedback
      showCartFeedback()
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

    const toggleCart = () => {
      isCartOpen.value = !isCartOpen.value
    }

    const viewProduct = (product) => {
      selectedProduct.value = product
      isProductModalOpen.value = true
    }

    const closeProductModal = () => {
      isProductModalOpen.value = false
      selectedProduct.value = null
    }

    const showCartFeedback = () => {
      // Create a temporary notification
      const notification = document.createElement('div')
      notification.className = 'fixed top-20 right-4 bg-green-500 text-white px-4 py-2 rounded-lg shadow-lg z-50 animate-slide-in'
      notification.textContent = 'Produto adicionado ao carrinho!'
      
      document.body.appendChild(notification)
      
      setTimeout(() => {
        notification.remove()
      }, 3000)
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
      isCartOpen,
      isProductModalOpen,
      selectedProduct,
      scrollToSection,
      addToCart,
      updateQuantity,
      removeItem,
      clearCart,
      toggleCart,
      viewProduct,
      closeProductModal
    }
  }
}
</script>

<style scoped>
/* Additional app-specific styles can go here */
</style>
