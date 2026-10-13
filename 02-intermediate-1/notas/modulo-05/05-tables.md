# Módulo 5 · Tables

> Los videos de esta semana, según los archivos en `material_curso/modulo-05/`, son: **V1 Tables · V2 Working With Tables · V3 Sorting And Filtering · V4 Automation · V5 Convert Range And Subtotal**. Las secciones 6, 7 y 8 usan material real de la lectura "Week 5: Toolbox"; las secciones 1-5 están redactadas con conocimiento general (el nombre de archivo confirma el tema, no el contenido exacto del video), así que algún detalle podría no calzar 100% con lo que muestra el curso.

## 1. Crear tablas (V1)

Una **tabla** de Excel es un rango de datos al que se le da un tratamiento especial: se expande sola, tiene herramientas de orden y filtro integradas y se puede formatear con un clic.

- **Crear**: seleccionar cualquier celda dentro de los datos > `Ctrl + T` (o Insertar > Tabla). Excel detecta el rango y pregunta si la tabla tiene **encabezados** (marcar la casilla si la primera fila son los títulos de columna).
- **Pestaña Diseño de tabla** (aparece al seleccionar una celda de la tabla): permite cambiar el estilo, activar/desactivar **filas con bandas**, **primera/última columna resaltada**, **fila de totales**, y darle un **nombre** a la tabla (cuadro "Nombre de tabla" a la izquierda).
- Buena práctica: dar a cada tabla un nombre descriptivo (`Ventas`, `Empleados`) en vez de dejar `Tabla1`, porque ese nombre se usa luego en las fórmulas.

## 2. Trabajar con tablas (V2)

- **Agregar datos**: al escribir en la fila justo debajo de la tabla (o en la columna justo a la derecha), la tabla se **extiende automáticamente** para incluirla — y con ella el formato, los filtros y las fórmulas.
- **Referencias estructuradas**: en vez de `B2:B100`, las fórmulas pueden referirse a la columna por su nombre: `=SUM(Ventas[Importe])`. Se escriben solas al hacer clic sobre una columna de la tabla dentro de una fórmula. `[@Importe]` se refiere a "el valor de la columna Importe **en esta fila**".
- **Seleccionar partes de la tabla**: ver los atajos de la sección 6.

## 3. Ordenar y filtrar tablas (V3)

Cada encabezado de una tabla trae una **flecha desplegable** con las opciones de orden (A→Z, Z→A, por color) y filtro (por valores, por texto, por número, por fecha, por color). Se aplican a la tabla **como una unidad completa**: al ordenar por una columna, todas las filas se reordenan juntas, sin riesgo de desalinear los datos. Es el mismo concepto de ordenar/filtrar visto en Essentials (Módulo 4), pero integrado en la tabla.

## 4. Automatización (V4)

Lo que la tabla hace "sola" y que en un rango normal habría que hacer a mano:

- **Columnas calculadas**: al escribir una fórmula en una celda de una columna de la tabla, Excel la copia automáticamente a toda la columna (y a las filas nuevas que se agreguen después).
- **Expansión automática**: las fórmulas, gráficos o tablas dinámicas que apuntan a la tabla incluyen automáticamente las filas nuevas, sin tener que reajustar el rango.
- **Fila de totales**: se activa desde Diseño de tabla; permite elegir por columna la función de resumen (suma, promedio, conteo, etc.) en un menú desplegable.
- **Formato persistente**: las filas nuevas heredan el formato y las bandas de la tabla.

## 5. Convertir a rango y subtotales (V5)

- **Convertir a rango**: Diseño de tabla > **Convertir en rango**. La tabla vuelve a ser un rango normal: se conserva el formato, pero se pierden las funciones propias de la tabla (expansión automática, referencias estructuradas — que se convierten en referencias normales de celda).
- **Por qué convertir**: la herramienta **Subtotal** (Datos > Subtotal) **no está disponible dentro de una tabla**, solo sobre un rango normal.
- **Cómo usar Subtotal**:
  1. **Ordenar** primero los datos por la columna por la que se quiere agrupar (ej. Región).
  2. Datos > Subtotal.
  3. Elegir: **Para cada cambio en** (la columna de agrupación), **Usar función** (Suma, Promedio, Contar...) y **Agregar subtotal a** (la(s) columna(s) a resumir).
  4. Excel inserta filas de subtotal y crea un **esquema** con niveles (1, 2, 3) para colapsar/expandir los grupos.
  5. Para quitarlos: Datos > Subtotal > **Quitar todos**.

