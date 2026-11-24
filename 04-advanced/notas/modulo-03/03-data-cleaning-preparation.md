# Módulo 3 · Data Cleaning and Preparation

> Los videos de esta semana, según los archivos en `material_curso/modulo-03/`, son: **V1 Replace blanks with repeating values · V2 Fix Dates (`DATE`, `MONTH`, `YEAR`, `DAY`, `TEXT`) · V3 Remove Unwanted Spaces (`TRIM`, `CLEAN`) · V4 Diagnostic Tools (`ISNUMBER`, `LEN`, `CODE`) · V5 Remove Unwanted Characters (`SUBSTITUTE`, `CHAR`, `VALUE`)**. Las secciones 1-5 se redactaron con las *flashcards* de cada video (tarjetas reales del curso), el contenido de los cinco libros (revisados) y conocimiento general; algún detalle del procedimiento podría no calzar al 100% con lo que muestra el video. La sección 7 (atajos, terminología y ninja tips) es material real de la lectura "Week 3: Toolbox".

**Idea central**: los datos que llegan de otra fuente casi nunca llegan limpios, y **hay que limpiarlos antes de analizarlos**. Los problemas más comunes son celdas vacías que deberían repetir el valor de arriba, fechas guardadas como texto, espacios y caracteres invisibles, y números que Excel ve como texto. Cada video resuelve uno, y todos usan el mismo patrón: **diagnosticar → limpiar con funciones → fijar los resultados → actualizar el análisis**.

Objetivos de la semana (lectura "Course goals"): arreglar fechas con ayuda de funciones, reemplazar blancos con valores repetidos y quitar espacios y caracteres no deseados de los datos.

## El caso de la semana

Los cinco libros trabajan sobre una **lista de clientes** (362 filas, columnas `Country`, `Organisation`, `Client ID`, `First Name`, `Last Name`, `Join Date`, `Email`, `Events`) y una tabla dinámica que cuenta o suma por organización. Cada libro trae además una hoja `ASCII` con la tabla de códigos (decimal, hexadecimal, binario, carácter, descripción). Los problemas de esos datos son justo los de la semana:

| Columna | Problema | Video que lo resuelve |
|---|---|---|
| `Country` | Solo está escrito en la primera fila de cada grupo; en el resto está en **blanco** (308 de las 362 filas) | V1 |
| `Join Date` | Llega como **texto** con puntos: `2014.01.21` | V2 |
| `Organisation` | Espacios dobles (`ASET  PLC`), espacios al final, y una celda con un carácter de **tabulación** | V3 |
| `Events` | Los números son **texto** con espacios sobrantes (`"2 "`) y espacios de no separación, `CHAR(160)` | V4 y V5 |
| `Email` | Una dirección tiene un `CHAR(160)` en medio | V5 |

Los libros se **arrastran**: cada uno parte del estado en que dejó el anterior, y la hoja limpia suma una columna nueva por video. Las anotaciones en cuadros de texto en español dentro de los libros son del usuario.

## 1. Reemplazar blancos con valores repetidos (V1)

**El problema**: en la columna `Country` solo la primera fila de cada grupo tiene el país; las demás están vacías. Así no se puede filtrar, ordenar ni armar una tabla dinámica por país.

**El método del libro** (hoja `Sheet1 (2)` en `W3_01_Replacing_Blanks.xlsx`):

1. **Duplicar la hoja** de datos. Se reaprovechan los encabezados y el formato, y la hoja original queda intacta.
2. En la celda `A2` de la hoja nueva, escribir:

   ```
   =IF(ISBLANK(Sheet1!A2), A1, Sheet1!A2)
   ```

3. Copiar la fórmula hacia abajo en toda la columna.
4. **Copiar** la columna y pegarla con *Paste Values* sobre sí misma, para quitar las fórmulas y quedarse con los resultados.

**Cómo se lee la fórmula**:

- `Sheet1!A2` es la celda **original** (la hoja con los blancos). `A1` es la celda de **arriba en la hoja nueva**, que ya fue rellenada.
- Si la original está en blanco (`ISBLANK` es `TRUE`), repite el valor de la fila de arriba. Si no lo está (`FALSE`), usa el valor de `Sheet1!A2`.
- Como `A1` ya está completo, el valor se **propaga hacia abajo** fila tras fila. En la primera fila (`A2`), `A1` es el encabezado `Country`, pero allí la celda original no está vacía, así que nunca se usa.

