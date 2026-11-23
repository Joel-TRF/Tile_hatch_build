# 🧱 Gerador de Padrões de Azulejo (Tile Hatch Builder)

Ferramenta web que cria **ficheiros de padrão `.pat`** para azulejos e pavimentos, prontos a importar no **Revit** ou no **AutoCAD**.

👉 **Abrir a ferramenta:** https://joel-trf.github.io/Tile_hatch_build/
📄 **Manual de utilizador (PDF):** [`docs/Manual_Gerador_Padroes_Azulejo.pdf`](docs/Manual_Gerador_Padroes_Azulejo.pdf)

---

## O que faz

- Gera padrões nos assentamentos **Junta direita, Junta desencontrada, Alternado, Espinha, Entrançado e Moinho**
- Permite rodar a 0°, 45° ou 90°
- Exporta para **Revit (padrão de modelo)**, **Revit (padrão de desenho)** ou **AutoCAD**, em milímetros ou polegadas
- A pré-visualização é desenhada a partir do próprio texto do `.pat`, por isso o que se vê é exatamente o que está no ficheiro
- Funciona em português e inglês (botão PT/EN)

## Utilização rápida

1. Abrir a ferramenta no browser (não é preciso instalar nada).
2. Escolher o assentamento e indicar o comprimento, a largura e a junta.
3. Escolher a rotação, o formato e as unidades.
4. Confirmar o padrão na pré-visualização e clicar em **Exportar .pat**.
5. **Revit:** Gerir › Definições adicionais › Padrões de preenchimento › Novo › Personalizado › Importar
   **AutoCAD:** copiar o `.pat` para uma pasta do caminho de suporte. O nome do ficheiro tem de ser igual ao nome do padrão.

O manual em PDF tem a explicação completa de cada opção.

---

## Contribuir

> **Todas as alterações, correções e sugestões são feitas através deste repositório.**
> Por favor não enviem versões do ficheiro por email ou por chat.

### Reportar erros ou sugerir ideias
Abrir uma [Issue](../../issues) com:
- o assentamento, as dimensões, a junta, a rotação e o formato usados;
- o que esperavam e o que aconteceu;
- se possível, uma captura de ecrã ou o `.pat` gerado.

### Alterar o código
1. Criar um *branch* a partir do `main` (por exemplo `feature/chevron` ou `fix/espinha-45`).
2. Fazer as alterações e testar no browser.
3. Abrir um *Pull Request* com uma descrição do que mudou e de como foi testado.
4. Esperar a revisão. **Não se fazem alterações diretamente no `main`.**

### Como testar
- Abrir o `index.html` diretamente no browser. Não há nada para compilar nem instalar.
- Verificar o padrão na pré-visualização em todos os assentamentos afetados.
- Quando a alteração mexe nas contas, importar o `.pat` no Revit e/ou no AutoCAD e confirmar que o desenho bate certo.

---

## Como o código está organizado

É um único ficheiro **`index.html`**, em JavaScript simples, sem bibliotecas e sem nada para compilar.

| Função | O que faz |
|---|---|
| `layoutCell()` | Define cada assentamento como uma célula de repetição: dois vetores (`A`, `B`) e a lista de peças `[x, y, largura, altura]`. **Para criar um assentamento novo, começa-se aqui.** |
| `edgeLine()` | Faz as contas principais: transforma cada aresta de uma peça numa linha do `.pat` (ângulo, origem, deslocamento, afastamento, traço/espaço). Já suporta arestas em qualquer direção. |
| `findCycle()` / `ratApprox()` / `egcd()` | Ajudas de aritmética para encontrar quando um padrão volta a repetir. |
| `build()` | Monta o cabeçalho (Revit/AutoCAD) e as linhas do ficheiro. |
| `draw()` | Volta a ler o texto do `.pat` e desenha-o no ecrã, o que também serve de verificação. |
| `TPL` | Guarda as versões completas da página em **PT** e **EN**. |

### Formato de uma linha `.pat`
```
ângulo, x, y, deslocamento, afastamento, traço, espaço
```
Cada peça da célula de repetição dá origem a quatro linhas (aresta inferior, superior, esquerda e direita).

### ⚠️ Cuidados
- **Textos da interface:** qualquer alteração tem de ser feita nas versões PT **e** EN dentro de `TPL`, e também no HTML principal.
- **Novo assentamento:** acrescentar um `case` em `layoutCell()`, um botão (com ícone SVG) nas três versões do HTML, e uma linha na tabela "Como o padrão é construído".

---

## Próximos passos

Quem quiser pegar num destes pontos, é só abrir uma Issue a dizer que está a trabalhar nele.

- [ ] Manual de utilização integrado na própria ferramenta
- [ ] Novos assentamentos: opus romano/Versailles, chevron, hexagonal
- [ ] Rotação livre, em vez de só 0/45/90
- [ ] Importar um `.pat` existente para o ver ou editar
- [ ] Conjunto de padrões de teste verificados no Revit e no AutoCAD

---

## Estrutura do repositório

```
Tile_hatch_build/
├── index.html                              ← a ferramenta
├── README.md
└── docs/
    └── Manual_Gerador_Padroes_Azulejo.pdf  ← manual de utilizador
```
