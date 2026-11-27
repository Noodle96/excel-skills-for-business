# Módulo 04 · Financial Functions and Working with Dates

> Las fórmulas usan `,` como separador. Si tu Excel está configurado con `;`, cámbialo al escribirlas.

## Mapa de la semana

| Sección | Tema | Libro del curso |
|---|---|---|
| 1 | Trabajar con fechas: `EOMONTH`, `EDATE`, `WORKDAY.INTL`, `DATEDIF` | `W4_01_Date_Functions.xlsx` |
| 2 | Funciones financieras: `FV`, `PV`, `PMT` | `W4_02_Intro_Financial_Functions.xlsx` |
| 3 | Calendario de préstamo | `W4_03_Loan_Schedule.xlsx` |
| 4 | `NPV` e `IRR` | `W4_04_NPV_and_IRR.xlsx` |
| 5 | Depreciación: `SLN`, `SYD`, `DDB` | `W4_05_Depreciation_Functions.xlsx` |

## 1. Trabajar con fechas

Una fecha es un número de serie, así que se puede sumar y restar. Estas funciones devuelven ese número: hay que dar **formato de fecha** a la celda.

| Función | Devuelve |
|---|---|
| `EOMONTH(start_date, months)` | El **último día del mes**, `months` meses después (0 = el mismo mes) |
| `EDATE(start_date, months)` | La misma fecha `months` meses después (o antes, si es negativo) |
| `WORKDAY(start_date, days, [holidays])` | La fecha tras sumar `days` **días hábiles**; fin de semana = sábado y domingo |
| `WORKDAY.INTL(start_date, days, [weekend], [holidays])` | Igual, pero con el **fin de semana configurable** |
| `DATEDIF(start_date, end_date, unit)` | La diferencia entre dos fechas |

### El caso del libro: facturas con plazo *EOM + 10wd*

El pago vence 10 días hábiles después del fin del mes de la factura. Festivos en `K5:K7` (25/12/2017, 26/12/2017 y 1/1/2018).

```excel
Vencimiento:    =WORKDAY.INTL(EOMONTH(C5,0), 10, 1, $K$5:$K$7)    13/8/2017 → 14/9/2017
Recordatorio:   =EDATE(C5, 2)                                     13/8/2017 → 13/10/2017
```

Los festivos llevan `$` para que no se desplacen al copiar la fórmula.

Más ejemplos con la fecha 13/8/2017:

| Fórmula | Resultado |
|---|---|
| `=EOMONTH(A1, 0)` | 31/8/2017 |
| `=EOMONTH(A1, 2)` | 31/10/2017 |
| `=EDATE(A1, -1)` | 13/7/2017 |
| `=EDATE(DATE(2017,1,31), 1)` | 28/2/2017 (si el día no existe, toma el último del mes) |

> `EOMONTH(fecha, 2)` aplicado al inicio de un trimestre da el **final del trimestre**.

### El argumento `weekend` de `WORKDAY.INTL`

Puede ser un número o una cadena de 7 caracteres.

| Número | Fin de semana |
|---|---|
| `1` (por defecto) | Sábado y domingo |
| `2` | Domingo y lunes |
| `7` | Viernes y sábado |
| `11` | Solo domingo |
| `17` | Solo sábado |

La cadena empieza en **lunes**; `0` = día laborable, `1` = no laborable. `"0001011"` significa que jueves, sábado y domingo no se trabajan:

```excel
=WORKDAY.INTL(EOMONTH(A1,0), 10, "0001011")   →  18/9/2017
```

### `DATEDIF`

No aparece en el autocompletado de Excel, así que hay que escribirla completa. Si la fecha inicial es posterior a la final, da `#NUM!`.

| `unit` | Devuelve |
|---|---|
| `"Y"` | Años completos |
| `"M"` | Meses completos |
| `"D"` | Días |
| `"YM"` | Meses que sobran sin contar los años |
| `"MD"` | Días que sobran sin contar meses ni años |
| `"YD"` | Días que sobran sin contar los años |

