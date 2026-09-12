---
name: Águila Dorada A.C.
description: Salud mental accesible en Culiacán — santuario cálido en tonos tierra y dorado con acentos institucionales por categoría.
colors:
  cream: "#FAF6EF"
  cream-alt: "#F1EBDE"
  paper: "#FFFDF9"
  brown: "#2B1B14"
  brown-soft: "#54423A"
  gold: "#C9A047"
  gold-deep: "#A9812F"
  gold-ink: "#836227"
  green: "#7BA384"
  green-deep: "#52765B"
  navy: "#1F3A5F"
  navy-deep: "#152A45"
  terracotta: "#D47A5A"
  terracotta-deep: "#B85C3D"
  line: "#E4DBC8"
  crisis-red: "#8F2F22"
typography:
  display:
    fontFamily: "Cormorant Garamond, Georgia, serif"
    fontSize: "52px"
    fontWeight: 600
    lineHeight: 1.15
    letterSpacing: "0.2px"
  headline:
    fontFamily: "Cormorant Garamond, Georgia, serif"
    fontSize: "36px"
    fontWeight: 600
    lineHeight: 1.15
  title:
    fontFamily: "Cormorant Garamond, Georgia, serif"
    fontSize: "26px"
    fontWeight: 600
    lineHeight: 1.15
  subtitle:
    fontFamily: "Cormorant Garamond, Georgia, serif"
    fontSize: "19px"
    fontWeight: 600
    lineHeight: 1.2
  body:
    fontFamily: "Montserrat, sans-serif"
    fontSize: "16px"
    fontWeight: 400
    lineHeight: 1.65
  label:
    fontFamily: "Montserrat, sans-serif"
    fontSize: "13px"
    fontWeight: 600
    letterSpacing: "0.2px"
  caption:
    fontFamily: "Montserrat, sans-serif"
    fontSize: "12px"
    fontWeight: 500
    lineHeight: 1.3
  micro:
    fontFamily: "Montserrat, sans-serif"
    fontSize: "11px"
    fontWeight: 600
    letterSpacing: "1.2px"
rounded:
  xs: "4px"
  sm: "5px"
  md: "6px"
  card: "10px"
  lg: "12px"
  xl: "14px"
  xxl: "16px"
  pill: "20px"
  circle: "50%"
spacing:
  xs: "8px"
  sm: "14px"
  md: "24px"
  lg: "40px"
  xl: "56px"
components:
  button-primary:
    backgroundColor: "{colors.gold}"
    textColor: "{colors.brown}"
    rounded: "{rounded.md}"
    padding: "12px 24px"
  button-primary-hover:
    backgroundColor: "{colors.gold-deep}"
  button-secondary:
    backgroundColor: "{colors.navy}"
    textColor: "{colors.cream}"
    rounded: "{rounded.md}"
    padding: "12px 24px"
  button-secondary-hover:
    backgroundColor: "{colors.navy-deep}"
  button-outline:
    backgroundColor: "transparent"
    textColor: "{colors.brown}"
    rounded: "{rounded.md}"
    padding: "12px 24px"
  button-outline-hover:
    backgroundColor: "{colors.brown}"
    textColor: "{colors.cream}"
---

# Design System: Águila Dorada A.C.

## Overview

**Creative North Star: "El Santuario Cálido"**

Águila Dorada se presenta como un refugio humano con la seriedad de una institución de salud mental formalmente constituida: cálido sin perder autoridad. La paleta parte de tonos tierra (crema, papel, café) que actúan como el "suelo" del sitio, sobre el cual el dorado marca los momentos de invitación (CTAs, acentos editoriales) y cuatro colores institucionales — dorado, navy, verde y terracota — codifican categorías de acción sin competir entre sí en una misma vista.

La tipografía combina una serif editorial (Cormorant Garamond) para todo titular, con una sans humanista (Montserrat) para el cuerpo — la misma dualidad calidez/seriedad que gobierna el color. El diseño es de baja densidad, con secciones amplias (56px de padding vertical) y bloques de texto acotados (46–64ch) que favorecen lectura pausada sobre densidad informativa.

**Key Characteristics:**
- Fondo cálido de base (crema/papel), nunca blanco puro ni gris frío.
- Un acento dorado que marca CTAs y momentos editoriales, usado con moderación.
- Cuatro colores de categoría fijos que codifican tipo de evento/acción en todo el sitio.
- Serif display + sans body como firma tipográfica constante.
- Secciones espaciosas, cards con esquinas suavemente redondeadas, casi sin bordes duros.

## Colors

La paleta es cálida y terrosa en la base, con un sistema de cuatro colores institucionales que funciona como código de categoría.

### Primary
- **Dorado Institucional** (`#C9A047`, hover `#A9812F`): el acento principal — CTAs primarios (`.btn-gold`), acentos de titulares (`.accent`), y la categoría "Comunidad" en tags/timeline/iconos.

