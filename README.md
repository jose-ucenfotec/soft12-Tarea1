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

soft12-Tarea1/
├── README.md
├── caso1/
│ ├── index.html
│ ├── css/estilos.css
│ └── img/ (estacion.svg, mapa.svg)
└── caso2/
├── index.html
├── css/estilos.css
└── img/


## Decisiones de diseño

- **Etiquetas semánticas:** `header` para los datos generales, `nav` para la
  navegación por anclas, `main` para el contenido principal, `section` por
  cada bloque temático (resumen, misiones, equipos, alertas, agenda / ahora,
  próximas, cambios, servicios, escenarios, información), `article` para cada
  elemento independiente y repetible (una misión, un equipo, una alerta, una
  actividad, un escenario), y `footer`/`address` para los datos de contacto.
- **Jerarquía de encabezados:** `h1` es el nombre del proyecto; `h2` titula
  cada `section` (vinculado con `aria-labelledby`); `h3` titula cada elemento
  individual dentro de una sección.
- **Accesibilidad:** `lang="es"` en el documento, `alt` descriptivo en
  imágenes de contenido y `alt=""` en las decorativas, `aria-labelledby` en
  cada sección, `aria-label` en los botones de cerrar alerta, `aria-current`
  en el enlace de navegación activo, y los estados (pendiente, en progreso,
  completada, suspendida / en este momento) se comunican con texto además
  de color.
- **Modelo de caja:** `box-sizing: border-box` global; `padding` interno en
  tarjetas y artículos; `margin-bottom` para separar secciones; `max-width`
  en el contenedor principal para no estirarse en pantallas grandes.
- **Posicionamiento:** `position: sticky` en la navegación (se usa porque el
  usuario necesita volver a las secciones sin perder el scroll); `position:
  absolute` dentro de un contenedor `position: relative` para superponer la
  etiqueta de prioridad (caso 1) y la etiqueta "En este momento" (caso 2)
  sobre la tarjeta sin sacarla del flujo del resto del "layout".
- **Cascada y especificidad:** clases con convención BEM
  (`bloque__elemento--modificador`) para evitar selectores anidados y
  sobrescrituras; los modificadores de estado/prioridad heredan de una clase
  base y solo cambian color/fondo.
- **Flexbox:** en la navegación, en los indicadores/servicios (fila que
  envuelve), y en la organización interna de tarjetas (agenda, próximas
  actividades, equipos).
- **CSS Grid:** en el layout principal de escritorio mediante
  `grid-template-areas`, y en las listas de tarjetas (misiones, equipos,
  escenarios) con `auto-fit`/`auto-fill` y `minmax()`.
- **Cambio de layout por tamaño:** en teléfono todo se apila en una columna
  siguiendo el orden de prioridad de lectura; en tableta se ajustan
  distribuciones intermedias; en escritorio el contenedor principal pasa a
  grid multi-columna con zonas nombradas.
- **Media queries:** `min-width: 601px` (tableta) y `min-width: 1025px`
  (escritorio).
- **Unidades relativas:** `rem` en tipografía y espaciados, `%`/`fr` en
  anchos de columnas y contenedores; solo se usan `px` en bordes y en el
  radio de píldora (999px), que son valores absolutos intencionales.
- **Variables CSS:** `--color-primario`/`--color-acento` para la identidad
  visual de cada caso, `--color-fondo`/`--color-superficie`/`--color-texto`
  para mantener el contraste consistente, `--espacio-2/3/4` como escala de
  espaciado reutilizable y `--radio` para el redondeo de tarjetas y botones.


## Instrucciones para abrir cada caso
Cada caso es independiente y no requiere servidor. Abrir directamente
`caso1/index.html` o `caso2/index.html` en cualquier navegador.

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