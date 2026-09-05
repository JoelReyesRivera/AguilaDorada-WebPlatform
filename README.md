# AguilaDorada-WebPlatform

Sitio web institucional de Aguila Dorada A.C., una asociacion civil dedicada a promover la salud mental y el bienestar emocional mediante atencion psicologica accesible, educacion emocional y acciones comunitarias en Culiacan y sus alrededores.

## Alcance inicial

El proyecto comienza como una landing page estatica para presentar la identidad, mision, valores, servicios y formas de participacion de la asociacion.

La pagina incluye:

- Hero institucional con acceso directo a WhatsApp para hablar sobre terapia.
- Aviso de crisis con acciones para llamar a la Linea de la Vida o enviar un mensaje.
- Bitacora visual del crecimiento de la asociacion.
- Servicios de terapia y diagnostico psicologico.
- Terapia presencial y en linea.
- Informacion de accesibilidad economica y estudio socioeconomico.
- Eventos comunitarios, educativos, culturales y asistenciales.
- Registro de eventos mediante Google Forms.
- Mision y valores institucionales: empatia, amor y humanismo.
- Seccion de voluntariado con distintas formas de participar.
- Informacion de donativos mediante transferencia bancaria.
- Copia de CLABE y envio de comprobante por WhatsApp.
- Enlaces institucionales de WhatsApp, Facebook e Instagram.
- Diseno responsive para escritorio, tablet y movil.
- Mejoras de accesibilidad: foco visible, textos alternativos, contraste reforzado y soporte para reducir movimiento.

## Tecnologias

- HTML5
- CSS3
- JavaScript vanilla
- Google Fonts

## Estructura

```text
.
├── aguila-dorada.html  # Landing page actual
├── assets/
│   ├── icons/          # Iconos y recursos graficos futuros
│   └── images/         # Imagenes externas futuras
├── docs/               # Documentacion del proyecto
├── .github/            # Automatizaciones y configuracion de GitHub
├── .gitignore
└── README.md
```

Las imagenes actuales de la landing estan embebidas en el HTML base. La carpeta `assets/` queda preparada para migrarlas posteriormente a archivos independientes.

## Ejecucion local

Al ser una pagina estatica, puede abrirse directamente en el navegador haciendo doble clic en `aguila-dorada.html`.

Para servirla localmente con Python:

```powershell
python -m http.server 8000
```

Despues visita `http://localhost:8000/aguila-dorada.html`.

## Estado del proyecto

En desarrollo. La landing es la primera etapa del sitio y servira como base para incorporar formularios propios, gestion de eventos, contenidos institucionales y futuras funcionalidades.

## Repositorio

Repositorio privado: [JoelReyesRivera/AguilaDorada-WebPlatform](https://github.com/JoelReyesRivera/AguilaDorada-WebPlatform)
