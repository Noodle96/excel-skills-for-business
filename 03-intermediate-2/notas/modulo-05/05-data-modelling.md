# Módulo 5 · Data Modelling

> Los videos de esta semana, según los archivos en `material_curso/modulo-05/`, son: **V1 SUMPRODUCT · V2 Data Tables · V3 Goal Seek · V4 Scenario Manager · V5 Solver**. Las secciones 7-8 son material real de la lectura "Week 5: Toolbox". El resto se redactó con conocimiento general apoyado en los nombres de los videos y en el contenido de los libros del curso (que sí se revisaron), así que algún detalle del procedimiento podría no calzar al 100% con lo que muestra el video.

**Idea central del módulo**: construir un **modelo** (una hoja donde unas celdas de entrada alimentan fórmulas que dan un resultado) y luego **hacerle preguntas "¿y si...?"** con las herramientas de **What-If Analysis** (Datos > Análisis de hipótesis). Cada herramienta responde una pregunta distinta:

| Herramienta | Pregunta que responde | Celdas que cambia |
|---|---|---|
| **Data Table** | "¿Qué resultado da mi modelo para *toda esta lista* de valores de entrada?" | 1 o 2 entradas, con muchos valores |
| **Scenario Manager** | "¿Qué pasa en el escenario optimista, en el base y en el pesimista?" | Varias entradas a la vez, guardadas con nombre |
| **Goal Seek** | "¿Qué valor de *esta* entrada necesito para que el resultado sea exactamente *X*?" | **1** celda |
| **Solver** | "¿Qué valores de *estas* entradas maximizan/minimizan el resultado, respetando restricciones?" | **Varias** celdas, con restricciones |

El caso del curso es un proyecto de **manuales de software** (hoja `Project Costing Model`): número de manuales, horas por manual, personal, horas por día y días por semana determinan la duración del proyecto, el costo salarial (con **horas extra** vía `IF`), los ingresos y la **ganancia proyectada**. Todas las herramientas de la semana se aplican a ese modelo.

## 1. `SUMPRODUCT` (V1)

**`SUMPRODUCT`** multiplica dos o más rangos celda a celda (el primero con el primero, el segundo con el segundo...) y **suma** los productos. Es el cálculo de un **promedio ponderado** en una sola celda.

```
=SUMPRODUCT(array1, [array2], …)
```

**Ejemplo del curso (estimación PERT)**: cada tarea tiene tres estimaciones de horas (más pesimista, más probable, más optimista) y unos pesos `1 · 4 · 1`:

```
=SUMPRODUCT(B6:D6, B$4:D$4) / SUM($B$4:$D$4)
```

- Se multiplica cada estimación por su peso y se suma. Dividir entre la suma de los pesos (`6`) lo convierte en promedio ponderado.
- Los pesos van con la **fila fija** (`B$4:D$4`, referencia mixta o absoluta) para copiar la fórmula hacia abajo por todas las tareas.
- **Los rangos deben tener la misma forma** (misma cantidad de filas y columnas). Si no, da `#VALUE!`.

**Ejemplo del assessment (probabilidades)**: horas de trabajo según el clima, ponderadas por la probabilidad de cada clima. Es el mismo patrón: los pesos son probabilidades que suman 1, así que no hace falta dividir.

**`SUMPRODUCT` como contador condicional**: con una comparación dentro, cuenta o suma filas que cumplen una condición.

```
=SUMPRODUCT(1*(E6:E11>=100))     → cuántas tareas tienen 100 horas o más
=COUNTIFS(E6:E11,">=100")         → el mismo resultado
```

- `E6:E11>=100` da una lista de `VERDADERO`/`FALSO`; multiplicarla por `1` los vuelve `1`/`0` y `SUMPRODUCT` los suma. (Otra forma habitual: `--(E6:E11>=100)`.)
- Para casos sencillos, `COUNTIFS`/`SUMIFS` son más fáciles de leer. `SUMPRODUCT` gana cuando la condición es más compleja (condiciones con `OR`, cálculos entre columnas).

## 2. Data Tables (V2)

