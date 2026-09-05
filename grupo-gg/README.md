# gidogreen — Landing Page (Grupo GG Grátis)

Landing page estática, **autossuficiente e independente**. Todo o código e os assets
usados estão dentro desta pasta — não depende de WordPress, de servidor externo nem
de CDNs (fontes, ícones e imagens são servidos localmente).

## Como rodar / publicar

É 100% estática. Sirva a pasta por qualquer servidor HTTP:

```bash
# Python
python3 -m http.server 8099
# abra http://localhost:8099/index.html

# ou Node
npx serve .
```

Para publicar, suba a pasta inteira em qualquer hospedagem estática
(Netlify, Vercel, Cloudflare Pages, GitHub Pages, S3, Apache/Nginx).
Arquivo de entrada: `index.html`.

## Estrutura

```
gidogreen/
├─ index.html          → página (código + estilos/scripts inline)
├─ README.md
└─ assets/
   ├─ css/             → todas as folhas de estilo
   ├─ js/              → todos os scripts (inclui os bundles do Elementor)
   ├─ img/             → imagens (hero, grafismos, backgrounds) em .webp
   └─ fonts/           → fontes locais (Bebas Neue, Barlow, Inter, Roboto/Slab,
                          Font Awesome, ícones do Happy Addons)
```

Só os assets efetivamente referenciados foram incluídos (sem arquivos órfãos,
backups ou fontes não usadas).

## Rastreamento e integrações

| O quê | Onde (`index.html`) | Valor |
|---|---|---|
| Google Tag Manager | `<script>` no `<head>` + `<noscript>` no `<body>` | Container `GTM-KXVVC6SC` |
| Planilha (leads) | `const SHEET_URL` | Google Apps Script (App da Web `/exec`) |
| Destino do CTA — Telegram | `DESTINOS.telegram` | `https://t.me/+17HqwvJOyYw1MWEx` |
| Destino do CTA — WhatsApp | `DESTINOS.whatsapp` | `https://chat.whatsapp.com/HZ1W4DkJYlLHU5BcM0YD1e` |
| Destino pós-formulário (popup desativado) | `const REDIRECT` | `https://w.app/gidogreen` |

- **Meta Pixel (Facebook) removido.** O rastreio fica por conta do GTM — configure
  seus pixels/eventos dentro do container `GTM-KXVVC6SC`.
- **Leads → Google Sheets.** Ao enviar o formulário, os dados são gravados na planilha
  via Apps Script (`SHEET_URL`) e um evento `lead` é empurrado para o `dataLayer` do GTM.
  O envio é "fire-and-forget": se a planilha falhar, o usuário **nunca** fica preso.
  Formato enviado: `{ lead:{email,telefone}, origem, event, timestamp, utm_params }`.

### Fluxo do CTA (ativo)
A LP tem **dois botões** no hero, para o lead escolher o canal:

1. **Telegram** (botão de cima, azul) → `DESTINOS.telegram`
2. **WhatsApp** (botão de baixo, verde) → `DESTINOS.whatsapp`

Os dois passam pelo mesmo rotacionador/rastreamento (`window.__pmRedirectBase`) que a
página já usava e empurram um evento `clique_grupo` para o `dataLayer` com o campo
`canal` (`telegram` ou `whatsapp`), permitindo separar os cliques por canal no GTM.

### Fluxo do formulário (popup DESATIVADO, preservado no `<template>`)
1. Clique no CTA → abre o modal com o formulário.
2. Preenche e-mail + telefone (validação local) → grava na planilha → tela "Vaga Garantida!".
3. Redireciona automaticamente para o grupo do WhatsApp (`REDIRECT`).

## Notas

- A única chamada externa em runtime é o GTM (o próprio da gidogreen). Todo o resto é
  servido localmente.
- Toda referência ao domínio/marca originais foi removida ou renomeada para gidogreen.
- O publicPath do Elementor foi ajustado (`elementorFrontendConfig.urls.assets = "assets/"`)
  para que os bundles carreguem de `assets/js/`.
- Há um `console.error` inofensivo (`form-field-telefone` nulo) herdado do código
  original; não afeta o funcionamento (a máscara de telefone ativa usa `phoneInput`).
