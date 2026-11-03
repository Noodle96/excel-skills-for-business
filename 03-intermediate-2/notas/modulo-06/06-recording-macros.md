# Módulo 6 · Recording Macros

> Los videos de esta semana, según los archivos en `material_curso/modulo-06/`, son: **V1 Recorded Macro · V2 Run Macro · V3 Edit Macro · V4 Work With Macros · V5 Relative Reference Macro**. Las secciones 7-9 son material real de la lectura "Week 6: Toolbox"; la sección 6 viene de lo que se trabajó paso a paso en el video 5 (descrito por el usuario). El resto se redactó con conocimiento general apoyado en los nombres de los videos, así que algún detalle del procedimiento podría no calzar al 100% con lo que muestra el curso.

**Idea central**: una **macro** es una secuencia de instrucciones que Excel puede repetir para automatizar una tarea. La **grabadora** (*Macro Recorder*) las escribe sola, mientras haces la tarea una vez, en un lenguaje llamado **VBA**; no hace falta saber programar para empezar, aunque saber leer el código sirve para corregirlo.

## 1. Preparación: pestaña Programador y seguridad (V1)

- **Pestaña Programador (Developer)**: no viene visible. Se activa en Archivo > Opciones > Personalizar cinta de opciones > marcar *Programador*. Ahí están *Grabar macro*, *Macros*, *Usar referencias relativas*, *Visual Basic* e *Insertar* (botones).
- **Guardar como libro habilitado para macros (`.xlsm`)**: un `.xlsx` **descarta** las macros al guardar. También son seguros el *Libro binario de Excel* (`.xlsb`) y la *Plantilla habilitada para macros* (`.xltm`).
- **Seguridad de macros** (Archivo > Opciones > Centro de confianza): por defecto Excel deshabilita las macros y avisa con una barra amarilla que ofrece **Habilitar contenido**. Las macros solo se activan solas si el archivo está en una **ubicación de confianza** (*Trusted Location*).

## 2. Grabar una macro (V1)

Programador > **Grabar macro** (o `Alt + T + M + R`, que también **detiene** la grabación). El cuadro pide:

| Campo | Qué es |
|---|---|
| **Nombre de la macro** | Debe empezar con letra y no puede tener espacios (se usa `_`) |
| **Tecla de método abreviado** | Un atajo `Ctrl + letra` para ejecutarla (ver el cuidado en la sección 8) |
| **Guardar macro en** | *Este libro* (viaja con el archivo), *Libro de macros personal* (disponible en todos tus libros) o *Nuevo libro* |
| **Descripción** | Texto libre para documentar qué hace |

Después, todo lo que haces se graba hasta que pulsas **Detener grabación**.

**Qué se graba y qué no**:
- Se graban las **acciones** (cada selección, escritura, formato, cambio de hoja), **no el tiempo**: las pausas no cuentan, y la macro corre a la velocidad del procesador. Su tamaño depende de cuántas acciones hay, no de cuánto te demoraste, y es de unos pocos KB.
- No se graba el scroll ni mover el mouse sin hacer clic.
- En cuadros de diálogo como *Formato de celdas*, la grabadora registra **todas** las propiedades del cuadro aunque solo cambies una (un cambio de fuente puede generar unas diez líneas).
- **El deshacer (`Ctrl + Z`) no se graba**: la acción deshecha **sí queda** en el código. Si cambias el tamaño a 20 y luego a 10, quedan dos líneas (una redundante); si pones 20 y lo deshaces, la macro dejará 20. Conviene revisar el código si se cometen errores durante la grabación.
- **Hay que acordarse de detener la grabación** al terminar: todo lo que se haga después también quedaría grabado.

**Buena práctica**: ensayar los pasos antes de grabar, grabar solo lo necesario y, si se cuelan pasos de más, quitarlos después en el código.

## 3. Ejecutar una macro (V2)