Una **Data Table** (Datos > Análisis de hipótesis > **Tabla de datos**) calcula **de una vez** el resultado de una fórmula para muchos valores de entrada. Excel escribe en cada celda una matriz `{=TABLE(...)}` que **no se puede editar celda a celda**.

### Tabla de una variable

Se ponen los valores a probar **en una columna** (o una fila) y una **fórmula** que dependa de la celda de entrada.

```
        A                B
17   (vacía)        =E18*(1+E19)     ← fórmula del modelo (esquina)
18   5,0%             ← aquí irá el resultado
19   6,5%
20   8,0%
...
```

- Con los valores **en columna**: la fórmula va **una fila arriba y una columna a la derecha** de la primera celda de valores.
- Con los valores **en fila**: la fórmula va **una columna a la izquierda y una fila debajo**.
- Se selecciona todo el bloque (valores + fórmula) y se abre la herramienta.

### Tabla de dos variables

Una variable recorre la **fila superior** y la otra la **columna izquierda**; la **esquina superior izquierda** contiene la fórmula.

```
        A                 B        C        D      …
29   =E24*E25*(1+E26)    30,00    32,50    35,00     ← precios (fila superior)
30        3%               ·        ·        ·
31        4%               ·        ·        ·
…
```

### Row input cell vs Column input cell

Este es el punto que más se confunde:

- La celda que se indica en el cuadro de diálogo es **la celda de entrada real del modelo** (la que usa la fórmula), **no** una celda de la tabla ni de las cabeceras.
- **"Fila" y "columna" se refieren a dónde están escritos los valores a probar**, no a dónde está la celda de entrada:

| Dónde están los valores de prueba | Dónde se pone la celda de entrada del modelo |
|---|---|
| **Escritos en la fila superior** (de lado, `B29:H29`) | **Row input cell** |
| **Escritos en la columna izquierda** (hacia abajo, `A30:A37`) | **Column input cell** |

Ejemplo del curso: precios de venta en la fila superior y aumento esperado en la columna izquierda → `Row input cell = Sale Price` y `Column input cell = Expected Increase`. En la hoja queda como `{=TABLE(E24;E26)}` (primero la celda de fila, luego la de columna).
En una tabla de una sola variable en columna, solo aparece una: `{=TABLE(;E19)}` (el espacio vacío antes de `;` es la celda de fila, que no se usó).

> **Si se invierten, Excel no da error**: simplemente sustituye los valores en la celda equivocada y los números salen mal en silencio. La forma de comprobarlo es tomar un caso conocido: la celda de la tabla que coincide con los valores actuales del modelo debe dar el **mismo resultado** que la fórmula de la esquina.

### Cómo verificar una Data Table

El curso incluye un truco útil: junto a la tabla, la misma cuenta **a mano con una fórmula arrastrada** (con referencias mixtas) y una columna de diferencia (`f-DT`) con valores `0,00`. Si todas las diferencias son cero, la tabla está bien armada.

### ¿Data Table o arrastrar una fórmula?

- Si el resultado se calcula con **una sola fórmula corta** que depende de las dos entradas, **arrastrar con referencias mixtas** (`$A30*B$29`) es más simple.
- La Data Table gana cuando el resultado depende de una **cadena larga de celdas** (varias hojas, `IF`, `VLOOKUP`...), porque basta con apuntar a las celdas de entrada y Excel recalcula todo el modelo.
- La entrada debe estar **en la misma hoja** que la Data Table, y la fórmula de la esquina no puede tener números fijos "escondidos" que deberían ser la entrada.

### Notas de cálculo
Como una Data Table recalcula el modelo entero por cada celda, en libros grandes se puede poner el cálculo en **Automático excepto para tablas de datos** (Módulo 4) y refrescarlas con `F9`.

## 3. Goal Seek (V3)

**Goal Seek** (Datos > Análisis de hipótesis > **Buscar objetivo**) responde: *"¿qué valor debe tener esta celda de entrada para que el resultado llegue a un valor que yo fijo?"*

Tiene tres campos:

| Campo | Qué es | Ejemplo del curso |
|---|---|---|
| **Set cell** (Definir la celda) | La celda con la **fórmula** del resultado (la **Target Cell**) | `E20`, la ganancia proyectada |
| **To value** (Con el valor) | El valor **fijo** que queremos obtener (se escribe a mano) | `0` (punto de equilibrio) |
| **By changing cell** (Cambiando la celda) | La celda **de entrada** que Excel modificará (la **Variable Cell**) | `B6`, las horas por manual |

Cómo se usa bien:
- **"Set cell" debe contener una fórmula** y depender, directa o indirectamente, de la celda que se cambia. La celda cambiante debe contener un **valor**, no una fórmula.
- Goal Seek **reemplaza** el valor de la celda cambiante por el resultado. Si no se quiere conservar, se cancela, o se hace `Ctrl + Z`.
- **Cambia una sola celda** y busca un valor **exacto** (no maximiza ni minimiza). Si la pregunta tiene varias variables o restricciones, es trabajo para Solver.
- Es **iterativo y aproximado**: prueba valores hasta que el resultado queda dentro de una tolerancia, así que a veces el valor "exacto" sale con muchos decimales (por ejemplo `1121,1228...`).
- Si la relación entre la celda cambiante y el resultado es **una curva** (no una recta), puede haber **más de una solución** (por ejemplo, un resultado que sube y luego baja). Goal Seek encuentra **la más cercana al valor de partida**; para ver la otra, se empieza desde otro valor.
- Si el valor que se pide no existe, Goal Seek avisa que "no encontró solución" (por ejemplo, si el resultado nunca llega a ese valor).

## 4. Scenario Manager (V4)

**Scenario Manager** (Datos > Análisis de hipótesis > **Administrador de escenarios**) guarda **conjuntos con nombre de valores** para varias celdas de entrada, para pasar entre ellos y compararlos.

Pasos:
1. **Agregar** un escenario: nombre (por ejemplo "Price decrease") y las **celdas cambiantes**.
2. Escribir los valores de cada celda en ese escenario.
3. Repetir para cada escenario (por ejemplo precio actual, precio más bajo, precio más alto).
4. **Mostrar** aplica el escenario elegido a la hoja; **Resumen** crea una hoja nueva con todos en una tabla.

En el ejemplo del curso se varían **dos entradas a la vez** que están relacionadas: `Sale_Price` y `Quantity_Sold`. Un precio más bajo viene con más unidades vendidas, y uno más alto con menos. Eso es precisamente lo que una Data Table de una variable no puede hacer (mueve una sola entrada).

**Resumen de escenarios** (Scenario Summary):
- Pide las **celdas de resultado** (por ejemplo la ganancia `E20`) y crea una hoja con una columna por escenario, más la columna *Current Values*.
- **Las celdas cambiantes aparecen resaltadas en gris** para cada escenario.
- Es una **hoja estática**: no se actualiza sola si cambia el modelo. Hay que volver a crear el resumen.
- Los **rangos con nombre** (Módulo 1 de Intermediate I) hacen el resumen mucho más legible: en el curso aparecen `Sale_Price` y `Quantity_Sold__Per_Manual` en vez de `$E$15`. Es un buen motivo para nombrar las celdas del modelo antes de crear escenarios.

**Los tres escenarios clásicos**: **Base Case** (valores esperados), **Best Case** (los más favorables) y **Worst Case** (los menos favorables). Scenario Manager permite crear cuantos se quieran.

**Limitación**: cada escenario admite hasta 32 celdas cambiantes.

## 5. Solver (V5)

**Solver** es un **complemento** (*add-in*) que resuelve problemas de **optimización**: encuentra los valores de varias celdas que **maximizan, minimizan o llevan a un valor** una celda objetivo, respetando **restricciones**. Si la opción no aparece en Datos > Análisis, hay que activarla: **Archivo > Opciones > Complementos > Administrar: Complementos de Excel > Ir... > Solver**.

### Los elementos de un problema en Solver

