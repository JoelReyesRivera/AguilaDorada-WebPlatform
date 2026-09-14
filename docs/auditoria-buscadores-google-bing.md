# Auditoría de configuración para buscadores (Google y Bing)

**Sitio:** https://aguiladorada.org · **Última verificación:** 14 de septiembre de 2026 (auditoría original: 11 de septiembre)
**Alcance:** las 7 páginas del sitio + `robots.txt`, `sitemap.xml`, verificaciones, IndexNow y datos estructurados.
**Método:** inspección estática de los archivos, verificación HTTP en vivo de cada URL y validación de los bloques JSON-LD. Este documento se actualiza en el sitio conforme se resuelven hallazgos, no se archiva una copia nueva por cada pasada.

---

## Resumen ejecutivo

La base técnica del sitio es **buena y mejoró de forma sostenida** desde la primera pasada: HTTPS correcto con redirección 301, `robots.txt` bien formado, un `<h1>` único por página, jerarquía de encabezados sin saltos, enlazado interno completo, y datos estructurados válidos en las 7 páginas (incluida `nosotros.html`, que no los tenía al principio). La mayoría de los hallazgos originales de esta auditoría ya están corregidos — ver §1.

**Lo que sigue abierto de verdad:**
1. **Google Search Console no está verificado.** Bing sí lo está. No hay forma de ver qué indexa Google, qué errores encuentra ni qué búsquedas traen visitas.
2. **El JSON-LD de la portada no declara `sameAs`** (perfiles de Instagram/Facebook), `telephone` ni `email` — Google no puede enlazar el sitio con sus perfiles sociales para el panel de conocimiento.
3. `lastmod` del sitemap y sitemap de imágenes (menores).

**Diagnóstico general: base sólida, con dos pendientes de configuración de cuenta y datos, ninguno de código complejo.**

---

## 1. Estado por buscador

| Elemento | Google | Bing |
|---|---|---|
| Verificación de propiedad | ❌ **No existe** — dar de alta el sitio en Search Console | ✅ `msvalidate.01` en `index.html` |
| Sitemap declarado en `robots.txt` | ✅ | ✅ |
| Rastreo permitido | ✅ `Allow: /` | ✅ |
| IndexNow (avisos instantáneos) | n/a (Google no lo usa) | ✅ Clave válida; workflow envía las 6 URLs públicas en cada push a `main` (antes solo enviaba la portada) |
| Datos estructurados | ✅ 7/7 páginas | ✅ igual |
| Vista previa al compartir (`og:image`) | ✅ `assets/og-image.jpg` existe y se sirve | ✅ igual |
| Canónicas coherentes con el sitemap | ✅ Las 7 usan `/pages/*.html`, consistente | ✅ igual |

**Nota sobre `pages/privacidad.html`:** se excluye a propósito del `sitemap.xml` y del `urlList` de IndexNow (ambos con comentario explicando por qué) porque es un borrador aún sin revisión legal. Sigue siendo públicamente accesible por URL directa — ver `auditoria-sitio-completa.md` §5 para el pendiente de `noindex`.

---

## 2. Resuelto desde la auditoría original (11 sep)

| # | Hallazgo original | Estado |
|---|---|---|
| C1 | Canónica de `nosotros.html` apuntaba a `/nosotros` (404) | ✅ Corregida a `/pages/nosotros.html`, consistente con las otras 6 |
| C2 | `assets/og-image.jpg` no existía (404 al compartir) | ✅ Existe (33KB) |
| A1 | `nosotros.html` sin datos estructurados | ✅ Se agregó `AboutPage` con `Person`/`EducationalOccupationalCredential` por cada integrante, tal como se propuso |
| A2 | IndexNow solo avisaba de la portada | ✅ El workflow envía ahora las 6 URLs públicas |
| A3 | 4 meta descripciones se cortaban en resultados (hasta 264 caracteres) | ✅ Las 7 están ahora entre 132 y 164 caracteres |
| A5 | Faltaban `og:site_name`, `og:locale` y tarjetas de Twitter/X | ✅ Presentes en las 7 páginas |
| M2 | Sin `BreadcrumbList` | ✅ En las 6 páginas internas |
| M3 | Sin página 404 propia (usaba la genérica de GitHub Pages) | ✅ `404.html` con la identidad del sitio |
| M4 | `logo.png` (108KB) y `hero.jpg` (201KB) pesados | ✅ Comprimidos a 35KB y 179KB |
| M5 | `theme-color` faltaba en la portada | ✅ Presente |

---

## 3. Abierto

### 🔴 Google Search Console sin verificar

No existe ninguna etiqueta `google-site-verification` en el sitio. Sin Search Console no hay manera de saber si Google indexó las páginas, si encontró errores de rastreo, qué consultas traen visitas ni si los datos estructurados se leen bien.

