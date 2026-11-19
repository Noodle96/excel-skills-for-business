# Módulo 2 · Advanced Formula Techniques

> Los videos de esta semana, según los archivos en `material_curso/modulo-02/`, son: **V1 Tables and Structured Referencing · V2 Using Functions to Sort Data · V3 Introduction to Array Formulas · V4 Working with an Array Function (`TRANSPOSE`) · V5 Solving Problems with Array Formulas**. Las secciones 1-5 se redactaron con conocimiento general apoyado en los nombres de los videos, en el contenido real de los cinco libros del curso (tablas, nombres, fórmulas y cuadros de texto con anotaciones en español, revisados) y en las dudas que fueron surgiendo al hacer el reto y el assessment, así que algún detalle del procedimiento podría no calzar al 100% con lo que muestra el video. La sección 6 (atajos, terminología y ninja tips) es material real de la lectura "Week 2: Toolbox".

**Idea central**: una fórmula puede trabajar con **un rango completo a la vez** (una *matriz*, *array*) en vez de celda por celda, y las **tablas** permiten escribir esas fórmulas con *nombres* en lugar de direcciones. Las dos ideas juntas dan hojas más cortas, más legibles y que se ajustan solas cuando los datos crecen.

Objetivos de la semana (lectura "Course goals"): aplicar referencias estructuradas en fórmulas, explicar el uso de las fórmulas matriciales, construir fórmulas con funciones matriciales y usar fórmulas matriciales en cálculos.

Los libros trabajan sobre un caso, `PHOTO GIFTS` (ventas por Internet de una tienda de regalos, con una lista de productos y una tabla de tipos de cambio). Los cinco libros arrastran el mismo caso y cada uno parte del estado en que dejó el anterior.

## 1. Referencias estructuradas (V1)

Al convertir un rango en **tabla** (`Ctrl + T`), Excel le da un nombre a la tabla y a cada columna, y las fórmulas pueden usar esos nombres en vez de `$B$4:$B$44`. A eso se le llama **referencia estructurada** (*structured reference*). Es algo que parece complicado la primera vez, pero se lee casi como una frase: `=SUM(tbl_sales[Revenue])` es "la suma de la columna Revenue de la tabla tbl_sales".

En el libro, la tabla `tbl_sales` (`A3:I44`) guarda las ventas trimestrales por país (Australia, UK, US, Singapore, France, China) y una tabla pequeña `tbl_TopQtr` (`L18:R21`) guarda los mejores trimestres.

### Sintaxis que se ve esta semana

| Forma | Qué devuelve |
|---|---|
| `tbl_sales[Australia]` | Los **datos** de la columna (sin encabezado ni fila de totales) |
| `tbl_sales[[#Headers],[Australia]]` | El **encabezado** de la columna: el texto "Australia" |
| `tbl_sales[@Australia]` o `tbl_sales[@[Australia]]` | El valor de la columna **en esta fila** (`@` = *this row*) |
| `tbl_sales[[Australia]:[China]]` | **Todas** las filas de datos, de la columna Australia a la China |
| `tbl_sales[@[Australia]:[China]]` | Solo **esta fila**, de la columna Australia a la China |
| `ROWS(tbl_sales)` | Número de filas de datos de la tabla |
| `tbl_sales[#Headers]`, `[#Totals]`, `[#All]` | La fila de encabezados, la de totales o toda la tabla |

- `@` es la forma corta de `[#This Row]`: `tbl_sales[@[Australia]:[China]]` equivale a `tbl_sales[[#This Row],[Australia]:[China]]`.
- Si el nombre de la columna tiene **espacios o caracteres especiales**, va entre dobles corchetes: `[@[Best Quarters]]`. Si no los tiene, los corchetes simples bastan.

### `@` frente a sin `@`: la duda que más aclaró

```
=SUM(tbl_sales[@[Australia]:[China]])    ← solo la fila actual (una columna "Total" de la tabla)
=SUM(tbl_sales[[Australia]:[China]])     ← todo el bloque, todas las filas (un total general)
```

Si la tabla es:

