# Auditoría de configuración para buscadores (Google y Bing)

**Sitio:** https://aguiladorada.org · **Fecha:** 11 de septiembre de 2026
**Alcance:** las 7 páginas del sitio + `robots.txt`, `sitemap.xml`, verificaciones, IndexNow y datos estructurados.
**Método:** inspección estática de los archivos, verificación HTTP en vivo de cada URL y validación de los bloques JSON-LD.

---

## Resumen ejecutivo

La base técnica del sitio es **buena**: HTTPS correcto con redirección 301 de `http://` y de `www.` hacia el dominio canónico, `robots.txt` bien formado, un `<h1>` único por página, jerarquía de encabezados sin saltos en las 7 páginas, enlazado interno completo sin páginas huérfanas y datos estructurados en 6 de 7 páginas. La carga es rápida y el `hero` ya tiene `preload` con `fetchpriority="high"`.

Hay, sin embargo, **tres problemas que hay que corregir antes de publicar**, porque anulan trabajo ya hecho:

1. **La etiqueta canónica de `nosotros.html` apunta a una URL que no existe.** Es el hallazgo más grave: le dice a Google "la versión buena de esta página está en otro lado" y ese otro lado da 404.
2. **La imagen de vista previa social (`og-image.jpg`) no existe.** Las 7 páginas la declaran y el archivo nunca se subió. Cada vez que alguien comparte el sitio por WhatsApp —el canal principal de la asociación— aparece sin imagen.
3. **Google Search Console no está verificado.** Bing sí lo está. Ahora mismo no hay forma de ver qué indexa Google, qué errores encuentra ni qué búsquedas traen visitas.

**Diagnóstico general: base sólida, con fallas puntuales de configuración que hay que cerrar.** Ninguna es de fondo; todas son de una o dos líneas salvo la imagen social.

---

## 1. Estado por buscador

| Elemento | Google | Bing | Notas |
|---|---|---|---|
| Verificación de propiedad | ❌ **No existe** | ✅ `msvalidate.01` en `index.html` | Falta dar de alta el sitio en Search Console |
| Sitemap declarado en `robots.txt` | ✅ | ✅ | `https://aguiladorada.org/sitemap.xml` |
| Sitemap enviado en el panel | ⚠️ No verificable sin acceso | ⚠️ No verificable sin acceso | Hay que confirmarlo manualmente en cada panel |
| Rastreo permitido | ✅ `Allow: /` | ✅ | Sin bloqueos accidentales |
| IndexNow (avisos instantáneos) | n/a (Google no lo usa) | ✅ Clave válida y publicada | Pero solo avisa de la portada — ver §4 |
| Datos estructurados | ⚠️ 6 de 7 páginas | ⚠️ igual | Falta `nosotros.html` |
| Vista previa al compartir | ❌ imagen 404 | ❌ imagen 404 | Afecta también a WhatsApp y Facebook |

**Verificación HTTP en vivo realizada el 11/09/2026:**

```
200  https://aguiladorada.org/
200  https://aguiladorada.org/sitemap.xml
200  https://aguiladorada.org/robots.txt
200  https://aguiladorada.org/cf3968cf08a949beb976d7c0f429976f.txt   (clave IndexNow)
404  https://aguiladorada.org/assets/og-image.jpg                    ← problema
301  http://aguiladorada.org/      → https://aguiladorada.org/       ✅
301  https://www.aguiladorada.org/ → https://aguiladorada.org/       ✅
```

El `sitemap.xml` **publicado** todavía solo contiene la portada; el `sitemap.xml` **local** ya trae las 7 URLs. Esto es correcto: el sitemap local se publicará junto con las páginas. Lo importante es que no se suba antes que ellas, porque entonces estaríamos anunciándole a Google seis URLs que dan 404.

---

## 2. Problemas por severidad

### 🔴 Críticos

#### C1 — Canónica rota en `nosotros.html`

```html
<!-- pages/nosotros.html, actual -->
<link rel="canonical" href="https://aguiladorada.org/nosotros">
<meta property="og:url" content="https://aguiladorada.org/nosotros">
```