**Corrección:** dar de alta la propiedad en https://search.google.com/search-console, verificar por etiqueta HTML y pegarla en el `<head>` de `index.html`, junto a la de Bing:
```html
<meta name="msvalidate.01" content="033CC8452195BD42DEB21FF409A09D6E" />
<meta name="google-site-verification" content="PEGAR_AQUI" />
```
Después, enviar `https://aguiladorada.org/sitemap.xml` desde el panel.

### 🟠 El JSON-LD de la portada no incluye redes ni contacto

El bloque `NGO`+`MedicalBusiness` de `index.html` trae nombre, dirección, geolocalización y especialidad, pero le falta `sameAs` (perfiles de Instagram y Facebook), `telephone` y `email`.

```json
  "sameAs": [
    "https://www.instagram.com/aguiladoradaac",
    "https://www.facebook.com/share/1TCRMLJVQr/"
  ],
  "telephone": "+52-667-211-5886",
  "email": "aguiladorada.admi@gmail.com"
```
⚠️ Verificar antes el enlace de Facebook: `facebook.com/share/1TCRMLJVQr/` es un acortador opaco. Para `sameAs` conviene la URL real del perfil (`facebook.com/nombredelapagina`). No cambiar sin confirmar — los datos de contacto y redes son fuente de verdad según `PRODUCT.md`.

### 🟡 Menores

| # | Hallazgo | Detalle |
|---|---|---|
| M1 | `lastmod` del sitemap puede quedar desactualizado | Dice `2026-09-11`; hay páginas editadas después (p. ej. `index.html` el 14 de septiembre). Actualizar al publicar cambios de contenido, o generarlo en el workflow. |
| M6 | Sin sitemap de imágenes | Las fotos del equipo y los flyers de eventos no se declaran. Opcional; ayuda a aparecer en Google Imágenes con búsquedas locales. |

---

## 4. Oportunidades de palabras clave

> Sin una herramienta de SEO conectada (Ahrefs, Semrush) no hay volúmenes reales. Las estimaciones de abajo se basan en el análisis del mercado local y en los competidores identificados.