**Por qué pegar valores**: las fórmulas dependen de la hoja original. Si esa hoja se borra o cambia, la hoja limpia se rompe. *Paste Values* la deja independiente.

**Alternativa sin fórmula** (conocimiento general, no es lo que muestra el libro):

1. Seleccionar la columna con los datos.
2. `Ctrl + G` → *Special* → **Blanks**. Esto selecciona **solo las celdas vacías**.
3. Escribir `=` y pulsar la flecha ↑ (apunta a la celda de arriba), y terminar con `Ctrl + Enter`. Se rellenan todas las celdas seleccionadas a la vez.
4. Pegar como valores.

**El inconveniente de arreglar los datos en el mismo sitio**: se **pierde el original**. Si las reparaciones son regulares (datos que llegan cada semana), conviene guardar una hoja con los **datos originales** y limpiar siempre sobre una copia. Es la idea de la hoja de datos crudos de la introducción.

> **`ISBLANK` solo reconoce celdas realmente vacías.** El Toolbox lo advierte: una celda que contiene un espacio *parece* vacía, pero `ISBLANK` devuelve `FALSE`. Si el patrón no rellena algunos blancos, esa celda tiene un espacio o un carácter invisible, y hay que diagnosticarla con las herramientas del V4. Una prueba alternativa es `LEN(TRIM(celda))=0`.

## 2. Arreglar fechas (V2)

**El problema**: `2014.01.21` es **texto**, no una fecha: no se puede ordenar por fecha, restar ni agrupar por mes. Excel solo reconoce una fecha si es un **número de serie** con formato de fecha.

**`DATE(year, month, day)`** tiene **tres argumentos**, en ese orden (el primero es el año), y devuelve una **fecha que Excel reconoce**. Se combina con funciones de texto para extraer las partes:

| Función | Qué devuelve | En `2014.01.21` |
|---|---|---|
| `LEFT(texto, 4)` | Los primeros 4 caracteres | `2014` |
| `MID(texto, 6, 2)` | 2 caracteres desde la posición 6 | `01` |
| `RIGHT(texto, 2)` | Los **2 caracteres de la derecha** | `21` |

Las posiciones: caracteres 1-4 = año, 5 = punto, 6-7 = mes, 8 = punto, 9-10 = día. Con eso:

```
=DATE(LEFT(F2,4), MID(F2,6,2), RIGHT(F2,2))
```

`LEFT`, `MID` y `RIGHT` devuelven **texto**, pero `DATE` lo convierte solo a número; no hace falta `VALUE`. La celda devuelve un número de serie, así que hay que darle **formato de fecha** (`Ctrl + 1`) si no aparece solo.

Es una fórmula de **posiciones fijas**: sirve mientras todas las fechas tengan 4 dígitos de año y 2 de mes y de día. Con `2014.1.5` falla; habría que localizar los puntos con `FIND`.

### `TEXT`: mostrar una fecha como texto

`TEXT(valor, formato)` toma un valor (normalmente una fecha) y lo **muestra** en el formato textual que se indique como segundo argumento. El resultado es **texto**, no un número.

| Código | Resultado con 31/12/2024 en este Excel |
|---|---|
| `"dd"` | `31` |
| `"ddd"` | nombre **abreviado** del día: `mar` |
| `"dddd"` | nombre completo del día: `martes` |
| `"mm"` | mes en dos cifras: `12` |
| `"mmm"` | mes abreviado: `dic` |
| `"mmmm"` | nombre **completo** del mes: `diciembre` |
| `"yy"` / `"yyyy"` | año en 2 / 4 cifras: `24` / `2024` |

En el libro, la columna `Join month` hace `=TEXT(DATE(LEFT(...),MID(...),RIGHT(...)),"mmmm")`, y en el libro siguiente se separa en dos pasos: la columna `Join Date` pasa a ser una fecha real con `DATE` y `Join Month` hace `=TEXT(F2,"mmmm")`. Separarlas es mejor: la fecha real sigue disponible para ordenar y calcular, y el mes en texto sirve para agrupar o filtrar.

La hoja también tiene una demostración con `=TODAY()` en una celda y `TEXT` sobre ella con `dd`, `ddd`, `dddd`, `mm`, `mmm`, `mmmm`, `yy` y `yyyy`.

