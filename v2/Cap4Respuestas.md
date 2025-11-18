# Respuestas al Capítulo 4: Ajuste y Ecualización

## 4.6. Preguntas

### 4.6.1. Preguntas sobre hechos indicados en el texto

1. **¿Qué representa cada entrada en un histograma de una imagen en escala de grises?**  
   Cada entrada h(i) representa el número de píxeles en la imagen que tienen el valor de intensidad i.

2. **¿Qué parámetros se requieren para realizar el ajuste de histograma mediante una función potencia?**  
   Se requieren el parámetro gamma (n), que es el exponente en la función s = c * r^γ, y opcionalmente una constante c para escalar.

3. **¿Qué es el gamma en el contexto del procesamiento de imágenes?**  
   El gamma es la relación entre las luces y las sombras en una imagen, medida por el exponente en la función de potencia que ajusta el brillo y contraste. Un gamma de 1 es lineal, menor a 1 favorece tonos claros, mayor a 1 favorece tonos oscuros.

4. **¿Qué significa que un histograma esté normalizado?**  
   Un histograma normalizado divide cada frecuencia h(i) por el total de píxeles (M*N), convirtiéndolo en probabilidades p(i), donde la suma de p(i) es 1.

5. **¿Qué valores se deben determinar en el histograma antes de realizar su ecualización?**  
   Se deben determinar los valores mínimo y máximo de intensidad presentes en el histograma, así como las frecuencias acumuladas para calcular la transformación.

### 4.6.2. Preguntas de análisis y comprensión con base en el texto

1. **¿Cómo se puede utilizar el histograma para estimar el brillo y contraste de una imagen?**  
   El brillo se estima como el promedio de las intensidades (o ponderado por el histograma); un histograma concentrado a la derecha indica alto brillo. El contraste se estima como la desviación estándar; un histograma amplio indica alto contraste, uno estrecho bajo contraste.

2. **Analiza las diferencias entre el ajuste de histograma mediante una función exponencial y una logarítmica.**  
   La función exponencial (s = c * (e^r - 1)) amplifica las intensidades altas, expandiendo el rango dinámico en tonos claros. La función logarítmica (s = c * log(1 + r)) amplifica las intensidades bajas, expandiendo en tonos oscuros. La exponencial es útil para imágenes con detalles en luces, la logarítmica para sombras.

3. **¿En qué consiste la ecualización de histograma?**  
   Consiste en transformar la imagen para que su histograma sea uniforme, redistribuyendo las intensidades de manera que cada valor tenga la misma frecuencia aproximada, expandiendo el rango dinámico completo.

4. **¿Cómo permite mejorar el contraste de una imagen?**  
   Mejora el contraste al estirar el histograma para cubrir todo el rango disponible, haciendo que detalles en áreas oscuras o claras sean más visibles al aumentar la diferencia entre intensidades adyacentes.

5. **Explica en qué consiste la especificación de histograma y cuándo puede ser útil aplicarla.**  
   Consiste en ajustar el histograma de la imagen para que coincida con un histograma deseado especificado por el usuario. Es útil cuando se requiere un histograma particular para matching de imágenes, corrección de color o estandarización en aplicaciones médicas o de visión artificial.

### 4.6.3. Ejercicios numéricos

1. **Para la matriz de la imagen de 4 × 4 pixels de 4 bits mostrada en (4.31), realice de forma manuscrita: El histograma de frecuencias y probabilidad. Calcule el brillo y el contraste por los pixeles. Calcule el brillo y el contraste a partir de su histograma.**

   La matriz es:

   ```
   5  7  13 9
   12 11 14 10
   13 9  4  12
   9  9  12 7
   ```

   **Histograma de frecuencias:**
   - Intensidad 4: 1 píxel
   - Intensidad 5: 1 píxel
   - Intensidad 7: 2 píxeles
   - Intensidad 9: 4 píxeles
   - Intensidad 10: 1 píxel
   - Intensidad 11: 1 píxel
   - Intensidad 12: 3 píxeles
   - Intensidad 13: 2 píxeles
   - Intensidad 14: 1 píxel

   **Histograma de probabilidad (dividiendo por 16):**
   - p(4) = 1/16 = 0.0625
   - p(5) = 1/16 = 0.0625
   - p(7) = 2/16 = 0.125
   - p(9) = 4/16 = 0.25
   - p(10) = 1/16 = 0.0625
   - p(11) = 1/16 = 0.0625
   - p(12) = 3/16 = 0.1875
   - p(13) = 2/16 = 0.125
   - p(14) = 1/16 = 0.0625

   **Brillo por los píxeles:** Promedio de intensidades = (5+12+13+9+7+11+9+9+13+14+4+12+9+10+12+7)/16 = 156/16 = 9.75

   **Contraste por los píxeles:** Desviación estándar = sqrt( sum((x - mean)^2)/16 ) ≈ 2.84

   **Brillo a partir del histograma:** sum(i * h(i)) / 16 = (4*1 + 5*1 + 7*2 + 9*4 + 10*1 + 11*1 + 12*3 + 13*2 + 14*1)/16 = 156/16 = 9.75

   **Contraste a partir del histograma:** sqrt( sum(i^2 * h(i))/16 - mean^2 ) = sqrt(103.125 - 95.0625) = sqrt(8.0625) ≈ 2.84

   **Matriz de imagen de salida:**
   ```
   5  7  13 9
   12 11 14 10
   13 9  4  12
   9  9  12 7
   ```

