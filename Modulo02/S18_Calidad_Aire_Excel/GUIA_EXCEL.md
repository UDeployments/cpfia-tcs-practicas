# Procedimiento en Excel

Este documento describe cómo se construyó en Excel el análisis de calidad del aire, de la etapa 2 a la 8 del proceso de la Sesión 18, para que cualquier persona pueda replicarlo. Las etapas 1 y 2 en Python están en `notebooks/01_preparacion_datos.ipynb`.

Los menús corresponden a Excel en español para Mac. En equipos que separan los argumentos con `;`, las comas de las fórmulas se reemplazan por punto y coma.

**Requisito:** haber ejecutado `notebooks/01_preparacion_datos.ipynb`, que genera `data/processed/calidad_aire_mexico_limpio.xlsx`.

**Capturas para el reporte:** en los puntos marcados con 📷 se toma una captura de pantalla de la ventana de Excel y se guarda en `outputs/capturas/` con el nombre indicado, en `.png` o `.jpg`. El notebook `02_reporte_pdf.ipynb` busca las capturas por ese nombre exacto. En Mac: `Cmd + Shift + 4`, luego `Espacio` y clic sobre la ventana.

---

## Etapa 1 · Datos crudos

El archivo original de la OMS, `data/raw/who_ambient_air_quality_database_v8_2026.xlsx`, se abrió en Excel para revisar su estructura. Cada fila es un texto separado por comas dentro de la columna A, y las filas largas continúan en las columnas B, C, D... Al seleccionar una celda de la columna A, la barra de fórmulas muestra el registro completo. Con esta estructura no es posible filtrar ni resumir los datos, por eso la limpieza se hizo en Python.

> 📷 **Captura `01_datos_crudos.png`:** el archivo original abierto, con una celda de la columna A seleccionada y su texto en la barra de fórmulas. Después se cierra sin guardar.

## Etapa 2 · Datos limpios

1. Abrir `data/processed/calidad_aire_mexico_limpio.xlsx`.
2. **Archivo → Guardar como** y guardar como `outputs/S18_Calidad_Aire_Mexico.xlsx`, en formato Libro de Excel. Todo el análisis se hace en este archivo.
3. Verificar que la hoja `Datos` tenga 857 filas y 11 columnas: `Anio`, `Estado`, `Municipio`, `Region`, `PM25`, `PM10`, `NO2`, `Estaciones`, `Poblacion`, `Latitud` y `Longitud`.

> 📷 **Captura `02_datos_limpios.png`:** la hoja `Datos` de `S18_Calidad_Aire_Mexico.xlsx`.

## Etapa 3 · Tabla de Excel

1. Seleccionar cualquier celda con datos y presionar `Ctrl + T`, o usar **Insertar → Tabla**.
2. Confirmar el rango `$A$1:$K$858` con la opción **La tabla tiene encabezados**.
3. En la pestaña **Diseño de tabla**, en **Nombre de tabla**, escribir `tblCalidadAire`.
4. Elegir un estilo de tabla.

Nombrar la Tabla permite usar referencias estructuradas, como `tblCalidadAire[PM25]`, que se ajustan solas si cambian los datos.

> 📷 **Captura `03_tabla_excel.png`:** la Tabla con la pestaña **Diseño de tabla** abierta y el nombre `tblCalidadAire` visible.

## Etapa 4 · Fórmulas y cálculos

### Columnas calculadas

1. En `L1` escribir `Veces_Guia_PM25`. La Tabla se amplía sola.
2. En `L2` escribir la fórmula siguiente; Excel la copia a toda la columna:

   ```
   =SI(ESNUMERO([@PM25]),[@PM25]/5,"")
   ```

   Divide el PM2.5 entre 5 µg/m³, la guía anual de la OMS, para saber cuántas veces la supera.

3. En `M1` escribir `Categoria_PM25` y en `M2`:

   ```
   =SI(NO(ESNUMERO([@PM25])),"Sin dato",SI([@PM25]>35,"Muy alto (>35)",SI([@PM25]>25,"Alto (25-35)",SI([@PM25]>15,"Moderado (15-25)",SI([@PM25]>10,"Bajo (10-15)",SI([@PM25]>5,"Cerca de guía (5-10)","Cumple guía (≤5)"))))))
   ```

   Clasifica cada medición según los objetivos intermedios de la OMS 2021.

4. Dar formato de número con 2 decimales a `Veces_Guia_PM25`.

> 📷 **Captura `04_columnas_calculadas.png`:** las columnas `Veces_Guia_PM25` y `Categoria_PM25`, con una celda de `Categoria_PM25` seleccionada y su fórmula en la barra de fórmulas.

### Fórmulas base

En una hoja nueva llamada `Formulas` se armó esta tabla. La columna C lleva la fórmula como texto, con un apóstrofo al inicio, y la columna D la fórmula real:

