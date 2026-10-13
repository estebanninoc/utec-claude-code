# Estilo Claude — extracción real (ORDEN CC3, 2026-10-13)

Metodología: Playwright (Chromium 1228, headful bajo Xvfb, build local en
`/opt/dispatch/projects/taller/node_modules/playwright`) contra 3 fuentes públicas reales,
en viewport **1440×1000** (desktop) y **390×844** (mobile):

- `https://www.anthropic.com/` (home)
- `https://claude.com/product/overview` (marketing de producto Claude — llamado en el pedido
  "anthropic.com/claude", redirige ahí)
- `https://claude.ai/login` (la app real, pre-login — `claude.ai/` redirige acá sin sesión)

`getComputedStyle()` sobre `body`, `h1`, `h2`, `h3`, `p`, botones y una muestra de encabezados/
botones reales de cada página. Capturas en este mismo folder (`referencia_*_1440.png` /
`referencia_*_390.png`). El logo oficial se descargó directo de `https://claude.ai/favicon.svg`
(200 OK, `image/svg+xml`) → `claude-spark-oficial.svg`, un único `<path>` relleno `#D97757`.

**Límite honesto:** el "thinking spinner" animado de la app (la chispa girando mientras Claude
responde) vive DENTRO de la app autenticada — sin login no hay forma de inspeccionar su CSS/JS
real. La animación de la landing (ver sección Animación) es una **recreación** fiel al
comportamiento públicamente conocido (giro continuo + pulso de opacidad), no una extracción byte
a byte. Se documenta así para no afirmar algo que no se verificó.

## Colores (medidos, rgb → hex)

| Token | Valor medido | Hex | Dónde se vio |
|---|---|---|---|
| Fondo marca (marketing) | `rgb(250,249,245)` | `#FAF9F5` | `body` en anthropic.com y claude.com |
| Fondo app (claude.ai) | `rgb(252,252,251)` | `#FCFCFB` | `body` en claude.ai/login |
| Texto principal | `rgb(20,20,19)` | `#141413` | `body`/`h1` en anthropic.com, claude.com |
| Texto principal (app) | `rgb(11,11,11)` | `#0B0B0B` | `body` en claude.ai/login |
| Texto secundario/mutado | `rgb(135,134,127)` / `rgb(176,174,165)` | `#87867F` / `#B0AEA5` | párrafos secundarios, nav |
| Borde sutil | `rgb(94,93,89)` | `#5E5D59` | borde 1px de botones secundarios |
| **Acento (naranja/terracota oficial)** | de `claude-spark-oficial.svg` | **`#D97757`** | fill del logo oficial |
| Foco/hover acento (variante) | `rgb(196,104,73)` | `#C46849` | anillo de foco "Skip to main content" |

La landing CC2 ya usaba `#FAF9F5` / `#141413` / `#D97757` por coincidencia de buen criterio
previo — se mantienen porque **son exactamente los valores reales medidos**, no hay que tocarlos.

## Tipografía

| Nivel | Fuente medida | Tamaño desktop | Tamaño mobile | Peso | Line-height | Letter-spacing |
|---|---|---|---|---|---|---|
| H1 (hero) | Anthropic Serif / anthropicSerif | 60–72px | 35–40px | 400–500 | ~1.08–1.1 | normal a -1.2px en tamaños >75px |
| H2 | Anthropic Serif | 52–80px | 35–48px | 400–500 | ~1.1–1.15 | -0.7 a -1.2px en tamaños grandes |
| H3 | Anthropic Sans | 19–32px | 20–24px | 500–600 | ~1.2–1.3 | normal |
| Cuerpo / `p` | Anthropic Serif o Anthropic Sans (varía por página) | 15–24px | 15–24px | 400 | ~1.4–1.6 | normal |
| Botón | Anthropic Sans | 14–17px | 14–17px | 400–500 | ~1.15–1.2 | normal |
| Small / nav | Anthropic Sans | 12–16px | 12–15px | 400 | ~1.3–1.5 | normal |

Patrón real confirmado por captura (`referencia_claude-ai_1440.png`): el wordmark usa el logo +
"Claude" **en serif**, junto a un H1 enorme también en serif ("Question what's next"), con el
subtítulo y los botones en sans. Ese mismo patrón (ícono + wordmark serif, H1 serif, cuerpo/UI
sans) es el que se aplicó al hero de esta landing.

### Reemplazo de fuentes (licencia comercial → gemelo libre)

| Original (Anthropic, licencia comercial — NO se descarga) | Reemplazo self-hosted usado acá | Nota |
|---|---|---|
| **Anthropic Serif** (= Tiempos/Copernicus, serif editorial) | **Newsreader var** (Google Fonts, variable, ya self-hosted desde ORDEN CC2 en `fonts/newsreader-var.woff2`) | Ya estaba en el repo — mismo criterio que pide el pedido, cero peso extra. Tracking ajustado a -0.01/-0.02em en tamaños grandes para imitar el apretado del original. |
| **Anthropic Sans** (= Styrene, grotesca geométrica) | **Inter var** (Google Fonts, variable, ya self-hosted en `fonts/inter-var.woff2`) | Styrene es más condensada que Inter; se compensa con `letter-spacing` ligeramente negativo en títulos sans (H3) y pesos 500/600 en vez de 400 donde el original usa semibold, para no perder presencia. |

