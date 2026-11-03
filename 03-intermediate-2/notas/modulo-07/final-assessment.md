# Assessment final del curso (Intermediate II) — habilidades a dominar

Este documento **no guarda respuestas, resultados numéricos ni puntajes**: toma cada pregunta como una **habilidad** que hay que saber ejecutar sin ayuda, con el procedimiento para hacerla, para poder repasarla y practicarla después. Sirve además como repaso de todo el curso: cada habilidad apunta al módulo donde se vio.

**Cómo es el assessment**: un solo libro de una empresa de planificación de eventos que organiza una **cena benéfica**, con 30 preguntas que se resuelven en orden sobre tres hojas de trabajo, y que recorre **los seis módulos del curso**:

| Hoja | Qué se hace | Módulos que se repasan |
|---|---|---|
| `Client Database` | Validación de datos, fórmulas condicionales, búsqueda por rango, formato condicional y orden por color | 1, 2, 3 |
| `Attendees` | Funciones de búsqueda sobre una tabla de asistentes y un plan de asientos | 3 |
| `Cost Overview` | Auditoría de errores, Goal Seek, escenarios, Solver, protección y macros | 4, 5, 6 |

El libro trae tablas y **rangos con nombre** ya creados, y se pueden crear más. Las reglas de comprobación de errores deben estar en su configuración por defecto.

## Habilidades que pedía el assessment

| # | Qué se pedía | Habilidad | Módulo |
|---|---|---|---|
| 1-2 | Elegir el tipo de validación para el campo de nombre y para el ID de cliente | Tipos de validación de datos | [1](../modulo-01/01-data-validation.md) |
| 3 | Validar con listas de otra hoja, rodear los datos no válidos y corregirlos | Validación tipo lista + *Rodear con un círculo datos no válidos* | [1](../modulo-01/01-data-validation.md) |
| 4 | Impedir fechas futuras con validación personalizada | Validación personalizada con fórmula (`TODAY`) | [1](../modulo-01/01-data-validation.md) |
| 5 | Marcar como "New" a los clientes desde una fecha | `IF` con comparación de fechas | [2](../modulo-02/02-conditional-logic.md) |
| 6 | Marcar a los clientes con antigüedad o muchos eventos | `IF` + `OR` + `YEAR` | [2](../modulo-02/02-conditional-logic.md) |
| 7 | Asignar una categoría según tramos (Bronze, Silver, Gold, Platinum) | `VLOOKUP` por rango | [3](../modulo-03/03-automating-lookups.md) |
| 8 | Resaltar filas con dos criterios y ordenar por color | Formato condicional con fórmula + `AND`, orden por color | [1](../modulo-01/01-data-validation.md), [2](../modulo-02/02-conditional-logic.md) |
| 9 | Buscar el nombre de cada asistente, localizar el `#N/A` y eliminar ese registro | Búsqueda de coincidencia exacta; tratar un error | [3](../modulo-03/03-automating-lookups.md) |
| 10-11 | Buscar apellido, país y estado; ordenar por apellido | Búsqueda de coincidencia exacta + ordenar | [3](../modulo-03/03-automating-lookups.md) |
| 12 | Buscar la zona de asientos según estado **y** país | Búsqueda en dos sentidos | [3](../modulo-03/03-automating-lookups.md) |
| 13-14 | Encontrar de dónde viene un error y cuántas celdas afecta | *Rastrear error* (Trace Error) | [4](../modulo-04/04-formula-auditing-protection.md) |
| 15-16 | Corregir el error y revisar un triángulo verde | Comprobación de errores | [4](../modulo-04/04-formula-auditing-protection.md) |
| 17-18 | Contar dependientes y hallar el precedente directo de una celda | *Rastrear dependientes* y *precedentes* | [4](../modulo-04/04-formula-auditing-protection.md) |
| 19 | Hallar la cantidad mínima de invitados para ganancia cero | Goal Seek | [5](../modulo-05/05-data-modelling.md) |
| 20-21 | Crear tres escenarios con nombre, mostrar uno y resumirlos | Scenario Manager + Scenario Summary | [5](../modulo-05/05-data-modelling.md) |
| 22 | Minimizar el costo por invitado con restricciones y guardar el resultado como escenario | Solver | [5](../modulo-05/05-data-modelling.md) |
| 23-24 | Preparar y aplicar la protección de la hoja dejando editables dos celdas | Bloqueo/desbloqueo de celdas + *Proteger hoja* | [4](../modulo-04/04-formula-auditing-protection.md) |
| 25 | Decidir si unas macros necesitan referencias relativas | Absoluta vs relativa | [6](../modulo-06/06-recording-macros.md) |
| 26 | Grabar una macro que muestra un escenario y reconocer su línea de código | Grabadora + Scenario Manager | [6](../modulo-06/06-recording-macros.md) |
| 27 | Copiar una macro, renombrarla y cambiar el escenario en el editor | Editar macros en VBA | [6](../modulo-06/06-recording-macros.md) |
| 28 | Asignar macros a botones | Controles de formulario | [6](../modulo-06/06-recording-macros.md) |
| 29 | Eliminar una macro | Administrar macros | [6](../modulo-06/06-recording-macros.md) |
| 30 | Guardar el libro conservando las macros | Formatos habilitados para macros | [6](../modulo-06/06-recording-macros.md) |

