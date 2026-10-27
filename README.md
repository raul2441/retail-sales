# Análisis de ventas globales y de Europa

Prueba técnica para la vacante de **Becario Data Analyst Jr.**

## Objetivo

Desarrollar un pipeline de procesamiento de datos para extraer información de ventas, realizar una limpieza inicial, almacenarla en PostgreSQL y generar reportes por país y región.

## Herramientas utilizadas

- **Google Colab**: entorno de ejecución del notebook.
- **Python y pandas**: lectura, exploración, transformación y análisis de datos.
- **KaggleHub**: descarga del dataset desde Kaggle.
- **PostgreSQL**: almacenamiento y consultas SQL.
- **SQLAlchemy y psycopg**: conexión entre Python y PostgreSQL.

No se utilizó un proyecto propio de Google Cloud Platform (GCP): PostgreSQL se ejecuta temporalmente dentro de la sesión de Google Colab.

## Fuente de datos

Dataset: [Retail Sales Dataset (Kaggle)](https://www.kaggle.com/datasets/danielsowah123/retail-sales-dataset), publicado por `danielsowah123`.

En la ejecución documentada se trabajó con **18,045 registros y 36 columnas**. La descarga se realiza desde el notebook con `kagglehub.dataset_download(...)`; no es necesario incluir una copia del CSV en el repositorio.

## Contenido del repositorio

```text
retail-sales-data-analysis/
├── examen_prueba.ipynb   # Notebook principal: pipeline, SQL y reportes
├── README.md             # Descripción e instrucciones de ejecución
└── requirements.txt      # Dependencias de Python
```

> **Importante:** Subir el archivo `examen_prueba.ipynb` original de Colab. El archivo `.py` exportado contiene comandos específicos del notebook (por ejemplo, `!apt-get`) y no está preparado para ejecutarse directamente con `python archivo.py`.

## Instrucciones para ejecutar el proyecto

1. Abrir `examen_prueba.ipynb` en [Google Colab](https://colab.research.google.com/).
2. Seleccionar un entorno de ejecución de **Python** con conexión a Internet.
3. Ejecutar una celda inicial de instalación de dependencias:

   ```python
   %pip install -q kagglehub pandas sqlalchemy "psycopg[binary]"
   ```

4. Ejecutar las celdas **en orden, de arriba hacia abajo**. El notebook descarga automáticamente el dataset desde Kaggle, explora los registros y realiza una limpieza inicial.
5. En la sección **Consulta en SQL**, ejecutar las celdas de instalación e inicio de PostgreSQL, así como las instrucciones que crean el usuario local `root` y la base de datos `retaildb`.
6. Ejecutar la conexión mediante SQLAlchemy y la carga del DataFrame `ventas` en la tabla `ventas_globales`.
7. Continuar con los apartados 1–4 para generar los reportes, verificar los totales y exportar los CSV.

**Nota:** PostgreSQL se ejecuta dentro de Colab, por lo que la base de datos y sus tablas pueden desaparecer cuando termina o se reinicia el entorno. Para repetir el análisis, ejecutar nuevamente las celdas en orden. La autenticación local del ejemplo está pensada exclusivamente para el entorno temporal de Colab, no para un servidor público.

## Desarrollo del examen

### 1. Pipeline de ventas globales

- Extracción del dataset con KaggleHub.
- Exploración de dimensiones, columnas y valores faltantes.
- Estandarización de nombres de columnas y espacios en campos de texto.
- Eliminación de filas completamente vacías.
- Unificación de variantes conocidas de regiones (por ejemplo, `British columbia` y `British Columbia`; `NRW` y `North Rhine-Westphalia`; `Northeast` y `North East`).
- Carga en PostgreSQL y consulta de registros por país y región.
- Comparación del número de registros originales, almacenados y agrupados.

### 2. Ventas de Europa

Se identifican los países europeos presentes en este dataset: **Reino Unido, Alemania y Francia**. Mediante consultas SQL se obtiene el número de registros y el importe de ventas por país y región, además del total europeo.

### 3. Tabla de ventas por país europeo

Se calculan las ventas de cada país europeo con `GROUP BY`, `COUNT()` y `SUM(sales_amount)`, y la participación porcentual de cada país en el total europeo. Se exporta el reporte `ventas_paises_europa.csv`.

### 4. Participación en ventas globales e indicador

Se agrupan las ventas por país y región, se calcula su participación porcentual en las ventas globales y se aplica un **indicador exploratorio de baja participación**: ventas regionales inferiores al **50 % del promedio de ventas por región**.

El criterio es una regla sencilla definida para el ejercicio; **no representa por sí solo una conclusión de bajo rendimiento empresarial**. Para evaluar el desempeño de forma definitiva harían falta información sobre metas, tamaño de mercado y otros factores. El reporte se exporta como `reporte_ventas_globales.csv`.

## Resultados de referencia

En la ejecución revisada del notebook se obtuvieron:

- **18,045 registros** de ventas globales.
- **6 países** representados en la fuente.
- **6,699 registros** correspondientes a Reino Unido, Alemania y Francia.
- **3,024,365.63** como importe total de ventas europeas, en las unidades monetarias del dataset.

Los importes se obtienen al ejecutar las consultas, por lo que deben verificarse nuevamente si Kaggle actualiza el dataset.

## Archivos de salida

Al ejecutar las celdas de exportación se generan en el entorno de Colab:

- `/content/ventas_paises_europa.csv`
- `/content/reporte_ventas_globales.csv`

Estos archivos pueden descargarse desde el panel de archivos de Colab. Para una nueva sesión deben generarse otra vez.

## Alcances y limitaciones

- La limpieza implementada es **inicial**: no incluye imputación general de valores faltantes ni validación exhaustiva de todas las columnas.
- La estandarización geográfica contempla **variantes conocidas**; no sustituye una tabla maestra de localidades.
- La base de datos es **temporal** dentro de Colab y no constituye un despliegue persistente.
- Las categorías de participación se basan en un umbral exploratorio y no en objetivos comerciales proporcionados por la empresa.
