<template>
  <section 
    id="home"
    class="relative min-h-screen flex items-center justify-center overflow-hidden bg-gradient-to-br from-primary-50 via-white to-secondary-50 hero-mobile"
  >
    <!-- Background Pattern -->
    <div class="absolute inset-0 bg-pattern opacity-30"></div>
    
    <!-- Floating Elements -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <!-- Girassol 1 -->
      <div class="absolute top-20 left-10 w-16 h-16 opacity-20 floating-element">
        <img src="/assets/girassol.png" alt="Girassol decorativo" class="w-full h-full object-contain">
      </div>
      
      <!-- Girassol 2 -->
      <div class="absolute top-40 right-20 w-20 h-20 opacity-15 floating-element">
        <img src="/assets/girassol.png" alt="Girassol decorativo" class="w-full h-full object-contain">
      </div>
      
      <!-- Girassol 3 -->
      <div class="absolute bottom-32 left-1/4 w-12 h-12 opacity-25 floating-element">
        <img src="/assets/girassol.png" alt="Girassol decorativo" class="w-full h-full object-contain">
      </div>
      
      <!-- Girassol 4 -->
      <div class="absolute bottom-20 right-1/3 w-14 h-14 opacity-20 floating-element">
        <img src="/assets/girassol.png" alt="Girassol decorativo" class="w-full h-full object-contain">
      </div>
      
      <!-- Girassol 5 -->
      <div class="absolute top-1/2 left-5 w-10 h-10 opacity-15 floating-element">
        <img src="/assets/girassol.png" alt="Girassol decorativo" class="w-full h-full object-contain">
      </div>
    </div>

    <!-- Main Content -->
    <div class="relative z-10 container mx-auto px-4 sm:px-6 lg:px-8 text-center">
      <div class="max-w-4xl mx-auto">
        <!-- Main Title -->
        <h1 class="hero-title font-bold text-3xl sm:text-4xl lg:text-6xl xl:text-7xl mb-4 sm:mb-6 text-shadow-lg">
          <span class="text-gradient">Girassol GS</span>
        </h1>
        
        <!-- Subtitle -->
        <p class="hero-subtitle text-lg sm:text-xl lg:text-3xl font-light text-gray-700 mb-3 sm:mb-4 text-shadow">
          Flores que transformam momentos
        </p>
        
        <!-- Description -->
        <p class="text-base sm:text-lg lg:text-xl text-gray-600 mb-8 sm:mb-12 max-w-2xl mx-auto leading-relaxed px-2">
          Descubra nossa coleção exclusiva de buquês, caixas de bombom e flores unitárias. 
          Cada produto é cuidadosamente selecionado para tornar seus momentos especiais ainda mais únicos.
        </p>
        
        <!-- CTA Buttons -->
        <div class="flex flex-col sm:flex-row items-center justify-center gap-3 sm:gap-6 px-4 sm:px-0">
          <button 
            @click="$emit('scroll-to-products')"
            class="btn-primary group relative overflow-hidden w-full sm:w-auto py-3 px-6 sm:py-4 sm:px-8 text-base sm:text-lg font-semibold"
          >
            <span class="relative z-10 flex items-center justify-center space-x-2">
              <i class="fas fa-shopping-bag"></i>
              <span>Ver Produtos</span>
            </span>
          </button>

          <a 
            href="https://wa.me/5511999999999?text=Olá! Gostaria de saber mais sobre os produtos da Girassol GS"
            target="_blank"
            class="btn-secondary group relative overflow-hidden w-full sm:w-auto py-3 px-6 sm:py-4 sm:px-8 text-base sm:text-lg font-semibold"
          >
            <span class="relative z-10 flex items-center justify-center space-x-2">
              <i class="fab fa-whatsapp"></i>
              <span>Falar no WhatsApp</span>
            </span>
          </a>
        </div>
        
        <!-- Features -->
        <div class="mt-12 sm:mt-16 grid grid-cols-1 sm:grid-cols-3 gap-4 sm:gap-6 lg:gap-8 px-4 sm:px-0">
          <div class="feature-card text-center">
            <div class="w-12 h-12 sm:w-16 sm:h-16 bg-gradient-to-br from-primary-400 to-primary-600 rounded-2xl flex items-center justify-center mx-auto mb-3 sm:mb-4 shadow-medium">
              <i class="fas fa-shipping-fast text-white text-lg sm:text-2xl"></i>
            </div>
            <h3 class="text-base sm:text-lg font-semibold text-gray-800 mb-2">Entrega Rápida</h3>
            <p class="text-gray-600 text-xs sm:text-sm">Entregamos em até 5 dias úteis</p>
          </div>
          
          <div class="feature-card text-center">
            <div class="w-12 h-12 sm:w-16 sm:h-16 bg-gradient-to-br from-secondary-400 to-secondary-600 rounded-2xl flex items-center justify-center mx-auto mb-3 sm:mb-4 shadow-medium">
              <i class="fas fa-heart text-white text-lg sm:text-2xl"></i>
            </div>
            <h3 class="text-base sm:text-lg font-semibold text-gray-800 mb-2">Feito com Amor</h3>
            <p class="text-gray-600 text-xs sm:text-sm">Cada produto é único e especial</p>
          </div>
          
          <div class="feature-card text-center">
            <div class="w-12 h-12 sm:w-16 sm:h-16 bg-gradient-to-br from-accent-coral to-pink-500 rounded-2xl flex items-center justify-center mx-auto mb-3 sm:mb-4 shadow-medium">
              <i class="fas fa-star text-white text-lg sm:text-2xl"></i>
            </div>
            <h3 class="text-base sm:text-lg font-semibold text-gray-800 mb-2">Qualidade Premium</h3>
            <p class="text-gray-600 text-xs sm:text-sm">Materiais selecionados e duradouros</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Scroll Indicator -->
    <div class="absolute bottom-8 left-1/2 transform -translate-x-1/2 animate-bounce">
      <div class="w-6 h-10 border-2 border-primary-500 rounded-full flex justify-center">
        <div class="w-1 h-3 bg-primary-500 rounded-full mt-2 animate-pulse"></div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'HeroSection',
  emits: ['scroll-to-products']
}
</script>

<style scoped>
.feature-card {
  @apply transition-all duration-300 hover:scale-105 hover:-translate-y-2 p-6 rounded-2xl bg-white/50 backdrop-blur-sm border border-white/30 shadow-light hover:shadow-medium;
}

/* Custom gradient text */
.text-gradient {
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 50%, #FF8C00 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Enhanced button hover effects */
.btn-primary:hover {
  transform: translateY(-2px) scale(1.05);
  box-shadow: 0 20px 40px rgba(255, 215, 0, 0.3);
}

.btn-secondary:hover {
  transform: translateY(-2px) scale(1.05);
  box-shadow: 0 20px 40px rgba(45, 80, 22, 0.3);
}

/* Floating animation delays */
.floating-element:nth-child(1) { animation-delay: 0s; }
.floating-element:nth-child(2) { animation-delay: -2s; }
.floating-element:nth-child(3) { animation-delay: -4s; }
.floating-element:nth-child(4) { animation-delay: -1s; }
.floating-element:nth-child(5) { animation-delay: -3s; }

</style>
