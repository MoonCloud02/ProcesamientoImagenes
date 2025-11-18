# Respuestas al Capítulo 5: Modelos Básicos del Color

## 5.5. Preguntas

### 5.5.1. Preguntas directas sobre el texto

1. **¿Cuál es la principal razón evolutiva propuesta para el origen de la visión del color en los animales, según el texto?**

   Una de las explicaciones propuestas indica que la visión del color podría haber aparecido como una adaptación específica para observar las plantas y los animales. Esta capacidad perceptiva surge en animales cuyos sistemas nerviosos están organizados para codificar y evaluar variaciones en las distribuciones espectrales de la luz, proporcionando información valiosa para la toma de decisiones.

2. **En la teoría simplificada de la visión del color mencionada, ¿cómo se produce el color amarillo a partir de los estímulos de los conos en la retina?**

   El color amarillo se produce cuando el rojo y el verde se superponen de forma aditiva con la misma proporción de intensidades. Los conos especializados para percibir longitudes de onda del color rojo y verde se estimulan simultáneamente, creando la percepción del amarillo.

3. **¿Cuál es la diferencia principal entre el observador estándar de 1931 y el de 1964 en el modelo de color CIE XYZ?**

   La diferencia principal radica en el campo de visión: el observador estándar de 1931 tenía un campo de visión de 2 grados, mientras que la especificación de 1964 amplió el campo de visión a 10 grados. Esta ampliación permitió obtener valores tristimulus que reflejaran una mayor sensibilidad de la retina y tuvieran en cuenta la visión periférica del observador, lo cual se consideró más adecuado en muchos casos.

4. **Según el texto, ¿por qué se necesitó implementar el modelo teórico CIE XYZ a pesar de la existencia del modelo CIE RGB?**

   Fue necesario implementar el modelo CIE XYZ porque el modelo CIE RGB tenía la limitación de que no todos los colores podían ser representados adecuadamente usando las tres fuentes de luz aditivas (rojo, verde y azul). Específicamente, para las longitudes de onda inferiores a 550 nm, se requerían valores "negativos" que eran físicamente imposibles de generar. El modelo CIE XYZ removió esta limitación de los valores negativos, proporcionando una descripción teórica más completa.

5. **¿Qué ventajas hay entre el modelo HSV y el RGB?**

   El modelo HSV es más intuitivo que RGB para la selección de colores porque:
   - Separa el color (Hue) de su intensidad (Value) y pureza (Saturation)
   - El componente Hue representa directamente el color, facilitando la selección del tono deseado
   - La Saturación controla la cantidad o pureza del color
   - El Value (brillo) controla la luminosidad
   - Es más cercano a cómo los humanos perciben y describen los colores (por ejemplo, "rojo brillante" o "azul pálido")
   - En RGB, cambiar un color requiere ajustar tres componentes simultáneamente, mientras que en HSV se pueden modificar independientemente el tono, la saturación y el brillo

### 5.5.2. Preguntas de análisis y comprensión con base en el texto

1. **¿Por qué el modelo HSV es más intuitivo que el modelo RGB para la selección de colores en dispositivos como las lámparas RGB?**

   El modelo HSV es más intuitivo porque separa conceptos que son naturales para la percepción humana: el tipo de color (Hue), qué tan "puro" o intenso es ese color (Saturation), y qué tan brillante u oscuro es (Value). En una lámpara RGB, si se quiere cambiar de rojo a azul manteniendo el mismo brillo, en RGB se deben ajustar los tres canales simultáneamente, mientras que en HSV solo se modifica el Hue. Esto hace que HSV sea más práctico para interfaces de usuario donde se seleccionan colores mediante controles deslizantes o ruedas de color.

2. **Dado que cada capa de color (rojo, verde y azul) en una imagen se visualiza como una sola tonalidad, ¿cómo se representa el color amarillo en términos de la saturación y el brillo, y cuál es la contribución de cada capa de la imagen (rojo, verde y azul) para producir ese color?**

   El color amarillo puro se representa en RGB con valores máximos de rojo (R=255) y verde (G=255), y mínimo de azul (B=0). 
   
   En términos de HSV:
   - **Hue (H)**: 60° o 1/6 normalizado (posición entre rojo y verde en la rueda de color)
   - **Saturation (S)**: 1 (máxima saturación, color puro sin mezcla con blanco)
   - **Value (V)**: 1 (máximo brillo)

   La contribución de cada capa es:
   - Capa Roja: Contribuye 100% (255)
   - Capa Verde: Contribuye 100% (255)
   - Capa Azul: No contribuye (0)

   Para amarillos menos saturados o menos brillantes, se ajustarían proporcionalmente los valores RGB manteniendo la relación R≈G>>B.

