# Pruebas de UI — Página "Nosotros"

**Herramienta:** `agent-browser` 0.37.1 (Chrome for Testing 153, CDP)
**Objetivo:** `pages/nosotros.html` · **Fecha:** 2026-09-08 · **Rama:** `pagina-nosotros`
**Complementa:** [`auditoria-ux-nosotros.md`](auditoria-ux-nosotros.md) — allí están los hallazgos de diseño; aquí, lo que se pudo **medir**.

---

## Resultado

**11 de 13 pruebas pasan.** Los dos fallos son menores y ya estaban previstos en la auditoría. Lo más valioso de esta tanda es que **descarta un falso positivo** y **convierte tres estimaciones en mediciones**.

| # | Prueba | Resultado |
|---|---|---|
| T1 | Sin desbordamiento horizontal (1440 / 1024 / 768 / 390) | ✅ Pasa |
| T2 | Nav colapsa a hamburguesa en móvil | ✅ Pasa |
| T3 | Menú móvil abre, cierra y sincroniza `aria-expanded` | ✅ Pasa |
| T4 | Fallback de foto rota → monograma | ✅ Pasa |
| T5 | Revelación de tarjetas (las 11 quedan visibles) | ✅ Pasa |
| T6 | Peso de imagen servido vs mostrado | ⚠️ **10× más grande** |
| T7 | Inventario de enlaces | ✅ Sin rotos |
| T8 | Área táctil de "Agendar sesión" | ✅ Pasa AA (al límite) |
| T9 | Contraste de "Pendiente:" en `.tm-personal` | ✅ 9.32:1 |
| T9b | Contraste de "Pendiente:" en `.tm-aporte` | ❌ **4.46:1** |
| T10 | La banda de cierre se renderiza | ✅ Pasa |
| T11 | Navegación por teclado y foco visible | ✅ Pasa |
| T12 | Destinos de WhatsApp duplicados | ⚠️ **5 enlaces idénticos** |
| T13 | `aria-current` en enlaces a la página actual | ⚠️ Falta en el footer |

---

## Falso positivo descartado

### El recorte de texto en móvil no existe

Durante toda la construcción, las capturas de Edge headless a 390-414 px mostraban el texto cortado por la derecha, en esta página **y en el landing**. Medido con el navegador real:

```
1440px -> scrollW 1440, clientW 1440, overflow: false
1024px -> scrollW 1024, clientW 1024, overflow: false
 768px -> scrollW  768, clientW  768, overflow: false
 390px -> scrollW  390, clientW  390, overflow: false
```

**Cero desbordamiento en los cuatro anchos.** Era un artefacto de captura de Edge headless, no un bug. Queda descartado y no hay que tocar nada.

---

## Estimaciones que ahora son mediciones

### T6 · Las fotos se sirven 10× más grandes de lo que se ven

```json
{ "naturalPx": "1066x1599", "mostradoPx": "110x110", "vecesMasGrande": 10 }
```

Confirma el hallazgo **M6** de la auditoría con número exacto. Cada avatar de 110 px descarga una imagen de 1066 px de ancho. Sirviendo versiones de ~240×240 se recorta el peso de las 7 fotos en más del 80 %.

### T9b · El contraste de "Pendiente:" sí falla AA

```json
{ "color": "rgb(184, 92, 61)", "size": "14px", "weight": "700", "ratio": 4.46, "pasaAA": false }
```

`4.46:1` contra el mínimo de `4.5:1`. Falla por 0.04. Confirma **D2**.

Matiz que la prueba encontró: el mismo "Pendiente:" dentro de `.tm-personal` da **9.32:1** y pasa de sobra, porque `.tm-personal` reescribe el color a `--brown-soft`. Solo falla la variante dentro de `.tm-aporte`. Como estos textos desaparecen al completar el contenido, el problema se va solo — pero si la etiqueta terracota se reutiliza en otro sitio, hay que subirle el contraste.

### T8 · El área táctil pasa AA, pero justo

```json
{ "alto": 24, "ancho": 124, "cumpleAA24": true, "cumpleAAA44": false }
```

Exactamente **24 px**, el mínimo de WCAG 2.2 AA (2.5.8). La auditoría lo había marcado como "por debajo del mínimo" — **corrección: cumple AA**, pero sin un solo píxel de margen y sin llegar a AAA (44 px). Cualquier ajuste futuro de `padding` o `font-size` lo puede tirar por debajo.

---

## Hallazgos nuevos

### T12 · Un mismo enlace de WhatsApp, cinco veces, con dos promesas distintas

```json
[{ "veces": 5, "etiquetas": ["Quiero agendar mi terapia", "Agendar sesión"] }]
```

Cinco enlaces con el **href idéntico**, incluido el mismo `text=` pre-rellenado: el CTA del header más los 4 botones "Agendar sesión" de cada psicóloga. El botón bajo cada psicóloga sugiere agendar *con ella*, pero el mensaje que se abre en WhatsApp no la menciona.

Es la evidencia dura del hallazgo **M5**. Arreglo: meter el nombre en el `text=` de cada uno, o cambiar la etiqueta por una que no prometa personalización.

### T13 · Falta `aria-current` en el enlace del footer

```json
[{ "donde": "nav",    "ariaCurrent": "page" },
 { "donde": "nav",    "ariaCurrent": "page" },
 { "donde": "footer", "ariaCurrent": null   }]
```

Los dos enlaces "Nosotros" de la navegación marcan correctamente que ya estás en esa página; el del footer no. Un lector de pantalla lo anuncia como un enlace normal a otro sitio. Arreglo de un atributo.

### T10 · `content-visibility: auto` deja la banda de cierre en blanco al capturar

La sección existe y se renderiza bien en uso normal (912 px de alto, título y ambos botones presentes). Pero sale **vacía en capturas de página completa**, porque `content-visibility: auto` no pinta lo que está fuera de pantalla.

No afecta a quien visita el sitio. Sí afecta a capturas, generación de PDF y cualquier herramienta de captura larga. Vale la pena saberlo antes de que alguien reporte "la sección se ve vacía".

---

## Lo que quedó verificado

- **Fallback de foto (T4).** Rompiendo el `src` a propósito: el `<img>` se elimina solo del DOM y aparece el monograma "MC". El `onerror` hace exactamente lo que promete.
- **Revelación de tarjetas (T5).** Las 11 tarjetas reciben `is-in` y quedan a opacidad 1. El arreglo de especificidad aguanta.
- **Menú móvil (T3).** Abre (`aria-expanded="true"`, `display: flex`), cierra al pulsar un enlace y devuelve `aria-expanded="false"`. El estado accesible va sincronizado con el visual.
- **Teclado (T11).** El skip link es el primer foco de la página y los cuatro primeros elementos muestran contorno de 3 px. La ruta de teclado está sana.
- **Enlaces (T7).** 14 destinos únicos, ninguno roto, rutas relativas (`../index.html#...`) correctas desde `pages/`.

---

## Cómo reproducir

```bash
# Ojo en Windows: NO usar pipe, el daemon hereda stdout y cuelga el shell
agent-browser open "file:///.../pages/nosotros.html"  >/tmp/ab.log 2>&1 </dev/null

agent-browser set viewport 390 844          # responsive
agent-browser set media light reduced-motion # capturas sin esperar la animación
agent-browser snapshot -i                    # árbol de accesibilidad con refs @eN
agent-browser eval "<js>"                    # mediciones
agent-browser close --all
```