| Concepto de Solver | Qué es | Vocabulario de optimización lineal |
|---|---|---|
| **Set Objective** | La celda objetivo (**Target Cell**): una fórmula | Función objetivo |
| **To: Max / Min / Value Of** | Qué se quiere hacer con la celda objetivo | Maximizar / minimizar |
| **By Changing Variable Cells** | Las celdas de entrada que Solver ajusta (**Variable Cells**) | Variables de decisión |
| **Subject to the Constraints** | Los límites (**Constraints**): `<=`, `=`, `>=`, entero (`int`), binario (`bin`), todos distintos (`dif`) | Restricciones |
| **Make Unconstrained Variables Non-Negative** | Obliga a las variables sin restricción a ser `>= 0` | Condición `x ≥ 0` |
| **Select a Solving Method** | El motor de resolución (sección 6) | Algoritmo (Simplex, etc.) |

Pasos: **Datos > Solver** → definir la celda objetivo y si se maximiza o minimiza → indicar las celdas cambiantes → **Añadir** restricciones → elegir el método → **Resolver**. Al terminar, Solver pregunta si **Mantener la solución** o **Restaurar los valores originales**, y ofrece informes (Respuestas, Sensibilidad, Límites).

### Ejemplo del curso: modelo de manuales de software
- **Objetivo**: la ganancia proyectada (`E20`), o bien llevar la duración a un valor concreto.
- **Variables**: las celdas de personal, horas por día y días por semana (`B7`, `B8`, `B9`).
- **Restricciones**: límites realistas (por ejemplo, un máximo de horas por día o de días por semana, y valores enteros para el personal).
- Solver puede **guardar la solución como escenario** (botón *Save Scenario*), y por eso aparecen en el libro del curso resúmenes con escenarios como "20 weeks", "30 weeks (original)" o "21 weeks": cada uno corresponde a una duración objetivo del proyecto.

### Ejemplo del assessment (una sola variable, función curva)
Con ingresos y costos como función de la cantidad `Q` de jardines atendidos (ingresos `(240 − 30Q)·1000Q` y costos `(120 + 8Q)·1000`), la ganancia es una **parábola**: sube y luego baja porque el precio cae al aumentar la cantidad. Maximizar esa ganancia con Solver es un caso de modelo **no lineal** (hay un `Q` multiplicado por otro `Q`).

### Ejemplo genérico de optimización lineal (producción)
Una fábrica produce dos productos, con una ganancia por unidad y un límite de horas de máquina y de materia prima.

| Elemento | En Solver |
|---|---|
| Variables de decisión `x1`, `x2` (unidades de cada producto) | Celdas cambiantes (`B2:C2`) |
| Función objetivo `ganancia = c1·x1 + c2·x2` | Celda objetivo `=SUMPRODUCT(B2:C2, B3:C3)`, **Max** |
| Restricciones de recursos `a·x1 + b·x2 <= disponible` | Una restricción por recurso: `celda_uso <= celda_disponible` |
| `x1, x2 ≥ 0` | Casilla *Make Unconstrained Variables Non-Negative* |
| Método | **Simplex LP** (el modelo es lineal) |

`SUMPRODUCT` es la herramienta natural para esto: los coeficientes de la función objetivo y de cada restricción son sumas de productos entre las variables y sus coeficientes.

## 6. Los tres motores de Solver

| | **Simplex LP** | **GRG Nonlinear** | **Evolutionary** |
|---|---|---|---|
| **Para qué modelos** | **Lineales**: objetivo y restricciones son sumas de variables por constantes | **No lineales suaves**: continuos y derivables (productos entre variables, potencias, logaritmos) | **No suaves**: con saltos o cortes (`IF`, `VLOOKUP`, `ABS`, `MAX`, `CHOOSE`) |
| **Cómo funciona** | Método Simplex: recorre los vértices de la región factible | Método del gradiente: sigue la pendiente hacia el óptimo | Algoritmo genético: genera una población de soluciones, las cruza y muta |
| **¿Garantiza el óptimo?** | **Sí, el global** | **Solo un óptimo local** (depende del punto de partida) | **No** (heurística) |
| **Velocidad** | La más rápida | Media | La más lenta |
| **¿Siempre da lo mismo?** | Sí | Sí, desde el mismo punto de partida | **No**: tiene azar |
| **Informes** | Respuestas, sensibilidad, límites | También sensibilidad | Respuestas, sin sensibilidad |