3. **En el ejemplo de segmentación de color amarillo de la bandera colombiana, ¿qué modelo de color (RGB, HSV o CIELAB) parece ser el más intuitivo y fácil de usar para esta tarea? Explique su respuesta.**

   El modelo **CIELAB** parece ser el más intuitivo y fácil para esta tarea por las siguientes razones:

   - **Percepción uniforme**: CIELAB está diseñado para que las distancias en el espacio de color correspondan a diferencias perceptibles por el ojo humano
   - **Separación natural**: En CIELAB, el amarillo se caracteriza por valores altos en la componente a* (positiva hacia amarillo-rojo) y b* (positiva hacia amarillo-azul), mientras que L* controla el brillo
   - **Robustez a la iluminación**: CIELAB es menos sensible a variaciones de iluminación que RGB
   - **Precisión en la segmentación**: Como se menciona en el texto, CIELAB destaca por su precisión y correspondencia con la percepción humana del color

   HSV es mejor que RGB para esta tarea porque permite seleccionar un rango de Hue cercano al amarillo, pero CIELAB ofrece mayor precisión y corresponde mejor a cómo percibimos las diferencias de color.

4. **¿Qué utilidad tiene el modelo CMYK? Dé ejemplo de uso.**

   El modelo CMYK (Cian, Magenta, Amarillo, Negro) es un modelo sustractivo utilizado principalmente en **impresión**. Su utilidad radica en:

   - **Impresión offset y digital**: Representa cómo se combinan tintas o pigmentos sobre papel
   - **Modelo sustractivo**: Los pigmentos absorben (sustraen) longitudes de onda de luz, a diferencia del modelo aditivo RGB usado en pantallas
   - **Optimización de costos**: El negro (K) se añade porque es más económico que mezclar C+M+Y para obtener negro, y además produce mejores tonos oscuros
   - **Control de consumo de tinta**: Permite gestionar la cantidad de tinta depositada en el papel

   **Ejemplos de uso**:
   - Impresión de revistas, libros, carteles y material publicitario
   - Diseño gráfico profesional destinado a impresión
   - Periódicos y documentos impresos en impresoras láser o de inyección de tinta
   - Preparación de archivos PDF para envío a imprenta

### 5.5.3. Preguntas de cálculos

1. **Dada la matriz de colores RGB a HSV, presente los resultados en una tabla con las columnas: Pixel, Cmax, Cmin, δ, H, S y V. Realice todos los cálculos a mano, mostrando cada paso del proceso.**

   **Matrices de entrada:**
   
   $$
   R = \begin{bmatrix} 0 & 255 \\ 255 & 0 \end{bmatrix} \quad
   G = \begin{bmatrix} 255 & 0 \\ 255 & 255 \end{bmatrix} \quad
   B = \begin{bmatrix} 255 & 255 \\ 0 & 0 \end{bmatrix}
   $$

   **Normalización (dividiendo por 255):**
   
   $$
   r = \begin{bmatrix} 0.000 & 1.000 \\ 1.000 & 0.000 \end{bmatrix} \quad
   g = \begin{bmatrix} 1.000 & 0.000 \\ 1.000 & 1.000 \end{bmatrix} \quad
   b = \begin{bmatrix} 1.000 & 1.000 \\ 0.000 & 0.000 \end{bmatrix}
   $$

   **Cálculos paso a paso:**

**Pixel (1,1): r=0, g=1, b=1**
    - $C_{max} = \max(0, 1, 1) = 1$
    - $C_{min} = \min(0, 1, 1) = 0$
    - $\delta = C_{max} - C_{min} = 1 - 0 = 1$
    - Como $C_{max} = g = 1$: $H = 60° \times \left[\frac{b-r}{\delta} + 2\right] = 60° \times \left[\frac{1-0}{1} + 2\right] = 60° \times 3 = 180°$
    - $S = \frac{\delta}{C_{max}} = \frac{1}{1} = 1$
    - $V = C_{max} = 1$

**Pixel (1,2): r=1, g=0, b=1**
    - $C_{max} = \max(1, 0, 1) = 1$
    - $C_{min} = \min(1, 0, 1) = 0$
    - $\delta = C_{max} - C_{min} = 1 - 0 = 1$
    - Como $C_{max} = b = 1$: $H = 60° \times \left[\frac{r-g}{\delta} + 4\right] = 60° \times \left[\frac{1-0}{1} + 4\right] = 60° \times 5 = 300°$
    - $S = \frac{\delta}{C_{max}} = \frac{1}{1} = 1$
    - $V = C_{max} = 1$