En la hoja `Invoices`, las columnas `Months` y `Days` vienen vacías en el libro. Una forma de calcular cuánto tardó cada pago, sin error en las facturas no pagadas:

```excel
Meses:  =IF(F5="", "", DATEDIF(C5, F5, "M"))     13/8/2017 a 28/11/2017 → 3
Días:   =IF(F5="", "", DATEDIF(C5, F5, "MD"))    → 15
```

## 2. Funciones financieras: `PV`, `FV`, `PMT`

### Una idea común: cada función despeja una variable

| Argumento | Significado |
|---|---|
| `rate` | Tasa **por periodo** (si los pagos son mensuales, tasa anual / 12) |
| `nper` | Número de periodos (**N**), en la misma unidad que `rate` |
| `pmt` | Pago regular y periódico |
| `pv` | Valor presente: el monto medido **hoy** (el *principal* de un préstamo) |
| `fv` | Valor futuro: el saldo que queda o se quiere alcanzar al final. Si se omite, es 0 |
| `type` | `0` = pagos al **final** del periodo (por defecto), `1` = al **inicio** |

| Función | Sintaxis | Responde a |
|---|---|---|
| `FV` | `FV(rate, nper, pmt, [pv], [type])` | ¿Cuánto tendré al final? |
| `PV` | `PV(rate, nper, pmt, [fv], [type])` | ¿Cuánto vale **hoy**? |
| `PMT` | `PMT(rate, nper, pv, [fv], [type])` | ¿Cuánto pago o ahorro **cada periodo**? |

> `PMT` lleva `pv` en tercer lugar, no `pmt`.

### Regla de signos

Siempre desde **tu bolsillo**: dinero que **entra** es positivo, dinero que **sale** es negativo. Antes de escribir la fórmula, pregúntate por cada monto si entra o sale; el signo del resultado dice qué pasa con la cantidad que buscabas.

### Casos del curso (`W4_02_Intro_Financial_Functions.xlsx`)

| Caso | Pregunta | Fórmula | Resultado |
|---|---|---|---|
| Kevin Sanders | Aporta $15,000 al año y $500,000 hoy, 5%, 25 años: ¿cuánto tendrá? | `=FV(5%, 25, -15000, -500000, 0)` | $2,409,084 |
| Xiaoli Fong | Quiere $1,500,000 en 25 años aportando $10,000 al año, 5%: ¿cuánto invertir hoy? | `=PV(5%, 25, -10000, 1500000, 0)` | -$302,015 |
| Pierre Fabre | Invierte $50,000 hoy y quiere $1,000,000 en 30 años, 4%: ¿cuánto ahorrar al año? | `=PMT(4%, 30, -50000, 1000000)` | -$14,939 |

### Qué significa `PV` de una serie de pagos

```excel
=PV(5%, 10, -1000)   →  7,721.73
```

| Argumento | Valor | Significado |
|---|---|---|
| `rate` | `5%` | Tasa anual a la que se "descuenta" el dinero futuro |
| `nper` | `10` | 10 pagos anuales |
| `pmt` | `-1000` | Pago de cada año; negativo porque **tú lo pagas** |
| `fv` | omitido (0) | No hay monto final extra |
| `type` | omitido (0) | Cada pago ocurre al final del año |

El resultado es **cuánto vale hoy** esa serie de pagos. $1,000 que llegan en un año valen hoy $952.38, porque $952.38 al 5% se convierten en $1,000. Sumando cada pago descontado:

| Año | Pago | Valor hoy (`1000 / 1.05^año`) |
|---|---|---|
| 1 | 1,000 | 952.38 |
| 2 | 1,000 | 907.03 |
| 3 | 1,000 | 863.84 |
| 4 | 1,000 | 822.70 |
| 5 | 1,000 | 783.53 |
| 6 | 1,000 | 746.22 |
| 7 | 1,000 | 710.68 |
| 8 | 1,000 | 676.84 |
| 9 | 1,000 | 644.61 |
| 10 | 1,000 | 613.91 |
| **Suma** | **10,000** | **≈ 7,721.73** |

