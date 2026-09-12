---
version: 1
slug: "pages-servicios-html"
primary_target: "pages/servicios.html"
related_targets: []
---

# Surface: Servicios

Scope: nueva página interna `pages/servicios.html`, enlazada desde el nav "Servicios" (antes ancla `index.html#servicios`) en index, nosotros, eventos-pasados y contacto. La sección `#servicios` del landing se conserva como resumen; esta página es la versión completa.
Visitor mode: Persuade (que la persona entienda qué servicio necesita y agende).
Audiencia: personas buscando terapia o una evaluación, y quienes exploran los programas comunitarios antes de acercarse.
Job: explicar a fondo los dos servicios clínicos base (terapia, diagnóstico) — modalidades, para quién, cómo es el proceso, cómo se define la cuota — y ubicar los cuatro frentes comunitarios como programa aparte.
Acción: abrir WhatsApp para agendar / seguir a la página de contacto; secundaria: ver eventos.
Constraints: mundo visual fijo (DESIGN.md "El Santuario Cálido"); reutiliza header, crisis-bar, footer y styles.css. Sitio estático sin backend. Número de WhatsApp desde `WHATSAPP_NUMBER`. Cuota $150–$350 y estudio socioeconómico son hechos reales del sitio — no alterar. Proceso, modalidades y FAQ son BORRADOR editable (comentario en el `<head>`). No inventar testimonios ni cifras de impacto.

## Direction contract

THESIS: La página de servicios como una recepción que orienta, no un catálogo. Se niega a la rejilla de tarjetas iguales donde todo pesa lo mismo: primero los dos servicios clínicos con su jerarquía (tarjeta sólida café/navy del sistema), después el proceso paso a paso, y solo al final los programas comunitarios como banda distinta — para que nadie confunda "agendar terapia" con "venir a un taller".

OWN-WORLD: Suelo cálido por bloque (crema / crema-alt / papel). Encabezado a dos columnas compartido (`.ph-grid`) con kicker verde-salvia, H1 Cormorant corto y ficha `.ph-aside` con las tres garantías (cuota ajustada, presencial o en línea, confidencial). Los dos servicios base reusan `.service-card` sólida (`.sc-terapia` café, `.sc-diagnostico` navy) verbatim del landing. Proceso en tarjetas papel numeradas con disco dorado (`.proc-step`). Programas comunitarios en `.cm-grid` con los cuatro tintes de categoría del sitio (`ic-comunidad` oro, `ic-educativo` navy, `ic-asistencial` verde, `ic-cultural` terracota). FAQ reusa `.info-card` / `.info-row`. Cierre en banda café (`.team-close`). Sin sombras salvo hover.

STORY: El visitante distingue de inmediato terapia de diagnóstico y ve que son independientes, entiende que la cuota se ajusta a su situación con un estudio socioeconómico, sigue el proceso de cuatro pasos sin sorpresas, ubica los programas comunitarios como otra puerta, y agenda.

FIRST VIEWPORT: Encabezado a dos columnas (H1 + intro a la izquierda, ficha de garantías a la derecha con borde-línea); el CTA primario "Agenda una primera cita" vive en la ficha. Debajo asoma la rejilla de dos servicios base sobre `section-alt`.

FORM: Extensión de superficie dentro del mundo establecido — sin torneo de concepto, mismo criterio que la página de contacto. Seed key: n/a (code-led, extensión).

FINISH: unreviewed and undocumented is unfinished; this build ends with the finish review, the verdict, DESIGN.md, and every shipping raster carrying its provenance.

## Documentation note (2026-09-10)

Post-build documentation pass. Disposition: **no changes to DESIGN.md or `.impeccable/design.json`**. Ordinary surface extension inside "El Santuario Cálido"; the recorded system still matches the shipped artifact.

Evidence checked:
- `DESIGN.md` + `.impeccable/design.json` (incumbent system, schemaVersion 2) read in full.
- `styles.css` appended block "Página Servicios" (lines ~2659–2743): `.page-head--services` (padding 60/48px only), `.proc-list` (2-col grid, 16px gap, collapses to 1col at 820px), `.proc-step`, `.proc-step::before`, `.proc-note`, `.programa-card .cm-link`. No custom properties touched; no existing selector modified. Confirmed against the token frontmatter.
- `pages/servicios.html` full source: reuses crisis-bar, `header`/nav, `.ph-grid`/`.ph-aside`, `.section-alt`, `.head-block`, `.service-card`/`.sc-terapia`/`.sc-diagnostico`, `.pill-row`/`.pill`, `.cm-grid`/`.cm-card` with the four fixed category tints (`ic-comunidad`/`ic-educativo`/`ic-cultural`/`ic-asistencial`), `.info-card`/`.info-row`, `.team-close`, `footer` — all verbatim from the established pages.
- `.impeccable/config.json`: two pre-sanctioned detector entries for `pages/servicios.html` (`cream-palette`, `cramped-padding`) mirroring the sibling pages.

`.proc-step` (net-new pattern) is built entirely from existing primitives: `--paper` ground, 1px `--line` border, 12px radius (`rounded.lg`), `--gold` numeral disc with `--brown` text, Montserrat/Cormorant. It reads as a local composition of the incumbent card language (same ground/border/radius as `.info-card`, gold disc echoing the CTA-invitation role of gold) — not a new token or a new form language — so it does not require a Components entry to keep future surfaces on-brand.

Divergences and defects carried, NOT canonized and NOT repaired here:
- **Kickers/eyebrows.** The page ships five `.kicker` eyebrows ("Qué ofrecemos", "Servicios base", "Paso a paso", "Programa aparte", "Preguntas frecuentes"). This is the ratified site-wide `.kicker` pattern (standing note b) and DESIGN.md already references kickers under the Label role; the site-wide count is pre-existing drift. Recorded here as a craft-floor defect the build carries, not promoted to a new design-system rule.
- **Empty band above the aside in the first viewport.** The short H1 ("Cómo te acompañamos") leaves whitespace above `.ph-aside` on wide viewports (standing note a). Pre-existing behaviour of the shared `.ph-grid` head; polish opportunity, not a system change.
- **`.programa-card .cm-link` colour.** The OWN-WORLD block describes "the four category tints" for the community-program links; the build actually ships `--brown` default / `--gold-ink` hover (one treatment for all four cards). Build wins; the divergence is cosmetic and not worth a token or rule.
