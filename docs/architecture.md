# Arquitectura

## Archivos

```
index.html              todo el markup + JS (un único <script> al final del body)
style.css                todo el CSS compartido
fotos/foto1..10.jpeg      fotos del carrusel, en orden fijo
.github/workflows/deploy.yml   despliegue a GitHub Pages
```

No hay build step: lo que está en el repo es exactamente lo que sirve
GitHub Pages. Los estilos viven en dos lugares:

- `style.css` — todo lo "de producto": paleta, tipografía, layout del
  carrusel, corazones, listón, footer.
- El `<style>` inline dentro de `<head>` de `index.html` — solo el layout
  específico del panel del ramo y el botón del corazón (posicionamiento,
  breakpoints propios). Se dejó ahí porque nació como un parche puntual;
  si se vuelve a tocar mucho, vale la pena moverlo a `style.css`.

## Piezas interactivas (todas viven en el `<script>` de `index.html`)

### 1. Carrusel

`goTo()/next()/prev()` mueven `.carousel-track` con un `translateX` en
porcentaje. Los puntos indicadores se generan dinámicamente (uno por
`.slide`) y no hay botones prev/next visibles — la navegación es por
puntos, teclado (flechas) y swipe (`pointerdown/move/up` con un umbral de
40px).

El autoplay (`setInterval`, 4s) **no arranca hasta que se abre el
listón** — ver siguiente sección.

### 2. Listón de apertura (`#openingCover`)

Cubre el carrusel hasta que se hace click/Enter/Espacio. Al abrirse:
agrega la clase `.opened` (dispara la animación CSS de las dos mitades
del listón separándose), espera el `transitionend` para eliminar el
elemento del DOM, y ahí recién arranca el autoplay. Es la razón por la
que las fotos no se ven hasta que Ale interactúa — es intencional, parte
del "ritual de regalo".

### 3. Corazones flotantes (`createBgHearts`)

Genera `<span class="bh heart-shape">` con tamaño, posición, color
(vino o dorado, aleatorio) y duración/retraso de animación aleatorios,
y los inyecta en `.bg-hearts` (detrás del carrusel) y `.page-hearts`
(cubriendo toda la página, oculto en mobile por rendimiento). La forma
de corazón es puro CSS (`.heart-shape` en `style.css`: un cuadrado
rotado 45° + dos círculos), no SVG ni emoji — ver
[design-system.md](design-system.md#forma-de-corazón).

En mobile se generan menos (18+12 en vez de 48+84) para no castigar
batería/CPU.

### 4. Contador "días juntos" (`startTogetherCounter`)

Calcula la diferencia entre `Date.now()` y una fecha de inicio fija
(`new Date(2025, 6, 26, 16, 0, 0)` — 26 de julio de 2025, 4:00 p.m.,
hora local del navegador de quien mira la página) y actualiza cada
segundo dos elementos: el número grande de días (`#togetherDays`) y un
reloj `HH:MM:SS` (`#togetherClock`). No usa `aria-live` a propósito —
anunciarlo cada segundo sería muy molesto con lector de pantalla.

Para cambiar la fecha de inicio, ver
[content-guide.md](content-guide.md#fecha-de-inicio-del-contador).

### 5. Ramo procedural (`assembleBouquet`)

Al tocar el botón del corazón (`#bouquetBtn`), genera un SVG desde cero
dentro de `#bouquetRoot`: por cada flor (24 por defecto) dibuja un tallo
(`makeStem`, una curva Bézier con grosor y verde aleatorios) y una flor
en la punta (`makeFlower`, 5–8 pétalos elípticos alrededor de un centro,
con una paleta elegida al azar entre 4 combinaciones rosa/durazno). Cada
pieza entra con una animación de opacidad/escala escalonada
(`setTimeout` con offsets aleatorios) para que el ramo se sienta "crecer"
en vez de aparecer de golpe. Es 100% generativo — no hay dos ramos
iguales.

`assembleBouquet` queda expuesta en `window.assembleBouquet` por si se
quiere invocar manualmente desde la consola (debug).

## Accesibilidad / rendimiento

- `@media (prefers-reduced-motion: reduce)` en `style.css` reduce todas
  las animaciones a ~0 para quien lo pida a nivel de sistema operativo.
- Las imágenes usan `loading="lazy"`.
- Los corazones de fondo se reducen a la mitad en mobile.
