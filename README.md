# SOFT-12 · Tarea 1 — Construcción de interfaces web adaptables con HTML y CSS

## Identificación
- **Estudiante:** Jose Ricardo Barrantes Saenz
- **Curso:** SOFT-12 · Programación web avanzada
- **Sección:** SCV2
- **Docente:** Álvaro Cordero Peña
- **Fecha de entrega:** 2026-09-20

## Descripción de los casos

### Caso 1 — Centro de control de una expedición científica
Pantalla tipo "centro de control" para el equipo coordinador de una expedición
científica en Monteverde. Prioriza la visualización simultánea de indicadores,
misiones activas, equipos, alertas y agenda, pensada para permanecer abierta en
computadora o tableta.

### Caso 2 — Panel público de información de un festival
Panel de consulta para asistentes del Festival Cultural Ribera, pensado
"mobile-first". Tiene las siguentes secciones qué está pasando ahora, qué sigue, cambios de última hora,
servicios disponibles y programación por escenario.

## Estructura de carpetas

```
soft12-Tarea1/
├── README.md
├── caso1/
│   ├── index.html
│   ├── css/estilos.css
│   └── img/
└── caso2/
    ├── index.html
    ├── css/estilos.css
    └── img/
```


## Decisiones de diseño

- **¿Por qué seleccionó determinadas etiquetas semánticas?** 
Usé header para la cabecera de la expedición porque agrupa la identificación del documento (nombre, ubicación, día, estado). nav envuelve la lista de enlaces a las secciones. main contiene el contenido principal único de la página. Cada bloque temático (Misiones, Equipos, Alertas, Agenda) es una section con su propio h2, porque son agrupaciones de contenido relacionado con título. Cada misión y cada alerta es un article porque tiene sentido por sí misma y podría mostrarse de forma independiente. Las alertas van en un aside cuando son complementarias al flujo principal. Usé ul/li para la agenda porque es una lista de ítems y time para las horas. Reservé div solo para agrupar por motivos de layout donde no había un significado semántico.
- **¿Cómo organizó la jerarquía de encabezados?** 
Cada página tiene un único h1 (el nombre de la expedición / del festival). Cada sección principal tiene un h2 (Resumen de operaciones, Misiones activas, Equipos, Alertas, Agenda). Dentro de cada sección, los elementos individuales usan h3 (el nombre de cada misión, de cada equipo). No salté niveles: nunca hay un h3 sin un h2 por encima. Cuando necesité texto grande sin ser un título (el número de un indicador) usé un span/p con una clase, no un encabezado, porque los encabezados definen estructura, no tamaño. Comprobé la jerarquía con la vista de esquema del navegador usando DevTools.
- **¿Cómo incorporó la accesibilidad básica?** 
`lang="es"` en el documento 
`alt` descriptivo en imágenes de contenido y `alt=""` en las decorativas
`aria-labelledby` en cada sección, `aria-label` en los botones de cerrar alerta, `aria-current` en el enlace de navegación activo
Los estados (pendiente, en progreso, completada, suspendida / en este momento) se comunican con texto además de color.
- **¿Cómo funciona el modelo de caja en sus principales componentes?** 
Apliqué *, *::before, *::after { box-sizing: border-box } globalmente, de modo que el width de una tarjeta incluye su padding y su borde y puedo declarar width: 100% sin desbordar. En las tarjetas de misión el espaciado interno es padding: var(--espacio-3) y la separación entre tarjetas la aporta el gap del contenedor Grid/Flex, no márgenes individuales, así no hay márgenes dobles ni correcciones con :last-child. Los contenedores tienen max-width en rem y width: 100%, de forma que en teléfono ocupan todo el ancho y en escritorio se limitan. Las imágenes tienen max-width: 100%; height: auto. Los márgenes verticales entre secciones siguen una escala (--espacio-2, --espacio-4) definida en variables para que el espaciado sea un sistema y no valores arbitrarios.
- **¿Dónde utilizó posicionamiento, cuál valor de position y por qué?**
Caso 1: el encabezado con los indicadores usa position: sticky; top: 0; z-index: 10 para que el estado general permanezca visible mientras el coordinador recorre las misiones; elegí sticky y no fixed porque sticky respeta el flujo y no obliga a compensar con padding. Además, la etiqueta de prioridad de cada misión usa position: absolute; top: .5rem; right: .5rem dentro de la tarjeta, que tiene position: relative para ser su contenedor de referencia; la tarjeta reserva padding-top suficiente para que la etiqueta nunca tape el título en 320px. Caso 2: la navegación es position: sticky; bottom: 0 en teléfono para que siempre esté al alcance del pulgar, y la etiqueta «En este momento» es absolute sobre la tarjeta relative. En ningún caso el posicionamiento construye el layout general: eso lo hacen Grid y Flexbox.
- **¿Por qué algunos estilos prevalecen sobre otros?**
Por la cascada: cuando dos reglas afectan a la misma propiedad gana la de mayor especificidad y, a igual especificidad, la que aparece después en el archivo. Organicé el CSS en ese orden: reset y variables, estilos base de elementos, layout, componentes, modificadores y por último las media queries, de modo que una regla posterior sobrescriba intencionalmente a una anterior con la misma especificidad (una clase). Usé casi exclusivamente selectores de clase (especificidad 0,1,0) para que ningún selector «pese» demasiado; los modificadores como .alerta--critica van después de .alerta y por eso prevalecen. No usé !important ni IDs para estilos, porque rompen la cascada y obligan a más correcciones.
- **¿Dónde utilizó Flexbox y por qué?** 
Usé Flexbox donde había que distribuir elementos en una dimensión: la navegación (.nav__lista) es una fila de enlaces con gap que se envuelve con flex-wrap en teléfono; la cabecera de cada misión usa justify-content: space-between para llevar el título a la izquierda y la prioridad a la derecha; los indicadores se reparten el ancho con flex y se envuelven; dentro de las tarjetas, flex-direction: column con margin-top: auto en el pie mantiene el estado alineado abajo; los badges de estado usan inline-flex para alinear icono y texto. En el festival, la navegación cambia de columna a fila con una media query. No usé Flexbox para la estructura general porque esa es bidimensional.

