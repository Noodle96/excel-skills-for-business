# Reto práctico avanzado (Week 1) — resuelto

Archivo: `material_curso/modulo-01/advanced_challenge/W1_AdvPracticeChallenge.xlsx`. No trae archivo `Soln` — se resolvió directamente aplicando las técnicas de los 5 videos del módulo (ver [01-multiple-worksheets-workbooks.md](01-multiple-worksheets-workbooks.md)).

El libro trae hojas **April**, **May**, **June** (horas de soporte del help desk, mismo layout las 3) y **Report** (con datos de tickets por semana, incluyendo columnas de Priority, días abierto y Satisfaction Rating).

## Instrucciones originales del reto

**Paso 1**: Descargar el libro y abrirlo en Excel.

**Paso 2**: Renombrar las hojas para que tengan sentido: Sheet1 → April, Sheet2 → May, Sheet3 → June, Sheet4 → Report.

**Paso 3**: Hacer una copia de la hoja June, moverla a la izquierda de todas las demás (primera del libro), y llamarla Summary.

**Paso 4**: Dar pistas visuales a quien use el archivo — cambiar el color de las pestañas para distinguir las cifras mensuales de los datos de resumen.

**Paso 5**: Como las hojas mensuales tienen estructura idéntica, son ideales para referencias 3D. Usar una fórmula 3D para sumar las horas de help desk de April, May y June en la hoja Summary.

**Paso 6**: En la hoja Report, usar fórmulas de vínculo para traer el total de tickets de April, May y June desde los libros de origen suministrados.

> A partir de aquí el enunciado avisa que se pone más difícil — pide usar las herramientas del módulo para tareas no explicadas paso a paso, sin necesidad de agregar columnas nuevas.

**Paso 7**: Usar Consolidar para generar un resumen del número de tickets levantados por prioridad, combinando May Week 4, June Week 1 y June Week 2. Ordenar el resultado por Priority.

**Paso 8**: Usar Consolidar para generar un resumen del promedio de días que un ticket estuvo abierto, por prioridad, para esas mismas 3 semanas. Ordenar por Priority y cambiar a 2 decimales (cambiando el **formato**, sin usar una función de redondeo).

> Aviso del propio enunciado: antes del siguiente paso, borrar las demás referencias que quedaron en el cuadro de Consolidar.

**Paso 9**: Usar Consolidar para generar un resumen del número de tickets por cada calificación de satisfacción, para esas mismas 3 semanas, usando la función **Contar**. Ordenar por Satisfaction Rating.

## Cómo se resolvió cada paso

### Pasos 1-4: preparar el libro
- **Paso 2**: doble clic sobre cada pestaña para renombrarla.
- **Paso 3**: clic derecho en la pestaña June > **Mover o copiar** > marcar **Crear una copia** > elegir "mover al final" antes de April (o arrastrar con `Ctrl` presionado para duplicar y reposicionar) > renombrar la copia a "Summary".
- **Paso 4**: clic derecho en cada pestaña > **Color de etiqueta** — un color para las hojas mensuales (April/May/June) y otro distinto para Summary y Report.

### Paso 5: fórmula 3D
Como las 3 hojas mensuales comparten exactamente el mismo layout, califican para una referencia 3D (ver [sección 2 de la nota principal](01-multiple-worksheets-workbooks.md#2-fórmulas-3d-v2)). En Summary, por cada fila de staff:
```
=SUM(April:June!B4)
```
y se copia hacia abajo con el fill handle.

### Paso 6: fórmulas de vínculo entre libros
En Report, para traer el total de tickets de cada mes desde un libro externo (ver [sección 3](01-multiple-worksheets-workbooks.md#3-vinculación-entre-libros-v3)):
```
='[NombreDelLibro.xlsx]NombreHoja'!$B$5
```

### Pasos 7-9: Consolidar por categoría — la parte "tricky"

Los tres pasos usan la misma mecánica (Consolidar con **Usar etiquetas en: Columna izquierda**, porque las categorías —Priority o Satisfaction Rating— vienen en distinto orden en cada semana), cambiando solo la **función** y la **columna de valores**:

| Paso | Función | Columnas a seleccionar en cada referencia |
|---|---|---|
| 7 | Contar | Priority + una columna cualquiera con datos por ticket |
| 8 | Promedio | Priority + la columna de días que el ticket estuvo abierto |
| 9 | Contar | Satisfaction Rating + una columna cualquiera con datos por ticket |

Procedimiento por paso:
1. Seleccionar una celda vacía distinta para cada resultado.
2. Datos > Consolidar.
3. Elegir la función de la tabla de arriba.
4. Agregar las 3 referencias (May Week 4, June Week 1, June Week 2), seleccionando siempre **las 2 columnas** (la de la etiqueta y la que se va a consolidar) — Consolidar exige al menos 2 columnas: la primera se usa como etiqueta y la(s) otra(s) se calculan con la función elegida.
5. Marcar **Usar etiquetas en: Columna izquierda**.
6. Aceptar.
7. Ordenar el resultado por la columna de categoría correspondiente.

**Solo en el Paso 8**, además: seleccionar la columna de promedios > `Ctrl + 1` > Número > 2 decimales. Es un cambio de **formato visual**, no del valor almacenado — por eso el enunciado prohíbe usar `ROUND()`, que sí alteraría el valor real guardado en la celda.

### Gotchas descubiertos al resolverlo

- **El cuadro de Consolidar no se limpia solo entre usos.** La lista "Todas las referencias" conserva lo último que se usó; si no se borra antes de la siguiente consolidación, el nuevo resultado mezcla las referencias viejas con las nuevas. Por eso el enunciado avisa explícitamente antes del Paso 9 — pero aplica igual entre el Paso 7 y el Paso 8, aunque no lo mencione ahí.
- **Borrar esas referencias no afecta los resultados ya generados.** Cada consolidación, una vez aceptada, queda como valores fijos en su celda — la lista del cuadro de diálogo es solo memoria de trabajo para la *próxima* consolidación, no un vínculo permanente hacia cada resultado ya creado. Si más adelante se quiere modificar un consolidado anterior, hay que reconstruir manualmente su lista de referencias desde cero.
- **"Usar etiquetas en"** tiene 3 variantes según qué eje no está alineado entre las hojas de origen:
  - Solo **Columna izquierda**: las filas/categorías cambian de orden entre hojas (el caso de estos 3 pasos).
  - Solo **Fila superior**: los encabezados de columna cambian de orden, pero las filas ya están alineadas.
  - **Ambas**: ni filas ni columnas están alineadas — típico al combinar tablas 2D completas (categoría × período) de distintas fuentes.

---

### Fuente
Instrucciones originales del "Advanced Practice Challenge" de la Semana 1 del curso, resuelto en conversación con dudas puntuales sobre el comportamiento del cuadro de diálogo Consolidar (referencias no se limpian solas, diferencia entre Top row/Left column, formato vs. redondeo).