El sitio se sirve con GitHub Pages, que publica cada archivo en su propia ruta. El archivo vive en `pages/nosotros.html`, así que la URL real es `https://aguiladorada.org/pages/nosotros.html`. La dirección `/nosotros`, sin extensión, **no existe** — GitHub Pages solo genera URLs "limpias" para archivos llamados `index.html` dentro de una carpeta.

Las otras seis páginas sí usan la forma correcta, y el `sitemap.xml` también. Esta es la única inconsistente.

**Por qué importa:** una canónica es una instrucción, no una sugerencia. Le estamos diciendo a Google y a Bing que la versión oficial de la página del equipo está en una dirección que devuelve 404. El resultado típico es que la página **no se indexe en absoluto**.

**Corrección:**
```html
<link rel="canonical" href="https://aguiladorada.org/pages/nosotros.html">
<meta property="og:url" content="https://aguiladorada.org/pages/nosotros.html">
```

*(Alternativa de fondo, opcional y más trabajosa: mover cada página a `nombre/index.html` para tener URLs limpias en todo el sitio. Se describe en §6.)*

#### C2 — La imagen de vista previa social no existe

Las 7 páginas declaran:
```html
<meta property="og:image" content="https://aguiladorada.org/assets/og-image.jpg">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
```

El archivo `assets/og-image.jpg` **no está en el repositorio** y devuelve 404 en vivo. La carpeta `assets/` solo contiene `icons/` (vacía) e `images/`.

**Por qué importa:** cuando alguien comparte un enlace de Águila Dorada por WhatsApp, Facebook o Messenger, en lugar de una tarjeta con imagen aparece un enlace gris y pelón. Para una asociación cuya difusión ocurre sobre todo en WhatsApp e Instagram, esto le resta mucho a cada enlace que comparte el equipo.

**Corrección:** crear `assets/og-image.jpg` a 1200×630 px, menos de 300 KB, con el logo, el nombre y la frase de posicionamiento ("Salud mental accesible en Culiacán"). Con eso las 7 páginas quedan resueltas de golpe, porque todas apuntan al mismo archivo.

#### C3 — Google Search Console sin verificar

No existe ninguna etiqueta `google-site-verification` ni archivo `google*.html` en el sitio. Bing sí quedó verificado en su momento.

**Por qué importa:** sin Search Console no hay manera de saber si Google indexó las páginas nuevas, si encontró errores de rastreo, qué consultas traen visitas ni si los datos estructurados se están leyendo bien. Es la herramienta de diagnóstico principal y es gratuita.

**Corrección:** dar de alta la propiedad en https://search.google.com/search-console, elegir verificación por etiqueta HTML y pegarla en el `<head>` de `index.html`, junto a la de Bing:
```html
<meta name="msvalidate.01" content="033CC8452195BD42DEB21FF409A09D6E" />
<meta name="google-site-verification" content="PEGAR_AQUI" />
```
Después, enviar `https://aguiladorada.org/sitemap.xml` desde el panel.

---

### 🟠 Altos

#### A1 — `nosotros.html` es la única página sin datos estructurados

Las otras seis tienen JSON-LD (`NGO`+`MedicalBusiness`, `ContactPage`, `ItemList` de eventos, `MedicalBusiness` con catálogo de servicios, `WebPage`). La del equipo no tiene ninguno.

Es justo la página donde más pesan: el sitio es de **salud mental**, una categoría que Google clasifica como YMYL ("tu dinero o tu vida") y evalúa con criterios de experiencia y confiabilidad más estrictos. Declarar en formato legible por máquina quiénes son las personas, qué cédula profesional tienen y qué rol cumplen es una señal de confianza directa.

**Corrección:** agregar un bloque `AboutPage` con la lista de personas. Esquema sugerido (solo con datos que ya están publicados en la página):