### Secondary
- **Azul Marino Profundo** (`#1F3A5F`, hover `#152A45`): segundo color de acción — CTAs de eventos/registro (`.btn-navy`), servicio de Diagnóstico, y la categoría "Educativo".

### Tertiary
- **Verde Salvia** (`#7BA384`, texto `#52765B`): usado en kickers/etiquetas ("hero-kicker", "kicker") y la categoría "Asistencial".
- **Terracota** (`#D47A5A`, texto `#B85C3D`): acentos de hover en navegación y la categoría "Cultural".

### Neutral
- **Crema** (`#FAF6EF`): fondo base del body.
- **Crema Alterna** (`#F1EBDE`): fondo de secciones alternas (`.section-alt`) y tarjetas info/hoy en la bitácora.
- **Papel** (`#FFFDF9`): fondo de header, footer, bitácora y event cards — la superficie "elevada" sin sombra.
- **Café** (`#2B1B14`): texto principal, títulos, y fondo de la banda CTA/franja de café.
- **Café Suave** (`#54423A`): texto secundario/subtítulos en todo el sitio.
- **Línea** (`#E4DBC8`): bordes y divisores sutiles.
- **Rojo de Crisis** (`#8F2F22`): reservado exclusivamente para la barra de aviso de crisis; nunca se usa como color decorativo.
- **Dorado Tinta** (`#836227`): variante de texto del dorado, usada solo donde el dorado debe llevar texto pequeño (ej. `.tl-date` en la bitácora) y `--gold-deep` no alcanza 4.5:1 de contraste. `--gold-deep` sigue siendo válido para acentos grandes e iconos (umbral 3:1).
- **Degradados decorativos** (`#EFE6D2`→`#D9C5A0`, `#EFE6D2`→`#DDCBAA`): paradas de gradiente de un solo uso en `.photo-block` y `.event-photo` como relleno de imagen ausente; no son tokens de color de UI y no requieren contraste de texto.

### Named Rules
**La Regla del Código de Categoría.** Cada una de las cuatro categorías institucionales tiene un color fijo y exclusivo: **Comunidad = Dorado**, **Educativo = Navy**, **Asistencial = Verde**, **Cultural = Terracota**. Este mapeo se aplica siempre en tags de evento, puntos de la bitácora e iconos de estadística; los colores nunca se reasignan a otra categoría ni se usan de forma puramente decorativa fuera de este código.

**La Regla del Rojo de Crisis.** `#8F2F22` (y su acento `#FFE09B`) se usa únicamente en la barra de aviso de crisis. Ningún otro componente del sitio debe tomar este color, para que la señal de emergencia permanezca inequívoca.

## Typography

**Display Font:** Cormorant Garamond (con fallback serif/Georgia)
**Body Font:** Montserrat (con fallback sans-serif)

**Character:** Un serif editorial de peso medio-alto para todo encabezado transmite calidez institucional y tradición; una sans humanista de peso variable lleva el cuerpo, las etiquetas y los datos, aportando claridad funcional. La pareja crea un contraste "voz cálida / información clara" consistente en todo el sitio.

### Hierarchy
- **Display** (600, 52px / 34–38px en móvil, line-height 1.15): H1 del hero.
- **Headline** (600, 32–38px, line-height 1.15): títulos de sección (`.head-block h2`, `.bitacora-head h2`).
- **Title** (600, 22–26px, line-height 1.15): títulos de tarjeta de servicio y de columna CTA.
- **Subtitle** (600, 17–19px, line-height 1.2, itálica en cita de foto): títulos de tarjeta de evento (`.event-info h3`), cita bajo el hero.
- **Body** (400, 14.5–17px, line-height 1.65): párrafos y descripciones; ancho máximo 38–64ch. Variaciones de ±0.5–2px entre componentes (14.5 vs 15 vs 16.5px) son ajustes finos aceptados de este mismo rol, no drift.
- **Label** (500–700, 12–14px, letter-spacing 0.2–1.2px, a veces mayúsculas): kickers, tags, precios, fechas de evento.
- **Caption** (500, 12px, line-height 1.3): metadatos secundarios (hora, sede, "cupo limitado").
- **Micro** (600, 10–11px, letter-spacing 1.1–1.2px, mayúsculas): etiqueta de marca (`.brand-text .tag`); nunca baja de 11px, ni en el breakpoint más angosto.

Valores puntuales fuera de esta lista (p. ej. 7px en el símbolo de un punto de bitácora, 20px en el icono de menú) son detalles de un solo componente, no pasos del sistema tipográfico.

## Layout

Contenedor centrado con `max-width: 1180px` y padding lateral de 32px (20px en móvil muy angosto). Las secciones usan padding vertical de 56px (40px en móvil ≤640px). El grid principal es de dos columnas asimétricas para hero (1.05fr/.95fr) y misión (1fr/1fr), colapsando a una columna en ≤980px. Las grillas de tarjetas (servicios, eventos) usan `repeat(auto-fit, minmax(260px,1fr))` o columnas fijas que se reducen progresivamente en breakpoints de 980px, 640px y 480px. El ritmo espacial se apoya en pasos de 8/14/24/40/56px.

