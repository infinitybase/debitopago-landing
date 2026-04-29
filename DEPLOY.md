# DEPLOY — DebitoPago LP

Guia operacional para subir `debitopago-marketing-v2/` em produção em
**debitopago.com.br** (e www). O site é 100% estático: HTML + CSS + assets,
sem build step, sem JS de produto, sem backend. Compatível com qualquer host
estático moderno.

---

## ✅ Antes de deploy — checklist crítico

Não publique sem completar. Detalhamento dos itens em `placeholders.md`.

- [ ] **Placeholders preenchidos** — find&replace global de `[ENDERECO_SEDE_LANET]`, `[DATA_ULTIMA_ATUALIZACAO]`, `[EMAIL_*]`, `[DPO_NOME]`, `[DPO_EMAIL]`, `[COMARCA_FORO]`, `[URL_POLITICA_FEE]`, `[URL_LISTA_SUBPROCESSADORES]`. Após preenchimento, **remover a classe `placeholder`** dos `<span>` correspondentes (ou apagar o `<span>`) para que o fundo amarelo deixe de aparecer.
- [ ] **Notas internas removidas** — pés de página marcados "Nota interna (não publicar)" em `pages/termos.html`, `pages/privacidade.html`, `pages/contato.html` precisam ser deletados antes do deploy público.
- [ ] **Termos e Privacidade revisados por advogado(a)** especializado(a) em CDC + LGPD.
- [ ] **OG image** atualizada para 1200×630 com texto institucional adequado (atual `assets/brand/fullmark-debitopago-color-800w.png` é insuficiente para preview social rico).
- [ ] **Selo Meta Verified Business** confirmado no número WhatsApp +55 14 95976-0004.
- [ ] **Caixas de e-mail** configuradas com auto-resposta e SLA documentado.
- [ ] **Lista de subprocessadores** publicada na URL referenciada na §6 da Privacidade.
- [ ] **Política de Fee** publicada na URL referenciada na §6 dos Termos.
- [ ] **robots.txt e sitemap.xml** revisados (incluídos no pacote — ajustar domínio se diferente).

---

## 🌐 DNS

```
debitopago.com.br      A     <IP_DA_HOSPEDAGEM>     (ou ALIAS/ANAME para o host)
www.debitopago.com.br  CNAME debitopago.com.br
```

Política recomendada: **apex canônico** (`https://debitopago.com.br`) com redirect 301 de `www` para apex. Garante consistência de SEO e evita duplicação de conteúdo.

---

## 🔐 HTTPS

Certificado válido obrigatório. Em qualquer host moderno (Cloudflare Pages, Netlify, Vercel, Cloudflare DNS+VPS) o TLS é automático via Let's Encrypt ou cert do próprio CDN. Renovação deve ser automática.

Configurações exigidas:
- TLS 1.2+ (ideal: TLS 1.3)
- HSTS habilitado (ver headers abaixo)
- HTTP → HTTPS redirect 301

Validar pós-deploy:
```bash
curl -sI https://debitopago.com.br | head -5
# Deve retornar HTTP/2 200 (ou 1.1 200) com Strict-Transport-Security presente
```

---

## 📦 Opções de hospedagem (do mais simples ao mais flexível)

### Opção A — Cloudflare Pages (recomendada para começar)

1. Push do conteúdo do pacote para um repositório git (sem o ZIP, sem `qa-screenshots/`).
2. Em Cloudflare → Pages → Connect to Git → seleciona o repo.
3. Build command: *(vazio)* — site estático.
4. Build output directory: `/` (raiz do repo).
5. Domínio custom: `debitopago.com.br` + `www.debitopago.com.br`.
6. O arquivo `_headers` (incluso no pacote) é aplicado automaticamente.

Tempo de subida: ~2 minutos. Custo: $0 no plano free.

### Opção B — Netlify

Mesma lógica do Cloudflare Pages. O arquivo `_headers` também é honrado nativamente. Drag & drop da pasta inteira no Netlify também funciona para deploy manual.

### Opção C — Vercel

Suporta sites estáticos. Headers de segurança devem ser configurados via `vercel.json` (não Vercel não lê `_headers`). Snippet de exemplo abaixo.

### Opção D — VPS com nginx

Configuração mínima (ver `nginx.conf.example` se precisar — ou aplicar o snippet abaixo).

```nginx
server {
    listen 443 ssl http2;
    server_name debitopago.com.br;

    root /var/www/debitopago;
    index index.html;

    ssl_certificate     /etc/letsencrypt/live/debitopago.com.br/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/debitopago.com.br/privkey.pem;
    ssl_protocols       TLSv1.2 TLSv1.3;

    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-Frame-Options "DENY" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Permissions-Policy "camera=(), microphone=(), geolocation=()" always;
    add_header Content-Security-Policy "default-src 'self'; img-src 'self' data:; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src https://fonts.gstatic.com; script-src 'self'; connect-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self';" always;

    location ~* \.(css|js|svg|png|jpg|jpeg|gif|ico|woff2?)$ {
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    location / {
        try_files $uri $uri/ $uri.html =404;
    }

    error_page 404 /404.html;
}

server {
    listen 80;
    server_name debitopago.com.br www.debitopago.com.br;
    return 301 https://debitopago.com.br$request_uri;
}

server {
    listen 443 ssl http2;
    server_name www.debitopago.com.br;
    ssl_certificate     /etc/letsencrypt/live/debitopago.com.br/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/debitopago.com.br/privkey.pem;
    return 301 https://debitopago.com.br$request_uri;
}
```