> **Comprobado en este Excel** (365, separador de argumentos `;`, nombres de mes y día en español): los **códigos de formato siguen siendo los ingleses** (`dd`, `mm`, `yyyy`), y los nombres que devuelve salen en español (`enero`, `martes`). Una prueba con `"aaaa"` no devolvió el año sino el nombre del día, así que no hay que usarlo. En otras instalaciones o configuraciones regionales los códigos podrían variar; si uno falla, probar con el inglés primero.

### El camino inverso: `YEAR`, `MONTH`, `DAY`

Estas tres funciones del título del video **no aparecen en los libros**. Hacen lo opuesto a `DATE`: sacan una parte de una **fecha real** y devuelven un número.

```
=YEAR(F2)    =MONTH(F2)    =DAY(F2)
```

Sirven, por ejemplo, para corregir un año (`=DATE(YEAR(F2)+1, MONTH(F2), DAY(F2))`) o para agrupar por mes con un número en lugar de un texto. Diferencia con `TEXT`: `MONTH(F2)` da el número `12`; `TEXT(F2,"mmmm")` da el texto `diciembre`.

## 3. Quitar espacios no deseados (V3)

**El problema**: para Excel, `"CTX"` y `"CTX "` (con un espacio al final) son **dos valores completamente distintos**. En una tabla dinámica aparecen como dos filas separadas, y el conteo queda repartido. Lo peor es que **los usuarios normalmente no ven esos espacios**. Una pista visible, en el ejemplo del video, es `ASET  PLC` con un **espacio doble** entre las palabras.

**`TRIM`**: quita los espacios del inicio y del final, y reduce los espacios dobles entre palabras a **uno solo**. Los espacios simples entre palabras se **dejan intactos**.

**`CLEAN`**: quita los **caracteres no imprimibles**. Son caracteres que están en la celda pero no se ven ni se imprimen (por ejemplo, la tabulación del código 9, que aparece en la celda `DENIL`). `CLEAN` elimina solo los primeros 32 caracteres del ASCII, los de código decimal **0 a 31**.

La combinación del libro:

```
=TRIM(CLEAN(Data!B2))
```

`CLEAN` está **dentro** de `TRIM`, así que se calcula primero. Se ve en el libro como `=TRIM(CLEAN(Data!B2))` en la columna `Organisation` de la hoja limpia.

**Después de limpiar, hay que actualizar la tabla dinámica**: no se actualiza sola al cambiar los datos de origen. Se hace en la pestaña **Analyze → Refresh** (o `Alt + F5`; `Ctrl + Alt + F5` actualiza todas). Hasta que no se haga, la tabla seguirá mostrando `CTX` y `CTX ` por separado. El libro `W3_03` incluye la tabla dinámica `Count of Organisation`, que cuenta las organizaciones: tras limpiar, una sola fila por organización.

**Los límites**: `CLEAN` **no elimina todos** los caracteres no imprimibles, y `TRIM` tampoco quita el espacio de no separación (`CHAR(160)`). Para eso están los dos videos siguientes. Tampoco arreglan **errores de escritura**: en las tablas dinámicas de los libros aparece `Inteligence Systems` junto a `Intelligence Systems`, y ninguna función de limpieza las une; eso se corrige con *Find & Replace* (`Ctrl + H`) o a mano.

## 4. Herramientas de diagnóstico (V4)

Antes de limpiar hay que saber **qué hay** en la celda. Las tres funciones del video, más `ISTEXT` y los códigos ASCII:

| Función | Pregunta que responde |
|---|---|
| `ISNUMBER(celda)` | ¿Es un número de verdad? (`TRUE`/`FALSE`) |
| `ISTEXT(celda)` | ¿Es texto? (la versión opuesta de `ISNUMBER`) |
| `LEN(celda)` | ¿Cuántos caracteres tiene, **incluyendo los que no se ven**? |
| `CODE(celda)` | ¿Cuál es el código decimal ASCII del **primer** carácter? |

**Pista visual**: los números reales se alinean a la **derecha** por defecto. Si algo que parece un número va a la **izquierda**, probablemente está guardado como **texto**. Es solo una pista, porque la alineación se puede cambiar a mano. `ISNUMBER` es la prueba fiable.

**ASCII**: los ordenadores solo almacenan datos en **binario**, así que cada carácter tiene un código numérico. La `A` mayúscula es el **65** y el **espacio es el 32**. En el libro, la hoja `ASCII` trae la tabla completa.

### El diagnóstico del libro

