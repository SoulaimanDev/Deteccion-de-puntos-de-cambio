

# Detección de puntos de cambio en diversas trayectorias

## Menú de navegación
1. [Introducción](#introducción)
2. [Estado del arte](#estado-del-arte)
3. [Evaluación de Métodos](#evaluación-de-Métodos)
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
### Introducción

La detección de puntos de cambio consiste en identificar momentos en el tiempo donde las propiedades estadísticas de una serie de datos presentan variaciones. Esta técnica es clave en distintos ámbitos, como las finanzas, el monitoreo ambiental y el control de calidad, ya que entender los cambios en las tendencias de los datos permite mejorar la toma de decisiones y el análisis predictivo. Detectar estos cambios ayuda a los analistas a reaccionar de forma anticipada ante patrones o anomalías que puedan surgir en los datos.
Lo importante de detectar puntos de cambio es que ayuda a entender mejor los datos y tomar decisiones acertadas. Por ejemplo, en el clima, una variación repentina en la temperatura o la presión puede anticipar tormentas o cambios bruscos en el tiempo. En la salud, notar alteraciones en los signos vitales de un paciente puede servir para prevenir problemas graves. En general, identificar estos cambios a tiempo permite reaccionar rápido y adaptarse mejor. 
Existen varios métodos para detectar puntos de cambio, como las pruebas estadísticas, que comparan los datos antes y después del cambio, o los algoritmos de aprendizaje automático, que encuentran patrones en datos más complejos. También se utilizan enfoques bayesianos, que ajustan la detección a medida que se recibe nueva información. Cada uno de estos métodos tiene sus ventajas dependiendo del tipo de datos y el objetivo del análisis.
Aunque la detección de puntos de cambio tiene muchas ventajas, también presenta varios desafíos. Uno de los problemas más comunes es elegir el método adecuado, porque cada técnica puede dar resultados distintos dependiendo del tipo de datos que tengas. Además, el ruido y los valores atípicos pueden complicar el proceso, provocando falsos positivos o incluso dejando cambios sin detectar. Por eso, es importante que los profesionales tomen en cuenta las características de sus datos al aplicar estos métodos.
Hay varias herramientas y programas que puedes usar para detectar puntos de cambio en los datos. Lenguajes como Python y R tienen bibliotecas especiales para esto. Por ejemplo, en Python puedes usar la biblioteca ruptures, que te permite aplicar diferentes algoritmos para detectar cambios, visualizar los resultados y ajustar los parámetros según lo necesites. En R, existe el paquete changepoint, que también facilita este tipo de análisis. 

---

[Retour au menu](#Menú-de-navegación)

---

### Estado del arte

El problema de los puntos de cambio ha sido y sigue siendo un tema importante en el estudio de trayectorias, ya que permite identificar momentos clave en los que un proceso cambia. Los primeros trabajos sobre este tema fueron de Page en 1954 y 1955, quien presentó esquemas de inspección continua para detectar cambios en los datos. En 1955, también propuso una prueba estadística para identificar cambios en parámetros sin conocer el punto exacto. Más tarde, Chernoff y Zacks (1964) utilizaron un enfoque bayesiano para estimar la media de una distribución normal que cambia con el tiempo. Más recientemente, Gichuhi, Franke y Weizsacker (2008) utilizaron redes neuronales para detectar puntos de cambio en datos binarios, lo que abrió nuevas posibilidades con el aprendizaje automático.
Muchos investigadores también han trabajado en los problemas relacionados con los múltiples puntos de cambio, entre ellos. Bai y Perron (1998) propusieron métodos para estimar y probar modelos con múltiples cambios estructurales. Pan y Chen (2006) usaron un criterio de información modificado para detectar varios puntos de cambio en los datos. Por su parte, Yao (1984) planteó un enfoque bayesiano para estimar funciones escalón ruidosas, mientras que Barry y Hartigan (1992) introdujeron modelos de partición para manejar los puntos de cambio. Lee (1998) trabajó en estimar el número de puntos de cambio utilizando un enfoque bayesiano. Más recientemente, Lavielle (1999) se centró en detectar múltiples puntos de cambio en secuencias de variables dependientes, y Lai, Liu y Xing (2005) analizaron modelos autorregresivos con volatilidad constante para tratar los cambios en los parámetros de los datos.

Uno de los más conocidos es la segmentación binaria, que es bastante popular en este tipo de estudios. Es un método aproximado con una complejidad computacional de \( O(n \log n) \), donde \( n \) es el número de puntos de datos. Por otro lado, el algoritmo de vecindad de segmentos (Scott y Knott, 1974) es más preciso, ya que busca de manera exacta en todo el espacio de segmentación, pero su costo computacional es mucho mayor, \( O(Qn^2) \), donde \( Q \) es el número máximo de puntos de cambio y \( n \) es el número de datos. Si el número de puntos de cambio crece linealmente con el aumento de los datos, el costo puede llegar a ser \( O(n^3) \), lo que lo hace menos eficiente a medida que se acumulan más datos.

El método de partición óptima (Auger y Lawrence, 1989) es otra opción, que mejora la eficiencia de la vecindad de segmentos, pero sigue sin igualar la rapidez de la segmentación binaria. A pesar de esto, es un enfoque exacto, con una complejidad computacional de \( O(n^2) \), y se aplica en un tipo más reducido de problemas. 
El método PELT (Pruned Exact Linear Time) es una técnica bastante eficiente para detectar múltiples puntos de cambio en series temporales. En el estudio de Killick, Eckley y Jonathan (2011), este método se aplicó a series de datos oceánicos y mostró ser muy rápido en comparación con otras técnicas, ya que puede manejar grandes volúmenes de datos en un tiempo relativamente corto. La clave de su eficiencia está en que PELT realiza una poda en el proceso de búsqueda, lo que significa que no tiene que revisar todas las posibles divisiones de los datos, sino solo las más relevantes, haciendo que funcione en un tiempo O(n). Por otro lado, en el trabajo de Madon y Hingrat (2014), PELT se utilizó en combinación con un árbol de clasificación para analizar el comportamiento de los animales. Este enfoque ayudó a detectar cambios significativos en sus movimientos de manera rápida y precisa, lo que es especialmente útil cuando se tiene que manejar una gran cantidad de datos sobre los patrones de movimiento de los animales.

---

[Retour au menu](#Menú-de-navegación)

---

#### Evaluación


Los métodos para detectar puntos de cambio se pueden evaluar de dos formas: una es demostrando ciertas propiedades matemáticas de los algoritmos y la otra es hacerlo de forma empírica, calculando distintas métricas.

En lo que sigue, al conjunto de los puntos de cambio verdaderos lo denoto como `\mathcal{T}^* = \{ t_1^*, \ldots, t_K^* \}`, y al conjunto de los puntos de cambio estimados lo denoto como `\widehat{\mathcal{T}} = \{\hat{t}_1, \ldots, \hat{t}_{\hat{K}} \}`.
#### F1-Score

La métrica F1-Score emerge como indicador robusto para evaluar el rendimiento en esta tarea. Su cálculo se basa en dos componentes esenciales:

**Precisión (Prec)**: Mide la fiabilidad de las detecciones
```math
\text{Prec} = \frac{\text{Detecciones correctas}}{\text{Total de detecciones}} = \frac{|\text{Tp}|}{\hat{K}}
```

**Exhaustividad (Rec)**: Evalúa la capacidad de descubrimiento
```math
\text{Rec} = \frac{\text{Detecciones correctas}}{\text{Total de puntos reales}} = \frac{|\text{Tp}|}{K^*}
```

 Considero que una detección es válida cuando existe coincidencia dentro de un margen $M$ muestral:
```math
\text{Tp} = \big\{ t^* \in \mathcal{T}^* \mid \exists\, \hat{t} \in \widehat{\mathcal{T}} \,:\, |\hat{t} - t^*| < M \big\}
```

**F1-score** se define como la media armónica entre la precisión y el recall:

```math
\text{F1} = 2 \cdot \frac{\text{Prec} \cdot \text{Rec}}{\text{Prec} + \text{Rec}} \in [0,1]
```

El mejor valor posible para esta métrica es 1, indicando una detección perfecta, mientras que su peor valor es 0.
#### Hausdorff

Desde un punto de vista formal, esta métrica corresponde a la mayor distancia temporal entre un punto de cambio y su correspondiente estimación:

```math
\text{Hausdorff}(\mathcal{T}^*,\widehat{\mathcal{T}}) = \max \left\{
\underbrace{
\max_{\hat{t}\in\widehat{\mathcal{T}}} \min_{t^*\in\mathcal{T}^*} |\hat{t}-t^*|
}_{\text{Error máximo de detección}},
\underbrace{
\max_{t^*\in\mathcal{T}^*} \min_{\hat{t}\in\widehat{\mathcal{T}}} |\hat{t}-t^*|
}_{\text{Error máximo de omisión}}
\right\}
```

donde:

- El primer término evalúa la máxima distancia de cualquier punto detectado al punto real más cercano
- El segundo término mide la máxima distancia de cualquier punto real al punto detectado más cercano

Este valor representa el peor error cometido por el algoritmo que genera el conjunto de puntos estimados `\widehat{\mathcal{T}}`, y se expresa en número de muestras. Cuando su valor es cero, significa que ambos conjuntos de puntos de cambio coinciden exactamente. Por el contrario, cuanto mayor sea su valor, mayor será la distancia existente entre algún punto de cambio verdadero en `\mathcal{T}^{*}` y el punto estimado más cercano en `\widehat{\mathcal{T}}`, o viceversa.

\subsection{ Índice de Rand}

La métrica fundamental Índice de Rand  cuantifica la precisión en la detección de puntos de cambio. Esta medida estadística compara la similitud entre la segmentación obtenida `\widehat{\mathcal{T}}` y la segmentación de referencia `\mathcal{T}^{*}`, proporcionando una evaluación global del rendimiento del algoritmo.

El Índice de Rand calcula la proporción de pares de muestras que son:

- **Concordantes**: 
 
- Pertenecen al mismo segmento en ambas segmentaciones
-Pertenecen a segmentos diferentes en ambas segmentaciones

 **Discordantes**:
    
- Asignados al mismo segmento en una segmentación y a diferentes en la otra
   
Para formalizar esta idea, se definen las siguientes relaciones para un conjunto de puntos de cambio `\mathcal{T}`:

```math
\text{SameSeg}(\mathcal{T}) &:= \{(s,t) \mid 1 \leq s < t \leq T \text{ tales que } s \text{ y } t \text{ se encuentran en el mismo segmento según } \mathcal{T}\} \\
\text{DiffSeg}(\mathcal{T}) &:= \{(s,t) \mid 1 \leq s < t \leq T \text{ tales que } s \text{ y } t \text{ pertenecen a segmentos distintos según } \mathcal{T}\}
```

A partir de estas definiciones, el Índice de Rand se expresa como:

```math
\textsc{RI}(\mathcal{T}^{*},\widehat{\mathcal{T}}) := \frac{|\text{SameSeg}(\widehat{\mathcal{T}}) \cap \text{SameSeg}(\mathcal{T}^{*})| + |\text{DiffSeg}(\widehat{\mathcal{T}}) \cap \text{DiffSeg}(\mathcal{T}^{*})|}{T(T-1)/2}
```

Este valor se encuentra normalizado en el intervalo entre 0 (cuando no existe ningún acuerdo entre las segmentaciones) y 1 (cuando las segmentaciones son idénticas). 
### funciones de costo

Esta sección presenta el primer elemento definitorio de los métodos de detección de cambios, que son las funciones de costo. En la mayoría de los casos, estas funciones se derivan a partir de un modelo de señal. A continuación, se agrupa los modelos y sus funciones de costo asociadas en dos categorías: paramétricas y no paramétricas.

### Función de Costo L1

Esta función de costo detecta cambios en la mediana de una señal. En general, es un estimador robusto para detectar desplazamientos en el punto central (ya sea media, mediana o moda) de una distribución .
Formalmente, dado un segmento de señal `\{y_t\}_{t \in I}` donde $I$ representa el intervalo de análisis, el costo se calcula como:

```math
c(y_I) = \sum_{t \in I} \|y_t - \tilde{y}\|_1
```

donde `\tilde{y}` corresponde a la mediana muestral del segmento.

#### Función de Costo L2
La función CostL2 cuantifica la variabilidad alrededor de la media muestral mediante la norma euclídea al cuadrado. Para un segmento de señal `\{y_t\}_{t \in I}` donde $I$ denota el intervalo de estudio, el costo se define como:

```math
c(y_I) = \sum_{t \in I} \|y_t - \bar{y}\|_2^2 
```

donde `\tilde{y}` corresponde a la mediana muestral del segmento.

\subsection{Función de Costo Normal}
Esta función de costo permite detectar cambios tanto en la media como en la matriz de covarianza de una secuencia de variables aleatorias gaussianas multivariadas. Formalmente, para un segmento de señal `\{y_t\}_{t \in I}` con `y_t \in \mathbb{R}^d`, la función de costo se define como:

```math
c(y_I) = |I| \cdot \log \det(\widehat{\Sigma}_I + \epsilon I_d)
```math

donde:
```math
 `\widehat{\Sigma}_I = \frac{1}{|I|-1} \sum_{t \in I} (y_t - \bar{y}_I)(y_t - \bar{y}_I)^\top`
```
es la matriz de covarianza muestral del segmento.
  - `\bar{y}_I` es la media empírica del segmento.
  - `\epsilon > 0` es un término de regularización (típicamente `\epsilon = 10^{-6}`) que se añade para evitar problemas numéricos en matrices mal condicionadas.
 - `I_d` es la matriz identidad de dimensión `d \times d`.

#### Cambio de media con kernel (CostRbf)

La función **CostRbf** opera mediante el mapeo de los datos a un espacio de características `\mathcal{H}` mediante la función `\Phi(\cdot)`, donde se analizan las propiedades estadísticas. Para un segmento `\{y_t\}_{t \in I}` con `y_t \in \mathbb{R}^d`, el costo se calcula como:

```math
c(y_I) = \sum_{t \in I} \|\Phi(y_t) - \bar{\mu}\|_{\mathcal{H}}^2
```

donde `\bar{\mu} = \frac{1}{|I|}\sum_{t \in I} \Phi(y_t)` representa la media en el espacio de características.
 
El kernel radial (RBF) implementado tiene la forma:

```math
k(x,y) = \exp\left(-\gamma \|x - y\|^2\right), \quad \gamma > 0
```

donde `\gamma = 1/\text{mediana}(\{\|y_i - y_j\|^2\}_{i,j})`.

#### Cambio de media con kernel (CostCosine)

La función de costo evalúa la variabilidad en el espacio de características `\mathcal{H}` generado por el kernel coseno:

```math
c(y_{a..b}) = \sum_{t=a}^{b-1} \|\Phi(y_t) - \bar{\mu}_{a..b}\|_{\mathcal{H}}^2
```

donde:

- `\Phi: \mathbb{R}^d \rightarrow \mathcal{H}` es el mapeo al espacio de características
- `\bar{\mu}_{a..b} = \frac{1}{b-a}\sum_{t=a}^{b-1} \Phi(y_t)` es la media empírica en `\mathcal{H}`
- El kernel coseno se define como:
    ```math
    k(x,y) = \frac{\langle x, y \rangle}{\|x\|_2 \|y\|_2} \in [-1,1]
   ```


donde `\langle \cdot \mid \cdot \rangle$ y $\|\cdot\|` corresponden al producto escalar y la norma euclidiana respectivamente. Dicho de otro modo, equivale al producto punto normalizado en norma L2 de los vectores.

#### Cambio en modelo lineal (CostLinear)
Consideremos una serie temporal `\{y_t\}_{t=1}^n` con posibles puntos de cambio en `t_1, t_2, \ldots, t_k`. El modelo de regresión por segmentos se define como:

```math
y_t = x_t' \delta_j + \varepsilon_t, \quad t_j \leq t < t_{j+1}
```

donde:

- `y_t \in \mathbb{R}`: Variable respuesta
- `x_t \in \mathbb{R}^p`: Vector de covariables
- `\delta_j \in \mathbb{R}^p`: Coeficientes de regresión para el $j$-ésimo segmento
- `\varepsilon_t`: Término de error con `\mathbb{E}[\varepsilon_t] = 0`


Las estimaciones por mínimos cuadrados de las fechas de ruptura se obtienen minimizando la suma de los residuos al cuadrado. Formalmente, la función de costo asociada a un intervalo $I$ se define como:

```math
c(y_I) = \min_{\delta \in \mathbb{R}^p} \sum_{t \in I} \| y_t - \delta' x_t \|_2^2
```
#### Cambio lineal continuo (CostCLinear)
Dado un conjunto de knots  `\{t_k\}_{k=1}^K`, el spline lineal continuo `f:\mathbb{R}\rightarrow\mathbb{R}^d` se define mediante:


- **Comportamiento afín por intervalos**:
    ```math
    f(t) = \alpha_k(t - t_k) + \beta_k, \quad \alpha_k,\beta_k \in \mathbb{R}^d, \quad t \in [t_k,t_{k+1})
   ```
    
- \textbf{Condición de continuidad}:
    ```math
    \lim_{t\to t_k^-} f(t) = \lim_{t\to t_k^+} f(t), \quad \forall k
    ```

La función de costo \texttt{CostCLinear} mide el error al aproximar la señal con una spline lineal. Formalmente, se define para `0 < a < b \leq T` como:

```math
c(y_{a,b}) := \sum_{t=a}^{b-1} \left\| y_t - y_{a-1} - \frac{t-a+1}{b-a} (y_{b-1} - y_{a-1}) \right\|^2
```

y se toma `c(y_{0,b}) = c(y_{1,b})`.

#### Función de costo basada en rangos (CostRank)
La clave de este método reside en la transformación de los datos originales `\{y_t\}_{t=1}^T` a sus rangos `\{r_t\}_{t=1}^T`, donde:

```math
r_t = \text{rango}(y_t \text{ en } \{y_1,\ldots,y_T\})
```

Para un segmento `y_{a..b}`, la función de costo se define como:

```math
c_{\text{rank}}(a,b) = -(b-a) \bar{r}_{a..b}^\prime \widehat{\Sigma}_r^{-1} \bar{r}_{a..b}
```

donde:

- `\bar{r}_{a..b} = \frac{1}{b-a}\sum_{t=a+1}^b r_t` es la media de rangos en el segmento
- `\widehat{\Sigma}_r` es la matriz de covarianza estimada de los rangos completos

#### Detección de cambios con una métrica de tipo Mahalanobis (CostMl)

Dada una matriz semidefinida positiva `M \in \mathbb{R}^{d \times d}`, definimos la pseudométrica:

```math
\|x - y\|_{M}^2 = (x - y)^T M (x - y)
```

Para un segmento de señal `\{y_t\}_{t \in I}`, la función de costo se calcula como:

```math
c(y_I) = \sum_{t \in I} \|y_t - \bar{\mu}\|_M^2
```

donde `\bar{\mu} = \frac{1}{|I|}\sum_{t \in I} y_t` es la media empírica del segmento.

#### Cambio de modelo autorregresivo (CostAR)

Considerando una serie temporal `\{y_t\}_{t=1}^n` con posibles puntos de cambio en `\{t_j\}_{j=1}^k`, el modelo `AR``(p)` por segmentos se define como:

```math
y_t = \sum_{i=1}^p \delta_{j,i} y_{t-i} + \epsilon_t, \quad t_j \leq t < t_{j+1}
```

donde:

- `p`: Orden del modelo (seleccionado mediante AIC en mi implementación)
- `\delta_j \in \mathbb{R}^p`: Coeficientes AR para el `j`-ésimo segmento
- `\epsilon_t`: Innovaciones con `\mathbb{E}[\epsilon_t] = 0`, `\text{Var}(\epsilon_t) = \sigma^2`

La función de costo implementada minimiza la suma de residuos al cuadrado:

```math
c(y_I) = \min_{\delta \in \mathbb{R}^p} \sum_{t \in I} (y_t - \delta' z_t)^2
```

con `z_t = [y_{t-1}, \ldots, y_{t-p}]'`.
### Métodos de búsqueda de puntos de cambio

Esta sección presenta el segundo elemento definitorio de los métodos de detección de cambios, concretamente el **método de búsqueda**. Los algoritmos de búsqueda determinan cómo se exploran las posibles configuraciones de puntos de cambio en la serie temporal, afectando tanto a la precisión como a la eficiencia computacional del método.
#### Segmentación Binaria (BinSeg)

El algoritmo de **Segmentación Binaria (BinSeg)** es un método iterativo para detectar puntos de cambio en series temporales que opera mediante una estrategia greedy. En cada iteración, el algoritmo identifica el punto de cambio óptimo que minimiza la suma de costos de los segmentos adyacentes:

```math
\hat{t}^{(k)} = \underset{a<t<b}{\text{argmin}} \left[c(y_{a..t}) + c(y_{t..b})\right]
```

donde `c(\cdot)` representa típicamente el error cuadrático medio. A pesar de su eficiencia computacional (`\mathcal{O}(n \log n)`), el enfoque greedy implica que cada punto de cambio se estima condicionado a los cambios anteriores, lo que puede afectar la optimalidad global. 
## Algoritmo BinSeg (Binary Segmentation)

**Input**: 
- Señal `{y_t}_{t=1}^T` (serie temporal)
- Función de costo `c(·)` (ej: L1, L2, normal)
- Criterio de parada (ej: número máximo de cambios o umbral de ganancia)

**Output**: 
- Conjunto `L` de puntos de cambio estimados (índices)

```python
1: Initialize L = ∅  # Lista vacía de puntos de cambio
2: repeat
3:   k = |L|  # Número actual de puntos de cambio
4:   t₀ = 0, t_{k+1} = T  # Límites iniciales
5:   if k > 0 then
6:     Sort L = {t₁, ..., t_k} ascendentemente  # Ordenar puntos existentes
7:   end if
8:   Initialize array G of length k+1  # Almacenará ganancias por segmento
9:   for i = 0 to k do
10:    # Calcular ganancia potencial para el segmento actual
11:    G[i] = c(y_{t_i..t_{i+1}}) - min_{t_i < t < t_{i+1}} [c(y_{t_i..t}) + c(y_{t..t_{i+1}})]
12:  end for
13:  î = argmax_i G[i]  # Índice del segmento con máxima ganancia
14:  # Encontrar mejor punto de división en el segmento seleccionado
15:  t̂ = argmin_{t_î < t < t_{î+1}} [c(y_{t_î..t}) + c(y_{t..t_{î+1}})]
16:  L = L ∪ {t̂}  # Añadir nuevo punto de cambio
17: until stopping criterion is met  # Ej: |L| ≥ K_max o max(G) < ε
18: return sorted(L)  # Devuelve puntos ordenados
```




El algoritmo termina cuando se alcanza un número máximo de cambios o cuando la máxima ganancia $G[i]$ está por debajo de un umbral predefinido. Esta aproximación balancea eficiencia computacional con capacidad de detección, siendo particularmente útil cuando el número de segmentos es desconocido a priori.


#### Segmentación PELT (Pruned Exact Linear Time)
El método PELT es un algoritmo de detección exacta de puntos de cambio que combina optimalidad global con eficiencia computacional mediante técnicas de poda dinámica. A diferencia de métodos aproximados como BinSeg, PELT garantiza encontrar la partición óptima de la serie temporal minimizando:

```math
\sum_{i=1}^{m+1} c(y_{\tau_{i-1}:\tau_i}) + \beta m
```

donde $c(\cdot)$ es la función de costo, $\beta$ el parámetro de penalización y $m$ el número de cambios. La clave del algoritmo reside en su capacidad para descartar particiones subóptimas manteniendo únicamente las soluciones relevantes.

## Algoritmo PELT (Pruned Exact Linear Time)

**Input**:
- Señal `{y_t}_{t=1}^T`
- Función de costo `c(·)`
- Parámetro de penalización `β`

**Output**:
- Conjunto `L[T]` de puntos de cambio estimados

```python
1: Initialize Z[0] = -β  # Costos acumulados
2: Initialize L[0] = ∅   # Lista de cambios
3: Initialize χ = {0}    # Conjunto activo de índices
4: for t = 1 to T do
5:   # Encontrar punto óptimo previo
6:   t̂ = argmin_{s ∈ χ} [Z[s] + c(y_{s:t}) + β]
7:   
8:   # Actualizar costo acumulado
9:   Z[t] = Z[t̂] + c(y_{t̂:t}) + β
10:  
11:  # Registrar cambios
12:  L[t] = L[t̂] ∪ {t̂}
13:  
14:  # Poda dinámica
15:  χ = {s ∈ χ | Z[s] + c(y_{s:t}) ≤ Z[t]} ∪ {t}
16: end for
17: return L[T]
```


El algoritmo mantiene un conjunto activo $\chi$ de índices candidatos, actualizando en cada iteración tanto los costos acumulados $Z[t]$ como la lista de cambios $L[t]$. La regla de poda (línea 9) asegura que solo se conserven las particiones que potencialmente pueden llevar a la solución óptima global, reduciendo así la complejidad computacional sin sacrificar precisión.
#### Segmentación Bottom-Up
El método Bottom-Up es un enfoque no greedy para la detección de puntos de cambio que opera mediante fusión progresiva de segmentos. A diferencia de métodos como BinSeg que dividen la señal, este algoritmo sigue una estrategia de unificación: comienza con una partición inicial fina (definida por un parámetro de grilla $\delta$) y fusiona iterativamente los pares de segmentos más similares hasta satisfacer un criterio de parada. Matemáticamente, el proceso optimiza:

```math
\min_{L} \sum_{i=0}^{k} c(y_{t_i:t_{i+1}})
```

donde $L = \{t_1, \ldots, t_k\}$ son los puntos de cambio y $c(\cdot)$ es la función de costo. La clave del método reside en su métrica de fusión:

```math
G[i] = c(y_{t_{i-1}:t_{i+1}}) - \left[c(y_{t_{i-1}:t_i}) + c(y_{t_i:t_{i+1}})\right]
```
que cuantifica la ganancia al fusionar dos segmentos adyacentes.

## Algoritmo Bottom-Up

**Input**:
- Señal `{y_t}_{t=1}^T`
- Función de costo `c(·)`
- Tamaño de grilla `δ > 2`
- Criterio de parada

**Output**:
- Conjunto `L` de puntos de cambio estimados

```python
1: Initialize L = {δ, 2δ, ..., (⌊T/δ⌋-1)δ}  # Puntos iniciales en grilla
2: repeat
3:   k = |L|  # Número actual de puntos de cambio
4:   Sort L = {t₁, ..., t_k} ascendentemente
5:   Initialize array G of length k-1  # Ganancias de fusión
6:   for i = 1 to k-1 do
7:     # Calcular ganancia de fusión
8:     G[i-1] = c(y_{t_{i-1}:t_{i+1}}) - [c(y_{t_{i-1}:t_i}) + c(y_{t_i:t_{i+1}})]
9:   end for
10:  î = argmin_i G[i]  # Seleccionar fusión óptima
11:  L = L \ {t_{î+1}}  # Eliminar punto de cambio
12: until stopping criterion is met
13: return L
```
#### Detección Window-Based

El método \textit{Window-Based} es un algoritmo eficiente para detectar puntos de cambio mediante el análisis de discrepancia entre \textbf{segmentos adyacentes de tamaño \(w\)}. Utiliza dos fragmentos de la señal \(\{y_t\}_{t=1}^T\) que se deslizan a lo largo de ella, comparando sus propiedades estadísticas mediante una medida de discrepancia derivada de la función de costo \(c(\cdot)\).

```math
d(y_{u..w}, y_{v..w}) = c(y_{u..w}) - [c(y_{v..w}) + c(y_{u..v})]
```

donde $u < v < w$ son índices temporales. La curva de discrepancia se define para cada punto $t$ como:

```math
Z[t] = c(y_{t-w..t+w}) - [c(y_{t-w..t}) + c(y_{t..t+w})]
```

Los picos en esta curva indican potenciales puntos de cambio, detectados mediante un procedimiento de búsqueda de máximos locales (PKSearch).

## Algoritmo Window-Based

**Input**:
- Señal `{y_t}_{t=1}^T`
- Función de costo `c(·)`
- Ancho de media ventana `w`
- Procedimiento `PKSearch` (detección de picos)

**Output**:
- Conjunto `L` de puntos de cambio estimados

```python
1: Initialize Z = [0,...,0]  # Array de longitud T
2: for t = w to T-w do
3:   p = (t-w)..t      # Ventana izquierda (pasado)
4:   q = t..(t+w)      # Ventana derecha (futuro)
5:   r = (t-w)..(t+w)  # Ventana combinada
6:   Z[t] = c(y_r) - [c(y_p) + c(y_q)]  # Cálculo de discrepancia
7: end for
8: L = PKSearch(Z)     # Detectar picos en Z
9: return L
```

---

[Retour au menu](#Menú-de-navegación)

---


---

En el archivo Generación_de_series.ipynb he generado dos series temporales para probar los algoritmos que voy a presentar. Los puntos de cambio en la primera serie temporal son más fáciles de detectar,
mientras que en la segunda son más difíciles de identificar.

En el archivo Varianza_constante_PELT.ipynb Se hace la evaluación del Algoritmo PELT con Varianza Constante.
En el archivo Varianza_variable_PELT.ipynb Se hace la evaluación del Algoritmo PELT con Varianza Variable.
En el archivo Varianza_constante_ChangeFinder.ipynb Se hace la evaluación del Algoritmo ChangeFinder con Varianza Constante.
En el archivo Varianza_variable_ChangeFinder.ipynb Se hace la evaluación del Algoritmo ChangeFinder con Varianza Variable.

