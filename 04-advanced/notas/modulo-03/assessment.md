# Módulo 3 · Assessment (Final Assignment de la semana)

> Archivo del curso: `material_curso/modulo-03/assessment/`. Aquí se documentan **las habilidades que evalúa**, no las respuestas ni los resultados. Para la teoría, ver [03-data-cleaning-preparation.md](03-data-cleaning-preparation.md); para el reto de la semana, [practice-challenge.md](practice-challenge.md).

**Escenario**: se reciben **datos de ventas sucios** de la base de datos de la empresa (496 registros, todos de octubre de 2017) y hay que limpiarlos con **fórmulas**, porque el análisis se repetirá cada mes con registros nuevos y el libro debe poder actualizarse fácilmente. Cada registro es **una sola cadena de texto** con un patrón fijo, según el administrador de datos: unas barras `/`, el producto con prefijo `PR:`, unos guiones bajos `_`, el vendedor con prefijo `SP:`, la fecha en formato `yyyymmd`, unos espacios de no separación, el importe con prefijo `$` y, al final, un carácter aleatorio de control (código ASCII 28 a 31). Son **19 preguntas**, pero la numeración tiene huecos: las preguntas 4 y 5 son teóricas y no tienen columna propia. Las demás piden el valor de una celda de comprobación (*check sum*) o una suma.

**Método de trabajo**: en la hoja `Data` se construye **una columna por paso, hacia la derecha**, y cada columna refina la anterior; al final se separan cuatro datos (producto, vendedor, fecha e importe) que la hoja `Clean` ya tiene enlazados y que quedan listos para analizar.

**Avisos del enunciado**: comprobar que el *Check Sum* de `Data!B2` sea el valor que indica el libro antes de empezar, existen dos versiones del libro (Unicode, para Excel 2013 o posterior y Office 365; y otra para Excel 2010 y 2007), no insertar filas ni columnas en `Data` (cambiaría las comprobaciones) y guardar el trabajo.

## Habilidades por pregunta

### Parte 1 · Diagnosticar y quitar los caracteres de control finales

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 1 | **Diagnosticar** el último carácter de cada registro y **contar** cuántos tienen un código dado | `RIGHT` + `CODE` para el código ASCII final; `COUNTIFS` para contar |
| 2 | **Quitar** los caracteres no imprimibles finales | `CLEAN` sobre la columna original; comprobar con el *check sum* |
| 3 | **Verificar** que la limpieza anterior funcionó volviendo a mirar el último carácter | La misma fórmula `RIGHT` + `CODE`, ahora sobre la columna limpia |

### Parte 2 · Reemplazar y quitar caracteres con `SUBSTITUTE`

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 4 | **Elegir la función** que cambia todas las apariciones de un carácter por otro (pregunta teórica) | `SUBSTITUTE` frente a `REPLACE` |
| 5 | **Representar el espacio de no separación** dentro de una fórmula (pregunta teórica) | El código del carácter para `CHAR`; `UNICHAR` en el libro Unicode |
| 6 | **Reemplazar** los espacios de no separación por espacios normales | `SUBSTITUTE` con `CHAR` en el 2.º argumento |
| 7 | **Quitar** las barras iniciales reemplazándolas por **nada** | `SUBSTITUTE` con texto vacío `""` |
| 8 | **Reemplazar** los guiones bajos por **un espacio** (no por nada), para poder separar después producto y vendedor | `SUBSTITUTE` con `" "` |

### Parte 3 · Espacios y prefijos

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 9 | **Reducir los espacios** repetidos y los de los extremos después de haber quitado `/` y `_`; **comprobar con el largo** en lugar de un *check sum* | `TRIM` y `LEN`, con la suma que ya trae el libro |
| 10 | **Diagnosticar** que todos los registros terminen en un dígito; **quitar el prefijo** del producto | `CODE` y `RIGHT`; `SUBSTITUTE` con `""` |
| 11 | **Quitar el prefijo** del vendedor | `SUBSTITUTE` con `""`, partiendo de la columna anterior |
| 12 | **Insertar un espacio** entre el vendedor y la fecha reemplazando el inicio común de las fechas | `SUBSTITUTE` con un texto lo bastante largo para no alterar los importes |

