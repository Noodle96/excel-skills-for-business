# Módulo 3 · Automating Lookups

> Los videos de esta semana, según los archivos en `material_curso/modulo-03/`, son: **V1 CHOOSE · V2 VLOOKUP Range · V3 VLOOKUP Exact Match · V4 MATCH · V5 INDEX_MATCH**. Las secciones 6-8 son material real de la lectura "Week 3: Toolbox", que además aporta el contenido base de las funciones (secciones 1-5, que combinan esa terminología con conocimiento general apoyado en los nombres de los videos), así que algún detalle puntual podría no calzar 100% con lo que muestra el curso. La sección 10 sale de dudas resueltas en conversación al practicar.

## 1. CHOOSE (V1)

`CHOOSE` es una función de la familia de búsqueda: **recupera un valor de una lista según un número que tú indicas**. Es buena para listas pequeñas que no cambian a menudo.

```
=CHOOSE(index_num, valor1, valor2, …)
```

- **`index_num`**: el número que indica de qué posición de la lista se toma el valor (1 = el primero, 2 = el segundo…).
- **`valor1, valor2, …`**: las celdas de la lista, escritas **una por una**: `B4,B5,B6` y **no** `B4:B6`.

Ejemplo: `=CHOOSE(2, B4, B5, B6)` devuelve el contenido de `B5`.

## 2. VLOOKUP por rango (V2)

`VLOOKUP` es una función de la familia de búsqueda que sirve para **recuperar y categorizar datos**. Por ejemplo, automatiza poner la nota (grade) de cada persona según la puntuación (mark) que sacó: las notas y los puntajes van en una **tabla de búsqueda**. La **V** es de *vertical*: solo sirve cuando los datos están organizados en vertical.

```
=VLOOKUP(lookup_value, table_array, col_index, [range_lookup])
```

| Argumento | Qué es |
|---|---|
| `lookup_value` | El valor que se quiere buscar. Excel lo busca en la **primera columna** de la tabla |
| `table_array` | La tabla que contiene el valor buscado y el valor que se quiere recuperar; se puede indicar con un nombre de tabla, un rango con nombre o referencias de celda |
| `col_index` | El número de la columna (contada desde la primera de la tabla) que contiene el valor que Excel debe devolver |
| `range_lookup` | Argumento opcional (va entre corchetes). Se omite en el VLOOKUP por rango; debe ser `FALSE` en el de coincidencia exacta |

**VLOOKUP por rango (Range VLOOKUP)**: busca un valor sin que necesariamente exista una coincidencia exacta: Excel usa **el mayor valor que sea menor o igual** al valor buscado. Para que funcione, los datos de la primera columna deben estar **ordenados** (alfabética o numéricamente, de menor a mayor). **Es el comportamiento por defecto de VLOOKUP en Excel.**

## 3. VLOOKUP de coincidencia exacta (V3)

**Exact Match VLOOKUP**: se usa para buscar un valor con una identidad única, por ejemplo el ID de un empleado o el número de cuenta, donde la coincidencia debe ser **exacta**. Para eso el cuarto argumento debe ser `FALSE`:

```
=VLOOKUP(A5, $G$4:$H$20, 2, FALSE)
```

- Si no encuentra el valor, devuelve el error **`#N/A`** (ver terminología).
- Detalle práctico: el `table_array` casi siempre debe ir **absoluto** (`$G$4:$H$20`, con `F4`) para que no se desplace al copiar la fórmula hacia abajo.
- Limitaciones: solo busca en la **primera columna** de la tabla y devuelve valores **a su derecha**.

## 4. MATCH (V4)

`MATCH` es una función de la familia de búsqueda que funciona casi como `VLOOKUP`: busca un valor en una fila o columna y **devuelve un número con la posición** donde está. Suele usarse combinada con otras funciones de búsqueda, como `INDEX`.

```
=MATCH(lookup_value, lookup_array, [match_type])
```

| `match_type` | Qué hace | Requisito |
|---|---|---|
| `0` | **Coincidencia exacta**: devuelve la posición del primer valor que coincide | Ninguno |
| `1` | **Menor que**: devuelve la posición del mayor valor menor o igual al buscado. Es el valor por defecto si se omite | Datos en orden **ascendente** |
| `-1` | **Mayor que**: devuelve la posición del menor valor mayor o igual al buscado | Datos en orden **descendente** |