## Cómo se hace cada una

### Validación de datos (1-4)
- **Elegir el tipo según la restricción**: *List* si los valores válidos se conocen de antemano (opciones cerradas, como una lista de clientes habituales); *Whole number* o *Decimal* para identificadores y cantidades numéricas; *Text length* para limitar el largo; *Custom* para reglas con fórmula; *Any value* no valida nada.
- **Lista desde otra hoja**: Datos > Validación de datos > Lista > Origen: el rango de la lista (mejor con un rango con nombre). Después, **Rodear con un círculo datos no válidos** marca lo que ya estaba mal; se corrigen eligiendo el valor de la lista.
- **Validación personalizada de fechas**:
  ```
  =E7<=TODAY()
  ```
  La fórmula se escribe pensando en la **primera celda del rango seleccionado**, con **referencia relativa** (sin `$`) para que se ajuste a cada fila, y `TODAY()` para la fecha de hoy.

### `IF` condicionales (5-6)
```
=IF(fecha_inicio >= fecha_límite, "New", "")
=IF(OR(YEAR(fecha_inicio) < 2013, eventos >= 15), "Yes", "")
```
- La fecha límite va en una celda de supuestos, con referencia **absoluta** (`$N$7`), para poder copiar la fórmula hacia abajo.
- `""` deja la celda vacía. `OR` basta con que una condición se cumpla; `YEAR` extrae el año de una fecha.

### `VLOOKUP` por rango (7)
```
=VLOOKUP(eventos, $tabla_de_tramos, 2)
```
- La tabla de tramos debe estar ordenada de **menor a mayor** por el límite inferior. Sin cuarto argumento (o con `TRUE`), usa el tramo **menor o igual**.
- La tabla va absoluta para copiar la fórmula.

### Formato condicional de toda la fila y orden por color (8)
1. Seleccionar toda la tabla (sin encabezados).
2. Inicio > Formato condicional > Nueva regla > **Usar una fórmula**.
3. Fórmula con `AND` que evalúa varias condiciones, con la **columna fija y la fila relativa** (referencia mixta: `$F7`):
   ```
   =AND($F7="GB", $I7="New")
   ```
4. Elegir el relleno. Después, Datos > Ordenar o el filtro de la columna > **Ordenar por color** > color de celda.

### Funciones de búsqueda de coincidencia exacta (9-11)
```
=VLOOKUP(ID, tabla_de_clientes, col_index, FALSE)
```
- Con **`FALSE`** (o `0`) para IDs únicos. Si el valor no existe, da `#N/A`: así se localiza el cliente que no está.
- Para eliminar un registro de una tabla **sin tocar datos al lado** (como un plan de asientos), se eliminan solo las **filas de la tabla** (clic derecho > Eliminar > Filas de la tabla), no las filas enteras de la hoja.
- Al **ordenar** una tabla con fórmulas de búsqueda, las referencias deben apuntar a celdas de la misma fila; con tablas y referencias relativas funciona sin problema.

