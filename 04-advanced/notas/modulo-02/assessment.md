# Módulo 2 · Assessment (Final Assignment de la semana)

> Archivo del curso: `material_curso/modulo-02/assessment/`. Aquí se documentan **las habilidades que evalúa**, no las respuestas ni los resultados. Para la teoría, ver [02-advanced-formula-techniques.md](02-advanced-formula-techniques.md); para el reto de la semana, [practice-challenge.md](practice-challenge.md).

**Escenario**: análisis de datos meteorológicos básicos: temperaturas altas y bajas anuales promedio de **51 ciudades de EE. UU.**, en grados Celsius y Fahrenheit. La hoja `Raw_Data` trae las ciudades en orden aleatorio, la hoja `Table_Data` trae un subconjunto (ciudades de la A a la D) como una **tabla** llamada `City_Subset`, y la hoja `Distances` trae una tabla de distancias entre 5 pueblos ficticios. Son **11 preguntas** y cada una pide enviar el valor de una celda de comprobación (*check sum*).

**Avisos del enunciado**: no insertar ni borrar filas o columnas ni mover celdas (cambiaría las comprobaciones), y guardar el libro con frecuencia.

## Habilidades por pregunta

### Parte 1 · Referencias estructuradas (hoja `Table_Data`)

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 1 | **Contar** las ciudades de la tabla con una referencia estructurada (sin espacios, con el nombre en inglés de la función) | `COUNTA` sobre una columna de la tabla |
| 2 | **Estadísticas** de columnas de la tabla y un producto de celdas | `AVERAGE`, `MIN` y `PRODUCT` (o `*`) con referencias estructuradas |
| 3 | **Convertir de Fahrenheit a Celsius** con una referencia estructurada que se pueda arrastrar en las dos direcciones | `[@Columna]` y fórmula `(F − 32) × 5/9` |

### Parte 2 · Ordenar con funciones (hoja `Raw_Data`)

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 4 | **Posición alfabética** de cada ciudad (comprobación con una ciudad de referencia) | `COUNTIFS` con criterio `"<="&celda` |
| 5 | **Localizar** el valor del índice auxiliar entre las posiciones calculadas | `MATCH` con coincidencia exacta |
| 6 | **Lista ordenada** de ciudades | `INDEX` con la columna original y el resultado del `MATCH` |

### Parte 3 · Fórmulas matriciales

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 7 | **Fórmula matricial de varias celdas**: los 5 valores más altos de una columna | `LARGE` con un rango de `k` (1 a 5), selección previa de las 5 celdas y `Ctrl + Shift + Enter`; después, una suma de los 5 |
| 8 | **Fórmula matricial de una celda**: sumar el producto de dos columnas solo donde una condición se cumple (algo que un `SUMIFS` no resuelve) | `SUM` + `IF` con producto de columnas, `Ctrl + Shift + Enter` |

### Parte 4 · `TRANSPOSE` y tabla cruzada (hoja `Distances`)

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 9 | **Títulos de filas y de columnas** de una tabla cruzada a partir de un rango con nombre | Matriz de una columna con el rango con nombre, y `TRANSPOSE` para los títulos horizontales |
| 10 | **Llenar la cuadrícula** buscando la distancia de cada par de pueblos | `SUMIFS` con dos criterios (fila y columna), con referencias mixtas, rellenada en dos direcciones |
| 11 | **Fórmula matricial de varias celdas** que deja solo una mitad de la cuadrícula y pone 0 en la otra | `IF` con contadores de fila y de columna (los números 1 a 5) comparados con `<=`, y `Ctrl + Shift + Enter` |

## Cosas a tener en cuenta (trampas)

- **Seleccionar primero** el rango de salida en las fórmulas matriciales de varias celdas, con el tamaño exacto (no más, que sobran `#N/A`, ni menos).
- **Confirmar con `Ctrl + Shift + Enter`**. En Excel 365 un `Enter` también derrama el resultado, y el valor final es el mismo.
- **`COUNTA` y no `COUNT`** para contar texto.
- **Referencias estructuradas con `@`** en la conversión de temperaturas, y comprobar que al arrastrar a la derecha la columna cambia de la alta a la baja (el orden de las columnas importa).
- **No usar un rango con filas de más**: si la tabla tiene una fila de totales dentro del rango, el resultado se contaría doble.
- **`COUNTIFS` con `"<="&celda`** da una posición que incluye a la propia ciudad: la primera es 1.
- **`SUMIFS` en dos direcciones** necesita `$` bien puestos: columna fija en el título de la fila y fila fija en el título de la columna.
- **La diagonal da 0** en la tabla cruzada, y es lo esperado: la distancia de un pueblo a sí mismo es 0.
- **No tocar el diseño** del libro: las comprobaciones dependen de que no se muevan celdas.

## Checklist del assessment

- [ ] Contar, promediar y buscar el mínimo con referencias estructuradas
- [ ] Convertir temperaturas con `[@Columna]` arrastrando en dos direcciones
- [ ] Posición alfabética con `COUNTIFS` y `"<="&`
- [ ] Localizar con `MATCH` y traer el nombre con `INDEX`
- [ ] Fórmula matricial de varias celdas con `LARGE`
- [ ] Fórmula matricial de una celda con `SUM(IF())` y producto de columnas
- [ ] Títulos con una matriz de un rango con nombre y con `TRANSPOSE`
- [ ] `SUMIFS` con dos criterios y referencias mixtas
- [ ] Máscara triangular con `IF` y contadores de fila y columna
