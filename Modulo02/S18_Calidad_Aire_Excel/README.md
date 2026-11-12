# S18 · Calidad del aire en México con Excel

**Módulo:** 02 · **Sesión:** 18 · Excel para análisis de datos I · **Fecha:** 2026-11-12

## Objetivo

En esta práctica apliqué el proceso que vimos en la Sesión 18, *de datos crudos a decisiones de negocio*, a un tema de contaminación en México. Elegí el área de **aire** y trabajé con una base de datos pública de la OMS para responder esta pregunta:

| Pregunta | Métrica | Dimensión | Filtro | Análisis accionable |
|---|---|---|---|---|
| ¿Qué entidades de México respiran el aire más contaminado y cómo ha cambiado? | Promedio anual de PM2.5 en µg/m³ | Entidad (filas) y año (columnas) | Región y año | Priorizar dónde invertir primero en control de emisiones y monitoreo |

## Datos

Usé la [WHO Ambient Air Quality Database, versión 8.0](https://www.who.int/publications/m/item/who-ambient-air-quality-database-(update-jun-2026)), publicada por la OMS en junio de 2026. Tiene los promedios anuales de PM2.5, PM10 y NO2 de ciudades de todo el mundo; los datos de México vienen del SINAICA del INECC. Para México hay 868 registros de 2010 a 2023.

Como guía usé los valores anuales recomendados por la OMS en 2021: 5 µg/m³ para PM2.5, 15 para PM10 y 10 para NO2.

## Cómo lo hice

**1. Datos crudos.** Cuando abrí el archivo de la OMS en Excel me di cuenta de que no se podía usar así: cada fila venía como un texto separado por comas dentro de la columna A, y las filas largas se partían en varias celdas.

**2. Limpieza en Python.** Por el tamaño del archivo, más de 55 mil filas, hice la limpieza en un notebook (`notebooks/01_preparacion_datos.ipynb`). Reconstruí las columnas, filtré México, separé el municipio y el estado, unifiqué nombres repetidos y descarté las mediciones que cubrían menos del 75% del año. Antes de decidir ese 75% revisé cuántas mediciones quedaban fuera con distintos umbrales. Al final exporté un Excel limpio con 857 registros y calculé con pandas unos valores de control para comprobar después mis resultados en Excel.

**3 a 8. Análisis en Excel.** Todo lo demás lo hice a mano en Excel, en `outputs/S18_Calidad_Aire_Mexico.xlsx`:

- Convertí los datos en una Tabla llamada `tblCalidadAire`.
- Agregué dos columnas calculadas con `SI`: cuántas veces supera cada medición la guía de la OMS y una categoría según su nivel.
- Hice una hoja con las fórmulas base (`SUMA`, `PROMEDIO`, `CONTAR`, `CONTARA`, `CONTAR.SI` y `SI`), cada una respondiendo una pregunta.
- Armé una tabla dinámica con el promedio de PM2.5 por entidad y año, ordenada y con escala de color.
- Hice dos gráficos dinámicos: barras para comparar entidades y una línea para ver la tendencia.
- Junté todo en un dashboard con indicadores y segmentadores de región y año conectados a las tablas dinámicas.
- Escribí el insight y la decisión en una hoja aparte.

Comparé las fórmulas base y la tabla dinámica con los valores de control del notebook y coincidieron. El procedimiento completo, con menús y fórmulas, está en [`GUIA_EXCEL.md`](GUIA_EXCEL.md).

**Reporte.** El PDF lo generé con `notebooks/02_reporte_pdf.ipynb`. Junta en orden las capturas que fui tomando, con el notebook de limpieza completo después de los datos crudos.

## Resultados

- Entre 2018 y 2023 el PM2.5 promedio fue de **19.6 µg/m³, casi 4 veces la guía de la OMS**, y el 99.2% de las mediciones estuvo por arriba.
- Las entidades con el aire más contaminado son el **Estado de México, Jalisco y Baja California**. Los municipios más altos están en el Valle de Toluca (San Mateo Atenco, Metepec y Toluca) y en Guadalajara.
- El promedio nacional bajó de 31.9 en 2013 a cerca de 20 en 2018, pero desde entonces casi no ha cambiado.
- Tabasco y Oaxaca tienen estaciones, pero no miden PM2.5.

**Decisión:** enfocar primero el presupuesto de control de emisiones en el Estado de México, Jalisco y Baja California, y poner monitores de PM2.5 donde hoy no se mide.

**Limitaciones:** solo se incluyen municipios con estaciones, 2023 tiene pocas mediciones y se descartaron las de baja cobertura.

## Qué aprendí

- Que los datos públicos no siempre vienen listos para usarse; la mayor parte del trabajo estuvo en entender y limpiar el archivo.
- A plantear primero la pregunta, la métrica y la dimensión, y después pasarlas a Filas, Columnas y Valores de la tabla dinámica.
- A conectar segmentadores a varias tablas dinámicas para que un solo filtro cambie todo el dashboard.
- Que conviene tener valores de control para comprobar que lo que hago en Excel está bien.

## Archivos

| Archivo | Contenido |
|---|---|
| `notebooks/01_preparacion_datos.ipynb` | Descarga, exploración y limpieza de los datos |
| `notebooks/02_reporte_pdf.ipynb` | Generación del reporte PDF |
| `GUIA_EXCEL.md` | Procedimiento en Excel para replicar el análisis |
| `data/processed/calidad_aire_mexico_limpio.xlsx` | Datos limpios, base del trabajo en Excel |
| `data/processed/referencia_tabla_dinamica.csv` | Tabla dinámica de control |
| `outputs/S18_Calidad_Aire_Mexico.xlsx` | Mi libro de Excel con todo el análisis |
| `outputs/S18_Dashboard_Calidad_Aire_Mexico.pdf` | Reporte final |
| `outputs/capturas/` | Capturas de cada paso en Excel |

El archivo original de la OMS (1.9 MB) no está en el repositorio; el notebook 01 lo descarga en `data/raw/`.

## Cómo replicarlo

1. Ejecutar `notebooks/01_preparacion_datos.ipynb` con el kernel **Python (CPFIA)**. La primera vez necesita internet para descargar la base de la OMS.
2. Seguir `GUIA_EXCEL.md` en Excel y guardar las capturas en `outputs/capturas/` con los nombres que ahí se indican.
3. Ejecutar `notebooks/02_reporte_pdf.ipynb`. Necesita Google Chrome y la librería `pypdf` de `requirements.txt`.

## Referencias

- World Health Organization. *WHO Ambient Air Quality Database, version 8.0*, 2026.
- World Health Organization. *WHO global air quality guidelines*, 2021.
- INECC. [Sistema Nacional de Información de la Calidad del Aire (SINAICA)](https://sinaica.inecc.gob.mx/).
- TCS Empowers / TecNM CPFIA. *Sesión 18: Excel para análisis de datos I*.
