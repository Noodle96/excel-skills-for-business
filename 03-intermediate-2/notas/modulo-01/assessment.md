# Assessment del Módulo 1 (Data Validation) — habilidades a dominar

Archivo: `material_curso/modulo-01/assessment/C3-W1-Assessment.xlsx`. Este documento **no guarda respuestas**: toma cada pregunta del assessment como una **habilidad** que hay que saber ejecutar sin ayuda, con el procedimiento para hacerla, para poder repasarla y practicarla después.

**Datos de la práctica**: una hoja con 60 trabajos de una empresa en tres estados (NSW, QLD y VIC), con las columnas *Job number, Customer company name, Job start date, Estimated finish date, Project cost, Markup percentage, Quoted price to the customer, Number of people* y *State*, y una zona de cálculo a la derecha de la tabla.

## Habilidades que pedía el assessment

| # | Qué se pedía | Habilidad |
|---|---|---|
| 1 | Encontrar en qué columna hay validación de datos | Localizar celdas con validación |
| 2 | Encontrar y corregir una celda con un dato no válido | Detectar datos que incumplen una regla |
| 3 | Encontrar una celda con validación innecesaria y quitarla | Quitar validación sobrante |
| 4 | Lista desplegable de estados y fórmula con el número de trabajos del estado elegido | Lista desplegable + fórmula que depende de ella |
| 5 | Otra lista de estados (desde cero o copiando la anterior) y fórmula con la ganancia total del estado elegido | Copiar solo la validación + fórmula con criterios |
| 6 | Lista de números del 1 al 9 y formato condicional que cuente/resalte los trabajos con más personas que el número elegido | Validación + formato condicional con fórmula |
| 7 | Evitar números de trabajo duplicados y comprobar si ya había alguno | Validación personalizada con fórmula |
| 8 | Validar un porcentaje de recargo dentro de un rango | Elegir el tipo de criterio correcto |
| 9 | Validar que una fecha de inicio sea posterior a un límite | Elegir el tipo de criterio correcto |
| 10 | Copiar la validación de una columna de fechas a otra y revisar qué datos la incumplen | Copiar validación con Pegado especial y revisar el resultado |

## Cómo se hace cada una

### Encontrar, revisar y quitar validación (1-3)
1. **Encontrar las celdas con validación**: Inicio > Buscar y seleccionar > **Validación de datos**. Quedan seleccionadas todas las celdas de la hoja con alguna regla.
2. **Detectar datos que incumplen la regla**: Datos > Validación de datos (flecha) > **Rodear con un círculo datos no válidos**. Las celdas con un valor fuera de la regla quedan marcadas con un círculo rojo; se corrigen escribiendo un valor permitido y luego se usa **Borrar círculos de validación**.
3. **Quitar validación sobrante**: la que está fuera de la tabla principal no tiene motivo de existir. Se selecciona **solo esa celda** y Datos > Validación de datos > **Borrar todos**. Se comprueba repitiendo el paso 1.

### Lista desplegable + fórmula que depende de ella (4)
- Celda de la lista: Datos > Validación de datos > Permitir: **Lista** > Origen con los valores separados por coma (`;` si la coma es separador decimal).
- Fórmula de conteo con criterio, apuntando a la **celda de la lista**, no a un texto fijo:
  ```
  =COUNTIF(rango_State, celda_de_la_lista)
  ```

### Copiar la validación + ganancia por estado (5)
- Para reutilizar una regla: copiar la celda y en el destino **Pegado especial > Validación**; así no se pega el valor, solo la regla. (También sirve crearla desde cero.)
- La ganancia de cada trabajo es **precio cotizado − costo del proyecto**, así que para un grupo se restan dos `SUMIFS`:
  ```
  =SUMIFS(Quoted_price, State, celda_de_la_lista) - SUMIFS(Project_cost, State, celda_de_la_lista)
  ```
- Cuidado con `SUMIFS`: el rango a sumar va **primero** y los rangos deben tener el mismo tamaño.

### Lista + formato condicional (6)
- Lista con los números **1, 2, 3, …, 9** (Origen `1,2,3,4,5,6,7,8,9`).
- En la columna de personas: Formato condicional > Nueva regla > resaltar celdas **mayores que** la celda de la lista, con referencia **absoluta** (`$N$14`) para que todas se comparen contra la misma celda.
- Al cambiar el número de la lista, el resaltado se actualiza solo; contar las celdas resaltadas da el resultado.

### Evitar duplicados con validación personalizada (7)
- En la columna: Permitir **Personalizada** con
  ```
  =COUNTIF($B$2:$B$61,B2)=1
  ```
  (ajustando el rango a donde estén los datos). Solo acepta un valor si aparece una vez.
- **La regla no corrige datos que ya estaban**: para ver si había duplicados se usa **Rodear con un círculo datos no válidos**.

### Elegir el tipo de criterio (8-9)
- Un porcentaje con decimales dentro de un rango → **Decimal / entre**. *Número entero* rechazaría valores intermedios y *Cualquier valor* no valida nada.
- Una fecha posterior a un límite → **Fecha / mayor que**. *Menor que* sería lo contrario y *entre* necesita dos fechas.

### Copiar validación a otra columna y revisar (10)
1. Copiar la columna origen.
2. Seleccionar la columna destino > **Pegado especial > Validación**.
3. **Rodear con un círculo datos no válidos** para ver qué celdas incumplen la regla heredada.

## Gotchas

- **La validación solo controla lo que se escribe.** No corrige datos anteriores ni lo que se pega; por eso el círculo de datos no válidos es la herramienta de revisión.
- **Para borrar la validación de una sola celda hay que seleccionar solo esa celda**; de lo contrario se borra también la de las vecinas del rango.
- **En `SUMIFS` el rango a sumar va primero**, a diferencia de la antigua `SUMIF`.
- **El criterio de las fórmulas es la celda de la lista**, no el texto de una opción, para que el resultado cambie al elegir otra.
- **Referencias en las reglas**: absolutas (`$N$14`, `$B$2:$B$61`) para lo que debe quedar fijo, relativas (`B2`) para la celda que se evalúa.

## Checklist: lo que debo ser capaz de hacer sin ayuda

- [ ] Encontrar todas las celdas con validación de datos de una hoja
- [ ] Rodear con un círculo los datos que incumplen la regla y corregirlos
- [ ] Quitar la validación de una celda concreta sin afectar a las demás
- [ ] Crear una lista desplegable desde cero (valores escritos, rango o rango con nombre)
- [ ] Copiar solo la validación de una celda a otra con Pegado especial
- [ ] Escribir una fórmula de conteo o suma que dependa de una lista desplegable
- [ ] Calcular una ganancia como diferencia entre dos `SUMIFS`
- [ ] Aplicar un formato condicional con fórmula que compare contra otra celda
- [ ] Evitar duplicados con una validación personalizada (`COUNTIF`)
- [ ] Elegir el tipo de criterio correcto (decimal, entero, fecha, lista, longitud, personalizada)
- [ ] Reutilizar una regla en otra columna y revisar qué datos quedan fuera de ella