| A: Fórmula | B: Pregunta | C: Fórmula escrita | D: Resultado |
|---|---|---|---|
| SUMA | ¿Cuántas estaciones suman todos los registros? | `'=SUMA(tblCalidadAire[Estaciones])` | `=SUMA(tblCalidadAire[Estaciones])` |
| PROMEDIO | ¿Cuál es el PM2.5 promedio de todas las mediciones? | `'=PROMEDIO(tblCalidadAire[PM25])` | `=PROMEDIO(tblCalidadAire[PM25])` |
| CONTAR | ¿Cuántos registros tienen PM2.5 medido? | `'=CONTAR(tblCalidadAire[PM25])` | `=CONTAR(tblCalidadAire[PM25])` |
| CONTARA | ¿Cuántos registros municipio-año hay? | `'=CONTARA(tblCalidadAire[Municipio])` | `=CONTARA(tblCalidadAire[Municipio])` |
| CONTAR.SI | ¿Cuántas mediciones cumplen la guía de 5 µg/m³? | `'=CONTAR.SI(tblCalidadAire[PM25],"<=5")` | `=CONTAR.SI(tblCalidadAire[PM25],"<=5")` |
| SI | ¿El promedio nacional cumple la guía? | `'=SI(D3<=5,"Cumple","No cumple")` | `=SI(D3<=5,"Cumple","No cumple")` |

En la fila de SI, `D3` es la celda con el resultado de PROMEDIO. Los resultados deben coincidir con los valores de control de la sección 7 del notebook 01. El resultado de PROMEDIO se muestra con 1 decimal.

> 📷 **Captura `05_formulas_base.png`:** la hoja `Formulas` completa.

## Etapa 5 · Tabla dinámica

1. Seleccionar una celda de la Tabla `tblCalidadAire`.
2. **Insertar → Tabla dinámica**, con origen `tblCalidadAire` y destino **Nueva hoja de cálculo**.
3. Renombrar la hoja como `TD_Principal` y, en **Analizar tabla dinámica → Nombre de tabla dinámica**, nombrar la tabla `TD_Principal`.
4. En **Campos de tabla dinámica**, colocar:
   - `Estado` en **Filas**
   - `Anio` en **Columnas**
   - `PM25` en **Valores**
   - `Region` en **Filtros**
5. En **Suma de PM25 → Configuración de campo de valor**, cambiar a **Promedio** y, en **Formato de número**, 1 decimal.

> 📷 **Captura `06_tabla_dinamica_campos.png`:** la tabla dinámica con el panel **Campos de tabla dinámica** abierto, mostrando Filas, Columnas, Valores y Filtros.

6. Ordenar los estados de mayor a menor: la tabla tiene una fila *Total general* abajo y una columna *Total general* a la derecha de 2023. Clic derecho sobre cualquier número de la **columna** Total general → **Ordenar → Ordenar de mayor a menor**. Si esa columna no aparece, activarla en **Diseño de tabla dinámica → Totales generales → Activado para filas y columnas**.
7. Seleccionar los valores sin los totales y aplicar **Inicio → Formato condicional → Escalas de color**.

Los valores deben coincidir con la tabla de control del notebook 01, guardada también en `data/processed/referencia_tabla_dinamica.csv`.

> 📷 **Captura `07_tabla_dinamica.png`:** la tabla dinámica terminada, ordenada y con la escala de color.

## Etapa 6 · Gráficos

Siguiendo la regla de oro de la clase, cada gráfico parte de una tabla dinámica. Las dos tablas y sus gráficos se colocan en una misma hoja nueva llamada `TD_Graficos`, una al lado de la otra.

**Comparación entre entidades**

1. Insertar una tabla dinámica desde `tblCalidadAire` en la hoja `TD_Graficos`, celda `A3`, con `Estado` en Filas y **Promedio de PM25** en Valores, ordenada de mayor a menor. Nombrarla `TD_Estados` en **Analizar tabla dinámica → Nombre de tabla dinámica**.
2. Quitar Tabasco y Oaxaca, que no tienen PM2.5, desde el filtro de **Etiquetas de fila**.
3. **Insertar → Gráfico dinámico → Barra agrupada**.
4. Título: `PM2.5 promedio por entidad (µg/m³)`, sin leyenda.
5. En el eje vertical, **Dar formato al eje → Categorías en orden inverso**, para que la entidad con mayor PM2.5 quede arriba.

**Tendencia**

1. Insertar otra tabla dinámica desde `tblCalidadAire` en la misma hoja, a la derecha del primer gráfico, con `Anio` en Filas y **Promedio de PM25** en Valores. Nombrarla `TD_Tendencia`.
2. Insertar un **Gráfico dinámico de Líneas con marcadores**.
3. Título: `Tendencia nacional de PM2.5 (µg/m³)`, sin leyenda.

Para reducir el ruido visual, en cada gráfico se usó **Analizar gráfico dinámico → Botones de campo → Ocultar todo**.

