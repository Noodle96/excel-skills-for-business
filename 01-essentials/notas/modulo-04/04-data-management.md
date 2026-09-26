# Módulo 4 · Excel Data Management

> Los videos de esta semana, según los archivos en `material_curso/modulo-04/`, son: **V01 Work with Data · V02 Work with Data (Find and Replace) · V03 Work with Data (Filtering) · V04 Work with Data (Sorting) · V05 Conditional Formatting**. Las secciones 1 y 2 están redactadas con conocimiento general (el nombre de archivo confirma el tema, no el contenido exacto del video); las secciones 3, 4 y 5 combinan eso con la terminología real de la lectura "Week 4: Toolbox".

## 1. Trabajar con filas y columnas (V01)

- **Insertar/eliminar** filas o columnas: clic derecho sobre el encabezado de fila/columna > Insertar o Eliminar (o `Ctrl + Signo más` / `Ctrl + Signo menos` con la fila/columna seleccionada).
- **Ajustar ancho/alto**: arrastrar el borde del encabezado, o doble clic en el borde para autoajustar al contenido.
- **Ocultar/mostrar** filas o columnas (ver atajos en la sección 6) — útil para simplificar la vista sin borrar datos.
- **Inmovilizar paneles (Freeze Panes)**: en la pestaña Vista, fija filas/columnas (típicamente encabezados) para que no se muevan al hacer scroll en datasets largos.

## 2. Buscar y reemplazar (V02)

- **Buscar**: `Ctrl + F` abre el cuadro de búsqueda; recorre resultados con "Buscar siguiente/anterior".
- **Reemplazar**: `Ctrl + H` abre el mismo cuadro con un campo adicional para el valor de reemplazo, y un botón "Reemplazar todo".
- Opciones avanzadas (botón "Opciones" en el cuadro): coincidir mayúsculas/minúsculas, coincidir con el contenido completo de la celda, buscar dentro de fórmulas vs. solo en los valores mostrados, y buscar por formato de celda en vez de por texto.

## 3. Filtrado (V03)

**Filter (Filtro)**: aplicar un filtro a una o varias columnas muestra rápidamente solo las filas que contienen la información que buscas. Se accede de tres formas: menú contextual (clic derecho), pestaña Inicio, o pestaña Datos. El filtro controla qué datos se muestran en pantalla — las filas que no cumplen el criterio elegido quedan ocultas mientras el filtro esté activo. Al quitar el filtro, la vista de datos vuelve a la normalidad.

Atajo: `Ctrl + Shift + L` (Mac: `Cmd + Shift + F`) activa/desactiva el filtro.

## 4. Ordenamiento (V04)

**Sorting (Ordenar)**: organiza los datos en un orden específico. Se accede de tres formas: menú contextual, pestaña Inicio, o pestaña Datos. Es una herramienta potente: permite ordenar por **varios niveles** a la vez, distinguir mayúsculas/minúsculas, y ordenar tanto de izquierda a derecha como de arriba hacia abajo. Para usarla, basta hacer clic en cualquier celda del conjunto de datos y abrir la herramienta de Ordenar.

## 5. Formato condicional (V05)

**Conditional Formatting**: en su forma más básica, aplica formato automáticamente cuando se cumple cierto criterio. Se accede desde el grupo Estilos, en la pestaña Inicio, tras seleccionar los datos a formatear condicionalmente. Existen varias opciones prediseñadas (resaltar celdas, escalas de color, barras de datos, conjuntos de iconos, reglas de superior/inferior). Es una herramienta muy versátil: el formato se actualiza dinámicamente cuando cambian los valores, lo cual la hace sumamente útil.

## 6. Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + 0` (Mac: `Cmd + 0`) | Ocultar la columna de la celda seleccionada |
| `Ctrl + 9` (Mac: `Cmd + 9`) | Ocultar la fila de la celda seleccionada |
| `Ctrl + Shift + 0` (Mac: `Cmd + Shift + 0`) | Mostrar columna oculta (selecciona las celdas alrededor de la columna oculta primero) |
| `Ctrl + Shift + 9` (Mac: `Cmd + Shift + 9`) | Mostrar fila oculta (selecciona las celdas alrededor de la fila oculta primero) |
| `Ctrl + Shift + L` (Mac: `Cmd + Shift + F`) | Activar/desactivar filtro |

