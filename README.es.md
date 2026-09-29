# ¿Qué explica el PIB per cápita de un país? — Regresión lineal múltiple

[English](README.md) · **Español**

>**Proyecto académico** · Máster en Ciencia de Datos e Inteligencia Artificial (MCDIA) · Afi Escuela · Curso 2025-26
> Módulo: *Basic Regression* · Trabajo individual

![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)
![statsmodels](https://img.shields.io/badge/statsmodels-OLS-4B8BBE)
![pandas](https://img.shields.io/badge/pandas-150458?logo=pandas&logoColor=white)

## Objetivo

Construir un modelo de regresión lineal **interpretable** que explique las diferencias de PIB per cápita entre 227 países a partir de indicadores demográficos, sanitarios, geográficos y de estructura económica, y traducir los resultados en recomendaciones razonadas.

## Datos

`data/countries_gdp.csv`: 227 países × 20 variables (población, superficie, migración neta, mortalidad infantil, alfabetización, teléfonos por cada 1.000 habitantes, uso del suelo, clima, natalidad/mortalidad y peso de agricultura, industria y servicios en la economía).

## Metodología

| Fase | Qué se hace |
|---|---|
| **Análisis exploratorio** | Análisis univariante de todas las variables, correlaciones y diagramas de dispersión por pares |
| **Valores perdidos** | Imputación por **mediana regional** (numéricas) y **moda regional** (`Climate`); renormalización de sectores económicos y uso del suelo para que sumen 1 |
| **Transformaciones** | Logaritmos en `GDP`, `Infant.mortality` y `Phones` para corregir asimetrías y linealizar relaciones |
| **Selección de variables** | **Best Subset Selection** comparando R² ajustado, AIC y BIC |
| **Diagnóstico** | Residuos studentizados, *leverage*, distancia de Cook y colinealidad (VIF) |
| **Hipótesis del modelo** | Linealidad, media nula, homocedasticidad, independencia y normalidad (Shapiro-Wilk, Anderson-Darling, Q-Q plot) |
| **Evaluación** | Comparación de especificaciones alternativas: con y sin observaciones influyentes, con y sin transformaciones |

## Modelo final

$$
\log(GDP) = \beta_0 + \beta_1 \log(\text{Infant mortality}) + \beta_2 \log(\text{Phones}) + \beta_3\,\text{Net migration} + \beta_4\,\text{Agriculture} + \beta_5\,\text{Deathrate} + \beta_6\,\text{Crops} + \varepsilon
$$

| Especificación | N | R² ajustado | AIC | BIC |
|---|---|---|---|---|
| **Muestra completa (modelo elegido)** | 226 | **0.824** | 337.5 | 361.4 |
| Sin las 3 observaciones más influyentes | 223 | 0.854 | 284.0 | 307.8 |
| Sin logaritmos en las explicativas | 226 | 0.813 | 351.2 | 375.1 |

Se mantiene la **muestra completa** aunque quitar observaciones influyentes mejora las métricas: esos países no son errores de medida, sino realidades económicas extremas que forman parte del fenómeno estudiado.

## Conclusiones principales

- **Conectividad** (`Phones`) y **migración neta positiva** se asocian con un mayor PIB per cápita.
- La **mortalidad infantil** y una economía **dependiente del sector primario** (agricultura, cultivos) se asocian con una renta menor.
- El signo positivo de `Deathrate` se interpreta como un efecto de **composición demográfica** (poblaciones más envejecidas en países ricos), no como una relación causal.
- Como el análisis es observacional y transversal, las recomendaciones de política económica se plantean como **asociaciones, no como causalidad**.

## Estructura del repositorio

```
├── gdp_regression.ipynb   # Notebook con todo el análisis, comentado paso a paso
├── data/
│   └── countries_gdp.csv  # Dataset
└── requirements.txt
```

## Cómo ejecutarlo

```bash
python -m venv venv
venv\Scripts\activate          # Windows  (source venv/bin/activate en macOS/Linux)
pip install -r requirements.txt
jupyter notebook gdp_regression.ipynb
```

---

*Trabajo realizado como parte de la evaluación del Máster en Ciencia de Datos e Inteligencia Artificial de Afi Escuela. Calificación y enunciado no incluidos.*
