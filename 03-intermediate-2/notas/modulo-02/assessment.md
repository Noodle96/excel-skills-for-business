# Assessment del Módulo 2 (Conditional Logic) — habilidades a dominar

Archivo: `material_curso/modulo-02/assessment/C3-W2-Final-Assessment.xlsx`. Este documento **no guarda respuestas**: toma cada pregunta como una **habilidad** que hay que saber ejecutar sin ayuda, con el procedimiento para hacerla, para poder repasarla y practicarla después.

**Contexto de la práctica**: una hoja de pedidos de una empresa. El caso: decidir a qué clientes llamar para ofrecerles el envío **Express**, y quién debe hacer la llamada (el vendedor, el subgerente o el gerente) según la prioridad del pedido, el modo de envío, la duración promedio de las llamadas y el valor del pedido. Cada pregunta construye una **columna nueva** que se apoya en las anteriores, y casi todas piden además un conteo "arriba de la columna".

## Habilidades que pedía el assessment

| # | Qué se pedía | Habilidad |
|---|---|---|
| 1 | Columna `X`: VERDADERO si la prioridad es High o Critical, **o** si el envío es Express Air; contar los VERDADERO | `OR` con varias pruebas + conteo de valores lógicos |
| 2 | Columna `Y`: VERDADERO si la prioridad es High o Critical **y** además el envío es Express Air | `AND` combinado con `OR` |
| 3 | Columna `Z`: VERDADERO si la prioridad es Medium, High o Critical | `OR` con tres pruebas |
| 4 | Columna `AA`: VERDADERO si **no** elige Express Air (pista: `NOT`) | `NOT` o el operador `<>` |
| 5 | Columna `AB`: "Call" si `Z` **y** `AA` son VERDADERO, "Do Not Call" si no; contar los "Call" | `IF` + `AND` sobre columnas que ya son lógicas |
| 6 | Columna `AC`: lista desplegable de dos opciones y fórmula que la llene solo si `AB` es "Call"; cambiar una celda a mano | Validación en lista + `IF` con `""` + sustituir una fórmula por un valor |
| 7 | Columna `AF`: promedio de duración de llamadas, "not called yet" si hay error, vacío si no se llama; promedio de la columna con 2 decimales | `IF` + `IFERROR` anidados + `AVERAGE` + formato `#.##` |
| 8 | Columna `AG`: "Star" si el total del pedido supera un valor, vacío si no; contar | `IF` simple con texto y `""` |
| 9 | Columna `AH`: clasificar en "5 Star" / "4 Star" / vacío según dos umbrales | `IF` anidado con tramos |
| 10 | Columna `AI`: llamada del subgerente si el cliente es "a llamar" **y** el pedido es "Star" | `IF` + `AND` sobre columnas de texto |
| 11 | Columna `AJ`: llamada del gerente si el promedio de llamada supera un valor, el pedido es "4 Star" o "5 Star", se marcó "Call" y **ya se llamó** | `IF` con `AND` y `OR` combinados y un "distinto de" para el texto |

## Cómo se hace cada una

### Columnas lógicas: `OR`, `AND` y `NOT` (1-5)
- **Qué devuelve cada función**: `OR` da VERDADERO si **al menos una** condición se cumple; `AND`, si se cumplen **todas**. No hace falta `IF`: la propia función ya devuelve VERDADERO/FALSO.
  ```
  =OR(prioridad="High", prioridad="Critical", envío="Express Air")
  =AND(OR(prioridad="High", prioridad="Critical"), envío="Express Air")
  =OR(prioridad="Medium", prioridad="High", prioridad="Critical")
  =NOT(envío="Express Air")        equivale a   =envío<>"Express Air"
  ```
- **Combinar `AND` con `OR`**: cuando una de las condiciones admite varias opciones, el `OR` va **dentro** del `AND`.
- **Columna que depende de otras columnas**: en la pregunta 5, `Z` y `AA` ya son VERDADERO/FALSO, así que se usan **directamente** como prueba:
  ```
  =IF(AND(Z2, AA2), "Call", "Do Not Call")
  ```
- **Contar valores lógicos arriba de la columna**: `=COUNTIF(rango, TRUE)`. `TRUE` va **sin comillas** (es un valor lógico, no un texto). Para contar texto: `=COUNTIF(rango, "Call")`.

