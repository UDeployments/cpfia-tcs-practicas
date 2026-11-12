# Prácticas IA Explorer – TCS / TecNM (CPFIA)

Portafolio de prácticas desarrolladas durante el programa **IA Explorer** de TCS, impartido en el Centro Público de Formación en Inteligencia Artificial (CPFIA) del TecNM.

El repositorio reúne los ejercicios de análisis de datos, Python e inteligencia artificial que se trabajan en cada sesión del programa.

## Prácticas

| Módulo | Sesión | Práctica | Descripción |
|--------|--------|----------|-------------|
| 02 | 18 | [Calidad del aire en México con Excel](Modulo02/S18_Calidad_Aire_Excel/) | Base de calidad del aire de la OMS: limpieza en notebook, tabla dinámica y dashboard nativos en Excel, reporte PDF con capturas |

## Estructura de cada práctica

Todas las prácticas siguen la misma estructura, basada en una versión ligera de [Cookiecutter Data Science](https://cookiecutter-data-science.drivendata.org/).

### Carpetas

```
Practicas/
├── README.md                     # Este índice
├── requirements.txt
├── _plantilla/                   # Plantilla base para cada práctica nueva
└── ModuloNN/
    └── SNN_Tema_Corto/
        ├── README.md             # Objetivo, qué aprendí, cómo ejecutar, resultados
        ├── notebooks/
        │   └── 01_descripcion.ipynb
        ├── data/
        │   ├── raw/              # Datos originales, sin modificar
        │   └── processed/        # Datos limpios generados por los notebooks
        └── outputs/              # Gráficas, tablas y archivos exportados
```

### Nombres

- Módulos: `Modulo01`, `Modulo02`, …
- Prácticas: `SNN_Tema_Corto` con la sesión a dos dígitos. Ejemplo: `S17_Python_Analisis_Datos`.
- Notebooks: `NN_descripcion.ipynb` numerados en orden de ejecución. Ejemplo: `01_exploracion.ipynb`.
- Sin espacios ni acentos en nombres de carpetas o archivos.

### Secciones de cada notebook

1. Título y metadatos: práctica, módulo, sesión, fecha, autor.
2. Objetivo.
3. Configuración: imports y rutas relativas en una sola celda.
4. Carga de datos: fuente y descripción.
5. Exploración de datos (EDA).
6. Limpieza y transformación.
7. Análisis o modelado.
8. Resultados: gráficas y tablas exportadas a `outputs/`.
9. Conclusiones.
10. Referencias.

### Buenas prácticas

- Una celda Markdown antes de cada bloque de código que explique qué hace.
- Solo rutas relativas. Nunca rutas absolutas como `/Users/...`.
- Ejecutar **Restart Kernel and Run All Cells** antes de cada commit para asegurar que el notebook corre de principio a fin.
- No subir datos pesados ni privados. Si un dataset es grande, documentar el enlace de descarga en el README de la práctica.

### Crear una práctica nueva

```bash
mkdir -p Modulo02
cp -R _plantilla Modulo02/S17_Python_Analisis_Datos
```

Después renombra el notebook, completa el README de la práctica y agrega una fila a la tabla de prácticas.

## Tecnologías

- Python 3.10+ (desarrollado con 3.13)
- Jupyter Lab / Notebook
- NumPy, pandas
- Matplotlib
- scikit-learn
- openpyxl (lectura y escritura de Excel)
- pypdf (unir archivos PDF)

## Cómo ejecutar

1. Clona el repositorio:

   ```bash
   git clone <url-del-repositorio>
   cd <carpeta-del-repositorio>
   ```

2. Crea un entorno virtual:

   ```bash
   python3 -m venv .venv
   ```

3. Activa el entorno:

   - macOS / Linux:

     ```bash
     source .venv/bin/activate
     ```

   - Windows (PowerShell):

     ```powershell
     .venv\Scripts\Activate.ps1
     ```

4. Instala las dependencias:

   ```bash
   pip install -r requirements.txt
   ```

5. Abre Jupyter:

   ```bash
   jupyter lab
   ```

## Autor

**Saúl González**
