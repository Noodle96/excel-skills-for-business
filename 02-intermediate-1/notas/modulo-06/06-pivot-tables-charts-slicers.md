# Módulo 6 · Pivot Tables, Charts and Slicers

> Los videos de esta semana, según los archivos en `material_curso/modulo-06/`, son: **V1 Create PivotTables · V2 Modify PivotTables · V3 Group Sort Filter · V4 Report Filters · V5 Pivot Charts · V6 Slicers**. La terminología y el ninja tip (secciones 7 y 8) son material real de la lectura "Week 6: Toolbox"; las secciones 1-6 están redactadas con conocimiento general (el nombre de archivo confirma el tema, no el contenido exacto del video), así que algún detalle podría no calzar 100% con lo que muestra el curso. La sección 10 sale de los problemas de práctica que se resolvieron en conversación.

## 1. Crear tablas dinámicas (V1)

Una **tabla dinámica (PivotTable)** resume un conjunto de datos grande: es parecida a una tabulación cruzada de otros programas.

1. Hacer clic en cualquier celda de los datos de origen (idealmente una **tabla**, Módulo 5: así la tabla dinámica incluye las filas nuevas).
2. Insertar > **Tabla dinámica**; elegir si va en una hoja nueva o en una existente.
3. En el panel de campos, **arrastrar los campos** a las cuatro áreas:
   - **Filtros**: filtran toda la tabla (filtro de informe).
   - **Columnas** y **Filas**: las categorías que se muestran en cada eje.
   - **Valores**: lo que se calcula (suma, conteo, promedio...).

## 2. Modificar tablas dinámicas (V2)

- **Agregar, quitar o mover campos** arrastrándolos entre áreas; los cambios se reflejan al instante.
- **Configuración de campo de valor** (clic derecho sobre un valor): cambiar la función de resumen (**Suma, Cuenta, Promedio, Máx, Mín**...), el **formato de número** y el **nombre personalizado** de la columna.
- **Mostrar valores como**: presentar el mismo valor como **% del total general**, % del total de la columna o de la fila, diferencia con otro elemento, etc.
- **Actualizar** (`Alt + F5`): la tabla dinámica **no se recalcula sola** cuando cambian los datos de origen; hay que actualizarla.
- **Diseño y estilo**: pestaña Diseño permite elegir diseño compacto, esquema o tabular, mostrar/ocultar totales y subtotales, y aplicar estilos.

## 3. Agrupar, ordenar y filtrar (V3)

- **Agrupar**: fechas (por meses, trimestres, años), números (en rangos de un tamaño dado) o elementos de texto seleccionados a mano (clic derecho > Agrupar).
- **Ordenar**: por etiqueta (A→Z) o por valor (mayor a menor), desde la flecha del campo o con clic derecho sobre un valor.
- **Filtrar**: la flecha de cada campo de fila/columna ofrece tres tipos de filtro (ver sección 10): marcar casillas, **filtros de etiqueta** y **filtros de valor** (incluido "Diez mejores").

## 4. Filtros de informe (V4)

Un campo arrastrado al área **Filtros** filtra toda la tabla dinámica por uno o varios de sus elementos (se pueden seleccionar varios activando "Seleccionar varios elementos").

**Mostrar páginas de filtro de informe**: (Análisis de tabla dinámica > Opciones > Mostrar páginas de filtro de informe) crea **una hoja nueva por cada elemento** del campo de filtro, cada una con la tabla dinámica ya filtrada por ese elemento. Útil, por ejemplo, para generar un informe por estado o por vendedor de una sola vez.

## 5. Gráficos dinámicos (V5)

Un **gráfico dinámico (PivotChart)** es un gráfico basado en una tabla dinámica: combina la capacidad de resumen y filtro de la tabla dinámica con el poder de visualización de los gráficos de Excel.

- Se crea desde Análisis de tabla dinámica > Gráfico dinámico, o Insertar > Gráfico dinámico, o seleccionando la tabla dinámica y eligiendo un tipo de gráfico.
- Está **vinculado** a la tabla dinámica: si cambias los campos de la tabla, el gráfico cambia; y los botones de campo del gráfico permiten filtrar y reordenar, y esos cambios se reflejan en la tabla.
- Todo lo visto en Essentials (tipos de gráfico, estilos, elementos) se aplica igual al gráfico dinámico.

