# Módulo 1 · Spreadsheet Design and Documentation

> Los videos de esta semana, según los archivos en `material_curso/modulo-01/`, son: **V1 Spreadsheet Design Principles · V2 Calculations · V3 Formatting · V4 Documentation · V5 Interface and Navigation**. Las secciones 1-5 se redactaron con conocimiento general apoyado en los nombres de los videos y en el contenido real de los libros del curso (que sí se revisaron: nombres, tablas, fórmulas, comentarios y botones), así que algún detalle del procedimiento podría no calzar al 100% con lo que muestra el video. La sección 6 (atajos, terminología y ninja tips) es material real de la lectura "Week 1: Toolbox".

**Idea central**: una hoja de cálculo buena no es solo la que da el resultado correcto, sino la que **otra persona (o tú en seis meses) puede entender, auditar y ampliar sin romperla**. Todo el módulo trabaja sobre un mismo caso, un informe del mercado inmobiliario de Melbourne (`Melbourne Sales`, más de 12.000 ventas), que va ganando estructura, cálculos, formato, documentación y navegación a lo largo de los cinco videos.

Objetivos de la semana (lectura "Course goals"): diseñar hojas flexibles y auditables, construir cálculos sólidos y transparentes, crear hojas que se documenten solas y usar el formato para mejorar la funcionalidad.

## 1. Principios de diseño y estructura (V1)

**Separar los datos por función**: cada hoja tiene un papel claro, y así se cambia una parte sin tocar las demás. En el libro del curso:

| Hoja | Papel |
|---|---|
| `Data` | Los datos de origen (una tabla con las ventas) |
| `Lookup` / `LookUpList` | Las listas de apoyo (suburbios y su región, tipos de vivienda, métodos de venta, municipios) |
| `Calc` | Los cálculos intermedios |
| `Report` | Lo que ve quien lee el informe |
| `Home` | La portada con los accesos a cada hoja (V5) |

**Principios generales** (conocimiento general, no literal del curso):
- **Una sola fuente por dato (evitar la entrada doble)**: cada valor se escribe en un solo lugar y el resto lo obtiene por fórmula o referencia. Los supuestos (tasas, fechas límite) van en celdas etiquetadas, no escritos dentro de las fórmulas.
- **Separar entradas, cálculos y resultados**, de modo que se vea de dónde sale cada número.
- **Una fórmula consistente por columna**: la misma fórmula de arriba abajo, sin excepciones escondidas.
- **Pensar en que los datos crezcan**: con **tablas**, las fórmulas, las listas y los informes se extienden solos al agregar filas.
- **Evitar celdas combinadas** y filas o columnas en blanco dentro de los datos: dificultan ordenar, filtrar y hacer tablas dinámicas.
- **Validar la entrada** con listas desplegables y validación de datos ([Intermediate II M1](../../../03-intermediate-2/notas/modulo-01/01-data-validation.md)).
- **Dibujar un wireframe antes de construir**: un diagrama en papel con el contenido, la disposición y la jerarquía de la hoja, y los vínculos entre sus partes. En proyectos grandes ayuda a visualizar y optimizar las ideas antes de tocar Excel (ver el término en la sección 6).

### Tablas y rangos con nombre
- Los datos de `Data` son una **tabla** con nombre (`tbl_Sales`), y las listas de `Lookup` son otras tablas (`tbl_Suburbs`, `tbl_Dwellings`, `tbl_Methods`, `tbl_Councils`). Las fórmulas las usan con **referencias estructuradas** (`tbl_Sales[Price]`, `[@Date]`), que se leen mejor y se expanden solas ([Intermediate I M5](../../../02-intermediate-1/notas/modulo-05/05-tables.md)).
- Las columnas de las listas tienen **rangos con nombre** (`Suburb`, `Region`, y luego `lkp_Suburb`, `lkp_Region`...), que se usan en fórmulas de búsqueda y en listas desplegables ([Intermediate I M3](../../../02-intermediate-1/notas/modulo-03/03-named-ranges.md)).
- **Orden entre tabla y nombre**: el libro indica crear primero la tabla y luego los nombres. En una prueba hecha en Excel, un nombre definido sobre toda la columna de datos **crece igual** con la tabla, sin importar si se creó antes o después de ella. La forma más robusta es definirlo como referencia estructurada (`=NombreTabla[Columna]`), que sigue siempre a la tabla.
- **Convenciones de nombres**: en los libros se ve la evolución de los nombres: de `Suburb` y `Region` sueltos a prefijos que dicen qué es cada cosa: `tbl_` para tablas, `lkp_` (*lookup*) para listas de búsqueda y `nav_` para la navegación. Un nombre descriptivo con prefijo hace la hoja **autoexplicativa**.

