# Módulo 4 · Formula Auditing and Protection

> Los videos de esta semana, según los archivos en `material_curso/modulo-04/`, son: **V1 Errors · V2 Calculation Options · V3 Trace Precedents · V4 Evaluate Formula · V5 Protection**. Las secciones 6-8 son material real de la lectura "Week 4: Toolbox", que además aporta la terminología base de las secciones 1-5; el resto de esas secciones se redactó con conocimiento general apoyado en los nombres de los videos, así que algún detalle puntual podría no calzar 100% con lo que muestra el curso.

## 1. Errores en las fórmulas (V1)

**Formula Auditing (auditoría de fórmulas)** es el grupo de la pestaña **Fórmulas** que reúne las herramientas para identificar y corregir errores en las fórmulas. La **verificación de errores** (Fórmulas > Comprobación de errores) recorre la hoja y marca las celdas con problemas; las celdas con posibles errores también muestran un **triángulo verde** en su esquina.

Los errores más comunes:

| Código | Significado |
|---|---|
| `#DIV/0!` | Se dividió entre 0 (cero) o entre una celda vacía |
| `#NAME?` | La fórmula contiene un texto que Excel no reconoce (por ejemplo, el nombre de una función mal escrito) |
| `#VALUE!` | Un valor de la fórmula es del tipo de dato equivocado |
| `#####` | **No es realmente un error**: la columna no es lo bastante ancha para mostrar el número. La celda sigue sirviendo en los cálculos; basta con ensanchar la columna |

También aparecen `#N/A` (una función de búsqueda no encontró el valor, visto en el Módulo 3) y `#REF!` (la fórmula apunta a una celda que fue borrada).

**Referencia circular (Circular Reference)**: un error que ocurre cuando una fórmula **se refiere a la celda que la contiene**. Por ejemplo, escribir `=A1+A2` en la celda `A1`. Excel avisa al abrir un libro que tenga una o más referencias circulares, y también lo muestra en la pestaña Fórmulas (en Comprobación de errores) y en la **barra de estado**.

## 2. Opciones de cálculo (V2)

Fórmulas > **Opciones para el cálculo** permite elegir cuándo recalcula Excel:

- **Automático**: recalcula al cambiar cualquier dato (el valor por defecto).
- **Automático excepto para tablas de datos**.
- **Manual**: solo recalcula cuando se le pide; útil con libros muy pesados.

Para recalcular a mano (ver atajos en la sección 6): `F9` recalcula las fórmulas de **todos los libros abiertos**; `Shift + F9`, solo la **hoja activa**.

**Conceptos para entender los cálculos**:
- **Valor mostrado (Displayed Value)**: el valor de una celda que ve el usuario.
- **Valor almacenado (Stored Value)**: el valor real que Excel tiene asignado a la celda.
- **Precisión (Precision)**: el nivel de exactitud de un cálculo. El valor almacenado tiene la **máxima precisión**; la precisión del valor mostrado se puede cambiar al nivel que se desee (por ejemplo, con el formato de número).
- **Iteración (Iteration)**: una repetición de un cálculo por parte de Excel. Entender las iteraciones es importante para resolver las referencias circulares: con el **cálculo iterativo** activado (Archivo > Opciones > Fórmulas), Excel repite el cálculo un número de veces en vez de marcar un error.

## 3. Rastrear precedentes y dependientes (V3)

- **Celda precedente (Precedent Cell)**: una celda a la que se refiere la fórmula de la celda activa. Ejemplo: si en `D5` la fórmula es `=A1` y `D5` está seleccionada, `A1` es precedente de `D5`.
- **Celda dependiente (Dependent Cell)**: una celda cuya fórmula se refiere a la celda activa. Con el mismo ejemplo, si se selecciona `A1`, `D5` es dependiente de `A1`.

En la pestaña Fórmulas, grupo Auditoría de fórmulas:
- **Rastrear precedentes** y **Rastrear dependientes** dibujan flechas desde o hacia la celda activa.
- **Quitar flechas** las borra.
- **Mostrar fórmulas** muestra las fórmulas en vez de sus resultados.
- **Ventana de inspección (Watch Window)**: barra de herramientas que permite vigilar celdas y sus fórmulas mientras uno se mueve por el libro.

Con el teclado, `Ctrl + [` va a las celdas precedentes y `Ctrl + ]` a las dependientes (sección 6).

## 4. Evaluar fórmula (V4)

Fórmulas > **Evaluar fórmula** abre un cuadro que recorre una fórmula **paso a paso**, mostrando qué calcula cada parte y en qué orden (de la función más interna hacia afuera, con las funciones anidadas). Sirve para entender o depurar fórmulas largas.

**Limitaciones**: las funciones `RAND`, `AREAS`, `INDEX`, `OFFSET`, `CELL`, `INDIRECT`, `ROWS`, `COLUMNS`, `NOW`, `TODAY` y `RANDBETWEEN` se **recalculan cada vez que cambia la hoja**, y por eso pueden hacer que Evaluar fórmula dé resultados distintos de los que aparecen en la celda (según el soporte de Microsoft Excel).

## 5. Protección (V5)

**Protección** es una función de privacidad y seguridad que permite **restringir la modificación o el acceso** a un archivo de Excel o a una hoja concreta.

- **Proteger hoja** (Revisar > Proteger hoja): impide modificar las celdas **bloqueadas**. Por defecto todas las celdas están bloqueadas, pero el bloqueo solo surte efecto cuando la hoja está protegida; por eso, antes de proteger se **desbloquean** las celdas que el usuario sí debe poder editar (clic derecho > Formato de celdas > pestaña **Proteger** > desmarcar *Bloqueada*).
- **Proteger libro** (Revisar > Proteger libro): impide añadir, eliminar, mover o renombrar hojas.
- **Contraseña**: es opcional; ver el ninja tip de la sección 8.