| Producto | Australia | NZ | China |
|---|---|---|---|
| A | 10 | 20 | 30 |
| B | 5 | 15 | 25 |

- En la fila de A, la primera fórmula da `60`; en la de B, `45`. Cada fila obtiene su propio total, que es lo que se quiere en una **columna calculada**.
- La segunda da `105` en cualquier fila (todo el bloque). Es la fórmula de un total general, normalmente fuera de la tabla (en el libro está en `M3`).
- **Trampa**: si se escribe la versión sin `@` dentro de una columna calculada, **todas las filas muestran el mismo total**. Suele ser señal de que se olvidó el `@`.

### Comportamiento al arrastrar: relativa y absoluta

Las referencias estructuradas también pueden ser **relativas** o **absolutas** al arrastrar con el controlador de relleno, aunque no se ve con `$`:

- Una referencia a **una sola columna** (`[@State]`, `tbl[Revenue]`) **se desplaza** al arrastrar hacia la derecha: pasa a la columna siguiente. Es el comportamiento "relativo".
- Una referencia **de columna a columna sobre la misma columna** (`[@[State]:[State]]`) **se queda fija**. Es el truco de la **referencia estructurada absoluta** (nombre usado en el curso y en el Toolbox).
- Analogía con A1: `[@State]` ≈ `D5` (relativa); `[@[State]:[State]]` ≈ `$D5` (columna fija).

En el libro (`Sales Summary`), la tabla de los mejores trimestres lo usa así:

```
=LARGE(tbl_sales[Australia], tbl_TopQtr[@[Best Quarters]:[Best Quarters]])
```

Al arrastrar a la derecha, `tbl_sales[Australia]` pasa a `tbl_sales[UK]`, `tbl_sales[US]`... (relativa, lo que se quiere), pero el `k` de `LARGE` se queda siempre en la columna `Best Quarters` (absoluta). En el libro hay un cuadro de texto que lo resume: *"Drag fill horizontal: referencias relativas. Drag fill vertical: referencias absolutas"*.

> Al arrastrar a la derecha, las columnas estructuradas cambian **por posición**, no por nombre: funciona bien cuando las columnas de las dos tablas están en el mismo orden. Si se reordenan, el relleno automático deja de dar la columna correcta.

### Más fórmulas reales del libro

```
=ROWS(tbl_sales)                                      ← cuántas filas tiene la tabla (M4)
=SUM(tbl_sales[[Australia]:[China]])                  ← total de todo el bloque (M3)
=tbl_sales[[#Headers],[Australia]]                    ← traer los títulos por fórmula (M7:R7)
=MAX(tbl_sales[Year])-K8                              ← año relativo al más reciente (L8)
=SUMIFS(tbl_sales[Australia],Year,$L8)                ← ventas de un año; `Year` es un rango con nombre (M8)
```

Aquí se mezclan **rangos con nombre** (`Year`) y **referencias estructuradas** (`tbl_sales[Australia]`) dentro de la misma fórmula, que es perfectamente válido.

### ¿Qué ventaja tiene una referencia estructurada frente a un rango con nombre básico?

Pregunta típica de este tema. Ambas hacen más legible la fórmula y se pueden usar desde otras hojas; lo que **solo** da la referencia estructurada es:

| | Rango con nombre fijo (`Sales` = `$A$2:$D$100`) | Referencia estructurada (`Sales[...]`) |
|---|---|---|
| Se agregan filas nuevas | **No crece**: hay que redefinirlo | **Crece sola** |
| Apuntar a una columna | Un nombre por columna | `Sales[Revenue]` |
| Apuntar a la fila actual | No se puede | `Sales[@Revenue]` |
| Encabezados y totales | Hay que excluirlos a mano | `Sales[Revenue]` toma solo los datos |
| Se renombra una columna | El nombre no se entera | La fórmula **se actualiza sola** |

Matiz: un rango con nombre definido sobre la **columna entera** (`=Hoja!$A:$A`) sí crece. Pero en el caso normal de un rango fijo, no.

### Trampas con tablas

