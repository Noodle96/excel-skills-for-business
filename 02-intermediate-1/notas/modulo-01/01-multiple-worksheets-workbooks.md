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

## 6. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + Av Pág` (Mac: `Option + →`) | Ir a la siguiente hoja del libro |
| `Ctrl + Re Pág` (Mac: `Option + ←`) | Ir a la hoja anterior del libro |

> En versiones recientes de Excel para Mac, los atajos con `Ctrl+` también funcionan.

## 7. Terminología

- **Referencia 3D (3-D Reference)**: una referencia 2D (columna + fila) extendida a varias hojas — una dimensión extra para identificar la celda. Puede ser una celda (`Sean:Carlos!C8`) o un rango (`Sean:Carlos!C8:E13`) repetido en varias hojas.
- **Estructura (Structure)**: la forma en que los datos están organizados dentro del libro — cuántas filas y columnas tiene, y el orden en que aparecen.
- **Libro (Workbook)**: el archivo de Excel completo; contiene una o más hojas.
- **Hoja (Worksheet)**: donde viven los datos y gráficos. Un libro debe tener al menos una hoja; el límite superior de hojas solo depende de los recursos del computador.

## 8. Ninja tip: el cuadro de diálogo "Activar"

Cuando un libro tiene muchas hojas, moverse entre ellas con las flechas de navegación puede ser lento. Clic derecho sobre las flechas de navegación de las pestañas (a la izquierda de las pestañas) abre el cuadro de diálogo **Activar**, con la lista completa de hojas del libro — se elige la deseada y se hace clic en Aceptar. Con pocas hojas no ahorra mucho tiempo, pero es muy útil cuando hay muchas.

## 9. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Fórmulas 3D con funciones distintas a SUM**: también funcionan con `AVERAGE`, `MAX`, `MIN`, `COUNT`, etc. — la sintaxis `Hoja1:HojaN!Rango` es la misma dentro de cualquiera de estas funciones.
- **Insertar/eliminar una hoja dentro del rango de una fórmula 3D**: si insertas una hoja nueva *entre* las hojas que delimitan una fórmula 3D (ej. entre "Enero" y "Diciembre"), Excel la incluye automáticamente en el cálculo. Si eliminas una de las hojas límite, el rango de la referencia se ajusta al siguiente límite disponible.
- **Riesgo de "romper" vínculos al compartir un archivo**: si envías por correo el libro que tiene fórmulas vinculadas a otro libro, quien lo reciba no tendrá el archivo de origen — verá el último valor guardado, pero no podrá actualizarlo. Antes de compartir, suele convenir "aplanar" esos valores (copiar > pegado especial > solo valores).
- **Consolidar vs. fórmulas 3D**: las fórmulas 3D exigen que las hojas estén en el **mismo libro**; la herramienta Consolidar puede combinar datos de **libros distintos** (incluso cerrados) y no requiere que las hojas estén contiguas ni en el mismo orden.

---

### Práctica
Archivos del curso: `material_curso/modulo-01/` (resolver los que no terminan en `Soln`). Los videos 3, 4 y 5 tienen su propia subcarpeta con los libros adicionales que hay que enlazar/consolidar.

### Fuente
Lectura "Week 1: Toolbox" del curso (atajos, terminología de 3-D Reference/Structure/Workbook/Worksheet, ninja tip del cuadro Activar — secciones 2, 6, 7 y 8). Introducción "Dialogue" de la semana (confirma los 3 ejes temáticos). Secciones 1, 3, 4 y 5 se basan en los nombres de los videos y sus subcarpetas, redactadas con conocimiento general. Sección 9 es complemento propio.