### Búsqueda con dos criterios (12)
Dos opciones habituales, ambas del Módulo 3:
- `INDEX` con **dos `MATCH`** si el plan de asientos es una matriz con estados en las filas y países en las columnas:
  ```
  =INDEX(matriz_de_zonas, MATCH(estado, lista_de_estados, 0), MATCH(país, lista_de_países, 0))
  ```
- Una **columna auxiliar** que une estado y país (`=estado&país`) y un `VLOOKUP` exacto sobre esa clave, si el plan es una lista.

### Auditoría de errores (13-18)
- **Rastrear error**: seleccionar la celda con el error > Fórmulas > Comprobación de errores > **Rastrear error**. Las flechas **rojas** señalan las celdas afectadas; la celda donde nace el error es el origen. Se corrige el origen, y las demás se arreglan solas.
- **Triángulo verde**: seleccionar la celda y abrir el icono de advertencia para leer cuál es el problema (por ejemplo, una fórmula inconsistente con las vecinas, o un rango que no incluye celdas adyacentes). Se corrige según lo que indica.
- **Rastrear dependientes / precedentes**: Fórmulas > Auditoría de fórmulas. Las flechas **azules** salen de los precedentes hacia la celda y de ella hacia los dependientes. **Directos** son solo los del primer nivel; se cuentan con una sola pulsación. **Quitar flechas** las borra.

### Goal Seek (19)
Datos > Análisis de hipótesis > **Buscar objetivo**:
- *Set cell*: la celda de ganancia (con fórmula).
- *To value*: `0`, escrito a mano.
- *By changing cell*: la celda de invitados.
- Cuidado con el **valor inicial**: si la relación es una curva o tiene cortes, el resultado depende de él. El resultado puede salir con decimales (se pide como entero), y como cuenta de personas conviene **redondear hacia arriba**, porque con menos invitados habría pérdida.

### Scenario Manager y resumen (20-21)
1. Datos > Análisis de hipótesis > Administrador de escenarios > Agregar.
2. Nombre del escenario y **celda cambiante** (por ejemplo, la del proveedor), con el valor del escenario. El primero toma el **valor actual**.
3. Repetir para los demás.
4. **Mostrar** aplica uno a la hoja; **Resumen** crea la hoja con todos y la celda de resultado indicada.
- Los resultados que muestra el resumen salen del modelo en el momento de crearlo, así que conviene dejar el modelo en el estado correcto antes.

### Solver (22)
1. Datos > Solver.
2. **Set Objective**: la celda de costo por invitado → **Min**.
3. **Variable Cells**: las celdas que se pueden ajustar (varias, separadas por coma).
4. **Restricciones** con *Añadir*: mínimos (`>=`) y máximos (`<=`).
5. Método (en un modelo con divisiones o `IF`, normalmente GRG Nonlinear o Evolutionary; ver [Módulo 5](../modulo-05/05-data-modelling.md)).
6. Resolver → **Guardar escenario** (nombre pedido) → **Mantener la solución**.

### Protección (23-24)
1. **Antes de proteger**, desbloquear las celdas que sí deben poder editarse: clic derecho > Formato de celdas > pestaña **Proteger** > desmarcar **Bloqueada**. (Todas las celdas están bloqueadas por defecto, pero el bloqueo solo actúa cuando la hoja está protegida.)
2. Revisar > **Proteger hoja** protege el resto de la hoja.
- *Proteger libro* protege la estructura (hojas); *Cifrar con contraseña* impide abrir el archivo. No son lo mismo que proteger una hoja.

