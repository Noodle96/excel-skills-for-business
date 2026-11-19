# Módulo 2 · Reto de práctica (Practice Challenge)

> Archivo: `material_curso/modulo-02/challenge/C4-W2-Practice-Challenge.xlsx` (con su solución `...-Soln.xlsx`). El reto se trabajó paso a paso en conversación, y este documento recoge el procedimiento y los problemas que fueron saliendo. Va con la nota principal: [02-advanced-formula-techniques.md](02-advanced-formula-techniques.md).

**Escenario**: analizar los datos de las sucursales de una empresa grande de Australia. La hoja `Accounting Table` trae ingresos y gastos por sucursal (50 sucursales); la hoja `Stock Array` trae el inventario por sucursal (49 sucursales y 5 productos).

**Habilidades**: referencias estructuradas, rangos con nombre, `SUMIFS`, fórmulas matriciales (`LARGE`/`IF`, división por un valor), `INDEX`/`MATCH`, validación de datos, `TRANSPOSE`, `SUMPRODUCT` y comprobaciones cruzadas.

> En el libro las tablas se llaman `Table15` (sucursales), `Table26` (totales) y `Table37` (resumen por estado); en el procedimiento se escriben como `tbl_accounting`, `tbl_totals` y `tbl_summary`, los nombres que se les puede dar desde *Diseño de tabla*. Las fórmulas usan `,` como separador; en un Excel con configuración regional en español sería `;`.

## Hoja `Accounting Table`

| # | Qué pide | Cómo se resuelve |
|---|---|---|
| 1 | Crear una **tabla** y un **rango con nombre** para la columna `State` | `Ctrl + T`, y el nombre `State` = `=tbl_accounting[State]` (una referencia estructurada, que crece con la tabla) |
| 2 | **Gross Margin** = Revenue − Cost of goods sold | Columna calculada: `=[@Revenue]-[@[Cost of goods sold]]` |
| 3 | **Net Income** = Gross Margin − Selling and Admin costs | `=[@[Gross Margin]]-[@[Selling and Admin costs]]` |
| 4 | Convertir `J5:L6` en tabla | `Ctrl + T` sobre `Number of Branches / Total Net Income / Average Net Income` |
| 5 | Número de sucursales | `=ROWS(tbl_accounting)` o `=COUNTA(tbl_accounting[Branch Code])` |
| 6 | **Total Net Income** | `=SUM(tbl_accounting[Net Income])` |
| 7 | **Average Net Income** por sucursal | `=[@[Total Net Income]]/[@[Number of Branches]]` (la pista pide usar los resultados anteriores; se comprueba con `AVERAGE(tbl_accounting[Net Income])`) |
| 8 | Traer los 5 títulos a la tabla `J10:O15` con **referencias estructuradas** | `=tbl_accounting[[#Headers],[Revenue]]`, etc. (ver trampa 1) |
| 9 | Completar la tabla resumen por estado con `SUMIF`, rango con nombre y referencias estructuradas | `=SUMIFS(tbl_accounting[Revenue],State,tbl_summary[@[State]:[State]])` y se rellena en las dos direcciones |
| 10 | **Lista de validación** con los estados | *Datos → Validación → Lista* sobre la lista de estados |
| 11 | Las **3 sucursales con mayor Net Income** del estado elegido, con su código | `LARGE(IF())` matricial para el valor y `INDEX`/`MATCH` para el código |
| 12 | **Proporción** del Net Income de cada estado sobre el total de la empresa | Una fórmula matricial en todo el rango (división de 5 valores por 1) |

### Detalle del paso 9: la referencia absoluta

La fórmula lleva la forma larga `tbl_summary[@[State]:[State]]` en vez de `[@State]`. Con `[@State]`, al arrastrar a la derecha el criterio pasaba a `[@Revenue]`, `[@[Cost of goods sold]]`... y los resultados salían mal. La forma de columna a columna se queda **fija en `State`** (referencia estructurada absoluta), mientras la columna a sumar (`tbl_accounting[Revenue]`) sí cambia por posición. Eso funciona porque las columnas de las dos tablas están en el **mismo orden**.

La otra forma de lograr lo mismo es una referencia normal con `$`: `$J11` en el criterio.

Comprobaciones: la suma de cada columna de la tabla resumen coincide con `SUM(tbl_accounting[...])` y la suma de `Net Income` de los 5 estados coincide con el **Total Net Income** del paso 6.

### Detalle del paso 11: los 3 mayores de un estado

En `K20:K22` (con el estado elegido en `J20`):

```
=LARGE(IF(State=$J$20,tbl_accounting[Net Income]),ROWS(K$20:K20))     ← matricial (CSE)
```

En `L20:L22`, el código de sucursal:

```
=INDEX(tbl_accounting[Branch Code],MATCH(K20,tbl_accounting[Net Income],0))
```