Los pagos suman $10,000, pero hoy valen $7,721.73: es el **valor del dinero en el tiempo**.

### `-1000` frente a `+1000`: de qué lado del trato estás

El número es el mismo; cambia la interpretación.

| Fórmula | Hoy | Años 1 a 10 | Interpretación |
|---|---|---|---|
| `=PV(5%, 10, -1000)` → **+7,721.73** | **Recibes** $7,721.73 | **Pagas** $1,000 cada año | **Préstamo** |
| `=PV(5%, 10, 1000)` → **-7,721.73** | **Pagas** $7,721.73 | **Recibes** $1,000 cada año | **Inversión** |

- Con `-1000`: "Puedo pagar $1,000 al año durante 10 años. ¿Cuánto me pueden **prestar** hoy?"
- Con `+1000`: "Quiero cobrar $1,000 al año durante 10 años. ¿Cuánto tengo que **poner** hoy?"

Son las dos caras del mismo contrato: lo que para ti es un préstamo, para el banco es una inversión.

### Invertir hoy y terminar en 0

> ¿Cuánto debo invertir hoy, al 5% anual, para retirar $1,000 cada año durante 10 años y que al final el saldo sea 0?

```excel
=PV(5%, 10, 1000, 0)   →  -7,721.73
```

La pregunta necesita el **retiro anual**: si no hay retiros (`pmt = 0`) y el saldo final es 0 (`fv = 0`), la inversión es trivialmente 0.

Comprobación (cada año el saldo crece 5% y se retiran $1,000):

| Año | Saldo inicial | Con 5% de interés | Retiro | Saldo final |
|---|---|---|---|---|
| 1 | 7,721.73 | 8,107.82 | -1,000 | 7,107.82 |
| 2 | 7,107.82 | 7,463.21 | -1,000 | 6,463.21 |
| 3 | 6,463.21 | 6,786.37 | -1,000 | 5,786.37 |
| 4 | 5,786.37 | 6,075.69 | -1,000 | 5,075.69 |
| 5 | 5,075.69 | 5,329.48 | -1,000 | 4,329.48 |
| 6 | 4,329.48 | 4,545.95 | -1,000 | 3,545.95 |
| 7 | 3,545.95 | 3,723.25 | -1,000 | 2,723.25 |
| 8 | 2,723.25 | 2,859.41 | -1,000 | 1,859.41 |
| 9 | 1,859.41 | 1,952.38 | -1,000 | 952.38 |
| 10 | 952.38 | 1,000.00 | -1,000 | **0** |

| Lo que quieres saber | Fórmula |
|---|---|
| ¿Cuánto invertir hoy para retirar X cada periodo hasta quedar en 0? | `=PV(rate, nper, X)` |
| ¿Cuánto invertir hoy para tener un monto M al final, sin retiros? | `=PV(rate, nper, 0, M)` |
| ¿Cuánto retirar cada periodo si invierto V hoy y termino en 0? | `=PMT(rate, nper, -V)` |

### Más ejemplos

| Situación | Fórmula | Resultado |
|---|---|---|
| Auto de $20,000 a 4 años, pagos mensuales, 6% anual | `=PMT(6%/12, 4*12, -20000)` | 469.70 |
| El mismo, pagando al inicio de cada mes | `=PMT(6%/12, 48, -20000, 0, 1)` | 467.36 |
| Ahorrar $1,000 al año durante 10 años al 5% | `=FV(5%, 10, -1000)` | 12,577.89 |
| Lo mismo más $5,000 invertidos hoy | `=FV(5%, 10, -1000, -5000)` | 20,722.37 |
| Lo mismo aportando al inicio de cada año | `=FV(5%, 10, -1000, 0, 1)` | 13,206.79 |
| Hipoteca de $400,000 a 30 años, 4.5% anual | `=PMT(4.5%/12, 30*12, -400000)` | 2,026.74 |

En la hipoteca se pagan en total unos $729,627 por un préstamo de $400,000; la diferencia es el interés.