### Lista desplegable + `IF` con `""` (6)
- Seleccionar las celdas de la columna > Datos > Validación de datos > Permitir: **Lista** > Origen con las dos opciones separadas por coma (`;` si la coma es separador decimal).
- Fórmula: `=IF(AB2="Call","interest potential","")`; copiarla hacia abajo.
- **La validación no se aplica a lo que calcula una fórmula**: la lista y la fórmula conviven en la columna. Al elegir un valor del desplegable en una celda con fórmula, la **fórmula se reemplaza** por el valor.
- Para ubicar "la enésima celda con contenido, ignorando las vacías": filtrar la columna por ese valor (`Ctrl + Shift + L`) y contar las filas visibles; contar **antes** de cambiar, porque al cambiar la celda deja de cumplir el filtro.

### `IF` con `IFERROR` dentro y promedio con formato (7)
```
=IF(AB2="Call", IFERROR(tiempo_total/num_llamadas, "not called yet"), "")
```
- El `IF` externo decide **a quién aplica**; el `IFERROR` interno decide **qué mostrar si el cálculo falla** (división entre cero cuando todavía no hay llamadas).
- Promedio de la columna: `=AVERAGE(rango)`, que **ignora el texto y las celdas vacías**. La celda del promedio debe quedar **fuera del rango** para no crear una referencia circular.
- 2 decimales: `Ctrl + 1` > Número > Personalizada > `#.##`. Es **formato visual**, no `ROUND`.

### `IF` simple y `IF` anidado (8-9)
```
=IF(total>1000, "Star", "")
=IF(total>1000, "5 Star", IF(total>500, "4 Star", ""))
```
- **El orden de las pruebas importa**: se escribe primero la más restrictiva (`>1000`); si se invirtiera, un pedido de 1500 se quedaría en "4 Star" porque ya cumple `>500`.
- Los tramos se definen por **"más de"** el umbral inferior y **"hasta"** el superior: "más de 500 y hasta 1000" es `>500` evaluado después de descartar `>1000`.
- Para contar cada categoría: `=COUNTIF(rango, "4 Star")`.

### `IF` con `AND`/`OR` sobre columnas de texto (10-11)
```
=IF(AND(AB2="Call", AG2="Star"), "Call", "")
=IF(AND(AB2="Call", AF2<>"not called yet", AF2>7, OR(AH2="4 Star", AH2="5 Star")), "Call", "")
```
- **Texto comparado con un número**: en Excel un texto se considera **mayor que cualquier número**, así que `"not called yet" > 7` da VERDADERO. Por eso, antes de comparar `AF2>7` hay que excluir el texto con `AF2<>"not called yet"`. Lo mismo ocurre con las celdas que contienen `""`: la condición `AB2="Call"` evita que esos casos pasen.
- Las pruebas del `AND` se pueden escribir en cualquier orden, pero el `OR` (dos clasificaciones aceptadas) va **anidado dentro** del `AND`.

## Gotchas

- **`TRUE` sin comillas es un valor lógico; `"TRUE"` con comillas es texto.** Contar con `COUNTIF(rango,TRUE)` solo cuenta los valores lógicos.
- **`OR`, `AND` y `NOT` ya devuelven VERDADERO/FALSO**: envolverlos en un `IF` solo hace falta cuando se quiere mostrar otra cosa (un texto como "Call").
- **`=` entre textos no distingue mayúsculas de minúsculas**; si hiciera falta, se usa `EXACT`.
- **Cada columna nueva puede apoyarse en columnas anteriores** (`Z` y `AA` → `AB` → `AC`…); si una anterior cambia, todas se recalculan.
- **`IFERROR` oculta todos los errores**, no solo el esperado; usarlo cuando se sabe qué error puede aparecer.
- **Un texto es "mayor" que cualquier número** en una comparación: cuidado al comparar columnas que mezclan números y texto (como un promedio que a veces dice "not called yet").
- **La validación no se aplica a fórmulas** y una celda con `""` **no está vacía** para Excel aunque lo parezca.

## Checklist: lo que debo ser capaz de hacer sin ayuda

- [ ] Escribir una columna VERDADERO/FALSO con `OR` y varias pruebas
- [ ] Combinar `AND` y `OR` anidando el `OR` dentro del `AND`
- [ ] Negar una condición con `NOT` o con `<>`
- [ ] Usar una columna lógica directamente como prueba de un `IF`
- [ ] Contar valores lógicos (`COUNTIF(rango,TRUE)`) y textos (`COUNTIF(rango,"Call")`) arriba de una columna
- [ ] Devolver una celda vacía con `""` cuando no se cumple una condición
- [ ] Anidar `IF` para clasificar en tramos, ordenando las pruebas de la más restrictiva a la menos
- [ ] Combinar `IF` con `IFERROR` para mostrar un mensaje en vez de un error
- [ ] Promediar una columna que mezcla números, texto y vacíos, y darle formato `#.##` sin redondear
- [ ] Crear una lista desplegable en una columna que también tiene fórmulas
- [ ] Excluir un texto antes de comparar una columna con un número
