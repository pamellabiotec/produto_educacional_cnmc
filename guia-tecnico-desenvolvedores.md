# Guia Técnico para Desenvolvedores - Reino dos Elementos

## 📋 Visão Geral Técnica

Este documento fornece instruções detalhadas para desenvolvedores que desejam implementar, personalizar ou contribuir com o produto educacional "Reino dos Elementos". O projeto utiliza tecnologias web modernas e integração com H5P para criar uma experiência educacional gamificada.

## 🏗️ Arquitetura do Sistema

### Stack Tecnológico

```
Frontend:
├── 🌐 HTML5 - Estrutura semântica
├── 🎨 CSS3 - Estilos e animações
├── ⚡ JavaScript ES6+ - Interatividade
├── 📱 H5P Framework - Conteúdo interativo
└── 🎮 Web APIs - Gamificação e persistência

Backend (Opcional):
├── 🔄 Node.js - Servidor de desenvolvimento
├── 📊 Express.js - API REST
└── 🗄️ JSON/LocalStorage - Persistência local

Deployment:
├── 🌍 GitHub Pages - Hospedagem estática
├── 📦 GitHub Actions - CI/CD
└── 🔗 CDN - Recursos externos
```

### Estrutura de Diretórios Detalhada

```
produto-educacional-cnmc/
├── 📁 .github/                       # Configurações GitHub
│   ├── 📁 workflows/                 # GitHub Actions
│   │   ├── deploy.yml               # Deploy automático
│   │   └── tests.yml                # Testes automatizados
│   ├── ISSUE_TEMPLATE/              # Templates de issues
│   └── PULL_REQUEST_TEMPLATE.md     # Template de PR
├── 📁 docs/                         # Documentação
│   ├── 📁 assets/                   # Imagens da documentação
│   ├── 📁 api/                      # Documentação da API
│   └── 📁 guides/                   # Guias técnicos
├── 📁 src/                          # Código fonte
│   ├── 📁 css/                      # Estilos CSS
│   │   ├── main.css                 # Estilos principais
│   │   ├── gamification.css         # Estilos de gamificação
│   │   ├── responsive.css           # Media queries
│   │   └── accessibility.css        # Estilos de acessibilidade
│   ├── 📁 js/                       # Scripts JavaScript
│   │   ├── main.js                  # Script principal
│   │   ├── gamification.js          # Sistema de gamificação
│   │   ├── h5p-integration.js       # Integração H5P
│   │   ├── accessibility.js         # Recursos de acessibilidade
│   │   └── modules/                 # Módulos específicos
│   ├── 📁 h5p-content/              # Conteúdos H5P
│   │   ├── branching-scenario.h5p   # Cenário ramificado
│   │   ├── interactive-video.h5p    # Vídeo interativo
│   │   ├── course-presentation.h5p  # Apresentação
│   │   └── question-set.h5p         # Conjunto de questões
│   ├── 📁 assets/                   # Recursos multimídia
│   │   ├── 📁 images/               # Imagens
│   │   ├── 📁 videos/               # Vídeos
│   │   ├── 📁 audio/                # Áudios
│   │   └── 📁 icons/                # Ícones
│   └── 📁 data/                     # Dados estáticos
│       ├── territories.json         # Dados dos territórios
│       ├── badges.json              # Sistema de badges
│       └── glossary.json            # Glossário de termos
├── 📁 tests/                        # Testes automatizados
│   ├── unit/                        # Testes unitários
│   ├── integration/                 # Testes de integração
│   └── e2e/                         # Testes end-to-end
├── 📁 deployment/                   # Configurações de deploy
│   ├── github-pages.yml            # Config GitHub Pages
│   ├── netlify.toml                # Config Netlify
│   └── docker/                     # Containerização
├── 📄 index.html                   # Página principal
├── 📄 package.json                 # Dependências Node.js
├── 📄 package-lock.json            # Lock de versões
└── 📄 .gitignore                   # Arquivos ignorados
```

## 🚀 Configuração do Ambiente de Desenvolvimento

### Pré-requisitos

```bash
# Versões mínimas requeridas
Node.js >= 16.0.0
npm >= 8.0.0
Git >= 2.30.0
```

### Instalação e Setup

#### 1. Clone do Repositório

```bash
# Clone o repositório
git clone https://github.com/pamellabiotec/produto-educacional-cnmc.git
cd produto-educacional-cnmc

# Configure remote upstream (para contribuidores)
git remote add upstream https://github.com/pamellabiotec/produto-educacional-cnmc.git
```