### Variaciones de signos por función (caso Jorgito, mensual)

La regla de signos es **la misma** en las tres funciones; lo que cambia es cuál monto es la incógnita.

**Condiciones para todos los casos:** tasa de **1% mensual** (12% nominal anual, 12.68% efectiva anual) y **12 meses**. Es un valor didáctico: las tasas reales cambian según país, banco y producto. Como la tasa ya es mensual, se escribe `1%` directamente, sin dividir entre 12. "Hoy" es `pv`, "cada mes" es `pmt`, "al final" es `fv`; los pagos son al final de cada mes.

#### `FV(1%, 12, pmt, pv)`: la incógnita es lo que pasa al final

| Hoy (`pv`) | Cada mes (`pmt`) | Resultado al mes 12 | Historia | Jorgito |
|---|---|---|---|---|
| -5,000 | -1,000 | **+18,316.63** | Ahorro | Jorgito abre una cuenta con $5,000 y deposita $1,000 cada mes. Al mes 12 retira $18,316.63. |
| +5,000 | -1,000 | **+7,048.38** | Préstamo pagado de más | Jorgito recibe un préstamo de $5,000 y paga $1,000 cada mes. Como paga mucho más de lo que debe, al mes 12 le devuelven $7,048.38. |
| -5,000 | +1,000 | **-7,048.38** | Retiro excesivo | Jorgito deposita $5,000 y retira $1,000 cada mes. Saca más de lo que tenía y al mes 12 debe $7,048.38. |
| +5,000 | +1,000 | **-18,316.63** | Deuda sin cuotas | Jorgito recibe $5,000 hoy y $1,000 cada mes sin pagar nada. Al mes 12 devuelve $18,316.63 de una sola vez. |

#### `PV(1%, 12, pmt, fv)`: la incógnita es lo que pasa hoy

| Cada mes (`pmt`) | Al final (`fv`) | Resultado hoy | Historia | Jorgito |
|---|---|---|---|---|
| -1,000 | -5,000 | **+15,692.32** | Préstamo con pago final | Jorgito se compromete a pagar $1,000 cada mes y $5,000 más en el mes 12. Hoy le prestan $15,692.32. |
| -1,000 | +5,000 | **+6,817.83** | Compensación hoy | Jorgito acepta pagar $1,000 cada mes y cobrar $5,000 en el mes 12. Como paga más de lo que cobra, hoy recibe $6,817.83. |
| +1,000 | -5,000 | **-6,817.83** | Inversión con pago final (poco habitual) | Jorgito entrega hoy $6,817.83, cobra $1,000 cada mes y en el mes 12 paga $5,000. |
| +1,000 | +5,000 | **-15,692.32** | Inversión tipo bono | Jorgito invierte hoy $15,692.32, cobra $1,000 cada mes y $5,000 en el mes 12. |

#### `PMT(1%, 12, pv, fv)`: la incógnita es lo que pasa cada mes

| Hoy (`pv`) | Al final (`fv`) | Resultado mensual | Historia | Jorgito |
|---|---|---|---|---|
| +10,000 | -5,000 | **-494.24** | Préstamo *balloon* | Jorgito recibe $10,000 prestados, paga $494.24 cada mes y liquida $5,000 en el mes 12. |
| +10,000 | +5,000 | **-1,282.73** | Recibe en las dos puntas | Jorgito recibe $10,000 hoy y $5,000 en el mes 12, y lo devuelve con cuotas de $1,282.73. |
| -10,000 | +5,000 | **+494.24** | Inversión con renta | Jorgito invierte $10,000, cobra $494.24 cada mes y recupera $5,000 en el mes 12. |
| -10,000 | -5,000 | **+1,282.73** | Aporta en las dos puntas | Jorgito pone $10,000 hoy y $5,000 en el mes 12, y cobra $1,282.73 cada mes. |

#### El signo del resultado no siempre es "el contrario"

Si las dos entradas tienen **signos opuestos**, el signo del resultado depende de los **montos**. El límite aquí es 11,268.25, lo que crecen solos 10,000 en 12 meses al 1%.