**Panorama competitivo en Culiacán:** los términos genéricos ("psicólogos en Culiacán") están dominados por directorios —[Psychology Today](https://www.psychologytoday.com/mx/psicologos/si/culiacan), [psico.org](https://www.psico.org/mx/culiacan), [psico.mx](https://www.psico.mx/psicologos/psicoterapia/culiacan-sinaloa)— y por consultorios privados como [SanaMente](https://www.sanamente-espacioterapeutico.com/), [Psicología Clínica América](https://www.psicologiaclinicaamerica.com/) y [psicologiaculiacan.com](https://psicologiaculiacan.com/).

**La conclusión estratégica es clara:** no conviene pelear de frente por "psicólogo en Culiacán". Águila Dorada tiene un terreno que ningún consultorio privado puede disputarle — **precio accesible, figura de A.C., cuota ajustada por estudio socioeconómico y programas comunitarios**. Ahí la competencia es mínima y la intención de búsqueda es altísima.

| Palabra clave | Dificultad | Oportunidad | Intención | Dónde trabajarla |
|---|---|---|---|---|
| terapia psicológica económica Culiacán | Baja | **Alta** | Transaccional | `servicios.html` — ya es el tema de la página |
| psicólogo barato Culiacán | Baja | **Alta** | Transaccional | `servicios.html` |
| terapia psicológica gratis Culiacán | Baja | **Alta** | Transaccional | `servicios.html` — aclarar cuota mínima |
| apoyo psicológico bajo costo Sinaloa | Baja | **Alta** | Transaccional | `servicios.html` |
| asociación civil salud mental Culiacán | Muy baja | **Alta** | Comercial | `index.html` + `nosotros.html` |
| estudio socioeconómico terapia Culiacán | Muy baja | **Alta** | Transaccional | `servicios.html` — diferenciador único |
| psicólogo para estudiantes Culiacán | Baja | **Alta** | Transaccional | Contenido nuevo (la competencia ya ofrece descuento) |
| voluntariado salud mental Culiacán | Muy baja | **Alta** | Transaccional | `voluntariado.html` — ya existe |
| terapia de pareja Culiacán | Media | Media | Transaccional | `servicios.html` |
| terapia familiar Culiacán | Media | Media | Transaccional | `servicios.html` |
| diagnóstico psicológico Culiacán | Baja | Media | Transaccional | `servicios.html` |
| talleres de salud emocional Culiacán | Baja | Media | Informacional | `eventos-pasados.html` + página de próximos |
| psicólogo en línea Sinaloa | Media | Media | Transaccional | `servicios.html` — ya se ofrece modalidad en línea |
| dónde pedir ayuda psicológica en Culiacán | Baja | Media | Informacional | Guía nueva |
| cuánto cuesta una terapia psicológica en Culiacán | Baja | **Alta** | Informacional | Guía nueva — atrae y precalifica |
| cómo saber si necesito ir al psicólogo | Media | Media | Informacional | Guía nueva |
| ansiedad tratamiento Culiacán | Media | Media | Transaccional | Página por padecimiento |
| depresión ayuda Culiacán | Media | Media | Transaccional | Página por padecimiento |
| psicólogos Culiacán | **Alta** | Baja | Transaccional | Dominada por directorios; no priorizar |
| donar a asociación civil Culiacán | Baja | Media | Transaccional | Sección de donativos |

**Hueco de contenido más grande:** el sitio no tiene **ninguna página informativa**. Todo es institucional (quiénes somos, qué ofrecemos, cómo contactar). Las búsquedas del tipo "cuánto cuesta una terapia" o "cómo saber si necesito ir al psicólogo" son las que traen gente que todavía no decide, y son exactamente las personas a las que esta asociación quiere llegar. Una sola guía honesta sobre costos y sobre cómo funciona el estudio socioeconómico podría rendir más que todo lo demás.

⚠️ Cualquier contenido nuevo debe respetar `PRODUCT.md`: **nada de testimonios, cifras de impacto ni casos de éxito inventados.**

---

## 5. Plan de acción

### Esta semana

| # | Acción | Impacto | Esfuerzo | Depende de |
|---|---|---|---|---|
| 1 | Verificar el sitio en Google Search Console y enviar el sitemap | **Alto** | 15 min | Acceso a la cuenta de Google |
| 2 | Agregar `sameAs`, `telephone` y `email` al JSON-LD de la portada | Medio | 10 min | Confirmar URL real de Facebook (no el acortador `share/`) |
| 3 | Actualizar `lastmod` del sitemap al publicar cambios | Bajo | 2 min | — |

### Este trimestre

| # | Acción | Impacto | Esfuerzo |
|---|---|---|---|
| 4 | **Guía "Cuánto cuesta una terapia psicológica en Culiacán"** explicando el estudio socioeconómico. Es el diferenciador de la asociación y nadie más lo cubre. | **Alto** | Medio |
| 5 | **Reclamar y optimizar la ficha de Google Business.** Para "psicólogo cerca de mí" el paquete local pesa más que el sitio. Los horarios del sitio ya se tomaron de ahí, así que la ficha existe. | **Alto** | Medio |
| 6 | Página de "Próximos eventos" propia (hoy es un ancla en la portada) con schema `Event`, para optar a los resultados enriquecidos de eventos | Medio | Medio |
| 7 | Páginas por padecimiento (ansiedad, depresión, duelo) con criterio clínico y revisión de la Lic. Cázarez | Medio | Alto |
| 8 | Migrar a URLs limpias (`servicios/index.html` → `/servicios/`) con redirecciones. Mejora legibilidad y tasa de clic, pero hay que hacerlo de una vez y con cuidado. | Medio | Alto |
| 9 | Conseguir enlaces entrantes: directorios de OSC, Servicios de Salud de Sinaloa, universidades con las que se colabore | **Alto** | Alto |

---

## 6. Cómo comprobar que quedó bien

1. **Google Search Console** → *Inspección de URLs*: probar `https://aguiladorada.org/pages/nosotros.html` y confirmar que la canónica declarada y la seleccionada por Google coinciden.
2. **[Prueba de resultados enriquecidos](https://search.google.com/test/rich-results)**: pasar las 7 URLs y verificar que cada bloque JSON-LD se lea sin errores.
3. **[Depurador de Open Graph de Facebook](https://developers.facebook.com/tools/debug/)**: pegar la portada y forzar el rescrapeo.
4. **[Bing Webmaster Tools](https://www.bing.com/webmasters)** → *Sitemaps* y *IndexNow*: confirmar que el sitemap se leyó y que los avisos llegan con las URLs correctas.
5. **[PageSpeed Insights](https://pagespeed.web.dev/)**: medir portada y servicios en móvil.

---

## Fuentes

- [Psicólogos en Culiacán — Psychology Today](https://www.psychologytoday.com/mx/psicologos/si/culiacan)
- [Centros de Psicología en Culiacán — psico.org](https://www.psico.org/mx/culiacan)
- [Psicoterapeutas en Culiacán — psico.mx](https://www.psico.mx/psicologos/psicoterapia/culiacan-sinaloa)
- [Programa Salud Mental — Servicios de Salud de Sinaloa](https://saludsinaloa.gob.mx/index.php/salud-mental/)
- [SanaMente Espacio Terapéutico](https://www.sanamente-espacioterapeutico.com/)
- [Psicología Clínica América](https://www.psicologiaclinicaamerica.com/)
- [Psicólogo en Culiacán](https://psicologiaculiacan.com/)
