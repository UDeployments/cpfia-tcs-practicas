# S17 · Calculadora en Python

**Módulo:** 02 · **Sesión:** 17 · Python para Análisis de Datos · **Fecha:** 2026-11-12

## Objetivo

La Sesión 17 fue la introducción a Python en notebooks. Para practicar construí en un notebook de Jupyter una calculadora que pide dos números y una operación, muestra el resultado, maneja los errores sin detenerse y guarda un historial de lo que se calculó.

Mi intención fue usar en un solo programa los temas básicos de la clase, en lugar de hacer ejercicios sueltos. Todo está hecho con Python puro, sin librerías externas.

## Cómo lo hice

**1. Repaso de tipos y operadores.** Antes de programar revisé con `type()` cómo guarda Python los valores, probé los siete operadores aritméticos y los de comparación y lógicos. También provoqué a propósito los errores que vimos en clase (`TypeError`, `ValueError` y `ZeroDivisionError`) para ver sus mensajes.

**2. Funciones.** Escribí una función por operación (suma, resta, multiplicación, división, división entera, módulo y potencia) y las guardé en un diccionario con su símbolo, para no tener una cadena larga de `if` / `elif`. Agregué una función que convierte a número el texto que llega de `input()` y otra que hace el cálculo.

**3. Pruebas.** Con una lista de casos y un ciclo `for` comprobé que cada operación diera el resultado que calculé a mano, incluyendo la división entre cero.

**4. Calculadora interactiva.** Junté todo en un ciclo `while` que pide los datos con `input()` hasta que escribo `salir`. Cada operación, correcta o con error, se guarda en una lista de diccionarios.

**5. Resumen de la sesión.** Recorrí el historial con un `for` para mostrar cada operación con su resultado o su error, y con un diccionario conté cuántas veces usé cada operación.

**Adaptación de la plantilla.** Mis prácticas siguen una plantilla pensada para análisis de datos, con secciones de configuración, carga, EDA y limpieza. Como aquí no hay librerías ni un dataset de entrada, cambié esas secciones por el repaso de tipos y operadores, las funciones y las pruebas. Por eso tampoco hay carpeta `data/`.

## Resultados

- Las 11 pruebas de las operaciones salieron correctas, incluidas las divisiones entre cero.
- La calculadora maneja tres tipos de error sin detenerse: un texto que no es número, una operación que no existe y la división entre cero.
- Al terminar, la calculadora muestra el historial de la sesión y cuántas veces se usó cada operación.

## Qué aprendí

- Que `input()` siempre regresa texto y hay que convertirlo antes de calcular; ahí aparecen la mayoría de los errores.
- A guardar funciones dentro de un diccionario para elegir la operación sin tantos `if`.
- A escribir funciones pequeñas y probarlas antes de juntarlas en un programa.
- A usar `try` / `except` para que un programa no se detenga cuando el usuario se equivoca.
- Detalles de Python que no esperaba: `/` siempre da decimal, `//` redondea hacia abajo con negativos y `0.1 + 0.2` no da exactamente `0.3`.
- Que una lista de diccionarios sirve para guardar un historial y recorrerlo después.

## Archivos

| Archivo | Contenido |
|---|---|
| `notebooks/01_calculadora.ipynb` | Repaso de Python, funciones, pruebas, calculadora y resumen de la sesión |
| `notebooks/02_reporte_pdf.ipynb` | Generación del reporte PDF |
| `GUIA_PROCESO.md` | Procedimiento en VS Code y momento de cada captura |
| `outputs/S17_Calculadora_Python.pdf` | Reporte final |
| `outputs/capturas/` | Capturas de pantalla del proceso |

## Cómo replicarlo

1. Abrir `notebooks/01_calculadora.ipynb` en VS Code (o Jupyter Lab) con el entorno `.venv` del repositorio, reiniciar el kernel y ejecutar todas las celdas. En la sección 5 el notebook espera a que se escriban operaciones en la calculadora; al escribir `salir` muestra el resumen.
2. Guardar el notebook con sus resultados.
3. Ejecutar `notebooks/02_reporte_pdf.ipynb`. La calculadora no usa librerías, pero el reporte sí: necesita Google Chrome y las librerías `matplotlib`, `nbconvert` y `pypdf` de `requirements.txt`.

El detalle de cada paso y de las capturas está en [`GUIA_PROCESO.md`](GUIA_PROCESO.md).

## Referencias

- TCS Empowers / TecNM CPFIA. *Sesión 17: Python para Análisis de Datos* (Basic Notebook con Python).
- Python Software Foundation. [Tutorial de Python](https://docs.python.org/es/3/tutorial/).
- Python Software Foundation. [Errores y excepciones](https://docs.python.org/es/3/tutorial/errors.html).