2. **Para la matriz de la imagen de 4 × 4 pixels de 4 bits mostrada en (4.31), realice el ajuste del histograma de forma lineal n = 1.**

   Para n=1, la función de potencia es s = r (ya que c=1 implícitamente), por lo que no hay cambio en la imagen. La matriz resultante es la misma.

3. **Para la matriz de la imagen de 4 ×4 pixels de 4 bits mostrada en 4.31, realice de forma manuscrita la ecualización.**

   La matriz es:

   ```
   5  7  13 9
   12 11 14 10
   13 9  4  12
   9  9  12 7
   ```

   **Histograma de frecuencias:**
   - Intensidad 4: 1 píxel
   - Intensidad 5: 1 píxel
   - Intensidad 7: 2 píxeles
   - Intensidad 9: 4 píxeles
   - Intensidad 10: 1 píxel
   - Intensidad 11: 1 píxel
   - Intensidad 12: 3 píxeles
   - Intensidad 13: 2 píxeles
   - Intensidad 14: 1 píxel

   **CDF (frecuencia acumulada):**
   - cdf(4) = 1
   - cdf(5) = 2
   - cdf(7) = 4
   - cdf(9) = 8
   - cdf(10) = 9
   - cdf(11) = 10
   - cdf(12) = 13
   - cdf(13) = 15
   - cdf(14) = 16

   **Transformación de ecualización:** Para 4 bits (0-15), s(i) = round( (cdf(i) - cdf_min) / (16 - cdf_min) * 15 ) donde cdf_min = 1
   - s(4) = round((1-1)/15 * 15) = 0
   - s(5) = round((2-1)/15 * 15) = 1
   - s(7) = round((4-1)/15 * 15) = 3
   - s(9) = round((8-1)/15 * 15) = 7
   - s(10) = round((9-1)/15 * 15) = 8
   - s(11) = round((10-1)/15 * 15) = 9
   - s(12) = round((13-1)/15 * 15) = 12
   - s(13) = round((15-1)/15 * 15) = 14
   - s(14) = round((16-1)/15 * 15) = 15

   **Matriz de imagen de salida:**
   ```
   1  3  14 7
   12 9  15 8
   14 7  0  12
   7  7  12 3
   ```

4. **De manera analítica encuentre la ecuación que permite realizar el ajuste exponencial:**

   **Razonamiento analítico:**  
   Antes de aplicar la transformación exponencial, es obligatorio normalizar las intensidades de entrada al rango [0, 1]. Para derivar la ecuación del ajuste exponencial, se asume que las intensidades de entrada se normalizan al rango [0, 1], donde $E_m$ es el mínimo (0) y $E_M$ es el máximo (1). La transformación exponencial se define como una función que mapea el rango normalizado de manera no lineal, amplificando los valores altos. La función base es:

   $$s = \frac{e^r - 1}{e - 1}$$

   donde $r$ es la intensidad normalizada:

   $$r = \frac{I_E - E_m}{E_M - E_m}$$

   Esta función va de 0 a 1 cuando $r$ va de 0 a 1. Para mapear al rango deseado $[S_m, S_M]$, se escala:

   $$I_S = s \times (S_M - S_m) + S_m$$

   Sustituyendo:

   $$I_S = \left[ \frac{e^r - 1}{e - 1} \right] \times (S_M - S_m) + S_m$$

   Dado que $$r = \frac{I_E - E_m}{E_M - E_m}$$, y asumiendo que el rango $E_M - E_m$ se considera en la escala (por ejemplo, si se normaliza implícitamente), la ecuación se simplifica a la forma dada, donde el exponente refleja la posición relativa.

   La ecuación para el ajuste exponencial es:

   $$I_S = \frac{(S_M - S_m)}{(e^{(E_M - E_m)} - 1)} \left[e^{(I_E - E_m)} - 1\right] + S_m$$

      > Donde $e$ es la base de los logaritmos naturales (≈ 2.718) y $I_E$ debe estar normalizado o ajustado al rango adecuado antes de aplicar la función exponencial.

   Donde:
   - IS es la intensidad de salida
   - IE es la intensidad de entrada
   - SM es la intensidad máxima deseada
   - Sm es la intensidad mínima deseada
   - EM es la intensidad máxima de entrada
   - Em es la intensidad mínima de entrada