- **¿Dónde utilizó CSS Grid y por qué?**
En el Caso 1 el main es un Grid con grid-template-areas: en teléfono una sola columna (resumen, alertas, misiones, equipos, agenda), en tableta dos columnas y en escritorio tres, con las misiones ocupando dos filas y las alertas siempre visibles en la primera fila; así reorganizo zonas completas cambiando solo la plantilla de áreas. También usé repeat(auto-fit, minmax(16rem, 1fr)) para la cuadrícula de equipos, que ajusta el número de columnas al ancho sin media queries. En el Caso 2 la programación por escenarios es un Grid cuyas columnas son los cuatro escenarios y cuyas filas son las franjas horarias, lo que permite compararlos visualmente en escritorio; en teléfono pasa a una columna.
- **¿Cómo cambia el layout entre teléfono, tableta y escritorio?**
en teléfono todo se apila en una columna
  siguiendo el orden de prioridad de lectura; en tableta se ajustan
  distribuciones intermedias; en escritorio el contenedor principal pasa a
  grid multi-columna con zonas nombradas.
- **¿Cuáles media queries utilizó y por qué seleccionó esos breakpoints?** 
Usé dos media queries de tipo min-width (mobile-first): @media (min-width: 601px) y @media (min-width: 1024px), que corresponden a la referencia de la consigna (teléfono hasta 600, tableta hasta 1024, escritorio a partir de 1024). Los elegí porque a partir de ~600px ya caben dos tarjetas de 16rem con su separación, y a partir de ~1024px caben tres zonas legibles. Las verifiqué con el modo dispositivo de DevTools en 320, 375, 600, 768, 1024 y 1440px.
- **¿Cuáles unidades relativas utilizó?** 
Tipografía en rem con clamp() para el título fluido; espaciados y radios en rem/em a través de variables; anchos de contenedores en %/max-width en rem; columnas de Grid en fr; alturas mínimas de la cabecera en vh cuando aplica. Solo usé px para bordes de 1px y sombras.
- **¿Para qué sirven las variables CSS que definió?** 
Definí en :root un sistema: colores (--color-primario, --color-fondo, --color-texto, --color-alerta-critica…), una escala de espaciado (--espacio-1 a --espacio-6), radios (--radio) y sombras. Sirven para centralizar los valores y mantener consistencia, ya que cambiar el color primario en un sitio actualiza toda la interfaz; la escala de espaciado evita valores arbitrarios en lo posible.