### Macros (25-30)
- **¿Relativas?** Una macro necesita referencias relativas cuando actúa en un lugar que cambia (la siguiente fila libre, la celda activa). Una que actúa siempre sobre lo mismo (mostrar un escenario por su nombre en una hoja fija) no las necesita.
- **Grabar** (nombre pedido, **sin tecla de método abreviado**, guardada en **Este libro**): ir a la hoja del escenario > grabar > Administrador de escenarios > escenario > **Mostrar** > Cerrar > detener. Sin hacer ninguna otra acción mientras se graba.
  ```vba
  ActiveSheet.Scenarios("NombreDelEscenario").Show
  ```
  `ActiveSheet` indica que la macro actúa sobre la hoja activa. La línea se identifica por `Scenarios("...")` y `.Show`.
- **Duplicar una macro editándola**: copiar todo de `Sub` a `End Sub`, pegarlo debajo, **cambiar el nombre** (sin espacios, por ejemplo `ShowJanelles`) y el **texto del escenario** entre comillas. Desde el botón *Macros* aparecen ambas.
- **Asignar a un botón**: con la hoja **desprotegida**, Programador > Insertar > **Botón (control de formulario)** > dibujarlo > en el cuadro que aparece, elegir la macro. Para un botón ya existente: clic derecho > **Asignar macro**.
- **Eliminar una macro**: Programador > Macros > seleccionar > **Eliminar**, o borrar su código en el editor de VBA. Ocultar botones o desactivar macros no las elimina.
- **Guardar conservando las macros**: *Libro de Excel habilitado para macros* (`.xlsm`) o *Libro binario* (`.xlsb`). Un `.xlsx` o una plantilla `.xltx` las descartan.

## Gotchas

- **Validación personalizada**: la fórmula se escribe para la primera celda del rango y con referencias **relativas**, no con `$`; si se bloquea (`$E$7`) todas las filas se comparan con la misma celda.
- **`VLOOKUP` por rango** con la tabla de tramos sin ordenar da resultados equivocados sin avisar.
- **Rodear datos no válidos solo marca**: hay que corregir los valores a mano.
- **Eliminar filas de una tabla** (no filas de la hoja) cuando hay datos al lado que no deben tocarse.
- **Quitar las flechas de rastreo** al terminar cada paso; desaparecen también al editar cualquier celda.
- **Orden de las herramientas de la hoja de costos**: Goal Seek y Solver **sobrescriben** las celdas de entrada, y algunas preguntas dependen del valor que dejaste antes. Conviene fijar el valor pedido en cada paso.
- **La protección va al final**, después de crear los botones y de asignarles macros; para asignarlas hay que **desproteger** primero.
- **Detalles de formato de las respuestas** (decimales, enteros, sin `$`): se aplican con el formato de la celda.

## Checklist: lo que debo ser capaz de hacer sin ayuda

- [ ] Elegir el tipo de validación adecuado (List, Whole number, Text length, Custom, Any value) según la restricción
- [ ] Validar con una lista de otra hoja y rodear los datos no válidos
- [ ] Escribir una validación personalizada con fórmula y referencias relativas
- [ ] Armar `IF`, `AND` y `OR` con fechas y años (`YEAR`, `TODAY`)
- [ ] Clasificar con `VLOOKUP` por rango sobre una tabla de tramos ordenada
- [ ] Armar un formato condicional de fila completa con `AND` y referencia mixta, y ordenar por color
- [ ] Buscar con `VLOOKUP` exacto y localizar el `#N/A`; hacer una búsqueda con dos criterios
- [ ] Eliminar filas de una tabla sin dañar los datos que hay al lado
- [ ] Usar *Rastrear error*, *Rastrear dependientes* y *Rastrear precedentes*, y diferenciar flechas rojas y azules
- [ ] Interpretar un triángulo verde y corregirlo
- [ ] Usar Goal Seek, Scenario Manager y su resumen, y Solver con restricciones guardando el escenario
- [ ] Desbloquear celdas y proteger una hoja en el orden correcto
- [ ] Grabar una macro que muestre un escenario, duplicarla editando el código y asignarla a un botón
- [ ] Eliminar una macro y guardar un libro conservando las macros
