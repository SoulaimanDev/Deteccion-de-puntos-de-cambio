

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

### Evaluación


Los métodos para detectar puntos de cambio se pueden evaluar de dos formas: una es demostrando ciertas propiedades matemáticas de los algoritmos y la otra es hacerlo de forma empírica, calculando distintas métricas.

En lo que sigue, al conjunto de los puntos de cambio verdaderos lo denoto como T* = { t1*, ..., tK* }, y al conjunto de los puntos de cambio estimados lo denoto como T̂ = { t̂1, ..., t̂K̂ }.

#### F1-Score

La métrica F1-Score emerge como indicador robusto para evaluar el rendimiento en esta tarea. Su cálculo se basa en dos componentes esenciales:

**Precisión (Prec)**: Mide la fiabilidad de las detecciones  

$$
\text{Prec} = \frac{\text{Detecciones correctas}}{\text{Total de detecciones}} = \frac{|\text{Tp}|}{\hat{K}}
$$

**Exhaustividad (Rec)**: Evalúa la capacidad de descubrimiento

$$
\text{Rec} = \frac{\text{Detecciones correctas}}{\text{Total de puntos reales}} = \frac{|\text{Tp}|}{K^*}
$$

Considero que una detección es válida cuando existe coincidencia dentro de un margen $M$ muestral:

