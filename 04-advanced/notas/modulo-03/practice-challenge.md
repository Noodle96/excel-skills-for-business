# Módulo 3 · Practice Challenge: base de datos de estudiantes

> Reto de la semana (`material_curso/modulo-03/challenge/C4-W3-Practice-Challenge.xlsx`). Este documento describe los 13 pasos, el método que usa cada uno, las fórmulas probadas y los problemas que aparecen. El reto se hizo de forma independiente (no paso a paso con Claude); las fórmulas se revisaron sobre una copia del libro del usuario y sobre el libro de solución del curso.

**Escenario**: hay que preparar **tablas dinámicas** a partir de la hoja `Student database` (153 estudiantes). Los datos llegan sucios y el reto repasa todo el módulo: blancos, fechas, espacios, caracteres no imprimibles y números guardados como texto. Objetivos de la semana: arreglar fechas con funciones, reemplazar blancos con valores repetidos y quitar espacios y caracteres no deseados.

## Los datos de partida

| Columna | Contenido | Problema |
|---|---|---|
| `Student ID` | `K150100`, `K150101`... | Una `K` al inicio que hay que quitar |
| `Name` | `Mr.  Behrouz  COURT   ` | Espacios dobles y al final |
| `Unit` | `Intro to ECommerce` | (Solo en la versión del usuario del libro) |
| `Campus` | `Sydney     `, `   Brisbane` | Espacios delante y detrás |
| `First enrolment trimester` | `T2-2015` | No es una fecha |
| `Number of workshop hours` | `1  `, `22  ` | **Texto** con espacios y `CHAR(160)` |
| `Course` | Solo escrito en la primera fila de cada grupo | Celdas **en blanco** |
| `Date of Birth` | `1995-10-24` | **Texto** con guiones |
| `AgentName` | Con huecos | Celdas **en blanco** |

La hoja `Instructions` trae una tabla con el inicio de cada trimestre (en las celdas `B28:D28`):

| T1 | T2 | T3 |
|---|---|---|
| 13 de marzo | 10 de julio | 6 de noviembre |

## Los 13 pasos

### 1. Rellenar `Course` con *Go To Special*

1. Seleccionar la columna `Course` (con los datos).
2. `Ctrl + G` → *Special* → **Blanks**.
3. Escribir `=` y pulsar la flecha ↑ (apunta a la celda de arriba), y terminar con `Ctrl + Enter`. Las celdas vacías seleccionadas se rellenan a la vez, cada una con la celda de arriba.
4. Copiar la columna y pegarla con ***Paste Values*** sobre sí misma. Es la parte que no hay que olvidar: sin ella siguen siendo fórmulas del estilo `=G2`.

### 2. Copiar la hoja y enlazarla

Duplicar `Student database`, llamar a la copia `Database edit` y enlazar cada celda con la original: `='Student database'!A2`, arrastrada a todas las columnas y filas. Así la original queda intacta y las correcciones se hacen siempre en la copia, que es el método del módulo.

> **Trampa**: enlazar una celda **vacía** devuelve **`0`**, no una celda vacía. Por eso en el paso 3 el `ISBLANK` se aplica a la hoja **original**, no a la copia enlazada.

### 3. `AgentName`: repetir el agente de arriba

```
=IF(ISBLANK('Student database'!I3), J2, 'Student database'!I3)
```

Es el patrón del V1: si la celda original está vacía, repite el valor ya rellenado de la fila de arriba (`J2`, en la hoja nueva); si no, usa el original. Se copia hacia abajo.

### 4. Convertir el trimestre en fecha

Insertar una columna a la derecha de `First enrolment trimester` llamada `First enrolment date`. Las pistas del reto: usar `DATE`, sacar el año con `RIGHT`, y el día y el mes con `IF`, `MID`, `LEFT` y `RIGHT`.

La solución del curso anida `IF` con texto: `=DATE(RIGHT(D2,4), RIGHT(IF(...,"13/03",...),2), LEFT(IF(...),2))`. Una versión más legible usa `IFS` y toma el día y el mes **de la tabla de la hoja `Instructions`**, así no se escriben las fechas a mano:

```
=IFS(
  LEFT(E2,2)="T1", DATE(RIGHT(E2,4), MONTH(Instructions!$B$28), DAY(Instructions!$B$28)),
  LEFT(E2,2)="T2", DATE(RIGHT(E2,4), MONTH(Instructions!$C$28), DAY(Instructions!$C$28)),
  LEFT(E2,2)="T3", DATE(RIGHT(E2,4), MONTH(Instructions!$D$28), DAY(Instructions!$D$28)))
```

- `LEFT(E2,2)` da `T2`, `RIGHT(E2,4)` da `2015`.
- El año que llega con `RIGHT` es **texto**; `DATE` lo convierte solo.
- Si la fecha de inicio de un trimestre cambia, basta con cambiarla en la tabla.

Ejemplo del enunciado: `T2-2015` empezó el 10 de julio de 2015.

> **Error en la solución oficial**: en el libro de solución, la fórmula trata `1` como "cualquier otro" y le da el 6 de noviembre, así que `T1-2014` sale como **6/11/2014** en lugar de 13/03/2014. Los trimestres T1 y T3 están **intercambiados**. Por eso en su tabla dinámica de meses `Mar` y `Nov` aparecen con los conteos cambiados. La respuesta `July` no cambia, porque T2 está bien. Conviene fiarse de la tabla del enunciado y no de la solución en este punto.

### 5. Mes de inscripción

```
=TEXT(F2,"mmm")
```

Devuelve las **tres primeras letras** del mes (`jul`, `mar`, `nov` en este Excel). El resultado es texto. Con `"mmmm"` saldría el nombre completo.

### 6. Edad en el primer día de inscripción

Añadir la columna `Age at first enrolment`. La pista de esta versión del reto es `YEARFRAC`; la solución del curso usa `YEAR`.

- `Date of Birth` es **texto** (`1995-10-24`); lo primero es convertirlo en fecha: `=DATE(LEFT(I2,4), MID(I2,6,2), RIGHT(I2,2))`, o `DATEVALUE(I2)` (que depende de la configuración regional pero lee bien el formato `yyyy-mm-dd`).
- **`YEAR(inscripción) - YEAR(nacimiento)`** (la fórmula de la solución) solo resta años del calendario: ignora si el estudiante ya cumplió años ese año. Verificado: **difiere de la edad real en 74 de los 153 estudiantes**.
- **`=INT(YEARFRAC(nacimiento, inscripción))`** da la edad en años cumplidos. Comprobado contra `DATEDIF(nacimiento, inscripción, "y")`: coincide en los 153 casos. Sin `INT`, `YEARFRAC` devuelve decimales (por ejemplo 20,7), y la tabla dinámica agruparía por edades fraccionarias.

La conclusión del paso 13 (la edad más frecuente) **no cambia** con ninguna de las dos fórmulas.

### 7. Tabla dinámica `Students by Campus`

Una tabla dinámica en una hoja nueva, llamada `Students by Campus`, con `Campus` en filas y el recuento de estudiantes en valores.

### 8. Limpiar `Campus` y actualizar la tabla

La primera tabla muestra el mismo campus **más de una vez** (`Sydney`, `Sydney     `, `   Sydney`...) por los espacios. En la columna `Campus` de `Database edit`:

```
=TRIM(CLEAN('Student database'!D2))
```

Después, **actualizar la tabla** (*Analyze → Refresh*, o `Alt + F5`): sin ese paso sigue mostrando los duplicados. Si hace falta, volver a elegir la columna limpia como campo de la tabla dinámica.

### 9. Quitar la `K` del ID

```
=SUBSTITUTE('Student database'!A2, "K", "")
```

`K150100` pasa a `150100`. Es la pista de la solución, pero hay dos matices:
- `SUBSTITUTE` quita **todas** las `K` del texto, no solo la primera. Aquí no importa porque los IDs son `K` + dígitos, pero para quitar solo la inicial es más seguro `=IF(LEFT(A2,1)="K", MID(A2,2,LEN(A2)), A2)`.
- Los IDs que no empiezan por `K` quedan igual.
- El resultado es **texto** (`"150100"`). Si se necesitara como número, envolverlo en `VALUE`.

