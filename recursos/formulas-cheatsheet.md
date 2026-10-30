# Chuleta de fórmulas

Funciones y sintaxis vistas hasta ahora, con el módulo donde se explican. Se usan los nombres en inglés de las funciones, como en el curso. En países donde la coma es separador decimal, los argumentos se separan con `;` en vez de `,`.

> Cobertura: Essentials (módulos 1-6), Intermediate I (módulos 1-6) e Intermediate II (módulos 1-4). Se amplía al cerrar cada módulo de los cursos siguientes.

## Básicas y cálculo

| Fórmula | Sintaxis | Para qué sirve | Dónde |
|---|---|---|---|
| Operadores | `+ - * /` | Aritmética. `*` y `/` se calculan antes que `+` y `-`: `=3+4*5` da 23, `=(3+4)*5` da 35 | [Essentials M2](../01-essentials/notas/modulo-02/02-calculations.md) |
| `SUM` | `=SUM(A1:A12)` | Suma un rango | [Essentials M2](../01-essentials/notas/modulo-02/02-calculations.md) |
| `AVERAGE` | `=AVERAGE(A1:A12)` | Promedio | [Essentials M2](../01-essentials/notas/modulo-02/02-calculations.md) |
| `MIN` / `MAX` | `=MIN(A1:A12)` | Valor más pequeño / más grande | [Essentials M2](../01-essentials/notas/modulo-02/02-calculations.md) |
| `RANDBETWEEN` | `=RANDBETWEEN(bottom, top)` | Número entero al azar entre dos valores | [Essentials M2](../01-essentials/notas/modulo-02/02-calculations.md) |

## Conteo y criterios

| Fórmula | Sintaxis | Para qué sirve | Dónde |
|---|---|---|---|
| `COUNT` | `=COUNT(rango)` | Celdas con un **número** | [Intermediate I M4](../02-intermediate-1/notas/modulo-04/04-summarising-data.md) |
| `COUNTA` | `=COUNTA(rango)` | Celdas **no vacías** | [Intermediate I M4](../02-intermediate-1/notas/modulo-04/04-summarising-data.md) |
| `COUNTBLANK` | `=COUNTBLANK(rango)` | Celdas **vacías** | [Intermediate I M4](../02-intermediate-1/notas/modulo-04/04-summarising-data.md) |
| `COUNTIFS` | `=COUNTIFS(criteria_range1, criteria1, [criteria_range2, criteria2]…)` | Cuenta las filas que cumplen **todos** los criterios (`COUNTIF` es la versión de un solo criterio) | [Intermediate I M4](../02-intermediate-1/notas/modulo-04/04-summarising-data.md) |
| `SUMIFS` | `=SUMIFS(sum_range, criteria_range1, criteria1, …)` | Suma solo las filas que cumplen los criterios. **El rango a sumar va primero** (en la antigua `SUMIF` va al final) | [Intermediate I M4](../02-intermediate-1/notas/modulo-04/04-summarising-data.md) |
| `AVERAGEIFS` | `=AVERAGEIFS(average_range, criteria_range1, criteria1, …)` | Promedio con criterios (mismo orden que `SUMIFS`) | [Intermediate I M4](../02-intermediate-1/notas/modulo-04/04-summarising-data.md) |

**Patrones útiles con criterios**:
- **Ganancia por grupo** (precio − costo): `=SUMIFS(precio, estado, M10) - SUMIFS(costo, estado, M10)`. El criterio apunta a la celda de una lista desplegable, no a un texto fijo — [Intermediate II M1](../03-intermediate-2/notas/modulo-01/assessment.md).
- **Evitar duplicados** (validación personalizada): `=COUNTIF($B$2:$B$61,B2)=1` — [Intermediate II M1](../03-intermediate-2/notas/modulo-01/01-data-validation.md).

**Operadores en los criterios** (con comillas): `"=50"`, `"<>50"`, `">50"`, `">=50"`, `"<50"`, `"<=50"`. Con fechas: `"<2015-01-01"`. Con una celda: `">"&B1`. **Comodines** (solo texto): `?` = una letra, `*` = varias (`"Small *"`). No distinguen mayúsculas/minúsculas.

## Texto