| Fórmula | Resultado mensual | Hoy / meta | Historia | Jorgito |
|---|---|---|---|---|
| `=PMT(1%,12,-10000,5000)` | **+494.24** | Pagas 10,000 / meta 5,000 | La meta es menor que el límite | Jorgito invierte $10,000 y solo necesita $5,000 al final, así que puede retirar $494.24 cada mes. |
| `=PMT(1%,12,-10000,20000)` | **-688.49** | Pagas 10,000 / meta 20,000 | La meta es mayor que el límite | Jorgito invierte $10,000 pero quiere $20,000, así que debe aportar $688.49 cada mes. |

Si las dos entradas tienen **el mismo signo**, el resultado siempre sale con el signo contrario.

#### Qué no es posible

- En `FV`, `PV` y `PMT`, **ninguna combinación de signos da error**: Excel siempre despeja un número, aunque la historia sea poco realista.
- En `NPER` y `RATE`, que reciben los tres montos, puede no haber solución. Se nota con `#NUM!` o con un resultado sin sentido (plazo negativo). En `NPER` la incógnita son los meses, así que ahí no se fijan los 12.

| Fórmula | Resultado | Entradas | Historia | Jorgito |
|---|---|---|---|---|
| `=NPER(1%,-1000,-5000,-20000)` | -27.33 | Solo pagos | Sin sentido | Jorgito solo entrega dinero y nunca recibe; Excel responde con meses negativos. |
| `=RATE(12,-1000,-5000,-20000)` | `#NUM!` | Solo pagos | Sin solución | Ninguna tasa hace que a Jorgito le cuadre un trato en el que solo paga. |
| `=NPER(1%,50,-10000)` | `#NUM!` | Pagas 10,000 / recibes 50 | Nunca llega a 0 | Jorgito invierte $10,000, gana $100 al mes y retira $50: su saldo nunca se agota. |
| `=NPER(1%,1000,10000)` | -9.58 | Solo cobros | Sin sentido | Jorgito recibe $10,000 y además $1,000 cada mes, sin deber nada al final; el plazo sale negativo. |
| `=NPER(1%,-1000,-5000,20000)` | 13.42 | Pagas / recibes | Correcto | Jorgito deposita $5,000 y $1,000 cada mes; llega a $20,000 en 13.42 meses (en la práctica, en el mes 14). |

`type` no es un signo (es 0 o 1). `rate` puede ser negativa (tasas negativas); `nper` negativo no tiene sentido práctico.

### Errores típicos

- Tasa anual con periodos mensuales, o al revés.
- Poner todos los montos con el mismo signo.
- Saltarse un argumento sin dejar su coma: en `PMT(rate, nper, pv, 1)` el `1` es `fv`; para indicar `type` es `PMT(rate, nper, pv, , 1)`.
- Olvidar que `type` es 0 si se omite.

### Relacionadas

`NPER` y `RATE` despejan las otras dos variables. Por ejemplo, `=NPER(5%, -10000, -300000, 1500000)` da 25.08 años.

## 3. Calendario de préstamo (*loan schedule*)

Tabla de **amortización**: detalla cada pago, cuánto es interés, cuánto reduce la deuda y qué saldo queda. Cada pago cubre primero el interés y el resto amortiza. Además permite registrar **pagos adicionales**.

**Datos del libro:** préstamo de $200,000, 7% anual, 15 años, pagos mensuales, primer pago el 23/10/2017.

Cada dato tiene un nombre, creado con **Create from Selection → Left column** (`Ctrl + Shift + F3`). Un nombre apunta siempre a la misma celda, así que las fórmulas se copian sin necesidad de `$`.

### Celdas de resumen

