# Chuleta de fórmulas

Funciones y sintaxis vistas hasta ahora, con el módulo donde se explican. Se usan los nombres en inglés de las funciones, como en el curso. En países donde la coma es separador decimal, los argumentos se separan con `;` en vez de `,`.

> Cobertura: Essentials (módulos 1-6), Intermediate I (módulos 1-6) e Intermediate II (módulo 1). Se amplía al cerrar cada módulo de los cursos siguientes.

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

## Por completar

Estas categorías se llenan al avanzar por los cursos siguientes:

- **Lógicas** (`IF`, `AND`, `OR`, `IF` anidados, `IFERROR`) — Intermediate II
- **Búsqueda y referencia** (`VLOOKUP`, `INDEX`, `MATCH`) — Intermediate II
- **Modelado** (`SUMPRODUCT`, Goal Seek, Solver) — Intermediate II
- **Financieras** (`FV`, `PV`, `PMT`, `NPV`, `IRR`, depreciación) — Advanced