> Nota del curso: en algunas versiones de Windows, `Ctrl+Shift+0` está asignado a una función del sistema operativo. Si no funciona, hay que desactivar esa asignación en la configuración de Windows.

## 7. Ninja tip: Formato condicional vs. Filtrado

Ambos permiten mostrar datos según criterios específicos, pero no son lo mismo:

- **Filtrado**: solo muestra los datos que cumplen el criterio. Se pueden combinar varios filtros, pero solo queda visible lo que cumple **todas** las condiciones a la vez.
- **Formato condicional**: resalta los datos que cumplen **cualquiera** de las condiciones elegidas (no exige cumplirlas todas), y aporta elementos visuales/gráficos (colores, iconos, barras) en vez de ocultar filas.

## 8. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria y más extensa de lo habitual — la lectura oficial de esta semana es bastante básica frente a todo lo que Excel permite hacer en gestión de datos, así que vale la pena ampliarla aquí.

**Filtrado avanzado**
- Filtros de texto (empieza por, contiene, termina en), filtros de número (mayor que, top 10, por encima del promedio) y filtros por color/icono, disponibles en la flecha de cada encabezado filtrado.
- **Filtro avanzado** (Datos > Avanzado): permite criterios complejos con múltiples condiciones AND/OR definidas en un rango de celdas aparte, algo que el AutoFiltro normal no puede hacer.

**Ordenamiento avanzado**
- Ordenar por **color de celda o de fuente**, no solo por valor.
- **Listas personalizadas**: ordenar según un orden que no es alfabético ni numérico (ej. Bajo/Medio/Alto, o días de la semana) definiendo la lista en Archivo > Opciones > Avanzadas > Editar listas personalizadas.
- Ordenar de **izquierda a derecha** (por fila) usando el botón "Opciones" dentro del cuadro de Ordenar — útil cuando los datos están organizados en columnas en vez de filas.

**Formato condicional avanzado**
- **Reglas personalizadas con fórmula**: en vez de usar solo las reglas prediseñadas, se puede escribir una fórmula propia que devuelva VERDADERO/FALSO (ej. `=MOD(FILA(),2)=0` para colorear filas alternas).
- **Administrar reglas** (Inicio > Formato condicional > Administrar reglas): edita, reordena o elimina reglas existentes, y define su prioridad cuando varias reglas aplican a la misma celda.
- **"Detener si es verdadera"**: opción para que, si una regla se cumple, las reglas de menor prioridad no se evalúen sobre esa misma celda.

**Otras herramientas de gestión de datos que vale la pena conocer**
- **Quitar duplicados** (Datos > Quitar duplicados): elimina filas repetidas según las columnas que elijas comparar.
- **Texto en columnas** (Datos > Texto en columnas): separa el contenido de una columna en varias, usando un delimitador (coma, espacio, etc.) o ancho fijo — muy común al importar datos de otros sistemas.
- **Agrupar/Desagrupar** (Datos > Agrupar): crea un esquema colapsable de filas o columnas relacionadas, útil para resumir secciones de una tabla grande sin borrar detalle.

**Accesibilidad**
El temario oficial de la especialización menciona "accesibilidad" en este módulo, aunque no hay un video específico entre los archivos de esta semana. En términos generales, esto se refiere a: usar el **Revisor de accesibilidad** (pestaña Revisar > Comprobar accesibilidad) para detectar problemas como falta de texto alternativo en imágenes/gráficos, contraste de color insuficiente, o un orden de lectura confuso para lectores de pantalla.

---

### Práctica
Archivos del curso: `material_curso/modulo-04/` (no hay archivos `Soln` esta semana; `manager-details` y `order-priority` son los datasets de práctica).

### Fuente
Lectura "Week 4: Toolbox" del curso (terminología de Filter, Sorting, Conditional Formatting, atajos y ninja tip — secciones 3, 4, 5, 6 y 7). Secciones 1 y 2 se basan en los nombres de los videos, redactadas con conocimiento general. Sección 8 es complemento propio, ampliado a pedido explícito por lo escueto del contenido oficial de esta semana.
