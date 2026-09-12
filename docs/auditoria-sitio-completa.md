# Auditoría completa del sitio — accesibilidad, diseño y rendimiento

**Sitio:** aguiladorada.org · **Fecha:** 11 de septiembre de 2026
**Alcance:** las 7 páginas (`index`, `servicios`, `nosotros`, `contacto`, `eventos-pasados`, `voluntariado`, `privacidad`).
**Método:** axe-core 4.10.2 sobre cada página en Chrome sin interfaz; mediciones propias de desbordamiento, tamaño de objetivos táctiles, tipografía, foco y rendimiento a 1440 px y 390 px; detector de diseño de Impeccable; revisión estática del marcado.

> El apartado de buscadores va aparte, en [`auditoria-buscadores-google-bing.md`](auditoria-buscadores-google-bing.md).

---

## Resumen ejecutivo

El sitio está **notablemente bien construido**. En 14 combinaciones de página y ancho no hubo **ni un solo desbordamiento horizontal**, el foco del teclado es visible en todas las páginas, la jerarquía de encabezados no tiene saltos, el enlazado interno es una malla completa sin huérfanas y el detector de diseño de Impeccable no reporta nada.

axe-core encontró **solo dos tipos de problema en todo el sitio**, y ambos se arreglan con una regla de CSS y una etiqueta HTML:

1. **Contraste insuficiente** en el texto `.kicker` sobre fondo crema (4.31:1 frente al 4.5:1 exigido) — afecta a 4 páginas.
2. **La barra de crisis queda fuera de las regiones de referencia** — afecta a las 7.

Hay además un hallazgo de usabilidad real: en `voluntariado.html` **siete enlaces distintos comparten el texto "postularme en esta área"**, lo que deja a quien usa lector de pantalla sin manera de distinguirlos.

**Diagnóstico general: base sólida.** Lo que falta es acabado fino, no reconstrucción.

---

## 1. Accesibilidad (axe-core, las 7 páginas)

| Página | Críticos | Serios | Moderados | Total |
|---|---:|---:|---:|---:|
| `index.html` | 0 | 3 | 1 | 4 |
| `contacto.html` | 0 | 2 | 2 | 4 |
| `servicios.html` | 0 | 2 | 1 | 3 |
| `voluntariado.html` | 0 | 2 | 1 | 3 |
| `nosotros.html` | 0 | 0 | 1 | 1 |
| `eventos-pasados.html` | 0 | 0 | 1 | 1 |
| `privacidad.html` | 0 | 0 | 1 | 1 |

**Cero problemas de impacto crítico en todo el sitio.**

### A1 🟠 Serio — Contraste del `.kicker` sobre fondo alterno

`#52765b` sobre `#f1ebde` da **4.31:1**; el mínimo de WCAG 2.1 AA (criterio 1.4.3) es 4.5:1 para texto de 14 px. Falla por poco, pero falla.

**Dónde:** `index.html` (1), `contacto.html` (2), `servicios.html` (2), `voluntariado.html` (2) — siempre el mismo patrón: `.section-alt > .wrap > .head-block > .kicker`.

**Corrección:** oscurecer el verde solo lo necesario. `#4a6b52` sobre `#f1ebde` da **5.04:1** y es visualmente casi idéntico, así que no rompe el sistema "El Santuario Cálido". Es un cambio en una sola declaración de `styles.css` y resuelve las 4 páginas a la vez.

### A2 🟠 Serio — Contraste de la etiqueta `.limited`

`#b85c3d` sobre `#fffdf9` da **4.45:1** en negrita de 12 px. Falla por 0.05.

**Dónde:** `index.html`, en las dos tarjetas de próximos eventos ("Cupo limitado").

**Corrección:** `#a85234` da **5.21:1**. Otra opción, ya que es un aviso de urgencia, es subir la tipografía a 13–14 px, lo que además ayuda a leerlo.

### A3 🟡 Moderado — `.crisis-bar` fuera de las regiones de referencia

En las **7 páginas**, la barra de crisis está fuera de `<header>`, `<main>` y `<footer>`. Quien navega por regiones con lector de pantalla se la salta, y es justamente el contenido más urgente del sitio: la línea de atención en crisis.

**Corrección:**
```html
<aside class="crisis-bar" aria-label="Atención en crisis">
```
Una etiqueta por página. Alto impacto respecto al esfuerzo, porque el contenido es sensible.

### A4 🟡 Moderado — `aside` anidado dentro de otra región

En `contacto.html`, `.contact-aside` es un `<aside>` dentro de `<main>`. axe lo marca porque un `complementary` anidado no aparece en la lista de regiones.

**Corrección:** cambiarlo a `<div>` (el contenido sigue siendo legible en orden) o sacarlo de `<main>`. La primera opción es más simple y no altera el diseño.

### A5 🟠 Serio (usabilidad) — Siete enlaces con el mismo texto

En `voluntariado.html`, siete enlaces dicen exactamente **"postularme en esta área"** y cada uno abre un WhatsApp distinto (marketing, gestión de eventos, ventas, difusión, talleres educativos, cultural, jornadas de víveres).

