# Módulo 4 · Summarising Data

> Los videos de esta semana, según los archivos en `material_curso/modulo-04/`, son: **V1 COUNT Functions · V2 COUNTIFS · V3 SUMIFS · V4 Sparklines · V5 Advanced Charting · V6 Trendlines**. Las secciones 2, 5, 6 y 7-9 usan material real de la lectura "Week 4: Toolbox" (esta semana especialmente completo); las secciones 1, 3 y 4 están redactadas con conocimiento general (el nombre de archivo confirma el tema, no el contenido exacto del video), así que algún detalle podría no calzar 100% con lo que muestra el curso.

## 1. Funciones COUNT (V1)

| Función | Qué cuenta | Ejemplo |
|---|---|---|
| `COUNT(rango)` | Celdas que contienen un **número** | `=COUNT(A2:A100)` |
| `COUNTA(rango)` | Celdas **no vacías** (números o texto) | `=COUNTA(A2:A100)` |
| `COUNTBLANK(rango)` | Celdas **vacías** | `=COUNTBLANK(A2:A100)` |

Son la base de lo que sigue: `COUNTIFS` es esta misma idea pero contando solo las filas que cumplen una o más condiciones.

## 2. COUNTIF y COUNTIFS (V2)

Las firmas de las dos funciones:

```
COUNTIF(range, criteria)
COUNTIFS(criteria_range1, criteria1, [criteria_range2, criteria2]…)
```

- Si solo tienes **1 criterio**, ambas funciones son idénticas. `COUNTIFS` permite además **varios criterios** a la vez (todos deben cumplirse).
- Otras funciones que siguen el mismo patrón: `SUMIF`/`SUMIFS` y `AVERAGEIF`/`AVERAGEIFS`. **El curso recomienda usar siempre las versiones `-IFS`.**
- **Rango de criterio (Criteria Range)**: el rango de datos que incluye el subconjunto de interés. Ejemplo: el rango "Account Manager" cuando el subconjunto de interés es Connor Betts.

Ejemplos (usando rangos con nombre del módulo 3):

```
=COUNTIFS(Account_Manager, "Connor Betts")
=COUNTIFS(Order_Quantity, 50)
```

Los operadores de comparación que admite el criterio (`>`, `<`, `<>`, comodines `?` y `*`) están detallados en el ninja tip de la sección 9.

## 3. SUMIFS (V3)

`SUMIFS` suma los valores de un rango **solo de las filas que cumplen uno o más criterios**:

```
SUMIFS(sum_range, criteria_range1, criteria1, [criteria_range2, criteria2]…)
```

> **Ojo con el orden de los argumentos**: en `SUMIFS` el rango a sumar (`sum_range`) va **primero**, mientras que en `COUNTIFS` no existe ese argumento. En la versión antigua `SUMIF` el orden es distinto: `SUMIF(range, criteria, [sum_range])`, con el rango a sumar al **final** y opcional. Por eso conviene usar siempre `SUMIFS`.

Ejemplo: `=SUMIFS(Sales, Region, "Norte", Year, 2015)` suma las ventas de la región Norte en 2015. `AVERAGEIFS` sigue la misma estructura que `SUMIFS` (el rango a promediar va primero).

## 4. Sparklines (V4)

Mini-gráficos que viven **dentro de una sola celda**, útiles para mostrar la tendencia de una fila de datos sin ocupar espacio como un gráfico completo.

- Se crean desde Insertar > Minigráficos, eligiendo el tipo: **Línea**, **Columna** o **Ganancia/Pérdida**.
- Se pueden resaltar el punto más alto, el más bajo, el primero, el último o los negativos, y personalizar el estilo y color desde la pestaña contextual de Minigráficos.
- Se pueden crear para varias filas a la vez (un sparkline por fila) y agrupar para que compartan formato y escala.

## 5. Gráficos avanzados (V5)