---

## 🛡️ Cabeçalhos de segurança

O arquivo `_headers` no pacote já está configurado para Cloudflare Pages e Netlify. Conteúdo:

```
/*
  Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
  X-Content-Type-Options: nosniff
  X-Frame-Options: DENY
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: camera=(), microphone=(), geolocation=()
  Content-Security-Policy: default-src 'self'; img-src 'self' data:; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src https://fonts.gstatic.com; script-src 'self'; connect-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self';

/assets/*
  Cache-Control: public, max-age=2592000, immutable
```

Para Vercel, equivalente em `vercel.json`:

```json
{
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "Strict-Transport-Security", "value": "max-age=63072000; includeSubDomains; preload" },
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" },
        { "key": "Permissions-Policy", "value": "camera=(), microphone=(), geolocation=()" },
        { "key": "Content-Security-Policy", "value": "default-src 'self'; img-src 'self' data:; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src https://fonts.gstatic.com; script-src 'self'; connect-src 'self'; frame-ancestors 'none'; base-uri 'self'; form-action 'self';" }
      ]
    }
  ]
}
```

> **Atenção:** se incluir analytics no futuro (Plausible, GA4, etc.), atualize a CSP com os domínios apropriados em `script-src` e `connect-src`. E atualize a §10 da `pages/privacidade.html`.

---

## 🤖 robots.txt + sitemap.xml

Ambos estão incluídos no pacote, configurados para `https://debitopago.com.br`. Se o domínio mudar, basta substituir a string. Submissão ao Google Search Console e Bing Webmaster Tools recomendada após o primeiro deploy estável.

---

## 🚦 Smoke test pós-deploy

Após o site no ar:

1. **HTTPS funcionando** em apex e www, www → apex 301:
   ```bash
   curl -sI https://www.debitopago.com.br | grep -i location
   ```
2. **Todas as 5 páginas carregam:**
   - `https://debitopago.com.br/`
   - `https://debitopago.com.br/pages/termos.html`
   - `https://debitopago.com.br/pages/privacidade.html`
   - `https://debitopago.com.br/pages/faq.html`
   - `https://debitopago.com.br/pages/contato.html`
3. **Ícones e CSS:** abrir DevTools → Network → recarregar com cache desabilitado e verificar 200 em todas as fontes Google, SVGs e PNGs.
4. **Inter carregando:** o texto não deve aparecer em fonte serif (default do browser). Se aparecer, há bloqueio de Google Fonts ou CSP errada.
5. **WhatsApp link:** clicar no botão "Abrir conversa no WhatsApp" e confirmar redirecionamento para o número correto.
6. **Mobile:** abrir em iOS e Android e validar que o mockup do WhatsApp (hero) e a comparação não estouram horizontalmente.
7. **Search Console:** validar `sitemap.xml` e indexação progressiva.

### Ferramentas externas

- [securityheaders.com](https://securityheaders.com) — meta: nota A ou A+
- [ssllabs.com/ssltest](https://www.ssllabs.com/ssltest) — meta: nota A ou A+
- [pagespeed.web.dev](https://pagespeed.web.dev) — meta: ≥90 mobile, ≥95 desktop
- [validator.w3.org](https://validator.w3.org) — sem erros HTML
- [search.google.com/test/rich-results](https://search.google.com/test/rich-results) — após adicionar schema (sprint posterior)

---

## ↩️ Rollback

Site é estático e versionado. Para reverter a versão anterior:
- **Cloudflare Pages / Netlify / Vercel:** painel → Deploys → "Restore" / "Rollback" para o build anterior. ~30 segundos.
- **VPS:** `cd /var/www && rm -rf debitopago && unzip debitopago-marketing-v2-<versão-anterior>.zip -d debitopago && systemctl reload nginx`.

---

## 📅 Versionamento

Cada deploy de produção deve ser tagueado e arquivado:

```
debitopago-marketing-v2-2026-04-25.zip   ← este pacote (draft inicial)
debitopago-marketing-v2-2026-MM-DD.zip   ← próximas revisões
```

Convenção:
- `v2.x` enquanto a base de código atual estiver em uso.
- `v3` apenas em redesigns ou refactor estrutural.
- Mudanças jurídicas em `pages/termos.html` ou `pages/privacidade.html` exigem atualização do `[DATA_ULTIMA_ATUALIZACAO]` e republicação imediata.

---

## 📞 Contatos de operação

Em caso de incidente em produção:
- **Owner técnico:** Rodrigo N. Füchter
- **Operadora:** LANET Tecnologia Ltda. (CNPJ 54.855.244/0001-85)
- **WhatsApp oficial (canal do produto):** +55 14 95976-0004
- **Domínio:** debitopago.com.br

---

**Build:** 2026-04-25 · **Pacote:** debitopago-marketing-v2 · **Brand system:** Serenitech v2.2
