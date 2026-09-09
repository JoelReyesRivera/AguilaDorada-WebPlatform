# Auditoría UX/UI — Página "Nosotros"

**Alcance:** `pages/nosotros.html` y su relación con el landing (`index.html`).
**Fecha:** 2026-09-08 · **Rama:** `pagina-nosotros`
**Método:** revisión contra el sistema de diseño del sitio ("El Santuario Cálido", `DESIGN.md`), `PRODUCT.md`, heurísticas de UX, WCAG 2.1 AA y patrones anti-genéricos ("AI tells").

---

## Veredicto

La página está **bien construida a nivel de sistema** (tipografía, color, componentes y comportamiento son coherentes con el resto del sitio) pero **no está lista para publicarse**: hay contenido marcador visible, una sección a medias y un cruce de navegación con el landing. Ninguno es un problema de código; son decisiones de contenido e IA que hay que cerrar antes de mergear a `main`.

- **Bloqueantes para publicar:** 4
- **Mejoras de peso:** 8
- **Detalles:** 7

---

## Lo que está bien

- **Consistencia con el sistema visual.** Reutiliza tokens, la pareja Cormorant/Montserrat, la paleta cálida, esquinas suaves y la doctrina de elevación (plano en reposo, sombra solo en hover). Se siente parte del sitio, no un injerto.
- **Personas reales, sin relleno.** Nombres, puestos y fotos verdaderos. Nada de "Jane Doe" ni avatares genéricos.
- **Degradación elegante.** Si una foto falta, el `<img>` se autoelimina (`onerror`) y queda el monograma. La revelación de tarjetas está protegida por triple vía: clase `.js`, `prefers-reduced-motion` y un timeout de seguridad a 1.4 s.
- **Accesibilidad de base.** Skip link, `aria-current="page"`, monograma decorativo con `aria-hidden`, foco visible heredado, estructura semántica (`<article>`, jerarquía de encabezados sin saltos), `alt` descriptivo en fotos.
- **Ruta de conversión clara y alineada con `PRODUCT.md`.** Barra de crisis intacta, CTA de WhatsApp por psicóloga, cierre con voluntariado. No se fabrican testimonios ni cifras de impacto.

---

## Bloqueantes para publicar

### B1 · Contenido marcador visible en una página de confianza
La sección "Equipo de psicología" muestra en producción: **"Psicóloga 3", "Psicóloga 4", "Cédula profesional por confirmar"** y la palabra **"Pendiente:"** en rojo. La línea *"En lo personal — Pendiente: un gusto, pasatiempo o algo que la comunidad no sabría de ella."* aparece **9 veces literalmente**.
Para una A.C. de salud mental, cuya página "Nosotros" existe justamente para generar confianza, esto lee como descuido.
**Acción antes de merge:** completar los textos reales, o esconder/eliminar las tarjetas y líneas incompletas. Nunca publicar con "Pendiente:" visible.

### B2 · `pages/nosotros.preview.html` es públicamente accesible y contiene datos ficticios
El archivo de vista previa está dentro de `pages/` (ruta de despliegue), sin `<meta name="robots" content="noindex">`. Contiene pasatiempos inventados de personas reales y 4 psicólogas ficticias. La insignia "Vista previa" no impide que se indexe ni que alguien llegue por URL.
**Acción:** antes de desplegar, una de tres — borrarlo, moverlo fuera de `pages/`, o añadirle `noindex` + `Disallow` en `robots.txt`.

### B3 · "Nosotros" y "Conócenos" — misma intención, destinos distintos
- Nav "Nosotros" (escritorio, móvil, footer) → `pages/nosotros.html` ✅
- Botón "Conócenos" del hero del landing → sigue apuntando a `#mision` (sección Misión del landing).

El visitante que quiere "conocer al equipo" y hace clic en "Conócenos" cae en la sección de misión y no encuentra a las personas.
**Acción:** decidir el rol de cada uno. Opción simple: "Conócenos" → `pages/nosotros.html`. La sección `#mision` del landing se queda como resumen y puede cerrar con un enlace "Conoce al equipo →".

### B4 · La sección de psicología es 2 tarjetas reales + 2 esqueletos
El visitante que llega buscando terapeuta ve 2 personas y 2 huecos. Es peor que mostrar solo a las 2 reales.
**Acción:** mostrar únicamente las psicólogas confirmadas hasta tener el resto. Si el equipo terapéutico son solo Maricela y Dayán por ahora, que la sección tenga 2 tarjetas y ya.

---

## Mejoras de peso

