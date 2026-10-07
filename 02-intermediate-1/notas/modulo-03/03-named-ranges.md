# Módulo 3 · Named Ranges

> Los videos de esta semana, según los archivos en `material_curso/modulo-03/`, son: **V1 Intro Named Ranges · V2 Create Named Ranges · V3 Manage Named Ranges · V4 Named Ranges In Formulas · V5 Apply Names**. Las secciones 1-5 siguen ese orden; combinan la terminología y los atajos reales de la lectura "Week 3: Toolbox" con conocimiento general (el nombre de archivo confirma el tema, no el contenido exacto del video), así que algún detalle puntual podría no calzar 100% con lo que muestra el curso.

## 1. Introducción a los rangos con nombre (V1)

**Named Range**: una forma de darle un nombre memorable a una celda o a un rango de celdas. Ese nombre se puede usar luego en fórmulas, donde funciona como una **referencia absoluta**. También vuelve las fórmulas más legibles, porque un nombre tiene más significado que una referencia de celda. Ejemplo: `=N4*Pension_Rate` se entiende mejor que `=N4*$P$2`.

Reglas generales para nombrar (conocimiento general):
- Sin espacios — se usa guion bajo o mayúsculas internas (`Pension_Rate`, `PensionRate`).
- Debe empezar con una letra, guion bajo o barra invertida (no con un número).
- No puede parecerse a una referencia de celda (`A1`, `R1C1` no son nombres válidos).
- No distingue mayúsculas de minúsculas.

## 2. Crear rangos con nombre (V2)

Hay varias formas de crearlos:

1. **Cuadro de nombres (Name Box)**: seleccionar la celda o rango, escribir el nombre en el cuadro de nombres (a la izquierda de la barra de fórmulas) y presionar `Enter`.
2. **Definir nombre (Define Name)**: pestaña Fórmulas > Definir nombre. Permite elegir el nombre, el ámbito (libro u hoja) y el rango al que se refiere. En Mac su atajo es `Cmd + fn + F3` (la herramienta existe en ambas plataformas, el atajo es solo de Mac).
3. **Crear desde la selección (Create Names from Selection)**: para crear varios nombres de una sola vez. Se selecciona el rango **incluyendo los encabezados**, se activa la herramienta (`Ctrl + Shift + F3`; Mac: `Cmd + Shift + fn + F3`) y se elige de dónde salen los nombres: normalmente **Fila superior** o **Columna izquierda**, pero también existen **Fila inferior** y **Columna derecha**.
4. **Administrador de nombres (Name Manager)**: botón "Nuevo" dentro del administrador (ver sección 3).

## 3. Administrar rangos con nombre (V3)

El **Administrador de nombres** (Name Manager, `Ctrl + F3` en Windows) permite **crear, modificar y eliminar** rangos con nombre. Ahí se ve, para cada nombre: su valor actual, a qué celdas se refiere ("Se refiere a"), su ámbito, y se puede filtrar la lista.

Dos atajos útiles para trabajar con ellos:
- **`F3` — Pegar nombre (Paste Name)**: al escribir una fórmula, si no recuerdas el nombre del rango que quieres usar, este diálogo muestra la lista de todos los nombres definidos en el libro.
- **`Ctrl + F3` — Administrador de nombres** (solo Windows).

## 4. Usar rangos con nombre en fórmulas (V4)

Dentro de una fórmula, el nombre sustituye a la referencia de celda:

```
=N4*Pension_Rate
```

Formas de insertarlo: escribirlo directamente (Excel sugiere el nombre mientras escribes), elegirlo de la lista de autocompletado, o pulsar `F3` para pegarlo desde la lista. Como un nombre funciona como referencia absoluta, al copiar la fórmula hacia abajo o hacia los lados siempre apunta a la misma celda o rango.

## 5. Aplicar nombres (V5)

**Apply Names** (Fórmulas > Definir nombre > Aplicar nombres): si ya tienes fórmulas escritas con referencias de celda normales (`=N4*$P$2`) y luego defines un nombre para `$P$2`, esta herramienta reemplaza automáticamente esas referencias por el nombre en todas las fórmulas existentes — sin tener que reescribirlas a mano una por una.

## 6. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + Enter` | Terminar de editar y **quedarse en la misma celda** (con `Enter` bajarías a la celda siguiente; con `Tab`, a la derecha) |
| `F3` | Pegar nombre (Paste Name) — lista de todos los nombres del libro, al escribir una fórmula |
| `Ctrl + F3` | Administrador de nombres (solo Windows) |
| `Cmd + fn + F3` | Definir nombre (solo Mac; la herramienta existe en ambas plataformas) |
| `Ctrl + Shift + F3` (Mac: `Cmd + Shift + fn + F3`) | Crear nombres desde la selección |
| `Ctrl + Shift + Flechas` | Seleccionar hasta el borde de los datos. Manteniendo `Ctrl + Shift`, otra flecha extiende la selección en otra dirección |

## 7. Terminología

- **Named Range (Rango con nombre)**: nombre memorable asignado a una celda o rango de celdas; se usa en fórmulas como una referencia absoluta y las hace más legibles (`=N4*Pension_Rate` en vez de `=N4*$P$2`).

## 8. Ninja tip: ver todos los rangos con nombre de un vistazo

Para ver todos los rangos con nombre que has creado en la hoja, basta con **alejar el zoom a menos del 40%**: Excel muestra el contorno y el nombre de cada rango directamente sobre la hoja.

## 9. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Ámbito (scope)**: un nombre puede ser de ámbito **Libro** (se usa desde cualquier hoja) o de ámbito **Hoja** (solo válido en esa hoja, y permite reutilizar el mismo nombre en hojas distintas). Se elige al crearlo en Definir nombre o en el Administrador de nombres.
- **Constantes con nombre**: un nombre no tiene por qué apuntar a una celda — también puede contener un valor fijo (ej. `IVA` = `0.19`) o incluso una fórmula, sin que ese valor viva en ninguna celda de la hoja.
- **Rangos con nombre dinámicos**: definidos con funciones como `OFFSET` o `INDEX`, crecen o se encogen automáticamente cuando se agregan datos. Hoy suelen reemplazarse con **tablas** (Módulo 5 de este curso), que se expanden solas.
- **Ir a un rango con nombre**: `F5` (Ir a) o el desplegable del cuadro de nombres permiten saltar directamente al rango y seleccionarlo.
- **Pegar lista de nombres**: en `F3` (Pegar nombre) existe el botón "Pegar lista", que escribe en la hoja una tabla con todos los nombres y a qué celdas se refieren — útil para documentar un libro.
- **Combinar con funciones ya vistas**: los nombres funcionan dentro de cualquier función (`SUM`, `AVERAGE`, `CONCAT`, `FIND`...), por ejemplo `=SUM(Ventas_Enero)` en vez de `=SUM(B2:B31)`.

---

### Práctica
Archivos del curso: `material_curso/modulo-03/` (resolver los que no terminan en `Soln`; los 5 videos traen versión `Soln`). El reto de práctica está en `challenge/` con su solución incluida; la carpeta `assessment/` está vacía.

### Fuente
Lectura "Week 3: Toolbox" del curso (atajos, terminología de Named Ranges, ninja tip del zoom — secciones 6, 7 y 8, y la base de las secciones 2, 3 y 4). Secciones 1-5 se apoyan además en los nombres de los videos, redactadas con conocimiento general. Sección 9 es complemento propio.