Los lectores de pantalla permiten listar los enlaces de una página fuera de contexto. Ahí aparecen siete entradas idénticas, indistinguibles. Incumple el criterio WCAG 2.4.4 (propósito del enlace en su contexto).

**Corrección** — sin tocar el diseño visual, con `aria-label`:
```html
<a href="https://wa.me/..." aria-label="Postularme como voluntario en marketing y promoción">
  postularme en esta área
</a>
```
El texto visible no cambia; el lector de pantalla anuncia el área concreta.

---

## 2. Diseño responsivo

Medido a **1440 px** y **390 px** en las 7 páginas (14 combinaciones).

| Comprobación | Resultado |
|---|---|
| Desbordamiento horizontal | ✅ **0 px en las 14 combinaciones** |
| Elementos que se salen del ancho | ✅ Ninguno *(el `skip-link` en −9999 px es intencional)* |
| Tamaño de objetivos táctiles (WCAG 2.2 AA, 2.5.8) | ✅ **Cero fallas reales** |
| Foco de teclado visible | ✅ `outline: 3px solid` en las 7 |

### Sobre los objetivos táctiles

La primera medición marcó decenas de enlaces con menos de 24 px de alto. **Los verifiqué aplicando las excepciones del criterio y ninguno es una falla real:**

- Los enlaces del pie y de navegación pasan por la **excepción de espaciado**: la distancia entre centros supera los 24 px.
- Los enlaces dentro de párrafos pasan por la **excepción de texto en línea**.
- Las dos casillas de radio de `contacto.html` miden 13×13 px, pero están dentro de etiquetas `<label>` clicables de **155×42** y **122×42** px. El objetivo real es la etiqueta completa y cumple de sobra.

Lo anoto explícitamente porque es el tipo de hallazgo que un informe automático reporta como falla sin serlo.

### Texto pequeño (observación, no falla)

No incumple ningún criterio, pero conviene tenerlo presente:

| Elemento | Tamaño | Dónde |
|---|---|---|
| `.ph-aside-label` | 11 px | 6 páginas |
| `.ca-label` | 11 px | `contacto.html` (×3) |
| `.vol-chip` | 11 px | `voluntariado.html` (×4) |
| `.ig-meta` | 11.5 px | `eventos-pasados.html` (×9) |
| `.limited`, `.event-tag`, `.tm-cred` | 12 px | varias |

Son etiquetas y metadatos, no texto de lectura. A 11 px la legibilidad en móvil ya se resiente; subir el mínimo a 12 px costaría poco.

---

## 3. Rendimiento

Medido en servidor local (sin latencia de red, así que los tiempos son un piso, no una predicción).

| Página | FCP | Recursos | Peso |
|---|---:|---:|---:|
| `index.html` | 216–708 ms | 5–7 | **369 KB** |
| `nosotros.html` | 52–144 ms | 12–13 | 257 KB |
| `eventos-pasados.html` | 232–284 ms | 7–12 | 34 KB + incrustados |
| `servicios.html` | 48–56 ms | 5 | ~85 KB |
| `contacto.html` | 292 ms | 5–6 | ~85 KB |
| `voluntariado.html` | 64–136 ms | 5 | ~90 KB |
| `privacidad.html` | 52–60 ms | 5 | ~85 KB |

### Lo que ya está bien

- **Cero scripts externos** en las 7 páginas. Todo el JS es interno y suma menos de 5 KB por página.
- Una sola hoja de estilos compartida (58 KB), que se almacena en caché entre páginas.
- `preconnect` a Google Fonts, `display=swap`, `preload` del `hero` con `fetchpriority="high"`.
- El iframe del mapa tiene `loading="lazy"` y `title`.

### P1 🟠 `logo.png` pesa 108 KB

Se carga en las **7 páginas**, dos veces cada una (encabezado y pie) y también como favicon — siempre a tamaño pequeño. Es el segundo recurso más pesado del sitio y el de peor relación peso/beneficio.

**Corrección:** comprimirlo o generar una versión de 200 px de ancho. Se recuperan unos 100 KB en cada visita.

### P2 🟠 `hero.jpg` pesa 201 KB

Es el recurso más pesado y determina el LCP de la portada. Ya tiene `preload` con `fetchpriority="high"`, lo cual está bien resuelto; lo que falta es bajarlo de peso.

**Corrección:** recomprimir con mozjpeg (calidad 80) o servir WebP con respaldo JPEG. Un ahorro razonable es del 40–50 % sin pérdida visible.

### P3 🟠 Los incrustados de Instagram son lo más lento del sitio

En `eventos-pasados.html`, las peticiones a Instagram tardaron entre **956 ms y 3 335 ms**. Es tráfico de terceros, fuera de nuestro control, y son 9 incrustados.

La página ya usa `IntersectionObserver` para cargarlos al acercarse a la pantalla, que es la mitigación correcta. Aun así, la primera tanda compite con el renderizado.

