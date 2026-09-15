# Firm-fundamentals-Stock-Screener
Quantitative screener for the NYSE/NASDAQ universe that identifies stocks with persistent alpha, anchors the predictive signal to fundamentals.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22759509.svg)](https://doi.org/10.5281/zenodo.22759509)
Martínez Valencia, J. A. (2026). Stock Screening por Segmentación Iterada Basado en Fundamentales [Graphic]. Zenodo. Curso de Análisis y Gestión Moderna de Inversiones - Maestría en Finanzas Cuantitativas, Universidad del Rosario. https://doi.org/10.5281/zenodo.22759509

> Pipeline de segmentación no supervisada y clasificación penalizada para identificar acciones con Alfa de Jensen persistente.

## Tabla de contenidos
- [Descripción](#descripción)
- [Metodología](#metodología)
- [Estructura del repositorio](#estructura-del-repositorio)
- [Requisitos](#requisitos)
- [Datos de entrada](#datos-de-entrada)
- [Cómo ejecutar](#cómo-ejecutar)
- [Resultados principales](#resultados-principales)
- [Limitaciones](#limitaciones)
- [Licencia](#licencia)
- [Referencias](#referencias)

## Descripción
Trabajo académico para el curso de Análisis y Gestión Moderna de Inversiones de la Maestría en Finanzas Cuantitativas de la Universidad del Rosario. Se desarrolla un screener cuantitativo sobre el universo NYSE/NASDAQ, con el objetivo de identificar acciones cuyo Alfa de Jensen sea explicable por fundamentales, de forma robusta frente a la elección arbitraria de hiperparámetros de segmentación.

## Metodología
1. **Filtrado de universo**: liquidez, precio mínimo, historial mínimo de precios.
2. **Variable objetivo**: Alfa de Jensen (CAPM) sobre ventana trailing de [N] días.
3. **Segmentación**: K-Medoids con distancia de Mahalanobis.
4. **Modelo supervisado**: Regresión Logística Elastic Net, validación cruzada K-Fold estratificada, calibración Platt.
5. **Barrido de K**: K = 1 a 25 parametrizable, filtro de consistencia (presencia en Top/Bottom en ≥15/25 configuraciones).
6. **Evaluación**: portafolios de mínima varianza y pesos iguales, comparados contra benchmark (SPY).

## Estructura del repositorio
```
├── notebook_screener.ipynb     # Notebook principal (Colab)
├── requirements.txt            # Dependencias de Python
├── README.md
└── outputs/                    # CSVs generados por el pipeline (opcional, livianos)
    ├── lista_exclusion_features.csv
    ├── pool_candidatos.csv
    └── ...
```

## Requisitos
```
pandas
numpy
scipy
scikit-learn
matplotlib
seaborn
networkx
scorecardpy
```
Instalación: `pip install -r requirements.txt`

## Datos de entrada
El notebook espera dos archivos los cuales serán enviados vía correo. Deben ser cargados en la carpeta "Archivos" de Colab en el menú izquierdo. 
No se cargaron por restricciones de tamaño, y no se incluyó el código para el scraping porque toma aproximadamente una hora la descarga.

- `precios_filtrado.parquet` — precios diarios (`ticker`, `fecha`, `close`, ...) del universo + benchmark (`SPY`).
- `fundamentales_filtrado.parquet` — fundamentales por ticker ([lista de 9 variables]).

Fuentes:  - Yahoo Finance API yfinance vía "https://finance.yahoo.com/quote/{etf}/holdings" 
          - Nasdaq API vía "https://api.nasdaq.com/api/screener/stocks"
          - Scraping vía "https://en.wikipedia.org/wiki/Nasdaq-100"
                        

## Cómo ejecutar
1. Abrir `segmentacion_v3.ipynb` en Google Colab.
2. Cargar los dos archivos de datos en el entorno (Drive o subida directa).
3. Ejecutar con la opción "Ejecutar todo" o ejecutar las celdas en orden. El notebook está organizado en bloques secuenciales (limpieza → escalado → alpha → EDA → barrido de K → validación → portafolios).

## Resultados principales
- AUC consolidado Out-of-Fold.
- Comparativas de portafolio Portafolio Top


## Limitaciones
- Análisis in-sample / de un solo período cross-sectional — no constituye evidencia de desempeño fuera de muestra prospectivo.
  

## Licencia
Todos los derechos reservados. Ver sección de licencia en este repositorio.

## Referencias
Hastie, T., Tibshirani, R., \& Friedman, J. (2009). The elements of statistical learning: Data mining, inference, and prediction (2nd ed.). \textit{Springer}.
Piotroski, J. D. (2000). Value investing: The use of historical financial statement information to separate winners from losers. \textit{J. Account. Res.}, 38, 1--41.