### 10. Horas de taller que faltan

Nueva columna `Remaining workshop hours`: cada estudiante debe hacer al menos 20 horas, y si ya hizo más, lo que falta es 0.

```
=IF(G2>=20, 0, 20-G2)
```

Con los datos tal cual, **todos los valores salen 0**. Es lo que anticipa la pista: las horas están en **texto**, y en Excel un texto siempre se considera mayor que cualquier número en una comparación, así que `G2>=20` es `TRUE` siempre.

### 11. Convertir las horas de texto en número

Es el diagnóstico del V4 y la limpieza del V5. Las pistas del reto, en orden: comprobar el número de caracteres (`LEN`), buscar el código de los caracteres no imprimibles (`CODE`) y usar `CHAR`, `SUBSTITUTE`, `TRIM` y `VALUE`:

```
=VALUE(TRIM(SUBSTITUTE('Student database'!F2, CHAR(160), "")))
```

Un `"1  "` tiene más caracteres de los que se ven, y el último tiene el código **160** (espacio de no separación), que `TRIM` no quita por sí solo. La fórmula funciona porque quita los 160, limpia los espacios normales y convierte en número. Después de este paso, la fórmula del paso 10 sí calcula.

### 12. Limpiar `Name`

```
=TRIM(CLEAN('Student database'!B2))
```

`Mr.  Behrouz  COURT   ` pasa a `Mr. Behrouz COURT`. Los espacios dobles entre palabras se reducen a uno, y los de los extremos desaparecen.

### 13. Responder con tablas dinámicas

| Pregunta | Respuesta | Cómo se obtiene |
|---|---|---|
| Mes de inscripción más popular en el campus de Sydney | **July** (21 estudiantes) | Tabla con el mes en filas y el campus en columnas (recuento) |
| Total de horas de taller que faltan en el campus de Brisbane | **672** | Tabla con el campus en filas y la **suma** de `Remaining workshop hours` |
| Edad objetivo para marketing | **23** | Tabla con la edad en filas y el recuento |

El total de horas pendientes de los tres campus es 1.932. La edad 23 es la más frecuente entre los 153 estudiantes (19 con `YEAR - YEAR`, 20 con la edad exacta).

## Problemas que aparecen y cómo se resuelven

| Problema | Causa | Solución |
|---|---|---|
| La tabla dinámica muestra el mismo campus varias veces | Espacios delante y detrás en los datos | `TRIM(CLEAN(...))` y *Refresh* |
| `Remaining workshop hours` sale 0 para todos | Las horas son texto y `texto >= 20` es siempre `TRUE` | `VALUE(TRIM(SUBSTITUTE(..., CHAR(160), "")))` |
| `Sum of Remaining workshop hours` en 0 en la tabla dinámica | Misma causa | Igual |
| `ISBLANK` no detecta los huecos en la copia | Un enlace a una celda vacía devuelve `0` | Aplicar `ISBLANK` a la hoja **original** |
| `Course` sigue siendo fórmulas | Falta *Paste Values* | Copiar y pegar valores |
| Las edades salen con decimales o desplazadas | `YEARFRAC` sin `INT`, o `YEAR - YEAR` | `INT(YEARFRAC(...))` |
| Los meses salen cambiados respecto a la tabla del enunciado | Error de la solución oficial (T1 y T3 intercambiados) | Seguir la tabla de `Instructions` |
| La tabla dinámica no refleja los cambios | No se actualiza sola | *Analyze → Refresh* o `Alt + F5` |

### Práctica
Archivos: `material_curso/modulo-03/challenge/C4-W3-Practice-Challenge.xlsx` (reto) y `C4-W3-Practice-Challenge-Solution.xlsx` (solución del curso, con el error de T1/T3 descrito en el paso 4). La nota del módulo está en [03-data-cleaning-preparation.md](03-data-cleaning-preparation.md).