| Fórmula | Sintaxis | Para qué sirve | Dónde |
|---|---|---|---|
| `CONCAT` / `&` | `=CONCAT(A2,D2,G2)` · `=A2&" "&B2` | Une texto. `CONCAT` admite rangos; `CONCATENATE` es la versión antigua | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `TEXTJOIN` | `=TEXTJOIN(separador, ignorar_vacías, texto1, …)` | Une con un separador único y puede ignorar celdas vacías | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `UPPER` / `LOWER` / `PROPER` | `=PROPER("hola mundo")` | MAYÚSCULAS / minúsculas / Primera Letra En Mayúscula | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `LEFT` / `RIGHT` | `=LEFT(texto, núm_caracteres)` | Extrae desde el inicio / desde el final | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `MID` | `=MID(texto, posición_inicial, núm_caracteres)` | Extrae desde una posición intermedia | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `FIND` | `=FIND(texto_buscado, dentro_del_texto, [núm_inicial])` | Posición de un texto dentro de otro (distingue mayúsculas) | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `SEARCH` | `=SEARCH(...)` | Como `FIND`, sin distinguir mayúsculas y con comodines | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `LEN` | `=LEN(texto)` | Cantidad de caracteres | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `TRIM` / `CLEAN` | `=TRIM(texto)` | Quita espacios sobrantes / caracteres no imprimibles | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `VALUE` / `TEXT` | `=TEXT(número, formato)` | Texto → número / número → texto con formato | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `CHAR(10)` | `=A2&CHAR(10)&B2` | Salto de línea dentro de una fórmula (activar *Ajustar texto*) | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |

**Funciones anidadas**: una función dentro de otra, resuelta de adentro hacia afuera. Ej.: `=MID(A2,2,FIND(" ",A2))`, donde `FIND` calcula el tercer argumento de `MID`.

## Fecha y hora

| Fórmula | Sintaxis | Para qué sirve | Dónde |
|---|---|---|---|
| `TODAY` | `=TODAY()` | Fecha de hoy, se actualiza al abrir el archivo (la fija es `Ctrl + ;`) | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `NOW` | `=NOW()` | Fecha y hora actuales, se actualiza (la fija es `Ctrl + Shift + ;`) | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `YEARFRAC` | `=YEARFRAC(fecha_inicio, fecha_fin)` | Fracción de año entre dos fechas | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| `WEEKDAY`, `DATEDIF`, `EDATE`, `EOMONTH` | — | Día de la semana, diferencia entre fechas, sumar meses, fin de mes (avance; se profundizan en Advanced) | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |

## Referencias

| Tipo | Ejemplo | Comportamiento | Dónde |
|---|---|---|---|
| Relativa | `D4` | Cambia al copiar (`E4` a la derecha, `D5` hacia abajo) | [Essentials M2](../01-essentials/notas/modulo-02/02-calculations.md) |
| Absoluta | `$D$4` | No cambia al copiar | [Essentials M2](../01-essentials/notas/modulo-02/02-calculations.md) |
| Mixta | `$D4` · `D$4` | Fija solo la columna / solo la fila | [Intermediate I M4](../02-intermediate-1/notas/modulo-04/04-summarising-data.md) |
| Otra hoja | `=Ventas!B5` · `='Enero 2026'!B5` | Comillas simples si el nombre tiene espacios | [Essentials M2](../01-essentials/notas/modulo-02/02-calculations.md) |
| 3D | `=SUM(Enero:Diciembre!B5)` | La misma celda/rango a través de hojas **consecutivas** del mismo libro | [Intermediate I M1](../02-intermediate-1/notas/modulo-01/01-multiple-worksheets-workbooks.md) |
| Otro libro | `='[Libro.xlsx]Hoja'!B5` | Vínculo externo; con el libro cerrado incluye la ruta completa. Se actualiza con Datos > Editar vínculos | [Intermediate I M1](../02-intermediate-1/notas/modulo-01/01-multiple-worksheets-workbooks.md) |
| Rango con nombre | `=N4*Pension_Rate` | Se comporta como referencia **absoluta** y hace la fórmula legible | [Intermediate I M3](../02-intermediate-1/notas/modulo-03/03-named-ranges.md) |
| Referencia estructurada | `=SUM(Ventas[Importe])` · `[@Importe]` | Columna de una tabla por nombre; `[@...]` es "esta fila". Se comporta como **relativa** y la tabla se extiende sola | [Intermediate I M5](../02-intermediate-1/notas/modulo-05/05-tables.md) |

## Herramientas que sustituyen o complementan fórmulas

| Herramienta | Cuándo usarla | Dónde |
|---|---|---|
| Consolidar (Datos > Consolidar) | Combinar varios rangos/libros: sin etiquetas (por posición), con **Columna izquierda**, **Fila superior** o **ambas** según qué eje no esté alineado | [Intermediate I M1](../02-intermediate-1/notas/modulo-01/01-multiple-worksheets-workbooks.md) |
| Texto en columnas | Dividir una columna en varias, una sola vez (para algo dinámico, usar `LEFT`/`MID`/`RIGHT`) | [Intermediate I M2](../02-intermediate-1/notas/modulo-02/02-text-date-functions.md) |
| Subtotal | Subtotales por grupo en un **rango normal** (hay que convertir la tabla a rango y ordenar antes) | [Intermediate I M5](../02-intermediate-1/notas/modulo-05/05-tables.md) |
| Tabla dinámica | Resumir y explorar muchos datos; varias funciones por campo, Top N, "Mostrar valores como" | [Intermediate I M6](../02-intermediate-1/notas/modulo-06/06-pivot-tables-charts-slicers.md) |
| Validación de datos (Datos > Validación de datos) | Limitar lo que acepta una celda: tipo (decimal, entero, fecha, lista, longitud o personalizada con fórmula), mensaje de entrada y de error. Solo controla lo que se **escribe**; para revisar datos ya existentes, *Rodear con un círculo datos no válidos* | [Intermediate II M1](../03-intermediate-2/notas/modulo-01/01-data-validation.md) |
| Formato condicional con fórmula | Resaltar celdas según una fórmula VERDADERO/FALSO, incluso comparando contra otra celda (ej. `$N$14`) | [Intermediate II M1](../03-intermediate-2/notas/modulo-01/01-data-validation.md) |

