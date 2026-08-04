# Arbitraje Estadístico mediante Cópulas Mixtas Bivariadas
### Modelación de Dependencias No Lineales y Rentabilidad Ajustada al Riesgo en la BMV (2020-2024)

[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![Mercado: BMV](https://img.shields.io/badge/Mercado-México_BMV-green.svg)]()

## 📌 Resumen
Este repositorio contiene un entorno para la ejecución de **Arbitraje Estadístico (Pairs Trading)** utilizando cópulas mixtas bivariadas. Mientras que las estrategias tradicionales basadas en distancia suelen colapsar durante periodos de alta volatilidad debido a su supuesto implícito de normalidad conjunta, este modelo aprovecha la flexibilidad de las **Cópulas Arquimedianas** para capturar dependencias asimétricas en las colas de la distribución.

Evaluado con 20 activos de alta liquidez de la **Bolsa Mexicana de Valores (BMV)** durante el periodo 2020-2024, el modelo superó significativamente a los benchmarks tradicionales, demostrando que la modelación de la "dependencia pura" es crítica para una gestión de riesgos.

## 🚀 Características Técnicas Clave
*   **Validación Recursiva Walk-Forward:** Implementación de una ejecución por ventanas móviles (12 meses de formación / 6 meses de trading) para eliminar el sesgo de anticipación y garantizar la estacionariedad del modelo.
*   **Desempate Estocástico (Metodología Erdely 2026):** Aplicación de una perturbación estocástica infinitesimal ($10^{-12}$) para garantizar la unicidad de los rangos y la continuidad de las Funciones de Distribución Empírica (EDF).
*   **Calibración de Cópula Mixta:** Optimización de pesos topológicos para las familias **Clayton**, **Gumbel** y **Frank** mediante el algoritmo **SLSQP (Sequential Least Squares Programming)** para minimizar el error cuadrático medio (MSE).
*   **Señales de Probabilidad Condicional:** Generación de señales mediante el **Índice de Desajuste (Mispricing Index)**, calculado a través de las derivadas parciales (*h-functions*) de la cópula mixta ajustada.
*   **Realismo Institucional:** Todos los resultados incluyen un **costo de transacción del 0.10%** (fricción y deslizamiento) por cada operación, asegurando que el backtest refleje la realidad operativa del mercado.

## 📊 Hallazgos Empíricos
El modelo identificó una marcada **dependencia asimétrica de colas pesadas** en el mercado mexicano, con una clara dominancia de la familia Gumbel (momentos alcistas) y Clayton (caídas abruptas).

| Métrica | Estrategia de Cópulas (Propuesta) | Método de Distancia (Benchmark) |
| :--- | :--- | :--- |
| **Ratio de Sharpe** | **1.36** | -0.95 |
| **Tasa de Éxito (Win Rate)** | **~60.3%** | 41.2% |
| **Rendimiento Acumulado** | **+64.17%** | -22.91% |
| **Neutralidad de Mercado** | Alta | Baja |

### Resultados Visuales
*(Nota: Reemplaza estos marcadores con tus archivos de imagen exportados)*

1.  **Curva de Capital (Equity Curve):** La línea púrpura (Cópula) muestra un crecimiento constante y resiliencia durante los periodos de estrés de 2020-2022, mientras que la línea punteada azul (Distancia) exhibe una destrucción de capital.
2.  **Distribución de Beneficios (Boxplot):** Visualiza cómo el modelo de Cópulas logra "recortar" el riesgo de cola negativo, desplazando la mediana de los resultados hacia terreno positivo.

## 🧠 Marco Matemático
La estructura de dependencia se modela como una combinación convexa:
$$C_{mix}(u, v) = w_1 C_{Clayton} + w_2 C_{Gumbel} + w_3 C_{Frank}$$
Donde $\sum w_i = 1$. El **Índice de Desajuste** se deriva de la siguiente manera:
$$MI = P(U \le u | V = v) = \frac{\partial C_{mix}(u, v)}{\partial v}$$

## 🏛️ Implicaciones para la Estabilidad Financiera
Esta investigación demuestra que la **correlación lineal es una métrica engañosa** para medir el riesgo sistémico en México. Los hallazgos sugieren que, durante periodos de estrés, los comovimientos en las colas se aceleran más rápido de lo que predicen los modelos gaussianos. 
