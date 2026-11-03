# Practice Challenge del Módulo 6 — Radmehr Accounting Services

Guía de cómo se resolvió el reto de práctica de la semana (`material_curso/modulo-06/challenge/`). Teoría de base en [06-recording-macros.md](06-recording-macros.md).

## 1. Enunciado (traducido)

Se pide organizar la facturación y la gestión de datos de **Radmehr Accounting Services**. El libro tiene tres hojas de trabajo:

- **`Data Entry`**: para introducir los datos de cada cliente.
- **`Table`**: la base de datos de facturas.
- **`Invoice Template`**: genera una factura de cualquier número de factura seleccionado.

Tareas:

1. En `Table`, convertir el rango en una **tabla** y usar **rangos con nombre** en las columnas.
2. Poner las fórmulas de **Total**, **Discount** (5% de descuento por más de 10 horas de servicio), **Total Amount Due** y **Total Revenue**.
3. En `Data Entry`, introducir: fecha 2016-08-04, cliente Rod Ltd, 22 horas de servicio y tarifa de $50.
4. Crear una macro `Transfer` que pase los datos de `Data Entry` a `Table` (pista: hay que usar *Usar referencias relativas* en algunas partes).
5. Más pistas: usar `Ctrl + Inicio` y `Ctrl + Flecha abajo` para llegar a la siguiente fila disponible; el **número de factura** debe basarse en el número anterior.
6. Antes de detener la grabación, volver a `Data Entry`, borrar los datos actuales y dejarla lista para la siguiente entrada.
7. Asignar un **botón** a la macro `Transfer` en `Data Entry`.

## 2. Cómo se resolvió cada paso

### Tareas 1-3: la tabla y las fórmulas
- **Tabla** (`Ctrl + T`, ver [Intermediate I M5](../../02-intermediate-1/notas/modulo-05/05-tables.md)), con nombres de rango por columna ([Intermediate I M3](../../02-intermediate-1/notas/modulo-03/03-named-ranges.md)).
- Las fórmulas se escriben **en la tabla**, para que las filas nuevas las hereden solas y la macro no tenga que tocar esas columnas:
  - Total = horas × tarifa.
  - Discount = `IF(horas > 10, Total × 5%, 0)` ([Intermediate II M2](../notas/modulo-02/02-conditional-logic.md)).
  - Total Amount Due = Total − Discount.
  - Total Revenue = suma de la columna Total Amount Due.
- En el libro de solución, `Invoice Template` toma sus datos con `VLOOKUP` de coincidencia exacta sobre el número de factura ([Intermediate II M3](../notas/modulo-03/03-automating-lookups.md)).

### Tareas 4-6: la macro `Transfer`
La clave es **qué parte va con referencias relativas y cuál con absolutas**:

| Zona | Referencias | Por qué |
|---|---|---|
| `Data Entry` | **Absolutas** (botón apagado) | Las celdas de origen son siempre las mismas |
| `Table` | **Relativas** (botón encendido) | El destino es la primera fila libre, que cambia en cada ejecución |

Procedimiento de grabación (celdas de ejemplo: fecha en `C5`, cliente/horas/tarifa en `C7:C9`; se empieza en `Data Entry` con el botón apagado):

| # | Acción | Botón de relativas |
|---|---|---|
| 1 | Clic en la pestaña `Table` | apagado |
| 2 | `Ctrl + Inicio` (punto de partida fijo) | apagado |
| 3 | Enciende el botón | ENCENDIDO |
| 4 | `Ctrl + Flecha abajo` (último registro) → `Flecha abajo` (primera celda libre) → `Tab` (columna de la fecha) | encendido |
| 5 | Apaga → pestaña `Data Entry` → clic en `C5` → `Ctrl + C` | APAGADO |
| 6 | Pestaña `Table` → enciende → `Ctrl + Alt + V` > Valores → `Tab` | ENCENDIDO |
| 7 | Apaga → `Data Entry` → selecciona `C7:C9` → `Ctrl + C` | APAGADO |
| 8 | Pestaña `Table` → enciende → `Ctrl + Alt + V` > Valores + Transponer → `Esc` | ENCENDIDO |
| 9 | `Ctrl + Flecha izquierda` (al primer campo de la fila nueva) | encendido |
| 10 | `=` → `Flecha arriba` → `+1` → `Ctrl + Enter` | encendido |
| 11 | Apaga → pestaña `Data Entry` | APAGADO |
| 12 | Clic en `C5` → `Ctrl` + selecciona `C7:C9` → `Supr` | apagado |
| 13 | Clic en `C5` (la hoja de entrada queda con el cursor en la fecha) | apagado |
| 14 | Clic en la pestaña `Table` → `Ctrl + Inicio` → `Ctrl + Flecha abajo` (deja el cursor en el registro recién agregado, para verlo) | apagado |
| 15 | Detener grabación | — |

Por qué funciona:
- El **punto de partida fijo** (`Ctrl + Inicio` en `Table`) hace que la macro no dependa de dónde estabas.
- Los pasos en `Data Entry` van **con el botón apagado**, así que el código guarda celdas fijas y no se desplazan entre ejecuciones.
- **Pegar valores** evita arrastrar el formato de la hoja de entrada a la tabla.
- El **número de factura se escribe al final**, con la fila ya dentro de la tabla, y se basa en la celda de arriba.
- `Ctrl + Enter` acepta la fórmula **sin bajar** de celda.
- El cierre (pasos 13-14) deja `Data Entry` lista para la siguiente entrada y muestra el registro agregado, sin necesitar referencias relativas.

### Tarea 7: el botón
Programador > Insertar > **Botón (control de formulario)** → dibujarlo en `Data Entry` → asignarle la macro `Transfer`.

## 3. Gotchas descubiertos

- **Escribir una fórmula (`=...`) en la fila inmediatamente debajo de la tabla activó la fila de totales (*Total Row*)** en lugar de extender la tabla, y Excel preguntó si se quería reemplazar. Escribir un **valor normal** en esa fila sí extiende la tabla sin problema. Por eso la fórmula del número de factura va **al final**, cuando la fila ya es parte de la tabla.
- **Con una fila de totales, `Ctrl + Flecha abajo` llega a esa fila** y el registro nuevo se escribiría en el lugar equivocado.
- **Si los clics en `Data Entry` se graban con el botón encendido**, se guardan como desplazamientos desde la celda activa. La macro funciona la primera vez, pero en las siguientes el origen se corre (en este caso la fecha salió **en blanco** en la segunda ejecución y con el **nombre del cliente** en la tercera). La solución es grabarlos con el botón apagado: el código guarda celdas fijas (`Range("C5").Select`).
- **El botón de relativas solo afecta a los clics y movimientos**, no a cambiar de pestaña. Lo importante es dejarlo en el estado correcto **antes del primer clic en una celda de esa hoja**.
- **Autocorrección de tablas**: al escribir la fórmula de la columna puede aparecer un icono que ofrece rellenar toda la columna con ella; no se acepta, porque pisaría los números anteriores.
- **El deshacer no se graba**: si algo sale mal durante la grabación, conviene detenerla, eliminar la macro y empezar de nuevo.
- **Al terminar, probar la macro varias veces seguidas con datos distintos** y comprobar que cada registro tenga la fecha correcta y el número de factura siguiente al anterior.
- **Reintentar limpio**: antes de volver a grabar, hay que eliminar la macro anterior (`Alt + F8` > Eliminar), borrar las filas de prueba de la tabla y volver a cargar los datos de ejemplo en `Data Entry`.