- **Los encabezados de una tabla no admiten fórmulas vivas**: Excel los convierte en texto fijo. Si se quieren traer los títulos con `tbl[[#Headers],[Col]]`, se escribe la fórmula en una celda libre, se copia y se pega como valores sobre el encabezado.
- **La fila de totales** dentro de un rango normal se cuenta como un dato más: un rango `D12:H61` que incluye la fila de totales de una tabla **duplicó** un resultado en el reto. Con la referencia estructurada (`tbl[[Product 1]:[Product 5]]`) no pasa.
- **Una tabla no admite fórmulas matriciales de varias celdas ni derrames**: si el destino es una tabla, hay que usar una fórmula por celda.
- Al hacer clic en las celdas mientras se escribe, Excel arma solo la referencia estructurada: es la forma más fácil de no equivocarse en los nombres.

## 2. Ordenar con funciones (V2)

Ordenar con el menú *Datos → Ordenar* **no se actualiza** si cambian los datos. Con fórmulas, la lista ordenada **se recalcula sola**. El curso lo hace con tres funciones encadenadas, que ya se conocían por separado:

| Paso | Función | Qué calcula |
|---|---|---|
| 1. Posición alfabética | `COUNTIFS` | Cuántos valores van antes o igual que este |
| 2. Dónde está cada posición | `MATCH` | La fila de la lista donde está la posición 1, 2, 3... |
| 3. Traer el nombre | `INDEX` | El valor de esa fila: la lista ordenada |

En el libro (`Conversion Table`, ordenar los códigos de moneda del rango con nombre `rateCodes`):

```
E5:  =COUNTIFS(rateCodes,"<="&'Current Rates'!D4)    ← cuántos códigos son ≤ a este (su posición alfabética)
H5:  =ROW()-4                                         ← columna auxiliar: 1, 2, 3... (la posición buscada)
J5:  =MATCH(H5,$E$5:$E$14,0)                          ← en qué fila está la posición 1, 2, 3...
N5:  =INDEX(rateCodes,J5)                             ← el código en esa fila: la lista ordenada (N)
```

- **`"<="&A1` con texto compara alfabéticamente**: `COUNTIFS` cuenta cuántos nombres son menores o iguales al de la fila, y como incluye al propio, la primera da 1 y la última da el total.
- Los `$` del `MATCH` (`$E$5:$E$14`) fijan el rango al arrastrar hacia abajo.
- Si dos valores fueran **idénticos**, tendrían el mismo ranking y el `MATCH` de la posición siguiente daría `#N/A`. Aquí los códigos son únicos.
- La **columna auxiliar** (`H`, "helper index") es necesaria para tener los números 1, 2, 3... contra los que buscar.
- En el libro hay una zona con cuadros de texto que explican cada paso, y en `Current Rates` se usa `=IFNA(VLOOKUP(A3,CodeLookup,2,0),"")` para traer los códigos de moneda a partir de su nombre.

Alternativa moderna: `SORT` o `SORTBY` en Excel 365 (sección 7).

### ¿Cuándo `VLOOKUP`/`HLOOKUP` y cuándo `INDEX` + `MATCH`?

| | `VLOOKUP` | `HLOOKUP` | `INDEX` + `MATCH` |
|---|---|---|---|
| Busca en | La **primera columna** del rango | La **primera fila** del rango | Cualquier columna o fila |
| Devuelve de | Una columna a la **derecha** | Una fila **hacia abajo** | **Cualquier** posición, izquierda o derecha |
| Datos en | Vertical | Horizontal | Vertical u horizontal |
| Columna de retorno | Un **número** fijo (`3`) | Un número fijo de fila | Se elige por referencia |

- **`VLOOKUP`**: datos en vertical, lo buscado en la primera columna, retorno a la derecha, estructura fija.
- **`HLOOKUP`**: lo mismo pero con los títulos en la primera fila (se usa poco).
- **`INDEX` + `MATCH`**: retorno a la **izquierda** del valor buscado, insertar o borrar columnas sin romper la fórmula, búsquedas con **varios criterios** y tablas muy grandes.
- Los tres necesitan `0` o `FALSE` para **coincidencia exacta**; sin eso Excel asume una aproximada y puede devolver un valor erróneo sin avisar.
- `XLOOKUP` (Excel 365/2021) reemplaza a los tres.

