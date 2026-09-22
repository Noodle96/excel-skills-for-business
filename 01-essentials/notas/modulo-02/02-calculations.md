# Módulo 2 · Excel Calculations

> Los videos de esta semana, según los archivos en `material_curso/modulo-02/`, son: **V01 Simple Formulas · V02 Formulas in Business · V03 Basic Functions · V04 Average Max Min · V05 Absolute Cell Refs · V06 Calculate Across Sheets**. Las secciones 1-6 siguen ese orden; el contenido está redactado con conocimiento general de Excel (los nombres de archivo confirman el tema, no el contenido exacto del video), así que algún ejemplo puntual podría no calzar 100% con lo que muestra el curso.

## 1. Fórmulas simples (V01)

Una fórmula empieza con `=` y puede combinar valores, referencias de celda y operadores aritméticos (`+ - * /`):

```
=A1+B1
=A1-B1
=A1*B1
=A1/B1
```

Al presionar `Enter`, la celda muestra el resultado; la fórmula en sí solo es visible en la barra de fórmulas (o con `Ctrl + ` `` para verla en todas las celdas a la vez).

## 2. Fórmulas aplicadas a negocios (V02)

Las mismas fórmulas simples resuelven cálculos típicos de negocio, combinando varias celdas en una sola expresión. Ejemplos:

```
=Cantidad*PrecioUnitario              (subtotal de una línea de venta)
=PrecioOriginal-(PrecioOriginal*Descuento)   (precio con descuento)
=(ValorFinal-ValorInicial)/ValorInicial       (variación porcentual)
```

La idea clave: en vez de calcular el resultado a mano, la fórmula referencia las celdas de entrada — si cambias un dato de origen (precio, cantidad, descuento), el resultado se recalcula solo.

## 3. Funciones básicas: SUM (V03)

`SUM` es la función más usada para sumar un rango: `=SUM(A1:A12)`.

**AUTOSUM (autosuma)**: botón `Σ` en la pestaña Inicio (atajo `Alt + =`). Detecta automáticamente el rango de celdas numéricas contiguas hacia arriba (o a la izquierda) de la celda activa y escribe `=SUM(...)` por ti. La flecha desplegable junto al botón también da acceso directo a AVERAGE, COUNT, MAX y MIN sin escribirlas a mano.

## 4. AVERAGE, MAX, MIN (V04)

Mismas sintaxis que SUM, sobre el mismo tipo de rango:

| Función | Qué hace | Ejemplo |
|---|---|---|
| `AVERAGE` | Calcula el promedio del rango | `=AVERAGE(A1:A12)` |
| `MAX` | Devuelve el valor más grande del rango | `=MAX(A1:A12)` |
| `MIN` | Devuelve el valor más pequeño del rango | `=MIN(A1:A12)` |

## 5. Referencias absolutas (V05)

Ya vistas en la Terminología (sección 8) — el caso de uso típico es una tasa o porcentaje fijo que se aplica a toda una columna de valores variables. Ejemplo: si `$B$1` tiene la tasa de impuesto y `A2:A10` tiene precios, la fórmula en `B2` sería `=A2*$B$1` — al copiarla hacia abajo con el fill handle, `A2` cambia a `A3`, `A4`... pero `$B$1` se mantiene fijo. Sin el `$`, la referencia a la tasa se movería junto con la fórmula y el cálculo saldría mal.

## 6. Cálculos entre hojas (V06)

Para usar en una fórmula el valor de una celda que está en **otra hoja** del mismo libro, se antepone el nombre de la hoja seguido de `!`:

```
=Ventas!B5
```

Si el nombre de la hoja tiene espacios, hay que encerrarlo entre comillas simples:

```
='Enero 2026'!B5
```

Esto permite combinar datos de varias hojas en una sola fórmula, por ejemplo: `=Ventas!B5+Gastos!B5` suma un valor de la hoja "Ventas" con uno de la hoja "Gastos".