| Celda | Fórmula | Qué calcula |
|---|---|---|
| `C12` | `=IF(C10="Monthly",12,1)` | Periodos por año |
| `G7` (`Rate`) | `=C8/C12` | Tasa por periodo (0.58%) |
| `C13` | `=C9*C12` | Número de pagos (180) |
| `C15` (`Monthly_Payment`) | `=-1*PMT(G7,C13,C7)` | Cuota: $1,797.66 |
| `G8` | `=C15*C13-C7` | Interés proyectado sin pagos extra |
| `G10` | `=SUM(E20:E269)` | Interés realmente pagado |
| `G11` | `=G8-G10` | Ahorro de interés |

El monto del préstamo va en el argumento `pv`. `PMT` sale negativo porque Excel trata los pagos como salidas de dinero; el `-1*` lo muestra en positivo.

### Columnas de la tabla (fila 21)

| Columna | Fórmula | Idea |
|---|---|---|
| Fecha | `=IF(G20<=0,"",EDATE(B20,IF($C$10="Monthly",1,12)))` | Suma 1 o 12 meses; vacío si la deuda ya se pagó |
| Pago | `=IF(G20+E21<Monthly_Payment,G20+E21,Monthly_Payment)` | La cuota, o solo lo que falta en el último pago |
| Pago adicional | valor manual | Se suma a la amortización |
| Interés | `=G20*Rate` | Saldo anterior × tasa por periodo |
| Principal | `=C21-E21+D21` | Pago − interés + extra |
| Saldo | `=G20-F21` | Saldo anterior − principal |

### Qué pasa en el libro

- El pago 180 (23/9/2032) es de **$381.44** y deja el saldo en 0.
- Con un pago extra de $500 en el pago 1, el interés total baja de $123,578.18 a $122,661.97: **ahorro de $916.21**. Siguen siendo 180 pagos; el extra solo reduce el último.
- Al cambiar la frecuencia a `Annual`, la cuota pasa a $21,958.92 y la siguiente fecha es 23/10/2018.

## 4. `NPV` e `IRR`

| Función | Responde a |
|---|---|
| `NPV(rate, value1, [value2], ...)` | ¿Cuánto valen **hoy** estos flujos futuros? |
| `IRR(values, [guess])` | ¿Qué tasa hace que el `NPV` sea **0**? (rendimiento de la inversión) |

`NPV` es como `PV`, pero admite un monto **distinto** en cada periodo: descuenta cada flujo con `flujo / (1+i)^periodo` y los suma.

### La inversión inicial va fuera de `NPV`

```excel
=NPV(Discount_rate, C7:C9) + C6    →  270,672.90   (correcto)
=NPV(Discount_rate, C6:C9)         →  259,017.13   (incorrecto)
```

`NPV` supone que el primer valor llega **al final del periodo 1**; la inversión ocurre al inicio y no debe descontarse. En `IRR`, en cambio, se seleccionan **todos** los flujos, incluida la inversión.

El libro lo comprueba además a mano: `=B16/(1+Discount_rate)^A16` por año, y luego la suma más la inversión.

### Ejemplo con Jorgito

Jorgito invierte $10,000 y cobra $4,000 al año durante 3 años.

| Cálculo | Resultado | Lectura |
|---|---|---|
| `NPV` al 5% | +892.99 | Le conviene |
| `NPV` al 10% | -52.59 | No le conviene |
| `IRR` | 9.70% | La tasa donde pasa de convenir a no convenir |

### Comparar dos opciones (tasa 4.5%)

| | Inversión | Flujos (años 1 a 3) | `NPV` | `IRR` |
|---|---|---|---|---|
| Opción 1 | $2,000,000 | 600k, 800k, 1.1M | **$270,673** | 10.85% |
| Opción 2 | $1,000,000 | 420k, 360k, 500k | $169,725 | **13.01%** |

Aquí **no coinciden**: `IRR` mide el rendimiento porcentual y `NPV` cuánto dinero se gana, que depende del tamaño. Si las opciones son excluyentes y hay capital para ambas, manda el `NPV`: el millón adicional de la opción 1 rinde 8.94%, más que el 4.5% que cuesta. Con una tasa de 12% la elección se invierte (-43,572 frente a +17,880).

### Reglas de decisión

