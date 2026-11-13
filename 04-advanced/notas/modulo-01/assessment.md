# Assessment del Módulo 1 (Spreadsheet Design and Documentation) — habilidades a dominar

Este documento **no guarda respuestas, resultados numéricos ni puntajes**: toma cada pregunta como una **habilidad** que hay que saber ejecutar sin ayuda, con el procedimiento para hacerla, para poder repasarla y practicarla después.

**Cómo es el assessment**: un solo libro en el que se construye, pregunta a pregunta, **un modelo financiero mensual de 60 periodos** (de enero de 2018 a diciembre de 2022, columnas `G` a `BN`). Tiene una hoja `Inputs` con los supuestos (valores iniciales, tasas de crecimiento por año y el mes en que ocurre el crecimiento) y una hoja de cálculos mensuales (`Calcs_Monthly`) que se va completando. Cada pregunta construye sobre la anterior y pide leer un valor concreto para comprobar el avance. Todo gira alrededor de **buen diseño**: una sola fórmula por fila, ningún valor escrito dentro de fórmulas, rangos con nombre y una convención de formato visible.

## Habilidades que pedía el assessment

| # | Qué se pedía | Habilidad |
|---|---|---|
| 1 | Ajustar el ancho de las columnas a unas medidas dadas | Ancho de columna |
| 2 | Congelar un área de filas y columnas | Inmovilizar paneles |
| 3 | Escribir cabeceras y etiquetas, aplicar la convención de formato, poner supuestos fijos con tres decimales y mostrar una fecha con un rango con nombre | Formato por convención + rango con nombre |
| 4 | Dar a una fecha un formato exacto (`01-Jan-18`) | Formato de número personalizado |
| 5 | Construir las filas de fechas con `EOMONTH` y "primer día del mes siguiente", y rellenar hacia la derecha | `EOMONTH` + relleno |
| 6 | Marcar en rojo la celda distinta de una fila y calcular mes y año de cada fecha | Convención de "cálculo único" + `MONTH` / `YEAR` |
| 7 | Calcular un factor de crecimiento anual con una sola fórmula que se pueda copiar | Fórmula flexible con `IF`, `INDEX` y `MATCH` |
| 8 | Nombrar rangos de los supuestos y reescribir la fórmula con esos nombres | Rangos con nombre |
| 9 | Repetir la construcción para la otra serie (gastos) con sus propios nombres | Reutilizar un patrón de fórmula |
| 10 | Dar tres decimales a la fila y leer el factor de un mes concreto | Formato de número + lectura de un resultado |
| 11 | Dar formato de miles con paréntesis para negativos y calcular los ingresos | Formato personalizado + rango con nombre en una fórmula |
| 12 | Calcular el total de una fila | `SUM` sobre el rango de la línea de tiempo |
| 13 | Calcular el costo de ventas (negativo) y el margen bruto | Signos y subtotales |
| 14 | Calcular los gastos (negativos) a partir de un valor inicial y un factor, y su total | Fórmula con nombre y factor |
| 15 | Calcular el resultado neto | Suma de subtotales y lectura de un mes |

## Cómo se hace cada una

### Ancho de columna (1)
Seleccionar la columna (o varias) > pestaña **Inicio** > **Formato** > **Ancho de columna** > escribir el valor. Para varias columnas no contiguas, `Ctrl` + clic en cada letra. Para un rango largo (como `G:BN`), escribir el rango en el **Cuadro de nombres** y pulsar Enter. Otro camino es clic derecho sobre la **letra** de la columna > *Ancho de columna*. (*Formato de celdas* no tiene ancho de columna, y *Diseño de página > Ajustar área de impresión > Ancho* es de impresión.)

### Inmovilizar paneles (2)
Excel inmoviliza **todo lo que queda por encima y a la izquierda de la celda seleccionada**. Para congelar de la fila 1 a la `n` y de la columna A a la `m`, se selecciona la celda de la fila `n+1` y la columna `m+1`, y luego Vista > **Inmovilizar paneles**.

### Formato por convención y rango con nombre (3)
- Se escriben las cabeceras, etiquetas de filas y de unidades, con color, negrita y bordes según la imagen (no tiene que ser exacto).
- La hoja trae una **leyenda de formato**: **azul** para lo que se escribe a mano (supuestos y etiquetas de unidades), **negro** para cálculos y **rojo** para el cálculo único. Los supuestos fijos van en azul.
- Tres decimales: `Ctrl + 1` > Número > 3 posiciones decimales.
- Para mostrar un dato con un rango con nombre: `=NombreDelRango`, o `F3` para elegirlo de la lista.
- Si el resultado de una fecha se ve como un número (por ejemplo `43101`), es el **número de serie**; no es un error, y el formato de fecha se aplica cuando el enunciado lo pida.

### Formato de número personalizado (4 y 11)
`Ctrl + 1` > Número > **Personalizada** > escribir el código en *Tipo*:
- **Fecha como `01-Jan-18`**: `dd-mmm-yy` (en un Excel en español, el código del año es `aa`: `dd-mmm-aa`). El nombre del mes sale según el idioma del sistema.
- **Miles sin decimales y negativos entre paréntesis**: `#,##0;(#,##0)` (con punto como separador de miles: `#.##0;(#.##0)`).
- Las opciones rápidas de la cinta (*Fecha larga*) no permiten un formato exacto, y `Ctrl + ;` solo inserta la fecha de hoy.

