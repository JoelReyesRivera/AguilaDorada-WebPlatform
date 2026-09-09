# Auditoría de diseño — Nosotros (segunda pasada)

**Alcance:** solo diseño visual y comportamiento responsive. No se revisa contenido.
**Objetivo:** `pages/nosotros.html` tras aplicar la primera auditoría · **Fecha:** 2026-09-08
**Método:** medición en navegador real (`agent-browser`) en 9 anchos, del 480 al 1440.

> **Actualización 2026-09-09 — aplicado:**
> - **#1 rejilla:** ahora salta 4 → 2 → 1 columnas sin pasar por 3. Se elimina el 3+3+1. Queda una fila de una sola tarjeta en Fundadores en el tramo 700–1039 px (7 es impar), pero centrada, no abandonada a la izquierda. Aceptado.
> - **#2 tarjeta estirada:** `max-width` de 400 px en una columna (antes 536).
> - **#3 regla cortada:** el texto se acota con un `<span>` interno; el `border-top` ocupa el ancho completo. Medido: 99 % en los 7 anchos (antes 62 % en una columna).
> - **#4 jerarquía por tamaño:** aporte 15 px / texto principal · "En lo personal" 14 px / marrón suave. El primario vuelve a ser el más grande.
> - **Regla de "En lo personal" descuadrada entre tarjetas de una fila** (reporte del usuario): el bloque estaba anclado al fondo (`margin-top: auto`), así que un "En lo personal" de 1, 2 o 3 líneas dejaba la regla a distinta altura entre vecinas (medido: hasta 40 px de diferencia). Arreglo: nombre con altura de 2 líneas reservada, el aporte crece para llenar el hueco (`flex-grow: 1`), y el texto de "En lo personal" reserva 3 líneas con más de una columna. Medido: alineado en los 7 anchos de 390 a 1440.
> - **#6 espaciados fuera de escala:** `page-head` 56/40, `team-section` 40/56. Ya están en la escala 8/14/24/40/56.
> - Sin resolver a propósito: **#5** (14 / 14.5 / 15 son variaciones aceptadas por DESIGN.md), **#8** y **#9** (menores).

---

## Veredicto

Las correcciones de la primera pasada aguantan. Lo que queda son **problemas de escalado entre anchos**: el diseño está afinado para 4 columnas y para móvil, pero los anchos intermedios y la columna única quedaron sin resolver.

Tres cosas valen la pena arreglar antes de dar la página por terminada.

---

## Alta

### 1 · La rejilla deja tarjetas huérfanas en anchos intermedios

Medido, número de tarjetas por fila:

| Ancho | Fundadores | Psicología |
|---|---|---|
| 1440 – 1180 | 4 + 3 | 4 |
| **1040 – 900** | **3 + 3 + 1** | **3 + 1** |
| **820 – 700** | **2 + 2 + 2 + 1** | 2 + 2 |
| 600 – 480 | 1 por fila | 1 por fila |

El cambio a flex centrado resolvió el hueco a la derecha con 4 columnas, pero **no resuelve la fila de una sola tarjeta**. Una tarjeta sola y centrada bajo dos filas llenas se lee como un error de maquetación, no como una decisión.

Afecta a portátiles de 1024 px y tablets en horizontal, que no son casos raros.

**Arreglo:** saltar de 4 a 2 columnas sin pasar por 3, de modo que 7 tarjetas den 4+3 o 2+2+2+1 → sigue habiendo huérfana. La solución real es fijar el número de columnas a un divisor cómodo y **aceptar la fila corta pero nunca de una sola tarjeta**: 4+3 (bien), 3+3+1 (mal), 2+2+2+1 (mal). Con 7 elementos, solo 4 columnas funciona limpio. Conviene forzar `min-width` de tarjeta más bajo para mantener 4 columnas hasta ~900 px, y de ahí saltar directo a 2.