**Pixel (2,1): r=1, g=1, b=0**
    - $C_{max} = \max(1, 1, 0) = 1$
    - $C_{min} = \min(1, 1, 0) = 0$
    - $\delta = C_{max} - C_{min} = 1 - 0 = 1$
    - Como $C_{max} = r = 1$: $H = 60° \times \left(\frac{g-b}{\delta} \bmod 6\right) = 60° \times \left(\frac{1-0}{1} \bmod 6\right) = 60° \times 1 = 60°$
    - $S = \frac{\delta}{C_{max}} = \frac{1}{1} = 1$
    - $V = C_{max} = 1$

**Pixel (2,2): r=0, g=1, b=0**
    - $C_{max} = \max(0, 1, 0) = 1$
    - $C_{min} = \min(0, 1, 0) = 0$
    - $\delta = C_{max} - C_{min} = 1 - 0 = 1$
    - Como $C_{max} = g = 1$: $H = 60° \times \left[\frac{b-r}{\delta} + 2\right] = 60° \times \left[\frac{0-0}{1} + 2\right] = 60° \times 2 = 120°$
    - $S = \frac{\delta}{C_{max}} = \frac{1}{1} = 1$
    - $V = C_{max} = 1$

   **Tabla de resultados:**

   $$
   \begin{array}{|c|c|c|c|c|c|c|c|c|c|c|}
   \hline
   \text{Pixel} & r & g & b & C_{max} & C_{min} & \delta & H & H_{norm} & S & V \\
   \hline
   (1,1) & 0.000 & 1.000 & 1.000 & 1 & 0 & 1 & 180° & 0.500 & 1 & 1 \\
   (1,2) & 1.000 & 0.000 & 1.000 & 1 & 0 & 1 & 300° & 0.833 & 1 & 1 \\
   (2,1) & 1.000 & 1.000 & 0.000 & 1 & 0 & 1 & 60° & 0.167 & 1 & 1 \\
   (2,2) & 0.000 & 1.000 & 0.000 & 1 & 0 & 1 & 120° & 0.333 & 1 & 1 \\
   \hline
   \end{array}
   $$

   **Resultado en formato HSV:**
   
   $$
   H = \begin{bmatrix} 180° & 300° \\ 60° & 120° \end{bmatrix} \quad
   S = \begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix} \quad
   V = \begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix}
   $$

