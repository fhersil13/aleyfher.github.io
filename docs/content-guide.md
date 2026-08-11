# Cómo cambiar contenido

No hace falta tocar la lógica del carrusel, los corazones ni el ramo
para actualizar lo que realmente cambia seguido: fotos, textos y la
fecha del contador.

## Fotos

Viven en `fotos/foto1.jpeg` … `foto10.jpeg` y se referencian una por una
en `index.html`, dentro de `.carousel-track` (cada `<div class="slide">`
tiene un `<img src="fotos/fotoN.jpeg">`). Para:

- **Reemplazar una foto**: sobrescribe el archivo `fotoN.jpeg` con el
  mismo nombre — no hay que tocar el HTML.
- **Agregar/quitar fotos**: agrega o borra un bloque `<div class="slide"
  data-index="N">...</div>` completo, y actualiza `data-total` en
  `.carousel-track` (hoy en `10`). Los índices (`data-index`) deben ser
  consecutivos empezando en `0`.
- **Favicon**: usa `fotos/foto6.jpeg` (ver `<link rel="icon">` en el
  `<head>`) — si cambias esa foto, cambia también el favicon.

## Textos

| Texto | Dónde |
|---|---|
| Eyebrow del hero ("14 de febrero") | `<p class="hero-eyebrow">` en `index.html` |
| Título del hero | `<h1>` dentro de `<header class="hero">` |
| Texto del listón | `<span class="r-text"><i>...</i></span>` |
| Firma del footer | `<p class="sig-line">` y `<p class="sig-mono">` |
| Título de la pestaña | `<title>` en el `<head>` |

## Fecha de inicio del contador

En `index.html`, dentro de `startTogetherCounter()`:

```js
const start = new Date(2025, 6, 26, 16, 0, 0);
```

Ojo: el mes es **0-indexado** (`6` = julio, no `7`). Los argumentos son
`(año, mes, día, hora24, minutos, segundos)`. Se calcula en la hora
local del navegador de quien visita la página — no se ajusta a ninguna
zona horaria fija.

## Paleta y tipografía

Ver [design-system.md](design-system.md) — todo son variables CSS en
`:root`, no hay que buscar hex sueltos por el archivo.