## 6. Segmentaciones de datos — Slicers (V6)

**Slicer**: una forma cómoda de **filtrar una tabla dinámica** con botones visuales, que además muestran qué filtros están activos en ese momento.

- Se crean desde Análisis de tabla dinámica > **Insertar segmentación de datos**, eligiendo el/los campo(s) por los que filtrar.
- Se seleccionan varios elementos con `Ctrl + clic`; el icono con una X (esquina del slicer) **borra el filtro**.
- Un mismo slicer se puede **conectar a varias tablas y gráficos dinámicos** (clic derecho > Conexiones de informe) para filtrarlos todos a la vez.
- Al combinar tablas dinámicas, gráficos dinámicos y slicers se arma un **dashboard** interactivo (ver terminología).

## 7. Terminología

- **Dashboard (panel)**: pantalla de resumen diseñada para dar al usuario una vista rápida de los datos. Suele contener gráficos y tablas de números, y a veces algo de interactividad para elegir los puntos de interés. Normalmente se actualiza solo para mostrar la información más reciente o relevante.
- **PivotChart (gráfico dinámico)**: un gráfico basado en una tabla dinámica. Combina el poder de resumen y filtro de la tabla dinámica con el poder de visualización de los gráficos de Excel.
- **PivotTable (tabla dinámica)**: una forma potente y cómoda de resumir datos en Excel. Similar a una tabulación cruzada en otros programas.
- **Slicer (segmentación de datos)**: una forma cómoda de filtrar una tabla dinámica; además muestra qué filtros están en uso.
- **Dataset (conjunto de datos)**: conjunto de datos que contiene campos (columnas) que suelen tener distinto formato, por ejemplo texto, número, moneda.
- **Field List (lista de campos)**: la lista de todas las columnas del conjunto de datos, por ejemplo Account Manager, Total Revenue, Cost.
- **Field (campo)**: uno de los elementos de la lista de campos (una de las columnas del conjunto de datos, ej. Account Manager).
- **Data Source (origen de datos)**: de dónde vienen los datos, es decir, la tabla original de la que depende la tabla dinámica.

## 8. Ninja tip: dar formato a las celdas vacías de una tabla dinámica

Clic derecho sobre cualquier celda de la tabla dinámica > **Opciones de tabla dinámica** > pestaña **Diseño y formato** > en **Formato**, elegir qué mostrar **en las celdas vacías** — por ejemplo un guion (`-`). El guion es solo un ejemplo razonable; se puede poner cualquier otra cosa que resulte más agradable.

## 9. Atajos de teclado

> El toolbox de esta semana no trae atajos; estos son de conocimiento general.

| Atajo | Acción |
|---|---|
| `Alt + F5` | Actualizar la tabla dinámica seleccionada |
| `Ctrl + Alt + F5` | Actualizar todas las conexiones/tablas dinámicas del libro |
| `Ctrl + clic` (en un slicer) | Seleccionar varios elementos a la vez |

## 10. Lo aprendido al practicar (problemas propios)

Para aplicar el módulo se resolvieron tres problemas sobre un conjunto de datos de pedidos (columnas: Order No, Order Date, Order Year, State, Customer Type, Account Manager, Order Priority, Product Name, Product Category, Product Container, Ship Mode, Ship Date, Cost Price, Retail Price, Profit Margin, Order Quantity, Sub Total, Discount %, Discount $, Order Total, Shipping Cost, Total).

| Nivel | Enunciado | Técnicas |
|---|---|---|
| Fácil | Total de `Order Total` por `Product Category`, de mayor a menor y con formato de moneda sin decimales | Valores (Suma), ordenar, formato de número |
| Medio | `Product Category` en filas y `Order Year` en columnas, suma de `Order Total`, `Customer Type` como filtro de informe y `Profit Margin` como promedio con 1 decimal, renombrando ambos campos de valor | Filas + columnas, filtro de informe, varios campos de valor con distinta función, nombre personalizado |
| Difícil | Por `Account Manager`: conteo de `Order No`, suma de `Total`, promedio de `Discount %` y suma de `Shipping Cost`; solo los 5 mejores por suma de `Total`; segunda columna de `Total` como % del total general; `Order Priority` y `State` en el filtro de informe y una hoja por `State` | Filtro de valor Top 10, Mostrar valores como, páginas de filtro de informe |

