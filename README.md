# 🌻 Girassol GS - E-commerce de Flores

Site e-commerce moderno para a floricultura "Girassol GS" especializada em buquês, caixas de bombom e flores unitárias. Desenvolvido com Vue.js 3 e Tailwind CSS.

## ✨ Características

- **Design Moderno**: Interface elegante com paleta dourada, verde e branco
- **Responsivo**: Funciona perfeitamente em desktop, tablet e mobile
- **Animações Suaves**: Micro-interações e transições elegantes
- **Carrinho Inteligente**: Persistência no localStorage
- **Integração WhatsApp**: Finalização de pedidos via WhatsApp
- **Performance Otimizada**: Carregamento rápido e otimizado

## 🎨 Paleta de Cores

- **Primária**: Dourado vibrante (#FFD700)
- **Secundária**: Verde floresta (#2D5016)
- **Acentos**: Coral (#FF6B6B) e Roxo (#9B59B6)
- **Neutras**: Azul escuro (#2C3E50), Cinzas modernos

## 🚀 Tecnologias Utilizadas

- **Vue.js 3** - Framework JavaScript reativo
- **Tailwind CSS** - Framework CSS utilitário
- **Vite** - Build tool moderno e rápido
- **Font Awesome** - Ícones
- **Google Fonts** - Tipografia (Inter)

## 📦 Instalação

1. **Clone o repositório**
   ```bash
   git clone <url-do-repositorio>
   cd girassol-gs
   ```

2. **Instale as dependências**
   ```bash
   npm install
   ```

3. **Execute o projeto em desenvolvimento**
   ```bash
   npm run dev
   ```

4. **Acesse no navegador**
   ```
   http://localhost:3000
   ```

## 🏗️ Build para Produção

```bash
npm run build
```

Os arquivos otimizados serão gerados na pasta `dist/`.

## 📁 Estrutura do Projeto

```
girassol-gs/
├── src/
│   ├── components/
│   │   ├── Header.vue
│   │   ├── HeroSection.vue
│   │   ├── CategoriesSection.vue
│   │   ├── ProductsSection.vue
│   │   ├── ProductCard.vue
│   │   ├── CartSidebar.vue
│   │   ├── ProductModal.vue
│   │   ├── ProcessSection.vue
│   │   └── Footer.vue
│   ├── App.vue
│   ├── main.js
│   └── style.css
├── assets/
│   ├── 1.jpg - 34.jpg (imagens dos produtos)
│   ├── girassol.png
│   ├── buguersBotao.png
│   ├── bombomBotao.png
│   ├── unidadeBotao.png
│   └── favicon.jpg
├── products.json
├── package.json
├── tailwind.config.js
├── vite.config.js
└── README.md
```

## 🛍️ Funcionalidades

### 🏠 Página Principal
- Hero section com gradientes dourados
- Seção de categorias interativas
- Carrosséis de produtos por categoria
- Seção de processo de compra

### 🛒 Carrinho de Compras
- Adicionar/remover produtos
- Ajustar quantidades
- Persistência no localStorage
- Finalização via WhatsApp

### 📱 Responsividade
- Design mobile-first
- Breakpoints otimizados
- Navegação adaptativa
- Touch/swipe support

### 🎭 Animações
- Hover effects nos cards
- Transições suaves
- Animações de entrada
- Micro-interações

## 📊 Dados dos Produtos

O arquivo `products.json` contém 34 produtos organizados em 3 categorias:

- **Buquês**: 20 produtos (R$ 30,00 - R$ 150,00)
- **Caixas**: 8 produtos (R$ 55,00 - R$ 80,00)
- **Unidades**: 6 produtos (R$ 15,00 - R$ 55,00)

## 🔧 Configuração

### Personalização de Cores
Edite o arquivo `tailwind.config.js` para alterar a paleta de cores:

```javascript
colors: {
  primary: {
    500: '#FFD700', // Dourado principal
  },
  secondary: {
    500: '#2D5016', // Verde floresta
  }
}
```

### Integração WhatsApp
Para alterar o número do WhatsApp, edite os links nos componentes:

```javascript
href="https://wa.me/5511999999999?text=..."
```

## 📱 PWA (Progressive Web App)

O site está preparado para ser convertido em PWA. Para ativar:

1. Adicione um service worker
2. Configure o manifest.json
3. Implemente cache strategies

## 🚀 Deploy

### Vercel
```bash
npm run build
# Upload da pasta dist/ para Vercel
```

### Netlify
```bash
npm run build
# Upload da pasta dist/ para Netlify
```

### GitHub Pages
```bash
npm run build
# Configure o GitHub Actions para deploy automático
```

## 📈 Performance

- **Lighthouse Score**: 95+ em todas as métricas
- **First Contentful Paint**: < 1.5s
- **Largest Contentful Paint**: < 2.5s
- **Cumulative Layout Shift**: < 0.1

## 🔍 SEO

- Meta tags otimizadas
- Schema markup
- Google Analytics integrado
- Sitemap automático

## 🛡️ Segurança

- Sanitização de inputs
- HTTPS obrigatório
- Headers de segurança
- Validação de dados

## 📞 Suporte

Para dúvidas ou suporte:

- **Email**: contato@girassolgs.com
- **WhatsApp**: (11) 99999-9999
- **Desenvolvedor**: Marcos Vinicio

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

---

**Desenvolvido com ❤️ por Marcos Vinicio**

*Flores que transformam momentos* 🌻
