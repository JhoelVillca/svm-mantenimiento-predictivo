# Motor de Mantenimiento Predictivo basado en SVM

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Scikit-Learn](https://img.shields.io/badge/scikit--learn-1.0%2B-orange)
![Google Colab](https://img.shields.io/badge/Google-Colab-yellow)

Este repositorio contiene la arquitectura, el preprocesamiento y el despliegue de un modelo de Machine Learning basado en **Maquinas de Vectores de Soporte (SVM)**. El sistema esta diseñado para entornos de administracion de servidores e infraestructura industrial (Sysadmin), con el objetivo de predecir fallos de hardware en tiempo real evaluando telemetria pura, eliminando asi la dependencia de los mantenimientos preventivos a ciegas.

---

## Arquitectura del Sistema y Dataset

El motor predictivo se alimenta del **AI4I 2020 Predictive Maintenance Dataset** (Kaggle), un conjunto de datos que cuenta con 10,000 registros de telemetria continua.

### Variables Predictoras (Inputs)
* **Temp_Aire_K:** Temperatura del aire ambiental (Kelvin).
* **Temp_Proceso_K:** Temperatura interna del componente (Kelvin).
* **RPM:** Velocidad de rotacion.
* **Torque_Nm:** Fuerza de torsion del eje.
* **Desgaste_min:** Tiempo de desgaste acumulado de la herramienta.

### Variable Objetivo (Output)
* **Fallo (Machine failure):** Variable booleana donde `0` indica hardware sano y `1` indica un fallo inminente.

---

## Nucleo Matematico

El modelo no traza cortes lineales simples. Debido a la naturaleza compleja de las fallas fisicas, el algoritmo emplea SVM resolviendo el problema de optimizacion dual para maximizar el margen de separacion:

$$\min \frac{1}{2} ||w||^2$$

Ademas, se implementa el **"Kernel Trick"** mediante una Funcion de Base Radial (RBF) para proyectar la telemetria a un espacio de dimension infinita, logrando envolver matematicamente los casos atipicos:

$$K(x_i, x_j) = \Phi(x_i) \cdot \Phi(x_j)$$

Para mitigar el desbalance extremo de los datos (96% de hardware sano vs 4% de fallos), se configuro el hiperparametro `class_weight='balanced'`, forzando a la maquina a penalizar severamente los falsos negativos.

---

## Dependencias del Entorno

Para ejecutar este pipeline de inferencia, el entorno requiere las siguientes librerias estandar de analisis de datos en Python:

* `pandas` (Ingesta y limpieza de datos en memoria)
* `numpy` (Manipulacion de matrices vectoriales)
* `scikit-learn` (Entrenamiento SVM, estandarizacion y metricas)
* `matplotlib` & `seaborn` (Analisis Exploratorio y renderizado del hiperplano 2D)

---

## Guia de Ejecucion (Despliegue)

La solucion fue diseñada para ser 100% reproducible en la nube utilizando **Google Colaboratory**, sin requerir provisionamiento de hardware local. Sigue estos pasos para auditar el sistema:

1. **Clonar o descargar el repositorio:** Descarga los archivos de este repositorio a tu maquina local. Necesitaras el archivo fuente `telemetria_hardware.csv` y el notebook de Jupyter.
2. **Abrir el entorno:** Importa el archivo `.ipynb` en [Google Colab](https://colab.research.get).
3. **Cargar la telemetria:** En el panel izquierdo de Colab, ve a la seccion de "Archivos" y sube el archivo crudo `telemetria_hardware.csv`.
4. **Ejecutar el pipeline:** En el menu superior, selecciona `Entorno de ejecucion` > `Ejecutar todas`.
5. **Auditar los resultados:** El notebook procesara los datos secuencialmente:
   * Realizara la limpieza de columnas innecesarias en memoria.
   * Mostrara el Analisis Exploratorio de Datos (EDA) con graficos de desbalance y matrices de correlacion.
   * Estandarizara los vectores.
   * Entrenara el modelo SVM y renderizara una proyeccion 2D del Hiperplano RBF.
   * Ejecutara la funcion de inferencia en tiempo real para predecir el estado de nuevos servidores de prueba.

---

**Autor:** Jhoel Mauricio Villca Villca  
*Proyecto Universitario - Ingenieria de Sistemas (MAT205)*
