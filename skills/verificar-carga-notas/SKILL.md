---
name: verificar-carga-notas
description: Verificar que una carga de notas en la planilla de cursada (hecha con cargar-notas-planilla o a mano) coincida con la lista original legajo-nota, sin modificar nada.
---

# Verificar la carga de notas

Usar cuando la persona pide revisar, controlar o verificar que las notas cargadas en su planilla (parciales, recuperatorios, integrador, final) estén bien. Complementa a la skill `cargar-notas-planilla`.

## Regla principal: solo informar

Esta skill **no modifica la planilla**. No corrige notas, no pinta celdas, no borra ni crea hojas. Solo lee, compara e informa. Si encuentra errores, los lista y pregunta a la persona si quiere corregirlos (la corrección se hace aparte, con su confirmación).

Si para comparar hace falta una fórmula temporal en Google Sheets, preferir leer los datos (texto de la página, exportar/descargar una copia, o leer el archivo local). Si no queda otra que usar una celda temporal, usar una celda vacía fuera de la tabla, avisar a la persona y borrarla al terminar, verificando que quedó vacía.

## Qué se necesita

1. **La lista original** de notas (pares `legajo nota`), tal como la recibió la persona: pegada en el chat, en un archivo (CSV, Excel, PDF) o en una hoja. Sin la lista original no se puede verificar nota por nota: decirlo y ofrecer solo los controles de estructura (controles 2, 5 a 8).
2. **La planilla** (Google Sheets abierta en el navegador, o archivo .xlsx en la computadora).
3. **La columna verificada** (ej.: "primer parcial", "segundo parcial", "Final"). Si no la dice, preguntar.

## Normalizar la lista original

Aplicar las mismas reglas que `cargar-notas-planilla`:
- Separar legajo y nota aunque haya espacios al inicio, espacios no separables o tabulaciones.
- Decimales con coma: `3,50` = 3,5.
- Cero adelante: `03,05` = 3,05; `06,05` = 6,05.
- Si una línea no se puede interpretar (sin nota, texto raro), anotarla como **dato de origen dudoso**, no adivinar.

## Controles

1. **Nota por nota**: para cada legajo de la lista original, buscar la fila con ese legajo en la planilla y comparar con la nota de la columna verificada. Comparar como número, con tolerancia 0,001 (la planilla puede mostrar 1 decimal pero guardar 3,05: verificar el valor guardado, no el que se ve).
2. **Alumnos sin nota**: filas de la planilla con legajo pero con la columna verificada vacía.
3. **Legajos sobrantes**: legajos de la lista original que no están en la planilla.
4. **Duplicados**: legajos repetidos en la lista original (y si tienen notas distintas) o en la planilla.
5. **Columna correcta**: que el encabezado de la columna verificada coincida con lo pedido, y que las columnas vecinas (ej. Rec PP, segundo parcial) no tengan notas que no deberían estar.
6. **Notas fijas**: que las celdas de la columna verificada contengan números y no fórmulas (en Google Sheets, revisar en la barra de fórmulas una muestra: primera fila, última fila y algunas del medio; en Excel, leer con openpyxl sin `data_only` y detectar valores que empiezan con `=`). Que no quede ninguna hoja auxiliar de la carga (ej. "Aux notas", "Hoja 1") si la persona dijo que no la quería.
7. **Valores fuera de rango**: notas menores que 0 o mayores que 10, o celdas con texto.
8. **Fórmulas intactas**: las columnas calculadas (Nota final PP, Nota final SP, Nota Final) siguen teniendo fórmula y no muestran errores (#REF!, #N/A, #VALUE!).

## Con un archivo Excel local (.xlsx)

Leer con Python (openpyxl o pandas), sin modificar el archivo: una lectura con fórmulas (para los controles 6 y 8) y otra con `data_only=True` (para valores). Hacer los controles 1 a 8 en código.

## Informe (en el chat)

Entregar un resumen corto y claro, en este orden:

1. **Resultado general**: "✅ Todo correcto" o "⚠️ Se encontraron N problemas".
2. **Números**: alumnos en la planilla, notas en la lista original, notas que coinciden.
3. **Problemas**, agrupados por tipo, con **nombre, legajo, fila**, lo que dice la lista y lo que dice la planilla. Ejemplo:
   - Nota distinta: Boratto, Santiago (68900, fila 20) — lista: 3,05 / planilla: 3,5
   - Sin nota: Muro, Tomás (69053, fila 127)
   - Legajo no encontrado en la planilla: 61234 (nota 7,00)
4. **Aclaraciones**: diferencias que no son errores (ej.: una nota cargada a mano porque no estaba en la lista original, como confirmó la persona).
5. Terminar preguntando si quiere que se corrijan los problemas encontrados. No corregir sin un "sí" explícito.

## Columnas habituales en NOTAS CURSADA (hoja Cursada)

No. | Comisión | Legajo | Alumno | dni | primer parcial | Rec PP | Nota final PP | segundo parcial | Rec Integrador | Nota final SP | Nota quices | Nota Final | comentarios | Final SGA ...
