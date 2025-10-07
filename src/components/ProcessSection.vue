<template>
  <section class="py-16 lg:py-24 bg-gradient-to-b from-white to-gray-50">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <!-- Section Header -->
      <div class="text-center mb-12 lg:mb-16">
        <h2 class="text-3xl sm:text-4xl lg:text-5xl font-bold text-gray-900 mb-4">
          Como <span class="text-gradient">Funciona</span>
        </h2>
        <p class="text-lg sm:text-xl text-gray-600 max-w-2xl mx-auto">
          Processo simples e rápido para garantir sua satisfação
        </p>
      </div>

      <!-- Process Steps -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8 lg:gap-12 max-w-5xl mx-auto">
        <!-- Step 1 -->
        <div class="process-step text-center">
          <div class="relative mb-8">
            <!-- Step Number -->
            <div class="w-20 h-20 bg-gradient-to-br from-primary-400 to-primary-600 rounded-full flex items-center justify-center mx-auto mb-6 shadow-heavy relative z-10">
              <span class="text-2xl font-bold text-white">1</span>
            </div>
            
            <!-- Connecting Line (hidden on mobile) -->
            <div class="hidden md:block absolute top-10 left-1/2 w-full h-0.5 bg-gradient-to-r from-primary-400 to-transparent transform translate-x-10"></div>
            
            <!-- Icon -->
            <div class="w-16 h-16 bg-white rounded-2xl flex items-center justify-center mx-auto shadow-medium border-4 border-primary-100">
              <i class="fas fa-mouse-pointer text-primary-600 text-2xl"></i>
            </div>
          </div>
          
          <h3 class="text-xl font-bold text-gray-900 mb-4">Escolha seu Produto</h3>
          <p class="text-gray-600 leading-relaxed">
            Navegue por nossa coleção e selecione o produto perfeito para sua ocasião especial
          </p>
        </div>

        <!-- Step 2 -->
        <div class="process-step text-center">
          <div class="relative mb-8">
            <!-- Step Number -->
            <div class="w-20 h-20 bg-gradient-to-br from-secondary-400 to-secondary-600 rounded-full flex items-center justify-center mx-auto mb-6 shadow-heavy relative z-10">
              <span class="text-2xl font-bold text-white">2</span>
            </div>
            
            <!-- Connecting Line (hidden on mobile) -->
            <div class="hidden md:block absolute top-10 left-1/2 w-full h-0.5 bg-gradient-to-r from-secondary-400 to-transparent transform translate-x-10"></div>
            
            <!-- Icon -->
            <div class="w-16 h-16 bg-white rounded-2xl flex items-center justify-center mx-auto shadow-medium border-4 border-secondary-100">
              <i class="fab fa-whatsapp text-secondary-600 text-2xl"></i>
            </div>
          </div>
          
          <h3 class="text-xl font-bold text-gray-900 mb-4">Faça seu Pedido</h3>
          <p class="text-gray-600 leading-relaxed">
            Entre em contato pelo WhatsApp e confirme todos os detalhes do seu pedido
          </p>
        </div>

        <!-- Step 3 -->
        <div class="process-step text-center">
          <div class="relative mb-8">
            <!-- Step Number -->
            <div class="w-20 h-20 bg-gradient-to-br from-accent-coral to-pink-500 rounded-full flex items-center justify-center mx-auto mb-6 shadow-heavy relative z-10">
              <span class="text-2xl font-bold text-white">3</span>
            </div>
            
            <!-- Icon -->
            <div class="w-16 h-16 bg-white rounded-2xl flex items-center justify-center mx-auto shadow-medium border-4 border-pink-100">
              <i class="fas fa-shipping-fast text-accent-coral text-2xl"></i>
            </div>
          </div>
          
          <h3 class="text-xl font-bold text-gray-900 mb-4">Receba com Amor</h3>
          <p class="text-gray-600 leading-relaxed">
            Aguarde a entrega e receba seu produto com todo o carinho e cuidado
          </p>
        </div>
      </div>

      <!-- CTA Section -->
      <div class="mt-16 text-center">
        <div class="bg-gradient-to-r from-primary-500 to-primary-600 rounded-3xl p-8 lg:p-12 text-white">
          <div class="max-w-3xl mx-auto">
            <div class="w-20 h-20 bg-white/20 rounded-full flex items-center justify-center mx-auto mb-6">
              <i class="fas fa-heart text-3xl"></i>
            </div>
            <h3 class="text-2xl lg:text-3xl font-bold mb-4">
              Pronto para fazer seu pedido?
            </h3>
            <p class="text-lg mb-8 opacity-90">
              Entre em contato conosco agora mesmo e vamos criar algo especial para você!
            </p>
            <a 
              href="https://wa.me/5511999999999?text=Olá! Gostaria de fazer um pedido na Girassol GS"
              target="_blank"
              class="inline-flex items-center space-x-3 bg-white text-primary-600 font-semibold py-4 px-8 rounded-xl shadow-heavy hover:shadow-xl hover:scale-105 transition-all duration-300"
            >
              <i class="fab fa-whatsapp text-xl"></i>
              <span>Falar no WhatsApp</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'ProcessSection'
}
</script>

<style scoped>
.process-step {
  @apply transition-all duration-500 hover:scale-105;
}

/* Custom gradient text */
.text-gradient {
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 50%, #FF8C00 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Enhanced hover effects */
.process-step:hover .w-16 {
  transform: scale(1.1) rotate(5deg);
}

.process-step:hover .w-20 {
  transform: scale(1.05);
}

/* Animation for step numbers */
.w-20 {
  transition: all 0.3s ease;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .process-step {
    margin-bottom: 2rem;
  }
  
  .w-20 {
    width: 4rem;
    height: 4rem;
  }
  
  .w-16 {
    width: 3rem;
    height: 3rem;
  }
}
</style>
