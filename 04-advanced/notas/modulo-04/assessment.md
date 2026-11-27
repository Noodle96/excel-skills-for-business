# Módulo 4 · Assessment (Final Assessment de la semana)

> Archivo del curso: `material_curso/modulo-04/assessment/`. Aquí se documentan **las habilidades que evalúa**, no las respuestas ni los resultados. Para la teoría, ver [04-financial-functions-dates.md](04-financial-functions-dates.md).

**Escenario**: un solo libro (hoja `Calcs`) con **cuatro secciones** independientes: fechas por trimestre, un calendario de préstamo a 3 años con pagos mensuales, el análisis de un préstamo desde el punto de vista del banco y un calendario de depreciación calculado de dos maneras. Son **12 preguntas**; se escribe en las celdas verdes y se responde con una comprobación o con un valor del libro.

**Método de trabajo**: avanzar de arriba hacia abajo. Varias preguntas parten de lo construido en la anterior, así que conviene anotar cada respuesta **antes** de pasar a la siguiente, porque la tabla cambia.

**Avisos del enunciado**: no insertar ni borrar filas o columnas (las referencias de las instrucciones dejarían de coincidir) y guardar el trabajo.

## Habilidades por pregunta

### Sección 1 · Fechas

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 1 | Obtener el **fin del trimestre** de cada fecha, calculando cuántos meses faltan | `EOMONTH` + `MONTH`, con `MOD` o `IF` para el número de meses; celdas auxiliares opcionales |
| 2 | Obtener el **fin del trimestre anterior** | `EOMONTH` con meses negativos, partiendo del resultado anterior |

### Sección 2 · Calendario de préstamo

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 3 | Calcular la **cuota mensual** que deja el saldo en cero | `PMT` con tasa y periodos mensuales; convención de signos |
| 4 | **Construir la tabla**: saldo inicial, interés, cuota y pagos adicionales; leer el saldo en una fecha | Referencias relativas y absolutas; interés = saldo inicial × tasa / 12 |
| 5 | Obtener el **interés total** | Fila de totales |
| 6 | Registrar un **pago adicional** y **recalcular la cuota** para los periodos restantes | `PMT` sobre el saldo pendiente y los periodos que quedan; las demás celdas apuntan a esa |
| 7 | Comparar el **interés total** tras el cambio | Fila de totales |

### Sección 3 · `IRR` y `NPV`

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 8 | Calcular la **tasa interna de retorno** mensual y anualizarla | `IRR` sobre todos los flujos, incluida la inversión; anualizar como pide el enunciado (tasa mensual × 12) |
| 9 | Completar el interés con la tasa mensual y luego **por días** | Saldo × tasa anual × días / 365; los días salen de restar dos fechas |
| 10 | Calcular el **valor presente neto** al final del periodo 0 | `NPV` con tasa mensual sobre los cobros futuros; la inversión inicial, fuera de la función |

### Sección 4 · Depreciación

| # | Habilidad | Funciones / ideas |
|---|---|---|
| 11 | Calendario con **doble saldo decreciente** y suma de un rango de años | `DDB` con supuestos en celdas absolutas y signo negativo |
| 12 | Reproducirlo **desde primeros principios** y agregar activos a mitad de la vida útil | Saldo inicial × factor / vida; la adición en la columna del año indicado |

## Cosas a tener en cuenta (trampas)

- **Meses negativos en `EOMONTH`**: aplicarlos sobre el fin de trimestre ya calculado, no sobre la fecha original.
- **Signos**: la tabla del préstamo **suma** todas las columnas, así que los pagos deben ser negativos.
- **Referencias**: las celdas de supuestos van con `$`; las del periodo anterior, sin `$`.
- **El pago adicional cambia la tabla**: los valores leídos antes ya no estarán en las mismas celdas.
- **Nueva cuota**: se escribe una sola vez y las filas siguientes apuntan a ella; los periodos anteriores conservan la cuota original.
- **Periodicidad**: flujos mensuales piden tasa mensual, tanto en `PMT` como en `NPV`.
- **`NPV` descuenta también el primer valor** del rango: no incluir en él la inversión inicial.
- **Interés por días**: al no durar todos los meses lo mismo, el saldo final ya no da exactamente cero.
- **Orden de los argumentos de `DDB`**: `cost, salvage, life, period, factor`; las celdas de la hoja están en otro orden.
- **Rango de años**: "del 10 al 15 inclusive" son seis columnas.
- **Momento de la adición**: si ocurre al final de un año, ese año todavía no la deprecia.
- **Guardar** el libro al terminar cada sección.

## Checklist del assessment

- [ ] Fin de trimestre actual y anterior con `EOMONTH`
- [ ] Cuota mensual con `PMT`
- [ ] Tabla de préstamo completa que termina en saldo cero
- [ ] Pago adicional y nueva cuota para los periodos restantes
- [ ] `IRR` mensual y anualizada
- [ ] Interés por días entre fechas
- [ ] `NPV` con la inversión inicial fuera de la función
- [ ] `DDB` y su equivalente sin la función
- [ ] Adición a la base de activos