- Un solo proyecto: aceptarlo si `NPV > 0`, o si `IRR` > costo de capital.
- Varios proyectos con **la misma inversión**: preferir mayor `NPV` / mayor `IRR`. Si aun así discrepan (por el calendario de los flujos), decidir con `NPV`.

### Cuidados

- Tasa y flujos con la **misma periodicidad**; intervalos regulares.
- Un periodo sin flujo se escribe como `0`: una celda vacía se ignora y corre los periodos siguientes.
- `IRR` necesita al menos un flujo positivo y uno negativo.
- `guess` es solo el punto de partida del cálculo (10% si se omite).
- Solo los flujos **convencionales** (un único cambio de signo) garantizan una sola `IRR`. Con `-100, 230, -132` hay dos: 10% y 20%, según el `guess`.

## 5. Depreciación

La **depreciación** es la reducción de valor de un activo con el tiempo. Refleja su valor razonable y se registra como gasto, lo que reduce impuestos.

| Término | Significado |
|---|---|
| `cost` | Lo que se pagó por el activo |
| `salvage` | **Valor residual** estimado al final de su vida útil |
| `life` | Años de vida útil |
| `period` | Año del que se quiere la depreciación |

| Función | Método | Cómo reparte |
|---|---|---|
| `SLN(cost, salvage, life)` | Línea recta | Igual cada año: `(cost − salvage) / life` |
| `SYD(cost, salvage, life, period)` | Suma de dígitos | Acelerado: con 10 años, 10/55, 9/55, ... 1/55 de `(cost − salvage)` |
| `DDB(cost, salvage, life, period, [factor])` | Doble saldo decreciente | Acelerado: el doble de la tasa lineal sobre el **valor que queda** |

### El caso del libro: muebles de $20,000, residual $3,000, 10 años

```excel
=SLN(Cost, Salvage, Life)
=SYD(Cost, Salvage, Life, B14)
=DDB(Cost, Salvage, Life, B14)
```

| Año | `SLN` | `SYD` | `DDB` |
|---|---|---|---|
| 1 | 1,700 | 3,090.91 | 4,000 |
| 2 | 1,700 | 2,781.82 | 3,200 |
| 3 | 1,700 | 2,472.73 | 2,560 |
| 8 | 1,700 | 927.27 | 838.86 |
| 9 | 1,700 | 618.18 | 355.44 |
| 10 | 1,700 | 309.09 | 0 |
| Valor final | 3,000 | 3,000 | 3,000 |

- El denominador de `SYD` (1+2+...+10 = 55) está en `L14`.
- `DDB` nunca baja del valor residual: en el año 9 solo deprecia lo que falta y en el 10 ya no queda nada.
- Valor del activo = valor al inicio del año − depreciación del año (filas 23 a 25).
- Las fechas de cada año se generan con `=EDATE(B15,12)`.

### Ejemplo con Jorgito

Jorgito compra una laptop de $1,000 que valdrá $100 en 3 años.

| Método | Año 1 | Año 2 | Año 3 |
|---|---|---|---|
| `SLN` | 300 | 300 | 300 |
| `SYD` (3/6, 2/6, 1/6) | 450 | 300 | 150 |
| `DDB` (66.7% del saldo) | 666.67 | 222.22 | 11.11 |

Los tres suman $900; solo cambia **cuándo** se registra la pérdida.

## 6. Atajos, terminología y ninja tips

### Atajos de teclado

| Atajo | Acción |
|---|---|
| `Ctrl + ;` | Fecha de hoy como valor **fijo**; `TODAY()` es volátil y se recalcula con cada cambio |

> El Toolbox da `Cmd + ;` para Mac; según la documentación de Microsoft la fecha es `Control + ;` y `Cmd + ;` corresponde a la hora. Conviene probarlo.

### Terminología

