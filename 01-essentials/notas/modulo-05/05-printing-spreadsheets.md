# Módulo 5 · Printing Excel Spreadsheets


## 1. Vista previa de impresión (V01)

- Abre la vista previa con `Ctrl + P` (diálogo de impresión, que incluye la vista previa a la derecha) o `Ctrl + F2` (ventana de vista previa directa).
- Sirve para revisar cómo quedará cada página **antes** de gastar papel: qué datos caen en cada hoja, si algo se corta, cuántas páginas resultan.
- Desde ahí mismo se ajustan casi todas las opciones de la sección 2.

## 2. Opciones de impresión (V02)

- **Qué imprimir**: hojas activas, todo el libro, o solo la selección actual.
- **Orientación**: vertical (portrait) u horizontal (landscape); la horizontal suele servir mejor para tablas anchas.
- **Tamaño de papel** (A4, Carta, etc.) y **márgenes** (normal, estrecho, ancho, o personalizados).
- **Escala**: reducir/ampliar el contenido, o usar "Ajustar hoja en una página" / "Ajustar todas las columnas en una página" para evitar que una tabla se reparta en varias hojas.
- **Copias** y **intercalado** cuando se imprimen varias hojas o varias copias.

## 3. Saltos de página (V03)

**Page break**: los saltos de página dividen una hoja en páginas separadas para la impresión. Hay dos tipos:
- **Automáticos** (línea azul punteada): se ajustan solos según la orientación, los márgenes y la escala.
- **Manuales** (línea azul continua): los fijas tú para forzar dónde empieza una página nueva.

La herramienta está en la pestaña **Diseño de página**. La **vista de salto de página** se activa desde la pestaña **Vista** o desde la barra de estado, y permite arrastrar las líneas azules para reubicar los cortes.

## 4. Títulos de impresión (V04)

**Print titles**: herramienta de la pestaña **Diseño de página**. En el cuadro de diálogo se define qué filas/columnas se repiten arriba y a la izquierda en **cada página impresa** (típicamente la fila de encabezados de una tabla larga). Funciona parecido a Inmovilizar paneles, con la diferencia de que Inmovilizar paneles no tiene ningún efecto en la página impresa.

**Print area (área de impresión)**: útil cuando se trabaja con conjuntos de datos grandes y solo se quiere imprimir una sección. Se define desde Diseño de página > Área de impresión > Establecer área de impresión (también está disponible en el cuadro de diálogo de configuración de página, pestaña Hoja).

## 5. Encabezados y pies de página (V05)

- Son texto que se repite en la parte superior (encabezado) o inferior (pie) de cada página impresa. Se editan desde Insertar > Encabezado y pie de página, o desde la vista Diseño de página.
- Cada uno se divide en tres zonas: izquierda, centro y derecha.
- Se pueden insertar elementos automáticos: número de página, total de páginas, fecha, hora, nombre del archivo, nombre de la hoja.
- Existe la opción de usar una **primera página diferente** o **páginas pares/impares distintas**.

## 6. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + P` (Mac: `Cmd + P`) | Abrir el diálogo de impresión |
| `Ctrl + F2` | Abrir la ventana de vista previa de impresión |
| `Ctrl + X` (Mac: `Cmd + X`) | Cortar la selección |
| `Ctrl + C` (Mac: `Cmd + C`) | Copiar la selección |
| `Ctrl + V` (Mac: `Cmd + V`) | Pegar datos (de una acción previa de cortar/copiar) |

## 7. Seleccionar celdas con teclado y mouse

- **Tecla `Shift`**: con clic en la primera celda, mantener `Shift` y hacer clic en la última, se selecciona todo el rango entre ambas.
- **Tecla `Ctrl`**: mantenida mientras se hace clic en celdas, columnas o filas, permite seleccionar varios elementos **no contiguos** (que no están uno junto al otro).

## 8. Ninja tip: tamaño de fuente vs. control de zoom

Es tentador agrandar la fuente de una hoja para ver mejor en pantalla, pero el tamaño de fuente es relativo a la página impresa: lo que se veía bien en pantalla queda desproporcionado al imprimir. Para ampliar los datos en pantalla, conviene usar el **control deslizante de zoom** de la barra de estado.

## 9. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Guardar como PDF / imprimir a PDF**: en Archivo > Exportar > Crear documento PDF/XPS (o eligiendo "Microsoft Print to PDF" como impresora). Es la forma habitual de compartir un reporte con formato fijo sin depender de que el destinatario tenga Excel. Este punto figura en el temario oficial del módulo, aunque no hay un video específico entre los archivos de esta semana.
- **Vista Diseño de página**: (Vista > Diseño de página) muestra la hoja como páginas reales, con márgenes, encabezados y pies editables directamente.
- **Configurar página (Page Setup)**: el diálogo completo (flecha en la esquina del grupo "Configurar página") reúne márgenes, orientación, escala, encabezados/pies y hoja en un solo lugar, incluyendo opciones no visibles en la vista previa rápida.
- **Imprimir líneas de cuadrícula y encabezados de fila/columna**: por defecto no se imprimen; se activan en Diseño de página > Opciones de la hoja > Imprimir.
- **Centrar en la página**: en Configurar página > Márgenes, para centrar el contenido horizontal y/o verticalmente en la hoja.
- **Ajustar a una página de ancho**: en Escala, elegir "1 página de ancho por automático de alto" evita que una tabla ancha se corte hacia la derecha, sin forzar que todo entre en una sola página.

---

### Práctica
Archivos del curso: `material_curso/modulo-05/` (resolver los que no terminan en `Soln`; esta semana solo `V05 - Headers and Footers` trae versión `Soln`).

### Fuente
Lectura "Week 5: Toolbox" del curso (atajos, selección con teclado/mouse, terminología de Print titles/Print area/Page break, ninja tip — secciones 3, 4, 6, 7 y 8). Secciones 1, 2 y 5 se basan en los nombres de los videos, redactadas con conocimiento general. Sección 9 es complemento propio.