- `IF(State=$J$20,…)` deja pasar solo el Net Income del estado elegido; los demás quedan en `FALSE`, que `LARGE` ignora.
- `ROWS(K$20:K20)` da 1, 2 y 3 al bajar.
- Esta versión del `MATCH` solo busca el valor. Funciona si ese valor no se repite en otro estado; en este libro los 3 mayores de cada estado son únicos en toda la tabla. La versión robusta añade la condición del estado: `MATCH(1,(State=$J$20)*(tbl_accounting[Net Income]=K20),0)` (matricial).

### Detalle del paso 12: división en una sola fórmula

En `K26:K30`, seleccionados antes (los 5 estados):

```
=tbl_summary[Net Income]/tbl_totals[Total Net Income]
```

También sirve, sin depender del orden de los estados: `=SUMIFS(tbl_accounting[Net Income],State,J26:J30)/tbl_totals[Total Net Income]`. Se da formato de porcentaje y las 5 proporciones suman 100%.

## Hoja `Stock Array`

Datos: cantidades en `D12:H60` (49 sucursales × 5 productos), precios en vertical en `C3:C7`.

| # | Qué pide | Cómo se resuelve |
|---|---|---|
| 13-14 | Pasar los precios de **vertical a horizontal** con una fórmula | `{=TRANSPOSE(C3:C7)}` en `L6:P6` (1 fila × 5 columnas) |
| 16 | **Valor total del stock** con `SUMPRODUCT` en `L2` | `=SUMPRODUCT(D12:H60*L6:P6)` |
| 17 | El mismo valor con `SUM` y fórmula matricial en `M2` | `{=SUM(D12:H60*L6:P6)}` |
| 18 | **Valor por producto** con `SUM` y matriz | En `L7`: `{=SUM(tbl_stock[Product 1]*L$6)}`, rellenado hasta `P7` |
| 19 | **Valor de cada producto por sucursal** con matriz | En `L12`: `=D12:D60*L6`, rellenado hasta `P` (un derrame por columna), o una sola matriz sobre `L12:P60` |
| 20 | Total sumando el paso 18 | `=SUM(L7:P7)` |
| 21 | Total sumando el paso 19 | `=SUM(L12:P60)` |
| 22 | **Comprobación** de que son iguales | `=IF(MAX(L2:O2)=MIN(L2:O2),"OK","Error")` |

Con un formato condicional sobre la celda de la comprobación (verde para `OK`, rojo para `Error`) el estado se ve de un vistazo.

### Por qué los precios se transponen

La matriz de cantidades es de 49×5 y los precios, una fila de 5. Con `*`, Excel **extiende** la fila de precios sobre las 49 filas, y cada producto se multiplica por su precio. Esa es la razón de transponer los precios antes (paso 14) y de usar `SUMPRODUCT(matriz*fila)` en vez de `SUMPRODUCT(matriz,fila)`: con la coma las dos matrices tienen que ser del **mismo tamaño** y daría `#VALUE!`.

## Trampas y errores que salieron

1. **Los encabezados de una tabla no admiten fórmulas vivas** (paso 8). Al escribir `=tbl_accounting[[#Headers],[Revenue]]` dentro del encabezado de la tabla, Excel lo convierte en un valor fijo (y mostró `0`). Se resolvió escribiendo la fórmula en una celda libre (`=tbl_accounting[[#Headers],[Revenue]:[Net Income]]`, que se derrama en 5 celdas), copiándola y pegándola **como valores** sobre los encabezados.
2. **`[@State]` se desplaza al arrastrar** (paso 9), y `[@[State]:[State]]` no. Es el truco de la referencia estructurada absoluta del Toolbox.
3. **La fila de totales dentro del rango** (paso 16). El rango `D12:H61` incluía una fila de totales de la tabla (en la fila 61), y el resultado salió **exactamente el doble** (3,158,498 en vez de **1,579,249**). Con `D12:H60` (o con `tbl_stock[[Product 1]:[Product 5]]`, que no incluye la fila de totales) el valor es el correcto, y sirve de referencia para las comprobaciones de los pasos 17, 20, 21 y 22.
4. **Una tabla no admite matrices ni derrames**: si el destino fuera una tabla (por ejemplo la del paso 19), habría que usar una fórmula por celda.
5. **`F9` no es "vista de fórmulas"** (se anotó en la semana 1): para ver las fórmulas es `` Ctrl + ` ``.
6. **Excel 365 derrama solo**: al escribir las fórmulas matriciales con `Enter` en la primera celda, el resultado se extiende; en un Excel antiguo habría que seleccionar el rango y confirmar con `Ctrl + Shift + Enter`.

## Cómo comprobarlo

- Los totales de los pasos 16, 17, 20 y 21 deben ser **el mismo número**, y la celda del paso 22 debe mostrar `OK`.
- Las proporciones del paso 12 suman 100%.
- La suma del `Net Income` de los 5 estados es igual al **Total Net Income** de la empresa.
- Al cambiar el estado elegido en la lista de validación, los 3 valores y sus códigos cambian.
