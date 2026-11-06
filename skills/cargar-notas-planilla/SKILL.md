---
name: cargar-notas-planilla
description: Cargar notas de estudiantes (parciales, recuperatorios, finales) en una planilla de cursada, buscando por legajo a partir de una lista pegada tipo 'legajo nota'.
---

# Cargar notas en la planilla de cursada

Usar cuando la persona pide cargar notas (primer parcial, segundo parcial, recuperatorios, integrador, final, etc.) en su planilla de notas (Google Sheets o Excel), a partir de una lista de pares **legajo + nota**.

## Antes de empezar

1. Confirmar (o deducir de la planilla):
   - **Archivo y hoja destino** (ej.: NOTAS CURSADA > hoja "Cursada").
   - **Columna destino** (ej.: "primer parcial", "Rec PP", "segundo parcial", "Final"). Leer los encabezados de la fila 1 y elegir la que coincida con lo pedido; si hay duda, preguntar.
   - **Columna de legajo** en la hoja destino (habitualmente C, "Legajo").
   - **De dónde vienen las notas**: texto pegado en el chat, otra hoja, un CSV/Excel.
2. Revisar que la columna destino esté vacía o avisar si ya tiene notas antes de sobrescribir.

## Formato de los datos de origen

Las notas suelen venir como texto en una sola celda o línea: `64691 3,50`. Tener en cuenta:
- Puede haber espacios al inicio, espacios no separables o tabulaciones entre legajo y nota.
- Decimales con **coma** (3,50).
- Notas con cero adelante: `03,05` = **3,05**, `06,05` = **6,05**. Tratarlas siempre como el número sin el cero inicial.

## Procedimiento en Google Sheets

La planilla está en configuración regional en español: separador de argumentos `;`, decimales con `,`.

1. **Hoja auxiliar**: crear una hoja temporal (ej. "Aux notas") y pegar los pares en la columna A.
2. **Separar legajo y nota** en B1 de la hoja auxiliar (robusto a espacios raros):
   `=ARRAYFORMULA(IF(A1:A="";"";SPLIT(TRIM(REGEXREPLACE(TO_TEXT(A1:A);"[^0-9,]+";" "));" ")))`
   → B = legajo, C = nota (numérica).
3. **Buscar la nota por legajo** en la hoja destino, en la fila 2 de la columna destino, y copiar hacia abajo hasta la última fila con alumnos:
   `=IFERROR(VLOOKUP(C2;'Aux notas'!$B:$C;2;0);"")`
   - No usar ARRAYFORMULA en la hoja destino si hay otras columnas con fórmulas que dependan de ella (dio #REF!). Mejor fórmula por celda.
   - Verificar que se haya llenado hasta la **última fila** (la carga por copiar/pegar a veces se queda corta).
4. **Controles** (con fórmulas temporales, luego borrarlas):
   - Alumnos sin nota: `=IFERROR(TEXTJOIN(",";1;FILTER(Cursada!D2:D199;Cursada!F2:F199=""));"-")` (ajustar columnas/rango).
   - Legajos del origen que no están en la planilla: `COUNTIF(Cursada!C:C;legajo)=0`.
   - Legajos duplicados en el origen: `COUNTIF(B:B;B1)>1`.
   - Cantidad cargada: `COUNT(columna destino)` vs. cantidad de alumnos.
5. **Informar** a la persona: cuántas notas se cargaron, quiénes quedaron sin nota (nombre y legajo) y cualquier legajo sobrante o duplicado. Pedir las notas faltantes y cargarlas a mano en la celda del alumno.
6. **Dejar las notas fijas**: seleccionar el rango de la columna destino (usar el *cuadro de nombre*, ej. `F2:F199`), copiar y **pegar solo valores** (Cmd/Ctrl+Shift+V). Verificar en la barra de fórmulas que una celda arriba y una abajo muestren un número, no una fórmula, y que la suma de la columna no cambió.
7. **Eliminar la hoja auxiliar** solo después de confirmar con la persona.

## Consejos de manejo en el navegador

- En Mac los atajos son con **Cmd** (cmd+c, cmd+shift+v, cmd+a).
- Para ir a un rango o celda, usar el **cuadro de nombre** (arriba a la izquierda): seleccionar su texto con cmd+a, escribir el rango y Enter.
- Activar Ver > Mostrar > Barra de fórmulas para verificar contenido.
- Después de cada pegado, revisar con una captura que se pegó lo correcto en el lugar correcto; si no, deshacer y corregir celda por celda.

## Procedimiento con un archivo Excel local (.xlsx)

Si el archivo está en la computadora de la persona: leerlo con Python (openpyxl/pandas), armar un diccionario legajo→nota a partir del texto de origen (aplicando las mismas reglas: limpiar espacios, coma decimal, ceros iniciales), escribir los valores numéricos en la columna destino según el legajo, guardar una copia nueva al lado del original (salvo que pida reemplazarlo) e informar faltantes, sobrantes y duplicados.

## Columnas habituales en NOTAS CURSADA (hoja Cursada)

No. | Comisión | Legajo | Alumno | dni | primer parcial | Rec PP | Nota final PP | segundo parcial | Rec Integrador | Nota final SP | Nota quices | Nota Final | comentarios | Final SGA ...

Las columnas "Nota final PP", "Nota final SP" y "Nota Final" son fórmulas: **no** escribir en ellas; se actualizan solas al cargar los parciales.