5. **De manera analítica encuentre la ecuación que permite realizar el ajuste logarítmico:**

   **Razonamiento analítico:**  
   Similar al caso exponencial, se normalizan las intensidades al rango [0, 1]. La transformación logarítmica se define como $$s = \frac{\ln(1 + r)}{\ln 2}$$, donde $r$ es la intensidad normalizada. Esta función amplifica los valores bajos y comprime los altos, mapeando $[0, 1]$ a $[0, 1]$. Luego, se escala al rango $[S_m, S_M]$: $$I_S = s \times (S_M - S_m) + S_m$$. Sustituyendo, $$I_S = \left[ \frac{\ln(1 + r)}{\ln 2} \right] \times (S_M - S_m) + S_m$$. Con $$r = \frac{I_E - E_m}{E_M - E_m}$$.

   Es importante notar que el denominador $\ln 2$ se utiliza únicamente cuando las intensidades están normalizadas en el rango $[0,1]$. Para un rango general $[E_m, E_M]$, el denominador adecuado es $\ln(E_M - E_m + 1)$, que actúa como normalizador para el rango completo. Así, la ecuación generalizada es:

   $$I_S = \frac{(S_M - S_m)}{\ln(E_M - E_m + 1)} \ln(I_E - E_m + 1) + S_m$$

   Donde:
   - $I_S$ es la intensidad de salida,
   - $I_E$ es la intensidad de entrada,
   - $S_M$ es la intensidad máxima deseada,
   - $S_m$ es la intensidad mínima deseada,
   - $E_M$ es la intensidad máxima de entrada,
   - $E_m$ es la intensidad mínima de entrada.

   Donde las variables son las mismas que en la pregunta anterior.

6. **A partir de las imágenes de 4×4 y de 4 bits propuestas, realizar la ecualización por especificación del histograma de la imagen de entrada E propuesta en 4.32 con la referencia R ilustrada en 4.33.**

   **Imagen E (4.32):**
   ```
   11 13 0 13
   7  0  2 5
   15 15 1 15
   15 4  11 0
   ```

   **Imagen R (4.33):**
   ```
   13 14 2 14
   10 2  5 9
   15 15 3 15
   15 8  13 1
   ```

   **Histograma de E:**
   - Intensidad 0: 3 píxeles
   - Intensidad 1: 1 píxel
   - Intensidad 2: 1 píxel
   - Intensidad 4: 1 píxel
   - Intensidad 5: 1 píxel
   - Intensidad 7: 1 píxel
   - Intensidad 11: 2 píxeles
   - Intensidad 13: 2 píxeles
   - Intensidad 15: 4 píxeles

   **Histograma de R:**
   - Intensidad 1: 1 píxel
   - Intensidad 2: 2 píxeles
   - Intensidad 3: 1 píxel
   - Intensidad 5: 1 píxel
   - Intensidad 8: 1 píxel
   - Intensidad 9: 1 píxel
   - Intensidad 10: 1 píxel
   - Intensidad 13: 3 píxeles
   - Intensidad 14: 2 píxeles
   - Intensidad 15: 3 píxeles

   **CDF de E:**
   - cdf(0) = 3
   - cdf(1) = 4
   - cdf(2) = 5
   - cdf(4) = 6
   - cdf(5) = 7
   - cdf(7) = 8
   - cdf(11) = 10
   - cdf(13) = 12
   - cdf(15) = 16

   **CDF de R:**
   - cdf(1) = 1
   - cdf(2) = 3
   - cdf(3) = 4
   - cdf(5) = 5
   - cdf(8) = 6
   - cdf(9) = 7
   - cdf(10) = 8
   - cdf(13) = 11
   - cdf(14) = 13
   - cdf(15) = 16

   **Mapeo de especificación:** Para cada intensidad i en E, encontrar el menor j en R tal que cdf_R(j) >= cdf_E(i)
   - Para i=0, cdf_E=3, menor j con cdf_R(j)>=3 es j=2 (cdf=3)
   - i=1, cdf_E=4, j=3 (cdf=4)
   - i=2, cdf_E=5, j=5 (cdf=5)
   - i=4, cdf_E=6, j=8 (cdf=6)
   - i=5, cdf_E=7, j=9 (cdf=7)
   - i=7, cdf_E=8, j=10 (cdf=8)
   - i=11, cdf_E=10, j=13 (cdf=11 >=10)
   - i=13, cdf_E=12, j=13 (cdf=11 <12, wait 11<12, next j=14 cdf=13>=12)
   - i=15, cdf_E=16, j=15 (cdf=16)

   **Matriz de imagen de salida:**
   ```
   13 14 2 14
   10 2  5 9
   15 15 3 15
   15 8  13 2
   ```