No se descargó ni se subió ninguna fuente de Anthropic. Ambas fuentes de reemplazo ya vivían en
el repo desde CC2 (hallazgo técnico de esa orden: Google Fonts `css2` dedupea los 4 pesos en un
solo archivo woff2) — CC3 no agregó peso de fuentes.

## Radios, bordes, sombras

- **Radio de botón:** 8px consistente (medido en CTA "Try Claude", "Accept all cookies",
  "Contact sales", pills de nav). La landing usaba 14px antes de CC3 → se bajó a 8px.
  Las chips/pills circulares (`border-radius:999px`) se mantienen — Anthropic también usa
  pills completos en badges/inputs de login (ver `referencia_claude-ai_1440.png`).
- **Borde:** 1px sólido, color sutil (`rgb(94,93,89)` en botones secundarios; `rgba(20,20,19,.1)`
  en superficies sobre crema — este último ya lo tenía la landing, se mantiene por ser visualmente
  equivalente sobre el mismo fondo).
- **Sombra:** prácticamente plana — `box-shadow: none` en el 95% de los elementos medidos
  (botones, nav, cards). La landing tenía sombras difusas de 24px de blur y el CTA tenía un glow
  de color; se reemplazó por sombras casi nulas (`0 1px 2px rgba(20,20,19,.05)` máximo) y el CTA
  quedó sin glow, apoyado solo en el borde/contraste — así se ve el sitio real.

## Botones

Medido: fondo sólido oscuro o acento, texto blanco/crema, radio 8px, sin sombra, peso 500–600,
sin mayúsculas. Hover: oscurece el fondo, no agrega glow. Aplicado igual en `.btn`.

## Logo / chispa

`claude-spark-oficial.svg` — path único, `viewBox 0 0 248 248`, relleno `#D97757`. Reemplaza el
asterisco casero de 8 barras (`.chispa .rayo`) en las 3 ubicaciones pedidas: hero, header del
bloque "pensando", y como ícono de carga (favicon — es literalmente lo que el navegador muestra
mientras la pestaña carga).

## Animación

- **Entrada al cargar** (solo ícono del hero, una vez): fade + scale desde 0.6 + rotación desde
  -45° hasta su posición final, ~0.7s con easing "overshoot" (`cubic-bezier(.34,1.56,.64,1)`).
- **"Pensando" / girar-pulsar** (loop continuo): rotación lineal infinita + pulso de opacidad.
  Se usan dos velocidades: una lenta (8s/2.6s) para el hero en reposo y el cierre, y una más
  rápida (3.2s/1.4s) en el header del bloque "pensando" para transmitir actividad — más rápida
  porque ahí el copy dice explícitamente "Claude Code piensa, decide y construye".
- `prefers-reduced-motion: reduce` desactiva todo (ya existía ese guardrail en la landing, se
  extendió a las nuevas clases).

## Verificación de peso y QA (resultados reales, medidos 2026-10-13)

- Capturas lado a lado en este folder: `referencia_claude-ai_{390,1440}.png` /
  `referencia_claude-com_{390,1440}.png` (fuente real) vs. `landing_{390,1440}.png` (esta landing).
- **Peso total servido: 141 822 bytes (~138.5 KB)** — HTML + 2 fuentes variable (Inter, Newsreader)
  + SVG inline del logo oficial, medido con Playwright contra `python3 -m http.server` real (no
  `file://`), sumando el `content-length` de cada respuesta del documento. Muy por debajo del tope
  de 400 KB. (Los PNG de este folder `shots-cc3/` son material de verificación, no se sirven en
  producción y no cuentan para ese presupuesto — igual que en órdenes previas de landings.)
- **0 errores de consola, 0 requests fallidos** en ambos viewports (1440 y 390), verificado con
  los listeners `page.on('console')`/`page.on('requestfailed')` de Playwright más un chequeo de
  `<img>` rotas (`naturalWidth === 0`) — ninguna.
- Las 3 chispas (hero, header del bloque "pensando", cierre) renderizan con el `<path>` oficial
  completo (`<use href="#chispa-oficial">` con bounding box correcto: 56px / 20px / 40px).
- Nota de QA: las capturas `landing_*.png` se tomaron con `prefers-reduced-motion: reduce`
  emulado (soportado nativamente por la landing) para que las animaciones de aparición en scroll
  queden resueltas de forma determinística en una sola captura — es el mismo mecanismo de
  accesibilidad que ya traía la landing, no algo agregado solo para la foto.