```json
{
  "@context": "https://schema.org",
  "@type": "AboutPage",
  "name": "Nosotros — Águila Dorada A.C.",
  "url": "https://aguiladorada.org/pages/nosotros.html",
  "inLanguage": "es",
  "about": {
    "@type": "NGO",
    "name": "Águila Dorada A.C.",
    "url": "https://aguiladorada.org",
    "employee": [
      {
        "@type": "Person",
        "name": "Maricela Cázarez Zamora",
        "honorificPrefix": "Lic.",
        "jobTitle": "Dirección y Depto. Educativo",
        "hasCredential": {
          "@type": "EducationalOccupationalCredential",
          "credentialCategory": "Cédula profesional",
          "identifier": "8820269"
        }
      }
    ]
  }
}
```
*(Completar el arreglo `employee` con el resto del equipo, respetando los nombres y puestos exactos que ya están en la página.)*

#### A2 — IndexNow solo avisa de la portada

`.github/workflows/indexnow.yml` hace:
```
curl "https://api.indexnow.org/indexnow?url=https://aguiladorada.org/&key=..."
```

Una sola URL: la portada. Cuando se actualicen servicios, eventos o el equipo, **Bing nunca se entera** de esas páginas. El mecanismo está bien montado pero desaprovechado.

**Corrección:** cambiar a un envío por lote con las 7 URLs (IndexNow acepta hasta 10 000 por petición):

```yaml
      - name: Send IndexNow Ping
        run: |
          curl -s -X POST "https://api.indexnow.org/indexnow" \
            -H "Content-Type: application/json" \
            -d '{
              "host": "aguiladorada.org",
              "key": "cf3968cf08a949beb976d7c0f429976f",
              "keyLocation": "https://aguiladorada.org/cf3968cf08a949beb976d7c0f429976f.txt",
              "urlList": [
                "https://aguiladorada.org/",
                "https://aguiladorada.org/pages/servicios.html",
                "https://aguiladorada.org/pages/nosotros.html",
                "https://aguiladorada.org/pages/contacto.html",
                "https://aguiladorada.org/pages/eventos-pasados.html",
                "https://aguiladorada.org/pages/voluntariado.html",
                "https://aguiladorada.org/pages/privacidad.html"
              ]
            }'
```

#### A3 — Cuatro descripciones se cortan en los resultados

Google muestra alrededor de 155–160 caracteres. Lo que sobra se reemplaza por "…".

| Página | Largo | Estado |
|---|---:|---|
| `servicios.html` | 264 | 🔴 se corta ~100 caracteres |
| `voluntariado.html` | 254 | 🔴 se corta ~95 caracteres |
| `privacidad.html` | 209 | 🟠 se corta ~50 |
| `contacto.html` | 190 | 🟠 se corta ~30 |
| `index.html` | 159 | ✅ |
| `nosotros.html` | 154 | ✅ |
| `eventos-pasados.html` | 145 | ✅ |

En `servicios.html` lo que queda fuera es justo "cuota ajustada a tu situación mediante estudio socioeconómico", que es el argumento más fuerte de la asociación. Propuestas en §5.

#### A4 — El JSON-LD de la portada no incluye redes ni contacto

El bloque `NGO`+`MedicalBusiness` de `index.html` trae nombre, dirección, geolocalización y especialidad, pero **le falta `sameAs`** (los perfiles de Instagram y Facebook), `telephone`, `email` y horarios.

`sameAs` es la señal con la que Google enlaza el sitio con sus perfiles sociales para armar el panel de conocimiento y la ficha de Google Business. Los perfiles existen y ya están enlazados en el pie de página; solo falta declararlos.

**Corrección** — agregar al bloque existente:
```json
  "sameAs": [
    "https://www.instagram.com/aguiladoradaac",
    "https://www.facebook.com/share/1TCRMLJVQr/"
  ],
  "telephone": "+52-667-211-5886",
  "email": "aguiladorada.admi@gmail.com"
```
⚠️ Verificar antes el enlace de Facebook: `facebook.com/share/1TCRMLJVQr/` es un acortador opaco. Para `sameAs` conviene la URL real del perfil (`facebook.com/nombredelapagina`). No lo cambié porque los datos de contacto y redes son fuente de verdad según `PRODUCT.md`.

