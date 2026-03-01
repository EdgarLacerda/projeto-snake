# 🐍 Snake — Game Design Document

## Visão Geral

| Campo | Detalhes |
|---|---|
| **Título** | Snake |
| **Gênero** | Arcade / Casual |
| **Plataforma** | PC (Desktop) |
| **Engine/Tecnologia** | Python + Pygame |
| **Resolução** | 1200 × 800 px |
| **Jogadores** | 1 (Single-player) |

**Conceito:** Clássico jogo Snake onde o jogador controla uma cobra que cresce a cada alimento consumido. O objetivo é sobreviver o máximo possível sem colidir com as paredes ou com o próprio corpo.

---

## Mecânicas de Jogo

### Movimentação
A cobra se move em quatro direções usando as teclas de seta do teclado. O movimento é discreto — a cobra avança um bloco por tick de jogo.

- **Seta para cima** → move para cima
- **Seta para baixo** → move para baixo
- **Seta para esquerda** → move para esquerda
- **Seta para direita** → move para direita

**Restrição de inversão:** A cobra não pode se mover na direção oposta ao seu movimento atual (ex.: se está indo para a direita, não pode ir direto para a esquerda). A exceção ocorre quando `tamanho_cobra == 1`, ou seja, quando a cobra tem apenas um segmento.

### Crescimento
A cada alimento consumido, `tamanho_cobra` aumenta em 1. O corpo da cobra é representado por uma lista de pixels (`pixels[]`), e o comprimento máximo dessa lista é controlado por `tamanho_cobra`.

### Pontuação
A pontuação exibida na tela é `tamanho_cobra - 1`, ou seja, começa em 0 e sobe a cada alimento coletado.

---

## Condições de Fim de Jogo

O jogo termina (`fim_jogo = True`) nas seguintes situações:

1. **Colisão com a parede** — a posição `x` ou `y` sai dos limites da tela (< 0 ou ≥ largura/altura).
2. **Colisão com o próprio corpo** — a cabeça da cobra ocupa a mesma posição de qualquer outro segmento do corpo (`pixel == [x, y]`).

Não há tela de Game Over nem reinício automático — o loop simplesmente encerra.

---

## Elementos Visuais

| Elemento | Cor | Forma |
|---|---|---|
| Fundo | Preto `(0, 0, 0)` | Tela inteira |
| Cobra | Branco `(255, 255, 255)` | Quadrado 20×20 px |
| Comida | Verde `(0, 255, 0)` | Quadrado 20×20 px |
| Pontuação | Vermelho `(255, 0, 0)` | Texto (Helvetica 30pt) |

A pontuação é renderizada no canto superior esquerdo da tela (`[1, 1]`).

---

## Sistema de Comida

A comida é gerada em posições aleatórias alinhadas à grade de 20×20 px, garantindo que sempre apareça dentro dos limites da tela. Uma nova comida é gerada imediatamente após a cobra consumi-la. Não há limite de comidas simultâneas — apenas uma por vez.

**Fórmula de geração:**
```
comida_x = round(randrange(0, largura - tamanho_quadrado) / 20.0) * 20.0
comida_y = round(randrange(0, altura - tamanho_quadrado) / 20.0) * 20.0
```

---

## Parâmetros de Configuração

| Parâmetro | Valor | Descrição |
|---|---|---|
| `largura` | 1200 px | Largura da janela |
| `altura` | 800 px | Altura da janela |
| `tamanho_quadrado` | 20 px | Tamanho de cada bloco (cobra e comida) |
| `velocidade_jogo` | 10 FPS | Tick rate do jogo |
| `tamanho_cobra` | 1 (inicial) | Comprimento inicial da cobra |

---

## Arquitetura do Código

```
snake.py
├── desenhar_comida()     → Renderiza o alimento na tela
├── gerar_comida()        → Gera posição aleatória alinhada à grade
├── desenhar_cobra()      → Renderiza todos os segmentos da cobra
├── desenhar_pontuacao()  → Exibe a pontuação no canto superior esquerdo
├── selecionar_velocidade() → Processa input do teclado e define direção
└── rodar_jogo()          → Loop principal do jogo
```

### Fluxo do Loop Principal (`rodar_jogo`)

```
Inicialização
    ↓
Limpar tela
    ↓
Processar eventos (quit / teclas)
    ↓
Desenhar comida
    ↓
Verificar colisão com paredes
    ↓
Atualizar posição (x, y)
    ↓
Atualizar lista de pixels (corpo da cobra)
    ↓
Verificar colisão com o próprio corpo
    ↓
Desenhar cobra + pontuação
    ↓
Atualizar display
    ↓
Verificar coleta de comida → gerar nova comida se coletada
    ↓
Clock tick (10 FPS)
    ↓
Repetir
```

---

## Requisitos para Execução

- Python 3.x
- Pygame (`pip install pygame`)

```bash
python snake.py
```

---

## Vídeo de Gameplay

[Clique aqui para assistir](https://drive.google.com/file/d/1qhb-ifSVNf7mSahSCzUfjWctWOZ3Q12U/view?usp=sharing)