- **Cuadro Macro**: Programador > *Macros* o `Alt + F8` → seleccionar y **Ejecutar**. Desde ahí también se pueden *Editar*, *Eliminar* y ver *Opciones*.
- **Atajo asignado**: `Ctrl + letra`, el que se eligió al grabar (o en Alt + F8 > Opciones).
- **Botón o forma**: Programador > Insertar > *Botón (control de formulario)*, o cualquier forma o imagen (clic derecho > *Asignar macro*). Es la forma más amigable para otros usuarios.
- **Barra de acceso rápido**: se puede agregar un botón de macro.
- **Para ejecutarla paso a paso**, ver la sección 4 (editor).

Antes de ejecutar conviene recordar que **las macros no se pueden deshacer con `Ctrl + Z`** (la pila de deshacer se borra al correr una macro), así que lo recomendable es guardar el libro antes de probarla.

## 4. Editar una macro (V3)

Se abre el **Visual Basic Editor (VBE)** con `Alt + F11` (alterna entre la hoja y el código) o desde Programador > *Visual Basic* o Macros > *Editar*.

Partes de la ventana:
- **Project Explorer (explorador de proyectos)**: árbol con los libros abiertos y sus hojas y **módulos**.
- **Code window (ventana de código)**: el código VBA. Las macros grabadas se guardan en un **módulo** (*Module1*, ...).
- **Compiler (compilador)**: es lo que convierte el código escrito en VBA (lenguaje de alto nivel) al lenguaje que entiende el equipo.

Estructura de una macro grabada:

```vba
Sub NombreDeLaMacro()
'
' NombreDeLaMacro Macro
' Descripción escrita al grabar
'
    ActiveCell.FormulaR1C1 = "Alex McIntyre"
    Range("B5").Select
End Sub
```

- Toda macro empieza con **`Sub`** (de *subroutine*) y termina con **`End Sub`**.
- Las líneas que empiezan con **apóstrofe (`'`)** son **comentarios**: Excel los ignora al ejecutar y sirven para documentar.
- Lo habitual al editar: **quitar líneas sobrantes** (selecciones innecesarias, cambios que se deshicieron), **corregir errores** y **añadir funcionalidad** que la grabadora no ofrece.
- **Depurar (Debug)**: en el editor, **`F8`** ejecuta línea a línea, **`F5`** ejecuta la macro completa, y si hay un error Excel resalta en amarillo la línea con el problema.
- **Convertir en comentario en vez de borrar**: para descartar una línea con error, se le pone un apóstrofe al inicio; así queda visible por si hace falta revisarla.

## 5. Trabajar con macros (V4)

- **Varias macros, cortas y simples**, en vez de una sola larga: se prueban, corrigen y reutilizan mejor.
- **Libro de macros personal** (`Personal.xlsb`): se guarda en la carpeta de inicio de Excel y se abre oculto con Excel, así sus macros sirven para **cualquier libro**. Se crea sola la primera vez que se graba una macro guardada ahí.
- **Libro actual**: la macro viaja con el archivo; es lo adecuado si se va a compartir.
- **Eliminar una macro**: no se puede recuperar ni deshacer.
- **Documentar con comentarios** descripciones y cambios de cada macro.
- **Copiar macros entre libros**: en el editor, arrastrar un módulo entre proyectos del Project Explorer, o copiar y pegar el código.

## 6. Macros con referencias relativas (V5)

### Absoluta vs relativa

El botón **Usar referencias relativas** (Programador) cambia cómo se graban los movimientos. Hay que activarlo **antes** de grabar.

| | **Absoluta (por defecto)** | **Relativa** |
|---|---|---|
| Qué guarda | La celda exacta: `Range("B5").Select` | Un desplazamiento desde la celda activa: `ActiveCell.Offset(1, 0).Select` |
| Dónde actúa | **Siempre en las mismas celdas** | **A partir de la celda donde estás parado** |
| Cuándo sirve | Tareas fijas | Tareas que se repiten en distinto lugar (agregar al final de los registros) |