## 3. Introducción a las fórmulas matriciales (V3)

Una **matriz** (*array*) es una cuadrícula de celdas, por ejemplo `A5:A7`. Una **fórmula matricial** (*array formula*, también "fórmula CSE") trabaja con la matriz completa en un solo cálculo, y en vez de recorrer fila a fila, opera sobre todo el rango a la vez. En la barra de fórmulas se reconoce por las **llaves** `{ }`, que Excel pone solo: no se escriben.

### Dos tipos

- **Fórmula matricial de varias celdas** (*multi-cell array*): da **varios resultados** a partir de una sola fórmula. Se **selecciona primero todo el rango de salida** y luego se escribe la fórmula.
- **Fórmula matricial de una celda** (*single-cell array*): usa matrices por dentro, pero devuelve **un solo resultado**.

### Cómo se escribe

1. Seleccionar el rango de salida (todas las celdas, si son varias).
2. Escribir la fórmula empezando por `=`.
3. Confirmar con **`Ctrl + Shift + Enter`** (`CSE`), no con `Enter`.
4. Para convertir una fórmula existente: `F2` y luego `Ctrl + Shift + Enter`.

### Fórmula de varias celdas (hoja `Product List`)

El libro convierte los precios de USD a AUD con una sola fórmula en todo el rango `D4:D36`:

```
{=C4:C36*H4}          ← en D4:D36, todos los precios de la columna C × el tipo de cambio de H4
```

- Se selecciona `D4:D36`, se escribe `=`, se selecciona `C4:C36`, `*`, `H4` y se confirma con `CSE`.
- Como todo el rango es **una sola fórmula**, **no se puede editar ni borrar una celda suelta**: Excel avisa. Hay que tratar el bloque completo.
- La otra forma, sin matriz, es `=C4*$H$4` en cada fila y arrastrar. Da lo mismo, pero con una fórmula distinta (con `$`) por cada fila.
- En el cuadro de texto del libro: *"matriz constante: k en LARGE()"*.

### Constantes matriciales

Una **constante matricial** (*array constant*) es una matriz de **valores** escritos entre llaves, no de referencias: `{1;2;3}`.

```
=LARGE(D4:D36,{1;2;3})       ← los tres mayores precios (1.º, 2.º y 3.º)
```

- En Excel con configuración en inglés, `;` separa **filas** (matriz vertical) y `,` separa **columnas** (matriz horizontal): `{1;2;3}` es vertical y `{1,2,3}` es horizontal.
- Con `LARGE` (o `SMALL`) y una constante de `k` se obtienen varios valores con una sola fórmula, en varias celdas.

### Fórmula de una celda

```
{=SUM(H7:H9*I7:I9)}          ← cantidad × precio de cada fila, sumado (equivale a SUMPRODUCT)
=SUM(IF(F4:F54<10,F4:F54,0))  ← suma solo los valores menores que 10 (con CSE)
```

- La segunda hace lo mismo que un `SUMIFS`, pero la versión matricial permite **cambiar el cálculo**: por ejemplo, sumar el **producto de dos columnas** donde se cumple una condición, algo que `SUMIFS` no puede hacer: `=SUM(IF(F4:F54<10,E4:E54*F4:F54,0))`.

### CSE frente a matrices dinámicas (Excel 365)

| | Con llaves `{=...}` (CSE) | Sin llaves (matriz dinámica, Excel 365/2021) |
|---|---|---|
| Se confirma con | `Ctrl + Shift + Enter` | `Enter` |
| Rango de salida | Se selecciona antes, tamaño exacto | Excel lo calcula y **derrama** el resultado |
| Si el origen crece | No se ajusta | Se ajusta |
| Error típico | `#N/A` en las celdas sobrantes | `#SPILL!` si algo bloquea |
| Versión | Todas | 365 / 2021 |

