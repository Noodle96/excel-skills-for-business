# Assessment del Módulo 5 (Data Modelling) — habilidades a dominar

Este documento **no guarda respuestas, totales ni puntajes**: toma cada pregunta como una **habilidad** que hay que saber ejecutar sin ayuda, con el procedimiento para hacerla, para poder repasarla y practicarla después.

**Cómo es el assessment**: un solo libro con una hoja `Introduction`, y una hoja `Landscaping Data` con **un único modelo** que se va ampliando pregunta a pregunta: una empresa de jardinería ("Landscaping Solutions") cuyas horas de trabajo dependen del clima (cinco climas, cada uno con un factor sobre las horas base y una probabilidad). Lo notable es que **las diez preguntas encadenan las cinco herramientas de la semana sobre el mismo modelo**: cada resultado alimenta el siguiente.

## Habilidades que pedía el assessment

| # | Qué se pedía | Habilidad |
|---|---|---|
| 1 | Horas estimadas de trabajo, ponderando por la probabilidad de cada clima | `SUMPRODUCT` |
| 2 | Costo salarial estimado sin bonos (horas × tarifa) | Fórmula con referencias a celdas de supuestos |
| 3 | Costo salarial para varias tarifas por hora, y suma de la columna de resultados | Data Table de una variable |
| 4 | Costo con tasa de bono; tabla para varias tarifas **y** varias tasas de bono, y suma de la matriz | Fórmula con `(1 + tasa)` + Data Table de dos variables |
| 5-7 | Ingresos, costo y ganancia en función de una cantidad `Q` | Traducir una ecuación a fórmulas con una celda de entrada |
| 8 | Cantidad que maximiza la ganancia | Solver |
| 9 | Guardar dos escenarios de cantidad y mostrar el resumen | Scenario Manager + Scenario Summary |
| 10 | Cantidad necesaria para una ganancia dada | Goal Seek |

## Cómo se hace cada una

### Promedio ponderado con `SUMPRODUCT` (1)
```
=SUMPRODUCT(rango_de_horas_por_clima, rango_de_probabilidades)
```
- Los dos rangos deben tener **la misma forma** (una fila de cinco celdas cada uno, en el mismo orden de climas).
- Los pesos son **probabilidades que suman 1**, por eso no hace falta dividir. Se fijan con `$` para copiar la fórmula hacia abajo por cada tarea.
- Las horas de cada clima salen de multiplicar las horas con buen clima por el factor de ese clima. Después, el total se obtiene con `SUM` de las horas ponderadas de cada tarea.
- **Verificar**: las probabilidades deben sumar 1 (`=SUM(...)`); si no suman 1, el promedio ponderado queda mal.

### Fórmula con supuestos en celdas (2)
```
=celda_horas_totales * celda_tarifa_por_hora
```
- La tarifa va en **su propia celda etiquetada** (celda de entrada), no escrita dentro de la fórmula. Es la condición para que la Data Table pueda cambiarla después.
- "Sin bonos" significa que esta celda solo lleva horas × tarifa.

### Data Table de una variable (3)
1. Escribir los valores de prueba **en una columna** (24, 26... en pasos de 2).
2. Poner la **fórmula del modelo en la celda de arriba, una columna a la derecha** (referencia a la celda de resultado de la pregunta 2).
3. Seleccionar el bloque completo (valores + fórmula) → Datos > Análisis de hipótesis > Tabla de datos.
4. Como los valores están en una **columna**, la celda de la tarifa va en **Column input cell**; Row input cell se deja vacío.
5. **Comprobar**: la fila de la tabla que coincide con la tarifa actual debe dar el mismo valor que la celda de resultado.

### Fórmula con tasa de bono y Data Table de dos variables (4)
```
=celda_costo_base * (1 + celda_tasa_de_bono)
```
- Un recargo del x% se modela como `1 + tasa`. La tasa también va en su propia celda.
- La fórmula va en la **esquina superior izquierda** de la tabla.
- Las tasas de bono van en la **fila superior** y las tarifas en la **columna izquierda**. Por tanto: tasa de bono → **Row input cell**; tarifa → **Column input cell**.
- Si se invierten, **no hay error**: los números salen mal. Se comprueba con la celda que coincide con los valores actuales (misma tarifa y misma tasa), que debe ser igual a la fórmula de la esquina.
- La suma de la matriz se hace con `SUM` sobre el área de resultados, sin incluir la fila y columna de cabeceras.

