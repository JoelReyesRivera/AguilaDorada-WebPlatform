---
version: 1
slug: "pages-nosotros-html"
primary_target: "pages/nosotros.html"
related_targets: []
---

# Surface: Nosotros (equipo)

Scope: nueva página interna `pages/nosotros.html`, enlazada desde el nav "Nosotros" del landing.
Visitor mode: Persuade (construir confianza institucional mostrando a las personas detrás).
Audiencia: personas evaluando terapia/colaboración que quieren saber quién está detrás de Águila Dorada A.C.
Job: reconocer al equipo — 7 fundadores y 4 psicólogas — con foto, puesto y aporte.
Acción: seguir hacia agendar terapia / sumarse como voluntario / volver al inicio.
Constraints: mundo visual fijo (DESIGN.md "El Santuario Cálido"); reutiliza header, crisis-bar, footer y styles.css del landing. No inventar nombres, puestos ni testimonios (PRODUCT.md) — contenido de personas va como marcador de posición claramente rotulado para que la asociación lo complete. Fotos: monograma con iniciales por ahora, reemplazables por archivos en `assets/images/equipo/`.

## Direction contract

THESIS: Esta página es un directorio con calidez editorial — retratos y nombres tratados como una publicación institucional, no una rejilla de tarjetas de producto con icono+título+texto. Rechaza el patrón "equipo" genérico de avatares circulares idénticos sin jerarquía.

OWN-WORLD: Fondo cálido de base (crema / crema-alt por bloque), tarjetas en papel con borde 1px `--line` y esquinas 12px. Cormorant Garamond para nombres (21px) y títulos de grupo (30px); Montserrat 700 en mayúsculas tracked para el puesto, en `--green-deep`. Avatar: círculo 76px con el degradado tierra del sitio (`#EFE6D2`→`#D9C5A0`) y monograma serif; psicólogas con degradado navy-tinte. Sin sombras salvo hover de tarjeta (`0 14px 28px rgba(43,27,20,.12)`), igual que `.event-card`.

STORY: El visitante entiende que Águila Dorada es un equipo real y multidisciplinario; cree que hay respaldo profesional (psicólogas con cédula) y compromiso comunitario (fundadores); y sigue hacia agendar o sumarse.

FIRST VIEWPORT: Encabezado de página a ancho de columna: kicker verde "Quiénes están detrás", H1 Cormorant ~46px en dos líneas, párrafo de intro (≤60ch). Debajo empieza el bloque "Fundadores" con su barra de título (nombre + conteo) y las primeras filas de la rejilla `auto-fill minmax(250px,1fr)`. Sin hero de imagen; la tipografía carga la entrada. Acciones primarias (agendar / voluntariado) viven en el cierre y en el header heredado.

FORM: Directorio editorial en dos grupos (Fundadores 7 / Equipo de psicología 4), rejilla fluida de tarjetas-retrato, cierre en banda café con dos CTAs. Extensión de superficie dentro del mundo establecido — sin torneo de concepto. Seed key: n/a (code-led, extensión).

FINISH: unreviewed and undocumented is unfinished; this build ends with the finish review, the verdict, DESIGN.md, and every shipping raster carrying its provenance