## 5. INDEX y MATCH juntas (V5)

`INDEX` devuelve un valor, o la referencia a un valor, desde dentro de una tabla o rango.

```
=INDEX(array, row_num, [col_num])
```

- **`array`**: el primer argumento; normalmente un rango de celdas.
- **`row_num`**: el número de la fila que contiene el valor a devolver. Es opcional si el rango tiene **una sola fila**.
- **`col_num`**: el número de la columna que contiene el valor a devolver. Es opcional si el rango tiene **una sola columna**.
- **`area_num`**: se especifica cuando se indican **varios conjuntos de referencias separados**, para decirle a Excel de cuál de ellos se trata.

**Nota del curso**: para extraer toda una fila o columna de una tabla con `INDEX`, se indica `col_num` o `row_num` como **0**. Hay que seleccionar las celdas donde irá el resultado y terminar la fórmula con `Ctrl + Shift + Enter`.

**La combinación `INDEX` + `MATCH`**: `MATCH` calcula la posición y `INDEX` devuelve el valor que está en ella:

```
=INDEX(rango_a_devolver, MATCH(valor_buscado, rango_donde_buscar, 0))
```

Ventajas frente a `VLOOKUP`: puede devolver valores a la **izquierda** de la columna de búsqueda, y no se rompe si se insertan columnas en la tabla (porque no cuenta columnas con un número fijo).

**Búsqueda en dos sentidos** (fila y columna a la vez), con dos `MATCH`:

```
=INDEX(matriz, MATCH(valor_de_fila, lista_de_filas, 0), MATCH(valor_de_columna, lista_de_columnas, 0))
```

Aquí el primer `MATCH` da la **fila** y el segundo la **columna**; el orden importa, y la matriz debe tener tantas filas y columnas como las dos listas.

## 6. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Tab` | Aceptar la función que Excel sugiere en el cuadro de autocompletado |
| `F4` | Justo después de escribir una referencia de celda en la barra de fórmulas: una pulsación la vuelve **absoluta**; si se sigue pulsando, cicla entre absoluta, mixta y relativa |

## 7. Terminología

- **VLOOKUP**: función de la familia de búsqueda usada para recuperar y categorizar datos; la V es de *vertical*.
- **Lookup Value (valor buscado)**: el valor que se quiere buscar en la tabla; Excel lo busca en la primera columna.
- **Lookup Table / Table Array (tabla de búsqueda)**: la tabla con el valor buscado y el valor a devolver.
- **Col_index (índice de columna)**: la columna que contiene el valor que Excel debe devolver.
- **Range Lookup (búsqueda por rango)**: argumento opcional; debe ser `FALSE` para la coincidencia exacta.
- **Range VLOOKUP**: búsqueda sin coincidencia exacta; usa el mayor valor menor o igual al buscado; los datos deben estar ordenados. Es el comportamiento por defecto.
- **Exact Match VLOOKUP**: búsqueda de un valor con identidad única (ID de empleado, número de cuenta).
- **INDEX**: devuelve un valor, o la referencia a un valor, de una tabla o rango. Sus argumentos: **Array**, **Row_num**, **Col_num**, **Area_num**.
- **CHOOSE**: recupera un valor de una lista según un número; buen para listas pequeñas que no cambian. Argumentos: **Index_num** y **Value 1, Value 2…** (referencias una por una).
- **MATCH**: busca un valor en una fila o columna y devuelve su posición. Argumentos: **Lookup_value**, **Lookup_array** y **Match type** (0, 1 o -1).
- **#N/A**: error que suele aparecer cuando la función de búsqueda no encuentra el valor buscado en la tabla.

## 8. Ninja tips de la semana

### Recordatorio: referencias relativas, absolutas y mixtas
- **Relativa** (`D4`): al copiarla una columna a la derecha pasa a `E4`; una fila hacia abajo, a `D5`.
- **Absoluta** (`$D$4`): con `$` delante de la letra de columna y del número de fila no cambia al copiar.
- **Mixta**: solo una parte queda "bloqueada". `$D4` no cambia al copiarla a la derecha pero sí hacia abajo (`$D5`); `D$4` no cambia hacia abajo pero sí hacia los lados.