### `EOMONTH` y relleno (5)
- `EOMONTH(fecha_inicio, 0)` da el **último día del mes** de esa fecha.
- Sumar 1 a un fin de mes da el **primer día del mes siguiente**.
- Las dos filas dependen una de la otra, así que hay que rellenar **ambas hasta el final** (`Ctrl + R` desde la primera celda del rango; el rango se escribe en el Cuadro de nombres). Si se rellena una sola, la otra mostrará fechas de 1900.

### Cálculo único en rojo + `MONTH` y `YEAR` (6)
- Cuando una fila tiene una fórmula consistente y una celda es distinta (aquí, la de partida), esa celda se marca con **fuente roja**, para avisar al lector.
- `=MONTH(fecha)` y `=YEAR(fecha)` con referencia a la fila de fechas, copiadas a toda la fila. Las filas de mes y año deben tener formato **General**, no de fecha.

### Factor de crecimiento flexible (7)
Una sola fórmula, copiable a toda la fila:
```
=celda_anterior * (1 + IF(mes_de_la_columna = mes_de_crecimiento,
                          INDEX(tasas, MATCH(año_de_la_columna, años, 0)),
                          0))
```
- Multiplica el factor del mes anterior por `1 + tasa`; la tasa solo se aplica **en el mes de crecimiento**, y en los demás meses es 0.
- `MATCH` busca el año en la lista de años y `INDEX` trae su tasa.
- **Referencias**: mes y año con la **fila fija** (`G$7`, `G$8`) y columna relativa; los supuestos, absolutos.
- El factor inicial (el valor de la celda a la izquierda de la primera) debe ser 1.

### Rangos con nombre y fórmulas legibles (8 y 9)
- Cuadro de nombres > escribir el nombre > Enter, sobre el rango. Los nombres **no admiten espacios**, y conviene escribirlos exactamente como se piden.
- Se sustituyen las referencias `Hoja!$F$32` por nombres (`GrowthRevMth`...). La fórmula **no se reescribe sola**: hay que volver a escribirla (o usar *Aplicar nombres*).
- Para la segunda serie se repite exactamente la construcción con los **nombres de esa serie** y la fila propia del factor.
- Si el Excel usa `;` como separador, en una respuesta escrita hay que usar **comas y sin espacios**.

### Formato y lectura de un resultado (10)
Seleccionar la fila, aplicar tres decimales, rellenar hasta el final y leer la celda de la columna pedida. Para saber de qué mes es una columna, se cuenta cuántas columnas hay desde la primera (cada una es un mes).

### Ingresos (11)
```
=valor_inicial_con_nombre * factor_de_crecimiento_del_mes
```
El valor inicial va como **nombre** (no se mueve al copiar) y el factor como referencia relativa a la fila de factores (la columna cambia al copiar).

### Total de una fila (12)
`=SUM(primera_celda:última_celda)` en la columna de totales, sobre **toda** la línea de tiempo y sin incluir celdas de unidades.

### Costo de ventas y margen bruto (13)
- **Costo de ventas** = ingresos × porcentaje de costo (nombre), con signo **negativo** (se escribe `=-ingresos*nombre`).
- **Margen bruto** = suma de ingresos y costo (`=SUM(...)` de las dos filas de arriba). Como el costo ya es negativo, **se suma**.

### Gastos (14)
`=-valor_inicial_con_nombre * factor_de_gastos_del_mes`, con signo negativo y usando la fila de factores **de gastos**. Se completa la fila y se calcula el total.

### Resultado neto (15)
`=margen_bruto + gastos` (los gastos ya son negativos). Se rellena toda la fila y se lee el mes pedido.

## Gotchas

- **Rellenar las dos filas** de fechas que dependen una de la otra; si no, aparecen fechas de 1900 (`31-ene-00`).
- **Formato General** en las filas de mes y año; con formato de fecha se ven mal.
- **El crecimiento ocurre el primer día del mes indicado**: en ese mes el factor ya sube.
- **Los nombres no se aplican solos** a una fórmula ya escrita.
- **Signo**: el costo y los gastos son negativos; los subtotales los **suman**, no los restan.
- **Referencias**: la fila fija (`G$7`) en las cabeceras de mes y año, y absolutas en los supuestos.
- **Respuestas escritas**: comas como separador de argumentos y sin espacios, aunque el Excel use `;`; números sin separador de miles; fechas con el formato exacto que pida el enunciado.
- **Comprobar con un mes conocido** antes de responder, por ejemplo, que el factor cambie en la columna del mes de crecimiento y no antes.

## Checklist: lo que debo ser capaz de hacer sin ayuda

- [ ] Cambiar el ancho de columnas (varias a la vez y un rango largo con el Cuadro de nombres)
- [ ] Decidir qué celda seleccionar para inmovilizar un área dada
- [ ] Aplicar una convención de formato por colores (supuestos en azul, cálculos en negro, cálculo único en rojo)
- [ ] Crear un formato de número personalizado para fechas y para miles con paréntesis
- [ ] Construir una línea de tiempo mensual con `EOMONTH` y rellenarla hacia la derecha
- [ ] Obtener mes y año de cada fecha con `MONTH` y `YEAR`
- [ ] Entender y escribir una fórmula con `IF` + `INDEX` + `MATCH` que escale cada año según los supuestos
- [ ] Crear rangos con nombre y reescribir una fórmula con ellos
- [ ] Reutilizar el mismo patrón de fórmula para otra serie de datos
- [ ] Calcular ingresos, costos, gastos, margen bruto y resultado neto con los signos correctos
- [ ] Calcular el total de una fila con `SUM`
- [ ] Comprobar un modelo leyendo celdas de control y verificando que los resultados tengan sentido