#### A5 — Faltan `og:site_name`, `og:locale` y las tarjetas de Twitter/X

Ninguna de las 7 páginas los declara. Sin `og:locale` algunas plataformas asumen inglés; sin `twitter:card` la vista previa en X queda como enlace simple.

**Corrección** (añadir a las 7, junto a las `og:` que ya existen):
```html
<meta property="og:site_name" content="Águila Dorada A.C.">
<meta property="og:locale" content="es_MX">
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="…mismo texto que og:title…">
<meta name="twitter:description" content="…mismo texto que og:description…">
<meta name="twitter:image" content="https://aguiladorada.org/assets/og-image.jpg">
```

---

### 🟡 Medios

| # | Hallazgo | Detalle y corrección |
|---|---|---|
| M1 | **`lastmod` desactualizado** | El sitemap dice `2026-09-10`; los archivos se editaron el `2026-09-11`. Hay que actualizarlo al publicar, o generarlo automáticamente en el workflow. |
| M2 | **Sin `BreadcrumbList`** | Con URLs del tipo `/pages/servicios.html`, Google muestra la ruta cruda. Un `BreadcrumbList` por página interna hace que aparezca `Inicio › Servicios`, más legible y con mejor tasa de clic. |
| M3 | **Sin página 404 propia** | `https://aguiladorada.org/loquesea` devuelve el 404 genérico de GitHub Pages, sin menú ni identidad. Un `404.html` en la raíz con el encabezado del sitio y enlaces a las secciones recupera a quien llega por un enlace roto. |
| M4 | **`logo.png` pesa 108 KB** | Se usa como favicon y como marca en encabezado y pie, siempre a tamaño pequeño. Comprimirlo o servir una versión reducida ahorra ~100 KB en cada página. `hero.jpg` (201 KB) también admite compresión. |
| M5 | **`theme-color` falta en la portada** | Las 6 páginas internas tienen `<meta name="theme-color" content="#FAF6EF">`; `index.html` no. Inconsistencia visual en el navegador móvil. |
| M6 | **Sin sitemap de imágenes** | Las fotos del equipo y los flyers de eventos no se declaran. Es opcional, pero ayuda a aparecer en Google Imágenes con búsquedas locales. |

---

### ✅ Lo que ya está bien (no tocar)

- **HTTPS y redirecciones:** `http://` y `www.` redirigen con 301 al dominio canónico. Impecable.
- **`robots.txt`:** correcto y mínimo, con el sitemap declarado. Sin bloqueos accidentales.
- **Títulos:** los 7 son únicos y caben completos (29–44 caracteres, el límite práctico son ~60).
- **Un solo `<h1>` por página**, en las 7.
- **Jerarquía de encabezados sin ningún salto** (`h1→h2→h3`) en las 7 páginas. Esto casi nunca sale bien a la primera.
- **Enlazado interno:** malla completa. Cada página enlaza a las otras seis; ninguna huérfana; ningún texto de ancla genérico tipo "clic aquí".
- **Enlaces externos:** los 76 con `target="_blank"` llevan `rel` con `noopener`. Cero excepciones.
- **Idioma declarado** (`<html lang="es">`) en las 7.
- **Rendimiento de carga:** `preconnect` a Google Fonts, `display=swap`, `preload` del `hero` con `fetchpriority="high"`, iframe del mapa con `loading="lazy"` y `title`. FCP medido entre 48 y 292 ms en local.
- **JSON-LD válido** en las 6 páginas que lo tienen (verificado con un analizador estricto).

---

## 3. Oportunidades de palabras clave

> Sin una herramienta de SEO conectada (Ahrefs, Semrush) no hay volúmenes reales. Las estimaciones de abajo se basan en el análisis del mercado local y en los competidores identificados. Para datos precisos se puede conectar una herramienta vía MCP.

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

## 4. Lista de verificación técnica

