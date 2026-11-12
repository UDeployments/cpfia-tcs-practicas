# Guía paso a paso en Excel

Esta guía cubre las etapas 3 a 7 del proceso de la Sesión 18. Las etapas 1 y 2 se hacen en `notebooks/01_preparacion_datos.ipynb`. El reporte final se arma con `notebooks/02_reporte_pdf.ipynb`.

Los menús están escritos para Excel en español en Mac. Si tu Excel separa los argumentos con `;` en lugar de `,`, cambia las comas de las fórmulas.

## Antes de empezar

- Ejecuta `01_preparacion_datos.ipynb` completo. Debe existir `data/processed/calidad_aire_mexico_limpio.xlsx`.
- Crea la carpeta `outputs/capturas/` si no existe.
- **Capturas en Mac:** `Cmd + Shift + 4`, luego `Espacio` y clic sobre la ventana de Excel. La captura se guarda en el Escritorio. Renómbrala con el nombre exacto que indica cada paso y muévela a `outputs/capturas/`. También sirve `.jpg`.
- En cada captura deja visible lo importante: la cinta de opciones con la pestaña que usaste, la barra de fórmulas cuando haya fórmula y el panel lateral cuando aplique.

Resumen de capturas (no hay `02_...`: esa parte la cubre el notebook 01 completo dentro del PDF):

| # | Archivo | Qué muestra |
|---|---|---|
| 1 | `01_datos_crudos.png` | Archivo original de la OMS abierto en Excel |
| 3 | `03_datos_limpios.png` | Archivo limpio abierto en Excel |
| 4 | `04_tabla_excel.png` | Tabla creada y con nombre |
| 5 | `05_columnas_calculadas.png` | Columnas calculadas con su fórmula |
| 6 | `06_formulas_base.png` | Hoja de fórmulas base |
| 7 | `07_tabla_dinamica_campos.png` | Panel de campos de la tabla dinámica |
| 8 | `08_tabla_dinamica.png` | Tabla dinámica terminada |
| 9 | `09_grafico_dinamico.png` | Gráficos dinámicos |
| 10 | `10_segmentadores.png` | Segmentadores y sus conexiones |
| 11 | `11_dashboard.png` | Dashboard completo |
| 12 | `12_dashboard_filtrado.png` | Dashboard con un filtro aplicado |

---

## Etapa 1 · Datos crudos

1. Abre en Excel `data/raw/who_ambient_air_quality_database_v8_2026.xlsx`.
2. Observa que cada fila es un texto separado por comas dentro de la columna A, y que las filas largas siguen en las columnas B, C, D...
3. Haz clic en una celda de la columna A para que el texto completo se vea en la barra de fórmulas.

📸 **Captura:** `01_datos_crudos.png`

Cierra el archivo sin guardar.

## Etapa 2 · Datos limpios

La limpieza se hace en el notebook 01 porque el archivo trae 55,221 filas con texto partido en varias celdas.

El notebook 01 completo va dentro del PDF, justo después de la etapa 1, así que aquí no necesitas captura del notebook.

1. Abre en Excel `data/processed/calidad_aire_mexico_limpio.xlsx`.
2. Usa **Archivo → Guardar como** y guárdalo como `outputs/S18_Calidad_Aire_Mexico.xlsx` en formato Libro de Excel. A partir de aquí trabaja siempre en este archivo.
3. Revisa que haya 857 filas de datos y 11 columnas: `Anio`, `Estado`, `Municipio`, `Region`, `PM25`, `PM10`, `NO2`, `Estaciones`, `Poblacion`, `Latitud` y `Longitud`.

📸 **Captura:** `03_datos_limpios.png`

## Etapa 3 · Tabla de Excel

1. Haz clic en cualquier celda con datos y presiona `Ctrl + T` o usa **Insertar → Tabla**.
2. Confirma que el rango sea `$A$1:$K$858` y que esté marcado **La tabla tiene encabezados**.
3. En la pestaña **Diseño de tabla**, en el cuadro **Nombre de tabla**, escribe `tblCalidadAire`.
4. Elige un estilo de tabla en la misma pestaña.

📸 **Captura:** `04_tabla_excel.png`, con la pestaña Diseño de tabla y el nombre visibles.

## Etapa 4 · Fórmulas y cálculos

### Columnas calculadas

1. En `L1` escribe `Veces_Guia_PM25`. La Tabla se amplía sola.
2. En `L2` escribe esta fórmula y presiona Enter. Excel la copia a toda la columna:

   ```
   =SI(ESNUMERO([@PM25]),[@PM25]/5,"")
   ```

   Divide el PM2.5 entre 5, la guía anual de la OMS, para saber cuántas veces la supera.