| Término | Significado |
|---|---|
| *Principal* | La parte del pago que reduce la deuda una vez descontado el interés. También se llama así al monto prestado |
| *Present Value* | Monto invertido o prestado hoy; valor hoy de una serie de flujos |
| *Future Value* | Cuánto valdrá una inversión, o el saldo residual de un préstamo |
| `NPER` | Número de periodos: 4 años mensuales = 48 |
| `PMT` | Pago regular; Excel lo supone constante |
| `type` | `0` al final del periodo (cada 31/12), `1` al inicio (cada 1/1) |
| *Sign convention* | Entradas positivas, salidas negativas |
| *Capital budgeting* | Técnicas para elegir entre proyectos cuando los recursos son escasos |
| *Cost of capital* | Rendimiento que exigen los accionistas por su dinero |

### Ninja tips

- **Interés** (un monto, p. ej. $200) no es lo mismo que **tasa de interés** (p. ej. 6%).
- **Tasa anualizada (nominal) ≠ tasa efectiva anual.**

| Si tienes | Obtienes | Cálculo |
|---|---|---|
| 2% efectivo mensual | 24% nominal anual | `2% * 12` |
| 2% efectivo mensual | 26.82% efectivo anual | `=(1+2%)^12-1` |
| 12% efectivo anual | 0.9489% efectivo mensual | `=(1+12%)^(1/12)-1` |
| 12% nominal anual | 1% mensual | `12%/12` |

  En general, `i = (1 + r)^(1/n) − 1`, con `n` periodos por año.
- **Flujos convencionales:** un solo cambio de signo; son los únicos con `IRR` única garantizada.

### Dos precisiones sobre el Toolbox

- En `WORKDAY.INTL`, la cadena `"0001011"` define los **días de fin de semana**, no los festivos. Los festivos van en el cuarto argumento.
- La unidad `"MD"` de `DATEDIF` puede dar resultados erróneos en algunos casos; la propia documentación de Microsoft desaconseja usarla sin revisarla.

## 7. Otros temas relacionados (no vistos directamente en el curso esta semana)

> Sección complementaria, agregada con conocimiento general para ampliar la documentación — no proviene de un video/lectura específico de este módulo.

- **`NETWORKDAYS.INTL`**: cuenta los días hábiles entre dos fechas (lo inverso de `WORKDAY.INTL`). Del 13/8/2017 al 28/11/2017 hay 77, con sábado y domingo libres y sin festivos.
- **`IPMT` y `PPMT`**: interés y principal de un pago concreto sin armar la tabla. En el préstamo del libro, pago 1: `=IPMT(7%/12,1,180,200000)` → -1,166.67 y `=PPMT(7%/12,1,180,200000)` → -630.99.
- **`CUMIPMT` / `CUMPRINC`**: interés o principal acumulado entre dos pagos. `=CUMIPMT(7%/12,180,200000,1,12,0)` → -13,752.28 de interés el primer año (sin pagos extra).
- **`EFFECT` / `NOMINAL`**: convierten entre tasa nominal y efectiva. `=EFFECT(12%,12)` → 12.68%.
- **`XNPV` / `XIRR`**: como `NPV` e `IRR`, pero con **fechas** para cada flujo, cuando los intervalos son irregulares.
- **`MIRR`**: `IRR` modificada, con tasas separadas para financiar y reinvertir; evita las soluciones múltiples.
- **`VDB`**: saldo decreciente que cambia a línea recta cuando conviene, para llegar al valor residual. `DDB` no siempre llega: con $2,000 de residual en el caso de los muebles, queda en $2,147.48.
- **`DB`**: saldo decreciente con tasa fija calculada a partir del costo y el residual.

## Práctica

- Libros en `material_curso/modulo-04/` (lista en el *Mapa de la semana*).
- Esta semana **no hay challenge**.
- [Assessment final de la semana](assessment.md): habilidades evaluadas.
- Ideas para repasar: cambiar los datos del calendario de préstamo (frecuencia y pagos extra) y rehacer las tablas de Jorgito con otra tasa.

## Fuente

Flashcards de las cinco clases (*Working with Dates*, *Financial Functions*, *Loan Schedule*, *NPV and IRR*, *Depreciation Functions*), el Toolbox de la semana 4, los libros del curso (revisados en copias) y las preguntas hechas durante el estudio.