En una macro relativa, **la celda de partida es parte de la macro**: si se ejecuta desde otra celda, todo cae desplazado. Terminología del curso: una macro relativa es la que se hace cuando las tareas a automatizar *no son idénticas*; se registra una posición relativa (por ejemplo, "cada vez que copio, bajo un paso") en contraste con una celda fija como `B9`.

### El ejercicio del video: agregar un `.txt` al final de los registros

Los archivos del módulo incluyen `BPAY20170814.txt`, `BPAY20170815.txt` y `BPAY20170816.txt` (pagos BPAY, un archivo por día, con la fecha en el nombre) y un libro con las hojas `Invoice Data` y `BPay`. Los registros del `.txt` van separados por `;` (formato **delimitado**), con unas líneas de encabezado antes de los datos, y la fecha de pago viene como **número de serie** de Excel (por ejemplo `42961`), no como texto.

Pasos grabados (con *Usar referencias relativas* activado):
1. Clic en una celda de la **primera columna** de la hoja.
2. `Ctrl + Inicio` → va al principio de la hoja.
3. `Ctrl + Flecha abajo` → va al **último registro**.
4. **Flecha abajo** → va a la **primera celda libre**.
5. Datos > Obtener datos > **Asistentes heredados** (*Legacy Wizards*) > **Desde texto (heredado)** y se elige el archivo `.txt`, con su asistente de importación (delimitado, separador `;`).
6. **Detener grabación**.

### Correcciones inmediatas en el editor

| Cambio | Por qué |
|---|---|
| Comentar `'.CommandType = 0` | La grabadora lo escribe al importar texto y **da error** en las versiones modernas de Excel (`0` no es un valor válido). Esa propiedad solo indica si la consulta es SQL o una tabla; en un archivo de texto no hace falta, así que comentarla no cambia nada. Solo se comenta si da error |
| `Destination:=Range("$A$9")` → `Destination:=ActiveCell` | Los pasos de movimiento sí se graban relativos, pero **la grabadora escribe siempre el destino de `QueryTables.Add` como celda absoluta** (aquí, `$A$9`, donde se estaba al grabar). Sin el cambio, la macro llega a la primera celda libre pero luego escribe en `A9` y pisa los datos. Con `ActiveCell`, el texto se importa donde terminaron los pasos anteriores. Se hace siempre que el destino deba depender de dónde se está; si debe ser fijo, se deja la celda absoluta |
| Insertar un `InputBox` en el nombre del archivo | Para que la macro sirva para cualquier día y no solo el archivo grabado |

El `InputBox` se une al nombre con `&`:

```vba
With ActiveSheet.QueryTables.Add(Connection:= _
    "TEXT;C:\...\modulo-06\BPAY" & InputBox("Fecha del archivo", "Importar BPAY") & ".txt" _
    , Destination:=ActiveCell)
```

- `"...\BPAY"`, lo que escribe el usuario (por ejemplo `20170814`) y `".txt"` forman el nombre completo.
- Si el usuario pulsa **Cancelar**, el `InputBox` devuelve texto vacío y la macro busca `BPAY.txt`, que no existe → error.
- La ruta está fija en el código; si se mueve la carpeta, hay que editarla.

### Gotchas descubiertos

- **Si se prueba la macro varias veces y se borran los registros entre pruebas**, hay que **ponerse siempre en la misma celda de partida** (por ejemplo, con `Ctrl + Inicio`); si no, los datos caen desplazados (columnas "muy a la derecha"). Para no depender de eso, se puede empezar la macro con una celda fija (`Range("A1").Select`) y dejar solo lo que sigue como relativo.
- **El formato (fecha, alineación, ancho) puede venir de las celdas ya formateadas, no de la macro.** Si se **eliminan las filas** (clic derecho > Eliminar), el formato desaparece; si se borra solo el contenido (`Supr`), se conserva. Por eso una segunda ejecución puede salir sin formato.
- **La fecha del `.txt` llega como número de serie** (por ejemplo `42961`), y se ve como "General" hasta que se aplica un **formato de fecha**. Para que la macro lo haga sola hay que **grabar también** el formato (seleccionar las columnas nuevas, formato de fecha, alineación, autoajuste del ancho) o indicar el tipo de cada columna en el asistente de importación.
- En el código de la importación, `.TextFileColumnDataTypes` define el formato de cada columna al importar, `.AdjustColumnWidth` ajusta el ancho y `.PreserveFormatting` mantiene el formato de las celdas.
- **Cada importación crea una consulta nueva** con el nombre del archivo; ejecutar la macro con varios archivos acumula consultas en el libro.

