<template>
  <div class="home-page">
    <!-- Hero Section -->
    <HeroSection @scroll-to-products="scrollToSection('products')" />
    
    <!-- Categories Section -->
    <CategoriesSection 
      @select-category="selectCategory"
      :active-category="activeCategory"
    />
    
    <!-- Products Section -->
    <ProductsSection 
      :products="filteredProducts"
      :active-category="activeCategory"
      @add-to-cart="addToCart"
      @view-product="viewProduct"
      @select-category="selectCategory"
    />
    
    <!-- Process Section -->
    <ProcessSection />
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import HeroSection from '../components/HeroSection.vue'
import CategoriesSection from '../components/CategoriesSection.vue'
import ProductsSection from '../components/ProductsSection.vue'
import ProcessSection from '../components/ProcessSection.vue'
import productsData from '../products.json'

export default {
  name: 'Home',
  components: {
    HeroSection,
    CategoriesSection,
    ProductsSection,
    ProcessSection
  },
  setup() {
    const router = useRouter()
    const products = ref(productsData.products)
    const activeCategory = ref('all')

    const filteredProducts = computed(() => {
      if (activeCategory.value === 'all') {
        return products.value
      }
      return products.value.filter(product => product.category === activeCategory.value)
    })

    const selectCategory = (category) => {
      activeCategory.value = category
      scrollToSection('products')
    }

    const scrollToSection = (sectionId) => {
      const element = document.getElementById(sectionId)
      if (element) {
        element.scrollIntoView({ behavior: 'smooth' })
      }
    }

    const addToCart = (product) => {
      // Emit to parent or use store
      const existingCart = JSON.parse(localStorage.getItem('girassol-cart') || '[]')
      const existingItem = existingCart.find(item => item.id === product.id)
      
      if (existingItem) {
        existingItem.quantity += 1
      } else {
        existingCart.push({
          ...product,
          quantity: 1
        })
      }
      
      localStorage.setItem('girassol-cart', JSON.stringify(existingCart))
      showCartFeedback()
    }

    const viewProduct = (product) => {
      router.push({ name: 'Product', params: { id: product.id } })
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

    // Load cart from localStorage on mount
    onMounted(() => {
      // Save all products to localStorage for category counting
      localStorage.setItem('girassol-all-products', JSON.stringify(products.value))
    })

    return {
      products,
      activeCategory,
      filteredProducts,
      selectCategory,
      scrollToSection,
      addToCart,
      viewProduct
    }
  }
}
</script>

<style scoped>
.home-page {
  min-height: 100vh;
}
</style>
