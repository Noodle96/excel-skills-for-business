# Módulo 2 · Text and Date Functions

> Los videos de esta semana, según los archivos en `material_curso/modulo-02/`, son: **V1 Combining Text · V2 Changing Case · V3 Extracting Text · V4 Find · V5 Date Functions**. Las secciones 2 y 4 están redactadas con conocimiento general (el nombre de archivo confirma el tema, no el contenido exacto del video); las secciones 1, 3 y 5, y las secciones 6-9, usan la terminología real de la lectura "Week 2: Toolbox" (esta semana el toolbox es especialmente completo).

## 1. Combinar texto: CONCAT, CONCATENATE y `&` (V1)

- **`CONCAT`** une texto: por referencia de celda `=CONCAT(B4,B3)`, o texto directo `=CONCAT("John","Smith")` (el texto va entre comillas). La coma indica qué unir.
- **`&`** hace lo mismo pero como operador dentro de una fórmula: `=B4&B3`, o con texto directo `="John"&"Smith"`. Como es parte de una fórmula, siempre debe empezar con `=`.
- **`CONCAT` es la versión moderna de `CONCATENATE`** (disponible desde Office 365, enero 2016). Hace todo lo que `CONCATENATE` hace, y además admite un **rango** como argumento: `=CONCAT(A5:A20)`. `CONCATENATE` sigue existiendo en versiones nuevas, pero sin esa funcionalidad extra.
- **Truco de teclado**: al escribir una función como `CONCAT` y necesitar seleccionar varias celdas, mantén presionado `Ctrl` mientras haces clic en cada celda — Excel inserta automáticamente la coma entre cada referencia. Ej.: para `=CONCAT(A2,D2,G2)`, mantén `Ctrl` y haz clic en `A2`, luego `D2`, luego `G2`.

## 2. Cambiar mayúsculas y minúsculas (V2)

| Función | Qué hace | Ejemplo |
|---|---|---|
| `UPPER` | Convierte todo el texto a MAYÚSCULAS | `=UPPER("hola")` → `HOLA` |
| `LOWER` | Convierte todo el texto a minúsculas | `=LOWER("HOLA")` → `hola` |
| `PROPER` | Pone en mayúscula la primera letra de cada palabra | `=PROPER("hola mundo")` → `Hola Mundo` |

Útil para normalizar datos importados con formato inconsistente (ej. nombres en mayúsculas mezclados con minúsculas).

## 3. Extraer texto: LEFT, MID, RIGHT, y Texto en columnas (V3)

| Función | Qué hace | Ejemplo |
|---|---|---|
| `LEFT(texto, núm_caracteres)` | Extrae desde el inicio | `=LEFT("Excel",3)` → `Exc` |
| `RIGHT(texto, núm_caracteres)` | Extrae desde el final | `=RIGHT("Excel",3)` → `cel` |
| `MID(texto, posición_inicial, núm_caracteres)` | Extrae desde una posición intermedia | `=MID("Excel",2,3)` → `xce` |

**Texto en columnas (Text to Columns)** es la alternativa "manual" a estas funciones, para un cambio puntual de una sola vez:

1. Abrir el archivo, seleccionar la columna a dividir (ej. una columna "Location" con datos tipo `Piso-Ala-Extensión`).
2. Datos > Texto en columnas.
3. Elegir **Delimitado** (divide donde encuentre un carácter, ej. espacio o guion) o **Ancho fijo** (divide según posiciones de caracteres exactas).
4. Con Delimitado: marcar los delimitadores que apliquen (ej. Espacio, y escribir "-" en Otro) y avanzar.
5. Elegir el formato de los datos resultantes (normalmente General) y Finalizar. Excel avisará que ya hay datos en esas columnas y preguntará si reemplazarlos.

**Ancho fijo** funciona bien si el patrón de caracteres es siempre del mismo largo (ej. el piso), pero falla si el largo varía (ej. un "ala" que a veces tiene 4 caracteres y a veces 5) — ahí conviene Delimitado o las funciones de texto.

**Texto en columnas es ideal para un cambio de una sola vez** cuando no necesitas conservar el dato original. Para cambios dinámicos y automáticos a medida que se agregan datos nuevos, las funciones (`LEFT`/`MID`/`RIGHT`) son mucho más útiles.

## 4. Buscar texto: FIND (V4)

`FIND(texto_buscado, dentro_del_texto, [núm_inicial])` devuelve la **posición** (un número) donde aparece el texto buscado. Ejemplo: `=FIND(" ","Excel Skills")` → `6` (el espacio está en la posición 6).

Se usa muy seguido como función "ayudante" dentro de `MID`/`LEFT`/`RIGHT` cuando no sabes de antemano cuántos caracteres extraer (ver funciones anidadas, sección 7).

## 5. Funciones de fecha (V5)

| Función | Qué hace |
|---|---|
| `TODAY()` | Devuelve la fecha de hoy (sin hora). Se actualiza cada vez que se abre el archivo |
| `NOW()` | Devuelve fecha y hora actuales. También se actualiza (es "volátil") |
| `YEARFRAC(fecha_inicio, fecha_fin)` | Calcula la fracción de año entre dos fechas (ej. para calcular antigüedad o edad de forma decimal) |

**Fecha/hora fija vs. función**: `Ctrl + ;` inserta la fecha de hoy como **valor fijo** (no cambia si abres el archivo mañana), a diferencia de `=TODAY()` que sí se recalcula cada vez. `Ctrl + Shift + ;` (Mac: `Cmd + ;`) hace lo mismo con la hora actual, a diferencia de `=NOW()`.