### 2 · En una columna la tarjeta se estira a 536 px

Con el viewport a 600 px la tarjeta mide **536 px** y dentro lleva un avatar de 148 px centrado y dos líneas de texto. El resultado es una tarjeta casi vacía a lo ancho.

La regla `max-width: none` por debajo de 640 px es demasiado permisiva. Una tarjeta de retrato no debería pasar de unos 380-420 px por muy ancha que sea la pantalla.

### 3 · La regla de "En lo personal" no llega al borde

`.tm-personal` lleva `max-width: 42ch`, y como el separador es un `border-top` de ese mismo elemento, **la línea se corta donde acaba el texto**, no donde acaba la tarjeta.

| Ancho de tarjeta | Ancho de la regla |
|---|---|
| 263 px (4 columnas) | 213 px de 215 → 99 %, imperceptible |
| 536 px (1 columna) | **301 px de 488 → 62 %** |

En columna única la línea se queda a media tarjeta y parece un corte accidental.

**Arreglo:** mover el `max-width` al texto (un `<span>` interno) y dejar que el `border-top` ocupe el ancho completo del contenedor.

---

## Media

### 4 · La jerarquía sigue invertida, ahora por tamaño

En la primera pasada corregí el color (el aporte pasó a texto principal, la etiqueta personal dejó el verde). Pero el **tamaño** quedó igual:

- `.tm-aporte` (dato primario): **14 px**
- `.tm-personal` (dato secundario): **15 px**

El contenido secundario sigue siendo tipográficamente más grande que el primario. El color tira en una dirección y el tamaño en la contraria.

### 5 · Tres tamaños de cuerpo casi idénticos

`14 px` (aporte), `14.5 px` (intro de sección, cierre), `15 px` (personal). Diferencias de medio punto que no se leen como pasos de una escala, solo como inconsistencia acumulada.

### 6 · Espaciados fuera de la escala del sitio

La escala documentada en `DESIGN.md` es 8 / 14 / 24 / 40 / 56.

- `.page-head` cierra con `padding-bottom: 4px` — un número mágico para compensar el `44px` de la sección siguiente.
- `.team-section` abre con `padding-top: 44px`.

Ninguno de los dos está en la escala. Funciona, pero es deuda: el siguiente que toque el espaciado no sabrá de dónde salen esos valores.

### 7 · Alineación mixta que se rompe al ensanchar

El bloque de identidad (foto, nombre, puesto) va centrado y el cuerpo (aporte, personal) a la izquierda. A 263 px de tarjeta la mezcla no se nota. A 536 px sí: el avatar flota en el centro y el texto arranca pegado al borde izquierdo, muy lejos de él.

Es consecuencia directa del punto 2; al limitar el ancho de tarjeta se corrige solo.

---

## Baja

### 8 · El avatar ocupa el 69 % del ancho útil

148 px de avatar sobre 215 px de área de contenido. Es la proporción que se pidió y funciona a 4 columnas, pero está en el límite: cualquier aumento más deja el texto sin aire.

### 9 · Las dos secciones tienen ritmos distintos

Fundadores presenta 4+3; Psicología, 4 en una fila. Al recorrer la página se percibe un cambio de compás entre secciones que comparten el mismo componente. No es un error, pero una rejilla que se comporte igual en ambas daría más unidad.

---

## Lo que sigue bien

- **Alturas de tarjeta consistentes por fila.** Medido: fila 1 de fundadores 492 px ×4, fila 2 471 px ×3, psicología 480 px ×4. El `min-height` del aporte cumple su función.
- **Cero desbordamiento horizontal** en los 9 anchos probados.
- **Colores dentro del sistema.** Los 5 tonos de texto de la tarjeta son todos tokens del sistema; el navy que aparece es el monograma de psicología, deliberado.
- **Escala de titulares limpia:** 46 → 30 → 21 px, con Cormorant en todos y sin pasos intermedios sobrantes.
