# Análisis de clientes de ConnectaTel

## Objetivo del proyecto

Analizar el comportamiento de los clientes de ConnectaTel, una empresa de telecomunicaciones en Latinoamérica, utilizando información registrada hasta 2024. El proyecto busca explorar y limpiar los datos, construir perfiles de clientes, identificar patrones de consumo y valores atípicos, y proponer segmentos y oportunidades comerciales para mejorar la oferta de planes y las estrategias de retención.

## Datasets utilizados

El análisis utiliza tres archivos CSV:

| Dataset | Descripción | Información principal |
|---|---|---|
| `plans.csv` | Catálogo de planes disponibles. | Precio, minutos incluidos, GB incluidos y costos por consumo adicional. |
| `users.csv` | Información de los clientes. | Identificador, edad, ciudad, fecha de registro, plan y fecha de cancelación (`churn_date`). |
| `usage.csv` | Registro del uso de los servicios. | Usuario, tipo de actividad (llamada o mensaje), fecha, duración y longitud. |

Los archivos deben estar disponibles para que el notebook pueda leerlos. En el entorno original del proyecto se utilizan las rutas `/datasets/plans.csv`, `/datasets/users.csv` y `/datasets/usage.csv`.

## Etapas del análisis

1. **Carga y exploración:** lectura de los tres CSV, revisión de dimensiones, tipos de datos, estructura y primeras filas.
2. **Calidad de datos:** identificación de valores nulos, sentinels, categorías y fechas fuera del periodo de estudio.
3. **Limpieza:** reemplazo del sentinel `-999` en `age` por la mediana, sustitución de `?` en `city` por un valor nulo y marcado de fechas fuera del rango 2020–2024 como ausentes. Se revisó también la relación de los nulos de `duration` y `length` con el tipo de registro; estos valores se conservaron cuando no aplicaban al tipo de actividad.
4. **Agregación por usuario:** cálculo de cantidad de mensajes, cantidad de llamadas y minutos de llamadas por `user_id`, y combinación de estos resultados con la información de clientes para construir el perfil de usuario.
5. **Análisis exploratorio y visualización:** revisión de distribuciones y comparación de edad, mensajes, llamadas y minutos de llamada según el plan Básico o Premium.
6. **Detección de valores atípicos:** uso de boxplots y del método IQR para identificar outliers en las variables de edad y consumo. Se evaluó conservar los valores extremos cuando representan patrones reales de uso.
7. **Segmentación de clientes:** clasificación por nivel de uso (Bajo uso, Uso medio y Alto uso) y por edad (Joven, Adulto y Adulto Mayor).
8. **Conclusiones y recomendaciones:** interpretación de los segmentos y patrones de consumo para proponer oportunidades de fidelización, migración entre planes y posibles ofertas para usuarios de alto o bajo consumo.

## Cómo ejecutar el notebook

### Opción 1: Google Colab

1. Descarga o clona este repositorio.
2. Abre [Google Colab](https://colab.research.google.com/).
3. Selecciona **Archivo → Subir notebook** y carga el archivo `.ipynb` del proyecto.
4. Sube los tres archivos CSV (`plans.csv`, `users.csv` y `usage.csv`) a la sesión de Colab o colócalos en Google Drive.
5. Ajusta las rutas de `pd.read_csv()` en las primeras celdas para que correspondan a la ubicación de los CSV en Colab. Por ejemplo, si los subes directamente a la sesión:
   ```python
   plans = pd.read_csv('/content/plans.csv')
   users = pd.read_csv('/content/userscsv')
   usage = pd.read_csv('/content/usage.csv')
   ```
6. Ejecuta las celdas en orden, desde el inicio hasta las conclusiones.

### Opción 2: Jupyter Notebook local

1. Descarga o clona el repositorio.
2. Instala Python y Jupyter Notebook o JupyterLab.
3. Instala las librerías utilizadas:
   ```bash
   pip install pandas matplotlib seaborn
   ```
4. Coloca los CSV en la carpeta indicada por el notebook o modifica las rutas de lectura.
5. Abre el notebook y ejecuta las celdas en orden.

## Guía breve para reproducir el análisis

- Conserva los nombres de los archivos CSV o actualiza las rutas de carga.
- Ejecuta primero las celdas de importación y lectura de datos.
- Ejecuta las etapas de limpieza antes de realizar agregaciones y gráficos.
- Mantén el orden de ejecución para que los DataFrames y columnas creadas en etapas anteriores estén disponibles en las siguientes.
- Revisa las salidas de las celdas de valores nulos, distribuciones, outliers y segmentación.
- Al finalizar, consulta la sección de análisis ejecutivo para revisar los principales hallazgos y recomendaciones.

## Herramientas utilizadas

- **Python**
- **Pandas** para manipulación y limpieza de datos.
- **Matplotlib** y **Seaborn** para visualización.
- **Jupyter Notebook / Google Colab** para ejecutar y documentar el análisis.

## Alcance y consideraciones

El análisis se basa en los datasets proporcionados y considera como periodo válido de referencia la información hasta 2024. Los valores atípicos no se deben interpretar automáticamente como errores: es necesario comprobar si representan consumos reales antes de eliminarlos. De igual forma, los valores nulos de duración o longitud deben interpretarse según el tipo de registro y si el dato aplica a esa actividad.

Las conclusiones comerciales son recomendaciones derivadas de los patrones observados; para confirmar la rentabilidad de cada segmento se necesitaría complementar el análisis con ingresos, costos y margen por cliente o plan.
