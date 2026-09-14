# Auditoría del sitio — accesibilidad, diseño y rendimiento

**Sitio:** aguiladorada.org · **Última verificación:** 14 de septiembre de 2026 (tarde)
**Alcance:** las 7 páginas (`index`, `servicios`, `nosotros`, `contacto`, `eventos-pasados`, `voluntariado`, `privacidad`).
**Método:** axe-core sobre cada página, navegador real (`agent-browser`) para desbordamiento/tamaño de objetivos táctiles/foco/rendimiento/navegación por teclado, y revisión estática del marcado. Este documento se actualiza en el sitio, no se archiva una copia nueva por cada pasada — la fecha de arriba es la de la última verificación.

> El apartado de buscadores va aparte, en [`auditoria-buscadores-google-bing.md`](auditoria-buscadores-google-bing.md).

---

## Resumen ejecutivo

El sitio está **notablemente bien construido y bien mantenido**: cero desbordamientos horizontales, foco de teclado visible en las 7 páginas, jerarquía de encabezados sin saltos, enlazado interno completo, cero scripts externos. Las rondas de auditoría de accesibilidad y contraste de los días 11 y 14 de septiembre ya se cerraron — lo que sigue abierto son mejoras de fondo (texto muy pequeño, pruebas manuales) y un pendiente operativo (el aviso de privacidad).

**Diagnóstico general: base sólida.** Lo que falta es acabado fino y mantenimiento, no reconstrucción.

---

## 1. Accesibilidad

### Resuelto y verificado en código + sitio en vivo

| Hallazgo | Corrección |
|---|---|
| Contraste `.kicker` sobre fondo alterno (4.31:1) | `--green-deep` → `#4A6B52` (5.04:1) |
| Contraste de `.limited` ("Cupo limitado") (4.45:1) | `--terracotta-deep` → `#A85234` (5.21:1) |
| `.crisis-bar` fuera de las regiones de referencia (`header`/`main`/`footer`) | `<aside aria-label="Atención en crisis">` en las 7 páginas |
| 7 enlaces "postularme en esta área" idénticos para lector de pantalla (`voluntariado.html`) | `aria-label` distinto por área (marketing, eventos, ventas, difusión, talleres, cultural, víveres) |
| Botón "Quiero agendar mi terapia" del header duplicado para lector de pantalla (patrón `.btn-full`/`.btn-short`) | `aria-label` único en el `<a>` + `aria-hidden` en ambos `<span>` |
| `.contact-aside` era un `<aside>` anidado dentro de `<main>` | Verificar en la próxima pasada si sigue así — no confirmado en esta ronda |
| **Menú móvil no se cerraba con Escape** (las 7 páginas) | `keydown` con `Escape` cierra el menú y devuelve el foco al botón — probado con navegador real |
| **Texto casi invisible en los datos de transferencia bancaria** (`index.html`): "Titular", "Banco", "CLABE", "Concepto" en crema sobre fondo casi igual | Faltaba el `<div class="bank-details">` que el CSS ya esperaba (fondo, padding, color dorado de las etiquetas); se agregó envolviendo los 4 campos |
| **Botón "Enviar mensaje" de la barra de crisis abría el WhatsApp institucional, no la Línea de la Vida** — quedaba justo al lado del texto de crisis, así que parecía la forma de escribirle a la línea de emergencia | Se quitó el botón en las 7 páginas; la barra de crisis solo ofrece "Llamar ahora" (`tel:8009112000`), que sí es la Línea de la Vida real |

### Descartado — falsos positivos de axe-core (verificados, no corregidos)

axe-core marcó contraste bajo en tres sitios donde el color real, comprobado con `getComputedStyle` y capturas de pantalla, es correcto: "CLABE:"/"Concepto:" en la tarjeta de donativos (2.26:1 reportado; el dorado se ve claramente legible sobre la tarjeta café oscuro), y el `<h1>`/`.kicker` de `eventos-pasados.html` (hasta 1.7:1 reportado; el `<h1>` mide `rgb(43,27,20)` a opacidad 1, el café correcto). Es una limitación conocida de axe-core: cuando hay una capa semitransparente sobre otro fondo (aquí, `.bank-details` con `rgba(250,246,239,.08)`), en vez de mezclarla con el fondo real detrás la mezcla contra blanco. No se tocó nada — cambiar esos colores a ciegas habría dañado un diseño que ya se ve bien. Si se vuelve a correr axe-core sobre estas zonas, esperar estos falsos positivos y verificar visualmente antes de "corregir".

### Aún pendiente

- **Prueba manual con NVDA/VoiceOver** del formulario de contacto (lectura de errores, etiquetas de los campos) y navegación completa por teclado del menú móvil. axe-core detecta ~30-40% de los problemas de accesibilidad; el resto requiere una persona con lector de pantalla real.
- **Texto por debajo de 12px**: `.ph-aside-label`, `.ca-label`, `.vol-chip`, `.ig-meta`, `.limited`, `.event-tag`, `.tm-cred` van de 11 a 12px. No incumple ningún criterio WCAG (son etiquetas/metadatos, no texto de lectura), pero a 11px la legibilidad en móvil se resiente.