3. En `M1` escribe `Categoria_PM25` y en `M2` esta fórmula:

   ```
   =SI(NO(ESNUMERO([@PM25])),"Sin dato",SI([@PM25]>35,"Muy alto (>35)",SI([@PM25]>25,"Alto (25-35)",SI([@PM25]>15,"Moderado (15-25)",SI([@PM25]>10,"Bajo (10-15)",SI([@PM25]>5,"Cerca de guía (5-10)","Cumple guía (≤5)"))))))
   ```

   Clasifica cada medición según los objetivos intermedios de la OMS 2021.

4. Da formato de número con 2 decimales a la columna `Veces_Guia_PM25`.

📸 **Captura:** `05_columnas_calculadas.png`, con una celda de `Categoria_PM25` seleccionada y su fórmula visible.

### Fórmulas base

1. Crea una hoja nueva llamada `Formulas`.
2. Arma esta tabla. Escribe la columna C como texto con un apóstrofo al inicio para que se vea la fórmula, y la columna D como fórmula real:

   | A: Fórmula | B: Pregunta | C: Fórmula escrita | D: Resultado |
   |---|---|---|---|
   | SUMA | ¿Cuántas estaciones suman todos los registros? | `'=SUMA(tblCalidadAire[Estaciones])` | `=SUMA(tblCalidadAire[Estaciones])` |
   | PROMEDIO | ¿Cuál es el PM2.5 promedio de todas las mediciones? | `'=PROMEDIO(tblCalidadAire[PM25])` | `=PROMEDIO(tblCalidadAire[PM25])` |
   | CONTAR | ¿Cuántos registros tienen PM2.5 medido? | `'=CONTAR(tblCalidadAire[PM25])` | `=CONTAR(tblCalidadAire[PM25])` |
   | CONTARA | ¿Cuántos registros municipio-año hay? | `'=CONTARA(tblCalidadAire[Municipio])` | `=CONTARA(tblCalidadAire[Municipio])` |
   | CONTAR.SI | ¿Cuántas mediciones cumplen la guía de 5 µg/m³? | `'=CONTAR.SI(tblCalidadAire[PM25],"<=5")` | `=CONTAR.SI(tblCalidadAire[PM25],"<=5")` |
   | SI | ¿El promedio nacional cumple la guía? | `'=SI(D3<=5,"Cumple","No cumple")` | `=SI(D3<=5,"Cumple","No cumple")` |

   En la fila de SI, cambia `D3` por la celda donde quedó el resultado de PROMEDIO.

3. Compara tus resultados con la sección 7 del notebook 01.

📸 **Captura:** `06_formulas_base.png`

## Etapa 5 · Tabla dinámica

1. Ve a la hoja de datos y haz clic en cualquier celda de la Tabla.
2. **Insertar → Tabla dinámica**. Confirma que la fuente sea `tblCalidadAire` y elige **Nueva hoja de cálculo**.
3. Renombra la hoja nueva como `TD_Principal`.
4. En el panel **Campos de tabla dinámica**, arrastra:
   - `Estado` a **Filas**
   - `Anio` a **Columnas**
   - `PM25` a **Valores**
   - `Region` a **Filtros**
5. En Valores, haz clic en **Suma de PM25 → Configuración de campo de valor** y cambia a **Promedio**. Ahí mismo, en **Formato de número**, elige 1 decimal.

📸 **Captura:** `07_tabla_dinamica_campos.png`, con el panel de campos visible.

6. Ordena de mayor a menor: clic derecho en un valor de la columna **Total general → Ordenar → De mayor a menor**.
7. Selecciona los valores de la tabla, sin los totales, y aplica **Inicio → Formato condicional → Escalas de color**.
8. Compara con la tabla de referencia de la sección 7 del notebook 01. También está en `data/processed/referencia_tabla_dinamica.csv`.

📸 **Captura:** `08_tabla_dinamica.png`

## Etapa 6 · Gráficos

Regla de oro de la clase: primero la tabla y después el gráfico.

### Comparación entre entidades

1. Desde la Tabla de datos, inserta otra tabla dinámica en una hoja nueva llamada `TD_Estados`: `Estado` en Filas y **Promedio de PM25** en Valores.
2. Ordénala de mayor a menor.
3. Con una celda de esa tabla seleccionada: **Insertar → Gráfico dinámico → Barra agrupada**.
4. Título del gráfico: `PM2.5 promedio por entidad (µg/m³)`. Quita la leyenda.

