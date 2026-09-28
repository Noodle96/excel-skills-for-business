# Módulo 6 · Excel Data Charts

> Los videos de esta semana, según los archivos en `material_curso/modulo-06/`, son: **V01 Basic Chart Types · V02 Move and Resize · V03 Change Styles and Type · V04 Modify Chart Elements**. Las secciones 1-4 siguen ese orden y están redactadas con conocimiento general (el nombre de archivo confirma el tema, no el contenido exacto del video), apoyadas en la terminología real de la lectura "Week 6: Toolbox".

## 1. Tipos básicos de gráficos (V01)

Para crear uno: seleccionar los datos (incluyendo encabezados) > pestaña **Insertar** > elegir el tipo de gráfico.

| Tipo | Cuándo usarlo |
|---|---|
| **Columnas** | Comparar valores entre categorías (ventas por región) |
| **Barras** | Igual que columnas, pero con las categorías en el eje vertical; útil con nombres largos |
| **Líneas** | Mostrar la evolución en el tiempo (ventas mes a mes) |
| **Circular (pie)** | Mostrar la proporción de cada parte respecto al total; funciona mejor con pocas categorías y una sola serie |

Atajos de creación rápida: `F11` inserta el gráfico en una hoja de gráfico nueva; `Alt + F1` lo inserta como gráfico incrustado en la hoja actual.

## 2. Mover y cambiar el tamaño (V02)

- **Mover**: arrastrar el gráfico por su área (no por sus elementos internos) a otra posición de la hoja.
- **Redimensionar**: arrastrar los tiradores de las esquinas o bordes; mantener `Shift` mientras se arrastra una esquina conserva las proporciones.
- **Herramienta Mover gráfico** (pestaña Diseño de gráfico): mueve el gráfico a una **hoja de gráfico** propia o de vuelta a una hoja de datos como objeto incrustado.
- Ojo: la acción de *Mover gráfico* **no se puede deshacer** con `Ctrl + Z` (ya se mencionaba en la lectura de la semana 1).

## 3. Cambiar estilo y tipo (V03)

- **Estilos y colores**: pestaña Diseño de gráfico > galería de estilos, y "Cambiar colores" para aplicar otra paleta.
- **Cambiar tipo de gráfico**: sin recrearlo desde cero; el gráfico conserva sus datos y solo cambia su representación (ej. de columnas a líneas).
- **Cambiar entre filas y columnas**: intercambia qué se usa como series y qué como categorías.
- **Seleccionar datos**: permite añadir, quitar o editar las series y el rango del que provienen.

## 4. Modificar elementos del gráfico (V04)

Elementos habituales que se pueden mostrar, ocultar o formatear: **título del gráfico**, **títulos de ejes**, **etiquetas de datos**, **leyenda**, **líneas de cuadrícula** y los propios ejes. Se gestionan con el botón `+` junto al gráfico, desde Diseño de gráfico > Agregar elemento de gráfico, o con clic derecho sobre el elemento > Dar formato (abre el panel lateral de formato).

## 5. Atajos de teclado

| Atajo | Acción |
|---|---|
| `F11` | Insertar un gráfico nuevo, en una hoja de gráfico, a partir de la selección |

> **Mac**: para usar las teclas de función hay que pulsar además `fn`. Se puede evitar en Preferencias del Sistema > Teclado > "Usar las teclas F1, F2, etc. como teclas de función estándar". Además, `F11` puede estar asignada por defecto a "Mostrar escritorio" (Mission Control); si pasa eso, se desactiva en Preferencias del Sistema > Teclado > Funciones rápidas > Mission Control > desmarcar "Mostrar escritorio". Precaución: reasignar teclas del sistema anula su comportamiento habitual.

## 6. Terminología

