# Módulo 1 · Data Validation

> Los videos de esta semana, según los archivos en `material_curso/modulo-01/`, son: **V1 Data Validation · V2 Drop Down Lists · V3 Formulas In Data Validation · V4 Working With Validation · V5 Advanced Conditional Formatting**. Las secciones 6 y 7 son material real de la lectura "Week 1: Toolbox" (esta semana no trae ninja tip); las secciones 1-5 se redactaron con conocimiento general apoyado en los nombres de los videos y en lo que pide el assessment, así que algún detalle puntual podría no calzar 100% con lo que muestra el curso. Las preguntas del assessment están documentadas en [assessment.md](assessment.md).

## 1. Validación de datos (V1)

**Data Validation** reduce los errores en un libro limitando lo que Excel acepta en una celda a los resultados razonables. Se abre en **Datos > Validación de datos**; el cuadro tiene tres pestañas:

- **Configuración**: qué se permite (campo **Permitir**) y bajo qué criterio.
- **Mensaje de entrada**: un aviso que aparece al hacer clic en la celda, para orientar al usuario sobre qué valores son razonables (solo si se definió).
- **Mensaje de error**: el aviso que aparece cuando se escribe algo que no cumple la regla. Hay tres estilos: **Detener** (bloquea la entrada), **Advertencia** (permite continuar si el usuario insiste) e **Información** (solo avisa).

Tipos de dato que se pueden permitir:

| Permitir | Para qué |
|---|---|
| Cualquier valor | Sin restricción (es el valor por defecto) |
| Número entero | Solo enteros, con criterio (entre, mayor que, menor que...) |
| Decimal | Números con decimales, con los mismos criterios |
| Lista | Solo los valores de una lista (ver sección 2) |
| Fecha / Hora | Fechas u horas dentro de un criterio |
| Longitud del texto | Cantidad de caracteres permitida |
| Personalizada | Una fórmula que debe dar VERDADERO (ver sección 3) |

Elegir el tipo adecuado importa: para un porcentaje de recargo entre 0,1 y 0,5 corresponde **Decimal** (un entero no admitiría 0,25); para una fecha posterior a un límite, **Fecha / mayor que**.

> **La validación no es infalible.** Solo controla lo que se **escribe** en la celda: si el usuario **pega** un valor, la regla se pierde, y los datos que ya estaban antes de aplicar la regla **no se corrigen solos** (ver "Círculo de datos no válidos" en la sección 4).

## 2. Listas desplegables (V2)

Con **Permitir: Lista**, la celda muestra una flecha con las opciones permitidas. El **Origen** puede ser:

- **Valores escritos a mano** separados por coma: `NSW,QLD,VIC` (con `;` si tu Excel usa coma decimal). Sirve para listas cortas y fijas.
- **Un rango específico** (*Specified range*), por ejemplo `B4:B9`.
- **Un rango con nombre**, con el que la lista se refiere a un nombre y no a celdas concretas.

**Lista de búsqueda (Lookup List)**: la lista que se guarda en algún lugar del libro y a la que apunta el cuadro de Origen.

**Lista desplegable automática (Automated drop-down List)**: se forma apuntando a una lista de búsqueda que está dentro de un **rango con nombre y una tabla** (Módulos 3 y 5 de Intermediate I): cuando se agrega o se cambia un elemento de la tabla, la lista desplegable se actualiza sola, sin tocar la regla.

## 3. Fórmulas en la validación de datos (V3)

Con **Permitir: Personalizada** se escribe una fórmula que debe dar **VERDADERO** para que el valor se acepte. Algunas ideas:

- **Evitar duplicados** en una columna: `=COUNTIF($B$2:$B$61,B2)=1` (cuenta cuántas veces aparece el valor; solo se acepta si es 1).
- Los criterios de los otros tipos también pueden usar **referencias de celda** en vez de números fijos (ej. un mínimo y un máximo escritos en celdas), así la regla cambia al editar esas celdas.
- La fórmula se escribe **pensando en la primera celda de la selección**: Excel la ajusta para las demás, así que hay que usar referencias **absolutas** (`$B$2:$B$61`) para el rango y **relativas** (`B2`) para la celda que se evalúa.

## 4. Trabajar con la validación (V4)

- **Encontrar qué celdas tienen validación**: Inicio > Buscar y seleccionar > **Validación de datos** (o `F5` > Especial > Validación de datos > *Todas*; la opción *Mismas* solo selecciona las que tienen la misma regla que la celda activa).
- **Copiar solo la validación** a otra celda o columna: copiar la celda, y en el destino **Pegado especial > Validación** (el atajo `Ctrl + Alt + V` abre el cuadro). Es lo que permite reutilizar una regla sin copiar el contenido.
- **Quitar la validación** de una celda: Datos > Validación de datos > **Borrar todos**.
- **Círculo de datos no válidos**: Datos > Validación de datos (flecha) > **Rodear con un círculo datos no válidos**. Marca con un círculo rojo las celdas que ya contienen valores que incumplen la regla — imprescindible cuando la regla se aplica **después** de que los datos ya estaban escritos. **Borrar círculos de validación** los quita.
- **Cambiar una regla ya aplicada**: el cuadro ofrece la casilla "Aplicar estos cambios a otras celdas con la misma configuración", útil para actualizar todas las celdas que comparten una regla de una sola vez.

