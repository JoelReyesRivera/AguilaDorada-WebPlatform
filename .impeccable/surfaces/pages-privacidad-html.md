---
version: 1
slug: "pages-privacidad-html"
primary_target: "pages/privacidad.html"
related_targets:
  - "styles.css"
  - "pages/contacto.html"
---

# Surface: Aviso de Privacidad

Scope: nueva página interna `pages/privacidad.html`. Enlazada desde la barra inferior del pie de TODAS las páginas (nuevo enlace `.foot-legal` "Aviso de privacidad", junto a la nota "Tu privacidad es nuestra prioridad") y desde el `.cf-lede` del formulario de contacto.
Visitor mode: Inform (documento legal de consulta).
Audiencia: personas que quieren saber cómo se tratan sus datos antes de agendar terapia o escribir; y el notario/abogado que revisará el texto.
Job: cumplir el requisito de aviso de privacidad de la LFPDPPP — responsable, datos recabados, finalidades primarias y secundarias, transferencias, conservación, derechos ARCO por admi@, cookies/terceros del sitio.
Acción: leer; secundaria: escribir a Administración para ejercer derechos ARCO.
Constraints: mundo visual fijo (DESIGN.md "El Santuario Cálido"); reutiliza header, crisis-bar, footer y styles.css. TEXTO = BORRADOR ESTÁNDAR pendiente de validación legal (comentario en el `<head>` lista los 5 puntos a confirmar: denominación legal, domicilio, responsable designado, plazo de conservación de expedientes, herramientas de terceros vigentes). Responsable declarado: "Águila Dorada (en formación)", domicilio del sitio, correo ARCO aguiladorada.admi@gmail.com — coherente con [[email-addresses]]. No prometer certificaciones ni medidas que la asociación no tenga.

## Direction contract

THESIS: El aviso de privacidad como un documento que de verdad se puede leer, no un muro de texto legal. Se niega al bloque monolítico: índice navegable arriba, secciones numeradas cortas, lenguaje llano con la obligación legal intacta debajo.

OWN-WORLD: Suelo cálido (crema / crema-alt). Encabezado a dos columnas compartido (`.ph-grid`) con kicker verde-salvia, H1 Cormorant y ficha `.ph-aside` que fija de un vistazo al responsable, el domicilio, el correo ARCO y la fecha de actualización. Cuerpo del documento en una sola tarjeta papel centrada y de medida legible (`.legal-doc`, max 760px) con: sello de estado ("Borrador para revisión legal"), índice a dos columnas (`.legal-toc`) y doce `.legal-section` numeradas con `counter()` en oro. Cierre en banda café (`.team-close`) con el CTA a Administración. Patrón propio (`.legal-doc` y familia) construido con las primitivas del sistema: papel, borde 1px `--line`, radio 14px, Cormorant para los H2, Montserrat para los H3-etiqueta, acento oro en la numeración — misma lógica que `.info-card` / `.proc-step`, sin tokens nuevos.

STORY: El visitante localiza en el índice la sección que le importa (casi siempre "Tus derechos ARCO"), la lee sin traductor legal, y sabe a qué correo escribir. El abogado encuentra el articulado completo en orden.

FIRST VIEWPORT: Encabezado a dos columnas (H1 "Aviso de Privacidad" + intro a la izquierda, ficha del responsable a la derecha con borde-línea). Debajo asoma la tarjeta del documento sobre `section-alt` con el sello de estado y el arranque del índice.

FORM: Extensión de superficie dentro del mundo establecido — sin torneo de concepto. Seed key: n/a (code-led, extensión).

FINISH: unreviewed and undocumented is unfinished. El texto legal queda explícitamente marcado como borrador hasta la revisión del asesor de la asociación.
