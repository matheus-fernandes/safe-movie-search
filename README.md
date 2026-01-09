# Safe Movie Search 🎬

Catálogo de filmes reativo e moderno, desenvolvido com **Angular 21 (Zoneless)**. A interface utiliza a estética de "Lente Protetora" para entregar um ambiente de busca seguro, performático e visualmente imersivo.

## 🏗️ Arquitetura do Sistema

O projeto segue os princípios de **Clean Architecture** e **Reactive State Management**, otimizado para a nova era do Angular sem Zone.js.

### 1. Organização de Camadas (Screaming Architecture)

* **Core:** Singleton services, interceptors de API, modelos de domínio e a configuração global de "Safe Content".
* **Features:** Módulos funcionais (Search, Filters, Catalog) que contêm "Smart Components".
* **Shared:** Componentes de UI puramente visuais (Dumb Components), diretivas de interação e pipes.
* **Data-Access:** Repositórios que gerenciam a comunicação com o TMDB e o estado reativo via Signals.

### 2. Gestão de Estado e Reatividade

* **Angular Signals:** Uso integral de `signal`, `computed` e `effect` para controle de estado granular.
* **Zoneless Change Detection:** Alta performance com `ChangeDetectionStrategy.OnPush` e detecção de mudanças baseada em sinais.
* **Reactive Data Flow:** Integração de RxJS (para debounce de busca) com Signals (para renderização de UI) através de `toSignal`.

---

## 🎨 Identidade Visual & Design System

O projeto adota uma estética cyberpunk-minimalista em dark mode.

* **Base — Midnight Blue (#0F172A):** Profundidade e redução de fadiga ocular.
* **Ação — Electric Indigo (#6366F1):** Modernidade e Vibe Coding.
* **Segurança — Emerald Green (#10B981):** Feedback de conteúdo "Safe".
* **Atenção — Soft Amber (#F59E0B):** Alertas de classificação etária.

### Princípios de UX

* **Lente Protetora:** Interface filtrada para precisão máxima.
* **Feedback háptico visual:** Indicadores de progresso durante o Long Press.
* **Microinterações:** Glassmorphism e transições suaves de escala via Tailwind/DaisyUI.

---

## 🛠️ Especificação Técnica & Componentes

### Componentes Propostos

* `SearchLensComponent`: Input reativo com lógica de `debounceTime`.
* `FilterPanelComponent`: Gerenciador de estado dos toggles de streaming e idade.
* `MovieGridComponent`: Grid otimizado com a nova sintaxe de controle de fluxo `@for`.
* `SafetySliderComponent`: Custom form control para notas 0-10.

### Serviços Principais

* `MovieRepository`: Abstração da API TMDB usando `rxResource` para fetching declarativo.
* `SafetyCoordinator`: Centraliza a lógica de exclusão de `keywords` críticas e filtros de idade.
* `StreamingProviderService`: Mapeia e injeta as cores dinâmicas de cada plataforma de streaming.

### 🚀 Funcionalidades (RFs)

1. **RF01 (Busca):** Case-insensitive em tempo real via Signals.
2. **RF02 (Streaming):** Toggles dinâmicos; seleção mutuamente exclusiva ou aditiva.
3. **RF03 (Rating):** Slider gradual com gradientes dinâmicos (Red -> Green).
4. **RF04 (Idade):** Lógica inclusiva inteligente. Clique simples (alvo) vs Long Press (cascata).
5. **RF05 (Safe Content):** Filtro negativo via `without_keywords` (IDs: 9715, 18035) aplicado na origem.

---

## 🖱️ Lógica de Interação: Long Press

A arquitetura delega a responsabilidade do Long Press para uma **Diretiva Estrutural/Atributo**:

1. **Timer:** Inicia 600ms no `mousedown`/`touchstart`.
2. **Visual:** Ativa um sinal de `progress` que o componente de UI consome para animar o preenchimento de um círculo.
3. **Ação:** Dispara a seleção cascata (ex: seleciona 14, 12, 10 e L simultaneamente).

---

## 📦 Desenvolvimento

### Requisitos

* Node.js (versão compatível com Angular 21)
* TMDB API Key

### Instalação

```bash
npm install
npm start

```

### Configuração de Ambiente

1. Renomeie `.env.example` para `.env`.
2. Informe `TMDB_API_KEY` com sua credencial.