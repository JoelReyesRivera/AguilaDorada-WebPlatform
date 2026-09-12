---
version: 1
slug: "pages-voluntariado-html"
primary_target: "pages/voluntariado.html"
related_targets: []
---

# Surface: Voluntariado

Scope: nueva página interna `pages/voluntariado.html`. Enlazada desde el pie de todo el sitio (nuevo enlace "Voluntariado" en Enlaces rápidos), el bloque de voluntariado del landing (`index.html` #donar, CTA repunteado de WhatsApp a esta página) y los CTA "Quiero sumarme como voluntario" de `nosotros.html` y `eventos-pasados.html`. No entra al nav principal (ya con 6 enlaces); vive en el pie y en los CTA.
Visitor mode: Persuade / reclutar (que la persona identifique su área y se postule).
Audiencia: personas que quieren donar tiempo; en especial estudiantes y pasantes de psicología que buscan práctica supervisada o servicio social.
Job: mostrar las áreas que reciben voluntariado hoy, el requisito de cada una, los requisitos generales y el proceso de alta.
Acción primaria: abrir WhatsApp con un mensaje pre-redactado por área; secundaria: pasar al formulario de contacto.
Constraints: mundo visual fijo (DESIGN.md "El Santuario Cálido"); reutiliza header, crisis-bar, footer y styles.css. Sitio estático sin backend. WhatsApp desde `WHATSAPP_NUMBER`. Áreas, requisitos, requisitos generales y proceso son BORRADOR editable (comentario en el `<head>`) que confirma el área de Administración y Voluntariado. El requisito de psicología (supervisión de la Lic. Maricela Cázarez, cédula 8820269; carta/convenio de la universidad) sale de hechos reales del sitio — no suavizar. Dato "6 voluntarios activos" tomado del landing. No inventar cifras de impacto ni testimonios.

## Direction contract

THESIS: La página de voluntariado como una convocatoria honesta, no un formulario de reclutamiento genérico. Se niega a la rejilla de "áreas" indistintas donde todas piden lo mismo: cada tarjeta lleva su línea de requisito al pie, para que quede claro de entrada que asistencial no pide nada y psicología pide formación y convenio.

OWN-WORLD: Suelo cálido por bloque (crema / crema-alt / papel). Encabezado a dos columnas compartido (`.ph-grid`) con kicker verde-salvia, H1 Cormorant corto y ficha `.ph-aside` con las condiciones de entrada (edad, horas, compromiso) y el conteo real de voluntarios. Áreas en `.cm-grid` / `.cm-card` con los cuatro tintes de categoría del sitio (`ic-comunidad` oro, `ic-educativo` navy, `ic-asistencial` verde, `ic-cultural` terracota) reciclados por afinidad temática. Único patrón propio: `.vol-req`, una línea de requisito con filete punteado y etiqueta oro-tinta al pie de cada tarjeta — composición local del lenguaje de tarjeta existente, no un token nuevo. Requisitos generales y FAQ reusan `.info-card` / `.info-row`; el proceso reusa `.proc-list` / `.proc-step`. Cierre en banda café (`.team-close`). Sin sombras salvo hover.

STORY: El visitante ve de inmediato en qué áreas hacen falta manos, entiende qué se pide en la suya, confirma los requisitos generales y el proceso de cuatro pasos, y se postula por WhatsApp con un mensaje que ya dice por qué área llega.

FIRST VIEWPORT: Encabezado a dos columnas (H1 + intro a la izquierda, ficha de condiciones a la derecha con borde-línea); el CTA primario "Postularme por WhatsApp" vive en la ficha. Debajo asoma la rejilla de áreas sobre `section-alt`.

FORM: Extensión de superficie dentro del mundo establecido — sin torneo de concepto, mismo criterio que contacto y servicios. Seed key: n/a (code-led, extensión).

FINISH: unreviewed and undocumented is unfinished.