### Tendencia

1. Inserta otra tabla dinámica en una hoja nueva llamada `TD_Tendencia`: `Anio` en Filas y **Promedio de PM25** en Valores.
2. Inserta un **Gráfico dinámico de Líneas con marcadores**.
3. Título: `Tendencia nacional de PM2.5 (µg/m³)`. Quita la leyenda.

Para menos ruido visual, en cada gráfico usa **Analizar gráfico dinámico → Botones de campo → Ocultar todo**.

📸 **Captura:** `09_grafico_dinamico.png`, con uno o los dos gráficos.

## Etapa 7 · Dashboard

### Hoja y gráficos

1. Crea una hoja llamada `Dashboard` y ponla al inicio del libro.
2. Quita la cuadrícula en **Vista → Líneas de cuadrícula**.
3. En `B2` escribe el título: `Calidad del aire en México · PM2.5`.
4. Corta (`Cmd + X`) los dos gráficos dinámicos y pégalos en `Dashboard`: barras a la izquierda y línea a la derecha.

### Indicadores (KPIs)

Arriba de los gráficos, crea cuatro bloques con una etiqueta y una fórmula. Todos usan el periodo 2018-2022:

| Etiqueta | Fórmula | Formato |
|---|---|---|
| PM2.5 promedio 2018-2022 | `=PROMEDIO.SI.CONJUNTO(tblCalidadAire[PM25],tblCalidadAire[Anio],">=2018",tblCalidadAire[Anio],"<=2022")` | 1 decimal |
| Veces sobre la guía OMS | `=celda_del_promedio/5` | 1 decimal |
| Mediciones sobre la guía | `=CONTAR.SI.CONJUNTO(tblCalidadAire[PM25],">5",tblCalidadAire[Anio],">=2018",tblCalidadAire[Anio],"<=2022")/CONTAR.SI.CONJUNTO(tblCalidadAire[PM25],">=0",tblCalidadAire[Anio],">=2018",tblCalidadAire[Anio],"<=2022")` | Porcentaje, 1 decimal |
| Municipios monitoreados | `=CONTARA(UNICOS(tblCalidadAire[Municipio]&" - "&tblCalidadAire[Estado]))` | Sin decimales |

`UNICOS` existe en Excel 365 y Excel 2021. Si tu versión no la tiene, escribe el valor de la sección 7 del notebook 01.

Usa letra grande para los valores, por ejemplo tamaño 24 en negrita, y un relleno gris claro para cada bloque.

### Segmentadores

1. Selecciona una celda de la tabla dinámica de `TD_Estados`.
2. **Analizar tabla dinámica → Insertar segmentación de datos** y marca `Region` y `Anio`.
3. Corta los dos segmentadores y pégalos en `Dashboard`, junto a los KPIs.
4. Clic derecho en el segmentador `Region` → **Conexiones de informe** y marca las tres tablas dinámicas: `TD_Principal`, `TD_Estados` y `TD_Tendencia`.
5. En el segmentador `Anio`, en **Conexiones de informe**, deja marcadas `TD_Principal` y `TD_Estados`. La tendencia no se filtra por año.

📸 **Captura:** `10_segmentadores.png`, con la ventana de Conexiones de informe abierta.

📸 **Captura:** `11_dashboard.png`, con el dashboard completo y sin filtros.

6. En el segmentador `Region`, elige una región, por ejemplo `Centro`. Los gráficos cambian; los KPIs no, porque se calculan sobre toda la Tabla.

📸 **Captura:** `12_dashboard_filtrado.png`

7. Quita el filtro con el botón de borrar del segmentador y guarda el libro.

## Etapa 8 · Reporte

1. Abre `notebooks/02_reporte_pdf.ipynb`.
2. Escribe tus textos en la sección 3, con tus palabras. Usa los números de la sección 7 del notebook 01 para el insight y la decisión.
3. Antes, asegúrate de haber guardado el notebook 01 ya ejecutado: el PDF incluye el notebook 01 completo, con su código y resultados, justo después de la etapa 1.
4. Ejecuta todo el notebook 02. Necesita Google Chrome instalado para convertir el notebook 01 a PDF. El PDF queda en `outputs/S18_Dashboard_Calidad_Aire_Mexico.pdf`. Si falta algo, el PDF lo marca en rojo y la tabla de la sección 4 te dice qué completar.