**Elementos de un gráfico**. Al intentar arrastrar un gráfico por la hoja es fácil mover por accidente un elemento interno en vez del gráfico entero. Para mover el gráfico completo hay que poner el mouse en el **Área del gráfico** (Chart Area): la parte del gráfico que no está cubierta por ningún otro elemento. Al pasar el mouse sobre el gráfico aparece un tooltip que indica sobre qué elemento estás. Elementos comunes:
- **Área del gráfico (Chart Area)**
- **Área de trazado (Plot Area)**
- **Título del gráfico (Chart Title)**
- **Leyenda (Legend)**

No siempre aparecen en el mismo lugar; se pueden mover a mano o usar la herramienta **Diseño rápido (Quick Layout)**.

**Cambiar la escala de un gráfico**. Con los **límites del eje (Axis Bounds)** se cambia la escala. Sirve para resaltar una característica del gráfico, pero también puede **engañar al lector** haciendo que un efecto parezca más importante o dramático de lo que es. No hay una regla fija: al presentar datos hay que representarlos fielmente y no solo contar la historia que uno o la audiencia quiere escuchar.

## 6. Líneas de tendencia (V6)

Al agregar una línea de tendencia a un gráfico, por defecto es **Lineal**. Las otras opciones:

| Tipo | Cuándo usarla |
|---|---|
| **Lineal** | Gráficos de línea recta; muestra una tasa de cambio constante |
| **Exponencial** | Se ajusta a muchas curvas; ideal cuando los valores suben o bajan a tasas cada vez mayores |
| **Logarítmica** | Cuando la tasa de cambio aumenta o disminuye rápido y luego se estabiliza |
| **Polinómica** | Datos que fluctúan: un solo pico es de orden 2, dos picos de orden 3, y se puede subir hasta orden 6 |
| **Potencial** | Conjuntos de datos que comparan mediciones que aumentan a una tasa específica |
| **Media móvil** | Suavizar las fluctuaciones de los datos |

## 7. Referencias relativas, absolutas y mixtas

- **Relativa** (`D4`): al copiarla una columna a la derecha cambia a `E4`; una fila hacia abajo, a `D5`. Muy útil: se escribe una fórmula y se copia hacia los lados o abajo, ajustándose sola.
- **Absoluta** (`$D$4`): con signos de dólar delante de la letra de columna y del número de fila; al copiar la fórmula la referencia **no cambia**.
- **Mixta**: solo una parte queda "bloqueada". `$D4` (columna fija) no cambia al copiarla a la derecha, pero sí al copiarla hacia abajo (`$D5`). `D$4` (fila fija) no cambia al copiarla hacia abajo, pero sí hacia los lados.

Con `F4` se recorre el ciclo: `D4` → `$D$4` → `D$4` (fila absoluta, columna relativa) → `$D4` (columna absoluta, fila relativa) → de vuelta a `D4`.

## 8. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + D` | Duplicar un gráfico |
| `F4` | Alternar entre referencias relativas, absolutas y mixtas (con una referencia seleccionada dentro de una fórmula) |
| `Ctrl + Shift + >` | Aumentar el tamaño de fuente de un elemento del gráfico (**solo Windows**; en Mac se usan las herramientas de tamaño de fuente de la pestaña Inicio) |

## 9. Ninja tips de la semana

### Operadores de comparación en los criterios

El criterio puede usar distintas comparaciones. La más simple es **igual a**, que es la que Excel usa si no especificas otra: `=COUNTIFS(Account_Manager, "Connor Betts")` o `=COUNTIFS(Order_Quantity, 50)`. Se puede escribir explícitamente (`"=50"`), pero normalmente no hace falta. Cuando se especifica un operador, **hay que usar comillas**.

| Operador | Significado | Ejemplo |
|---|---|---|
| `=` | Igual a | `=COUNTIFS(Order_Quantity, 50)` o `"=50"` |
| `<>` | Distinto de | `=COUNTIFS(Order_Quantity, "<>50")` |
| `>` | Mayor que | `=COUNTIFS(Order_Quantity, ">50")` |
| `>=` | Mayor o igual que | `=COUNTIFS(Order_Quantity, ">=50")` |
| `<` | Menor que | `=COUNTIFS(Order_Quantity, "<50")` |
| `<=` | Menor o igual que | `=COUNTIFS(Order_Quantity, "<=50")` |