Las columnas de diagnóstico, en `W3_04_Diagnostic_Tools.xlsx`, sobre la columna `Events`:

| Columna | Fórmula | Resultado en `"2 "` |
|---|---|---|
| `isNumber` | `=ISNUMBER(I4)` | `FALSE` (es texto) |
| `len` | `=LEN(I4)` | `2` (el valor visible tiene 1 carácter) |
| `code` | `=CODE(RIGHT(I4,1))` | `32`: **termina en un espacio** |

`CODE` solo mira el **primer** carácter, por eso se combina con `RIGHT(celda, 1)` para ver el último. Para revisar **cada posición**, el libro construye una cuadrícula con contadores `1, 2, 3, 4` en la fila de encabezado:

```
=IFERROR(CODE(MID($I3, M$1, 1)), "")
```

`MID($I3, M$1, 1)` toma el carácter de la posición indicada por el contador, `CODE` lo convierte a número y `IFERROR` deja la celda vacía cuando la posición ya no existe (el valor es más corto). Así se ve el código de cada carácter de la celda.

**La tabla dinámica es lo que delata el problema.** Al poner `Events` en *Values*, lo primero que hace Excel por defecto es **contar** (porque los valores son texto). Si se cambia a `Sum`, **todos los resultados salen en cero**, ya que no hay ningún número que sumar. Es lo que muestran las dos hojas del libro `W3_05`: `pivot_table_error` (suma en cero) y `pivot_table_without_error` (suma real, después de limpiar).

Verificado sobre una copia: los 362 valores de `Events` son texto (`ISNUMBER` no da `TRUE` en ninguno), 359 terminan en el carácter **160**, 2 terminan en un espacio normal (32) y uno viene limpio. Además, el 160 y el 32 están **mezclados en distintas posiciones** (`"1"` + 160 + 32, o `"11"` + 32 + 160), por eso hay que quitar ambos.

## 5. Quitar caracteres no deseados (V5)

**`CHAR(código)`** es básicamente el **opuesto de `CODE`**: recibe un código ASCII y devuelve el **carácter** correspondiente. **`CHAR(160)`** representa el **espacio de no separación** (*non-breaking space*), típico de datos copiados de páginas web: se ve igual que un espacio, pero no es el código 32, así que `TRIM` no lo quita, y `CLEAN` tampoco (solo llega al 31).

**`SUBSTITUTE(texto, texto_viejo, texto_nuevo, [núm_instancia])`** es el equivalente en fórmula de *Buscar y reemplazar*. El **tercer argumento** es el texto de reemplazo; si se pone `""` (comillas vacías), **elimina** los caracteres encontrados. Sin el cuarto argumento reemplaza **todas** las coincidencias.

```
=SUBSTITUTE(Data!H3, CHAR(160), "")     ← quita todos los caracteres 160
```

**Después de sustituir suelen quedar espacios extras**, así que se pasa `TRIM`. Y para que la tabla dinámica pueda sumar, **`VALUE`** convierte el texto en número. Solo funciona con texto que contiene **solo números**; si hay otros caracteres, devuelve un error.

La fórmula completa, de adentro hacia afuera:

```
=VALUE(TRIM(SUBSTITUTE(Data!H3, CHAR(160), "")))
```

1. `SUBSTITUTE` quita los espacios de no separación.
2. `TRIM` limpia los espacios normales que quedaron.
3. `VALUE` convierte el texto en número.

Se escribe en la primera celda y se copia al resto de la columna con **doble clic** en el controlador de relleno. Probada sobre una copia del libro: las 362 celdas de `Events` se convierten en números y la tabla dinámica ya puede sumar.

> **Un `SUBSTITUTE` con un espacio normal no quita el 160.** `SUBSTITUTE(Data!G2, " ", "")` quita los espacios con código 32, pero no los 160. En la columna `Email` hay una celda con un 160 en medio (`...@ctx<160>.com`), que esa fórmula dejaría intacta. Cuando `LEN` y `CODE` muestran que hay un carácter que no se ve, hay que **identificar su código antes de reemplazarlo**, y luego usar `CHAR(código)` en `SUBSTITUTE`.

> **`CHAR` solo cubre los códigos hasta el 255.** Los correos del libro tienen letras como `ż` o `š`, que son caracteres legítimos de otros idiomas y quedan fuera del ASCII estándar. Para esos casos existe **Unicode** (`UNICODE` y `UNICHAR`, ver ninja tips).

## 6. Receta completa de limpieza