**Corrección posible:** mostrar una miniatura propia (como se hace en los eventos de la portada) y cargar el incrustado solo al hacer clic. Cambia el diseño, así que queda como propuesta, no como corrección.

### P4 🟡 Marcas de logo sin `width`/`height`

Los `<img class="brand-mark">` del encabezado y el pie no declaran dimensiones en las 7 páginas. El navegador no reserva el espacio y puede producirse un pequeño salto de maquetación (CLS) al cargar.

**Corrección:** añadir `width` y `height` con las dimensiones reales.

---

## 4. Calidad del marcado

| Comprobación | Resultado |
|---|---|
| Un `<h1>` por página | ✅ 7/7 |
| Saltos de nivel en encabezados | ✅ **Ninguno en las 7** |
| Imágenes sin `alt` | ✅ **Ninguna real** *(ver nota)* |
| Enlaces internos rotos | ✅ Ninguno |
| Páginas huérfanas | ✅ Ninguna |
| Texto de ancla genérico ("clic aquí") | ✅ Ninguno |
| `rel="noopener"` en enlaces externos | ✅ 76/76 |
| `lang` declarado | ✅ 7/7 |
| Hallazgos del detector de Impeccable | ✅ 0 |

**Nota sobre `alt`:** dos análisis automáticos marcaron imágenes sin `alt`. Ambos son falsos positivos y los descarté tras revisarlos:
- `nosotros.html:51` — la cadena `<img>` aparece como texto **dentro de un comentario HTML** que explica el comportamiento de respaldo del monograma.
- `index.html:596` — es el `<img>` vacío de la caja de luz, que está dentro de un contenedor `[hidden]` y solo recibe `src` y `alt` cuando el JS lo abre. Ya está documentado como excepción en `.impeccable/config.json`.

**La jerarquía de encabezados sin un solo salto en 7 páginas es un resultado poco común y vale la pena conservarlo** al agregar contenido nuevo.

---

## 5. Plan de acción priorizado

### Ganancias rápidas — esta semana

| # | Acción | Archivos | Impacto | Esfuerzo |
|---|---|---|---|---|
| 1 | Envolver `.crisis-bar` en `<aside aria-label="Atención en crisis">` | las 7 | **Alto** | 10 min |
| 2 | Subir el contraste del `.kicker` a `#4a6b52` | `styles.css` | **Alto** | 2 min |
| 3 | `aria-label` distinto en los 7 enlaces "postularme en esta área" | `voluntariado.html` | **Alto** | 15 min |
| 4 | Subir el contraste de `.limited` a `#a85234` | `styles.css` | Medio | 2 min |
| 5 | Comprimir `logo.png` (−100 KB en las 7 páginas) | `assets/` | Medio | 10 min |
| 6 | Comprimir `hero.jpg` (−80 a 100 KB en la portada) | `assets/` | Medio | 10 min |
| 7 | `width`/`height` en las marcas de logo | las 7 | Bajo | 10 min |
| 8 | Cambiar `.contact-aside` de `<aside>` a `<div>` | `contacto.html` | Bajo | 2 min |

### Mejoras de fondo

| # | Acción | Impacto | Esfuerzo |
|---|---|---|---|
| 9 | Subir el tamaño mínimo de texto de 11 px a 12 px | Medio | Bajo |
| 10 | Sustituir los incrustados de Instagram por miniaturas propias que carguen al hacer clic | Medio | Alto |
| 11 | Revisión manual con lector de pantalla (NVDA o VoiceOver) del formulario de contacto | **Alto** | Medio |
| 12 | Revisión de navegación completa con teclado, comprobando el orden de tabulación en el menú móvil | **Alto** | Medio |

---

## 6. Qué comprobó cada herramienta

| Herramienta | Qué cubrió |
|---|---|
| **axe-core 4.10.2** | 7 páginas — contraste, regiones, ARIA, formularios, encabezados |
| **agent-browser** (Chrome sin interfaz) | 14 combinaciones de página y ancho — desbordes, objetivos táctiles, foco, tipografía, rendimiento |
| **Impeccable** (detector de diseño) | Todo el sitio — 0 hallazgos |
| **Análisis estático propio** | Marcado, enlaces, `alt`, encabezados, JSON-LD, referencias rotas |
| **Verificación HTTP en vivo** | URLs clave, redirecciones, recursos (ver informe de buscadores) |

### Lo que esta auditoría **no** cubre

- **Prueba con lector de pantalla real.** axe detecta cerca del 30–40 % de los problemas de accesibilidad; el resto requiere una persona usando NVDA, JAWS o VoiceOver.
- **Dispositivos reales.** Todo se midió en Chrome sin interfaz, no en un iPhone o un Android de gama baja.
- **Rendimiento con red real.** Las mediciones son en servidor local; con 4G los tiempos serán bastante mayores, sobre todo en la portada por el `hero`.
- **Revisión del contenido clínico.** Nada de lo que dice el sitio sobre servicios, cuotas o credenciales se verificó contra la realidad de la asociación.
