# Assessment del Módulo 3 (Automating Lookups) — habilidades a dominar

Este documento **no guarda respuestas, códigos ni totales**: toma cada pregunta como una **habilidad** que hay que saber ejecutar sin ayuda, con el procedimiento para hacerla, para poder repasarla y practicarla después.

**Cómo es el assessment**: un solo libro con una hoja de instrucciones y una hoja `Data` que contiene **cuatro tareas**, cada una con sus datos agrupados (se despliegan con el icono `+` junto a "Task 1", "Task 2"...). En cada tarea hay que rellenar unas **celdas verdes** usando una función concreta; una **celda amarilla** genera un código de comprobación que valida que las funciones se usaron correctamente. Las preguntas piden después datos de esas celdas.

## Habilidades que pedía el assessment

| # | Qué se pedía | Habilidad |
|---|---|---|
| 1 | Cuántas hojas tiene el libro | Conocer la estructura de un libro |
| 2 | Tarea 1: describir artículos codificados rellenando las celdas verdes | `CHOOSE` |
| 3-4 | Leer el contenido de celdas concretas de la hoja de instrucciones | Revisar el resultado de una fórmula en una celda dada |
| 5-6 | Tarea 2: calcular las tarifas de llamadas de larga distancia según una escala por tramos y el total | `VLOOKUP` por rango |
| 7-8 | Tarea 3: completar el pedido de un grupo en un café (ajustando lo necesario) y calcular el total | `VLOOKUP` de coincidencia exacta |
| 9-10 | Tarea 4: hallar el precio de tres artículos según su color en una tabla de doble entrada y el total | `INDEX` + `MATCH` |

## Cómo se hace cada una

### Estructura del libro y lectura de celdas (1, 3, 4)
- **Contar las hojas**: se miran las pestañas de la parte inferior; con muchas hojas, clic derecho sobre las flechas de navegación abre el cuadro **Activar** con la lista completa. Conviene comprobar también si hay hojas **ocultas** (clic derecho sobre una pestaña > Mostrar).
- **Ver el contenido de una celda de otra hoja**: ir a ella con el **Cuadro de nombres** o `F5` / `Ctrl + G` (por ejemplo `Instrucciones!D32`) y leerla en la barra de fórmulas.
- **Desplegar datos agrupados**: el icono `+` del margen izquierdo (o los botones de nivel `1`, `2`) expande las filas agrupadas.

### `CHOOSE` (2)
```
=CHOOSE(número_de_código, descripción1, descripción2, descripción3)
```
- El **primer argumento** es el número que indica la posición del valor a devolver.
- Las **celdas de la lista se escriben una por una** (`B4,B5,B6`), no como rango (`B4:B6`).
- Cada celda verde debe llevar la **función** (no el texto escrito a mano), porque el código de comprobación verifica que se usó `CHOOSE`.

### `VLOOKUP` por rango (5-6)
```
=VLOOKUP(duración, $tabla_de_tarifas, 2)          (sin cuarto argumento, o con TRUE)
```
- La tabla de tarifas debe estar ordenada de **menor a mayor** por el límite inferior de cada tramo, y la búsqueda devuelve el tramo **menor o igual** al valor buscado.
- La tabla va **absoluta** (`$G$4:$H$9`, con `F4`) para poder copiar la fórmula hacia abajo en todas las llamadas.
- El total se calcula con `SUM` al final de la columna.

### `VLOOKUP` de coincidencia exacta (7-8)
```
=VLOOKUP(artículo, $tabla_de_precios, 2, FALSE)
```
- **El cuarto argumento debe ser `FALSE`**, no `0`: el propio assessment lo exige para que el código de comprobación se genere correctamente.
- Si algún artículo del pedido da `#N/A`, hay que **ajustar el dato** (ortografía, espacios sobrantes con `TRIM`, texto distinto al de la tabla) en vez de forzar el resultado.
- El total del grupo es la suma de la columna de importes.

### `INDEX` + `MATCH` (9-10)
```
=INDEX($matriz_de_precios, MATCH(artículo, $lista_de_artículos, 0), MATCH(color, $lista_de_colores, 0))
```
- El primer `MATCH` da la **fila** (el artículo) y el segundo la **columna** (el color). Si se invierten, el resultado es el precio equivocado.
- Los `MATCH` usan coincidencia **exacta (`0`)**, y los rangos deben coincidir en tamaño con la matriz.

## Gotchas

- **El código de comprobación solo funciona en Excel en inglés (2013 o superior)**; con otro idioma, hay que cambiar el idioma de Excel a inglés mientras se resuelve.
- **No escribir en la celda amarilla**: si se toca, hay que volver a descargar el archivo.
- **Cada celda verde debe llevar la función pedida**, no el valor escrito: el código verifica el *uso* de `CHOOSE`, `VLOOKUP` o `INDEX` + `MATCH`.
- **`FALSE`, no `0`**, en el `VLOOKUP` exacto: es un requisito concreto del assessment.
- **Formato de las respuestas numéricas** (dos decimales, un decimal, sin signo `$`): se aplica con formato de celda, y se escribe solo el número.
- **`VLOOKUP` por rango con la tabla sin ordenar** devuelve un resultado equivocado sin mostrar error.

## Checklist: lo que debo ser capaz de hacer sin ayuda

- [ ] Localizar una celda de otra hoja con el Cuadro de nombres o `F5` y leer su contenido
- [ ] Desplegar datos agrupados (icono `+`) y revisar si hay hojas ocultas
- [ ] Escribir un `CHOOSE` con las celdas de la lista una por una
- [ ] Armar un `VLOOKUP` por rango sobre una tabla ordenada de tramos y copiarlo con la tabla absoluta
- [ ] Armar un `VLOOKUP` exacto con `FALSE` y corregir los `#N/A` ajustando los datos
- [ ] Calcular un total sumando una columna de resultados de búsqueda
- [ ] Escribir un `INDEX` con dos `MATCH` (fila y columna) en el orden correcto
- [ ] Decidir cuál función de búsqueda conviene según el caso (lista corta, tramos, identificador único, doble entrada)
