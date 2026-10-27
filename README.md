# Análisis de ventas globales y de Europa
Prueba técnica
## Objetivo
Diseñar y desarrollar un pipeline de procesamiento de datos que permita extraer, limpiar, transformar y almacenar información de ventas, para posteriormente generar reportes por mercado y país.
El propósito es evaluar conocimientos de programación, SQL, bases de datos y fundamentos de ingeniería de datos.

## Herramientas utilizadas

- **Google Colab**: entorno de ejecución del notebook.
- **Python**: lectura, exploración, transformación y análisis de datos.
- **KaggleHub**: descarga del dataset desde Kaggle.
- **SQL**: almacenamiento y consultas SQL.
- **SQLAlchemy y psycopg**: conexión entre Python y SQL.

No se utilizó un proyecto propio de Google Cloud Platform (GCP), se ejecuta temporalmente dentro de la sesión de Google Colab.

## Fuente de datos

Dataset: [Retail Sales Dataset (Kaggle)](https://www.kaggle.com/datasets/danielsowah123/retail-sales-dataset), publicado por `danielsowah123`.

En la ejecución documentada se trabajó con **18,045 registros y 36 columnas**. La descarga se realiza desde el notebook con `kagglehub.dataset_download(...)`.
## Contenido del repositorio

```text
retail-sales-data-analysis/
├── examen_prueba.ipynb   # Notebook principal
├── README.md             # Descripción de ejecución
└── requirements.txt      # Dependencias de Python
```

## Instrucciones para ejecutar el proyecto

1. Abrir `examen_prueba.ipynb` en [Google Colab](https://colab.research.google.com/).
2. Seleccionar un entorno de ejecución de **Python** con conexión a Internet.
3. Ejecutar las celdas **en orden, de arriba hacia abajo**. El notebook descarga automáticamente el dataset desde Kaggle, explora los registros y realiza una limpieza inicial.
4. En la sección **Consulta en SQL**, ejecutar las celdas de instalación e inicio, así como las instrucciones que crean el usuario local.
5. Ejecutar la conexión mediante SQLAlchemy y la carga del DataFrame `ventas` en la tabla `ventas_globales`.
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

Se agrupan las ventas por país y región, se calcula su participación porcentual en las ventas globales.

El criterio es una regla definida para el ejercicio; no representa una conclusión de bajo rendimiento empresarial. Para evaluar el desempeño hace falta información sobre metas, tamaño de mercado y otros factores. El reporte se exporta como `reporte_ventas_globales.csv`.

## Resultados de referencia

En la ejecución del notebook se obtuvieron:

- **18,045 registros** de ventas globales.
- **6 países** representados en la fuente.
- **6,699 registros** correspondientes a Reino Unido, Alemania y Francia.
- **3,024,365.63** como importe total de ventas europeas, en las unidades monetarias del dataset.

Los importes se obtienen al ejecutar las consultas del dataset.

## Archivos de salida

Al ejecutar las celdas de exportación se generan en el entorno de Colab:

- `/content/ventas_paises_europa.csv`
- `/content/reporte_ventas_globales.csv`

Estos archivos pueden descargarse desde el panel de archivos de Colab. 
## Alcances y limitaciones

- La limpieza implementada es inicial, no incluye imputación general de valores faltantes ni validación de todas las columnas.
- La estandarización geográfica contempla variantes conocidas.
- La base de datos es temporal dentro de Colab.
- Las categorías de participación se basan en un umbral exploratorio y no en objetivos comerciales proporcionados.