## 7. Atajos de teclado (lectura "Week 6: Toolbox")

| Atajo | Acción |
|---|---|
| `Alt + F8` | Abre el cuadro de diálogo de macros |
| `Alt + F11` | Alterna entre la hoja de Excel y la ventana de código |
| `Alt + T + M + R` | **Graba** una macro; también **detiene** la grabación |
| `Ctrl + Inicio` | Va a la celda superior izquierda de la hoja |
| `Shift + F10` (o `Fn + Shift + F10`) | Muestra el menú contextual |

## 8. Terminología (lectura "Week 6: Toolbox")

- **Macro**: conjunto de instrucciones de programación que se pueden aplicar en Excel para automatizar tareas repetitivas.
- **Relative Macro (macro relativa)**: la que se escribe cuando las tareas a automatizar no son idénticas; se registra una posición relativa (por ejemplo, "cada vez que copio, bajo un paso"), en contraste con una celda fija como `B9`.
- **Store in a workbook (guardar en el libro)**: la macro viaja con el libro.
- **Store in Personal Macro workbook (guardar en el libro de macros personal)**: se guarda en la carpeta de inicio de Excel y se abre, oculto, cada vez que se abre Excel. Sirve para varios libros, no solo uno.
- **Trusted Location (ubicación de confianza)**: las macros de esa ubicación se habilitan automáticamente, sin tener que habilitarlas.
- **Visual Basic for Applications (VBA)**: código informático que usa Excel para registrar macros.
- **Visual Basic Editor (VBE)**: ventana para crear y editar macros con VBA.
- **Debug (depurar)**: identificar y eliminar errores del código VBA de una macro.
- **Macro Dialog Box (cuadro de diálogo de macros)**: ventana para crear, editar, eliminar o ejecutar macros.
- **Macro Recorder (grabadora de macros)**: al empezar a grabar, sigue las acciones en Excel y las registra en código VBA.
- **Macro-Enabled (habilitado para macros)**: libro con las macros guardadas de forma segura para uso futuro.
- **Project Explorer (explorador de proyectos)**: panel de navegación al editar una macro, para moverse entre las macros de distintos libros.
- **Code window (ventana de código)**: igual que el VBE; contiene el código VBA en que se basan las macros.
- **Compiler (compilador)**: convierte el lenguaje de alto nivel (el VBA que se escribe) en lenguaje de bajo nivel, que es el que entiende el equipo.
- **Module (módulo)**: unidad de almacenamiento de un conjunto de macros.
- **Sub**: abreviatura de *subroutine*. Todas las macros grabadas empiezan con `Sub` y terminan con `End Sub`.
- **Comment (comentario)**: texto descriptivo que Excel ignora al ejecutar; cada comentario empieza con un apóstrofe.
- **Data import (importación de datos)**: permite a Excel leer datos que pueden venir de un archivo de texto, por ejemplo los datos de BPay.
- **Delimited (delimitado)**: datos separados por caracteres de un tipo concreto (comas, punto y coma, etc.).

## 9. Ninja tips de la semana