#### 2. Instalação de Dependências

```bash
# Instale dependências de desenvolvimento
npm install

# Ou usando yarn
yarn install
```

#### 3. Configuração do Ambiente

```bash
# Copie o arquivo de configuração de exemplo
cp .env.example .env

# Edite as variáveis de ambiente
nano .env
```

#### 4. Servidor de Desenvolvimento

```bash
# Inicie o servidor de desenvolvimento
npm start

# Ou usando yarn
yarn start

# Servidor estará disponível em http://localhost:3000
```

### Scripts NPM Disponíveis

```json
{
  "scripts": {
    "start": "serve -s . -p 3000",
    "build": "npm run optimize && npm run minify",
    "test": "jest",
    "test:watch": "jest --watch",
    "lint": "eslint src/ --ext .js",
    "lint:fix": "eslint src/ --ext .js --fix",
    "optimize": "npm run optimize:images && npm run optimize:css",
    "optimize:images": "imagemin src/assets/images/* --out-dir=dist/assets/images",
    "optimize:css": "postcss src/css/*.css --dir dist/css",
    "minify": "terser src/js/*.js --compress --mangle --output dist/js/main.min.js",
    "deploy": "gh-pages -d .",
    "validate:h5p": "node scripts/validate-h5p.js"
  }
}
```

## 🌍 Configuração do GitHub Pages

### Método 1: Configuração Via Interface GitHub

1. **Acesse Settings do Repositório**
   ```
   Repositório → Settings → Pages
   ```

2. **Configure Source**
   ```
   Source: Deploy from a branch
   Branch: main
   Folder: / (root)
   ```

3. **Aguarde Deploy**
   - GitHub processará automaticamente
   - Site estará disponível em: `https://[username].github.io/[repo-name]/`

### Método 2: GitHub Actions (Recomendado)

Crie `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout
      uses: actions/checkout@v3
      
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
        
    - name: Install dependencies
      run: npm ci
      
    - name: Run tests
      run: npm test
      
    - name: Build project
      run: npm run build
      
    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      if: github.ref == 'refs/heads/main'
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```

### Método 3: Deploy Manual

```bash
# Build do projeto
npm run build

# Deploy usando gh-pages
npm run deploy

# Ou usando git subtree
git subtree push --prefix dist origin gh-pages
```

### Configuração de Domínio Customizado

1. **Configure DNS** (no seu provedor de domínio):
   ```
   Type: CNAME
   Host: www
   Value: [username].github.io
   
   Type: A
   Host: @
   Value: 185.199.108.153
   Value: 185.199.109.153
   Value: 185.199.110.153
   Value: 185.199.111.153
   ```

2. **Configure no GitHub**:
   ```
   Settings → Pages → Custom domain
   Digite: www.seudominio.com
   ✅ Enforce HTTPS
   ```

3. **Adicione arquivo CNAME**:
   ```bash
   echo "www.seudominio.com" > CNAME
   git add CNAME
   git commit -m "Add custom domain"
   git push
   ```

## 🧩 Integração com H5P

### Estrutura de Integração

```javascript
// h5p-integration.js
class H5PIntegration {
  constructor() {
    this.h5pInstances = new Map();
    this.scoreTracker = new ScoreTracker();
    this.eventBus = new EventBus();
  }

  async loadH5PContent(contentId, containerId) {
    try {
      // Carrega conteúdo H5P
      const content = await this.fetchH5PContent(contentId);
      
      // Inicializa instância H5P
      const instance = new H5P.Instance(content, containerId);
      
      // Registra listeners de eventos
      this.registerEventListeners(instance, contentId);
      
      // Armazena instância
      this.h5pInstances.set(contentId, instance);
      
      return instance;
    } catch (error) {
      console.error('Erro ao carregar H5P:', error);
      this.showErrorMessage(containerId, error);
    }
  }

  registerEventListeners(instance, contentId) {
    // Escuta eventos de pontuação
    instance.on('xAPI', (event) => {
      if (event.getVerb() === 'answered') {
        const score = event.getScore();
        this.scoreTracker.updateScore(contentId, score);
        this.eventBus.emit('score-updated', { contentId, score });
      }
    });

    // Escuta eventos de completude
    instance.on('xAPI', (event) => {
      if (event.getVerb() === 'completed') {
        this.eventBus.emit('content-completed', { contentId });
      }
    });
  }
}
```