### M1 · Maricela y Dayán aparecen dos veces sin explicación
Salen en "Fundadores" y de nuevo en "Equipo de psicología" con la misma foto. Es correcto (doble rol) pero el usuario ve la misma cara dos veces y parece un bug de duplicado.
**Acción:** una línea introductoria en "Equipo de psicología": *"Nuestras psicólogas. Algunas también forman parte del equipo directivo."* — o un `<p>` bajo el `<h2>` como el que ya existe en `.head-block` del landing.

### M2 · La rejilla deja filas incompletas
`grid-template-columns: repeat(auto-fill, minmax(250px, 1fr))` con 7 tarjetas da 4 + 3 en escritorio ancho (hueco a la derecha) y puede dar 3 + 3 + 1 (una tarjeta sola) en anchos intermedios. La fila coja se lee como descuido.
**Acción:** columnas fijas por breakpoint (p. ej. 4 → 3 → 2 → 1) o centrar la última fila (`justify-content: center` en el grid + `max-width` por tarjeta).

### M3 · Alturas de tarjeta desiguales dentro de una fila
El "aporte" varía de 2 a 4 líneas y "En lo personal" está anclado abajo con `margin-top: auto`, así que su regla superior queda a distinta altura entre tarjetas vecinas. Se nota en la fila de Fundadores.
**Acción:** `-webkit-line-clamp: 3` en `.tm-aporte`, o quitar el `margin-top: auto` de `.tm-personal` y dejar que fluya bajo el aporte.

### M4 · Jerarquía invertida: "En lo personal" pesa más que el "aporte"
"En lo personal" tiene regla divisoria + itálica serif + etiqueta verde en versalitas. El "aporte" (el dato primario, lo que esa persona hace por la asociación) va en gris plano sin separador. El ojo va primero al dato secundario.
**Acción:** dar al "aporte" un poco más de presencia (peso o color de texto principal en la primera frase) y bajar el tratamiento de "En lo personal", o intercambiar el orden visual.

### M5 · Doble CTA de WhatsApp con el mismo mensaje
El header ("Quiero agendar mi terapia") y cada botón "Agendar sesión" abren el **mismo** `wa.me` con el **mismo** texto pre-rellenado. El botón por psicóloga promete agendar "con ella" pero el mensaje no la menciona.
**Acción:** si se quiere agendar con una psicóloga concreta, incluir su nombre en el `text=` del enlace. Si no, cambiar la etiqueta a algo que no prometa personalización ("Escríbenos para agendar").

### M6 · Peso de las fotos
7 JPEG de ~1600×1066 / 1066×1599 (~50 KB c/u) se muestran a 112 px. El navegador descarga el tamaño completo (~350 KB para avatares diminutos). `loading="lazy"` no ayuda con las 3-4 primeras, que están sobre el fold.
**Acción:** servir versiones de ~240×240 (2× del tamaño de pantalla) o usar `srcset`. Recorte cuadrado centrado en la cara de paso.

### M7 · `object-position` calculado a ojo
Cada foto lleva un `object-position` distinto estimado (`center 24%`–`32%`). En Gloria y Maricela la cara queda algo alta/descentrada en el círculo.
**Acción:** recortar las 7 fotos a cuadrado con la cara centrada (herramienta de imagen) y volver a un único `object-position: center`. Elimina el ajuste manual por foto.

### M8 · "Volver al inicio" es un CTA de bajo valor
En la banda de cierre compite visualmente con "Quiero sumarme como voluntario". El logo del header y el footer completo ya llevan al inicio.
**Acción:** dejar solo el CTA de voluntariado, o cambiar el secundario por intención real: "Conoce nuestros servicios" → `../index.html#servicios`.

---

## Detalles

| # | Observación | Acción sugerida |
|---|---|---|
| D1 | Área táctil de "Agendar sesión": **24 px medidos** — cumple WCAG 2.2 AA (mínimo 24 px) pero sin margen, y no llega a AAA (44 px). *(Corregido tras medir: la estimación inicial de "~20 px, por debajo del mínimo" era pesimista.)* | Opcional: subir `padding` vertical del `.tm-cta` para tener holgura. Cualquier ajuste futuro de fuente lo puede tirar por debajo. |
| D2 | Contraste de `.tm-todo` dentro de `.tm-aporte`: **4.46:1 medido** — falla AA por 0.04. Dentro de `.tm-personal` da 9.32:1 y sí pasa (ese contenedor reescribe el color). | Desaparece al quitar los "Pendiente:". Si la etiqueta terracota se reutiliza, subir contraste. |
| D3 | Em-dash (`—`) en `<title>`, `alt` del logo y barra de crisis. Es el patrón del sitio, pero los detectores de diseño lo marcan como "tell". | Coherencia con el sitio > regla. Solo se anota; si algún día se limpia el sitio, hacerlo en todos lados. |
| D4 | `.team-group-head` con `justify-content: space-between` ya no tiene segundo hijo (se quitó el conteo). | CSS muerto inofensivo; se puede simplificar. |
| D5 | Nodos vacíos en el HTML donde estaban los comentarios de plantilla. | Limpieza cosmética. |
| D6 | `.tm-role` (`margin-top: -4px`) y `.tm-cred` (`-3px`) son un hack frágil: se solapan si el nombre y el rol envuelven a 2 líneas. | Controlar el espaciado con el `gap` del flex o márgenes positivos pequeños. |
| D7 | La página no tiene `og:image` propio ni `theme-color`. | Añadir una imagen OG (puede ser el logo sobre fondo crema) para cuando se comparta el enlace. |
| D8 | El enlace "Nosotros" del **footer** no lleva `aria-current="page"` (los dos del nav sí). Un lector de pantalla lo anuncia como enlace a otra página. | Añadir el atributo. |
| D9 | `content-visibility: auto` deja la banda de cierre **en blanco en capturas de página completa** y generación de PDF. No afecta al uso normal. | Saberlo antes de que alguien reporte "la sección se ve vacía". |