## Instrucciones para abrir cada caso
Cada caso es independiente y no requiere servidor. Abrir directamente
`caso1/index.html` o `caso2/index.html` en cualquier navegador tras hacer un clone del repositorio.

## Resumen de commits

| # | Fecha | Hash | Mensaje | Caso | Cambio |
|---|------------|---------|--------------------------------------------------------------|--------|-------------------------------------------|
| 1 | 2026-09-05 | cb77f22 | Initial commit | Ambos | Estructura inicial del repositorio |
| 2 | 2026-09-05 | 2798ada | Prueba remote comit con Token | Ambos | Verificación de conexión remota |
| 3 | 2026-09-05 | db567a6 | Creacion de la estructura de archivos | Ambos | Carpetas caso1/caso2, index.html base |
| 4 | 2026-09-13 | 3124b82 | Creacion de jeraquia y encabezados para caso 1 | Caso 1 | h1/h2/h3 y secciones base |
| 5 | 2026-09-13 | f2c6538 | Creacion de jeraquia y encabezados para caso 2 | Ambos | h1/h2/h3 y ajustes en caso 1 |
| 6 | 2026-09-14 | 141f72f | Estructura con IDs necesarios para CSS en caso 1 | Caso 1 | IDs para anclas y estilos |
| 7 | 2026-09-14 | 90d6b28 | Creacion de menu con nav y seccion Ahora para caso 2 | Ambos | Nav de caso 2 y ajustes en caso 1 |
| 8 | 2026-09-14 | d364a49 | Creacion de la secion Agenda en HTML semantico para caso 1 | Caso 1 | Sección Agenda |
| 9 | 2026-09-14 | 56201d8 | Agregar placeholder para imagenes con alt descriptivo en caso 1 | Caso 1 | Imágenes con alt |
| 10 | 2026-09-14 | a25b532 | Agregar contraste y CSS basico para caso1 | Caso 1 | Paleta de color y contraste |
| 11 | 2026-09-16 | aa6b6e4 | Creacion de CSS base para caso2 | Caso 2 | CSS base con variables |
| 12 | 2026-09-18 | 525a5db | Creacion de Misiones y Resumen con HTML semantico y su respectivo CSS con Flex | Ambos | Misiones/Resumen en caso 1, imágenes SVG |
| 13 | 2026-09-18 | 67ba691 | Ajuste en alertas y media CSS para caso 1 | Caso 1 | Alertas y media queries |
| 14 | 2026-09-19 | 7f87b8d | Modificar caso 2 y agregar CSS en formato distinto al caso 1 | Caso 2 | CSS con variables consistentes |
| 15 | 2026-09-20 | 1bd06f1 | Agregar 2 alertas adicionales en caso 1 | Caso 1 | Alertas informativa y de retraso |
| 16 | 2026-09-20 | e3b4012 | Creacion de Readme segun consigna y ajustes en CSS | Ambos | README y ajustes de estilos |
| 17 | 2026-09-20 | 2386299 | Creacion de Readme, no se commit anteriormente | Ambos | README agregado al control de versiones |
| 18 | 2026-09-20 | 61418ca | Remover imagen cabecera en caso 1 y ajustes CSS | Caso 1 | Se elimina imagen de cabecera |
| 19 | 2026-09-20 | 1993b13 | Ajustes de variables globales para evitar usar valores estaticos en CSS | Ambos | Variables globales CSS |
| 20 | 2026-09-20 | 616be95 | Ajustes en colores, contraste y escritorio para caso 1 | Caso 1 | Refinamiento de paleta y responsive |
| 21 | 2026-09-20 | ee7bffd | Ajustes en colores, contraste y escritorio para caso 2 | Caso 2 | Refinamiento de paleta y responsive |
| 22 | 2026-09-20 | 3bed263 | Ajustes en colores, contraste y escritorio para CSS de caso 1 | Caso 1 | Ajustes finales de contraste CSS |
| 23 | 2026-09-20 | ed7a894 | Cambios en readme y agregar imagen en header para caso2 |  Ambos | README ajustes y agregar una imagen a caso 2 |
| 24 | 2026-09-20 | 5486ed0 | Cambios en el nav de caso 2 para evitar hacer control, y no puder usar JS | Caso2 | Cambiar el nowrap en nav, para mejorar experiencia de usuario |