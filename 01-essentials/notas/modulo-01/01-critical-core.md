# Módulo 1 · Critical Core of Excel

## 1. Interfaz de Excel

| Elemento | Qué es |
|---|---|
| **Cinta de opciones (Ribbon)** | Barra principal superior con pestañas (Inicio, Insertar, Fórmulas...) que agrupan comandos. Inicio tiene las herramientas más usadas. Doble clic en una pestaña la oculta para ganar espacio (un clic en Mac); repetir el gesto la vuelve a mostrar |
| **Barra de acceso rápido** | Está sobre la cinta (se puede mover debajo). Permite añadir herramientas de cualquier pestaña sin cambiar de pestaña |
| **Cuadro de nombres (Name Box)** | Muestra/permite escribir la referencia de la celda activa o un rango nombrado |
| **Barra de fórmulas** | Muestra el contenido real de la celda (fórmula o valor tal cual se escribió) |
| **Pestañas de hoja (Sheet tabs)** | Parte inferior; cada libro puede tener varias hojas. Se agregan con el `+` junto a la última pestaña; clic derecho sobre una pestaña para renombrarla y más comandos |
| **Barra de estado** | Debajo de la hoja: herramienta de zoom, tres opciones de vista y resultados de cálculo rápidos (suma, promedio, conteo) que se actualizan al seleccionar datos |

## 2. Terminología básica

- **Celda**: intersección de una columna y una fila; se nombra con la letra de columna y el número de fila (ej. `B3`).
- **Celda activa**: la celda seleccionada en ese momento (se ve con un borde grueso).
- **Fila**: se numeran; una hoja tiene **1.048.576** filas.
- **Columna**: se nombran con letras; una hoja tiene **16.384** columnas.
- **Rango**: grupo de celdas contiguas (ej. `B4:D10`).
- **Hoja de cálculo (Worksheet)**: una "pestaña" dentro de un libro.
- **Libro (Workbook)**: el archivo `.xlsx` completo (su nombre aparece arriba en la ventana); puede contener varias hojas.
- **Referencia de celda**: la "dirección" de una celda (relativa `A1`, absoluta `$A$1`).
- **Fill handle**: el cuadradito negro en la esquina inferior derecha de la celda activa. Se activa/desactiva en `Archivo > Opciones > Avanzadas > Opciones de edición > Habilitar controlador de relleno`.

## 3. Navegación

| Atajo | Acción |
|---|---|
| `Flechas` | Mover una celda a la vez |
| `Re Pág` / `Av Pág` | Subir/bajar una "página" (las filas visibles en pantalla) |
| `Ctrl + Flecha` | Saltar a la siguiente celda vacía en esa dirección; en datos sin huecos, llega al borde del bloque |
| `Ctrl + Inicio` | Ir al inicio de la hoja (`A1`). En teclados sin tecla Inicio: `Ctrl + Fn + Inicio` |
| `Ctrl + Fin` | Ir a la última celda usada de la hoja (esquina inferior derecha) |
| `Ctrl + Rueda del mouse` | Zoom in/out |
| `F5` o `Ctrl + G` | Abrir "Ir a" (Go To) — útil para saltar a una celda o rango específico |

## 4. Atajos esenciales

Si una combinación pide varias teclas, se pulsan a la vez. En Mac, `Cmd` sustituye a `Ctrl`.

| Atajo | Acción |
|---|---|
| `Ctrl + Z` | Deshacer (varios niveles; algunas acciones no se pueden deshacer, ej. eliminar una hoja con contenido o *Mover gráfico*) |
| `Ctrl + Y` | Rehacer la última acción; útil para repetir un paso varias veces |
| `Ctrl + N` | Nuevo libro |
| `Ctrl + O` | Abrir libro existente |
| `Ctrl + W` | Cerrar el libro actual |
| `Ctrl + S` | Guardar (si es la primera vez, pide nombre y ubicación). Hay que guardar con frecuencia |
| `Ctrl + A` | Seleccionar todo. En una celda vacía selecciona la hoja completa; dentro de un bloque de datos selecciona primero el bloque, y una segunda pulsación selecciona la hoja |
| `Alt + Enter` | Salto de línea dentro de la misma celda (mientras la editas) |

## 5. Entrada de datos básica

- Escribir y presionar `Enter` (baja) o `Tab` (avanza a la derecha).
- `Alt + Enter` inserta un salto de línea dentro de la misma celda.
- Excel auto-detecta el tipo de dato (texto, número, fecha) según el formato de lo que escribes.

## 6. Fill Handle (Manejador de relleno)

El cuadradito en la esquina inferior derecha de una celda/selección seleccionada.

- **Arrastrar**: copia el valor o continúa una serie reconocida (números, fechas, días, meses, listas personalizadas).
- **Doble clic** sobre el manejador: autocompleta hacia abajo hasta donde haya datos en la columna vecina.
- Con **dos celdas seleccionadas** (ej. 1 y 2), Excel detecta el patrón y continúa la secuencia (3, 4, 5...).
- `Ctrl` mientras arrastras una serie numérica: fuerza a copiar el valor en vez de continuar la serie.

## 7. Opciones de copiar y pegar

- `Ctrl + C` / `Ctrl + V`: copiar/pegar normal (todo: valor, fórmula, formato).
- `Ctrl + Alt + V` (Pegado especial): elegir pegar **solo valores**, **solo fórmulas**, **solo formato**, **transponer** (filas↔columnas), etc.
- Útil para "romper" una fórmula y dejar el resultado fijo, o para clonar solo el estilo visual sin arrastrar los datos.

## 8. Plantillas

- `Archivo > Nuevo` ofrece plantillas prediseñadas (facturas, calendarios, presupuestos).
- Se pueden guardar plantillas propias como `.xltx` para reutilizar un formato/estructura sin partir de cero.

## 9. Ninja tip: ocultar filas y columnas que no usas

Para despejar la hoja de filas/columnas infinitas:

1. Selecciona la primera **columna** que no necesitas y pulsa `Ctrl + Shift + →`. Clic derecho en la selección > **Ocultar**.
2. Selecciona la primera **fila** que no necesitas y pulsa `Ctrl + Shift + ↓`. Clic derecho en la selección > **Ocultar**.

Para recuperarlas: selecciona la última fila/columna visible, arrastra hacia la zona oculta y elige **Mostrar**.

---

### Práctica
Archivos del curso: `material_curso/modulo-01/` (resolver los que no terminan en `Soln`).

### Fuente
Lectura "Week 1: Toolbox" del curso (Keyboard Shortcuts, Terminology, Ninja Tips). Lista completa de atajos: [Microsoft Support](https://support.office.com/en-us/article/Excel-keyboard-shortcuts-and-function-keys-for-Windows-1798d9d5-842a-42b8-9c99-9b7213f0040f).