## 6. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + T` (Mac: `Cmd + T` o `Ctrl + T`) | Insertar tabla |
| `Shift + Espacio` | Seleccionar la fila de la tabla |
| `Ctrl + Espacio` | Seleccionar la columna de la tabla |
| `Ctrl + A` (Mac: `Cmd + A`) | Seleccionar la tabla |
| `Ctrl + .` | Moverse entre las esquinas de la tabla |

## 7. Terminología: rangos con nombre vs. tablas

Los rangos con nombre (Módulo 3) y las tablas se superponen en funcionalidad y uso, pero tienen diferencias clave:

| Característica | Rangos con nombre | Tablas |
|---|---|---|
| **Extensión** | **No** se extienden automáticamente en los bordes de su rango | **Se extienden automáticamente** para incluir las filas y columnas que se agregan |
| **Comportamiento en fórmulas** | Se comportan como una referencia **absoluta**: al copiar la fórmula a otra columna, la referencia al rango con nombre **no cambia** | Las **referencias estructuradas** se comportan como una referencia **relativa**: al copiar la fórmula a otra columna, la referencia estructurada **cambia** |
| **Formato** | **No** tienen herramientas de formato | Se pueden formatear **fácilmente** |
| **Orden y filtro / trabajo conjunto** | Un rango con nombre **no conoce** a otros rangos con nombre — no trabajan juntos | Ofrecen una gama de herramientas de **orden y filtro** que funcionan con la tabla como una unidad completa |

## 8. Ninja tip: mover columnas y filas dentro de una tabla

Se pueden reorganizar columnas y filas de una tabla arrastrándolas:

1. **Seleccionar** la columna que se quiere mover.
2. Llevar el mouse al borde **izquierdo o derecho** de esa selección (según hacia dónde se quiera mover) hasta que aparezca el **icono de mover**.
3. **Hacer clic y arrastrar** hasta la posición deseada: una **barra vertical** indica en qué borde de columna quedará al soltar.

Una fila se mueve con un proceso similar.

## 9. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Segmentaciones de datos (slicers) en tablas**: Diseño de tabla > Insertar segmentación de datos agrega botones visuales para filtrar la tabla — se ven con más detalle en el Módulo 6 junto con las tablas dinámicas.
- **Quitar duplicados**: Diseño de tabla > Quitar duplicados elimina filas repetidas según las columnas que se elijan comparar.
- **Más elementos de referencias estructuradas**: `Tabla[#Encabezados]` (la fila de títulos), `Tabla[#Totales]` (la fila de totales), `Tabla[#Datos]` (solo los datos) y `Tabla[[#Esta fila],[Columna]]` (equivale a `[@Columna]`).
- **Convención de nombres**: muchos usuarios anteponen `tbl` al nombre (`tblVentas`) para distinguir a simple vista una tabla de un rango con nombre en una fórmula.
- **Tablas como fuente de datos dinámica**: una tabla dinámica, un gráfico o un rango con nombre dinámico que apunte a una tabla crece automáticamente con ella — por eso hoy las tablas suelen reemplazar a los rangos con nombre dinámicos creados con `OFFSET`.
- **Cuándo no conviene una tabla**: con fórmulas 3D (Módulo 1) o la herramienta Consolidar es más común trabajar con rangos normales; y las celdas combinadas no se pueden usar dentro de una tabla.

---

### Práctica
Archivos del curso: `material_curso/modulo-05/`. Esta semana ningún video trae versión `Soln`; el video 3 trae además un archivo de tarea (`W5_SortingAndFiltering_task.xlsx`). Hay un reto de práctica en `challenge/` (sin solución incluida); la carpeta `assessment/` está vacía.

### Fuente
Lectura "Week 5: Toolbox" del curso (atajos, comparación entre rangos con nombre y tablas, ninja tip de mover columnas y filas — secciones 6, 7 y 8). Secciones 1-5 se basan en los nombres de los videos, redactadas con conocimiento general. Sección 9 es complemento propio.