### Ecuaciones como fórmulas (5-7)
- Se escribe la cantidad `Q` en **una celda de entrada** y se traduce cada ecuación, sustituyendo `Q` por la referencia a esa celda y respetando los **paréntesis** (por ejemplo `(a - b*Q)*c*Q`, donde la multiplicación entre `Q` va fuera del paréntesis).
- Ganancia = ingresos − costos.
- Estas tres celdas son la base de las tres preguntas siguientes: **la celda de la ganancia es la celda objetivo** de Solver y Goal Seek, y la de la cantidad es la celda variable.

### Solver (8)
1. Datos > Solver.
2. **Set Objective**: la celda de ganancia → **Max**.
3. **By Changing Variable Cells**: la celda de la cantidad.
4. Sin restricciones adicionales, o con la que pida el problema (por ejemplo, cantidades no negativas con *Make Unconstrained Variables Non-Negative*).
5. **Método**: la ganancia tiene un término `Q·Q` (curva), así que es **no lineal** → **GRG Nonlinear**, no Simplex LP.
6. Resolver y **Mantener la solución**.
- Si Solver no está en la pestaña Datos: Archivo > Opciones > Complementos > Ir... > marcar Solver.

### Scenario Manager (9)
1. Datos > Análisis de hipótesis > **Administrador de escenarios** > Agregar.
2. Nombre del escenario (por ejemplo "Less Gardens"); celda cambiante: la **cantidad**; valor del escenario.
3. Repetir para el segundo escenario.
4. **Resumen** > *Resumen del escenario* > celda de resultado: la **ganancia**.
5. La captura que se debe reconocer es la hoja **Scenario Summary**: una columna *Current Values* más una por escenario, con las celdas cambiantes en gris.
- Los resultados que muestra el resumen vienen del **valor de la celda de entrada en el momento de crearlo**, así que *Current Values* depende del valor que tenga la cantidad en ese instante.

### Goal Seek (10)
1. Datos > Análisis de hipótesis > **Buscar objetivo**.
2. **Set cell**: la celda de ganancia (contiene fórmula). **To value**: el valor pedido, escrito a mano. **By changing cell**: la celda de la cantidad.
3. Como la ganancia depende de `Q` en forma de **curva**, puede haber **dos cantidades** que den la misma ganancia: Goal Seek devuelve **la más cercana al valor inicial** de `Q`. Si el valor de partida cambia, puede dar la otra.
4. Si luego se quiere seguir trabajando, hay que **restaurar** el valor original de la cantidad.

## Gotchas

- **Orden de las herramientas**: Goal Seek y Solver **sobrescriben** la celda de entrada. Si después se quiere hacer el Scenario Summary con el valor original, hay que restaurarlo antes.
- **Row input cell vs Column input cell**: se elige según dónde están los **valores de prueba**, no según la ubicación de la celda de entrada. Si se invierten, salen números incorrectos sin ningún aviso.
- **La fórmula de la esquina no debe tener números fijos** donde debería haber una referencia: si la tarifa o la tasa se escribieron dentro de la fórmula, la Data Table devuelve el mismo valor en todas las celdas.
- **Las entradas de la Data Table deben estar en la misma hoja** que la tabla.
- **No se puede editar una celda dentro de una Data Table** (`{=TABLE(...)}`): hay que borrar toda la matriz o recrear la tabla.
- **Formato de las respuestas** (dos decimales, sin símbolo `$`): se aplica con formato de celda, y se escribe solo el número.
- **Excel en inglés**: los cuadros de diálogo y los nombres de los botones de este material están en inglés; los equivalentes en español están en las secciones del módulo.
- **Solver con el método equivocado**: Simplex LP en un modelo no lineal da un aviso de linealidad; Evolutionary da resultados que pueden variar de una ejecución a otra.

## Checklist: lo que debo ser capaz de hacer sin ayuda

- [ ] Calcular un promedio ponderado con `SUMPRODUCT` y comprobar que los pesos suman 1
- [ ] Dejar los supuestos (tarifa, tasa, precio) en celdas etiquetadas en vez de dentro de las fórmulas
- [ ] Armar una Data Table de una variable con los valores en columna y la fórmula en su lugar
- [ ] Armar una Data Table de dos variables y decidir qué celda va en Row input y cuál en Column input
- [ ] Verificar una Data Table comparándola con el caso actual del modelo
- [ ] Modelar un recargo porcentual con `(1 + tasa)`
- [ ] Traducir una ecuación con una cantidad a una fórmula con una celda de entrada
- [ ] Configurar Solver (objetivo, Max/Min, celdas variables, restricciones) y elegir el método correcto
- [ ] Crear escenarios con nombre en Scenario Manager y generar el Scenario Summary
- [ ] Usar Goal Seek (set cell, to value, by changing cell) y saber que puede haber más de una solución
- [ ] Explicar cuándo conviene Goal Seek y cuándo Solver