### Configuração de Conteúdos H5P

```javascript
// Configuração de conteúdos H5P
const H5P_CONFIG = {
  'branching-scenario': {
    file: 'src/h5p-content/branching-scenario.h5p',
    container: '#branching-container',
    ions: 50,
    badge: 'explorador-quimico'
  },
  'interactive-video': {
    file: 'src/h5p-content/interactive-video.h5p',
    container: '#video-container',
    ions: 30,
    badge: null
  },
  'question-set': {
    file: 'src/h5p-content/question-set.h5p',
    container: '#quiz-container',
    ions: 25,
    badge: 'mestre-equilibrio'
  }
};
```

## 🎮 Sistema de Gamificação

### Arquitetura de Gamificação

```javascript
// gamification.js
class GamificationSystem {
  constructor() {
    this.player = new Player();
    this.badgeManager = new BadgeManager();
    this.leaderboard = new Leaderboard();
    this.progressTracker = new ProgressTracker();
  }

  // Sistema de pontuação (íons)
  addIons(amount, reason) {
    this.player.addIons(amount);
    this.showIonAnimation(amount);
    this.saveProgress();
    
    // Verifica conquistas
    this.checkAchievements();
    
    // Dispara evento
    this.dispatchEvent('ions-added', { amount, reason, total: this.player.ions });
  }

  // Sistema de badges
  unlockBadge(badgeId) {
    if (this.badgeManager.hasBadge(badgeId)) return;
    
    const badge = this.badgeManager.unlock(badgeId);
    this.showBadgeAnimation(badge);
    this.addIons(badge.ionReward, `Badge: ${badge.name}`);
    
    // Salva progresso
    this.saveProgress();
  }

  // Sistema de progressão
  completeTerritory(territoryId) {
    this.progressTracker.completeTerritory(territoryId);
    
    // Desbloqueia próximo território
    const nextTerritory = this.progressTracker.getNextTerritory();
    if (nextTerritory) {
      this.unlockTerritory(nextTerritory.id);
    }
    
    // Verifica badges relacionados
    this.checkTerritoryBadges(territoryId);
  }
}
```

### Persistência de Dados

```javascript
// storage.js
class StorageManager {
  constructor() {
    this.prefix = 'reino-elementos-';
    this.version = '1.0';
  }

  save(key, data) {
    const storageData = {
      version: this.version,
      timestamp: Date.now(),
      data: data
    };
    
    try {
      localStorage.setItem(
        this.prefix + key, 
        JSON.stringify(storageData)
      );
    } catch (error) {
      console.warn('Falha ao salvar:', error);
      // Fallback para sessionStorage
      sessionStorage.setItem(
        this.prefix + key, 
        JSON.stringify(storageData)
      );
    }
  }

  load(key, defaultValue = null) {
    try {
      const item = localStorage.getItem(this.prefix + key) || 
                   sessionStorage.getItem(this.prefix + key);
      
      if (!item) return defaultValue;
      
      const parsed = JSON.parse(item);
      
      // Verifica versão
      if (parsed.version !== this.version) {
        this.migrate(parsed, key);
      }
      
      return parsed.data;
    } catch (error) {
      console.warn('Falha ao carregar:', error);
      return defaultValue;
    }
  }
}
```

## ♿ Implementação de Acessibilidade

### Recursos de Acessibilidade

```javascript
// accessibility.js
class AccessibilityManager {
  constructor() {
    this.preferences = this.loadPreferences();
    this.init();
  }

  init() {
    // Controle de contraste
    this.setupContrastControl();
    
    // Controle de fonte
    this.setupFontControl();
    
    // Leitor de tela
    this.setupScreenReader();
    
    // Navegação por teclado
    this.setupKeyboardNavigation();
  }

  // Modo alto contraste
  setupContrastControl() {
    const contrastToggle = document.getElementById('contrast-toggle');
    contrastToggle?.addEventListener('click', () => {
      document.body.classList.toggle('high-contrast');
      this.savePreference('highContrast', 
        document.body.classList.contains('high-contrast'));
    });
  }

  // Controle de tamanho da fonte
  setupFontControl() {
    const fontSizeSlider = document.getElementById('font-size-slider');
    fontSizeSlider?.addEventListener('input', (e) => {
      const size = e.target.value;
      document.documentElement.style.setProperty('--font-scale', size);
      this.savePreference('fontSize', size);
    });
  }

  // Integração com leitores de tela
  announceToScreenReader(message) {
    const announcement = document.createElement('div');
    announcement.setAttribute('aria-live', 'polite');
    announcement.setAttribute('aria-atomic', 'true');
    announcement.className = 'sr-only';
    announcement.textContent = message;
    
    document.body.appendChild(announcement);
    
    setTimeout(() => {
      document.body.removeChild(announcement);
    }, 1000);
  }
}
```