> **Verificado con pruebas de UI** (ver [`pruebas-ui-nosotros.md`](pruebas-ui-nosotros.md)): 11 de 13 pruebas pasan. El recorte de texto en móvil que se veía en las capturas **no existe** — cero desbordamiento horizontal en 1440/1024/768/390 px; era un artefacto de Edge headless.

---

## Relación con el landing

- El cambio de navegación está **bien hecho**: los 3 lugares ("Nosotros" en nav de escritorio, menú móvil y footer) apuntan a la página nueva.
- La página **reutiliza header, barra de crisis y footer** literalmente — consistencia total, cero deriva.
- **Falta un puente de vuelta contextual** cerca del inicio de la página (solo está el logo). Un enlace "Inicio / Nosotros" tipo miga de pan ayudaría a ubicarse.
- **Decisión de contenido pendiente:** la sección `#mision` del landing y esta página se solapan en propósito ("quiénes somos"). Definir si `#mision` se queda como resumen que enlaza al equipo completo, o si parte de su contenido se mueve aquí.

---

## Checklist antes de merge a `main`

**Aplicado (2026-09-08), verificado con las pruebas de UI:**

- [x] **B2** · `nosotros.preview.html` con `noindex, nofollow` + `Disallow` en `robots.txt`
- [x] **B3** · "Conócenos" del hero ahora abre la página del equipo; la sección Misión cierra con "Conoce al equipo que lo hace posible"
- [x] **M1** · Línea introductoria en "Equipo de psicología" explicando el doble rol
- [x] **M2** · Rejilla a flex: la última fila queda centrada en vez de dejar hueco
- [x] **M3** · `min-height` de 3 líneas en el aporte: las reglas de "En lo personal" se alinean entre tarjetas vecinas
- [x] **M4** · Jerarquía corregida: el aporte pasa a color de texto principal; la etiqueta "En lo personal" deja el verde (competía con el puesto)
- [x] **M5** · Mensaje de WhatsApp personalizado para Maricela y Dayán
- [x] **M6 + M7** · Fotos recortadas a cuadrado y optimizadas: **369 KB → 58 KB (-84 %)**, recortadas a 400×400 (2.7× el tamaño mostrado, nítidas en retina). Originales en `equipo/originales/`. Se eliminaron los 9 `object-position` ajustados a ojo
- [x] **M8** · "Volver al inicio" → "Conoce nuestros servicios"
- [x] **D1** · Área táctil del CTA: 24 px → **34 px**
- [x] **D2** · Contraste de "Pendiente:": 4.46 → **5.19** (pasa AA)
- [x] **D4 / D5** · CSS muerto y nodos vacíos eliminados
- [x] **D7** · `og:image` y `theme-color` añadidos
- [x] **D8** · `aria-current="page"` en el enlace del footer

**Pendiente — bloqueado por información que la asociación aún no tiene:**

- [ ] **B1** · Quitar toda ocurrencia visible de "Pendiente:" y "por confirmar"
- [ ] **B4** · Sección de psicología: decidir si son solo Maricela y Dayán o hay más
- [ ] Completar los 7 textos de "En lo personal" y revisar los borradores de "aporte"
- [ ] Cédulas profesionales de Maricela y Dayán

**Descartado:**

- **D3** (em-dash) · Es el patrón del sitio entero; cambiarlo aquí solo crearía inconsistencia
- **D6** (márgenes negativos) · Revisado: con `gap: 13px` y márgenes de -4/-3 px el espacio efectivo es de 9-10 px, no puede haber solape. La observación era infundada
- **D9** (`content-visibility`) · Solo afecta a capturas de página completa, no al uso real
