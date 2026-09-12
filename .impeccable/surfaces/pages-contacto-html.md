---
version: 1
slug: "pages-contacto-html"
primary_target: "pages/contacto.html"
related_targets: []
---

# Surface: Contacto

Scope: nueva página interna `pages/contacto.html`, enlazada desde el nav "Contacto" (antes ancla al footer del landing) en index, nosotros y eventos-pasados.
Visitor mode: Act (que la persona elija un canal y escriba).
Audiencia: personas que ya decidieron acercarse — para terapia, información, voluntariado o donaciones.
Job: dar todas las vías de contacto (WhatsApp, dos correos, redes, dirección, horario, mapa) y un formulario que arma el mensaje.
Acción: abrir WhatsApp / correo con el mensaje ya redactado; o seguir a agendar.
Constraints: mundo visual fijo (DESIGN.md "El Santuario Cálido"); reutiliza header, crisis-bar, footer y styles.css. Sitio estático sin backend: el formulario no envía nada — construye texto y abre `wa.me` o `mailto`. Número de WhatsApp desde `WHATSAPP_NUMBER`. Horarios de atención son borrador editable (comentario en el `<head>`).

## Direction contract

THESIS: Página de contacto como recepción cálida, no un formulario genérico solo. Los canales directos van primero (tarjetas en papel con ícono tintado, mismo lenguaje que `.tm-card` / `.event-card`); el formulario es una comodidad, no la única puerta.

OWN-WORLD: Fondo cálido por bloque (crema / crema-alt). Encabezado a dos columnas compartido (`.ph-grid`) con kicker "Estamos para escucharte" + H1 Cormorant corto ("Hablemos") y la ficha `.ph-aside` con dirección y garantías. Tarjetas de canal con los cuatro tintes del sitio (`ic-asistencial` verde, `ic-educativo` navy, `ic-comunidad` oro, `ic-cultural` terracota). Formulario + ficha lateral con el mismo reparto de columnas que `.ph-grid`; inputs en papel, borde `--line`, foco navy. FAQ reutiliza `.info-card` / `.info-row`. Cierre en banda café (`.team-close`) con dos CTAs. Sin sombras salvo hover.

STORY: El visitante ve que hay varias formas reales de llegar (respuesta el mismo día por WhatsApp), entiende qué correo es para qué, sabe dónde está la sede y qué pasa con sus datos, y escribe.

FIRST VIEWPORT: Encabezado a dos columnas; debajo, la rejilla de canales directos sobre `section-alt`.

FORM: Extensión de superficie dentro del mundo establecido — sin torneo de concepto. Seed key: n/a (code-led, extensión).

FINISH: unreviewed and undocumented is unfinished; este build termina con el finish review, el veredicto y DESIGN.md al día.