Un recorrido ordenado, que es el que va armando la hoja limpia (`Data (2)`) a lo largo de la semana:

| Paso | Qué se hace | Herramienta |
|---|---|---|
| 0 | Duplicar la hoja y conservar el original sin tocar | Hoja de datos crudos |
| 1 | Rellenar los blancos de `Country` | `IF(ISBLANK(...), celda de arriba, original)` |
| 2 | Convertir `Join Date` en fecha real | `DATE(LEFT, MID, RIGHT)` |
| 3 | Sacar el mes como texto | `TEXT(fecha, "mmmm")` |
| 4 | Quitar espacios y no imprimibles de `Organisation` | `TRIM(CLEAN(...))` |
| 5 | Diagnosticar `Events` | `ISNUMBER`, `LEN`, `CODE`, `MID` |
| 6 | Quitar `CHAR(160)`, los espacios y convertir en número | `VALUE(TRIM(SUBSTITUTE(..., CHAR(160), "")))` |
| 7 | **Pegar como valores** y actualizar la tabla dinámica | *Paste Values*, *Analyze → Refresh* |

Regla práctica: **diagnosticar antes de limpiar** (`LEN` y `CODE` dicen qué hay), **limpiar con funciones** (el original queda como respaldo), **fijar los resultados** cuando ya no hagan falta las fórmulas y **actualizar** todo lo que dependa de los datos.

## 7. Atajos, terminología y ninja tips

### Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + ;` (Mac: `Cmd + ;`) | Inserta la **fecha de hoy como valor fijo**, a diferencia de `TODAY()`, que es dinámica. `TODAY` es una **función volátil**: se recalcula cada vez que se cambia algo en la hoja y consume recursos de cálculo |

### Terminología

- **Non-Printing Character** (carácter no imprimible): un carácter que está ahí pero no se muestra al imprimir y no se puede ver, por ejemplo un espacio.
- **Binary** (binario): el lenguaje de bajo nivel que entiende el ordenador, hecho de 0 y 1.
- **Decimal**: el sistema numérico de base 10 (por ejemplo 13, 32). Es más fácil de entender que el binario y se usa en su lugar.
- **ASCII** (*American Standard Code for Information Interchange*): una tabla que relaciona caracteres conocidos, como el espacio, con su representación en binario, que es lo que entiende el ordenador.

### Funciones de limpieza (resumen del Toolbox)

| Función | Qué hace |
|---|---|
| `ISBLANK` | Prueba si una celda está en blanco. Una celda con un espacio parece vacía, pero Excel reconoce que tiene un carácter |
| `TEXT` | Toma un número en un formato dado y devuelve texto en el formato indicado. Con una fecha en `J1`, `TEXT(J1,"mmmm")` da el nombre completo del mes |
| `DATE` | Combina tres valores para formar una fecha que Excel reconoce |
| `TRIM` | Quita espacios iniciales, finales y sobrantes. Quita los dobles entre palabras, pero **no** los simples |
| `CLEAN` | Quita **algunos, no todos**, los caracteres no imprimibles: los 32 primeros del ASCII (decimal 0 a 31) |
| `ISNUMBER` | Devuelve `TRUE` si el argumento es numérico |
| `ISTEXT` | La versión para texto de `ISNUMBER` |
| `LEN` | Número de caracteres de la celda, **incluyendo los que no se ven** |
| `CODE` | Convierte un carácter en su equivalente decimal (valor ASCII) |
| `SUBSTITUTE` | El equivalente en fórmula de *Buscar y reemplazar* |
| `CHAR` | Lo opuesto de `CODE`: toma un valor decimal y devuelve el carácter |
| `VALUE` | Convierte texto en valores numéricos. Solo sirve con texto que contiene números; con otros caracteres no funciona |

### Ninja tips

**Unicode**
- Igual que ASCII, existe **Unicode**, que cubre muchos más caracteres, incluidos los de otros idiomas distintos del inglés.
- `UNICODE(texto)` devuelve el número (código) del **primer carácter** del texto. Ejemplo del Toolbox: `UNICODE("C")` devuelve el número de la letra C (67). Es la versión de `CODE` para cuando el carácter no cabe en el ASCII.