- En Excel 365 las dos formas dan **el mismo resultado**. En versiones antiguas, una fórmula matricial escrita **sin CSE** usa **intersección implícita** (toma un solo valor de cada rango, el de la misma fila) y da un resultado **equivocado sin avisar** (o `#VALUE!`).
- Un archivo hecho en 365 con matrices dinámicas, abierto en un Excel antiguo, muestra esas fórmulas con llaves.
- El curso enseña la forma con llaves porque funciona en cualquier versión; aunque la empresa tenga Excel actualizado, conviene saber **leer** estas fórmulas: los archivos viajan y se heredan libros antiguos.

## 4. Una función matricial: `TRANSPOSE` (V4)

`TRANSPOSE` convierte una fila en columna o una columna en fila. Es una función que **solo existe como matriz** (la salida ocupa varias celdas).

```
{=TRANSPOSE(A4:A14)}           ← la lista vertical A4:A14 convertida en horizontal
{=TRANSPOSE(1/B5:B14)}         ← se puede transformar al transponer: el inverso de cada tipo de cambio
```

En el libro (`Conversion Table`) se usa para armar una **tabla de conversión entre monedas**: los códigos de moneda de `A4:A14` se transponen como títulos de columna (`B3:L3`), y `1/B5:B14` da el tipo de cambio inverso para cada columna (`C4:...`).

Procedimiento (así viene anotado en un cuadro de texto del libro):
1. **Seleccionar el área de salida** con el tamaño **exacto** de la matriz transpuesta (una columna de 11 celdas → una fila de 11 celdas).
2. Escribir la fórmula con `=`.
3. Confirmar con `Ctrl + Shift + Enter`.

- Si se selecciona un rango **mayor**, las celdas sobrantes muestran `#N/A`; si es **menor**, el resultado se corta.
- En Excel 365, `=TRANSPOSE(A4:A14)` en una celda con `Enter` se derrama solo.
- **Sirve una tabla**: `=TRANSPOSE(tbl_sales[Revenue])`.
- Los **títulos de filas** se pueden traer con una matriz sencilla de una columna: `{=Towns}` en cinco celdas, y los títulos de columnas con `{=TRANSPOSE(Towns)}` en cinco celdas de una fila. Así quedan en el mismo orden.

### `TRANSPOSE` frente a *Pegado especial → Transponer*

| | `TRANSPOSE` | Pegado especial (valores + transponer) |
|---|---|---|
| Se actualiza si cambia el origen | **Sí** (está vinculada) | No (resultado fijo) |
| Copiar fórmulas y pegar transpuesto | — | Las referencias relativas **se giran**: solo la primera celda sale bien y las demás pueden dar `#N/A` o apuntar a celdas equivocadas |

Por eso, cuando se quieren las **fórmulas** de una columna en horizontal, no sirve copiar y pegar transpuesto: o se usa `TRANSPOSE` o se pegan **solo valores**.

## 5. Resolver problemas con fórmulas matriciales (V5)

Aquí se juntan las herramientas anteriores en casos prácticos.

### Contar con `TRUE`/`FALSE`

Una comparación entre dos rangos da una matriz de `TRUE` y `FALSE`. `SUM` **ignora los valores lógicos**, así que se multiplican por `1` para convertirlos en `1` y `0`:

```
{=SUM((USD_Price>Old_USD_Price)*1)}     ← cuántos productos AUMENTARON su precio
{=SUM((USD_Price<Old_USD_Price)*1)}     ← cuántos DISMINUYERON su precio
```

- `USD_Price` y `Old_USD_Price` son rangos con nombre del mismo tamaño (los precios actuales y los de la lista de 2016).
- La comparación se hace **elemento por elemento**: precio 1 con precio viejo 1, precio 2 con precio viejo 2...
- Sin CSE, en Excel antiguo, solo se compara el valor de la misma fila que la fórmula: **resultado equivocado sin aviso**.
- Se puede evitar el CSE con `SUMPRODUCT`, que ya evalúa matrices por sí sola: `=SUMPRODUCT(--(USD_Price>Old_USD_Price))` (el doble signo menos convierte como el `*1`).

### Tolerar errores: `IFNA`

```
{=SUM(IFNA(H7:H17*I7:I17,0))}            ← suma cantidad × precio, tratando los #N/A como 0
```

