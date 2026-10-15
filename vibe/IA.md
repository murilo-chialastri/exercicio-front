# IA.md — Documentação de Mudanças

## Projeto: Landing Page One Piece
**Pasta:** `vibe/`  
**Data de criação:** 2024  
**Stack:** HTML + CSS + Tailwind CSS (CDN) + JavaScript vanilla

---

## Direção de Design

**Estética:** Cinematic Adventure Editorial  
**Tom:** Épico, imersivo, editorial premium  
**Inspiração:** Layout do site Long Table (referências visuais) + animações de scroll do site Jovi Mobile OriginOS

### DFII (Design Feasibility & Impact Index)
| Dimensão                | Nota |
|-------------------------|------|
| Aesthetic Impact        | 4/5  |
| Context Fit             | 5/5  |
| Implementation Feasibility | 5/5  |
| Performance Safety      | 4/5  |
| Consistency Risk        | 2/5  |
| **DFII Total**          | **16 (Excelente)** |

---

## Paleta de Cores

| Cor           | Hex       | Uso                                |
|---------------|-----------|-------------------------------------|
| Vermelho      | `#C8472C` | Acentos, gradientes quentes         |
| Marrom Escuro | `#412A1E` | Texto escuro, footer, profundidade  |
| Amarelo       | `#F8DE3C` | Fundo seção Akuma no Mi             |
| Branco        | `#FEFEFE` | Texto principal, backgrounds claros |
| Azul Celeste  | `#58ACF4` | Fundo seção Tripulação              |
| Azul          | `#105EDD` | Acentos, gradientes frios           |
| Azul Marinho  | `#0B3075` | Background principal, texto escuro  |

---

## Tipografia

| Função   | Fonte              | Peso          | Uso                        |
|----------|--------------------|---------------|-----------------------------|
| Display  | Pirata One         | 400 (único)   | Títulos, headings grandes   |
| Body     | Poppins            | 100–900       | Corpo de texto, navegação, ênfases em itálico |

---

## Estrutura das 4 Seções

### Seção 1 — Hero
- Full viewport height
- Background com gradiente (placeholder para imagem)
- Overlay escuro com gradiente
- Título grande serif com palavra em itálico
- Subtítulo + CTA "Explorar →"
- Indicador de scroll no centro inferior
- **Animação:** Fade-in escalonado ao carregar a página

### Seção 2 — Galeria
- Duas imagens lado a lado (grid 2 colunas)
- Full bleed, sem gap
- Placeholders com gradientes temáticos
- **Animação:** Scale-in no scroll com stagger

### Seção 3 — Akuma no Mi (Frutas do Diabo)
- Layout split com sobreposição diagonal
- Esquerda: fundo amarelo (#F8DE3C) com texto navy
- Direita: imagem com clip-path diagonal (`polygon(20% 0, 100% 0, 100% 100%, 5% 100%)`)
- A imagem sobrepõe a área amarela criando corte diagonal
- **Animação:** Texto slide-from-left, imagem slide-from-right

### Seção 4 — A Tripulação
- Layout split invertido com sobreposição diagonal
- Esquerda: imagem com clip-path diagonal (`polygon(0 0, 95% 0, 80% 100%, 0 100%)`)
- Direita: fundo azul celeste (#58ACF4) com texto navy
- **Animação:** Imagem slide-from-left, texto slide-from-right

### Footer
- Fundo marrom escuro (#412A1E)
- Citação temática em itálico
- Copyright

---

## Animações Implementadas

| Tipo           | Classe CSS       | Efeito                        | Duração |
|----------------|------------------|-------------------------------|---------|
| Fade Up        | `.reveal`        | Opacity 0→1, Y 60px→0        | 1s      |
| Slide Left     | `.reveal-left`   | Opacity 0→1, X -80px→0       | 1s      |
| Slide Right    | `.reveal-right`  | Opacity 0→1, X +80px→0       | 1s      |
| Scale In       | `.reveal-scale`  | Opacity 0→1, Scale 0.92→1    | 1.2s    |
| Hero Entrance  | `.hero-enter`    | Keyframe fade+slide escalonado| 1.2s    |
| Parallax       | JS scroll        | Hero BG translateY + scale    | RAF     |
| **Image Reveal** | `.image-reveal`  | clip-path interpolado por scroll (lerp) | Contínuo |

**Easing:** `cubic-bezier(0.16, 1, 0.3, 1)` — curva "ease-out-expo" para suavidade natural  
**Trigger:** IntersectionObserver com threshold 15% e rootMargin -60px  
**Acessibilidade:** `prefers-reduced-motion: reduce` desativa todas as animações

---

## Como Adicionar Imagens

Cada placeholder tem um comentário `🔄` indicando onde substituir:

### Hero (Seção 1)
Trocar o `style` da div `.hero-bg`:
```html
style="background-image: url('sua-imagem.jpg'); background-size: cover; background-position: center;"
```

### Galeria (Seção 2) e Splits (Seções 3 e 4)
Substituir o conteúdo da div `.img-placeholder` por:
```html
<img src="sua-imagem.jpg" alt="Descrição" class="w-full h-full object-cover">
```

---

## Responsividade

- **< 768px:** Grid empilha em coluna única
- **< 768px:** Clip-paths diagonais são removidos
- **< 768px:** Padding e espaçamentos reduzidos
- Títulos usam `clamp()` para escalonamento fluido

---

## Changelog

### v1.0.0 — Criação inicial
- Criação do `index.html` com 4 seções + footer
- Implementação da paleta de cores completa
- Animações de scroll com IntersectionObserver
- Parallax no hero com requestAnimationFrame
- Cortes diagonais com CSS clip-path
- Navegação fixa com mix-blend-mode: difference
- Suporte a prefers-reduced-motion
- Layout responsivo para mobile

### v1.1.0 — Fontes + Animação de Reveal por Scroll
**Fontes:**
- Trocada fonte display de Cormorant Garamond → **Pirata One** (temática pirata)
- Trocada fonte body de Source Sans 3 → **Poppins** (moderna, legível)
- Palavras em `<em>` nos headings usam Poppins italic 300 para contraste com Pirata One
- Import via `@import url()` dentro de `<style>` ao invés de `<link>`

**Animação de Reveal (Seções 3 e 4):**
- Seções Akuma no Mi e Tripulação agora iniciam como cor sólida inteira
- Fundo amarelo/azul cobre 100% da seção (antes era 50%)
- Imagem começa oculta via `clip-path` colapsado
- Conforme o scroll, `clip-path` interpola via JS (`lerp`) revelando a imagem com corte diagonal
- Seção 3: imagem emerge da direita para a esquerda
- Seção 4: imagem emerge da esquerda para a direita
- Efeito controlado por `requestAnimationFrame` + scroll position
- Transition de 50ms no clip-path para suavidade extra