**Cómo elegir**:
1. ¿El modelo es lineal? → **Simplex LP**.
2. ¿Es no lineal pero suave (por ejemplo, ingresos = precio × cantidad con ambos como variables)? → **GRG Nonlinear**.
3. ¿Usa `IF`, búsquedas, valores absolutos o funciones con saltos? → **Evolutionary**.

**Detalles útiles**:
- Con **Simplex LP** y un modelo que no es lineal, Solver avisa que no se cumplen las condiciones de linealidad.
- **GRG Nonlinear** tiene la opción **Multistart**, que lo ejecuta desde varios puntos de partida para reducir el riesgo de quedar atrapado en un óptimo local.
- **Evolutionary** necesita **cotas** (límite inferior y superior) para las variables, y cambiar la semilla aleatoria o el tiempo de ejecución puede cambiar el resultado.
- Simplex no es una promesa de velocidad en el peor caso (en teoría puede ser exponencial), pero en la práctica es muy rápido, y para problemas lineales con enteros conviene Simplex LP.

### Goal Seek vs Solver

| | **Goal Seek** | **Solver** |
|---|---|---|
| Celdas que cambia | **1** | Varias |
| Qué busca | Un valor **exacto** del resultado | **Maximizar**, **minimizar** o un valor |
| Restricciones | No admite | Sí |
| Método | Iterativo por tolerancia | Tres motores |
| Cuándo usarlo | Una pregunta rápida con una incógnita | Optimización con varias variables o límites |

## 7. Terminología (lectura "Week 5: Toolbox")

- **SUMPRODUCT**: función de Excel que multiplica rangos o matrices entre sí y después suma los productos.
- **Variable Cell (celda variable)**: celda con un valor que se permite cambiar al inicio del análisis.
- **Target Cell (celda objetivo)**: celda que se fija a un valor concreto (o se maximiza/minimiza) cambiando las celdas variables.
- **What-If Analysis (análisis de hipótesis)**: grupo de herramientas de análisis de datos que permite experimentar con distintos valores en las fórmulas e investigar los resultados.
- **Constraint (restricción)**: límite al valor de una celda concreta.
- **Add-in (complemento)**: función que se puede agregar a Excel para complementar las existentes. Solver es un complemento.
- **Data Table (tabla de datos)**: herramienta de What-If Analysis que permite probar rápidamente distintos valores para una fórmula dada.
- **Scenario (escenario)**: conjunto concreto de valores dados a las celdas variables. Cada escenario se puede nombrar y comparar con Scenario Manager.
- **Base Case Scenario (escenario base)**: aquel en que todas las celdas variables tienen los valores esperados al inicio del análisis.
- **Best (Worst) Case Scenario (mejor o peor caso)**: aquel en que las celdas variables tienen los valores más (menos) favorables desde nuestra perspectiva.

## 8. Ninja tips de la semana

- **Goal Seek y Solver** son herramientas de análisis similares. Goal Seek es más simple y sirve para cálculos rápidos; Solver es más potente y permite cálculos más complejos.
- **Scenario Manager** da libertad para elegir cuántos escenarios crear, pero una técnica habitual del análisis de escenarios es tener un caso **mejor (optimista)**, uno **peor (pesimista)** y uno **base (probable)**.

## 9. Reto de práctica de la semana

El reto (`challenge/`) reúne casi todas las herramientas en un problema distinto: el diseño de una **lata cilíndrica** de volumen fijo.

- El modelo calcula **área superficial** y **volumen** a partir del **radio** y la **altura** (fórmulas con `PI()`).
- Una **Data Table de dos variables** muestra el volumen para combinaciones de radio y altura.
- **Solver** busca las dimensiones de la lata para un volumen dado, y también las del **cajón cúbico** donde caben las latas (con `FLOOR.MATH` para la versión "práctica", sin fracciones de lata).
- El precio del metal tiene cinco niveles con una **probabilidad** cada uno, y se obtiene un precio estimado con `SUMPRODUCT`; el costo del metal es el área de la lata por el precio, por la cantidad de latas.
- **Scenario Manager** compara un precio de metal alto y uno bajo, con la celda de resultado `Total_Metal_cost`, y se usan rangos con nombre (`Metal_price`, `Metal_Cost_per_can`).

