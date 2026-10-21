# Módulo 2 · Conditional Logic

> Los videos de esta semana, según los archivos en `material_curso/modulo-02/`, son: **V1 IF · V2 AND and OR · V3 IF with AND and OR · V4 Nested IF · V5 IFERROR**. Las secciones 6-9 son material real de la lectura "Week 2: Toolbox"; las secciones 1-5 se redactaron con conocimiento general apoyado en los nombres de los videos, así que algún detalle puntual podría no calzar 100% con lo que muestra el curso. La sección 10 sale de las preguntas del assessment que se resolvieron en conversación.

## 1. La función IF (V1)

**Conditional Logic (lógica condicional)**: controlar lo que muestra una celda según se cumpla o no una condición. `IF` hace una cosa si la condición se cumple y otra, que hay que especificar, si no.

```
=IF(prueba_lógica, valor_si_verdadero, valor_si_falso)
```

- **Prueba lógica (Logical Test)**: lo que se le pide a Excel comprobar, y que da verdadero o falso. Es el primer argumento. Ejemplo: `B4>5`.
- **Valor si verdadero / valor si falso**: pueden ser un número, un **texto entre comillas**, un cálculo, otra función, o `""` para dejar la celda **vacía**.

Ejemplos:

```
=IF(B4>5,"Alto","Bajo")
=IF(A5<>"","Ya registrado","")
=IF(B4>100000,B4*5%,0)
```

**Operadores lógicos** (la relación matemática de la prueba):

| Operador | Significado |
|---|---|
| `=` | Igual (sirve también con texto; no distingue mayúsculas/minúsculas) |
| `>` | Mayor que |
| `<` | Menor que |
| `>=` | Mayor o igual que |
| `<=` | Menor o igual que |
| `<>` | Distinto de (sirve también con texto; no distingue mayúsculas/minúsculas) |

## 2. AND y OR (V2)

Son **funciones lógicas**: devuelven VERDADERO o FALSO según si se cumple un conjunto de condiciones.

| Función | Devuelve VERDADERO cuando... | Ejemplo |
|---|---|---|
| `AND(prueba1, prueba2, …)` | **Todas** las condiciones son verdaderas | `=AND(B4>5, C4="Sí")` |
| `OR(prueba1, prueba2, …)` | **Al menos una** condición es verdadera | `=OR(B4>5, C4="Sí")` |

Solas devuelven VERDADERO/FALSO; su utilidad principal es combinarse dentro de un `IF` (sección 3).

## 3. IF con AND y OR (V3)

`AND` u `OR` ocupan el lugar de la **prueba lógica** del `IF`, para decidir con varias condiciones a la vez:

```
=IF(AND(B4>5, C4="Sí"), "Aprobado", "Revisar")      todas deben cumplirse
=IF(OR(B4>5, C4="Sí"), "Aprobado", "Revisar")       basta con una
```

## 4. IF anidados (V4)

Un `IF` dentro de otro permite controlar más la salida. Con dos `IF` anidados dentro de un tercero, si la primera condición (la del `IF` más externo) no se cumple, Excel todavía tiene otras dos condiciones por probar.

```
=IF(B4>=90,"A", IF(B4>=75,"B", IF(B4>=60,"C","D")))
```

- Cada `IF` nuevo va en el argumento de **valor si falso** del anterior.
- **El orden de las pruebas importa**: Excel se detiene en la primera que se cumple, así que se ordenan de la más restrictiva a la menos restrictiva.
- Cuantos más niveles, más difícil de leer; por eso conviene no abusar y revisar cada paréntesis.

## 5. IFERROR (V5)

`IFERROR` captura un error en un cálculo y lo reemplaza por lo que elijas:

```
=IFERROR(cálculo, valor_si_hay_error)
=IFERROR(B4/C4, "sin datos")
```

- Atrapa errores como `#DIV/0!`, `#N/A`, `#VALUE!`, `#REF!` o `#NAME?`.
- `IFNA` hace lo mismo pero **solo para `#N/A`**, útil cuando se quiere ocultar únicamente el error de "valor no encontrado" de una búsqueda y dejar visibles los demás.
- **Cuidado**: `IFERROR` oculta **todos** los errores, incluso los que indican un fallo real en la fórmula. Conviene usarlo cuando se sabe qué error se espera (por ejemplo, la división entre cero cuando todavía no hay datos).

## 6. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + Shift + +` (Mac: `Control + I`) | Con una fila o columna **entera seleccionada**, insertar una fila o columna nueva |
| `Ctrl + -` (Windows y Mac) | Con una fila o columna seleccionada, eliminarla |

## 7. Terminología