> 📷 **Captura `08_grafico_dinamico.png`:** la hoja `TD_Graficos` con las dos tablas dinámicas y sus gráficos.

## Etapa 7 · Dashboard

### Hoja y gráficos

1. Crear una hoja llamada `Dashboard` y moverla al inicio del libro.
2. Quitar la cuadrícula en **Vista → Líneas de cuadrícula**.
3. En `B2`, el título `Calidad del aire en México · PM2.5`.
4. Cortar los dos gráficos dinámicos y pegarlos en `Dashboard`: barras a la izquierda y línea a la derecha.

### Indicadores (KPIs)

Sobre los gráficos van cuatro bloques con etiqueta y fórmula, calculados para el periodo reciente 2018-2023 que se definió en la sección 7 del notebook 01:

| Etiqueta | Fórmula | Formato |
|---|---|---|
| PM2.5 promedio 2018-2023 | `=PROMEDIO.SI.CONJUNTO(tblCalidadAire[PM25],tblCalidadAire[Anio],">=2018",tblCalidadAire[Anio],"<=2023")` | 1 decimal |
| Veces sobre la guía OMS | `=celda_del_promedio/5` | 1 decimal |
| Mediciones sobre la guía | `=CONTAR.SI.CONJUNTO(tblCalidadAire[PM25],">5",tblCalidadAire[Anio],">=2018",tblCalidadAire[Anio],"<=2023")/CONTAR.SI.CONJUNTO(tblCalidadAire[PM25],">=0",tblCalidadAire[Anio],">=2018",tblCalidadAire[Anio],"<=2023")` | Porcentaje, 1 decimal |
| Municipios monitoreados | `=CONTARA(UNICOS(tblCalidadAire[Municipio]&" - "&tblCalidadAire[Estado]))` | Sin decimales |

`UNICOS` está disponible en Excel 365 y Excel 2021. Los valores usan letra de tamaño 24 en negrita sobre un relleno gris claro.

### Segmentadores

1. En la hoja `Dashboard`, seleccionar el gráfico de barras.
2. **Analizar gráfico dinámico → Insertar segmentación de datos**, con `Region` y `Anio`. Los segmentadores se crean en la misma hoja.
3. Colocarlos junto a los KPIs.
4. En el segmentador `Region`, clic derecho → **Conexiones de informe**, y marcar `TD_Principal`, `TD_Estados` y `TD_Tendencia`.
   > 📷 **Captura `09_segmentadores.png`:** la hoja `Dashboard` con la ventana **Conexiones de informe** del segmentador `Region` abierta y las tres tablas marcadas, antes de aceptar.

5. En el segmentador `Anio`, en **Conexiones de informe**, marcar `TD_Principal` y `TD_Estados`. La tendencia no se filtra por año.

> 📷 **Captura `10_dashboard.png`:** el dashboard completo, sin filtros.

Al elegir una región, por ejemplo `Centro`, los gráficos y las tablas dinámicas se actualizan. Los KPIs no cambian porque se calculan sobre toda la Tabla.

> 📷 **Captura `11_dashboard_filtrado.png`:** el dashboard con una región elegida en el segmentador `Region`. Después se borra el filtro y se guarda el libro.

## Etapa 8 · Decisión

En una hoja nueva llamada `Decision` se redactaron:

- **El insight:** el hallazgo principal, con las cifras del dashboard.
- **La decisión:** la acción recomendada a partir del insight.
- **Limitaciones:** lo que los datos no permiten afirmar.

> 📷 **Captura `12_decision.png`:** la hoja `Decision` completa.

## Reporte PDF

El reporte se generó con `notebooks/02_reporte_pdf.ipynb`. Junta en orden las capturas de pantalla, con el notebook 01 completo después de la primera. Necesita Google Chrome para convertir el notebook a PDF. Si falta alguna captura, su página se marca en rojo.

| Orden en el PDF | Archivo en `outputs/capturas/` | Etapa |
|---|---|---|
| 1 | Portada | — |
| 2 | `01_datos_crudos` | 1 · Datos crudos |
| 3 | Notebook 01 completo | 1 y 2 · Datos crudos y limpios |
| 4 | `02_datos_limpios` | 2 · Datos limpios |
| 5 | `03_tabla_excel` | 3 · Tabla de Excel |
| 6 | `04_columnas_calculadas` | 4 · Fórmulas y cálculos |
| 7 | `05_formulas_base` | 4 · Fórmulas y cálculos |
| 8 | `06_tabla_dinamica_campos` | 5 · Tabla dinámica |
| 9 | `07_tabla_dinamica` | 5 · Tabla dinámica |
| 10 | `08_grafico_dinamico` | 6 · Gráficos |
| 11 | `09_segmentadores` | 7 · Dashboard |
| 12 | `10_dashboard` | 7 · Dashboard |
| 13 | `11_dashboard_filtrado` | 7 · Dashboard |
| 14 | `12_decision` | 8 · Decisión |