## 7. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + Z` | Deshacer última acción |
| `F4` (Mac: `Fn+F4` / `Cmd+T`) | Recorrer los 4 tipos de referencia de celda (absoluta, mixta x2, relativa) |
| `` Ctrl + ` `` (Ctrl + tilde invertida) | Mostrar las fórmulas en la hoja en vez de sus resultados |
| `Shift + F3` (Mac: `Ctrl+A`) | Abrir el Asistente de funciones (Formula Builder) |
| `Ctrl + Re Pág` (Mac: `Cmd+Re Pág`) | Ir a la hoja anterior |
| `Ctrl + Av Pág` (Mac: `Cmd+Av Pág`) | Ir a la hoja siguiente |

## 8. Terminología

- **Fórmula**: se escribe en una celda para hacer un cálculo. Siempre empieza con `=`; al confirmar (`Enter`) muestra el resultado en esa celda. Puede ser un cálculo simple con valores, como en una calculadora: `=A1+B1` suma el valor de `A1` con el de `B1`.
- **Función**: un "mini-programa" que se usa dentro de una fórmula (también empieza con `=`) para cálculos más complejos. Opera con referencias de celda. Ejemplo: `=SUM(A1:A12)` suma todos los valores de `A1` a `A12`.
- **Barra de fórmulas**: está debajo de la cinta. Su primera línea es el **Cuadro de nombres** (referencia de la celda activa); la segunda es donde se escribe el contenido/fórmula. Al escribir `=`, aparece en el cuadro de nombres un menú desplegable con las funciones más usadas.
- **Valor**: dato numérico introducido en una celda. Si el texto no tiene formato de número se llama **etiqueta** (label). Solo los datos con formato de valor pueden usarse en fórmulas y funciones.
- **Rango**:
  - **Adyacente**: dos o más celdas contiguas, ej. `A1:C2` (el `:` significa "hasta").
  - **No adyacente**: celdas sueltas combinadas, ej. `A1:A2,C1:C2` (separadas por coma).
- **Referencia relativa**: cambia según la dirección en la que se copia. Ejemplo: si `C2` tiene `=A2*B2` y se arrastra con el fill handle hasta `C3` y `C4`, Excel ajusta automáticamente a `=A3*B3` y `=A4*B4`.
- **Referencia absoluta** ("el signo de dólar"): no cambia al copiarla. Se fija con `$` antes de cada elemento: `$A$1`. Atajo para convertir la referencia: `F4`.

## 9. Ninja tip: orden de las operaciones matemáticas

Excel sigue las reglas matemáticas estándar: **multiplicación (`*`) y división (`/`) antes que suma (`+`) y resta (`-`)**, sin importar el orden en que aparecen escritas de izquierda a derecha.

Ejemplo: `=3+4*5`
- Excel **no** calcula `(3+4)*5 = 35`.
- Excel calcula `3 + (4*5) = 23`, porque la multiplicación tiene prioridad.
- Para forzar que la suma se calcule primero, hay que usar paréntesis explícitos: `=(3+4)*5`.

## 10. Tip extra: RANDBETWEEN — "el sombrero digital"

La función `RANDBETWEEN` elige al azar un número dentro de un rango que tú defines. Ejemplo de uso real: asignar aleatoriamente quién lava los platos o quién tiene "cake-duty" en el equipo, en vez de usar un sombrero con papelitos.

- Necesita **2 argumentos**: el número más bajo y el más alto del rango (a diferencia de la mayoría de funciones vistas esta semana, que solo llevan 1 argumento).
- Sintaxis: `=RANDBETWEEN(B4,B17)` — asigna a cada persona un número dentro de ese rango y Excel elige uno al azar al presionar `Enter`.
- **Ojo con la región**: en países donde la coma es el separador decimal, los argumentos de las funciones se separan con `;` en vez de `,`. Ahí la misma fórmula se escribe `=RANDBETWEEN(B4;B17)`. No es un error, es solo una diferencia regional de Excel.

## 11. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Referencias mixtas**: el tercer y cuarto "click" del atajo `F4`. Fijan solo la fila o solo la columna: `A$1` (fila fija, columna libre) o `$A1` (columna fija, fila libre). Útiles en tablas donde una fórmula se copia tanto hacia abajo como hacia los lados (ej. tablas de multiplicar, comisiones por rango).
- **COUNT y COUNTA**: vecinas naturales de SUM/AVERAGE/MIN/MAX. `COUNT(rango)` cuenta cuántas celdas del rango tienen un **número**; `COUNTA(rango)` cuenta cuántas celdas **no están vacías** (números o texto). Se profundiza con criterios (`COUNTIFS`) más adelante, en el curso de Intermediate I.
- **Errores comunes de fórmulas**: útil reconocerlos desde ya.

  | Error | Causa típica |
  |---|---|
  | `#DIV/0!` | División entre una celda vacía o con 0 |
  | `#VALUE!` | Se usó texto donde se esperaba un número |
  | `#REF!` | La fórmula apunta a una celda que fue borrada |
  | `#NAME?` | Error de escritura en el nombre de una función |

- **Rellenar fórmulas con el Fill Handle**: no solo sirve para series (visto en el Módulo 1) — arrastrarlo también copia una fórmula hacia abajo/derecha, ajustando las referencias relativas fila por fila. Es la forma más rápida de aplicar el mismo cálculo a una columna entera.

---

### Práctica
Archivos del curso: `material_curso/modulo-02/` (resolver los que no terminan en `Soln`).

### Fuente
Lecturas "Week 2: Toolbox" y "Week 2: Excellent Tips and Resources" del curso (secciones 7, 8, 9, 10). Secciones 1-6 se basan en los títulos de los videos confirmados por los archivos en `material_curso/modulo-02/`, redactadas con conocimiento general de Excel. Sección 11 es complemento propio.