## Lógicas

| Fórmula | Sintaxis | Para qué sirve | Dónde |
|---|---|---|---|
| `IF` | `=IF(prueba_lógica, valor_si_verdadero, valor_si_falso)` | Hace una cosa si se cumple la condición y otra si no. `""` deja la celda vacía | [Intermediate II M2](../03-intermediate-2/notas/modulo-02/02-conditional-logic.md) |
| `AND` | `=AND(prueba1, prueba2, …)` | VERDADERO si **todas** las condiciones se cumplen | [Intermediate II M2](../03-intermediate-2/notas/modulo-02/02-conditional-logic.md) |
| `OR` | `=OR(prueba1, prueba2, …)` | VERDADERO si **al menos una** se cumple | [Intermediate II M2](../03-intermediate-2/notas/modulo-02/02-conditional-logic.md) |
| `NOT` | `=IF(NOT(A5=1), B5*5%, 0)` | Invierte una prueba lógica | [Intermediate II M2](../03-intermediate-2/notas/modulo-02/02-conditional-logic.md) |
| `EXACT` | `=IF(EXACT(A5,"JOHN"), B5*5%, 0)` | Compara dos textos **distinguiendo mayúsculas/minúsculas** (con `=` no se distingue) | [Intermediate II M2](../03-intermediate-2/notas/modulo-02/02-conditional-logic.md) |
| `IF` anidado | `=IF(B4>=90,"A", IF(B4>=75,"B", IF(B4>=60,"C","D")))` | Varios tramos; las pruebas van de la más restrictiva a la menos | [Intermediate II M2](../03-intermediate-2/notas/modulo-02/02-conditional-logic.md) |
| `IFERROR` | `=IFERROR(cálculo, valor_si_hay_error)` | Reemplaza cualquier error por un valor o mensaje | [Intermediate II M2](../03-intermediate-2/notas/modulo-02/02-conditional-logic.md) |
| `IFNA` | `=IFNA(cálculo, valor)` | Igual, pero solo para `#N/A` | [Intermediate II M2](../03-intermediate-2/notas/modulo-02/02-conditional-logic.md) |
| `IFS`, `SWITCH`, `ISBLANK` | — | Alternativas modernas a los `IF` anidados / prueba de celda vacía (avance) | [Intermediate II M2](../03-intermediate-2/notas/modulo-02/02-conditional-logic.md) |

**Operadores lógicos**: `=` igual (no distingue mayúsculas), `>`, `<`, `>=`, `<=`, `<>` distinto (no distingue mayúsculas).

**Patrones y trampas**:
- `IF` con `IFERROR` dentro: `=IF(AB2="Call", IFERROR(total/num, "not called yet"), "")`. El `IF` externo decide a quién aplica; el `IFERROR`, qué mostrar si falla.
- `TRUE` sin comillas es un valor lógico; `"TRUE"` con comillas es texto. Contar: `=COUNTIF(rango,TRUE)`.
- Un texto es "mayor" que cualquier número en una comparación: `"not called yet" > 7` da VERDADERO; hay que excluirlo antes (`AF2<>"not called yet"`).

## Búsqueda y referencia

