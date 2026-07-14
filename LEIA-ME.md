# LPs GIDOGREEN — Odds Altas & Grupo VIP

As 2 landing pages da **GIDOGREEN** (quiz + captação de leads): páginas
**limpas e self-contained** (sem WordPress/Elementor), com tema verde, fotos da
GidoGreen e um único bloco de configuração pra você editar.

## Estrutura

```
LPs-GidoGreen/
├── odds-altas/
│   ├── index.html          ← a LP (arquivo único, self-contained)
│   └── assets/
│       ├── hero.jpg        ← GidoGreen (boné verde + Brasil, estádio)
│       └── gido-cutout.png ← recorte PNG (Flamengo comemorando) p/ usar se quiser
├── grupo-vip/
│   ├── index.html
│   └── assets/
│       ├── hero.jpg        ← GidoGreen (Flamengo, Maracanã, comemorando)
│       └── gido-cutout.png ← recorte PNG (Brasil amarela)
├── _original/              ← fotos da GidoGreen em alta + arquivos de trabalho (NÃO versionado)
└── LEIA-ME.md
```

Cada `index.html` é **independente**: é só subir a pasta (`index.html` + `assets/`) em
qualquer hospedagem. Usa Tailwind, Lucide e Google Fonts via CDN (precisa de internet).

## Como pré-visualizar no seu Mac

Abra o Terminal na pasta `LPs-GidoGreen` e rode:

```
python3 -m http.server 8000
```

Depois abra no navegador: `http://localhost:8000/odds-altas/` e `http://localhost:8000/grupo-vip/`.
(Só dar duplo-clique no `index.html` também funciona na maioria dos navegadores.)

## ⚙️ Onde configurar (bloco CONFIG no topo do `<script>`)

Abra o `index.html` e no comecinho do último `<script>` tem o bloco **CONFIG** — edite SÓ ali:

| Campo | O que é | Padrão atual |
|---|---|---|
| `LINK_TELEGRAM` / `LINK_COMUNIDADE` / `LINK_X1` | seus 3 links da GidoGreen | já preenchidos |
| `REDIRECT_URL` | pra onde o lead vai **depois** de preencher o form | odds → Comunidade WhatsApp · vip → Telegram |
| `LEAD_ENDPOINT` | URL do seu webhook/CRM que **recebe** os dados do lead | **`''` (vazio)** |
| `FB_PIXEL_ID` | ID do Pixel da Meta | **`''` (vazio)** |
| `ORIGEM` | identificador enviado no payload | `gidogreen_odds_altas` / `gidogreen_grupo_vip` |

## ⚠️ O que FALTA pra ir pro ar “de verdade”

1. **Webhook de leads (`LEAD_ENDPOINT`)** — está **vazio**. Enquanto estiver assim, o
   lead é só salvo no navegador (localStorage) e o redirect acontece normal — ou seja,
   **você não recebe o e-mail/telefone em lugar nenhum**. Coloque aí a URL do seu
   CRM/automação (Make, n8n, Zapier, webhook próprio, etc.).
2. **Pixel da Meta (`FB_PIXEL_ID`)** — está **vazio**. Coloque o **seu** pra rastrear
   PageView + Lead. Vazio = sem rastreio.
3. **Confirmar o `REDIRECT_URL`** de cada página (ver tabela acima).

## Trocar copy / fotos

- **Copy:** está tudo em texto dentro do `<script>` (`renderIntro`, `renderStep`, `renderResult`,
  `openLeadModal`…). É só procurar o texto e trocar.
- **Foto hero:** substitua `assets/hero.jpg` (mesmo nome) ou aponte `BG_MOBILE`/`BG_DESKTOP` no JS.
- Tem mais fotos da GidoGreen em `_original/gidogreen-fotos/` (as `DSC_*.JPG` são as de alta
  resolução; `1.png`…`5.png` são recortes de camisas de vários times).

## Observações

- As duas páginas têm **1 pergunta** no quiz — dá pra adicionar mais perguntas no array
  `STEPS`/`QUESTIONS` se quiser.
- Textos de resultado (“+300K membros”, “98% das vagas”, bilhetes de exemplo, etc.) são
  **exemplos** — ajuste pros números reais da GidoGreen.
- Conteúdo é +18 / apostas: o aviso de jogo responsável foi mantido no rodapé.
