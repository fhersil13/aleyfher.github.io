# Sistema de diseño

Paleta "carta de amor": vino, hueso-durazno y dorado apagado, en vez del
rosa chicle genérico de San Valentín. Todo el color vive en variables
CSS en `:root` (`style.css`), nada de hex sueltos en el resto del
archivo salvo casos puntuales (rgba de sombras, degradados).

## Paleta

| Variable | Valor | Uso |
|---|---|---|
| `--bg` | `#fbf3f1` | fondo general de la página |
| `--card` | `#fffaf9` | superficies elevadas (no muy usado hoy) |
| `--accent` | `#7c2340` | vino — títulos, botón, listón, indicador activo |
| `--accent-bright` | `#a6335a` | vino claro — degradados, hover |
| `--muted` | `#6b4552` | texto secundario |
| `--ink` | `#2b141c` | "tinta": fondo del carrusel y del panel del ramo |
| `--gold` | `#a9793a` | eyebrow, firma, indicadores inactivos |
| `--gold-soft` | `#d9ad6b` | acentos suaves |
| `--blush` | `#f3d9d6` | reservado para superficies suaves |

Antes de este ajuste el fondo era `#fff0f6` y el acento `#ff6b9a` (rosa
chicle) sobre cajas negras planas (`#000`/`#111`). Se cambió porque
competía con los tonos de piel de las fotos y envejecía peor que un
vino profundo.

## Tipografía

Dos familias, cargadas desde Google Fonts (`<link>` en el `<head>` de
`index.html`, sin build step ni self-hosting):

- **Fraunces** (`ital,wght@0,600;1,500`) — serif con carácter, solo para
  gestos: el título del hero, el texto del listón, la firma del footer,
  el número grande del contador de días. Siempre en su variante itálica
  500 o normal 600, nunca para texto corrido.
- **Karla** (`wght@400;700`) — sans para todo lo demás: cuerpo,
  etiquetas, el eyebrow en mayúsculas.

Si Google Fonts no carga, el *fallback* es `Georgia, serif` para
Fraunces y la pila `system-ui` original para Karla — el sitio no se
rompe, solo pierde personalidad tipográfica.

## Forma de corazón

`.heart-shape` en `style.css` dibuja un corazón sin SVG ni emoji: un
cuadrado (`background: currentColor`) rotado 45°, con dos círculos del
mismo tamaño pegados por `::before`/`::after` en el borde superior e
izquierdo (antes de rotar). Se reutiliza en tres lugares:

1. El botón del ramo (`.heart-trigger .heart-shape`, 16×16px, blanco).
2. Los corazones flotantes (`.bh.heart-shape`, tamaño y color
   aleatorios vía JS).

Como usa `currentColor`, cambiar el corazón de color es tan simple como
poner `color: ...` en el elemento — no hay que tocar la forma.

**Si se vuelve a rotar la forma:** el ángulo correcto es `rotate(45deg)`
(sentido horario). Con `-45deg` el corazón queda de lado, con la punta
apuntando a la izquierda en vez de hacia abajo — ya se probó y se
corrigió una vez, ver `git log` de `style.css` si vuelve a pasar.

## Breakpoints

- `980px` — el panel del ramo pasa de apilado a estar al lado del
  carrusel (regla en el `<style>` inline de `index.html`).
- `760px` — `main.container` pasa de fila a columna.
- `720px` — reduce corazones de fondo, agranda hit-area de los
  indicadores, reduce padding del listón.
- `600px` — el panel del ramo ocupa el 100% del ancho.

## Movimiento

Todas las animaciones (ramo creciendo, listón abriéndose, corazones
flotando, pulso del botón) respetan
`@media (prefers-reduced-motion: reduce)`, definido una vez en
`style.css` y aplicado globalmente con `*`.
