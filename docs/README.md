# Documentación — Ale & Fher

Sitio de una sola página, sin frameworks ni build step: `index.html` +
`style.css`, desplegado en GitHub Pages. Esta carpeta documenta cómo está
armado, para poder retomarlo más adelante sin tener que releer todo el
código.

## Índice

- [architecture.md](architecture.md) — estructura de archivos y cómo
  funciona cada pieza interactiva (carrusel, listón, corazones, ramo,
  contador de días).
- [design-system.md](design-system.md) — paleta, tipografía y breakpoints.
- [content-guide.md](content-guide.md) — cómo cambiar fotos, textos y
  fechas sin tocar lógica.
- [deployment.md](deployment.md) — cómo se despliega en GitHub Pages, el
  workflow de Actions, y los problemas de dominio que ya resolvimos (para
  no volver a perder tiempo si se repiten).

## Resumen rápido

| | |
|---|---|
| Stack | HTML + CSS + JS vanilla, sin dependencias de build |
| Fuentes | Google Fonts: Fraunces (itálica, gestos) + Karla (cuerpo) |
| Hosting | GitHub Pages, vía Actions (`.github/workflows/deploy.yml`) |
| URL | `https://fherneysilva.github.io/valentine/` |
| Fotos | `fotos/foto1.jpeg` … `foto10.jpeg`, 10 fijas en el carrusel |
