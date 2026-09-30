# Módulo 1 · Multiple Worksheets & Workbooks

> Los videos de esta semana, según los archivos en `material_curso/modulo-01/`, son: **V1 Multiple Worksheets · V2 3D Formulas · V3 Linking Workbooks · V4 Consolidating by Position · V5 Consolidating by Reference**. Los videos 3, 4 y 5 traen su propia subcarpeta porque el ejercicio necesita varios libros distintos vinculados entre sí. Las secciones 1, 3, 4 y 5 están redactadas con conocimiento general (el nombre de archivo confirma el tema, no el contenido exacto del video); la sección 2 usa la terminología real de la lectura "Week 1: Toolbox". El "Dialogue" de introducción a la semana confirma los mismos 3 ejes: fórmulas 3D, comportamiento/límites de la vinculación entre libros, y la herramienta Consolidar.

## 1. Trabajar con múltiples hojas (V1)

- Seleccionar varias pestañas a la vez (clic en la primera, `Shift+clic` en la última para un rango contiguo, o `Ctrl+clic` para pestañas sueltas) activa el **modo Grupo**: lo que escribes o formateas se aplica a todas las hojas seleccionadas a la vez. Útil para crear varias hojas idénticas (ej. una por mes) de una sola pasada.
- Para salir del modo Grupo: clic derecho sobre cualquier pestaña > **Desagrupar hojas**, o simplemente hacer clic en una pestaña que no esté seleccionada.
- Otras acciones típicas sobre pestañas: clic derecho > Insertar, Eliminar, Cambiar nombre, Mover o copiar, Color de etiqueta, Ocultar.

## 2. Fórmulas 3D (V2)

Las referencias que hemos usado hasta ahora son referencias **2D**: tienen 2 dimensiones (columna y fila) e identifican una celda dentro de **una sola hoja** — una celda (`A3`), un rango en la misma fila/columna (`A3:D3`), o un rango que abarca varias filas y columnas (`A3:D6`).

Si esa referencia se extiende para incluir **varias hojas**, se obtiene una referencia **3D** (una dimensión extra para identificar la celda). Puede ser una sola celda en varias hojas (`Sean:Carlos!C8`) o un rango de celdas en varias hojas (`Sean:Carlos!C8:E13`).

En la práctica, una fórmula 3D suma (u otra operación) el mismo rango a través de varias hojas consecutivas: `=SUM(Enero:Diciembre!B5)` suma la celda `B5` de todas las hojas desde "Enero" hasta "Diciembre" (en el orden en que aparecen las pestañas, no alfabético).

## 3. Vinculación entre libros (V3)

- Una fórmula puede referenciar una celda de **otro libro** (archivo) abierto: `='[NombreLibro.xlsx]NombreHoja'!B5`. El nombre del libro va entre corchetes.
- Si el libro de origen está **cerrado**, la referencia incluye la ruta completa: `='C:\Ruta\[NombreLibro.xlsx]NombreHoja'!B5`.
- **Comportamiento y límites** (lo que el "Dialogue" de esta semana llama "Linking Behavior and Limitations"):
  - El valor mostrado es el de la **última vez que se actualizó el enlace** — si el libro de origen cambia y no está abierto, hay que actualizar manualmente con Datos > **Editar vínculos** > Actualizar valores.
  - Al abrir un libro con vínculos externos, Excel suele mostrar una advertencia de seguridad preguntando si se deben actualizar los vínculos.
  - Si el archivo de origen se **mueve, renombra o borra**, el vínculo se rompe y la fórmula muestra un error (o sigue con el último valor guardado hasta que se intente actualizar).
  - Copiar/mover la hoja con la fórmula puede alterar la ruta relativa del vínculo — conviene revisar los vínculos con Datos > Editar vínculos después de reorganizar archivos.

## 4. Consolidar por posición (V4)

Se usa cuando varios libros/hojas tienen **exactamente la misma estructura** (mismas filas y columnas, en el mismo orden) — por ejemplo, una hoja de gastos idéntica para Melbourne, Perth y Sydney. La herramienta **Consolidar** (pestaña Datos) suma (o aplica otra función: promedio, contar, máximo, etc.) los valores que están en la **misma posición** de cada rango de origen, sin importar las etiquetas de texto.

Pasos generales: Datos > Consolidar > elegir la función > agregar cada rango de origen (de distintas hojas/libros) > Aceptar.

## 5. Consolidar por referencia / categoría (V5)