### Parte 4 · Localizar espacios y extraer los cuatro datos

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 12 (2.ª parte) | **Localizar el primer espacio** del registro limpio (función no vista en los videos); la misma pregunta 12 lo pide justo después del reemplazo anterior | `FIND(" ", texto, 1)` |
| 13 | **Localizar el segundo espacio** usando el primero como punto de partida | `FIND` con el 3.er argumento igual a la posición anterior `+ 1` |
| 14 | **Localizar el tercer espacio**, con dos fórmulas equivalentes posibles | `FIND` con el segundo espacio `+ 1`, o `FIND("$", ...) - 1` |
| 15 | **Extraer el producto** (el inicio del texto) | `LEFT` con la posición del primer espacio `- 1` |
| 16 | **Extraer el vendedor** (el centro del texto) | `MID` con inicio y número de caracteres calculados con las posiciones de los espacios |
| 17 | **Extraer la fecha como texto** con la misma construcción, usando el 2.º y el 3.er espacio | `MID`; sin `TEXT` en este paso |
| 18 | **Extraer el importe como número** sin el `$` | `VALUE` + `MID` con un número de caracteres muy grande para tomar lo que quede |
| 19 | **Convertir la fecha de texto en fecha real** cuando el día tiene una o dos cifras | `DATE(2017,10,MID(...))`; comprobación con el *check sum* de la columna |

## Cosas a tener en cuenta (trampas)

- **Cada columna se calcula sobre la anterior**, no sobre el registro original. Si se parte de la columna equivocada, el *check sum* no coincide.
- **`CLEAN` no limpia todo**: quita los caracteres de control finales, pero no los espacios de no separación; para esos hace falta `SUBSTITUTE` con `CHAR` o `UNICHAR`.
- **Reemplazar por nada o por un espacio es una decisión de diseño**: las barras se quitan, pero los guiones bajos se convierten en espacio porque ese espacio sirve después para separar el producto del vendedor.
- **Esperar al `TRIM`** hasta que ya se hayan quitado `/` y `_`, para usarlo una sola vez.
- **`SUBSTITUTE` con un texto demasiado corto** (por ejemplo una sola cifra) puede alterar los importes. Elegir un texto lo bastante largo y específico, y recordar que esa fórmula **hay que actualizar cada mes**.
- **`FIND` empieza a buscar donde se le indica**: para encontrar el segundo espacio se parte de la posición del primero `+ 1`, o encuentra el mismo otra vez.
- **El día de la fecha no lleva cero a la izquierda**: hay fechas de 7 y de 8 caracteres, así que no sirve cortar con posiciones fijas desde la izquierda para el día.
- **`MID` con un número grande** devuelve lo que queda del texto, útil para extraer hasta el final.
- **`VALUE` para convertir el importe** en número; sin él, la suma da 0.
- **`Check_Sum` es relativo a la columna**: el mismo nombre definido calcula la suma de la columna donde se escribe. Cada celda de la fila 2 usa una versión distinta según la pregunta.
- **Guardar** el libro al terminar; la hoja `Clean` ya está enlazada y se rellena sola.

## Checklist del assessment

- [ ] Código ASCII del último carácter con `RIGHT` + `CODE` y conteo con `COUNTIFS`
- [ ] Quitar caracteres de control con `CLEAN` y verificar el resultado
- [ ] `SUBSTITUTE` con `CHAR` (o `UNICHAR`) para el espacio de no separación
- [ ] `SUBSTITUTE` con `""` para quitar y con `" "` para reemplazar por espacio
- [ ] `TRIM` al final de las sustituciones y comprobación con `LEN`
- [ ] Quitar los prefijos `PR:` y `SP:`
- [ ] Insertar un espacio con un reemplazo que no altere los importes
- [ ] `FIND` para localizar el 1.º, 2.º y 3.er espacio
- [ ] `LEFT` y `MID` con posiciones calculadas para producto, vendedor y fecha
- [ ] `VALUE` + `MID` para el importe numérico
- [ ] `DATE` con `MID` para pasar la fecha de texto a fecha real
