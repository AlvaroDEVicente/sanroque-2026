# Fiestas de San Roque · Valdeobispo

Web informativa del programa de las fiestas patronales de Valdeobispo (Cáceres),
del 13 al 16 de agosto de 2026.

**En producción:** https://sanroquevaldeobispo.netlify.app

El acceso principal del público es un código QR impreso en el cartel de fiestas,
por lo que la web está diseñada mobile-first y la URL es inmutable.

---

## Decisiones técnicas

**Un único archivo, sin dependencias.** Todo el sitio es un `index.html` con el
HTML, el CSS y el JavaScript en el mismo fichero. Sin framework, sin bundler,
sin `node_modules`, sin paso de build. La única carga externa son las fuentes de
Google Fonts.

El motivo no es minimalismo por gusto: es una web de contenido estable que
cualquiera debe poder abrir y editar dentro de cinco años, sin reinstalar un
ecosistema de herramientas que para entonces estará obsoleto. Un framework aquí
añadiría mantenimiento sin aportar nada.

**Contenido separado de la lógica.** El programa completo vive en un objeto
`FIESTAS` al principio del `<script>`, marcado como zona editable. Actualizar la
edición de un año a otro es cambiar ese objeto; no hay que tocar ni el marcado
ni los estilos.

```js
const FIESTAS = {
  edicion: 2026,
  dias: [
    { diaSemana:"Jueves", fecha:"2026-08-13", eventos:[
      { hora:"23:00", tipo:"cultura", titulo:"Pregón de fiestas..." },
      { hora:"01:30", madrugada:true, tipo:"verbena", titulo:"DJ..." },
    ]},
  ],
};
```

**Estado según la fecha.** La página se comporta distinto según cuándo se
visite: cuenta los días que faltan, y durante las fiestas abre automáticamente
por el día en curso y resalta el próximo acto. Sin backend: se calcula en
cliente a partir de las fechas del programa.

**Actos de madrugada.** Un concierto a las 03:00 del "viernes 14" ocurre en
realidad la noche del viernes al sábado. Los eventos marcados con
`madrugada: true` se muestran con el chip horario en tono nocturno y una nota
que aclara el día real, evitando el malentendido más probable en un programa de
fiestas.

**Accesibilidad.** El público es un pueblo entero, de todas las edades:
tipografía grande, contraste alto, foco visible en los elementos navegables,
navegación por teclado en el selector de días y respeto a
`prefers-reduced-motion`.

**Sin recogida de datos.** Ni analítica, ni cookies, ni scripts de terceros. La
web no necesita avisos legales porque no trata ningún dato personal.

---

## Estructura

```
.
├── index.html       # La web completa
├── escudo.svg       # Escudo del municipio, en el pie de página
├── favicon.png      # Icono de pestaña (bandera del municipio)
├── og-image.jpg     # Imagen de previsualización al compartir el enlace
├── CLAUDE.md        # Convenciones del proyecto
├── .gitattributes   # Normalización de finales de línea (LF)
└── README.md
```

## Desarrollo

No hay nada que instalar. Clonar el repositorio y abrir `index.html` en el
navegador.

```bash
git clone https://github.com/AlvaroDEVicente/sanroque-2026.git
```

## Despliegue

Alojado en Netlify con despliegue continuo: cada push a `main` publica
automáticamente. Sin comando de build; el directorio de publicación es la raíz
del repositorio.

> **Aviso:** el nombre del proyecto en Netlify no debe cambiarse. La URL está
> codificada en el QR impreso del cartel de fiestas; renombrarla dejaría todos
> los carteles apuntando a una dirección muerta.

## Actualizar de un año a otro

1. Editar el objeto `FIESTAS`: `edicion`, las `fecha` (formato `AAAA-MM-DD`) y
   el `diaSemana` de cada jornada.
2. **Verificar que el día de la semana declarado coincide con el calendario
   real.** San Roque cae siempre el 16 de agosto, pero el día de la semana
   cambia cada año: es el error más fácil de colar.
3. Commit y push a `main`. El resto es automático.

---

## Créditos

Programa oficial del Ayuntamiento de Valdeobispo, Concejalía de Festejos.

Desarrollo web: [Álvaro DE Vicente](https://github.com/AlvaroDEVicente).