## 5. Formato condicional avanzado (V5)

En Essentials (Módulo 4) se vieron las reglas prediseñadas. Aquí se pasa a **reglas personalizadas con fórmula**: Inicio > Formato condicional > Nueva regla > **Usar una fórmula que determine las celdas para aplicar formato**.

- La fórmula debe dar VERDADERO/FALSO y se escribe pensando en la **celda activa** (la primera del rango); las **referencias relativas y absolutas** controlan a qué celdas se aplica en el resto (`$` en la columna para formatear toda una fila según una sola columna).
- La regla puede **comparar contra otra celda**, incluso una con lista desplegable, de modo que el resaltado cambia al elegir otro valor en la lista: es la combinación de validación + formato condicional (por ejemplo, resaltar los trabajos con más personas que el número elegido en una celda).
- **Administrar reglas** permite editar, reordenar o eliminar reglas y definir su prioridad cuando varias aplican a la misma celda.

## 6. Atajos de teclado

| Atajo | Acción |
|---|---|
| `F3` | Pegar nombre: lista los rangos con nombre disponibles |
| `F4` | Alternar entre referencia absoluta y relativa |
| `Ctrl + T` | Insertar tabla |
| `Ctrl + F3` | Administrador de nombres (solo Windows): crear, modificar y eliminar rangos con nombre |
| `Cmd + F3` o `Ctrl + L` | Definir nombre (solo Mac); la herramienta existe también en Windows y sirve para crear rangos con nombre |
| `Ctrl + Shift + F3` (Mac: `Cmd + Shift + F3`) | Crear nombres desde la selección: se selecciona el rango **incluyendo encabezados** y se elige de dónde salen los nombres (fila superior, columna izquierda, fila inferior o columna derecha) |

> Ojo: en las notas de Intermediate I (Módulo 3) el atajo de Mac para Definir nombre figura como `Cmd + fn + F3`; esta lectura indica `Cmd + F3` o `Ctrl + L`. Probablemente depende de si el teclado usa las teclas F como estándar o con `fn`.

## 7. Terminología

- **Data Validation (validación de datos)**: método para reducir los errores que se ingresan en el libro, limitando los datos que Excel acepta a los resultados más razonables.
- **Data validation dialog box (cuadro de diálogo de validación)**: el cuadro que aparece al abrir la herramienta.
- **Drop-down List (lista desplegable)**: la lista que aparece junto a cada celda con validación de tipo Lista; muestra una lista predefinida que hay que especificar.
- **Named range (rango con nombre)**: una celda o rango de celdas con un nombre útil.
- **Lookup List (lista de búsqueda)**: la lista que se coloca en algún lugar del libro y a la que se hace referencia en el Origen del cuadro de validación.
- **Automated drop-down List (lista desplegable automática)**: lista que se forma apuntando a una lista de búsqueda (en un rango con nombre y una tabla); cambia sola cuando cambia la lista de búsqueda.
- **Paste Special (pegado especial)**: por defecto, al copiar y pegar una celda se copian su contenido y sus características especiales. Con Pegado especial se pega solo el contenido o solo algunas características; por ejemplo, la validación de datos de una celda a otra.
- **Input Message (mensaje de entrada)**: el mensaje que aparece al hacer clic en una celda con validación, para orientar sobre qué valores son razonables. Solo aparece si se definió.
- **Error Message (mensaje de error)**: el mensaje emergente cuando se escribe en la celda algo fuera de lo especificado en la validación.
- **Specified range (rango especificado)**: un rango dado por referencias de celda, por ejemplo `B4:B9`.
- **Conditional formatting (formato condicional)**: forma de dar formato a una celda o rango según si cumplen un criterio.

## 8. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Listas dependientes**: una segunda lista cuyo contenido depende de lo elegido en la primera (ej. país → ciudad). Se logra con rangos con nombre y la función `INDIRECT` en el Origen.
- **Límites de una lista escrita a mano**: el Origen escrito directamente en el cuadro admite hasta unos 255 caracteres; para listas largas conviene un rango o un rango con nombre.
- **Listas en otra hoja**: en versiones recientes de Excel el Origen puede apuntar directamente a un rango de otra hoja; en versiones antiguas hacía falta un rango con nombre.
- **Resaltar duplicados sin bloquearlos**: Formato condicional > Resaltar reglas de celdas > **Valores duplicados**, una alternativa a la validación personalizada cuando solo se quiere avisar. Para eliminarlos de verdad existe Datos > **Quitar duplicados**.
- **Validación y datos importados**: al importar o pegar datos la validación no se aplica, así que conviene revisar con **Rodear con un círculo datos no válidos** después de cargar una base nueva.
- **Longitud del texto y personalizada combinadas**: con `AND(...)` o `OR(...)` (que se ven en el Módulo 2) se pueden armar reglas de varias condiciones a la vez.

---

### Práctica
Archivos del curso: `material_curso/modulo-01/`. Ningún video trae versión `Soln`. Hay un reto de práctica con solución en `challenge/` y el assessment de la semana en `assessment/` (documentado en [assessment.md](assessment.md)).

### Fuente
Lectura "Week 1: Toolbox" del curso (atajos y terminología — secciones 6 y 7; no trae ninja tip). Secciones 1-5 se basan en los nombres de los videos y en las preguntas del assessment, redactadas con conocimiento general. Sección 8 es complemento propio.