### Búsqueda por rango frente a IF anidados
La búsqueda por rango es una alternativa eficiente a los `IF` anidados, que son difíciles de implementar, comprobar y editar. Ejemplo: clasificar el precio de venta según el nivel de descuento.

Con **IF anidados**, en la celda `E4`:
```
=IF(D4<$G$5,$H$4,IF(D4<$G$6,$H$5,IF(D4<$G$7,$H$6,$H$7)))
```

Con **búsqueda por rango**:
```
=VLOOKUP(D4,$G$4:$H$7,2)
```

En ambos casos se puede copiar la fórmula hacia abajo en la columna `E`, pero la segunda es mucho más eficiente y menos propensa a errores.

## 9. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **`XLOOKUP`**: la función moderna que reemplaza a `VLOOKUP`, `HLOOKUP` y `INDEX`+`MATCH` en las versiones recientes de Excel; busca en cualquier dirección y es exacta por defecto. Conviene conocerla, pero este curso se centra en las anteriores, que siguen siendo las más compatibles.
- **`HLOOKUP`**: igual que `VLOOKUP` pero para datos organizados en **horizontal** (la tabla se busca por filas).
- **Envolver la búsqueda en `IFERROR`/`IFNA`** (Módulo 2): `=IFERROR(VLOOKUP(...), "no encontrado")` reemplaza el `#N/A` por un mensaje legible. `IFNA` es más precisa porque solo oculta el `#N/A` y deja ver los demás errores.
- **El riesgo silencioso del rango**: si la primera columna **no está ordenada** y se usa la búsqueda por rango, `VLOOKUP` devuelve un resultado **incorrecto sin dar error**. Para IDs conviene siempre `FALSE`.
- **`col_index` dinámico**: en vez de contar columnas a mano, se puede calcular con `MATCH`: `=VLOOKUP(A5, tabla, MATCH("Descuento", encabezados, 0), FALSE)`.
- **Causas típicas de `#N/A`** cuando "el valor sí está": espacios sobrantes (`TRIM`), números guardados como texto (o al revés) y diferencias de ortografía.
- **Comodines con coincidencia exacta**: `?` y `*` funcionan en `VLOOKUP` con `FALSE` y en `MATCH` con tipo `0`, pero **solo con texto**.

## 10. Lo aprendido al practicar

- **Orden de los argumentos de `INDEX` con dos `MATCH`**: `INDEX(matriz; fila; columna)`. El primer `MATCH` debe dar la **fila** y el segundo la **columna**; si la matriz tiene las categorías al revés, hay que intercambiarlos.
- **Tamaño de los rangos con nombre**: la matriz debe tener exactamente tantas filas y columnas como las dos listas que se buscan; si una de las listas incluye los encabezados y la otra no, el resultado sale **desplazado una posición** sin ningún error.
- **Referencia relativa dentro de una tabla**: una fórmula con `[@Columna]` y una referencia como `I3` mezcla una referencia que cambia por fila con una que también se desplaza al rellenar. Si `I3` es **una sola celda de entrada**, debe ir **absoluta** (`$I$3`); si es una columna de cada fila, debe ser la referencia estructurada de esa columna.
- **Separadores regionales**: en Excel con coma decimal, los argumentos se separan con `;` (`INDEX(Discounts;MATCH(...);MATCH(...))`).

---

### Práctica
Archivos del curso: `material_curso/modulo-03/`. Ningún video trae versión `Soln`. Hay un reto de práctica con solución en `challenge/`; el assessment de la semana está documentado por habilidades en [assessment.md](assessment.md).

### Fuente
Lectura "Week 3: Toolbox" del curso (atajos, terminología de VLOOKUP/INDEX/CHOOSE/MATCH/#N/A, ninja tips de referencias y de búsqueda por rango frente a IF anidados — secciones 1-8 en lo que viene de ahí). Las secciones 1-5 se apoyan además en los nombres de los videos, con conocimiento general. Sección 9 es complemento propio. Sección 10 sale de dudas resueltas en conversación.
