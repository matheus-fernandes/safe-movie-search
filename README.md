# Safe Movie Search 🎬

Catálogo de filmes reativo e moderno, desenvolvido com Angular 21. Foco em filtros granulares e experiência de busca fluida.

## Sumário
- Introdução
- Funcionalidades Principais
- Especificação de Requisitos
- Integração Técnica e API
- Arquitetura e Reatividade
- Lógica de Clique Longo (UX)
- Desenvolvimento

## Funcionalidades Principais
O Safe Movie Search é construído seguindo rigorosos padrões de UX e reatividade:

- **Busca Reativa:** Filtragem instantânea e case-insensitive conforme a digitação.
- **Filtro de Streaming:** Seleção dinâmica de fornecedores (Netflix, Prime Video, Disney+, Max).
- **Filtro de Nota e Lançamento:** Controlo via range slider e inputs numéricos.
- **Classificação Etária Inteligente (Multiseleção):**
	- Seleção individual simples.
	- Seleção em massa: ao usar Ctrl+Clique ou Long Press (600ms), a faixa etária clicada e todas as inferiores são selecionadas automaticamente.
- **Filtro de Exclusão (Safe Content):** Oculta filmes com temas sensíveis (Violência, Drogas, Nudez, etc.) via IDs de palavras‑chave.
- **Design Adaptativo:** Interface otimizada para Desktop e Mobile com tema Dark Mode (Slate‑950).

## Especificação de Requisitos

### 1. Requisitos Funcionais (RF)
1. **RF01 (Busca):** Filtragem em tempo real no campo de pesquisa.
2. **RF02 (Providers):** Comportamento de rádio/toggle para streamings; clicar no ativo remove o filtro.
3. **RF03 (Nota):** Slider de 0 a 10 com step de 0.5.
4. **RF04 (Classificação):** Opções L, 10, 12, 14, 16, 18. Lógica inclusiva (selecionar abaixo de) via Ctrl+Click ou clique longo.
5. **RF05 (Exclusão por Keywords):** Filtro negativo que utiliza IDs específicos do TMDB para omitir resultados indesejados.

### 2. Integração Técnica e API
- **Provider de Dados:** TMDB API (v3).
- **Endpoints Utilizados:**
	- `GET /discover/movie`: Listagem principal. Usa `without_keywords` para exclusão e `with_watch_providers` para streamings.
	- `GET /search/movie`: Busca textual.
	- `GET /watch/providers/movie`: Mapeamento de logótipos de streaming.
	- `GET /movie/{movie_id}/keywords`: Palavras‑chave de um filme para validação local.
- **Gestão de Palavras‑Chave (Keywords):**
	- Lista predefinida de IDs críticos para "Safe Content" (ex.: Violência: `9715`, Conteúdo Sexual: `18035`).
	- Exclusão aplicada diretamente na query da API via `without_keywords`, garantindo resposta já higienizada do servidor.

## Arquitetura e Reatividade
- **State Management:** Angular Signals (`signal`, `computed`, `effect`).
- **Service Layer:**
	- `MovieService`: Centraliza chamadas HTTP e estado global de filtros.
	- `KeywordService`: Gere mapeamento de IDs de palavras‑chave e categorias de restrição.
- **Performance:**
	- `ChangeDetectionStrategy.OnPush`.
	- Lógica de filtragem via `computed()` para sincronizar UI e estado.
	- `debounceTime` aplicado na busca para otimizar consumo da API.

## Lógica de Clique Longo (UX)
- `mousedown`/`touchstart` → inicia timer de 600ms.
- Ao completar o timer, define `isLongPress = true` e executa seleção em massa.
- Em `click`, se `isLongPress` for verdadeiro, apenas reseta a flag (evita desseleção acidental).

## Desenvolvimento

### Servidor de Desenvolvimento
Para iniciar o servidor local, execute:

```bash
ng serve
```

Depois, acesse:

```
http://localhost:4200/
```

### Configuração de API Key
Renomeie o ficheiro `.env.example` para `.env` e insira o seu `TMDB_API_KEY`.

Para mais informações, visite a documentação oficial do Angular.