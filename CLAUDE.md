# Fiestas de San Roque · Valdeobispo

Web informativa del programa de fiestas del municipio de Valdeobispo (Cáceres).
La organiza el Ayuntamiento de Valdeobispo, Concejalía de Festejos.

## Contexto crítico

- **La web se difunde mediante un QR impreso en el cartel de fiestas.** El QR
  codifica `https://sanroquevaldeobispo.netlify.app`. Esa URL NO puede cambiar
  nunca: si se renombra el proyecto en Netlify, todos los carteles impresos
  dejan de funcionar. No tocar el nombre del sitio.
- **La mayoría del público entra desde el móvil.** Mobile-first no es una
  preferencia estética, es el caso de uso principal.
- El público es todo el pueblo, de todas las edades. Prima la legibilidad
  (tipografía grande, contraste alto) sobre cualquier florituras visual.

## Arquitectura

Un único archivo `index.html` en la raíz del repo. HTML + CSS + JS en el mismo
fichero, sin build, sin dependencias, sin framework. Solo se cargan fuentes de
Google Fonts por CDN.

Mantenerlo así: no introducir bundlers, npm, ni frameworks. El valor de este
proyecto es que cualquiera pueda abrir el archivo y editarlo.

Netlify despliega la rama `main` automáticamente en cada push.
Build command vacío, publish directory la raíz.

## Cómo se edita el contenido

Todo el programa vive en el objeto `FIESTAS`, al principio del `<script>`,
marcado como zona editable. Para actualizar de un año a otro solo se cambian
ahí `edicion`, las `fecha` (formato AAAA-MM-DD) y el `diaSemana`.

Cada evento admite:
- `hora`: "HH:MM"
- `madrugada: true` si el acto ocurre pasada la medianoche (se muestra con el
  chip de hora en tono nocturno y la nota "Madrugada del <día>")
- `tipo`: musica | cultura | fiesta | religioso | charanga | verbena | atracciones
- `titulo`: texto del acto

**Verificar siempre que el día de la semana declarado coincide con la fecha
real del calendario.** San Roque es el 16 de agosto, pero el día de la semana
cambia cada año y es un error fácil de colar.

## Convenciones

- Idioma: español. Comillas angulares («») para nombres de grupos y actos.
- Los textos de los actos se transcriben del programa oficial del Ayuntamiento.
  No inventar, reescribir ni "mejorar" actos: si algo del programa parece un
  error, señalarlo, no corregirlo por cuenta propia.
- Accesibilidad: mantener el contraste, el foco visible en los elementos
  navegables y el respeto a `prefers-reduced-motion`.
- No usar localStorage ni sessionStorage.
- No añadir analítica, cookies ni scripts de terceros sin pedirlo: obligaría a
  añadir avisos legales a una web que ahora mismo no recoge ningún dato.

## Qué NO incluir

- Logos o nombres de negocios patrocinadores (decisión tomada: fuera de la web).
- Datos personales de terceros (teléfonos particulares, etc.) sin permiso.

## Flujo de trabajo

Cambio en `index.html` → commit → push a `main` → Netlify publica solo.
Como cada push va directo a producción, no subir cambios a medias durante los
días de fiestas. Para cambios grandes, usar una rama y aprovechar los deploy
previews de Netlify.