| Fórmula | Sintaxis | Para qué sirve | Dónde |
|---|---|---|---|
| `CHOOSE` | `=CHOOSE(index_num, valor1, valor2, …)` | Toma un valor de una lista según un número; las celdas van **una por una** (`B4,B5,B6`, no `B4:B6`) | [Intermediate II M3](../03-intermediate-2/notas/modulo-03/03-automating-lookups.md) |
| `VLOOKUP` por rango | `=VLOOKUP(valor, tabla, col_index)` | Usa el mayor valor **menor o igual** al buscado; la primera columna debe estar **ordenada**. Es el comportamiento por defecto | [Intermediate II M3](../03-intermediate-2/notas/modulo-03/03-automating-lookups.md) |
| `VLOOKUP` exacto | `=VLOOKUP(valor, tabla, col_index, FALSE)` | Coincidencia exacta, para IDs y datos únicos; da `#N/A` si no lo encuentra | [Intermediate II M3](../03-intermediate-2/notas/modulo-03/03-automating-lookups.md) |
| `MATCH` | `=MATCH(lookup_value, lookup_array, [match_type])` | Devuelve la **posición**: `0` exacta, `1` menor (datos ascendentes, por defecto), `-1` mayor (datos descendentes) | [Intermediate II M3](../03-intermediate-2/notas/modulo-03/03-automating-lookups.md) |
| `INDEX` | `=INDEX(array, row_num, [col_num])` | Devuelve el valor que está en una posición dada. Con `0` en fila o columna devuelve una fila o columna entera | [Intermediate II M3](../03-intermediate-2/notas/modulo-03/03-automating-lookups.md) |
| `INDEX` + `MATCH` | `=INDEX(rango_a_devolver, MATCH(valor, rango_donde_buscar, 0))` | Búsqueda que puede devolver valores a la **izquierda** y no se rompe al insertar columnas | [Intermediate II M3](../03-intermediate-2/notas/modulo-03/03-automating-lookups.md) |
| `INDEX` + 2 `MATCH` | `=INDEX(matriz, MATCH(fila,…,0), MATCH(columna,…,0))` | Búsqueda en **dos sentidos**; el primer `MATCH` da la fila y el segundo la columna | [Intermediate II M3](../03-intermediate-2/notas/modulo-03/03-automating-lookups.md) |

**Búsqueda por rango vs. `IF` anidados**: `=VLOOKUP(D4,$G$4:$H$7,2)` reemplaza a `=IF(D4<$G$5,$H$4,IF(D4<$G$6,$H$5,IF(D4<$G$7,$H$6,$H$7)))` y es más fácil de revisar y editar.

**Cuidados**: el `table_array` casi siempre va **absoluto** (`$G$4:$H$7`); los rangos de `INDEX` + `MATCH` deben tener el mismo tamaño; `VLOOKUP` por rango con datos sin ordenar da un resultado incorrecto **sin error**.

## Errores y auditoría de fórmulas

| Error | Significado | Dónde |
|---|---|---|
| `#DIV/0!` | División entre cero o entre una celda vacía | [Intermediate II M4](../03-intermediate-2/notas/modulo-04/04-formula-auditing-protection.md) |
| `#NAME?` | Texto que Excel no reconoce (nombre de función mal escrito) | [Intermediate II M4](../03-intermediate-2/notas/modulo-04/04-formula-auditing-protection.md) |
| `#VALUE!` | Un valor de la fórmula es del tipo de dato equivocado | [Intermediate II M4](../03-intermediate-2/notas/modulo-04/04-formula-auditing-protection.md) |
| `#N/A` | Una función de búsqueda no encontró el valor | [Intermediate II M3](../03-intermediate-2/notas/modulo-03/03-automating-lookups.md) |
| `#REF!` | La fórmula apunta a una celda que fue borrada | [Intermediate II M4](../03-intermediate-2/notas/modulo-04/04-formula-auditing-protection.md) |
| `#####` | No es un error: la columna es demasiado estrecha para mostrar el número | [Intermediate II M4](../03-intermediate-2/notas/modulo-04/04-formula-auditing-protection.md) |

**Herramientas de Formulas > Auditoría de fórmulas** ([Intermediate II M4](../03-intermediate-2/notas/modulo-04/04-formula-auditing-protection.md)):
- **Rastrear precedentes / dependientes**: flechas desde o hacia la celda activa (`Ctrl + [` y `Ctrl + ]` para ir a ellas); doble clic en una flecha selecciona el otro extremo, y las flechas desaparecen al editar.
- **Evaluar fórmula**: recorre una fórmula paso a paso; las funciones volátiles (`RAND`, `INDEX`, `OFFSET`, `INDIRECT`, `NOW`, `TODAY`, `RANDBETWEEN`…) pueden dar resultados distintos de la celda.
- **Comprobación de errores**, **Mostrar fórmulas** (`` Ctrl + ` ``) y **Ventana de inspección**.
- **Opciones de cálculo**: Automático o Manual; `F9` recalcula todos los libros y `Shift + F9` solo la hoja. **Referencia circular**: una fórmula que se refiere a su propia celda; se resuelve con el cálculo iterativo o corrigiendo la fórmula.
- **Protección**: Revisar > Proteger hoja / Proteger libro; solo bloquea las celdas *Bloqueadas* cuando la hoja está protegida, y la contraseña no se puede recuperar.

## Por completar

Estas categorías se llenan al avanzar por los cursos siguientes:

- **Modelado** (`SUMPRODUCT`, Goal Seek, Solver) — Intermediate II
- **Financieras** (`FV`, `PV`, `PMT`, `NPV`, `IRR`, depreciación) — Advanced