| Verificación | Estado | Detalle |
|---|---|---|
| HTTPS activo | ✅ Pasa | Certificado válido |
| Redirección `www` → sin `www` | ✅ Pasa | 301 |
| Redirección `http` → `https` | ✅ Pasa | 301 |
| `robots.txt` presente y correcto | ✅ Pasa | Sin bloqueos accidentales |
| Sitemap presente y declarado | ✅ Pasa | En `robots.txt` |
| Sitemap sin URLs rotas | ✅ Pasa | El publicado solo tiene la portada |
| `lastmod` exacto | ⚠️ Aviso | Un día atrasado |
| Canónicas en todas las páginas | ⚠️ Aviso | 7/7 presentes, **1 apunta a 404** |
| Canónicas coherentes con el sitemap | ❌ Falla | `nosotros.html` no coincide |
| Verificación de Bing | ✅ Pasa | `msvalidate.01` |
| Verificación de Google | ❌ Falla | No existe |
| IndexNow configurado | ⚠️ Aviso | Funciona, pero solo la portada |
| Títulos únicos y en rango | ✅ Pasa | 7/7, 29–44 caracteres |
| Descripciones presentes | ✅ Pasa | 7/7 |
| Descripciones en rango | ❌ Falla | 4 de 7 se cortan |
| Un `<h1>` por página | ✅ Pasa | 7/7 |
| Jerarquía de encabezados | ✅ Pasa | Sin saltos en ninguna |
| `alt` en imágenes de contenido | ✅ Pasa | Sin faltantes reales |
| Datos estructurados | ⚠️ Aviso | 6/7; falta `nosotros.html` |
| JSON-LD sintácticamente válido | ✅ Pasa | Los 6 bloques |
| `sameAs` con redes sociales | ❌ Falla | No declarado |
| `BreadcrumbList` | ❌ Falla | No existe |
| Open Graph completo | ⚠️ Aviso | **imagen 404**, sin `site_name` ni `locale` |
| Tarjetas de Twitter/X | ❌ Falla | No existen |
| `lang` declarado | ✅ Pasa | `es` en las 7 |
| Enlaces internos rotos | ✅ Pasa | Ninguno |
| Páginas huérfanas | ✅ Pasa | Ninguna |
| `rel="noopener"` en externos | ✅ Pasa | 76/76 |
| Página 404 propia | ❌ Falla | Usa la genérica de GitHub Pages |
| Apto para móvil | ✅ Pasa | Sin desbordes de 390 a 1440 px |
| Bloqueo de renderizado | ✅ Pasa | Sin scripts externos; CSS único |

---

## 5. Descripciones propuestas

Reemplazos que conservan el mensaje y caben completos:

**`servicios.html`** (264 → 156):
> Terapia psicológica individual, de pareja y familiar en Culiacán, con cuota ajustada a tu situación mediante estudio socioeconómico. Agenda tu sesión.

**`voluntariado.html`** (254 → 152):
> Súmate como voluntario a Águila Dorada A.C. en Culiacán. Buscamos apoyo en marketing, gestión de eventos, ventas y difusión. Conoce requisitos y postúlate.

**`privacidad.html`** (209 → 148):
> Aviso de privacidad de Águila Dorada A.C.: qué datos personales recabamos, para qué los usamos y cómo ejercer tus derechos ARCO.

**`contacto.html`** (190 → 151):
> Contacta a Águila Dorada A.C. en Culiacán por WhatsApp, correo o nuestro formulario. Agenda terapia, súmate como voluntario o apoya con donaciones.

---

## 6. Plan de acción priorizado

### Ganancias rápidas — esta semana