```math
\text{Tp} = \{ t^* \in \mathcal{T}^* \mid \exists\, \hat{t} \in \widehat{\mathcal{T}} \text{ tel que } |\hat{t} - t^*| < M \}
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
\end{itemize}
Este valor representa el peor error cometido por el algoritmo que genera el conjunto de puntos estimados $\(\widehat{\mathcal{T}}\)$, y se expresa en número de muestras. Cuando su valor es cero, significa que ambos conjuntos de puntos de cambio coinciden exactamente. Por el contrario, cuanto mayor sea su valor, mayor será la distancia existente entre algún punto de cambio verdadero en $\(\mathcal{T}^{*}\)$ y el punto estimado más cercano en $\(\widehat{\mathcal{T}}\)$, o viceversa.

#### Índice de Rand

La métrica fundamental Índice de Rand  cuantifica la precisión en la detección de puntos de cambio. Esta medida estadística compara la similitud entre la segmentación obtenida $\widehat{\mathcal{T}}$ y la segmentación de referencia $\mathcal{T}^{*}$, proporcionando una evaluación global del rendimiento del algoritmo.

El Índice de Rand calcula la proporción de pares de muestras que son:

**Concordantes:**
    
- Pertenecen al mismo segmento en ambas segmentaciones
- Pertenecen a segmentos diferentes en ambas segmentaciones
    
**Discordantes:**

- Asignados al mismo segmento en una segmentación y a diferentes en la otra
    



Para formalizar esta idea, se definen las siguientes relaciones para un conjunto de puntos de cambio $\(\mathcal{T}\)$:

$$
\text{SameSeg}(\mathcal{T}) := \{(s,t) \mid 1 \leq s < t \leq T \text{ donde } s \text{ y } t \text{ están en el mismo segmento según } \mathcal{T}\}
$$

$$
\text{DiffSeg}(\mathcal{T}) := \{(s,t) \mid 1 \leq s < t \leq T \text{ donde } s \text{ y } t \text{ están en segmentos distintos según } \mathcal{T}\}
$$

A partir de estas definiciones, el Índice de Rand se expresa como:

```math
RI(\mathcal{T}^{*},\widehat{\mathcal{T}}) := \frac{|\text{SameSeg}(\widehat{\mathcal{T}}) \cap \text{SameSeg}(\mathcal{T}^{*})| + |\text{DiffSeg}(\widehat{\mathcal{T}}) \cap \text{DiffSeg}(\mathcal{T}^{*})|}{T(T-1)/2}
```

Este valor se encuentra normalizado en el intervalo entre 0 (cuando no existe ningún acuerdo entre las segmentaciones) y 1 (cuando las segmentaciones son idénticas). 



---

[Retour au menu](#Menú-de-navegación)

---
### Funciones de costo

Esta sección presenta el primer elemento definitorio de los métodos de detección de cambios, que son las funciones de costo. En la mayoría de los casos, estas funciones se derivan a partir de un modelo de señal. A continuación, se agrupa los modelos y sus funciones de costo asociadas en dos categorías: paramétricas y no paramétricas.

#### Función de Costo L1

Esta función de costo detecta cambios en la mediana de una señal. En general, es un estimador robusto para detectar desplazamientos en el punto central (ya sea media, mediana o moda) de una distribución.

Formalmente, dado un segmento de señal `{y_t}_{t ∈ I}` donde `I` representa el intervalo de análisis, el costo se calcula como:

```math
c(y_I) = ∑_{t ∈ I} ‖y_t - ȳ‖_1
```

donde `ȳ` corresponde a la mediana muestral del segmento.

#### Función de Costo L2

La función CostL2 cuantifica la variabilidad alrededor de la media muestral mediante la norma euclídea al cuadrado. Para un segmento de señal `{y_t}_{t ∈ I}` donde `I` denota el intervalo de estudio, el costo se define como:

```math
c(y_I) = ∑_{t ∈ I} ‖y_t - ȳ‖_2^2 
```

donde `ȳ` corresponde a la media muestral del segmento.

#### Función de Costo Normal

Esta función de costo permite detectar cambios tanto en la media como en la matriz de covarianza de una secuencia de variables aleatorias gaussianas multivariadas. Formalmente, para un segmento de señal `{y_t}_{t ∈ I}` con `y_t ∈ ℝ^d`, la función de costo se define como:

```math
c(y_I) = |I| · log det(Σ̂_I + εI_d)
```

donde:
- `Σ̂_I = 1/(|I|-1) ∑_{t ∈ I} (y_t - ȳ_I)(y_t - ȳ_I)^⊤` es la matriz de covarianza muestral del segmento
- `ȳ_I` es la media empírica del segmento
- `ε > 0` es un término de regularización (típicamente `ε = 10^{-6}`)
- `I_d` es la matriz identidad de dimensión `d × d`

#### Cambio de media con kernel (CostRbf)

La función `CostRbf` opera mediante el mapeo de los datos a un espacio de características `H` mediante la función `Φ(·)`. Para un segmento `{y_t}_{t ∈ I}` con `y_t ∈ ℝ^d`, el costo se calcula como:

```math
c(y_I) = ∑_{t ∈ I} ‖Φ(y_t) - μ̄‖_H^2
```

donde `μ̄ = 1/|I| ∑_{t ∈ I} Φ(y_t)` representa la media en el espacio de características.

El kernel radial (RBF) implementado tiene la forma:

```math
k(x,y) = exp(-γ‖x - y‖^2), γ > 0
```

donde `γ = 1/mediana({‖y_i - y_j‖^2}_{i,j})`.

#### Cambio de media con kernel (CostCosine)

La función de costo evalúa la variabilidad en el espacio de características `H` generado por el kernel coseno:

```math
c(y_{a..b}) = ∑_{t=a}^{b-1} ‖Φ(y_t) - μ̄_{a..b}‖_H^2
```

donde:
- `Φ: ℝ^d → H` es el mapeo al espacio de características
- `μ̄_{a..b} = 1/(b-a) ∑_{t=a}^{b-1} Φ(y_t)` es la media empírica en `H`
- El kernel coseno se define como:
  ```math
  k(x,y) = (⟨x, y⟩)/(‖x‖_2 ‖y‖_2) ∈ [-1,1]
  ```

#### Cambio en modelo lineal (CostLinear)

Consideremos una serie temporal `{y_t}_{t=1}^n` con posibles puntos de cambio en `t_1, t_2, ..., t_k`. El modelo de regresión por segmentos se define como:

```math
y_t = x_t' δ_j + ε_t, t_j ≤ t < t_{j+1}
```

donde:
- `y_t ∈ ℝ`: Variable respuesta
- `x_t ∈ ℝ^p`: Vector de covariables
- `δ_j ∈ ℝ^p`: Coeficientes de regresión para el j-ésimo segmento
- `ε_t`: Término de error con `𝔼[ε_t] = 0`

La función de costo asociada a un intervalo `I` se define como:

```math
c(y_I) = min_{δ ∈ ℝ^p} ∑_{t ∈ I} ‖ y_t - δ' x_t ‖_2^2
```

#### Cambio lineal continuo (CostCLinear)

Dado un conjunto de knots `{t_k}_{k=1}^K`, el spline lineal continuo `f:ℝ→ℝ^d` se define mediante:

- **Comportamiento afín por intervalos**:
  ```math
  f(t) = α_k(t - t_k) + β_k, α_k,β_k ∈ ℝ^d, t ∈ [t_k,t_{k+1})
  ```
- **Condición de continuidad**:
  ```math
  lim_{t→t_k^-} f(t) = lim_{t→t_k^+} f(t), ∀k
  ```

La función de costo `CostCLinear` se define para `0 < a < b ≤ T` como:

```math
c(y_{a,b}) := ∑_{t=a}^{b-1} ‖ y_t - y_{a-1} - (t-a+1)/(b-a) (y_{b-1} - y_{a-1}) ‖^2
```

#### Función de costo basada en rangos (CostRank)

La transformación de los datos originales `{y_t}_{t=1}^T` a sus rangos `{r_t}_{t=1}^T`:

```math
r_t = text{rango}(y_t text{ en } {y_1,...,y_T})
```

Para un segmento `y_{a..b}`, la función de costo:

```math
c_{rank}(a,b) = -(b-a) r̄_{a..b}' Σ̂_r^{-1} r̄_{a..b}
```

donde:
- `r̄_{a..b} = 1/(b-a)∑_{t=a+1}^b r_t` es la media de rangos
- `Σ̂_r` es la matriz de covarianza estimada de los rangos completos

#### Detección de cambios con métrica Mahalanobis (CostMl)

Dada una matriz semidefinida positiva `M ∈ ℝ^{d × d}`, la pseudométrica:

```math
‖x - y‖_{M}^2 = (x - y)^T M (x - y)
```

Para un segmento `{y_t}_{t ∈ I}`, la función de costo:

```math
c(y_I) = ∑_{t ∈ I} ‖y_t - μ̄‖_M^2
```

donde `μ̄ = 1/|I|∑_{t ∈ I} y_t` es la media empírica.

#### Cambio de modelo autorregresivo (CostAR)

Para una serie temporal `{y_t}_{t=1}^n` con posibles puntos de cambio, el modelo AR(p) por segmentos:

```math
y_t = ∑_{i=1}^p δ_{j,i} y_{t-i} + ϵ_t, t_j ≤ t < t_{j+1}
```

La función de costo implementada:

```math
c(y_I) = min_{δ ∈ ℝ^p} ∑_{t ∈ I} (y_t - δ' z_t)^2
```

con `z_t = [y_{t-1}, ..., y_{t-p}]'`.



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