Se usa cuando los datos de origen **no** están perfectamente alineados en la misma posición — por ejemplo, cada ciudad registra los mismos conceptos de licencias pero en distinto orden de filas. En vez de consolidar por posición, se marcan las casillas **"Usar etiquetas en"** (fila superior y/o columna izquierda) en el cuadro de diálogo Consolidar, y Excel empareja los datos por su **etiqueta de texto** (el nombre de la categoría), no por su posición física.

También existe la opción **"Crear vínculos con los datos de origen"** dentro del mismo cuadro: si se marca, el resultado consolidado queda vinculado (se actualiza si cambian los datos de origen) en vez de quedar como valores fijos.

## 6. Guía de referencia: "Usar etiquetas en" — Ninguna / Columna izquierda / Fila superior / Ambas

Cuál(es) casilla(s) marcar depende de en qué eje(s) tus hojas de origen **no están alineadas**.

### Ninguna marcada — Consolidar por Posición
Las hojas tienen *exactamente* la misma estructura, fila por fila:

```
Melbourne          Perth               Sydney
Rent      1200     Rent      1400      Rent      1350
Utilities  300     Utilities  280      Utilities  310
Salaries  5000     Salaries  5200      Salaries  4900
```

Como la fila 1 siempre es "Rent", la 2 siempre "Utilities", etc., no hace falta leer ninguna etiqueta: Excel suma la celda en la posición (1,1) de las 3 hojas, luego la (2,1), luego la (3,1). Si una hoja tuviera las filas en otro orden, el resultado saldría mal sin que Excel se diera cuenta — por eso Consolidar por Posición exige estructura idéntica.

### Solo Columna izquierda
Las categorías (filas) cambian de orden entre hojas, pero solo hay una columna de valores:

```
May Wk4              June Wk1             June Wk2
High      12         Low        9         Medium    15
Low        8         Medium    14         High       7
Medium    10         High       6         Low       11
```

Aunque "High" está en la fila 1, 3 y 2 según la hoja, Consolidar con **Columna izquierda** marcada los reconoce por el texto y los suma correctamente: High = 12+6+7, Low = 8+9+11, Medium = 10+14+15. Este es el caso de los Steps 7-9 del [reto práctico avanzado](advanced-practice-challenge.md).

### Solo Fila superior
Al revés del caso anterior: las categorías (filas) siempre están en el mismo orden, pero los **encabezados de columna** cambian de orden entre hojas — por ejemplo, 3 hojas trimestrales donde las filas (Rent/Utilities/Salaries) están fijas, pero los meses no:

```
Hoja 1                      Hoja 2                      Hoja 3
        Jan   Feb   Mar             Mar   Jan   Feb             Feb   Mar   Jan
Rent    1200  1250  1300     Rent   1300  1200  1250     Rent   1250  1300  1200
```

Aquí se marca **Fila superior**: Excel empareja por el texto del encabezado ("Jan" con "Jan" sin importar en qué columna esté), no por posición.

### Ambas marcadas
Se combinan los dos problemas a la vez: ni las filas ni las columnas están en el mismo orden entre hojas — por ejemplo, 3 sucursales que registran gasto × mes, cada una con las categorías en un orden distinto **y** los meses en un orden distinto:

```
Melbourne                        Perth                           Sydney
        Feb   Jan   Mar                  Jan   Mar   Feb                  Mar   Feb   Jan
Salaries 5000 4900  5100         Rent    1400  1500  1350         Utilities 310  300  290
Rent     1200 1150  1250         Salaries 5200 5300  5100         Rent      1400 1350 1300
Utilities 300  280   310         Utilities 280  290   270         Salaries  4900 4950 4800
```

Con **ambas** marcadas, Excel arma la tabla final emparejando cada celda por su categoría de fila **y** su mes de columna, sin importar dónde esté físicamente en cada hoja de origen. Es el caso más completo — y el único donde de verdad se necesitan las dos casillas a la vez.

### ¿Incluir o no la fila de encabezados en la selección?

Un error fácil de cometer: si al seleccionar el rango de referencia incluyes la fila de encabezados (ej. seleccionas desde la celda que dice "Priority" / "Days Open", no solo los datos), Excel la trata como una fila de datos más a menos que marques **Fila superior** — lo cual ensucia el resultado (aparece una categoría extra con el texto del encabezado). Hay dos formas válidas de resolverlo:

- **Excluir los encabezados de la selección** (como en los Steps 7 y 9): seleccionas solo las filas de datos, sin el texto de arriba. Con **Columna izquierda** marcada alcanza; el resultado sale sin encabezado de columna (se puede agregar a mano después si se quiere).
- **Incluir los encabezados y marcar ambas casillas**: Columna izquierda + Fila superior. Ventaja: el resultado consolidado trae automáticamente el encabezado (ej. "Days Open") en vez de una celda vacía.

Para mantener la misma lógica en los 3 pasos del reto (7, 8 y 9), conviene quedarse con la primera opción: excluir encabezados, marcar solo Columna izquierda.

## 7. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + Av Pág` (Mac: `Option + →`) | Ir a la siguiente hoja del libro |
| `Ctrl + Re Pág` (Mac: `Option + ←`) | Ir a la hoja anterior del libro |

> En versiones recientes de Excel para Mac, los atajos con `Ctrl+` también funcionan.

## 8. Terminología

- **Referencia 3D (3-D Reference)**: una referencia 2D (columna + fila) extendida a varias hojas — una dimensión extra para identificar la celda. Puede ser una celda (`Sean:Carlos!C8`) o un rango (`Sean:Carlos!C8:E13`) repetido en varias hojas.
- **Estructura (Structure)**: la forma en que los datos están organizados dentro del libro — cuántas filas y columnas tiene, y el orden en que aparecen.
- **Libro (Workbook)**: el archivo de Excel completo; contiene una o más hojas.
- **Hoja (Worksheet)**: donde viven los datos y gráficos. Un libro debe tener al menos una hoja; el límite superior de hojas solo depende de los recursos del computador.

## 9. Ninja tip: el cuadro de diálogo "Activar"

Cuando un libro tiene muchas hojas, moverse entre ellas con las flechas de navegación puede ser lento. Clic derecho sobre las flechas de navegación de las pestañas (a la izquierda de las pestañas) abre el cuadro de diálogo **Activar**, con la lista completa de hojas del libro — se elige la deseada y se hace clic en Aceptar. Con pocas hojas no ahorra mucho tiempo, pero es muy útil cuando hay muchas.

## 10. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Fórmulas 3D con funciones distintas a SUM**: también funcionan con `AVERAGE`, `MAX`, `MIN`, `COUNT`, etc. — la sintaxis `Hoja1:HojaN!Rango` es la misma dentro de cualquiera de estas funciones.
- **Insertar/eliminar una hoja dentro del rango de una fórmula 3D**: si insertas una hoja nueva *entre* las hojas que delimitan una fórmula 3D (ej. entre "Enero" y "Diciembre"), Excel la incluye automáticamente en el cálculo. Si eliminas una de las hojas límite, el rango de la referencia se ajusta al siguiente límite disponible.
- **Riesgo de "romper" vínculos al compartir un archivo**: si envías por correo el libro que tiene fórmulas vinculadas a otro libro, quien lo reciba no tendrá el archivo de origen — verá el último valor guardado, pero no podrá actualizarlo. Antes de compartir, suele convenir "aplanar" esos valores (copiar > pegado especial > solo valores).
- **Consolidar vs. fórmulas 3D**: las fórmulas 3D exigen que las hojas estén en el **mismo libro**; la herramienta Consolidar puede combinar datos de **libros distintos** (incluso cerrados) y no requiere que las hojas estén contiguas ni en el mismo orden.

---

### Práctica
Archivos del curso: `material_curso/modulo-01/` (resolver los que no terminan en `Soln`). Los videos 3, 4 y 5 tienen su propia subcarpeta con los libros adicionales que hay que enlazar/consolidar.

Reto adicional resuelto y documentado aparte: [advanced-practice-challenge.md](advanced-practice-challenge.md) — combina 3D, vinculación y Consolidar por categoría en un solo ejercicio, con los "gotchas" reales del cuadro de diálogo Consolidar.

### Fuente
Lectura "Week 1: Toolbox" del curso (atajos, terminología de 3-D Reference/Structure/Workbook/Worksheet, ninja tip del cuadro Activar — secciones 2, 7, 8 y 9). Introducción "Dialogue" de la semana (confirma los 3 ejes temáticos). Secciones 1, 3, 4 y 5 se basan en los nombres de los videos y sus subcarpetas, redactadas con conocimiento general. Sección 6 son ejemplos elaborados en conversación para aclarar el cuadro Consolidar. Sección 10 es complemento propio.