## Elevation & Depth

El sistema no sigue una regla única de "plano por defecto": la mayoría de las superficies (header, footer, bitácora, tarjetas de info) son planas con solo un borde de 1px en `--line` para separación, pero el hero (`.photo-block`) lleva una sombra ambiental permanente (`0 20px 38px rgba(43,27,20,.2)`) como acento fotográfico, y las tarjetas de evento (`.event-card`) ganan sombra y elevación (`translateY(-4px)`) solo como respuesta a hover/focus. Tratar cada caso según su contexto en vez de aplicar una sola doctrina de elevación al sistema completo.

### Shadow Vocabulary
- **Sombra de foto hero** (`box-shadow: 0 20px 38px rgba(43,27,20,.2)`): acento permanente bajo el bloque fotográfico del hero.
- **Sombra de hover de tarjeta** (`box-shadow: 0 14px 28px rgba(43,27,20,.14)`): feedback de interacción en `.event-card` y CTAs de evento (`0 5px 12px rgba(31,58,95,.2)`).

## Shapes

Esquinas suavemente redondeadas en casi todo, en una progresión de 4 a 16px según el tamaño del elemento: detalles pequeños (skip-link) a 4px, botones y filas de banco a 5–6px, bloques "hoy"/info a 10px, tarjetas e imágenes a 12–14px, el visual de misión a 16px, y chips/tags/pills en cápsula completa (20px o más). Avatares e iconos circulares usan 50%. No hay elementos con esquinas rectas duras salvo la barra de crisis y el header/footer (rectangulares por ser franjas de ancho completo).

## Components

### Buttons
- **Shape:** radio 6px (`border-radius: 6px`).
- **Primary (Dorado):** fondo `--gold`, texto `--brown`; hover a `--gold-deep`.
- **Secondary (Navy):** fondo `--navy`, texto `--cream`; hover a `--navy-deep`.
- **Outline:** borde 1.5px `--brown`, transparente; hover invierte a fondo `--brown` / texto `--cream`.
- **Hover/Focus común:** `translateY(-1px)` en todos los botones; foco visible con contorno de 3px en `--navy` (o `--gold-deep` sobre enlaces/botones).

### Chips / Tags (pills)
- **Style:** cápsula (`border-radius: 20px`), fondo sólido según categoría, texto contrastante; tags de evento en esquina superior izquierda de la foto.
- **State:** los pills de servicio usan borde `currentColor` sobre fondo oscuro con opacidad reducida (0.85) en vez de relleno sólido.

### Cards / Containers
- **Corner Style:** 12px (tarjetas de servicio, evento, info) a 14px (bloque fotográfico del hero) a 16px (visual de misión).
- **Background:** `--paper` para event-card; `--brown`/`--navy` sólidos para service-card (alto contraste); `--cream-alt` para info-card.
- **Shadow Strategy:** plano en reposo salvo el hero; ver Elevation & Depth.
- **Border:** 1px `--line` en la mayoría; ausente en las tarjetas de servicio de color sólido.
- **Internal Padding:** 40px (service-card), 20–22px (event-body), 28–30px (info-card).

### Timeline / Bitácora (signature component)
Punto circular de 16px por evento, coloreado según la Regla del Código de Categoría, con un símbolo interno (`+`, `□`, `✦`) que refuerza la categoría más allá del color. El ítem "hoy" se distingue con fondo `--cream-alt` y una barra vertical continua, funcionando como ancla visual del progreso institucional.

### Navigation
- Header pegajoso (`sticky`) sobre `--paper` con borde inferior `--line`. Enlaces en Montserrat 14.5px/500; hover a `--terracotta-deep`. En móvil (<980px) colapsa a menú hamburguesa con panel desplegable de enlaces apilados.

## Do's and Don'ts

### Do:
- **Do** usar el código de categoría (dorado/navy/verde/terracota) de forma consistente en tags, iconos y timeline — nunca reasignar un color a otra categoría.
- **Do** mantener el rojo de crisis (`#8F2F22`) exclusivo a la barra de aviso de crisis.
- **Do** usar Cormorant Garamond para todo titular y Montserrat para todo cuerpo/etiqueta; no introducir una tercera familia tipográfica.
- **Do** mantener esquinas redondeadas suaves (6–16px) y cápsulas completas para chips/tags.

### Don't:
- **Don't** usar blanco puro ni gris frío como fondo — el sistema es cálido/terroso de base (`--cream`, `--paper`).
- **Don't** aplicar sombra decorativa fuera de los casos ya establecidos (hero, hover de tarjetas); no convertir la elevación en una regla global del sistema.
- **Don't** inventar testimonios, cifras de impacto o casos de éxito en ningún componente visual — solo evidencia real aportada por la asociación (ver `PRODUCT.md`).
- **Don't** alterar los datos de contacto, CLABE o enlaces institucionales al ajustar estilos de los componentes que los muestran.