- **Con fechas**: también funcionan, escribiendo la fecha en el formato local, por ejemplo `=COUNTIFS(Order_Date,"<2015-01-01")`.
- **Con texto**: igual y distinto funcionan como se espera; mayor y menor comparan las palabras **letra por letra** (`"a"<"b"`, `"apple"<"apricot"`). No distingue mayúsculas/minúsculas: `"Small Box"` y `"small box"` dan el mismo resultado.
- **Comodines** (solo con texto): `?` sustituye **una** letra y `*` sustituye **varias**. `S?ng` coincide con Sang, Sing, Song, Sung (y palabras sin sentido como Sqng), pero no con Sting. `=COUNTIFS(Product_Container, "Small *")` cuenta Small Box y Small Pack.

### Usar una referencia de celda en el título de un gráfico

El título de un gráfico puede ser un valor fijo o una fórmula, pero solo fórmulas muy simples que apunten a otra celda. Si necesitas algo más complejo, haz la fórmula en una celda y referencia esa celda desde el título.

1. Haz clic en el título del gráfico, **solo seleccionándolo, sin entrar en modo edición**. Un borde **continuo** significa que está seleccionado; si haces clic dentro para editar, el borde pasa a ser **punteado**. Con `Esc` sales del modo edición.
2. Con el título seleccionado, escribe `=`, haz clic en la celda deseada y presiona `Enter`. En el libro del video Advanced Charting, la celda del título es `A37` (Orders by Year and State).
3. **En Mac**: selecciona el título, haz clic en la barra de fórmulas, escribe `=` y luego clic en la celda destino (en Windows también funciona, pero no hace falta clic en la barra).

## 10. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Ecuación y R² en la línea de tendencia**: en el panel Dar formato a línea de tendencia se puede mostrar la ecuación del ajuste y el valor de R² (qué tan bien se ajusta la línea a los datos), además de proyectar hacia delante o atrás (opciones de pronóstico).
- **Gráficos combinados y eje secundario**: combinar, por ejemplo, columnas con una línea cuando las series tienen escalas muy distintas; a la serie de menor escala se le asigna un eje secundario. Se configura desde Cambiar tipo de gráfico > Combinado.
- **Criterios con celdas**: el criterio no tiene que estar escrito dentro de la fórmula. Se puede concatenar el operador con una celda: `=COUNTIFS(Order_Quantity, ">"&B1)` cuenta los pedidos mayores que el valor de `B1`, y cambia al instante si se edita `B1`.
- **Criterios sobre varias columnas**: combinar `COUNTIFS`/`SUMIFS` con varios pares rango-criterio responde preguntas como "¿cuánto vendió Connor Betts en 2015 en la región Norte?". Se verá un salto de potencia al comparar con tablas dinámicas en el Módulo 6.
- **`SUMPRODUCT`**: alternativa para casos con lógica OR entre criterios, donde `SUMIFS` (que exige que se cumplan *todos* los criterios) no alcanza. Se ve con más detalle en el curso Intermediate II (Data Modelling).

---

### Práctica
Archivos del curso: `material_curso/modulo-04/`. Esta semana solo los videos 5 (Advanced Charting) y 6 (Trendlines) traen versión `Soln`; el video 6 tiene su propia subcarpeta con un archivo de ejemplos adicional (`TrendLines Examples`). Hay reto de práctica con solución en `challenge/`; la carpeta `assessment/` está vacía.

### Fuente
Lectura "Week 4: Toolbox" del curso (firmas de COUNTIF/COUNTIFS, rango de criterio, referencias relativas/absolutas/mixtas, tipos de líneas de tendencia, elementos del gráfico, cambio de escala, atajos, ninja tips de operadores de comparación y título de gráfico vinculado a una celda — secciones 2, 5, 6, 7, 8 y 9). Secciones 1, 3 y 4 se basan en los nombres de los videos, redactadas con conocimiento general. Sección 10 es complemento propio.