### Sobre los objetivos táctiles

Varios enlaces del pie y de navegación miden menos de 24px de alto, pero pasan por la **excepción de espaciado** de WCAG 2.2 AA 2.5.8 (distancia entre centros > 24px) o son enlaces dentro de párrafo (excepción de texto en línea). Las dos casillas de radio de `contacto.html` (13×13px) están dentro de `<label>` clicables de 155×42 y 122×42px — el objetivo real es la etiqueta completa. Se anota explícitamente porque es el tipo de hallazgo que un informe automático marca como falla sin serlo.

---

## 2. Diseño responsivo

Medido a 1440px y 390px en las 7 páginas: cero desbordamiento horizontal, cero elementos fuera del ancho (el `skip-link` en −9999px es intencional), foco de teclado visible (`outline: 3px solid`) en las 7.

---

## 3. Rendimiento

- **Cero scripts externos** en las 7 páginas; todo el JS es interno y suma menos de 5KB por página.
- Una sola hoja de estilos compartida (caché entre páginas).
- `preconnect` a Google Fonts, `display=swap`, `preload` del hero con `fetchpriority="high"`.
- `logo.png`: comprimido de 108KB a 35KB. `hero.jpg`: comprimido de 201KB a 179KB.
- `brand-mark` con `width`/`height` declarados en las 7 páginas (sin salto de maquetación al cargar).
- Los incrustados de Instagram de `eventos-pasados.html` (9 publicaciones) siguen siendo lo más lento del sitio: 956ms–3.3s cada uno, cargando por proximidad en el scroll (`IntersectionObserver`, `rootMargin: 500px`). Se evaluó cambiar a carga solo al hacer clic, pero el equipo prefiere que las 9 publicaciones estén visibles sin necesitar un toque — queda como pendiente de fondo, no como corrección: la única forma de tener ambas cosas (visibles + rápidas) sería una miniatura propia por publicación en vez de depender del incrustado de Instagram, y eso requiere conseguir esas imágenes.

---

## 4. Calidad del marcado

Un `<h1>` por página (7/7), sin saltos de nivel en encabezados, sin imágenes de contenido sin `alt`, sin enlaces internos rotos, sin páginas huérfanas, sin texto de ancla genérico, `rel="noopener"` en los 76 enlaces externos, `lang` declarado en las 7. La jerarquía de encabezados sin un solo salto en 7 páginas es un resultado poco común — vale la pena conservarlo al agregar contenido nuevo.

---

## 5. Pendiente operativo (no es un defecto de código)

**El aviso de privacidad está desplegado y público**, no solo en el repositorio: `pages/privacidad.html` carga en `https://aguiladorada.org/pages/privacidad.html`. Tiene `<meta name="robots" content="index, follow">` (indexable si algo lo enlaza) y publica en su JSON-LD el domicilio y correo de la asociación, pese a ser un borrador que aún no revisa un abogado. Mitigación ya aplicada: no está enlazado desde ningún menú ni pie de página activo, y está excluido a propósito del `sitemap.xml` y del `urlList` de IndexNow (ambos con comentarios explicando por qué). Pendiente de decidir: agregar `<meta name="robots" content="noindex, nofollow">` mientras no esté aprobado legalmente, o no desplegar el archivo en absoluto hasta entonces.

---

## 6. Plan de acción

| # | Acción | Prioridad | Esfuerzo |
|---|---|---|---|
| 1 | `noindex` en `pages/privacidad.html` mientras no la revise el abogado | 🔴 Alta | Bajo |
| 2 | Prueba manual con NVDA/VoiceOver del formulario de contacto y navegación por teclado del menú móvil | 🟡 Media | Media |
| 3 | Subir textos de 11px a 12px | 🟡 Media | Bajo |
| 4 | Confirmar si `.contact-aside` sigue siendo un `<aside>` anidado | 🟢 Baja | Bajo |
| 5 | Miniatura propia + clic para cargar Instagram en eventos pasados (si se consiguen las imágenes) | 🟢 Baja | Alto |

---

## Lo que esta auditoría no cubre

- **Prueba con lector de pantalla real** (ver §1).
- **Dispositivos reales** — todo se mide con Chrome sin interfaz, no en un iPhone o Android de gama baja.
- **Rendimiento con red real** — las mediciones son en servidor local; con 4G los tiempos serán mayores, sobre todo en la portada por el hero y en `eventos-pasados.html` por Instagram.
- **Revisión del contenido clínico** — nada de lo que dice el sitio sobre servicios, cuotas o credenciales se verifica contra la realidad de la asociación en esta auditoría.
