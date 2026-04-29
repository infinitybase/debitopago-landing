# DebitoPago — Marketing v2

Pacote de marketing institucional do **[DP] DebitoPago**, primeira plataforma da
família Serenitech a ir ao ar publicamente. HTML/CSS estático, multi-página,
responsivo, em português brasileiro, aderente à gramática da família Serenitech v2.2.

> **Status:** draft pronto para revisão jurídica. Não publicar antes de preencher
> os placeholders (ver `placeholders.md`) e revisar termos + privacidade com
> advogado(a) especializado(a).

---

## 📁 Estrutura

```
debitopago-marketing-v2/
├── README.md                     ← este arquivo
├── placeholders.md               ← variáveis a preencher antes da publicação
├── index.html                    ← landing page
├── pages/
│   ├── termos.html               ← Termos e Condições de Uso
│   ├── privacidade.html          ← Política de Privacidade (LGPD)
│   ├── faq.html                  ← Perguntas frequentes (versão completa)
│   └── contato.html              ← Canais oficiais
└── assets/
    ├── css/
    │   ├── tokens.css            ← design tokens (Serenitech v2.2 + [DP])
    │   └── styles.css            ← estilos da página (reset, layout, componentes)
    └── brand/
        ├── fullmark-debitopago-color.svg
        ├── fullmark-debitopago-dark.svg
        ├── fullmark-debitopago-grafite.svg
        ├── fullmark-debitopago-white.svg
        ├── ticker-dp-{color,dark,grafite,white}.svg
        ├── fullmark-debitopago-color-800w.png
        ├── favicon-{16,32}.png
        └── apple-touch-icon.png
```

---

## 🎨 Sistema visual

Aplicação direta da gramática **Serenitech v2.2** (aprovado em 2026-04-24):

- **Ticker:** `[DP]` em colchetes muted `#6B6B6B`, letras Verde Quitado `#1F7A3D`
- **Fullmark:** prefixo "debito" em muted, sufixo "pago" em verde
- **Tipografia:** Inter (sans) + JetBrains Mono (mono); carregadas via Google Fonts
- **Paleta:**
  - Primária `#1F7A3D` (CTA, ticker, destaque)
  - Acento dark `#4FB871`, hover `#155828`, banner soft `#E8F3EC`
  - Tons de contexto: grafite `#262626`, muted `#6B6B6B`, papel `#F5F4F0`
- **Layout:** container 1120px (regular) / 720px (narrow) / 640px (prose)
- **Border-radius:** sm 4 / md 8 / lg 12 / xl 20 / full 999

Todos os tokens estão consolidados em `assets/css/tokens.css` e devem ser
**reutilizados** quando o pacote do CarroPago e do MonFinance for criado — as
variáveis `--dp-verde*` viram `--cp-azul*` e `--mf-vermelho*`, mas a estrutura
permanece.

---

## 🗺️ Mapa das páginas

### `index.html` — landing page

Hero institucional + mockup de WhatsApp com o fluxo real (placa → consulta → Pix),
três pilares (consulta em 30s, Pix, fee transparente), seção "como funciona" em 3
passos, comparação contra super-app, bloco B2B `#frotas`, FAQ resumido (6 itens) e
CTA band. Todos os CTAs apontam para `wa.me/5514959760004`.

### `pages/termos.html`

14 seções de Termos e Condições de Uso. Identifica formalmente a **LANET
Tecnologia Ltda. (CNPJ 54.855.244/0001-85)** como operadora. Estrutura:

1. Objeto · 2. Definições · 3. Cadastro · 4. Funcionamento · 5. Obrigações do
Usuário · 6. Fee · 7. Propriedade Intelectual · 8. Privacidade (cross-link) ·
9. Responsabilidades e limites (cap em 12 meses de fees) · 10. Canais oficiais
(anti-fraude) · 11. Suspensão · 12. Alterações · 13. Foro · 14. Disposições finais.

### `pages/privacidade.html`

14 seções de Política de Privacidade alinhada à LGPD (Lei 13.709/2018):

1. Controladora · 2. Dados coletados · 3. Finalidades · 4. Bases legais ·
5. Compartilhamento · 6. Transferência internacional · 7. Retenção e eliminação ·
8. Direitos do titular (art. 18) · 9. Segurança · 10. Cookies · 11. Crianças e
adolescentes · 12. DPO · 13. Atualizações · 14. Lei aplicável.

### `pages/faq.html`

22 perguntas organizadas em 7 seções (Como funciona, Pagamento e Pix, Fee e
custos, Tipos de débito, Segurança e privacidade, Frotas e parceiros, Suporte),
com sumário no topo e accordion `<details>` semântico.

### `pages/contato.html`

Cinco cards de canal (suporte ao usuário, B2B, privacidade, imprensa, jurídico)
+ bloco anti-fraude e endereço formal para notificações.

---

## ♿ Acessibilidade

- `lang="pt-BR"` em todas as páginas
- Skip link `Pular para o conteúdo`
- Hierarquia de heading consistente (1 H1 por página)
- `aria-label` em navegação e marca
- `aria-current="page"` em itens de menu ativos
- Foco visível mantido (sem `outline: none`)
- Contraste verde primário sobre branco aprovado em WCAG AA para texto grande/CTA

> **Pendente:** auditoria WCAG 2.1 completa (incluindo simulação com leitor de
> tela e teste de contraste em estados de hover) deve ser executada antes do
> lançamento — usar `/design:accessibility-review`.

---

## 🚀 Antes de publicar

1. **Preencher placeholders** — ver `placeholders.md` (lista completa de
   variáveis com exemplos de preenchimento e sugestão de padrão de e-mail).
2. **Revisar termos + privacidade** com advogado(a) especializado(a) (CDC + LGPD).
3. **Configurar caixas de e-mail** (`contato@`, `privacidade@`, `parcerias@`,
   `imprensa@`, `juridico@`) com auto-resposta e SLA documentado.
4. **Validar selo Meta Verified Business** no número +55 14 95976-0004.
5. **Atualizar OG image** para 1200x630 com texto institucional adequado.
6. **Configurar analytics** (e respectivo banner de cookies, se for o caso —
   atualizar §10 da Política de Privacidade).
7. **Publicar lista de subprocessadores** em `/legal/subprocessadores` e atualizar
   o link na §6 da Política de Privacidade.
8. **Auditoria final** — `/design:accessibility-review` + `/marketing-skills:seo-audit`
   + `/marketing-skills:page-cro` na home.

---

## 🔄 Próximos pacotes

A mesma estrutura será replicada para os outros produtos da família operacional:

- `carropago-marketing-v2/` — paleta `#1E3A5F` Azul Escrow, ticker `[CP]`
- `monfinance-marketing-v2/` — paleta `#CE1126` Vermelho Mônaco, ticker `[MF]`

A camada `tokens.css` é **trocável**; `styles.css` permanece praticamente
idêntico (substituindo as referências a `--dp-verde*`).

---

## 📜 Copyright

```
© 2026 LANET Tecnologia Ltda. · CNPJ 54.855.244/0001-85
DebitoPago é uma marca do sistema Serenitech.
```

Bandeira corporativa: LANET é tier-corporativo `[LN]` na família Serenitech;
DebitoPago é tier-operacional `[DP]`.

---

**Build:** 2026-04-25 · **Owner:** Rodrigo N. Füchter · **Brand system:** Serenitech v2.2