### CSS para Acessibilidade

```css
/* accessibility.css */

/* Classes para leitores de tela */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Modo alto contraste */
.high-contrast {
  --primary-color: #000000;
  --background-color: #ffffff;
  --text-color: #000000;
  --accent-color: #0000ff;
}

.high-contrast * {
  color: var(--text-color) !important;
  background-color: var(--background-color) !important;
  border-color: var(--text-color) !important;
}

/* Controle de fonte */
:root {
  --font-scale: 1;
}

body {
  font-size: calc(16px * var(--font-scale));
}

/* Foco visível melhorado */
*:focus {
  outline: 3px solid var(--accent-color);
  outline-offset: 2px;
}

/* Animações respeitam preferências */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

## 🧪 Testes Automatizados

### Configuração do Jest

```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/tests/setup.js'],
  moduleNameMapping: {
    '\\.(css|less|scss|sass)$': 'identity-obj-proxy',
    '\\.(jpg|jpeg|png|gif|svg)$': '<rootDir>/tests/__mocks__/fileMock.js'
  },
  collectCoverageFrom: [
    'src/**/*.js',
    '!src/**/*.test.js',
    '!src/vendor/**'
  ],
  testMatch: [
    '<rootDir>/tests/**/*.test.js'
  ]
};
```

### Exemplos de Testes

```javascript
// tests/gamification.test.js
import { GamificationSystem } from '../src/js/gamification.js';

describe('GamificationSystem', () => {
  let gamification;

  beforeEach(() => {
    gamification = new GamificationSystem();
  });

  test('should add ions correctly', () => {
    gamification.addIons(10, 'test');
    expect(gamification.player.ions).toBe(10);
  });

  test('should unlock badge when criteria met', () => {
    gamification.player.ions = 100;
    gamification.checkAchievements();
    expect(gamification.badgeManager.hasBadge('first-100-ions')).toBe(true);
  });

  test('should save progress automatically', () => {
    const saveSpy = jest.spyOn(gamification, 'saveProgress');
    gamification.addIons(10, 'test');
    expect(saveSpy).toHaveBeenCalled();
  });
});
```

### Testes End-to-End com Playwright

```javascript
// tests/e2e/user-journey.spec.js
import { test, expect } from '@playwright/test';

test('complete user journey', async ({ page }) => {
  // Acessa página inicial
  await page.goto('/');
  
  // Verifica elementos principais
  await expect(page.locator('h1')).toContainText('Reino dos Elementos');
  
  // Inicia jornada
  await page.click('button:has-text("Iniciar Jornada")');
  
  // Completa primeiro território
  await page.click('[data-territory="terra-conceitos"]');
  await page.click('[data-quiz-option="1"]');
  await page.click('button:has-text("Confirmar")');
  
  // Verifica badge desbloqueado
  await expect(page.locator('[data-badge="explorador-quimico"]')).toBeVisible();
  
  // Verifica íons ganhos
  const ionCount = await page.locator('[data-ion-count]').textContent();
  expect(parseInt(ionCount)).toBeGreaterThan(0);
});
```

## 📊 Analytics e Monitoramento

### Configuração de Analytics

```javascript
// analytics.js
class AnalyticsManager {
  constructor() {
    this.events = [];
    this.sessionId = this.generateSessionId();
  }

  track(event, properties = {}) {
    const eventData = {
      event,
      properties: {
        ...properties,
        sessionId: this.sessionId,
        timestamp: Date.now(),
        userAgent: navigator.userAgent,
        url: window.location.href
      }
    };

    this.events.push(eventData);
    
    // Envia para Google Analytics (se configurado)
    if (window.gtag) {
      gtag('event', event, properties);
    }
    
    // Salva localmente para relatórios
    this.saveToLocal(eventData);
  }

  // Eventos específicos do produto educacional
  trackTerritoryEntered(territoryId) {
    this.track('territory_entered', { territoryId });
  }

