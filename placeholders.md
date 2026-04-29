# Placeholders — DebitoPago Marketing v2

Lista centralizada de variáveis a preencher antes da publicação. Os placeholders
estão marcados nas páginas como `<span class="placeholder">[NOME]</span>` (fundo
amarelo com borda tracejada) — fazer um find&replace global após preenchimento.

> **Atenção:** este pacote é um *draft* pronto para revisão jurídica. Antes de subir
> para produção, todos os campos abaixo precisam ser preenchidos e os documentos
> legais (`pages/termos.html`, `pages/privacidade.html`) revisados por advogado(a)
> especializado(a) em Direito do Consumidor + LGPD.

---

## 1. Datas e versionamento

| Variável | Onde aparece | Exemplo de preenchimento |
|---|---|---|
| `[DATA_ULTIMA_ATUALIZACAO]` | termos.html, privacidade.html | `25 de abril de 2026` |

---

## 2. Identidade da operadora (LANET)

> Já preenchidos no template: razão social, CNPJ, marca DebitoPago, número WhatsApp
> +55 14 95976-0004, domínio debitopago.com.br.

| Variável | Onde aparece | Observação |
|---|---|---|
| `[ENDERECO_SEDE_LANET]` | termos, privacidade, contato | endereço completo da sede para citação e correspondência |

---

## 3. Canais de e-mail

LGPD recomenda fortemente um canal dedicado a privacidade. Sugestão de padrão de
endereçamento (a confirmar com TI):

| Variável | Sugestão | Onde aparece |
|---|---|---|
| `[EMAIL_CONTATO]` | `contato@debitopago.com.br` | termos, contato |
| `[EMAIL_PRIVACIDADE]` | `privacidade@debitopago.com.br` | privacidade, contato |
| `[EMAIL_B2B]` | `parcerias@debitopago.com.br` | contato#b2b |
| `[EMAIL_IMPRENSA]` | `imprensa@debitopago.com.br` | contato#imprensa |
| `[EMAIL_JURIDICO]` | `juridico@lanet.com.br` (caixa institucional) | contato#juridico |

---

## 4. Encarregado pelo tratamento de dados (DPO)

Por exigência prática da LGPD (art. 41), é necessário identificar formalmente o
Encarregado, mesmo que seja um membro da equipe acumulando função.

| Variável | Onde aparece |
|---|---|
| `[DPO_NOME]` | privacidade, contato |
| `[DPO_EMAIL]` | privacidade, contato |

---

## 5. Política de Fee

| Variável | Onde aparece | Observação |
|---|---|---|
| `[URL_POLITICA_FEE]` | termos §6 | URL pública detalhando o fee por canal/UF/tipo de débito (pode ser um anexo PDF ou página dedicada do site) |

---

## 6. Subprocessadores e infraestrutura

A LGPD exige transparência sobre transferência internacional (art. 33). Manter
uma lista pública atualizada dos provedores que tratam dados pessoais por conta
da LANET.

| Variável | Onde aparece | Conteúdo esperado |
|---|---|---|
| `[URL_LISTA_SUBPROCESSADORES]` | privacidade §6 | URL para `/legal/subprocessadores` listando: cloud (provedor + região), WhatsApp BSP, antifraude, e-mail transacional, observabilidade |

---

## 7. Foro

| Variável | Onde aparece | Observação |
|---|---|---|
| `[COMARCA_FORO]` | termos §13 | comarca da sede da LANET; preservada a competência especial do CDC quando aplicável |

---

## 8. Checklist de revisão jurídica antes da publicação

- [ ] Endereço completo da LANET preenchido em todos os pontos
- [ ] DPO identificado e endereço de privacidade dedicado funcionando
- [ ] Política de Fee publicada na URL referenciada
- [ ] Lista de subprocessadores publicada e atualizada
- [ ] Texto de Termos revisado por advogado(a) — atenção especial às limitações de responsabilidade (CDC), suspensão sumária e renovação tácita
- [ ] Texto de Privacidade revisado por advogado(a) com viés LGPD — atenção a controlador conjunto vs autônomo no fluxo B2B, períodos de retenção, transferência internacional
- [ ] FAQ revisado: nenhuma promessa que extrapole o que está nos Termos (especialmente sobre prazos de baixa nos órgãos)
- [ ] Página de Contato com canais funcionando — auto-resposta configurada em cada caixa
- [ ] Banner de cookies (se houver analytics em prod) configurado e referenciado em `pages/privacidade.html` §10
- [ ] Tag `meta robots` revisada conforme estágio do lançamento (atualmente `index,follow`)
- [ ] Selo Meta Verified Business confirmado no número +55 14 95976-0004
- [ ] OG image `assets/brand/fullmark-debitopago-color-800w.png` atualizada para 1200x630 com texto institucional

---

## 9. Fora de escopo deste draft (a fazer em sprint posterior)

- Schema.org structured data (Organization, FAQPage, BreadcrumbList)
- Banner LGPD de cookies (caso analytics seja adotado)
- Páginas internas extras (Para frotas dedicada, Para concessionárias, Para fintechs)
- Variantes em inglês para parceiros internacionais
- A/B test de hero copy (versão "30 segundos" vs "no chat" vs "sem app")