> Nota: esta descripción se dedujo de las fórmulas del libro de solución, no de un enunciado pegado. Si luego se trabaja el reto paso a paso, se puede documentar aparte (como `advanced-practice-challenge.md` en Intermediate I).

## 10. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Qué significa el "bonus rate" y los porcentajes en un modelo**: un recargo del 10% se modela como `costo × (1 + tasa)`; un descuento, como `costo × (1 − tasa)`. Es lo que se hace con la tasa de bono en el assessment.
- **Poner los supuestos en celdas con etiqueta** (tarifa por hora, tasa de bono, precio) en vez de escribirlos dentro de las fórmulas. Es lo que permite que Data Table, Goal Seek, Solver y Scenario Manager puedan cambiarlos. Con **nombres de rango** se leen mejor.
- **Análisis de sensibilidad**: ver cómo cambia un resultado cuando varía una entrada. Una Data Table de una variable es la forma más rápida; el informe de sensibilidad de Solver da lo mismo para modelos lineales.
- **Punto de equilibrio (break-even)**: la cantidad o el precio con el que la ganancia es 0. Es el uso clásico de Goal Seek.
- **Optimización lineal en Excel**: modelar con `SUMPRODUCT` (coeficientes × variables) deja las restricciones y el objetivo con la forma exacta del método Simplex. Para problemas de asignación, transporte o mezcla es la forma estándar de plantearlos.
- **Restricciones de enteros y binarias**: `int` fuerza valores enteros (personas, unidades), `bin` fuerza 0 o 1 (decisiones sí/no). Convierten el problema en uno más difícil, y Solver tiene una tolerancia de enteros (*Integer Optimality*) que se puede ajustar en Opciones.
- **Óptimo local vs global**: en modelos no lineales, GRG puede quedarse en un máximo local; probar con otros valores iniciales o activar Multistart ayuda a comprobarlo.
- **Soluciones múltiples en Goal Seek**: si la curva cruza el valor buscado más de una vez, el resultado depende del valor inicial de la celda cambiante.
- **Otras herramientas de pronóstico** (se ven más a fondo en cursos posteriores): hoja de previsión (*Forecast Sheet*), `FORECAST`/`TREND` y líneas de tendencia en gráficos.
- **Simulación (Monte Carlo)**: con `RAND`/`RANDBETWEEN` y una Data Table de una columna vacía como "repetidor", se pueden simular muchos resultados al azar para estimar riesgos.
- **Un mismo modelo, varias herramientas**: lo habitual en la práctica es combinarlas: Goal Seek para un dato puntual, Data Table para ver el rango de resultados y Scenario Manager para presentar los casos a otras personas.

---

### Práctica
Archivos del curso: `material_curso/modulo-05/`, uno por video (`W5_V1 SUMPRODUCT.xlsx`, `W5_V2 DataTables.xlsx`, `W5_V3 GoalSeek.xlsx`, `W5_V4 ScenarioManager.xlsx`, `W5_V5 Solver.xlsx`). Ningún video trae versión `Soln`: en cada libro la hoja `Project Costing Model` es el modelo que se trabaja y la hoja `Projections & Estimates` trae las estimaciones y las tablas. Hay un reto de práctica con solución en `challenge/` (sección 9) y el assessment de la semana en `assessment/`, documentado por habilidades en [assessment.md](assessment.md).

### Fuente
Lectura "Week 5: Toolbox" del curso (terminología y ninja tips — secciones 7 y 8). Las secciones 1-6 y 9 se apoyan en los nombres de los videos y en el contenido de los libros del curso, con conocimiento general para los procedimientos. La comparación de los motores de Solver (sección 6) viene de una pregunta resuelta en conversación. Sección 10 es complemento propio.