  trackQuizCompleted(quizId, score, timeSpent) {
    this.track('quiz_completed', { quizId, score, timeSpent });
  }

  trackBadgeUnlocked(badgeId) {
    this.track('badge_unlocked', { badgeId });
  }

  trackH5PInteraction(contentId, interaction) {
    this.track('h5p_interaction', { contentId, interaction });
  }
}
```

## 🔄 CI/CD e Automação

### GitHub Actions Completo

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]
  release:
    types: [ published ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Run linting
        run: npm run lint
        
      - name: Run tests
        run: npm test -- --coverage
        
      - name: Upload coverage reports
        uses: codecov/codecov-action@v3
        
      - name: Validate H5P content
        run: npm run validate:h5p

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build project
        run: npm run build
        
      - name: Upload build artifacts
        uses: actions/upload-artifact@v3
        with:
          name: build-files
          path: dist/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v3
      
      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: build-files
          path: dist/
          
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
          cname: seudominio.com  # Se usando domínio customizado
```

## 🔧 Troubleshooting

### Problemas Comuns e Soluções

#### H5P não carrega

```javascript
// Diagnóstico
console.log('H5P Core carregado:', typeof H5P !== 'undefined');
console.log('H5P Instance disponível:', typeof H5P.Instance !== 'undefined');

// Solução
if (typeof H5P === 'undefined') {
  // Carrega H5P Core
  const script = document.createElement('script');
  script.src = 'https://h5p.org/sites/all/modules/h5p/library/js/h5p.js';
  script.onload = () => initH5P();
  document.head.appendChild(script);
}
```

#### Progresso não salva

```javascript
// Verifica suporte a localStorage
function checkStorageSupport() {
  try {
    const test = 'storage-test';
    localStorage.setItem(test, test);
    localStorage.removeItem(test);
    return true;
  } catch(e) {
    console.warn('localStorage não suportado, usando sessionStorage');
    return false;
  }
}

// Implementa fallback
const storage = checkStorageSupport() ? localStorage : sessionStorage;
```

#### Performance lenta

```javascript
// Implementa lazy loading
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      loadH5PContent(entry.target.dataset.contentId);
      observer.unobserve(entry.target);
    }
  });
});

document.querySelectorAll('[data-lazy-h5p]').forEach(el => {
  observer.observe(el);
});
```

## 📚 Recursos Adicionais

### Documentação de APIs

- **[H5P Documentation](https://h5p.org/documentation/developers)**
- **[GitHub Pages Docs](https://docs.github.com/en/pages)**
- **[Web Accessibility Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)**

### Ferramentas Úteis

```bash
# Validators
npm install -g html-validator
npm install -g eslint
npm install -g lighthouse

# Optimização
npm install -g imagemin-cli
npm install -g terser
npm install -g postcss-cli

# Deploy
npm install -g gh-pages
npm install -g surge
```

### Configuração do VS Code

```json
// .vscode/settings.json
{
  "eslint.enable": true,
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.associations": {
    "*.h5p": "zip"
  },
  "emmet.includeLanguages": {
    "javascript": "html"
  }
}
```

## 🤝 Contribuindo

### Fluxo de Desenvolvimento

1. **Fork** do repositório principal
2. **Clone** do seu fork
3. **Branch** para nova feature: `git checkout -b feature/nova-funcionalidade`
4. **Commit** das mudanças: `git commit -m 'Adiciona nova funcionalidade'`
5. **Push** para branch: `git push origin feature/nova-funcionalidade`
6. **Pull Request** para branch principal

### Padrões de Código

```javascript
// Exemplo de função bem documentada
/**
 * Calcula constante de equilíbrio Kc
 * @param {Array<number>} products - Concentrações dos produtos
 * @param {Array<number>} reactants - Concentrações dos reagentes
 * @param {Array<number>} coefficients - Coeficientes estequiométricos
 * @returns {number} Valor de Kc
 * @throws {Error} Se arrays têm tamanhos diferentes
 */
function calculateKc(products, reactants, coefficients) {
  // Implementação...
}
```

### Testes Obrigatórios

- **Unit tests** para funções puras
- **Integration tests** para módulos
- **E2E tests** para fluxos críticos
- **Accessibility tests** para WCAG compliance

---

**Autor:** Prof.ª Pâmella A. Balcaçar (@pamellabiotec)  
**Licença:** CC BY-NC-SA 4.0  
**Versão:** 1.0 - Janeiro 2025