- **Conditional Logic (lógica condicional)**: forma de controlar el resultado de una celda según se cumpla o no una condición. `IF` hace una cosa si se cumple y, si no, otra que hay que especificar.
- **Argument (argumento)**: en una función como `IF`, la entrada de la función. Por ejemplo, la prueba lógica es uno de los argumentos de `IF`.
- **Logical Test (prueba lógica)**: lo que se le indica a Excel que compruebe para saber si es verdadero o falso. Es el primer argumento de `IF`. Ejemplo: `B4>5`.
- **Logical operator (operador lógico)**: la relación matemática usada en la prueba lógica; por ejemplo `<>` significa "distinto de". Ver la tabla de la sección 1.
- **Nested functions (funciones anidadas)**: una función dentro de otra. Con `IF`, ayuda a controlar aún más la salida de una celda.
- **Logical functions (funciones lógicas)**: funciones que devuelven VERDADERO/FALSO según se cumpla un conjunto de condiciones. Con `AND` deben cumplirse todas; con `OR` basta con una.
- **Logical value (valor lógico)**: no es lo mismo que un texto; un valor lógico **no va entre comillas**. Ejemplo: `TRUE`. El valor lógico `TRUE` es distinto de la palabra `"TRUE"`: las comillas le dicen a Excel que la trate como texto, no como valor lógico.

## 8. Ninja tips de la semana: más funciones lógicas

**`NOT`**: se usa dentro de un `IF`, igual que `AND` y `OR`, para indicar que una prueba lógica **no** es verdadera:

```
=IF(NOT(A5=1), B5*5%, 0)
```
Se lee: "si el valor de `A5` no es igual a 1, multiplica `B5` por 5%; si no, devuelve 0".

**`EXACT`**: al comparar dos textos con `=`, Excel **ignora las mayúsculas y minúsculas**. Si se necesita que sean exactamente iguales, incluida la capitalización, se usa `EXACT`, con los dos textos separados por coma:

```
=IF(EXACT(A5,"JOHN"), B5*5%, 0)
```
Se lee: "si el valor de `A5` es exactamente JOHN, multiplica `B5` por 5%; si no, devuelve 0".

## 9. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **`IFS` y `SWITCH`**: alternativas más legibles a los `IF` anidados, disponibles en versiones recientes de Excel. `IFS` encadena pares prueba/resultado sin anidar paréntesis; `SWITCH` compara un valor contra una lista de opciones.
- **`ISBLANK`**: prueba si una celda está vacía (`=IF(ISBLANK(A5),"falta dato","ok")`); alternativa a `A5=""`.
- **Los valores lógicos funcionan como números**: `TRUE` vale 1 y `FALSE` vale 0, así que se puede multiplicar: `=(B4>5)*100` devuelve 100 si se cumple y 0 si no.
- **`IF` frente a `COUNTIFS`/`SUMIFS`**: `IF` decide **celda por celda**; `COUNTIFS`/`SUMIFS` resumen **muchas filas** de una vez según criterios. Se combinan con frecuencia.
- **Fechas en la prueba lógica**: comparar con una celda que contenga la fecha o con la función `DATE(año,mes,día)` es más seguro que escribir la fecha como texto.
- **Depurar fórmulas largas**: seleccionar una parte de la fórmula en la barra y pulsar `F9` muestra su resultado (con `Esc` se cancela). Se ve con más detalle en el Módulo 4 (Formula Auditing).

## 10. Lo aprendido al practicar (assessment)

Al resolver preguntas del assessment aparecieron estas ideas, que sirven para cualquier caso parecido:

- **`""` como "vacío"**: `=IF(AB2="Call","interest potential","")` deja la celda en blanco cuando no se cumple la condición. Para el programa esa celda **no está realmente vacía** (contiene una fórmula), pero se ve vacía.
- **Lista desplegable y fórmula en la misma columna**: la validación de datos solo controla lo que se **escribe o elige**, no lo que calcula una fórmula, así que conviven. Al elegir un valor del desplegable en una celda con fórmula, la **fórmula se reemplaza** por el valor.
- **Contar "ignorando las vacías"**: para ubicar la enésima celda con contenido, filtrar la columna por ese valor (`Ctrl + Shift + L`) y contar las filas visibles.
- **`IF` con `IFERROR` dentro**: `=IF(condición, IFERROR(cálculo, "mensaje"), "")` evita un `#DIV/0!` cuando aún no hay datos y deja vacía la celda de quienes no aplican. El `IF` externo decide **a quién aplica** y el `IFERROR` interno decide **qué mostrar si falla el cálculo**.
- **`AVERAGE` ignora el texto y las celdas vacías**: un promedio sobre una columna que mezcla números, `"not called yet"` y `""` solo promedia los números.
- **Referencia circular**: la celda con el promedio no puede estar dentro del rango que promedia.
- **Formato personalizado `#.##`**: es un cambio de **formato visual** (`Ctrl + 1` > Número > Personalizada); no altera el valor, a diferencia de `ROUND`.

---

### Práctica
Archivos del curso: `material_curso/modulo-02/`. Ningún video trae versión `Soln`. Hay un reto de práctica con solución en `challenge/` y el assessment de la semana en `assessment/` (documentado por habilidades en [assessment.md](assessment.md)).

### Fuente
Lectura "Week 2: Toolbox" del curso (atajos, terminología, ninja tips de `NOT` y `EXACT` — secciones 6, 7 y 8). Secciones 1-5 se basan en los nombres de los videos, redactadas con conocimiento general. Sección 9 es complemento propio. Sección 10 sale de preguntas del assessment resueltas en conversación (sin respuestas).