### Gotchas descubiertos

- **Los filtros de un campo son de tres tipos**, y no todos dependen de marcar casillas: **marcar elementos** a mano, **filtros de etiqueta** (según el texto del propio campo: empieza por, contiene...) y **filtros de valor** (según un campo de valores: mayor que, entre, diez mejores...).
- **Top N es un filtro de valor sobre el campo de filas**: flecha de Account Manager > Filtros de valor > Diez mejores > Superiores / 5 / Elementos / por *Suma de Total*. No hace falta arrastrar nada nuevo, pero **el campo de valores debe estar ya en el área Valores**, porque la lista "por" solo ofrece los campos que están ahí.
- **Con un filtro de valor, el Total general solo suma los elementos visibles.** Por eso una columna "% del total general" calculada con el Top 5 aplicado suma 100% entre esos 5, no sobre todos los managers.
- **Un mismo campo puede aparecer varias veces en Valores** con configuraciones distintas: arrastrar `Total` otra vez crea "Suma de Total2", y a esa copia se le aplica clic derecho > Mostrar valores como > % del total general y un nombre personalizado (ej. "Share %"). No hace falta otra tabla ni fórmulas.
- **`Total`, `Order Total` y `Sub Total` son columnas distintas**: hay que revisar cuál pide cada enunciado.
- **Contar `Order No` cuenta filas**, no importes: para "cantidad de pedidos" se usa **Cuenta**, no Suma.
- **`Order Year` es una columna aparte de `Order Date`**: para ver datos por año no hace falta agrupar fechas.

## 11. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Escala de tiempo (Timeline)**: un tipo de slicer exclusivo para campos de fecha, que permite filtrar por meses, trimestres o años arrastrando una barra.
- **Campos calculados**: Análisis de tabla dinámica > Campos, elementos y conjuntos > Campo calculado, para crear un campo nuevo a partir de una fórmula con otros campos (ej. ganancia = Total − Cost), sin añadir una columna a los datos de origen.
- **`GETPIVOTDATA`**: función que Excel escribe sola cuando haces clic en una celda de una tabla dinámica desde una fórmula; extrae un valor concreto de la tabla por nombre de campo.
- **Tabla dinámica vs. `SUMIFS`** (Módulo 4): `SUMIFS` da un valor concreto con criterios que tú escribes; la tabla dinámica resume todas las combinaciones de una vez y se reorganiza arrastrando campos. Para responder una pregunta puntual conviene `SUMIFS`; para explorar, la tabla dinámica.
- **Actualización y origen de datos**: si el origen es una tabla de Excel, las filas nuevas se incluyen al actualizar; si es un rango normal, hay que ampliar el origen manualmente (Cambiar origen de datos).
- **Lo que viene después**: los dashboards con gráficos dinámicos y slicers se retoman en el curso Advanced (módulo *Building Professional Dashboards*), y modelos con varias tablas en Power Pivot, que queda fuera de esta especialización.

---

### Práctica
Archivos del curso: `material_curso/modulo-06/`. Esta semana ningún video trae versión `Soln`; los videos 3 y 4 traen además un archivo de tarea (`W6_GroupSortFilter_task.xlsx`, `W6_ReportFilters_task.xlsx`). Hay un reto de práctica en `challenge/` (sin solución incluida); la carpeta `assessment/` está vacía.

### Fuente
Lectura "Week 6: Toolbox" del curso (terminología y ninja tip de celdas vacías — secciones 7 y 8). Secciones 1-6 y 9 se basan en los nombres de los videos, redactadas con conocimiento general. Sección 10 sale de los problemas de práctica resueltos en conversación. Sección 11 es complemento propio.
