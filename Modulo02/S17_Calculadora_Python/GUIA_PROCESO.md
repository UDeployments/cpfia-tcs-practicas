# Procedimiento en VS Code

Este documento describe cómo se ejecutó la práctica de la calculadora y en qué momento se tomó cada captura del reporte, para que cualquier persona pueda replicarla. Los notebooks se trabajaron en Visual Studio Code con la extensión de Jupyter; en Jupyter Lab los pasos son equivalentes.

**Requisito:** el entorno `.venv` del repositorio con las librerías de `requirements.txt`.

**Capturas para el reporte:** en los puntos marcados con 📷 se toma una captura de pantalla de la ventana de VS Code y se guarda en `outputs/capturas/` con el nombre indicado, en `.png` o `.jpg`. El notebook `02_reporte_pdf.ipynb` busca las capturas por ese nombre exacto. En Mac: `Cmd + Shift + 4`, luego `Espacio` y clic sobre la ventana.

---

## 1. Abrir el notebook

1. En VS Code, abrir la carpeta `Practicas` del repositorio con **File → Open Folder**.
2. Abrir `Modulo02/S17_Calculadora_Python/notebooks/01_calculadora.ipynb`.
3. Verificar en la esquina superior derecha que el kernel sea el entorno `.venv` (Python 3.13). Si no, dar clic en **Select Kernel** y elegirlo.

> 📷 **Captura `01_entorno.png`:** el notebook abierto, con la ruta del archivo arriba y el kernel `.venv` visible a la derecha.

## 2. Ejecutar el notebook completo

1. Dar clic en **Restart** y después en **Run All** en la barra del notebook.
2. Las celdas se ejecutan en orden hasta la sección 5. Ahí el notebook se detiene y aparece un cuadro de texto en la parte superior de la ventana, esperando el primer número.

## 3. Pruebas

Mientras la calculadora espera, subir a la sección 4 y revisar la salida de la celda de pruebas: cada caso con `OK` y al final `11 de 11 pruebas correctas`.

> 📷 **Captura `02_pruebas.png`:** la celda de la lista de casos y su salida completa.

## 4. Usar la calculadora

1. Bajar a la sección 5. En cada operación la calculadora pide tres datos, uno por uno: primer número, operación (`+`, `-`, `*`, `/`, `//`, `%` o `**`) y segundo número. Cada dato se escribe en el cuadro de texto y se confirma con `Enter`.
2. Para que el historial tenga casos de todo tipo, la sesión debe incluir varias operaciones correctas con símbolos distintos, con enteros y decimales, y al menos estos tres errores a propósito:
   - una división entre cero, por ejemplo `100 / 0`;
   - una palabra en lugar de un número, por ejemplo `doce - 3`;
   - un símbolo que no existe, por ejemplo `5 ^ 2`.
3. Para terminar, escribir `salir` cuando la calculadora pide el primer número. El notebook continúa solo con la sección 6.

> 📷 **Captura `03_calculadora_sesion.png`:** la salida de la calculadora con las operaciones y los mensajes de error, terminando en `Fin. Operaciones registradas`. Si la salida es muy larga, se captura la parte donde aparecen los errores y el final.

## 5. Resumen de la sesión

Al terminar la calculadora se ejecuta la sección 6, que muestra cada operación del historial con su resultado o su error, el total de operaciones correctas y con error, y cuántas veces se usó cada operación.

> 📷 **Captura `04_resumen_sesion.png`:** la salida completa de la celda de la sección 6.

Al final se guarda el notebook con `Cmd + S` para que el reporte incluya todos los resultados.

## 6. Reporte PDF

1. Abrir `notebooks/02_reporte_pdf.ipynb` con el mismo kernel `.venv`.
2. Dar clic en **Restart** y **Run All**. La lista de la sección 3 indica si falta alguna captura.
3. El reporte queda en `outputs/S17_Calculadora_Python.pdf`. Este notebook sí usa librerías (`matplotlib`, `nbconvert` y `pypdf`) y Google Chrome para convertir el notebook 01 a PDF.

| Orden en el PDF | Contenido |
|---|---|
| 1 | Portada con los temas de la sesión y dónde se usaron |
| 2 | `01_entorno` |
| 3 | `02_pruebas` |
| 4 | `03_calculadora_sesion` |
| 5 | `04_resumen_sesion` |
| 6 en adelante | Notebook `01_calculadora.ipynb` completo |