Si en la lista hay un producto sin precio, el `INDEX`/`MATCH` da `#N/A` y rompe la suma completa; `IFNA` lo convierte en `0` dentro de la propia matriz.

### Patrones que se repiten

| Problema | Patrón |
|---|---|
| Sumar con una condición, o el producto de dos columnas con una condición | `=SUM(IF(condición, columna1*columna2, 0))` (CSE) |
| Los `n` mayores de un grupo | `=LARGE(IF(grupo=criterio, valores), {1;2;3})` (CSE) |
| Qué elemento corresponde a ese valor | `=INDEX(nombres, MATCH(valor, valores, 0))` |
| Total de una matriz por un vector (cantidades × precios) | `=SUMPRODUCT(cantidades*precios)` o `=SUM(cantidades*precios)` (CSE) |
| Cálculo en dos direcciones (una matriz por una fila o una columna) | `=matriz*fila` (Excel extiende la fila sobre todas las filas) |
| Quedarse con la mitad de una matriz (triangular) | `=IF(contador_fila<=contador_columna, valores, 0)` |

- **`SUMPRODUCT` con coma o con `*`**: `SUMPRODUCT(A,B)` exige matrices del **mismo tamaño**. Para multiplicar una matriz de 49×5 por una fila de 5 se usa `SUMPRODUCT(matriz*fila)`; con coma daría `#VALUE!`.
- **Cruzar un criterio para buscar el nombre**: en `LARGE(IF(...))` + `INDEX/MATCH`, si dos filas de **otro grupo** tienen el mismo valor, el `MATCH` solo por valor puede devolver el nombre equivocado; para evitarlo, se añade la condición del grupo: `MATCH(1,(grupo=criterio)*(valores=k),0)`.
- **Dividir una matriz entre un valor suelto** da una matriz: `=SUMIFS(valores,grupo,lista_de_grupos)/total` devuelve una proporción por grupo, todas a la vez.
- **Contadores de fila y de columna** (los números 1 a 5 a la izquierda y arriba de una cuadrícula) permiten construir condiciones del estilo "fila ≤ columna" sobre toda la cuadrícula: la matriz de `TRUE`/`FALSE` resultante es la mitad superior.

## 6. Atajos, terminología y ninja tips

### Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + Shift + Enter` | Calcular una **fórmula matricial** (CSE) |
| `F2` y luego `Ctrl + Shift + Enter` | Convertir una fórmula existente en fórmula matricial |

### Terminología

- **Array** (matriz): una cuadrícula de cualquier número de celdas. En la barra de fórmulas, los dos puntos entre dos referencias indican una matriz: `A5:A7` es la matriz de las celdas A5, A6 y A7.
- **Array Formula** (fórmula matricial): también llamada fórmula **CSE** (Control-Shift-Enter). Es una fórmula avanzada que se basa en matrices; las llaves `{ }` en la barra de fórmulas indican que lo es.
- **Multi-Cell Array Formula**: fórmula matricial que da **resultados en varias celdas** con una sola fórmula.
- **Single-Cell Array Formula**: tipo más común, que da **un resultado en una sola celda** con una sola fórmula.
- **Array Constant** (constante matricial): una matriz de **valores** en vez de referencias a celdas, entre llaves y separados por punto y coma: `{1;2;3}`.
- **Transpose**: convertir una fila de valores en columna, o al revés.
- **Structured Referencing** (referencia estructurada): referencia que Excel usa en las tablas. Al crear una tabla, Excel nombra la tabla y cada encabezado; las fórmulas pueden usar esos nombres en lugar de direcciones de celda.

### Ninja tips

**Usar `TRUE` y `FALSE` para contar**
- Multiplicar los valores lógicos `TRUE`/`FALSE` por `1` devuelve `1`/`0`, y eso permite usar fórmulas basadas en los valores lógicos originales, que de otro modo no se podría.
- Ejemplo del Toolbox: con dos rangos con nombre del precio actual y el antiguo en USD, la fórmula matricial `{=SUM((USD_Price<Old_USD_Price)*1)}` convierte primero los `TRUE`/`FALSE` de la comparación en `1` y `0`, y luego los suma. Así se **cuenta cuántas bajadas de precio** hubo entre todos los pares de precios actual y antiguo.

