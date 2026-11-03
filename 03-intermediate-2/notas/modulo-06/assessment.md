# Assessment del Módulo 6 (Recording Macros) — habilidades a dominar

Este documento **no guarda respuestas, resultados ni puntajes**: toma cada pregunta como una **habilidad** que hay que saber ejecutar sin ayuda, con el procedimiento para hacerla, para poder repasarla y practicarla después.

**Cómo es el assessment**: se trabaja con un libro con una macro que escribe un encabezado y calcula gastos de viaje. Las preguntas piden **abrir el código en el editor de VBA y razonar sobre él**, grabar macros pequeñas y revisar qué línea se genera, además de una pregunta de fórmulas. (Las preguntas sobre mostrar escenarios con macros y sobre tipos de validación son del assessment final del curso: ver [final-assessment.md](../modulo-07/final-assessment.md).)

## Habilidades que pedía el assessment

| Qué se pedía | Habilidad |
|---|---|
| Qué pasa si se cambia el nombre de una macro en la línea `Sub` | Reglas de nombres de macros y del procedimiento `Sub` |
| Qué pasa si se edita el comentario `' Keyboard Shortcut: ...` | Distinguir un comentario del código real, y saber dónde se configura el atajo |
| Qué hace una fórmula en notación R1C1 al cambiar un número | Leer `R[-n]C` y razonar sobre el rango que genera |
| Atajo para volver de la ventana de código a la hoja | Atajos de macros (`Alt + F11`) |
| En qué celda cae un encabezado al ejecutar una macro desde otra celda | Referencias absolutas vs relativas en una macro grabada |
| Grabar una macro relativa que multiplica la celda de la izquierda y revisar su código | Grabar con *Usar referencias relativas* y reconocer las líneas `Offset` |
| Obtener la primera fecha de la semana actual con una fórmula | `TODAY` + `WEEKDAY` con tipo de devolución |

## Cómo se hace cada una

### Nombre de la macro y línea `Sub`
- La primera línea de una macro grabada es `Sub NombreDeLaMacro()`.
- El nombre debe **empezar con letra** y **no puede tener espacios** (se usa `_` o mayúsculas intermedias: `Travel_Expenses`, `TravelExpenses`). Con un espacio, la línea queda en rojo y la macro no corre.
- Cambiar el nombre válido cambia el nombre de la macro: los botones o atajos asignados al nombre viejo dejan de funcionar.

### Comentarios y atajo de teclado
- Las líneas que empiezan con **apóstrofe (`'`)** son **comentarios**: Excel no las ejecuta, así que editar el texto no cambia nada.
- El atajo real se configura con `Alt + F8` > seleccionar la macro > **Opciones**.
- Cuidado al elegir atajos: anulan el comando de Excel que ya los use (por ejemplo `Ctrl + E` es *Relleno rápido*). Mejor con mayúsculas: `Ctrl + Shift + letra`.

### Leer una fórmula en R1C1
```
ActiveCell.FormulaR1C1 = "=SUM(R[-5]C:R[-1]C)"
```
- `R[-5]C` = la celda 5 filas **arriba** de la celda activa, en la misma columna; `C` sin corchetes = la misma columna.
- Cada número entre corchetes cuenta cuántas filas hay desde la celda activa. Si el primer número pasa de 5 a 4, el rango incluye **una celda menos** (empieza una fila más abajo).
- El rango depende de **la celda activa al ejecutar la macro**: para saber qué suma, hay que ubicar esa celda y contar.
- No hay error: la fórmula sigue siendo válida y solo cambia el resultado.

### Volver de la ventana de código a la hoja
- `Alt + F11` alterna entre la hoja y el editor. Otros atajos útiles: `Alt + F8` (cuadro de macros) y `Alt + T + M + R` (grabar/detener).

### Absoluta vs relativa en una macro ya grabada
- Una macro grabada **sin** referencias relativas guarda una celda fija (`Range("A1").Select`): da igual desde qué celda se ejecute, el resultado cae en esa celda.
- Una grabada **con** referencias relativas guarda desplazamientos (`ActiveCell.Offset(1, 0).Select`): el resultado depende de la celda activa al ejecutar.
- Para saber cuál es, se abre la macro en el editor y se mira qué selecciones tiene.

### Grabar una macro relativa y leer su código
Pasos: activar *Usar referencias relativas* → empezar en la celda correcta → grabar la macro con el nombre pedido → escribir la fórmula (por ejemplo, la celda de la izquierda multiplicada por una constante) → **Enter** → detener.
```vba
Sub Calc()
    ActiveCell.FormulaR1C1 = "=RC[-1]*0.73"
    ActiveCell.Offset(1, 0).Range("A1").Select
End Sub
```
- Primera línea ejecutable: la fórmula (`RC[-1]` = misma fila, una columna a la izquierda).
- Segunda línea ejecutable: el **Enter** (baja una fila) se graba como `ActiveCell.Offset(1, 0).Range("A1").Select`.

### Primer día de la semana con `TODAY` y `WEEKDAY`
```
=TODAY()-WEEKDAY(TODAY(),3)
```
- `WEEKDAY(fecha, 3)` devuelve 0 para el lunes y 6 para el domingo, así que restarlo a la fecha de hoy da el **lunes** de la semana actual.
- Si Excel usa `;` como separador, o está en español, la fórmula se escribe con `;` y con `HOY` y `DIASEM`. Si no hay error pero se ve un número, la celda necesita formato de fecha.

## Gotchas

- **Un comentario no es código**: cambiar su texto no cambia el comportamiento.
- **Con espacios en el nombre**, la macro deja de funcionar (línea en rojo).
- **El deshacer no se graba** y las acciones que se deshacen sí quedan en el código.
- **No hacer ninguna acción extra mientras se graba una macro pequeña**: cada clic puede dejar una línea.
- **Guardar la macro en el libro**, no en el libro de macros personal, si el enunciado pide "store in workbook".
- **Detener la grabación** al terminar; si no, todo lo siguiente se graba.
- **Un `.xlsx` descarta las macros**: guardar como `.xlsm`.

## Checklist: lo que debo ser capaz de hacer sin ayuda

- [ ] Abrir el editor (`Alt + F11`) y volver a la hoja con el mismo atajo
- [ ] Reconocer `Sub`, `End Sub` y comentarios, y saber qué no se ejecuta
- [ ] Explicar por qué el nombre de una macro no puede tener espacios
- [ ] Configurar o cambiar el atajo de una macro desde `Alt + F8` > Opciones
- [ ] Leer `R[-n]C` y calcular qué rango suma una fórmula en R1C1
- [ ] Distinguir una macro absoluta de una relativa mirando su código
- [ ] Grabar una macro relativa y localizar su línea `Offset`
- [ ] Obtener el lunes de la semana actual con `TODAY` y `WEEKDAY(…,3)`
