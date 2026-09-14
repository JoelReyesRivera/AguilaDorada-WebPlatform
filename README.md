# AguilaDorada-WebPlatform

Sitio web institucional de Águila Dorada A.C., una asociación civil (en proceso de constitución) dedicada a promover la salud mental y el bienestar emocional mediante atención psicológica accesible, educación emocional y acciones comunitarias en Culiacán y sus alrededores.

Sitio en producción: **[aguiladorada.org](https://aguiladorada.org)**.

## Páginas

El sitio son 7 páginas estáticas, enlazadas entre sí sin páginas huérfanas:

| Página | Ruta | Contenido |
|---|---|---|
| Inicio | `index.html` | Hero, misión, servicios base, próximos eventos, bitácora, voluntariado/donativos |
| Servicios | `pages/servicios.html` | Terapia y diagnóstico psicológico, proceso, preguntas frecuentes |
| Nosotros | `pages/nosotros.html` | Fundadores y equipo de psicología, con credenciales |
| Eventos pasados | `pages/eventos-pasados.html` | Memoria de talleres y conferencias, con publicaciones de Instagram como evidencia |
| Voluntariado | `pages/voluntariado.html` | Áreas de voluntariado y postulación por WhatsApp |
| Contacto | `pages/contacto.html` | Formulario, WhatsApp, correo, mapa y horarios |
| Aviso de privacidad | `pages/privacidad.html` | Borrador aún sin revisión legal — deliberadamente sin enlazar desde el menú ni el `sitemap.xml` |

La página de inicio incluye, además: aviso de crisis con acceso directo a la Línea de la Vida, accesibilidad económica vía estudio socioeconómico, copia de CLABE para donativos, y enlaces institucionales a WhatsApp, Facebook e Instagram.

## Tecnologías

- HTML5, CSS3 (una sola hoja compartida, `styles.css`) y JavaScript vanilla — sin build step, sin frameworks, sin dependencias de runtime.
- Google Fonts (Cormorant Garamond + Montserrat), con `preconnect` y carga no bloqueante.
- Publicidad hosting en GitHub Pages, con un workflow de GitHub Actions (`.github/workflows/indexnow.yml`) que avisa a Bing/IndexNow en cada push a `main`.

## Estructura

```text
.
├── index.html            # Portada
├── styles.css             # Estilos de todo el sitio (compartido por las 7 páginas)
├── 404.html                # Página de error con la identidad del sitio
├── pages/                  # Las 6 páginas internas
├── assets/
│   └── images/             # Fotos, logo, flyers de eventos
├── docs/                   # Auditorías del sitio (accesibilidad, diseño, SEO)
├── .github/workflows/      # Automatización de IndexNow
├── robots.txt, sitemap.xml # Configuración para buscadores
├── DESIGN.md                # Sistema de diseño ("El Santuario Cálido")
├── PRODUCT.md                # Producto: usuarios, posicionamiento, principios
└── README.md
```

## Ejecución local

Al ser un sitio estático, `index.html` puede abrirse directamente con doble clic, pero algunas rutas relativas y `fetch` se comportan mejor servidas por HTTP. Cualquiera de estas opciones sirve:

```powershell
# Con Python
python -m http.server 8000

# Con Node (sin instalar nada, usando npx)
npx serve .
```

Después visita `http://localhost:8000/index.html` (o el puerto que indique la herramienta elegida).

## Documentación relacionada

- **[DESIGN.md](DESIGN.md)** — sistema de diseño: color, tipografía, componentes, reglas de negocio del código de categoría y del rojo de crisis.
- **[PRODUCT.md](PRODUCT.md)** — producto: usuarios, posicionamiento, restricciones de contenido (nunca fabricar testimonios ni cifras).
- **[docs/](docs/)** — auditorías del sitio: estado de accesibilidad/diseño/rendimiento y de configuración para buscadores. Cada documento indica su fecha y qué sigue vigente.

## Estado del proyecto

En producción, con mantenimiento continuo. La base técnica (accesibilidad, SEO técnico, rendimiento) se audita periódicamente — ver `docs/`.

## Repositorio

Repositorio privado: [JoelReyesRivera/AguilaDorada-WebPlatform](https://github.com/JoelReyesRivera/AguilaDorada-WebPlatform)