## 2. Cálculos transparentes (V2)

Los cálculos del libro (`Calc` y la tabla de datos) usan funciones de módulos anteriores combinadas con criterio:

```
=INDEX(Region, MATCH([@Suburb], Suburb, 0))        ← región de cada venta, buscada en la lista
=MONTH([@Date])   =YEAR([@Date])                   ← columnas auxiliares de mes y año
=MAX(tbl_Sales[Date])                              ← fecha de la última venta
=EOMONTH(B3,-12)+1                                 ← primer día del mes, 12 meses antes de la última venta
=EDATE(A7,1)                                       ← el mes siguiente, para ir bajando la serie
=AVERAGEIFS(tbl_Sales[Price], tbl_Sales[Type], D$6, tbl_Sales[Year], $C7, tbl_Sales[Month], $B7)
=(D18-D17)/D17                                     ← variación mensual
=MAX((tbl_Sales[Type]=D$6)*(tbl_Sales[Month]=$B18)*(tbl_Sales[Year]=$C18)*tbl_Sales[Price])   ← precio máximo (fórmula matricial)
```

Lo que enseñan, en términos de diseño:
- **La fecha de partida sale de los datos**, no se escribe a mano: `MAX` da la última venta y `EOMONTH`/`EDATE` construyen los últimos 12 meses. Cuando se agregan ventas, **la serie se actualiza sola**.
- **Una columna auxiliar por cada dato que se necesita** (mes, año, región): así los criterios de `AVERAGEIFS` son simples y cada paso se puede comprobar.
- **Referencias mixtas** (`D$6`, `$C7`, `$B7`) para escribir **una sola fórmula** y copiarla a toda la tabla (el tipo de vivienda viene de la fila de cabecera y el mes y el año de las columnas de la izquierda). El propio libro lo documenta en un comentario ([sección 4](#4-documentación-v4)).
- **Cálculos intermedios visibles** en la hoja `Calc`, en lugar de una fórmula gigante en el informe: el informe solo enlaza a ellos (`=Calc!D18`).
- **Funciones ya conocidas** reutilizadas: `INDEX` + `MATCH` ([Intermediate II M3](../../../03-intermediate-2/notas/modulo-03/03-automating-lookups.md)), `AVERAGEIFS` ([Intermediate I M4](../../../02-intermediate-1/notas/modulo-04/04-summarising-data.md)), y funciones de fecha ([Intermediate I M2](../../../02-intermediate-1/notas/modulo-02/02-text-date-functions.md)).
- **Fórmula matricial**: el cálculo del precio máximo multiplica condiciones dentro de `MAX`; es un adelanto de lo que se verá en el módulo 2 (el libro lo señala en un comentario: *"Array formula has been used"*).

## 3. Formato que ayuda a la función (V3)

El formato no es solo estética: **guía al lector y evita errores**. En el libro:
- **Formatos de número** coherentes (moneda en los precios, fechas con formato de mes y año, porcentajes con un decimal en las variaciones).
- **Formato de las celdas de entrada, de cálculo y de resultado** distinto, para que se distingan de un vistazo (conocimiento general: por ejemplo, un color para las celdas editables).
- **Cabeceras claras** y títulos en cada hoja (`Calculations`, `Lookup Data`, `Melbourne Market Update`).
- **La hoja `Report` como página final**: un resumen con las cifras clave (precio medio, cambio mensual, precio máximo, para casas y apartamentos), **gráficos** y una **segmentación de datos** (*slicer*) por región, con tablas dinámicas de apoyo (`Pivot_Council`, `Pivot_Method`) ([Intermediate I M6](../../../02-intermediate-1/notas/modulo-06/06-pivot-tables-charts-slicers.md)).
- **Estilos y temas** para controlar el formato de todo el libro de golpe (ver la sección 6): un estilo asigna fuente, número, bordes y relleno de una celda, y un tema define colores, fuentes y efectos del documento.
- **Convención de colores del modelo del assessment** (leyenda de la hoja `Inputs`): supuesto variable por el usuario (azul con relleno amarillo y borde), supuesto fijo (azul), cálculo (negro), cálculo único (rojo, para la celda de una fila que lleva una fórmula distinta) y etiqueta de unidades (azul).
- **Formato condicional** para resaltar celdas según su valor (general).
- **Consistencia**: el mismo estilo para lo mismo en todas las hojas; es el consejo que lleva el propio temario ("pay close attention to consistency in design").

## 4. Documentación (V4)

Una hoja **autodocumentada** explica qué hace y por qué, sin que haya que preguntarle a quien la hizo. Lo que se ve en el libro:
- **Comentarios (notas) en las celdas clave**. Por ejemplo, junto a la fecha de inicio de la serie, una nota que explica qué devuelve (*el primer día del mes, 12 meses antes de la fecha de la última venta*); en la fórmula de `AVERAGEIFS`, una que explica por qué se usan referencias mixtas; y en el máximo, que es una fórmula matricial.
- **Una hoja de apuntes o de documentación** dentro del libro (`apuntes`).
- **Nombres descriptivos** con prefijo, para que `tbl_Sales[Price]` o `lkp_Region` se entiendan por sí solos.
- **Etiquetas y títulos visibles** (`Last Sale Date`, `Our Calcs`, `Monthly Change`, `Highest Price`).

Otras prácticas habituales (conocimiento general): una hoja de **portada o instrucciones** (versión, autor, fecha, propósito), una tabla de **supuestos** documentados, y el **control de cambios** (qué se modificó y cuándo).

## 5. Interfaz y navegación (V5)

La hoja debe ser fácil de recorrer, sobre todo cuando tiene muchas filas.
- **Hoja de inicio (`Home`)** con botones (formas con **hipervínculo**) que llevan a cada hoja (`Data`, `Lookup`, `Report`).
- **Hipervínculos** de vuelta al inicio en cada hoja, para no depender de las pestañas.
- **Navegación dentro de los datos**: dos nombres definen los extremos de la tabla: `begin_nav` (`=Data!$A$4`, el inicio) y `nav_end_R` (`=INDEX(Data!F:F, ROWS(tbl_Sales)+4)`, el final). Como el final se calcula con `ROWS` sobre la tabla, **se ajusta solo al agregar ventas**. Dos flechas (arriba y abajo) en la hoja `Data` saltan a esos extremos, mucho más cómodo que desplazarse por más de 12.000 filas.
- **Reducir el ruido**: ocultar líneas de cuadrícula y encabezados en las hojas de presentación, inmovilizar paneles en las tablas y usar colores en las pestañas (conocimiento general).
- **Segmentación de datos (slicer)** en `Report` para filtrar por región de forma visual.

## 6. Atajos, terminología y ninja tips

### Atajos de teclado
Si un atajo pide pulsar dos o más teclas a la vez, se separan con `+`. En Mac, si es distinto, se indica en segundo lugar.

**Navegar**

| Atajo | Acción |
|---|---|
| `Ctrl + Inicio` (Mac: `Cmd + Inicio`) | Ir al inicio de la hoja |
| `Ctrl + flechas` (Mac: `Cmd + flechas`) | Ir al final de un rango contiguo |
| `F5` | Abrir el cuadro *Ir a* |
| `Ctrl + Re Pág / Av Pág` (Mac: `Cmd + Re Pág / Av Pág`) | Hoja anterior / siguiente |
| `Ctrl + Tab` (Mac: `Cmd + ~`) | Ir al libro siguiente |
| `Enter` | Confirmar el cambio de la celda y pasar a la de **abajo** |
| `Tab` | Confirmar el cambio de la celda y pasar a la de la **derecha** |

**Seleccionar**

| Atajo | Acción |
|---|---|
| `Shift + Espacio` | Seleccionar la fila entera |
| `Ctrl + Espacio` | Seleccionar la columna entera |
| `Ctrl + A` (Mac: `Cmd + A`) | Seleccionar toda la hoja (o el conjunto de datos contiguo) |
| `Ctrl + Shift + flechas` | Seleccionar un rango contiguo |

**Fórmulas y funciones**

| Atajo | Acción |
|---|---|
| `Ctrl + Shift + U` | Expandir o contraer la barra de fórmulas |
| `F9` | Según la lectura, "cambiar a la vista de fórmulas". Ojo: en Excel `F9` **recalcula** las fórmulas; para ver las fórmulas en vez de sus resultados se usa `` Ctrl + ` `` (ver [Intermediate II M4](../../../03-intermediate-2/notas/modulo-04/04-formula-auditing-protection.md)) |
| `Shift + F3` | Insertar una función / abrir el asistente de fórmulas |
| `Tab` (tras escribir `=` y el nombre de una función) | Seleccionar la función y abrir el paréntesis |

**Formato**

| Atajo | Acción |
|---|---|
| `Ctrl + 1` (Mac: `Cmd + 1`) | Abrir el cuadro de formato |
| `Ctrl + Shift + !` / `$` / `%` | Formato de número / moneda / porcentaje |
| `F4` | Alternar entre los tipos de referencia de celda |

**Editar y documentar**

| Atajo | Acción |
|---|---|
| `Ctrl + -` | Eliminar filas o columnas |
| `Ctrl + Shift + +` | Agregar filas o columnas |
| `Shift + F2` | Insertar o editar un comentario |
| `Ctrl + F3` (Mac: `Cmd + F3`) | Nombrar una celda o rango |
| `F3` | Abrir el cuadro *Pegar nombre* |

### Terminología
- **Tables (tablas)**: la función Tabla convierte un rango de celdas en un conjunto de datos cohesionado, con mucha automatización y flexibilidad. Ver [Intermediate I M5](../../../02-intermediate-1/notas/modulo-05/05-tables.md).
- **Named Ranges (rangos con nombre)**: también llamados *Defined Names*. Dan un nombre memorable a una celda o rango, y en una fórmula funcionan como una referencia absoluta. Hacen las fórmulas más legibles: `=N4*Pension_Rate` se entiende mejor que `=N4*$P$2`. Ver [Intermediate I M3](../../../02-intermediate-1/notas/modulo-03/03-named-ranges.md).
- **Wire-framing (esquema o boceto)**: término habitual en el diseño de sitios web y software, igual de útil para hojas de cálculo. Un *wireframe* es un diagrama básico del contenido, la disposición y la jerarquía de los elementos de la hoja. Se puede dibujar primero en papel, y en proyectos grandes ayuda a visualizar las ideas y a optimizar el contenido y los vínculos entre los elementos.
- **Styles (estilos)**: funcionan como en Word. Controlan el formato de todo un libro; un estilo asignado a una celda controla todos sus aspectos de formato: tipo, tamaño y color de fuente, formato de número, bordes, relleno, etc.
- **Themes (temas)**: controlan un conjunto de formato para todo el documento (colores, fuentes y efectos). Se pueden personalizar y compartir con colaboradores y con otros programas de Microsoft Office.

### Ninja tips
Hay muchas formas de documentar y dar estilo a una hoja; estos consejos conviene tenerlos en cuenta al diseñar y construir libros:

**Precisión (Accuracy)**
- Preguntarse en cada etapa: *¿este resultado tiene sentido de verdad?* **Nunca confiar en una hoja de cálculo.**
- Elegir la mejor función para el trabajo.
- Con `IF`, **evitar más de tres anidados**. Usar `Alt + Enter` para crear una línea nueva dentro de la fórmula y mejorar su legibilidad.

**Flexibilidad**
- **Evitar a toda costa los valores fijos dentro de las fórmulas** (*hard-coding*).
- Usar rangos con nombre y tablas siempre que se pueda.

**Capacidad de respuesta (Responsiveness)**
- Usar tablas siempre que se pueda. **Los rangos con nombre dentro de tablas son muy potentes**, porque crecen junto con los datos: una forma de "configurar y olvidarse".
- **Evitar las funciones volátiles**: pueden afectar seriamente al rendimiento del libro.
- **Evitar la entrada doble de datos** (*dual data entry*): puede producir errores e inconsistencias, y obliga a rastrear dónde hay que actualizar la misma información más de una vez.

**Fácil de mantener**
- Usar estilos y temas para el formato siempre que se pueda.
- Dedicar tiempo a definir **convenciones de nombres estándar** y aplicarlas en todo el trabajo. Agrupar rangos con nombre y tablas con **prefijos** (por ejemplo `tbl_`, `lkp_`, `nav_`). Si se quiere que los rangos con nombre aparezcan antes que las tablas y otros elementos, usar un prefijo como `AA`.
- **Evitar las macros si es posible**: son geniales pero difíciles de mantener, y casi siempre hay una forma más simple de hacerlo en Excel.

**Fácil de usar (User-friendly)**
- Otras personas tendrán que trabajar con la hoja y entenderla, **incluido tú mismo dentro de seis meses**.
- Revisar el trabajo con un colega (*peer review*).
- Usar comentarios y validación de datos donde corresponda.

## 7. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Modelos de buenas prácticas de hojas de cálculo**: el estándar **FAST** y el enfoque "una hoja de supuestos, una de cálculos, una de resultados" son referencias conocidas en el modelado financiero.
- **Estilos de celda** (Inicio > Estilos de celda) para aplicar el mismo formato a entradas, cálculos y títulos y cambiarlo de golpe.
- **Hoja de control de versiones**: tabla con fecha, autor y cambio realizado.
- **Comprobaciones (checks)**: celdas que verifican que los totales cuadran (por ejemplo, que la suma de las partes sea igual al total) y avisan si algo se rompe.
- **Inspeccionar el libro** (Archivo > Información > Comprobar si hay problemas) para revisar propiedades, comentarios y datos ocultos antes de compartirlo.
- **Protección**: bloquear las celdas de cálculo y dejar editables solo las de entrada ([Intermediate II M4](../../../03-intermediate-2/notas/modulo-04/04-formula-auditing-protection.md)).
- **Evitar números "mágicos"**: un valor fijo dentro de una fórmula (un `0,19` o un `12`) es un supuesto escondido; se saca a una celda con nombre.
- **Accesibilidad**: contraste de colores, texto alternativo en imágenes y gráficos, y no depender solo del color para transmitir información.
- **Hojas ocultas y muy ocultas**: para esconder cálculos auxiliares sin borrarlos, aunque conviene documentarlas.

---

### Práctica
Archivos del curso: `material_curso/modulo-01/`, un libro por video: `W1_01_Design_and_Structure.xlsx`, `W1_02_Calculations.xlsx`, `W1_03_Formatting.xlsx`, `W1_04_Documenting.xlsx` y `W1_05_UI_and_Navigation.xlsx`. Cada uno parte del estado en que dejó el anterior, así que el último es el libro terminado. Ningún video trae versión `Soln`. **Esta semana no hay reto de práctica** (el curso trae un *role play* de cierre, "Designing Professional Spreadsheets", que no se documenta aquí). El assessment de la semana está en `assessment/` y se documenta por habilidades en [assessment.md](assessment.md): construye una hoja mensual de 60 periodos (enero de 2018 a diciembre de 2022) con una sola fórmula por fila.

### Fuente
Temario del curso, lectura "Course goals and weekly learning objectives" (objetivos de la semana) y lectura "Week 1: Toolbox" (atajos, terminología y ninja tips: sección 6). Las secciones 1-5 se apoyan en los nombres de los videos y en el contenido de los libros del curso (nombres, tablas, fórmulas, comentarios y botones revisados), con conocimiento general para los principios. Sección 7 es complemento propio.