**Referencias estructuradas**
- Con referencias estructuradas, Excel mezcla referencias **relativas y absolutas**: en general son **absolutas para las fórmulas copiadas en vertical y relativas para las copiadas en horizontal**.
- Para pasar de relativa a absoluta, hay que **duplicar el nombre del encabezado de la columna** antes de copiar en horizontal. Ejemplo: `TopQtr[@[Best Quarters]:[Best Quarters]]` en vez de solo `TopQtr[@[Best Quarters]]`, donde `TopQtr` es el nombre de la tabla de la que se extrae y `Best Quarters` el encabezado de la columna deseada.

## 7. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Funciones de matriz dinámica de Excel 365**: `FILTER` (filtrar por condición), `SORT` y `SORTBY` (ordenar), `UNIQUE` (valores únicos), `SEQUENCE` (generar 1, 2, 3...), `XLOOKUP` y `LET` (nombres dentro de una fórmula). Hacen sin CSE y casi sin columnas auxiliares lo que esta semana se resuelve con `COUNTIFS` + `MATCH` + `INDEX` o con `LARGE(IF(...))`.
- **Operador de rango derramado `#`**: `=SUM(A1#)` hace referencia a todo el resultado derramado que empieza en `A1`, y se ajusta solo al tamaño.
- **El `@` de Excel 365 no es el `@` de las tablas**: en una fórmula normal, un `@` delante de un rango indica **intersección implícita** (un solo valor); en una tabla, `tbl[@Col]` significa "esta fila". Se parecen, pero hacen cosas distintas.
- **`MMULT`**: multiplicación de matrices, para casos como valores de stock por sucursal en una sola fórmula.
- **Lógica `AND` y `OR` dentro de una matriz**: se usan `*` (todas las condiciones) y `+` (alguna), porque `AND`/`OR` devuelven un solo valor: `SUM((A>5)*(B<3))` cuenta filas con las dos condiciones.
- **`AGGREGATE` y `SUBTOTAL`**: cálculos que ignoran filas ocultas o errores sin necesidad de matriz.
- **Fila de totales de una tabla**: *Diseño de tabla → Fila de totales* usa `SUBTOTAL` y se referencia con `tbl[#Totals]`.
- **Convertir una tabla en rango** (*Diseño de tabla → Convertir en rango*): las referencias estructuradas pasan a referencias normales.
- **Rendimiento**: las fórmulas matriciales sobre rangos muy grandes pueden ralentizar el libro; conviene acotar los rangos y evitar el uso de columnas completas dentro de una matriz.

---

### Práctica
Archivos del curso: `material_curso/modulo-02/`, un libro por video: `W2_01_Structured_Referencing.xlsx`, `W2_02_Sort_using_Functions.xlsx`, `W2_03_Array_Functions_Intro.xlsx`, `W2_04_Transpose.xlsx` y `W2_05_Arrays_for_Solving_Problems.xlsx`. Cada uno parte del estado en que dejó el anterior. En `challenge/` están el reto (`C4-W2-Practice-Challenge.xlsx`) y su solución (`...-Soln.xlsx`); el reto se documenta paso a paso en [practice-challenge.md](practice-challenge.md). El assessment de la semana está en `assessment/` y se documenta por habilidades en [assessment.md](assessment.md): temperaturas de 51 ciudades de EE. UU. con una tabla `City_Subset`, ordenar alfabéticamente con `COUNTIFS`/`MATCH`/`INDEX`, fórmulas matriciales con `LARGE` y `SUM(IF())`, y una tabla cruzada de distancias con `TRANSPOSE`.

### Fuente
Temario del curso, lectura "Course goals and weekly learning objectives" (objetivos de la semana) y lectura "Week 2: Toolbox" (atajos, terminología y ninja tips: sección 6). Las secciones 1-5 se apoyan en los nombres de los videos y en el contenido de los libros del curso (nombres, tablas, fórmulas y cuadros de texto revisados), en el reto, en el assessment y en las dudas que se fueron resolviendo al trabajar la semana. La sección 7 es complemento propio.
