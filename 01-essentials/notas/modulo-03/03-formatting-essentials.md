# Módulo 3 · Excel Formatting Essentials

> Los videos de esta semana, según los archivos en `material_curso/modulo-03/`, son: **V01 Font Formatting · V02 Borders · V03 Alignment · V04 Format Painter · V05 Number Formats · V06 Styles and Themes**. Las secciones 1-6 siguen ese orden; el contenido está redactado con conocimiento general de Excel (los nombres de archivo confirman el tema, no el contenido exacto del video), así que algún ejemplo puntual podría no calzar 100% con lo que muestra el curso. No hubo lectura "Toolbox" pegada esta semana, así que los atajos de la sección 7 también son de conocimiento general.

## 1. Formato de fuente (V01)

Controles básicos de texto, en la pestaña Inicio o en `Ctrl + 1` (Formato de celdas):

- Tipo de fuente, tamaño, **negrita** (`Ctrl+B`), *cursiva* (`Ctrl+I`), <u>subrayado</u> (`Ctrl+U`).
- Color de fuente vs. color de relleno de la celda (dos herramientas distintas en la cinta, ambas con una flechita para elegir color).
- Tachado, super/subíndice — disponibles en el diálogo `Ctrl+1 > Fuente`, no en la cinta por defecto.

## 2. Bordes (V02)

- La cinta ofrece bordes rápidos (todos, exterior, inferior, etc.) desde el botón de bordes en Inicio.
- Para control fino (grosor, estilo de línea, color, solo algunos lados): `Ctrl+1 > Bordes`.
- Diferencia clave: aplicar borde **exterior** a un rango dibuja un solo marco alrededor de todo el rango; aplicar borde a **todas las celdas** dibuja también las líneas internas entre cada celda.

## 3. Alineación (V03)

- Horizontal: izquierda / centro / derecha. Vertical: arriba / medio / abajo.
- **Ajustar texto (Wrap Text)**: hace que el contenido salte de línea dentro de la celda en vez de desbordarse.
- **Combinar celdas (Merge & Center)**: une varias celdas en una sola, útil para títulos; ojo que solo conserva el valor de la celda superior-izquierda.
- **Orientación**: se puede rotar el texto (diagonal o vertical) desde el botón de orientación o en `Ctrl+1 > Alineación`.
- **Sangría**: aumenta/disminuye el espacio antes del contenido dentro de la celda.

## 4. Format Painter — Pincel de formato (V04)

- Copia **solo el formato** de una celda (fuente, bordes, alineación, formato de número) sin copiar su contenido.
- Uso: seleccionar la celda de origen → botón "Copiar formato" (icono de brocha) en Inicio → clic (o arrastrar sobre un rango) en el destino.
- **Doble clic** sobre el botón: deja el pincel "activado" para aplicar el mismo formato a varias celdas o rangos no contiguos sin tener que volver a seleccionarlo cada vez. Se desactiva presionando `Esc` o volviendo a hacer clic en el botón.

## 5. Formatos de número (V05)

Cambian cómo se **muestra** un valor sin alterar el número real guardado en la celda.

| Formato | Ejemplo visual |
|---|---|
| General | `1234.5` |
| Número | `1,234.50` |
| Moneda | `$1,234.50` |
| Contabilidad | `$ 1,234.50` (alinea el símbolo a la izquierda) |
| Fecha / Hora | `25/09/2026` |
| Porcentaje | `12.5%` |
| Fracción | `1/2` |
| Científico | `1.23E+03` |
| Texto | fuerza a tratar el valor como texto (no calculable) |

Acceso rápido desde el menú desplegable de formato de número en Inicio, o control total en `Ctrl+1 > Número`, donde también se pueden crear **formatos personalizados** (ej. `$#,##0.00` o `0.0%`).

## 6. Estilos y temas (V06)

- **Estilos de celda**: combinaciones de formato predefinidas (fuente, relleno, bordes) que se aplican con un clic desde Inicio > Estilos de celda (ej. "Título 1", "Entrada", "Cálculo"). Se pueden modificar o crear estilos propios.
- **Temas del libro**: en la pestaña Diseño de página, cambian de golpe la paleta de colores, las fuentes y los efectos de **todo el libro** a la vez, manteniendo consistencia visual entre hojas, tablas y gráficos sin tener que reformatear celda por celda.
- Diferencia clave: un **estilo** se aplica a celdas puntuales; un **tema** afecta el aspecto general del libro completo.

## 7. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + B` | Negrita |
| `Ctrl + I` | Cursiva |
| `Ctrl + U` | Subrayado |
| `Ctrl + 1` | Abrir el diálogo "Formato de celdas" |
| `Ctrl + 5` | Tachado |
| `Ctrl + Shift + &` | Aplicar borde de contorno a la selección |
| `Ctrl + Shift + ~` | Aplicar formato General (quitar formato de número) |

## 8. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **Borrar formato sin borrar el contenido**: Inicio > Borrar > Borrar formatos. Útil para "resetear" una celda sobre-formateada sin perder los datos.
- **Formato de número personalizado con códigos**: además de los formatos predefinidos, se pueden escribir códigos propios en `Ctrl+1 > Número > Personalizada`, ej. `0.00 "kg"` muestra `12.50 kg` sin que el texto "kg" afecte los cálculos.
- **Formato condicional**: parecido en espíritu (cambia la apariencia de una celda), pero se basa en **reglas** que evalúan el valor de la celda (ej. resaltar en rojo si es negativo) en vez de aplicarse manualmente. Se ve a fondo en el Módulo 4 (Excel Data Management).
- **Formato vs. valor**: aplicar formato de moneda a una celda no cambia el número que guarda Excel internamente — solo cambia cómo se ve. Es clave recordarlo al copiar/pegar valores hacia otro sistema.

---

### Práctica
Archivos del curso: `material_curso/modulo-03/` (resolver los que no terminan en `Soln`).

### Fuente
Secciones 1-7 se basan en los títulos de los videos confirmados por los archivos en `material_curso/modulo-03/`, redactadas con conocimiento general de Excel (no hubo lectura "Toolbox" pegada esta semana). Sección 8 es complemento propio.