| # | Acción | Impacto | Esfuerzo | Depende de |
|---|---|---|---|---|
| 1 | Corregir la canónica y `og:url` de `nosotros.html` a `/pages/nosotros.html` | **Alto** | 2 min | — |
| 2 | Crear `assets/og-image.jpg` (1200×630) | **Alto** | 30 min | Diseño |
| 3 | Verificar el sitio en Google Search Console y enviar el sitemap | **Alto** | 15 min | Acceso a la cuenta de Google |
| 4 | Acortar las 4 descripciones largas (§5) | Medio | 10 min | — |
| 5 | Ampliar IndexNow a las 7 URLs (§A2) | Medio | 10 min | — |
| 6 | Agregar `sameAs`, `telephone` y `email` al JSON-LD de la portada | Medio | 10 min | Confirmar URL real de Facebook |
| 7 | Agregar `og:site_name`, `og:locale` y tarjetas de Twitter a las 7 | Medio | 20 min | Acción 2 |
| 8 | Agregar JSON-LD `AboutPage` a `nosotros.html` | Medio | 30 min | — |
| 9 | Actualizar `lastmod` del sitemap al publicar | Bajo | 2 min | — |
| 10 | Agregar `theme-color` a `index.html` | Bajo | 1 min | — |
| 11 | Comprimir `logo.png` y `hero.jpg` | Bajo | 15 min | — |

### Inversiones estratégicas — este trimestre

| # | Acción | Impacto | Esfuerzo |
|---|---|---|---|
| 12 | **Guía "Cuánto cuesta una terapia psicológica en Culiacán"** explicando el estudio socioeconómico. Es el diferenciador de la asociación y nadie más lo cubre. | **Alto** | Medio |
| 13 | **Reclamar y optimizar la ficha de Google Business.** Para "psicólogo cerca de mí" el paquete local pesa más que el sitio. Los horarios ya se tomaron de ahí, así que la ficha existe. | **Alto** | Medio |
| 14 | Añadir `BreadcrumbList` a las 6 páginas internas | Medio | Bajo |
| 15 | Crear `404.html` con la identidad del sitio | Medio | Bajo |
| 16 | Página de "Próximos eventos" propia (hoy es un ancla en la portada) con schema `Event`, para optar a los resultados enriquecidos de eventos | Medio | Medio |
| 17 | Páginas por padecimiento (ansiedad, depresión, duelo) con criterio clínico y revisión de la Lic. Cázarez | Medio | Alto |
| 18 | Migrar a URLs limpias (`servicios/index.html` → `/servicios/`) con redirecciones. Mejora legibilidad y tasa de clic, pero hay que hacerlo de una vez y con cuidado. | Medio | Alto |
| 19 | Conseguir enlaces entrantes: directorios de OSC, Servicios de Salud de Sinaloa, universidades con las que se colabore | **Alto** | Alto |

---

## 7. Cómo comprobar que quedó bien

Después de aplicar las correcciones:

1. **Google Search Console** → *Inspección de URLs*: probar `https://aguiladorada.org/pages/nosotros.html` y confirmar que la canónica declarada y la seleccionada por Google coinciden.
2. **[Prueba de resultados enriquecidos](https://search.google.com/test/rich-results)**: pasar las 7 URLs y verificar que cada bloque JSON-LD se lea sin errores.
3. **[Depurador de Open Graph de Facebook](https://developers.facebook.com/tools/debug/)**: pegar la portada y forzar el rescrapeo para que aparezca la imagen nueva.
4. **WhatsApp**: enviarse el enlace a uno mismo y ver si sale la tarjeta con imagen.
5. **[Bing Webmaster Tools](https://www.bing.com/webmasters)** → *Sitemaps* y *IndexNow*: confirmar que el sitemap se leyó y que los avisos llegan con las 7 URLs.
6. **[PageSpeed Insights](https://pagespeed.web.dev/)**: medir portada y servicios en móvil tras comprimir las imágenes.

---

## Fuentes

- [Psicólogos en Culiacán — Psychology Today](https://www.psychologytoday.com/mx/psicologos/si/culiacan)
- [Centros de Psicología en Culiacán — psico.org](https://www.psico.org/mx/culiacan)
- [Psicoterapeutas en Culiacán — psico.mx](https://www.psico.mx/psicologos/psicoterapia/culiacan-sinaloa)
- [Programa Salud Mental — Servicios de Salud de Sinaloa](https://saludsinaloa.gob.mx/index.php/salud-mental/)
- [SanaMente Espacio Terapéutico](https://www.sanamente-espacioterapeutico.com/)
- [Psicología Clínica América](https://www.psicologiaclinicaamerica.com/)
- [Psicólogo en Culiacán](https://psicologiaculiacan.com/)