2. **El resultado de la conversión obtenido en HSV del punto 1, llevarlo nuevamente a RGB mostrado en una tabla las columnas Pixel, C, X, m, r', g', b'. Realice todos los cálculos a mano, mostrando cada paso del proceso.**

   **Matrices HSV de entrada (del ejercicio anterior):**
   
   $$
   H = \begin{bmatrix} 180° & 300° \\ 60° & 120° \end{bmatrix} \quad
   S = \begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix} \quad
   V = \begin{bmatrix} 1 & 1 \\ 1 & 1 \end{bmatrix}
   $$

   **Cálculos paso a paso:**

   **Pixel (1,1): H=180°, S=1, V=1** (Cian)
   - $C = S \times V = 1 \times 1 = 1$
   - $\frac{H}{60°} = \frac{180}{60} = 3$
   - $3 \bmod 2 = 1$
   - $X = C \times (1 - |3 \bmod 2 - 1|) = 1 \times (1 - |1 - 1|) = 1 \times (1 - 0) = 1$
   - $m = V - C = 1 - 1 = 0$
   - Como $120° \leq 180° < 180°$: $(r', g', b') = (0, C, X) = (0, 1, 1)$
   - $R = 255 \times (0 + 0) = 0$
   - $G = 255 \times (1 + 0) = 255$
   - $B = 255 \times (1 + 0) = 255$

   **Pixel (1,2): H=300°, S=1, V=1** (Magenta)
   - $C = S \times V = 1 \times 1 = 1$
   - $\frac{H}{60°} = \frac{300}{60} = 5$
   - $5 \bmod 2 = 1$
   - $X = C \times (1 - |5 \bmod 2 - 1|) = 1 \times (1 - |1 - 1|) = 1 \times (1 - 0) = 1$
   - $m = V - C = 1 - 1 = 0$
   - Como $300° \leq 300° < 360°$: $(r', g', b') = (C, 0, X) = (1, 0, 1)$
   - $R = 255 \times (1 + 0) = 255$
   - $G = 255 \times (0 + 0) = 0$
   - $B = 255 \times (1 + 0) = 255$

   **Pixel (2,1): H=60°, S=1, V=1** (Amarillo)
   - $C = S \times V = 1 \times 1 = 1$
   - $\frac{H}{60°} = \frac{60}{60} = 1$
   - $1 \bmod 2 = 1$
   - $X = C \times (1 - |1 \bmod 2 - 1|) = 1 \times (1 - |1 - 1|) = 1 \times (1 - 0) = 1$
   - $m = V - C = 1 - 1 = 0$
   - Como $60° \leq 60° < 120°$: $(r', g', b') = (X, C, 0) = (1, 1, 0)$
   - $R = 255 \times (1 + 0) = 255$
   - $G = 255 \times (1 + 0) = 255$
   - $B = 255 \times (0 + 0) = 0$

   **Pixel (2,2): H=120°, S=1, V=1** (Verde puro)
   - $C = S \times V = 1 \times 1 = 1$
   - $\frac{H}{60°} = \frac{120}{60} = 2$
   - $2 \bmod 2 = 0$
   - $X = C \times (1 - |2 \bmod 2 - 1|) = 1 \times (1 - |0 - 1|) = 1 \times (1 - 1) = 0$
   - $m = V - C = 1 - 1 = 0$
   - Como $120° \leq 120° < 180°$: $(r', g', b') = (0, C, X) = (0, 1, 0)$
   - $R = 255 \times (0 + 0) = 0$
   - $G = 255 \times (1 + 0) = 255$
   - $B = 255 \times (0 + 0) = 0$

   **Tabla de resultados:**

   $$
   \begin{array}{|c|c|c|c|c|c|c|c|c|c|c|c|c|}
   \hline
   \text{Pixel} & H & S & V & C & X & m & r' & g' & b' & R & G & B \\
   \hline
   (1,1) & 180° & 1 & 1 & 1 & 1 & 0 & 0.000 & 1.000 & 1.000 & 0 & 255 & 255 \\
   (1,2) & 300° & 1 & 1 & 1 & 1 & 0 & 1.000 & 0.000 & 1.000 & 255 & 0 & 255 \\
   (2,1) & 60° & 1 & 1 & 1 & 1 & 0 & 1.000 & 1.000 & 0.000 & 255 & 255 & 0 \\
   (2,2) & 120° & 1 & 1 & 1 & 0 & 0 & 0.000 & 1.000 & 0.000 & 0 & 255 & 0 \\
   \hline
   \end{array}
   $$

   **Resultado en formato RGB:**
   
   $$
   R = \begin{bmatrix} 0 & 255 \\ 255 & 0 \end{bmatrix} \quad
   G = \begin{bmatrix} 255 & 0 \\ 255 & 255 \end{bmatrix} \quad
   B = \begin{bmatrix} 255 & 255 \\ 0 & 0 \end{bmatrix}
   $$

   **Verificación:** Las matrices RGB recuperadas coinciden exactamente con las matrices de entrada originales, confirmando que la conversión HSV → RGB es correcta y que el proceso es reversible.

3. **Dada la matriz de colores RGB a XYZ, presente los resultados en una tabla con las columnas Pixel r, g, b, rl, gl, bl, X, Y, Z, Xn, Yn, Zn. Realice todos los cálculos a mano, mostrando cada paso del proceso.**

   **Matrices de entrada:**
   
   $$
   R = \begin{bmatrix} 0 & 255 \\ 255 & 0 \end{bmatrix} \quad
   G = \begin{bmatrix} 255 & 0 \\ 255 & 255 \end{bmatrix} \quad
   B = \begin{bmatrix} 255 & 255 \\ 0 & 0 \end{bmatrix}
   $$

   **Paso 1: Normalizar valores RGB (0-1):**
   
   $$
   r = \begin{bmatrix} 0.000 & 1.000 \\ 1.000 & 0.000 \end{bmatrix} \quad
   g = \begin{bmatrix} 1.000 & 0.000 \\ 1.000 & 1.000 \end{bmatrix} \quad
   b = \begin{bmatrix} 1.000 & 1.000 \\ 0.000 & 0.000 \end{bmatrix}
   $$

   **Paso 2: Aplicar corrección gamma (conversión a lineal):**
   
   Para cada componente:
   $$
   v_l = \begin{cases}
   \frac{v}{12.92} & \text{si } v \leq 0.04045 \\
   \left(\frac{v + 0.055}{1.055}\right)^{2.4} & \text{si } v > 0.04045
   \end{cases}
   $$

   **Pixel (1,1): r=0, g=1, b=1**
   - $r_l = \frac{0}{12.92} = 0.000$
   - $g_l = \left(\frac{1+0.055}{1.055}\right)^{2.4} = \left(\frac{1.055}{1.055}\right)^{2.4} = 1^{2.4} = 1.000$
   - $b_l = \left(\frac{1+0.055}{1.055}\right)^{2.4} = 1.000$

   **Pixel (1,2): r=1, g=0, b=1**
   - $r_l = \left(\frac{1+0.055}{1.055}\right)^{2.4} = 1.000$
   - $g_l = \frac{0}{12.92} = 0.000$
   - $b_l = \left(\frac{1+0.055}{1.055}\right)^{2.4} = 1.000$

   **Pixel (2,1): r=1, g=1, b=0**
   - $r_l = 1.000$
   - $g_l = 1.000$
   - $b_l = \frac{0}{12.92} = 0.000$

   **Pixel (2,2): r=0, g=1, b=0**
   - $r_l = 0.000$
   - $g_l = 1.000$
   - $b_l = 0.000$

   **Paso 3: Multiplicar por la matriz de conversión RGB a XYZ (usando D65):**
   
   $$
   \begin{bmatrix} X \\ Y \\ Z \end{bmatrix} = 
   \begin{bmatrix}
   0.4124 & 0.3576 & 0.1805 \\
   0.2126 & 0.7152 & 0.0722 \\
   0.0193 & 0.1192 & 0.9505
   \end{bmatrix}
   \begin{bmatrix} r_l \\ g_l \\ b_l \end{bmatrix}
   $$

   **Pixel (1,1): $r_l=0$, $g_l=1$, $b_l=1$**
   - $X = 0.4124 \times 0 + 0.3576 \times 1 + 0.1805 \times 1 = 0.5381$
   - $Y = 0.2126 \times 0 + 0.7152 \times 1 + 0.0722 \times 1 = 0.7874$
   - $Z = 0.0193 \times 0 + 0.1192 \times 1 + 0.9505 \times 1 = 1.0697$

   **Pixel (1,2): $r_l=1$, $g_l=0$, $b_l=1$**
   - $X = 0.4124 \times 1 + 0.3576 \times 0 + 0.1805 \times 1 = 0.5929$
   - $Y = 0.2126 \times 1 + 0.7152 \times 0 + 0.0722 \times 1 = 0.2848$
   - $Z = 0.0193 \times 1 + 0.1192 \times 0 + 0.9505 \times 1 = 0.9698$

   **Pixel (2,1): $r_l=1$, $g_l=1$, $b_l=0$**
   - $X = 0.4124 \times 1 + 0.3576 \times 1 + 0.1805 \times 0 = 0.7700$
   - $Y = 0.2126 \times 1 + 0.7152 \times 1 + 0.0722 \times 0 = 0.9278$
   - $Z = 0.0193 \times 1 + 0.1192 \times 1 + 0.9505 \times 0 = 0.1385$

   **Pixel (2,2): $r_l=0$, $g_l=1$, $b_l=0$**
   - $X = 0.4124 \times 0 + 0.3576 \times 1 + 0.1805 \times 0 = 0.3576$
   - $Y = 0.2126 \times 0 + 0.7152 \times 1 + 0.0722 \times 0 = 0.7152$
   - $Z = 0.0193 \times 0 + 0.1192 \times 1 + 0.9505 \times 0 = 0.1192$

   **Paso 4: Normalizar con punto blanco D65 ($X_n = X/0.95047$, $Y_n = Y/1.00000$, $Z_n = Z/1.08883$):**

   **Pixel (1,1):**
   - $X_n = 0.5381/0.95047 = 0.5661$
   - $Y_n = 0.7874/1.00000 = 0.7874$
   - $Z_n = 1.0697/1.08883 = 0.9824$

   **Pixel (1,2):**
   - $X_n = 0.5929/0.95047 = 0.6239$
   - $Y_n = 0.2848/1.00000 = 0.2848$
   - $Z_n = 0.9698/1.08883 = 0.8907$

   **Pixel (2,1):**
   - $X_n = 0.7700/0.95047 = 0.8102$
   - $Y_n = 0.9278/1.00000 = 0.9278$
   - $Z_n = 0.1385/1.08883 = 0.1272$

   **Pixel (2,2):**
   - $X_n = 0.3576/0.95047 = 0.3763$
   - $Y_n = 0.7152/1.00000 = 0.7152$
   - $Z_n = 0.1192/1.08883 = 0.1095$

   **Tabla de resultados:**

   $$
   \begin{array}{|c|c|c|c|c|c|c|c|c|c|c|c|c|}
   \hline
   \text{Pixel} & r & g & b & r_l & g_l & b_l & X & Y & Z & X_n & Y_n & Z_n \\
   \hline
   (1,1) & 0.000 & 1.000 & 1.000 & 0.000 & 1.000 & 1.000 & 0.5381 & 0.7874 & 1.0697 & 0.5661 & 0.7874 & 0.9824 \\
   (1,2) & 1.000 & 0.000 & 1.000 & 1.000 & 0.000 & 1.000 & 0.5929 & 0.2848 & 0.9698 & 0.6239 & 0.2848 & 0.8907 \\
   (2,1) & 1.000 & 1.000 & 0.000 & 1.000 & 1.000 & 0.000 & 0.7700 & 0.9278 & 0.1385 & 0.8102 & 0.9278 & 0.1272 \\
   (2,2) & 0.000 & 1.000 & 0.000 & 0.000 & 1.000 & 0.000 & 0.3576 & 0.7152 & 0.1192 & 0.3763 & 0.7152 & 0.1095 \\
   \hline
   \end{array}
   $$

4. **El resultado de la conversión obtenido en XYZ del punto anterior, llevarlo nuevamente a RGB mostrado en una tabla las columnas Pixel, X, Y, Z, r, g, b, rl, gl, bl, R, G, B. Realice todos los cálculos a mano, mostrando cada paso del proceso.**

   **Matrices XYZ de entrada (del ejercicio anterior):**
   
   $$
   X = \begin{bmatrix} 0.5381 & 0.5929 \\ 0.7700 & 0.3576 \end{bmatrix} \quad
   Y = \begin{bmatrix} 0.7874 & 0.2848 \\ 0.9278 & 0.7152 \end{bmatrix} \quad
   Z = \begin{bmatrix} 1.0697 & 0.9698 \\ 0.1385 & 0.1192 \end{bmatrix}
   $$

   **Paso 1: Multiplicar por la matriz inversa de conversión XYZ a RGB lineal:**
   
   $$
   \begin{bmatrix} r_l \\ g_l \\ b_l \end{bmatrix} = 
   \begin{bmatrix}
   3.2406 & -1.5372 & -0.4986 \\
   -0.9689 & 1.8758 & 0.0415 \\
   0.0557 & -0.2040 & 1.0570
   \end{bmatrix}
   \begin{bmatrix} X \\ Y \\ Z \end{bmatrix}
   $$

   **Pixel (1,1): X=0.5381, Y=0.7874, Z=1.0697**
   - $r_l = 3.2406 \times 0.5381 + (-1.5372) \times 0.7874 + (-0.4986) \times 1.0697 = 1.744 - 1.210 - 0.533 = 0.001$
   - $g_l = -0.9689 \times 0.5381 + 1.8758 \times 0.7874 + 0.0415 \times 1.0697 = -0.521 + 1.477 + 0.044 = 1.000$
   - $b_l = 0.0557 \times 0.5381 + (-0.2040) \times 0.7874 + 1.0570 \times 1.0697 = 0.030 - 0.161 + 1.131 = 1.000$

   **Pixel (1,2): X=0.5929, Y=0.2848, Z=0.9698**
   - $r_l = 3.2406 \times 0.5929 + (-1.5372) \times 0.2848 + (-0.4986) \times 0.9698 = 1.921 - 0.438 - 0.483 = 1.000$
   - $g_l = -0.9689 \times 0.5929 + 1.8758 \times 0.2848 + 0.0415 \times 0.9698 = -0.574 + 0.534 + 0.040 = 0.000$
   - $b_l = 0.0557 \times 0.5929 + (-0.2040) \times 0.2848 + 1.0570 \times 0.9698 = 0.033 - 0.058 + 1.025 = 1.000$

   **Pixel (2,1): X=0.7700, Y=0.9278, Z=0.1385**
   - $r_l = 3.2406 \times 0.7700 + (-1.5372) \times 0.9278 + (-0.4986) \times 0.1385 = 2.495 - 1.426 - 0.069 = 1.000$
   - $g_l = -0.9689 \times 0.7700 + 1.8758 \times 0.9278 + 0.0415 \times 0.1385 = -0.746 + 1.740 + 0.006 = 1.000$
   - $b_l = 0.0557 \times 0.7700 + (-0.2040) \times 0.9278 + 1.0570 \times 0.1385 = 0.043 - 0.189 + 0.146 = 0.000$

   **Pixel (2,2): X=0.3576, Y=0.7152, Z=0.1192**
   - $r_l = 3.2406 \times 0.3576 + (-1.5372) \times 0.7152 + (-0.4986) \times 0.1192 = 1.159 - 1.099 - 0.059 = 0.001$
   - $g_l = -0.9689 \times 0.3576 + 1.8758 \times 0.7152 + 0.0415 \times 0.1192 = -0.346 + 1.341 + 0.005 = 1.000$
   - $b_l = 0.0557 \times 0.3576 + (-0.2040) \times 0.7152 + 1.0570 \times 0.1192 = 0.020 - 0.146 + 0.126 = 0.000$

   **Paso 2: Aplicar corrección gamma inversa (lineal a sRGB):**
   
   Para cada componente:
   $$
   v = \begin{cases}
   12.92 \times v_l & \text{si } v_l \leq 0.0031308 \\
   1.055 \times v_l^{1/2.4} - 0.055 & \text{si } v_l > 0.0031308
   \end{cases}
   $$

   Todos los valores son $> 0.0031308$ o $= 0$, entonces aplicamos la fórmula correspondiente:

   **Pixel (1,1): $r_l \approx 0$, $g_l=1$, $b_l=1$**
   - $r = 12.92 \times 0.001 \approx 0.013 \approx 0$
   - $g = 1.055 \times 1^{1/2.4} - 0.055 = 1.055 - 0.055 = 1.000$
   - $b = 1.000$

   **Pixel (1,2): $r_l=1$, $g_l=0$, $b_l=1$**
   - $r = 1.000$
   - $g = 0.000$
   - $b = 1.000$

   **Pixel (2,1): $r_l=1$, $g_l=1$, $b_l=0$**
   - $r = 1.000$
   - $g = 1.000$
   - $b = 0.000$

   **Pixel (2,2): $r_l \approx 0$, $g_l=1$, $b_l=0$**
   - $r = 0.000$
   - $g = 1.000$
   - $b = 0.000$

   **Paso 3: Desnormalizar a rango 0-255:**
   
   **Tabla de resultados:**

   $$
   \begin{array}{|c|c|c|c|c|c|c|c|c|c|c|c|c|}
   \hline
   \text{Pixel} & X & Y & Z & r_l & g_l & b_l & r & g & b & R & G & B \\
   \hline
   (1,1) & 0.5381 & 0.7874 & 1.0697 & 0.001 & 1.000 & 1.000 & 0.000 & 1.000 & 1.000 & 0 & 255 & 255 \\
   (1,2) & 0.5929 & 0.2848 & 0.9698 & 1.000 & 0.000 & 1.000 & 1.000 & 0.000 & 1.000 & 255 & 0 & 255 \\
   (2,1) & 0.7700 & 0.9278 & 0.1385 & 1.000 & 1.000 & 0.000 & 1.000 & 1.000 & 0.000 & 255 & 255 & 0 \\
   (2,2) & 0.3576 & 0.7152 & 0.1192 & 0.001 & 1.000 & 0.000 & 0.000 & 1.000 & 0.000 & 0 & 255 & 0 \\
   \hline
   \end{array}
   $$

   **Resultado en formato RGB:**
   
   $$
   R = \begin{bmatrix} 0 & 255 \\ 255 & 0 \end{bmatrix} \quad
   G = \begin{bmatrix} 255 & 0 \\ 255 & 255 \end{bmatrix} \quad
   B = \begin{bmatrix} 255 & 255 \\ 0 & 0 \end{bmatrix}
   $$

5. **El resultado de la conversión obtenida en XYZ del punto 3, llevarlo a CieLab. Los cálculos numéricos de X, Y, Z, xr, yr, zr, fx, fy, fz, L, a, b de cada pixel (i, j). Realice todos los cálculos a mano, mostrando cada paso del proceso.**

   **Matrices XYZ de entrada (del ejercicio 3):**
   
   $$
   X = \begin{bmatrix} 0.5381 & 0.5929 \\ 0.7700 & 0.3576 \end{bmatrix} \quad
   Y = \begin{bmatrix} 0.7874 & 0.2848 \\ 0.9278 & 0.7152 \end{bmatrix} \quad
   Z = \begin{bmatrix} 1.0697 & 0.9698 \\ 0.1385 & 0.1192 \end{bmatrix}
   $$

   **Constantes (usando D65):**
   - Punto blanco: $X_r = 0.95047$, $Y_r = 1.00000$, $Z_r = 1.08883$
   - $\varepsilon = 0.008856$, $\kappa = 903.3$

   **Función auxiliar:**
   $$
   f_v(v_r) = \begin{cases}
   v_r^{1/3} & \text{si } v_r > \varepsilon \\
   \frac{\kappa \times v_r + 16}{116} & \text{si } v_r \leq \varepsilon
   \end{cases}
   $$

   **Cálculos paso a paso:**

   **Pixel (1,1): X=0.5381, Y=0.7874, Z=1.0697**
   - $x_r = 0.5381/0.95047 = 0.5661$
   - $y_r = 0.7874/1.00000 = 0.7874$
   - $z_r = 1.0697/1.08883 = 0.9824$
   
   Todos $> \varepsilon$, entonces:
   - $f_x = 0.5661^{1/3} = 0.8272$
   - $f_y = 0.7874^{1/3} = 0.9236$
   - $f_z = 0.9824^{1/3} = 0.9941$
   
   Componentes Lab:
   - $L = 116 \times 0.9236 - 16 = 107.14 - 16 = 91.14$
   - $a = 500 \times (0.8272 - 0.9236) = 500 \times (-0.0964) = -48.20$
   - $b = 200 \times (0.9236 - 0.9941) = 200 \times (-0.0705) = -14.10$

   **Pixel (1,2): X=0.5929, Y=0.2848, Z=0.9698**
   - $x_r = 0.5929/0.95047 = 0.6239$
   - $y_r = 0.2848/1.00000 = 0.2848$
   - $z_r = 0.9698/1.08883 = 0.8907$
   
   Todos $> \varepsilon$, entonces:
   - $f_x = 0.6239^{1/3} = 0.8543$
   - $f_y = 0.2848^{1/3} = 0.6580$
   - $f_z = 0.8907^{1/3} = 0.9620$
   
   Componentes Lab:
   - $L = 116 \times 0.6580 - 16 = 76.33 - 16 = 60.33$
   - $a = 500 \times (0.8543 - 0.6580) = 500 \times 0.1963 = 98.15$
   - $b = 200 \times (0.6580 - 0.9620) = 200 \times (-0.3040) = -60.80$

   **Pixel (2,1): X=0.7700, Y=0.9278, Z=0.1385**
   - $x_r = 0.7700/0.95047 = 0.8102$
   - $y_r = 0.9278/1.00000 = 0.9278$
   - $z_r = 0.1385/1.08883 = 0.1272$
   
   Todos $> \varepsilon$, entonces:
   - $f_x = 0.8102^{1/3} = 0.9316$
   - $f_y = 0.9278^{1/3} = 0.9755$
   - $f_z = 0.1272^{1/3} = 0.5031$
   
   Componentes Lab:
   - $L = 116 \times 0.9755 - 16 = 113.16 - 16 = 97.16$
   - $a = 500 \times (0.9316 - 0.9755) = 500 \times (-0.0439) = -21.95$
   - $b = 200 \times (0.9755 - 0.5031) = 200 \times 0.4724 = 94.48$

   **Pixel (2,2): X=0.3576, Y=0.7152, Z=0.1192**
   - $x_r = 0.3576/0.95047 = 0.3763$
   - $y_r = 0.7152/1.00000 = 0.7152$
   - $z_r = 0.1192/1.08883 = 0.1095$
   
   Todos $> \varepsilon$, entonces:
   - $f_x = 0.3763^{1/3} = 0.7222$
   - $f_y = 0.7152^{1/3} = 0.8941$
   - $f_z = 0.1095^{1/3} = 0.4783$
   
   Componentes Lab:
   - $L = 116 \times 0.8941 - 16 = 103.72 - 16 = 87.72$
   - $a = 500 \times (0.7222 - 0.8941) = 500 \times (-0.1719) = -85.95$
   - $b = 200 \times (0.8941 - 0.4783) = 200 \times 0.4158 = 83.16$

   **Tabla de resultados:**

   $$
   \begin{array}{|c|c|c|c|c|c|c|c|c|c|c|c|c|}
   \hline
   \text{Pixel} & X & Y & Z & x_r & y_r & z_r & f_x & f_y & f_z & L & a & b \\
   \hline
   (1,1) & 0.5381 & 0.7874 & 1.0697 & 0.5661 & 0.7874 & 0.9824 & 0.8272 & 0.9236 & 0.9941 & 91.14 & -48.20 & -14.10 \\
   (1,2) & 0.5929 & 0.2848 & 0.9698 & 0.6239 & 0.2848 & 0.8907 & 0.8543 & 0.6580 & 0.9620 & 60.33 & 98.15 & -60.80 \\
   (2,1) & 0.7700 & 0.9278 & 0.1385 & 0.8102 & 0.9278 & 0.1272 & 0.9316 & 0.9755 & 0.5031 & 97.16 & -21.95 & 94.48 \\
   (2,2) & 0.3576 & 0.7152 & 0.1192 & 0.3763 & 0.7152 & 0.1095 & 0.7222 & 0.8941 & 0.4783 & 87.72 & -85.95 & 83.16 \\
   \hline
   \end{array}
   $$

   **Resultado en formato Lab:**
   
   $$
   L = \begin{bmatrix} 91.14 & 60.33 \\ 97.16 & 87.72 \end{bmatrix} \quad
   a = \begin{bmatrix} -48.20 & 98.15 \\ -21.95 & -85.95 \end{bmatrix} \quad
   b = \begin{bmatrix} -14.10 & -60.80 \\ 94.48 & 83.16 \end{bmatrix}
   $$