## 6. Atajos de teclado

| Atajo | Acción |
|---|---|
| `F2` | Muestra la fórmula en modo de edición y los **precedentes de cada argumento codificados por colores** |
| `F9` | Recalcula las fórmulas de **todos los libros abiertos** |
| `Shift + F9` | Recalcula las fórmulas solo de la **hoja activa** |
| `Ctrl + [` | Ir a las celdas **precedentes** de la celda activa |
| `Ctrl + ]` | Ir a las celdas **dependientes** de la celda activa |

## 7. Terminología

- **Circular References (referencias circulares)**: error de fórmula que ocurre cuando una fórmula se refiere a la celda que la contiene (ej. `=A1+A2` escrito en `A1`).
- **Watch Window (ventana de inspección)**: barra de herramientas que permite vigilar celdas y sus fórmulas mientras uno navega por el libro.
- **Iteration (iteración)**: repetición de un cálculo por parte de Excel; importante para resolver referencias circulares.
- **Displayed Value (valor mostrado)**: el valor de una celda que ve el usuario.
- **Stored Value (valor almacenado)**: el valor real que Excel reconoce como asignado a una celda.
- **Precision (precisión)**: el nivel de exactitud de un cálculo; el valor almacenado tiene la máxima y la del valor mostrado se puede cambiar.
- **Formula Auditing (auditoría de fórmulas)**: grupo de la pestaña Fórmulas con herramientas para identificar y corregir errores en las fórmulas.
- **Protection (protección)**: función de privacidad y seguridad para restringir la modificación o el acceso a un archivo o una hoja.
- **Active Cell (celda activa)**: la celda seleccionada en ese momento; el texto aparece en ella al empezar a escribir. Solo puede haber una a la vez y se rodea de un borde grueso.
- **Precedent Cell (celda precedente)** y **Dependent Cell (celda dependiente)**: ver sección 3.
- **Cell Errors (errores de celda)**: `#DIV/0!`, `#NAME?`, `#VALUE!` y `#####`; ver sección 1.

## 8. Ninja tips de la semana

### Rastrear precedentes y dependientes
- **Doble clic en cualquier flecha** para seleccionar la celda del otro extremo de la flecha.
- Cuando se hace **cualquier cambio** (modificar una fórmula, insertar celdas…), **las flechas de rastreo desaparecen**. Si se quiere conservar el rastreo original, conviene trabajar sobre una copia del libro o de la hoja.

### Revisar también la ortografía
La comprobación de errores mantiene la atención en los errores de fórmulas, pero en algunos casos es igual de importante vigilar los **errores de ortografía**, que se corrigen con el corrector ortográfico de la pestaña **Revisar**.

### Contraseñas de protección
Conviene pensar si una **contraseña** es realmente necesaria. Si se usa, hay que **anotarla y guardarla en un lugar seguro**: Excel **no puede recuperarla** si se olvida.

### Limitaciones de Evaluar fórmula
Ver la sección 4: las funciones volátiles (`RAND`, `INDEX`, `OFFSET`, `INDIRECT`, `NOW`, `TODAY`, `RANDBETWEEN`…) pueden dar resultados distintos de los de la celda.

## 9. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Otros errores**: `#REF!` (referencia borrada), `#N/A` (valor no encontrado), `#NUM!` (número no válido para la operación) y `#NULL!` (intersección de rangos inexistente). `IFERROR` e `IFNA` (Módulo 2) permiten sustituirlos por un mensaje.
- **Vigilar celdas con la Ventana de inspección**: Fórmulas > Ventana de inspección > Agregar inspección. La ventana queda siempre visible, útil para ver cómo cambia una celda clave al editar datos en otra hoja.
- **Ir a Especial** (`F5` > Especial): selecciona de golpe todas las celdas con **fórmulas**, con **constantes**, con **errores** o con **validación**; muy útil al auditar una hoja ajena.
- **Ocultar las fórmulas** de una hoja protegida: Formato de celdas > Proteger > marcar *Oculta*, y proteger la hoja; la fórmula deja de verse en la barra de fórmulas.
- **Proteger no es lo mismo que cifrar**: la contraseña de una **hoja** se puede quitar con herramientas externas; para impedir que alguien **abra** el archivo hay que usar Archivo > Información > Proteger libro > Cifrar con contraseña.
- **Permitir editar rangos concretos** en una hoja protegida (Revisar > Permitir que los usuarios modifiquen rangos), y elegir qué acciones se permiten (dar formato, ordenar, filtrar).
- **`F9` dentro de la fórmula**: en la barra de fórmulas, seleccionar una parte y pulsar `F9` muestra su resultado (con `Esc` se cancela); una alternativa rápida a Evaluar fórmula.

---

### Práctica
Archivos del curso: `material_curso/modulo-04/`. Ningún video trae versión `Soln`. Hay un reto de práctica con solución en `challenge/`, y el assessment de la semana en `assessment/` (la carpeta tiene dos copias del mismo archivo, una con un prefijo de identificador).

### Fuente
Lectura "Week 4: Toolbox" del curso (atajos, terminología, tabla de errores, ninja tips — secciones 1-8 en lo que viene de ahí). Las secciones 1-5 se apoyan además en los nombres de los videos, con conocimiento general. Sección 9 es complemento propio.