- En vez de una macro larga, suele ser mejor usar **macros cortas y simples** que hagan la misma tarea.
- **Cuidado al asignar un atajo de teclado**: anula cualquier comando de Excel que use el mismo atajo. Lo mejor es usar **letras mayúsculas** (`Ctrl + Shift + letra`) para evitar el problema.
- Al **eliminar una macro**, Excel **no puede recuperarla**.
- Se pueden usar macros **sin saber VBA**, aunque se pueda aprender y escribirlas con código.
- Al grabar se intenta evitar errores para mejorar la eficiencia de la macro, pero también es vital **acordarse de detener la grabación** al terminar.
- Para que **no se pierda la macro al guardar**, guardar el archivo como **Libro de Excel habilitado para macros**. El *Libro binario de Excel* y la *Plantilla habilitada para macros* también son seguros.
- Usar **comentarios** en el código para documentar descripciones y actualizaciones; ayudan cuando el código se vuelve abrumador.
- En vez de borrar una línea con error, **convertirla en comentario** con un apóstrofe al inicio, para poder revisarla.

## 10. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Evitar `Select` en el código**: la grabadora llena el código de `Select` y `ActiveCell`. Un código más limpio actúa directamente: `Range("B5").Value = "Alex"` en vez de seleccionar y luego escribir. Es más rápido y no depende de dónde esté el cursor.
- **Acelerar una macro**: al inicio `Application.ScreenUpdating = False` (no redibuja la pantalla) y al final `= True`.
- **Mostrar mensajes**: `MsgBox "Listo"` avisa al usuario; `InputBox` le pide un dato (visto en la sección 6).
- **Variables**: guardan valores intermedios (`Dim fecha As String`), y con `fecha = InputBox(...)` se puede comprobar si está vacía antes de seguir (por ejemplo, para el caso de que el usuario pulse *Cancelar*).
- **Condicionales y bucles**: `If ... Then ... End If` y `For Each celda In Range(...)` permiten repetir una acción por muchas celdas, algo que la grabadora no puede hacer por sí sola.
- **Manejo de errores**: `On Error Resume Next` y `On Error GoTo etiqueta` evitan que la macro se detenga con un cuadro de error.
- **Bloques `With ... End With`**: la grabadora los usa para no repetir el objeto (`With Selection.Font ... End With`).
- **Botones**: un *control de formulario* ejecuta macros sin más; un *control ActiveX* tiene más propiedades y eventos, pero es más delicado.
- **Eventos**: macros que se ejecutan solas ante un suceso (`Workbook_Open`, `Worksheet_Change`); son código escrito a mano, no grabado.
- **Macros y seguridad**: nunca habilitar macros de archivos de origen desconocido; el código puede hacer cualquier cosa. Las firmas digitales y las ubicaciones de confianza son la forma de distribuirlas en una organización.
- **Power Query como alternativa a importar con macro**: Datos > Obtener datos > *Desde texto/CSV* hace la importación del `.txt` de forma actualizable y sin código. Las macros siguen siendo útiles cuando el proceso incluye pasos fuera de la importación.
- **Después del curso**: VBA permite crear funciones propias (`Function`), formularios (*UserForms*) y automatizar otras aplicaciones de Office.

---

### Práctica
Archivos del curso: `material_curso/modulo-06/`, con un libro por video (`W6_V1 RecordedMacro`, `W6_V2 RunMacro`, `W6_V3 EditMacro`, `W6_V4 WorkWithMacros`, `W6_V5 RelativeReferenceMacro`; los que traen macros son `.xlsm`) y los tres archivos de texto `BPAY2017081x.txt` del video 5. Hay un reto de práctica con solución en `challenge/` (una empresa de contabilidad: convertir una hoja en tabla, fórmulas de descuento y total, y una macro `Transfer` con referencias relativas que pasa los datos de una hoja de entrada a la tabla y se asigna a un botón), documentado paso a paso en [advanced-practice-challenge.md](advanced-practice-challenge.md). El assessment de la semana está documentado por habilidades en [assessment.md](assessment.md).

### Fuente
Lectura "Week 6: Toolbox" del curso (atajos, terminología y ninja tips — secciones 7, 8 y 9) y el ejercicio del video 5 descrito por el usuario (sección 6, incluidas las correcciones al código y las preguntas resueltas en conversación). Las secciones 1-5 se apoyan en los nombres de los videos, con conocimiento general para los procedimientos. Sección 10 es complemento propio.