## 6. Anatomía de una función

Usando `MID` como ejemplo: `=MID(texto, posición_inicial, núm_caracteres)`

- **`=`**: toda función debe empezar con el signo igual.
- **Nombre de la función** (`MID`): la sintaxis estándar usa mayúsculas, pero funciona igual en minúsculas.
- **`(`**: después del nombre de la función va un paréntesis de apertura; ahí empiezan los argumentos.
- **Argumentos** (`texto, posición_inicial, núm_caracteres`): las entradas que la función necesita para procesar el resultado. Se pueden escribir directamente o calcular con otra función (ver funciones anidadas).
- **Referencia de celda o rango**: según la función, un argumento puede pedir una celda (`A2`) o un rango (`A2:A10`, donde `:` se lee como "hasta").
- **`,`**: las comas separan los argumentos entre sí, para que Excel sepa dónde termina uno y empieza el siguiente. Algunas funciones tienen un solo argumento y no necesitan coma.
- **`)`**: el paréntesis de cierre le indica a Excel que ya no hay más argumentos. En versiones recientes de Excel, si lo omites y presionas Enter, Excel lo agrega automáticamente.

## 7. Funciones anidadas

Una función puede usarse como argumento de otra — eso es una **función anidada**. Ejemplo:

```
=MID(A2,2,FIND(" ",A2))
```

Aquí `FIND(" ",A2)` se usa como el tercer argumento de `MID` (el número de caracteres). Excel calcula primero la función **más interna** y va resolviendo hacia afuera — en vez de escribir un número fijo de caracteres, la función "ayudante" (`FIND`) hace que ese valor sea **dinámico**: se recalcula si el texto de origen cambia.

## 8. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl` + clic (al seleccionar celdas dentro de una función) | Inserta automáticamente la coma entre cada referencia seleccionada |
| `Ctrl + ;` | Inserta la fecha de hoy como valor **fijo** (no como función) |
| `Ctrl + Shift + ;` (Mac: `Cmd + ;`) | Inserta la hora actual como valor **fijo** |
| `F4` (Mac: `Cmd + T`) | Alternar entre referencia relativa y absoluta |

## 9. Ninja tips de la semana

**TEXTJOIN**: otra función para unir texto, con dos ventajas sobre `CONCAT`:
1. Defines el separador **una sola vez**, en vez de tener que incluirlo entre cada par de textos.
2. Puedes elegir si **ignorar celdas vacías** del rango o no.

Sintaxis: `=TEXTJOIN(separador, ignorar_vacías, texto1, [texto2], ...)`. Ejemplo: `=TEXTJOIN(" ", FALSE, "JOHN", "SMITH")` → `JOHN SMITH`. También acepta un rango: `=TEXTJOIN(" ", TRUE, A5:A12)`. Al igual que `CONCAT`, solo está disponible en Office 365.

**Salto de línea dentro de una función de texto**: el atajo `Alt + Enter` (visto en Essentials) para insertar un salto de línea **no funciona** dentro de una función de texto. En su lugar, se usa la función `CHAR(10)`:
```
=A2&CHAR(10)&B2
=CONCAT(A2,CHAR(10),B2)
```
Para que el salto de línea se vea, hay que activar **Ajustar texto (Wrap Text)** en esa celda — si no, todo el texto se muestra en una sola línea.

## 10. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **`SEARCH` vs. `FIND`**: `SEARCH` hace lo mismo que `FIND` pero **no distingue mayúsculas de minúsculas** y sí admite comodines (`*`, `?`). `FIND` sí distingue mayúsculas/minúsculas y no admite comodines.
- **`TRIM` y `CLEAN`**: `TRIM(texto)` quita espacios sobrantes (deja solo uno entre palabras); `CLEAN(texto)` quita caracteres no imprimibles. Muy usadas al limpiar datos importados — se ven a fondo en el curso Advanced (módulo de Data Cleaning).
- **`LEN(texto)`**: cuenta cuántos caracteres tiene un texto. Combinada con `FIND`, permite extraer "todo lo que queda después de X carácter" sin contar a mano.
- **`VALUE` y `TEXT`**: `VALUE(texto)` convierte un texto que representa un número (ej. `"123"`) en un número real que se puede calcular; `TEXT(número, formato)` hace lo contrario — convierte un número en texto con un formato específico (ej. `=TEXT(HOY(),"DD/MM/YYYY")`).
- **Otras funciones de fecha útiles**: `WEEKDAY()` (día de la semana como número), `DATEDIF()` (diferencia entre 2 fechas en años/meses/días — función "oculta", no aparece en el autocompletado pero funciona), `EDATE()`/`EOMONTH()` (sumar meses a una fecha / fin de mes). Estas últimas se ven con más detalle en el curso Advanced.

---

### Práctica
Archivos del curso: `material_curso/modulo-02/` (no hay archivos `Soln` para los videos; el reto de la semana sí trae solución en `PracticeChallenge/C2-W2-Practice-Challenge-Solution.xlsx`).

### Fuente
Lectura "Week 2: Toolbox" del curso (atajos, anatomía de una función, funciones anidadas, CONCAT/CONCATENATE/&, Texto en columnas, ninja tips de TEXTJOIN y salto de línea — secciones 1, 3, 5, 6, 7, 8 y 9). Secciones 2 y 4 se basan en los nombres de los videos, redactadas con conocimiento general. Sección 10 es complemento propio.