## 8. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Power Query** (*Datos → Obtener y transformar*): hace todo lo de esta semana sin fórmulas y de forma **repetible**. *Rellenar hacia abajo* (**Fill Down**) sustituye al patrón de `ISBLANK`; *Recortar* (**Trim**), *Limpiar* (**Clean**) y *Reemplazar valores* sustituyen a `TRIM`, `CLEAN` y `SUBSTITUTE`; y al cargar datos nuevos la limpieza se aplica sola, sin tocar el original. Es la respuesta al problema de las reparaciones regulares.
- **Texto en columnas** (*Datos → Text to Columns*): separa una cadena como `2014.01.21` por el delimitador `.` en tres columnas (año, mes, día) y se puede armar la fecha con `DATE`, sin `LEFT`/`MID`/`RIGHT`. En el último paso se puede indicar el tipo de columna *Fecha* con orden `AMD` y convertirla directamente.
- **`DATEVALUE`**: convierte un texto con aspecto de fecha en número de serie. Depende de la configuración regional del equipo, por eso `DATE` con `LEFT`/`MID`/`RIGHT` es más robusta.
- **Convertir texto en número sin `VALUE`**: multiplicar por 1 (`=A2*1`), usar el doble signo `--A2`, o el *Pegado especial → Multiplicar* con un 1; también el aviso verde de la celda (*Convert to Number*).
- **`NUMBERVALUE`**: como `VALUE`, pero permite indicar el separador decimal y el de miles; útil cuando los números vienen con el formato de otro país.
- **Reemplazar el 160 a mano**: en *Buscar y reemplazar* (`Ctrl + H`) se puede escribir el carácter con `Alt + 0160` (teclado numérico) en el cuadro "Buscar". Sirve para limpiar el libro sin fórmulas, aunque no es repetible.
- **Más funciones de texto**: `UPPER`, `LOWER` y `PROPER` (mayúsculas, minúsculas y nombre propio), `REPLACE` (reemplaza por **posición** en lugar de por texto, a diferencia de `SUBSTITUTE`), `FIND`/`SEARCH` (localizar un carácter), `CONCAT` y `TEXTJOIN`.
- **Funciones nuevas de Excel 365**: `TEXTBEFORE`, `TEXTAFTER` y `TEXTSPLIT` separan texto por delimitador sin `LEFT`/`MID`/`RIGHT`; `UNICHAR` es la versión Unicode de `CHAR`.
- **`ISBLANK`, `=""` y `LEN(...)=0` no son lo mismo**: `ISBLANK` solo es `TRUE` en celdas realmente vacías; `=""` también es `TRUE` en una celda con una fórmula que devuelve texto vacío; `LEN(TRIM(celda))=0` detecta además las que solo tienen espacios.
- **Validación de datos** (*Datos → Validación*): prevenir es mejor que limpiar. Restringir el formato de fecha o la longitud del texto evita que entren datos sucios.
- **`LET`** (Excel 365): permite dar nombre a un paso intermedio dentro de una fórmula larga, útil para encadenar varias limpiezas con claridad.

---

### Práctica
Archivos del curso: `material_curso/modulo-03/`, un libro por video: `W3_01_Replacing_Blanks.xlsx`, `W3_02_Fix_Dates.xlsx`, `W3_03_Trim_and_Clean.xlsx`, `W3_04_Diagnostic_Tools.xlsx` y `W3_05_Remove_Unwanted_Characters.xlsx`. En `challenge/` están el reto (`C4-W3-Practice-Challenge.xlsx`) y su solución (`...-Solution.xlsx`). El reto (13 pasos sobre una base de datos de estudiantes: blancos, fechas de trimestre, espacios, `CHAR(160)` y tablas dinámicas) se documenta en [practice-challenge.md](practice-challenge.md). El assessment está en `assessment/` y se documenta por habilidades en [assessment.md](assessment.md): limpiar 496 registros de ventas, cada uno una sola cadena de texto, con una columna de fórmulas por paso (`CLEAN`, `SUBSTITUTE`, `TRIM`), localizar los espacios con `FIND` y extraer producto, vendedor, fecha e importe con `LEFT`, `MID`, `VALUE` y `DATE`.

### Fuente
Temario del curso, lectura "Course goals and weekly learning objectives" (objetivos de la semana), las *flashcards* de los cinco videos (V1 a V5) y la lectura "Week 3: Toolbox" (sección 7). Las secciones 1-6 se apoyan en esas tarjetas y en el contenido de los libros del curso (columnas, fórmulas, tablas dinámicas y cuadros de texto revisados, con comprobaciones sobre copias). La sección 8 es complemento propio.
