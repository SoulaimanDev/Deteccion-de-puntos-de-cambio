# Detección de puntos de cambio en diversas trayectorias

# Detección de puntos de cambio en diversas trayectorias

## Índice
1. [Introducción](#introducción)
2. [Estado del arte](#estado-del-arte)
3. [Evaluación](#evaluación)
   - [F1-Score](#f1-score)
   - [Distancia de Hausdorff](#distancia-de-hausdorff)
   - [Índice de Rand](#índice-de-rand)
4. [Funciones de costo](#funciones-de-costo)
   - [Función de Costo L1](#función-de-costo-l1)
   - [Función de Costo L2](#función-de-costo-l2)
   - [Función de Costo Normal](#función-de-costo-normal)
   - [Cambio de media con kernel (CostRbf)](#cambio-de-media-con-kernel-costrbf)
   - [Cambio de media con kernel (CostCosine)](#cambio-de-media-con-kernel-costcosine)
   - [Cambio en modelo lineal (CostLinear)](#cambio-en-modelo-lineal-costlinear)
   - [Cambio lineal continuo (CostCLinear)](#cambio-lineal-continuo-costclinearcosto)
   - [Función de costo basada en rangos (CostRank)](#función-de-costo-basada-en-rangos-costrank)
   - [Detección de cambios con métrica Mahalanobis (CostMl)](#detección-de-cambios-con-métrica-mahalanobis-costml)
   - [Cambio de modelo autorregresivo (CostAR)](#cambio-de-modelo-autorregresivo-costar)
5. [Métodos de búsqueda de puntos de cambio](#métodos-de-búsqueda-de-puntos-de-cambio)
   - [Segmentación Binaria (BinSeg)](#segmentación-binaria-binseg)
   - [Segmentación PELT](#segmentación-pelt)
   - [Segmentación Bottom-Up](#segmentación-bottom-up)
   - [Detección Window-Based](#detección-window-based)



---

En el archivo Generación_de_series.ipynb he generado dos series temporales para probar los algoritmos que voy a presentar. Los puntos de cambio en la primera serie temporal son más fáciles de detectar,
mientras que en la segunda son más difíciles de identificar.

En el archivo Varianza_constante_PELT.ipynb Se hace la evaluación del Algoritmo PELT con Varianza Constante.
En el archivo Varianza_variable_PELT.ipynb Se hace la evaluación del Algoritmo PELT con Varianza Variable.
En el archivo Varianza_constante_ChangeFinder.ipynb Se hace la evaluación del Algoritmo ChangeFinder con Varianza Constante.
En el archivo Varianza_variable_ChangeFinder.ipynb Se hace la evaluación del Algoritmo ChangeFinder con Varianza Variable.