- **Área del gráfico (Chart Area)**: todo el gráfico; incluye sus elementos típicos: series de datos, ejes, títulos y leyendas.
- **Hoja de gráfico (Chart Sheet)**: hoja que contiene únicamente un gráfico. Para pasar un gráfico a una hoja de gráfico se usa la herramienta **Mover gráfico** en la pestaña Gráficos/Diseño. Cuando el gráfico aparece en una hoja junto con otros datos, se dice que está **incrustado**.
- **Etiqueta de datos (Data Label)**: información adicional asociada a un punto de datos; a menudo muestra el valor real (la altura de una barra o el porcentaje de una porción). No siempre se muestran en el gráfico.
- **Puntos de datos (Data Points)**: valores de celdas de la hoja, representados como barras, líneas, columnas, porciones u otras formas del gráfico.
- **Serie de datos (Data Series)**: conjunto de valores relacionados representados en el gráfico.
- **Gráfico incrustado (Embedded Chart)**: el gráfico se incrusta como objeto en la hoja, junto a los datos de los que proviene. Se puede imprimir como parte de esa hoja o como elemento independiente. Ideal cuando los datos deben mostrarse en el contexto de los datos de la hoja.
- **Líneas de cuadrícula (Gridlines)**: líneas que cruzan el área de trazado y ayudan a la vista a seguir los valores de los ejes.
- **Leyenda (Legend)**: se muestra fuera de la cuadrícula delimitada por los ejes; es una clave, en un pequeño recuadro junto al gráfico, que indica qué colores y símbolos representan cada serie de datos.
- **Área de trazado (Plot Area)**: la parte del gráfico delimitada por los ejes vertical y horizontal y sus lados opuestos.
- **Ejes X e Y**: el eje X va por la parte inferior y suele usarse para categorías; el eje Y va por un lateral y se usa para los valores de las series. En los gráficos de barras los ejes están invertidos.

## 7. Ninja tips de la semana

**No subestimes el clic derecho.** Hacer clic derecho sobre un gráfico permite modificar prácticamente cualquier parte y aspecto de él, con acceso rápido y cómodo a la mayoría de opciones.

**Transponer columnas a filas (y viceversa).** A veces se arma un conjunto de datos y luego se prefiere que lo que está en columnas aparezca en filas (o al revés). Solución: copiar la fila o columna a transponer, clic derecho en la celda de destino > **Pegado especial**, marcar la casilla **Transponer** (abajo del cuadro) y aceptar.

## 8. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Compartir gráficos**: el temario oficial de la especialización menciona compartirlos, aunque no hay un video específico entre los archivos de esta semana. Lo habitual: copiar el gráfico y pegarlo en Word o PowerPoint; al pegar se elige entre mantenerlo **vinculado** al libro de Excel (se actualiza si cambian los datos) o **incrustado/como imagen** (queda fijo). También se puede guardar como imagen con clic derecho > Guardar como imagen.
- **Elegir bien el tipo de gráfico**: circular solo para partes de un todo con pocas categorías (unas 5-6 como máximo); líneas para tendencias en el tiempo; columnas/barras para comparar categorías. Evitar 3D y efectos decorativos que distorsionan la lectura de los valores.
- **Gráficos recomendados**: Insertar > Gráficos recomendados sugiere tipos según la forma de los datos seleccionados, útil como punto de partida.
- **Lo que viene más adelante**: en el curso Intermediate I (módulo 4, *Summarising Data*) se amplía con **sparklines** (mini-gráficos dentro de una celda), gráficos avanzados y líneas de tendencia; en el módulo 6 de ese mismo curso, gráficos dinámicos y slicers.
- **Buena práctica de legibilidad**: siempre incluir título y títulos de ejes con unidades, y quitar elementos que no aporten (leyenda con una sola serie, cuadrículas excesivas).

---

### Práctica
Archivos del curso: `material_curso/modulo-06/` (esta semana no hay archivos `Soln`; `W06-workbook` y `W06-Challenge-1` son ejercicios de cierre). Ojo: el video 3 aparece en dos archivos casi idénticos (`W06-V03 Change Styles and Type` y `W06-V03-Change-Styles-and-Type`), probablemente una versión duplicada.

### Fuente
Lectura "Week 6: Toolbox" del curso (atajo F11, terminología de gráficos, ninja tips de clic derecho y transposición — secciones 5, 6 y 7). Secciones 1-4 se basan en los nombres de los videos, redactadas con conocimiento general. Sección 8 es complemento propio.
