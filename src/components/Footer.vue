<template>
  <footer class="bg-gradient-to-br from-gray-900 via-gray-800 to-gray-900 text-white">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <!-- Main Footer Content -->
      <div class="py-16 lg:py-20">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8 lg:gap-12">
          <!-- Brand Section -->
          <div class="lg:col-span-2">
            <div class="flex items-center space-x-3 mb-6">
              <div class="w-12 h-12 bg-gradient-to-br from-primary-500 to-primary-600 rounded-xl flex items-center justify-center shadow-medium">
                <i class="fas fa-sun text-white text-xl"></i>
              </div>
              <div>
                <h3 class="text-2xl font-bold text-gradient">Girassol GS</h3>
                <p class="text-gray-400 text-sm">Flores que transformam momentos</p>
              </div>
            </div>
            
            <p class="text-gray-300 mb-6 leading-relaxed max-w-md">
              Especializada em buquês, caixas de bombom e flores unitárias. 
              Cada produto é criado com amor e cuidado para tornar seus momentos especiais ainda mais únicos.
            </p>
            
            <!-- Social Links -->
            <div class="flex space-x-4">
              <a 
                href="https://wa.me/5511999999999"
                target="_blank"
                class="social-link bg-green-500 hover:bg-green-600"
                title="WhatsApp"
              >
                <i class="fab fa-whatsapp"></i>
              </a>
              
              <a 
                href="https://instagram.com/girassolgs"
                target="_blank"
                class="social-link bg-pink-500 hover:bg-pink-600"
                title="Instagram"
              >
                <i class="fab fa-instagram"></i>
              </a>
              
              <a 
                href="https://facebook.com/girassolgs"
                target="_blank"
                class="social-link bg-blue-600 hover:bg-blue-700"
                title="Facebook"
              >
                <i class="fab fa-facebook-f"></i>
              </a>
            </div>
          </div>

          <!-- Quick Links -->
          <div>
            <h4 class="text-lg font-semibold mb-6">Links Rápidos</h4>
            <ul class="space-y-3">
              <li>
                <button 
                  @click="scrollToSection('home')"
                  class="footer-link"
                >
                  <i class="fas fa-home mr-2"></i>
                  Início
                </button>
              </li>
              <li>
                <button 
                  @click="scrollToSection('products')"
                  class="footer-link"
                >
                  <i class="fas fa-shopping-bag mr-2"></i>
                  Produtos
                </button>
              </li>
              <li>
                <button 
                  @click="scrollToSection('about')"
                  class="footer-link"
                >
                  <i class="fas fa-info-circle mr-2"></i>
                  Sobre Nós
                </button>
              </li>
              <li>
                <button 
                  @click="scrollToSection('contact')"
                  class="footer-link"
                >
                  <i class="fas fa-envelope mr-2"></i>
                  Contato
                </button>
              </li>
            </ul>
          </div>

          <!-- Contact Info -->
          <div>
            <h4 class="text-lg font-semibold mb-6">Contato</h4>
            <ul class="space-y-4">
              <li class="flex items-start space-x-3">
                <div class="w-8 h-8 bg-primary-500/20 rounded-full flex items-center justify-center flex-shrink-0 mt-1">
                  <i class="fas fa-phone text-primary-400 text-sm"></i>
                </div>
                <div>
                  <p class="text-gray-300 text-sm">Telefone</p>
                  <a href="tel:+5511999999999" class="text-white hover:text-primary-400 transition-colors duration-300">
                    (11) 99999-9999
                  </a>
                </div>
              </li>
              
              <li class="flex items-start space-x-3">
                <div class="w-8 h-8 bg-primary-500/20 rounded-full flex items-center justify-center flex-shrink-0 mt-1">
                  <i class="fas fa-envelope text-primary-400 text-sm"></i>
                </div>
                <div>
                  <p class="text-gray-300 text-sm">Email</p>
                  <a href="mailto:contato@girassolgs.com" class="text-white hover:text-primary-400 transition-colors duration-300">
                    contato@girassolgs.com
                  </a>
                </div>
              </li>
              
              <li class="flex items-start space-x-3">
                <div class="w-8 h-8 bg-primary-500/20 rounded-full flex items-center justify-center flex-shrink-0 mt-1">
                  <i class="fas fa-map-marker-alt text-primary-400 text-sm"></i>
                </div>
                <div>
                  <p class="text-gray-300 text-sm">Endereço</p>
                  <p class="text-white text-sm">
                    São Paulo, SP<br>
                    Brasil
                  </p>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>

      <!-- Bottom Bar -->
      <div class="border-t border-gray-700 py-6">
        <div class="flex flex-col md:flex-row items-center justify-between space-y-4 md:space-y-0">
          <div class="text-gray-400 text-sm">
            © 2025 Girassol GS. Todos os direitos reservados.
          </div>
          
          <div class="flex items-center space-x-2 text-gray-400 text-sm">
            <span>Desenvolvido com</span>
            <i class="fas fa-heart text-red-500 animate-heartbeat"></i>
            <span>por</span>
            <a 
              href="https://github.com/marcosvinicio"
              target="_blank"
              class="text-primary-400 hover:text-primary-300 transition-colors duration-300 font-medium"
            >
              Marcos Vinicio
            </a>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
  setup() {
    const scrollToSection = (sectionId) => {
      const element = document.getElementById(sectionId)
      if (element) {
        element.scrollIntoView({ behavior: 'smooth' })
      }
    }

    return {
      scrollToSection
    }
  }
}
</script>

<style scoped>
/* Custom gradient text */
.text-gradient {
  background: linear-gradient(135deg, #FFD700 0%, #FFA500 50%, #FF8C00 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Social links */
.social-link {
  @apply w-10 h-10 rounded-full flex items-center justify-center transition-all duration-300 hover:scale-110 hover:shadow-lg;
}

/* Footer links */
.footer-link {
  @apply text-gray-300 hover:text-primary-400 transition-colors duration-300 flex items-center text-sm;
}

/* Heartbeat animation */
.animate-heartbeat {
  animation: heartbeat 1.5s ease-in-out infinite;
}

@keyframes heartbeat {
  0%, 100% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
}

/* Hover effects */
.social-link:hover {
  transform: translateY(-2px) scale(1.1);
}

.footer-link:hover {
  transform: translateX(4px);
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .grid {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
  
  .lg\:col-span-2 {
    grid-column: span 1;
  }
}
</style>
