# Capítulo 8: El Ruido - Respuestas

## 8.5. Preguntas

---

## 8.5.1. Preguntas sobre hechos del texto

### 1. ¿Cuáles son las tres métricas referenciales destacadas para evaluar la efectividad del filtrado de imágenes mencionadas en el documento?

Las tres métricas referenciales destacadas son:

1. **Error Cuadrático Medio (MSE - Mean Squared Error)**: Mide la diferencia promedio al cuadrado entre los valores de los píxeles de la imagen original y la imagen filtrada.

2. **Relación Señal-Ruido de Pico (PSNR - Peak Signal-to-Noise Ratio)**: Métrica que evalúa la calidad de una imagen filtrada en comparación con la imagen original, expresando esta comparación en términos logarítmicos y en decibeles (dB).

3. **Índice de Similitud Estructural (SSIM - Structural Similarity Index)**: Métrica utilizada para evaluar la similitud entre dos imágenes, enfocándose en tres aspectos clave: luminancia, contraste y estructura.

Estas se consideran **métricas de referencia completa (Full-Reference, FR)**, ya que requieren de la imagen original de referencia sin ruido nativo $I_r$ y la imagen con ruido $I_n$ agregado en algún proceso.

---

### 2. ¿En qué unidades se mide el Error Cuadrático Medio (MSE)?

El Error Cuadrático Medio (MSE) se mide en **unidades de intensidad al cuadrado**.

Por ejemplo, si los valores de los píxeles se encuentran en un rango de 0 a 255, como es común en imágenes de 8 bits, el MSE estará en las **unidades de intensidad al cuadrado** (es decir, sin unidad específica, pero corresponde a la magnitud al cuadrado del rango de píxeles).

**Ejemplo**: Si tenemos un MSE = 3.25, esto significa que el promedio de las diferencias al cuadrado entre los píxeles es 3.25 unidades cuadradas de intensidad.

---

### 3. ¿Qué tres aspectos clave evalúa el Índice de Similitud Estructural (SSIM)?

El SSIM evalúa tres aspectos clave que reflejan la percepción visual humana:

1. **Luminancia $l(r,n)$**: Mide la intensidad de la luz y el brillo global de las imágenes. Compara los valores medios de intensidad entre ambas imágenes.

   $$l(r, n) = \frac{2\mu_r\mu_n + C_1}{\mu_r^2 + \mu_n^2 + C_1}$$

2. **Contraste $c(r,n)$**: Evalúa la variación y diferencia de intensidades entre píxeles. Compara las desviaciones estándar de ambas imágenes.

   $$c(r, n) = \frac{2\sigma_r\sigma_n + C_2}{\sigma_r^2 + \sigma_n^2 + C_2}$$

3. **Estructura $s(r,n)$**: Mide la correlación entre las dos imágenes utilizando la covarianza, analizando los patrones, texturas y relaciones espaciales entre píxeles.

   $$s(r, n) = \frac{\sigma_{rn} + C_3}{\sigma_r\sigma_n + C_3}$$

El SSIM combina estos tres componentes mediante:

$$\text{SSIM}(r, n) = [l(r, n)]^\alpha \cdot [c(r, n)]^\beta \cdot [s(r, n)]^\gamma$$

Donde comúnmente se utiliza $\alpha = \beta = \gamma = 1$, otorgando la misma importancia a todas las variables.

---

### 4. ¿Cuál es el rango de valores que puede tomar el SSIM y qué indican estos valores?

**Rango de valores del SSIM:**

El SSIM proporciona un valor que está **entre -1 y 1**.

**Interpretación de los valores:**

- **SSIM = 1**: Indica una **similitud estructural perfecta** entre las dos imágenes. Las imágenes son idénticas en términos de luminancia, contraste y estructura.

- **SSIM = -1**: Representa una **inversión completa**, aunque esta situación es rara en imágenes naturales.

- **Rango práctico [0, 1]**: En la práctica, los valores del SSIM se sitúan generalmente entre 0 y 1:
  - **SSIM ≈ 0**: Indica la **ausencia de correlación** entre las imágenes (muy diferentes)
  - **SSIM cercano a 1**: Indica **alta similitud** estructural entre las imágenes

**Interpretación intuitiva:** Los valores del SSIM más cercanos a 1 indican una alta similitud, lo cual es fácil de interpretar y comparar. Por ejemplo, un SSIM de 0.98 indica que las imágenes son muy similares estructuralmente.

---

### 5. ¿Cuál es el valor típico de PSNR considerado aceptable para imágenes de 8 bits?

El valor típico de PSNR considerado **aceptable** para imágenes de 8 bits es **a partir de los 30 dB (decibeles)**.

**Criterios de calidad según PSNR:**

- **PSNR > 40 dB**: Calidad excelente, diferencias imperceptibles
- **PSNR ≈ 30-40 dB**: Buena calidad, aceptable para la mayoría de aplicaciones
- **PSNR < 30 dB**: Calidad pobre, distorsiones notables

**Ejemplo:** En el texto se muestra un ejemplo donde un PSNR de 43.01 dB indica que "la imagen filtrada es de buena calidad", lo cual está considerablemente por encima del umbral mínimo de 30 dB.

La medición en decibeles permite comparar diferencias grandes en una escala reducida y comprensible, siendo una medida intuitiva donde **valores más altos implican mejor calidad** de imagen.

---

## 8.5.2. Preguntas de análisis y comprensión sobre el texto

### 1. ¿Por qué el MSE, a pesar de ser una medida directa de distorsión, tiene limitaciones en la evaluación de la calidad de imágenes? Explique.

El MSE, aunque es una medida cuantitativa útil, presenta varias **limitaciones importantes** en la evaluación de la calidad de imágenes:

#### **Limitaciones principales:**

1. **No captura la percepción humana:**
   - El MSE mide solo el error absoluto entre píxeles correspondientes
   - No considera cómo el sistema visual humano percibe las diferencias
   - Dos imágenes con el mismo MSE pueden tener calidades percibidas muy diferentes

2. **Falta de normalización:**
   - El MSE no está normalizado, por lo que sus valores absolutos dependen de la escala de los píxeles
   - Esto complica la comparación entre diferentes imágenes o condiciones de evaluación
   - Un MSE de 100 puede ser bueno o malo dependiendo del contexto de la imagen

3. **Unidades al cuadrado no intuitivas:**
   - Al medir el error en unidades al cuadrado, el MSE produce valores que no son intuitivos
   - Difícil de interpretar en términos perceptuales, especialmente para valores de error grandes
   - No hay una correspondencia directa con la calidad visual percibida

4. **No considera aspectos estructurales:**
   - El MSE trata todos los errores por igual, sin importar su ubicación o contexto
   - No distingue entre errores que afectan la estructura visual y los que no
   - Ignora aspectos como texturas, bordes y patrones que son importantes para la percepción

#### **Ejemplo ilustrativo:**

Dos imágenes pueden tener el mismo MSE pero diferente calidad percibida:
- **Imagen A**: Ruido uniformemente distribuido (poco perceptible)
- **Imagen B**: Artefactos de bloque concentrados en áreas importantes (muy perceptible)

Ambas podrían tener MSE = 100, pero la Imagen B sería percibida como de mucha peor calidad.

#### **Conclusión:**

Como menciona el texto: *"En muchas ocasiones, una imagen con un MSE bajo aún puede presentar artefactos visuales significativos que no son evidentes a partir de la métrica del error cuadrático. Por ello, el MSE se complementa usualmente con otras métricas como el PSNR y el Índice de Similitud Estructural (SSIM)."*

---

### 2. Compare las ventajas y desventajas del PSNR frente al MSE en la evaluación de la calidad de imágenes filtradas.

#### **Tabla comparativa PSNR vs MSE:**

$$
\begin{array}{|c|c|c|}
\hline
\text{Aspecto} & \text{MSE} & \text{PSNR} \\
\hline
\textbf{Unidades} & \text{Intensidad al cuadrado} & \text{Decibeles (dB)} \\
\textbf{Escala} & \text{Lineal} & \text{Logarítmica} \\
\textbf{Interpretación} & \text{Difícil (valores grandes)} & \text{Intuitiva (más alto = mejor)} \\
\textbf{Rango típico} & 0 \text{ a } \infty & 0 \text{ a } \infty \text{ dB (típicamente 20-50 dB)} \\
\textbf{Normalización} & \text{No normalizado} & \text{Normalizado por } \text{MAX}_I^2 \\
\textbf{Relación} & - & \text{Se calcula a partir del MSE} \\
\hline
\end{array}
$$

#### **Ventajas del PSNR sobre el MSE:**

1. **Interpretación intuitiva:**
   - El uso de decibeles facilita la comprensión de la calidad
   - Un PSNR más alto implica menos distorsión y mejor fidelidad
   - Escala logarítmica más manejable para comparaciones

2. **Normalización implícita:**
   - Al usar $\text{MAX}_I^2$ en el numerador, se normaliza respecto al rango de la imagen
   - Facilita comparaciones entre imágenes con diferentes profundidades de bits

3. **Estándar ampliamente aceptado:**
   - Métrica común en procesamiento de imágenes y video
   - Facilita la comparación entre diferentes algoritmos y técnicas
   - Valores de referencia conocidos (30 dB como mínimo aceptable)

4. **Escala comprimida:**
   - La escala logarítmica comprime grandes diferencias en valores de MSE
   - Hace más evidentes las mejoras incrementales en calidad

#### **Desventajas compartidas entre PSNR y MSE:**

1. **No captura percepción visual:**
   - Ambas métricas no consideran aspectos perceptuales, estructurales o visuales
   - Pueden no correlacionarse bien con la calidad percibida por humanos

2. **Sensibilidad a picos de error:**
   - El PSNR puede ser engañoso en imágenes con errores localizados (artefactos)
   - Estos impactan la percepción visual más de lo que reflejan los valores numéricos

3. **Limitación para altos valores:**
   - Valores muy altos de PSNR pueden no traducirse en mejoras perceptuales significativas
   - El ojo humano no percibe diferencias a partir de ciertos umbrales

#### **Fórmula de relación:**

$$\text{PSNR} = 10 \cdot \log_{10}\left(\frac{\text{MAX}_I^2}{\text{MSE}}\right)$$

**Ventaja clave:** El PSNR convierte el MSE en una escala logarítmica con unidades estándar (dB), haciendo la interpretación mucho más intuitiva sin perder la información cuantitativa del error.

#### **Ejemplo comparativo:**

Para imágenes de 8 bits ($\text{MAX}_I = 255$):

$$
\begin{array}{|c|c|c|}
\hline
\text{MSE} & \text{PSNR (dB)} & \text{Interpretación} \\
\hline
1 & 48.13 & \text{Excelente} \\
10 & 38.13 & \text{Muy buena} \\
100 & 28.13 & \text{Límite aceptable} \\
1000 & 18.13 & \text{Pobre} \\
\hline
\end{array}
$$

Como se observa, el PSNR proporciona una escala más intuitiva para evaluar la calidad.

---

### 3. ¿Cómo se relaciona el SSIM con la percepción visual humana y por qué esto es importante en el procesamiento de imágenes?

#### **Relación del SSIM con la percepción visual humana:**

El SSIM está **específicamente diseñado** para alinearse con la forma en que el sistema visual humano (HVS - Human Visual System) percibe y evalúa la calidad de las imágenes. Esta alineación se logra a través de sus tres componentes fundamentales:

#### **1. Luminancia - Sensibilidad al brillo:**

$$l(r, n) = \frac{2\mu_r\mu_n + C_1}{\mu_r^2 + \mu_n^2 + C_1}$$

- El sistema visual humano es muy sensible a cambios en el brillo promedio
- El componente de luminancia evalúa si la intensidad global es similar entre imágenes
- Los humanos perciben diferencias en brillo antes que diferencias en detalles finos

#### **2. Contraste - Sensibilidad a variaciones:**

$$c(r, n) = \frac{2\sigma_r\sigma_n + C_2}{\sigma_r^2 + \sigma_n^2 + C_2}$$

- El HVS es sensible a las variaciones de intensidad local
- El contraste determina nuestra capacidad de distinguir objetos del fondo
- Las diferencias de contraste son críticas para la percepción de calidad

#### **3. Estructura - Patrones y texturas:**

$$s(r, n) = \frac{\sigma_{rn} + C_3}{\sigma_r\sigma_n + C_3}$$

- El HVS es extremadamente sensible a cambios estructurales
- Reconocemos objetos y escenas principalmente por su estructura
- La correlación espacial entre píxeles es fundamental para la percepción

#### **Importancia en el procesamiento de imágenes:**

**1. Evaluación más realista de la calidad:**
- Proporciona medidas que se correlacionan mejor con juicios subjetivos humanos
- Útil para validar algoritmos de compresión, restauración y mejora
- Ayuda a optimizar parámetros según criterios perceptuales

**2. Aplicaciones críticas:**

**a) Compresión de imágenes/video:**
- Permite ajustar algoritmos para preservar características visualmente importantes
- Optimiza el balance entre tamaño de archivo y calidad percibida
- Streaming y transmisión en tiempo real

**b) Restauración de imágenes:**
- Asegura que el proceso de restauración preserve la integridad estructural
- Evita introducir artefactos perceptualmente molestos
- Valida técnicas de denoising y deblurring

**c) Procesamiento de imágenes médicas:**
- **Crítico**: Garantiza que las imágenes conserven su integridad estructural
- Esencial para diagnóstico clínico correcto
- Detecta degradaciones que podrían afectar la interpretación médica

**d) Realidad aumentada/virtual:**
- Valida que las imágenes sintéticas sean perceptualmente coherentes
- Importante para la experiencia de usuario

#### **Ventaja sobre MSE/PSNR:**

Como menciona el texto: *"Esta técnica está alineada con la percepción visual humana, lo que la hace especialmente útil para comparar imágenes de manera que refleje cómo los humanos perciben las diferencias."*

**Ejemplo práctico:**

Dos imágenes con el mismo MSE/PSNR pero diferente SSIM:
- **Imagen A**: Ruido gaussiano uniforme → MSE bajo, SSIM alto (poco perceptible)
- **Imagen B**: Desplazamiento de bordes → MSE bajo, SSIM bajo (muy perceptible)

El SSIM detecta correctamente que la Imagen B tiene peor calidad percibida, mientras que MSE/PSNR no hacen esta distinción.

#### **Conclusión:**

El SSIM es importante porque **trasciende las métricas puramente numéricas** para incorporar principios de la percepción visual, proporcionando una herramienta más efectiva para evaluar y optimizar algoritmos de procesamiento de imágenes en función de cómo los humanos realmente experimentan la calidad visual.

---

### 4. Analice por qué el SSIM podría ser preferible al MSE o PSNR en aplicaciones de procesamiento de imágenes médicas.

#### **Razones por las que el SSIM es preferible en imágenes médicas:**

El procesamiento de imágenes médicas tiene **requisitos únicos y críticos** que hacen al SSIM particularmente valioso:

#### **1. Preservación de la integridad estructural (CRÍTICO):**

**Importancia médica:**
- En diagnóstico médico, las **estructuras anatómicas** son fundamentales
- Tumores, lesiones, calcificaciones y otras patologías se identifican por su estructura
- La pérdida de información estructural puede llevar a **diagnósticos erróneos**

**Ventaja del SSIM:**
- El componente estructural $s(r,n)$ evalúa específicamente la correlación espacial
- Detecta alteraciones en patrones y texturas que son imperceptibles para MSE/PSNR
- Garantiza que las características anatómicas se preserven fielmente

**Ejemplo:**
```
Escenario: Imagen de mamografía procesada

MSE/PSNR: Valor excelente (MSE bajo, PSNR alto)
- Ruido reducido uniformemente
- Pero microcalcificaciones ligeramente difuminadas

SSIM: Valor moderado
- Detecta pérdida de definición estructural
- Alerta sobre posible degradación de características diagnósticas
```

#### **2. Sensibilidad a distorsiones clínicamente relevantes:**

**a) Cambios de contraste locales:**
- En imágenes médicas, el contraste local es esencial para distinguir tejidos
- El componente $c(r,n)$ del SSIM evalúa específicamente el contraste
- MSE/PSNR pueden no detectar pérdida de contraste que sí afecta el diagnóstico

**b) Preservación de bordes:**
- Bordes de órganos, tumores y vasos sanguíneos son críticos
- SSIM es sensible a cambios en bordes y transiciones
- MSE puede reportar valores bajos incluso con bordes degradados

**c) Detección de artefactos:**
- Artefactos de compresión o procesamiento pueden ser clínicamente engañosos
- SSIM detecta artefactos estructurales mejor que métricas puntuales

#### **3. Modalidades médicas específicas:**

**a) Resonancia Magnética (MRI):**
- Múltiples secuencias con diferentes contrastes
- La estructura anatómica debe preservarse a través de todas las secuencias
- SSIM valida consistencia estructural entre secuencias

**b) Tomografía Computarizada (CT):**
- Reconstrucción 3D a partir de proyecciones
- Estructuras óseas y tejidos blandos deben mantenerse coherentes
- SSIM asegura integridad en reconstrucciones

**c) Rayos X digitales:**
- Compresión necesaria para almacenamiento PACS
- SSIM garantiza que estructuras finas (fracturas, nódulos) se preserven

**d) Ultrasonido:**
- Alto nivel de ruido speckle
- SSIM evalúa si el denoising preserva estructuras reales vs. elimina ruido

#### **4. Regulaciones y responsabilidad legal:**

**Contexto regulatorio:**
- Imágenes médicas están sujetas a regulaciones estrictas (FDA, CE, etc.)
- Se requiere demostrar que el procesamiento no afecta el valor diagnóstico
- SSIM proporciona evidencia cuantitativa de preservación estructural

**Trazabilidad:**
- SSIM permite documentar que el procesamiento mantiene fidelidad diagnóstica
- Importante en auditorías y casos médico-legales

#### **5. Optimización de algoritmos médicos:**

**a) Reducción de dosis de radiación:**
- Objetivo: minimizar radiación manteniendo calidad diagnóstica
- SSIM guía la optimización: ¿cuánto ruido es aceptable sin perder estructura?
- MSE/PSNR no proporcionan esta información perceptual

**b) Compresión de archivos PACS:**
- Sistemas de almacenamiento médico requieren compresión
- SSIM asegura que la compresión sea "perceptualmente lossless"
- Equilibrio entre espacio de almacenamiento y calidad diagnóstica

**c) Registro y fusión de imágenes:**
- Combinación de diferentes modalidades (PET/CT, MRI/fMRI)
- SSIM valida que las estructuras coincidan correctamente
- MSE puede ser bajo incluso con desalineaciones estructurales

#### **6. Correlación con evaluación de radiólogos:**

**Validación clínica:**
- Estudios muestran que SSIM se correlaciona mejor con evaluaciones de expertos
- Los radiólogos evalúan calidad basándose en:
  - Visibilidad de estructuras anatómicas (estructura)
  - Diferenciación de tejidos (contraste)
  - Brillo apropiado (luminancia)
- Estos son exactamente los componentes del SSIM

**Ejemplo de flujo de trabajo:**

```
Algoritmo de denoising en imágenes CT:

Paso 1: Aplicar filtrado
Paso 2: Evaluar con múltiples métricas

MSE = 50  → Parece aceptable
PSNR = 31 dB → Justo en el límite
SSIM = 0.75 → CUIDADO: Estructura comprometida

Decisión: Ajustar parámetros del filtro
         Mayor preservación estructural
         
Resultado final:
MSE = 80  → Aumentó
PSNR = 29 dB → Disminuyó  
SSIM = 0.92 → Estructura preservada

Validación radiológica: Imagen diagnósticamente superior
```

#### **7. Comparación de métricas en contexto médico:**

$$
\begin{array}{|c|c|c|c|}
\hline
\text{Aspecto} & \text{MSE/PSNR} & \text{SSIM} & \text{Relevancia Médica} \\
\hline
\text{Ruido uniforme} & \text{Detecta bien} & \text{Detecta bien} & \text{Media} \\
\text{Pérdida de bordes} & \text{Detecta mal} & \text{Detecta bien} & \textbf{ALTA} \\
\text{Cambios de contraste} & \text{Detecta mal} & \text{Detecta bien} & \textbf{ALTA} \\
\text{Distorsión estructural} & \text{Detecta mal} & \text{Detecta bien} & \textbf{CRÍTICA} \\
\text{Artefactos localizados} & \text{Detecta mal} & \text{Detecta bien} & \textbf{ALTA} \\
\text{Difuminación} & \text{Detecta mal} & \text{Detecta bien} & \textbf{ALTA} \\
\hline
\end{array}
$$

#### **8. Limitaciones y consideraciones:**

**Aspectos a considerar:**
- SSIM es computacionalmente más costoso (puede ser aceptable en contexto no crítico de tiempo)
- Requiere implementación cuidadosa para ventanas apropiadas
- Debe complementarse con evaluación clínica, no reemplazarla

#### **Conclusión:**

Como menciona el texto: *"En el ámbito del procesamiento de imágenes médicas, desempeña un papel relevante al garantizar que las imágenes conserven su integridad estructural, algo **crítico en el diagnóstico clínico**."*

El SSIM es preferible en aplicaciones médicas porque:

1. **Preserva estructuras críticas** para el diagnóstico
2. **Detecta distorsiones clínicamente relevantes** que MSE/PSNR pasan por alto
3. **Se correlaciona con evaluación de expertos** médicos
4. **Proporciona trazabilidad regulatoria** y legal
5. **Optimiza algoritmos** según criterios diagnósticos reales

En imágenes médicas, **un error en la evaluación de calidad puede tener consecuencias fatales**. El SSIM proporciona la seguridad adicional de que las imágenes procesadas mantienen su valor diagnóstico, algo que las métricas puramente numéricas no pueden garantizar.

---

### 5. ¿Cómo afecta la escala logarítmica del PSNR a la interpretación de la calidad de la imagen en comparación con el MSE?

#### **Impacto de la escala logarítmica del PSNR:**

La transformación logarítmica del MSE al PSNR tiene efectos profundos en cómo interpretamos y comparamos la calidad de imágenes:

#### **1. Compresión de la escala de valores:**

**Fórmula de transformación:**

$$\text{PSNR} = 10 \cdot \log_{10}\left(\frac{\text{MAX}_I^2}{\text{MSE}}\right)$$

**Efecto de compresión:**

$$
\begin{array}{|c|c|c|}
\hline
\text{MSE} & \text{PSNR (dB)} & \text{Diferencia PSNR} \\
\hline
1 & 48.13 & - \\
10 & 38.13 & -10 \text{ dB} \\
100 & 28.13 & -10 \text{ dB} \\
1000 & 18.13 & -10 \text{ dB} \\
10000 & 8.13 & -10 \text{ dB} \\
\hline
\end{array}
$$

**Observación clave:** 
- Incrementos de **10× en MSE** corresponden a reducciones de **10 dB en PSNR**
- La escala logarítmica comprime grandes variaciones en MSE a diferencias manejables en dB
- Facilita la comparación visual de rangos muy diferentes

#### **2. Interpretación intuitiva mejorada:**

**Escala MSE (lineal):**
```
MSE:    1    10    100    1000    10000    100000
        |----|----|------|-------|--------|---------> (difícil de visualizar)
        Excelente          Pobre         Terrible
```

**Escala PSNR (logarítmica):**
```
PSNR:   48   38    28     18      8       -2  dB
        |----|----|------|-------|--------|---------> (fácil de visualizar)
        Excelente  Límite  Pobre  Terrible
```

**Ventajas de interpretación:**
- Valores típicos en un rango manejable (20-50 dB)
- Umbrales conocidos (30 dB como mínimo aceptable)
- Diferencias incrementales más evidentes

#### **3. Sensibilidad diferencial a cambios:**

**a) Región de alta calidad (MSE bajo):**

**Ejemplo:** Mejora de MSE = 10 a MSE = 1

$$\Delta\text{PSNR} = 38.13 - 48.13 = 10 \text{ dB}$$

- **Cambio en MSE:** Factor de 10 (90% de reducción)
- **Cambio en PSNR:** +10 dB (incremento significativo)
- **Interpretación:** Mejora sustancial muy visible

**b) Región de baja calidad (MSE alto):**

**Ejemplo:** Mejora de MSE = 1000 a MSE = 100

$$\Delta\text{PSNR} = 18.13 - 28.13 = 10 \text{ dB}$$

- **Cambio en MSE:** Factor de 10 (90% de reducción)
- **Cambio en PSNR:** +10 dB (mismo incremento)
- **Interpretación:** Mejora proporcional a la escala de calidad

**Implicación:** La escala logarítmica hace que **mejoras proporcionales** (factores multiplicativos) en MSE se traduzcan en **incrementos aditivos constantes** en PSNR.

#### **4. Comparación visual de mejoras incrementales:**

**Escenario: Optimización de un algoritmo de compresión**

**Con MSE (lineal):**
```
Iteración 1: MSE = 1000
Iteración 2: MSE = 500  → Mejora de 500 (50%)
Iteración 3: MSE = 250  → Mejora de 250 (50%)
Iteración 4: MSE = 125  → Mejora de 125 (50%)
Iteración 5: MSE = 62.5 → Mejora de 62.5 (50%)
```
- **Problema:** Las mejoras absolutas disminuyen, difícil ver el progreso proporcional
- **Visualización:** La gráfica es exponencial decreciente

**Con PSNR (logarítmico):**
```
Iteración 1: PSNR = 18.13 dB
Iteración 2: PSNR = 21.14 dB → Mejora de 3.01 dB
Iteración 3: PSNR = 24.15 dB → Mejora de 3.01 dB
Iteración 4: PSNR = 27.16 dB → Mejora de 3.01 dB
Iteración 5: PSNR = 30.17 dB → Mejora de 3.01 dB
```
- **Ventaja:** Las mejoras proporcionales constantes (50%) son visibles como incrementos constantes
- **Visualización:** La gráfica es lineal ascendente

#### **5. Percepción de diferencias de calidad:**

**Aspecto psicofísico:**

La **Ley de Weber-Fechner** establece que la percepción humana de cambios es **logarítmica**:
- Los humanos percibimos diferencias proporcionales, no absolutas
- Un cambio de 1 a 2 se percibe similar a un cambio de 10 a 20

**Alineación del PSNR:**
- La escala logarítmica del PSNR se alinea mejor con la percepción humana
- Diferencias de 3 dB son típicamente perceptibles
- Diferencias de 1 dB pueden ser apenas detectables

**Ejemplo perceptual:**

$$
\begin{array}{|c|c|c|c|}
\hline
\text{Comparación} & \text{Diferencia MSE} & \text{Diferencia PSNR} & \text{Percepción} \\
\hline
A \text{ (MSE=1) vs } B \text{ (MSE=2)} & 1 & 3 \text{ dB} & \text{Apenas notable} \\
C \text{ (MSE=100) vs } D \text{ (MSE=101)} & 1 & 0.04 \text{ dB} & \text{Imperceptible} \\
E \text{ (MSE=1) vs } F \text{ (MSE=10)} & 9 & 10 \text{ dB} & \text{Muy notable} \\
G \text{ (MSE=100) vs } H \text{ (MSE=1000)} & 900 & 10 \text{ dB} & \text{Muy notable (similar a E vs F)} \\
\hline
\end{array}
$$

**Conclusión:** PSNR refleja mejor la **percepción relativa** de diferencias de calidad.

#### **6. Facilitación de benchmarking:**

**Ventajas para comparación de algoritmos:**

**Con MSE:**
```
Algoritmo A: MSE = 85
Algoritmo B: MSE = 90  → 5.9% peor
Algoritmo C: MSE = 150 → 76% peor

¿Es el algoritmo B mucho mejor que C?
```

**Con PSNR:**
```
Algoritmo A: PSNR = 38.84 dB
Algoritmo B: PSNR = 38.59 dB → -0.25 dB (diferencia marginal)
Algoritmo C: PSNR = 36.37 dB → -2.47 dB (diferencia notable)

Interpretación clara: B ≈ A > C
```

**Estándares de la industria:**
- Benchmarks típicos: "Nuestro algoritmo logra 35 dB en CODEC X"
- Comparaciones directas: "+2 dB sobre el estado del arte"
- Umbrales conocidos: "Cumple con el estándar de 30 dB mínimo"

#### **7. Limitaciones de la escala logarítmica:**

**a) Asimetría en errores:**

$$\text{PSNR}(\text{MSE} \rightarrow 0) \rightarrow \infty$$

- Cuando MSE es muy pequeño, PSNR tiende a infinito
- Diferencias en región de muy alta calidad pueden ser exageradas

**b) Sensibilidad reducida en baja calidad:**

- Grandes cambios en MSE alto se comprimen en pequeños cambios de PSNR
- Puede ocultar mejoras significativas en imágenes de baja calidad

**c) No elimina la desconexión perceptual:**

- Aunque la escala es logarítmica, PSNR sigue sin capturar aspectos estructurales
- La mejora de interpretación es sobre MSE, no sobre percepción visual real

#### **8. Ejemplo comparativo completo:**

**Contexto:** Evaluación de 5 versiones de un algoritmo de denoising

$$
\begin{array}{|c|c|c|c|c|c|c|}
\hline
\text{Versión} & \text{MSE} & \text{Cambio MSE} & \text{PSNR (dB)} & \text{Cambio PSNR} & \text{Interpretación MSE} & \text{Interpretación PSNR} \\
\hline
V1 & 1000 & - & 18.13 & - & \text{Inicial} & \text{Muy pobre} \\
V2 & 500 & -500 \text{ (-50\%)} & 21.14 & +3.01 & \text{Mejora grande} & \text{Mejora constante} \\
V3 & 250 & -250 \text{ (-50\%)} & 24.15 & +3.01 & \text{Mejora media} & \text{Mejora constante} \\
V4 & 125 & -125 \text{ (-50\%)} & 27.16 & +3.01 & \text{Mejora pequeña} & \text{Mejora constante} \\
V5 & 62.5 & -62.5 \text{ (-50\%)} & 30.17 & +3.01 & \text{Mejora muy pequeña} & \text{Mejora constante} \\
\hline
\end{array}
$$

**Con MSE:** Parece que el progreso se desacelera (mejoras absolutas decrecientes)

**Con PSNR:** Muestra claramente que cada versión logra una mejora proporcional constante (50% de reducción en error)

#### **Conclusión:**

La escala logarítmica del PSNR afecta profundamente la interpretación de la calidad:

1. **Comprime rangos grandes** en escalas manejables
2. **Hace visibles mejoras proporcionales** como incrementos constantes
3. **Se alinea mejor** con la percepción humana logarítmica
4. **Facilita comparaciones** y establecimiento de umbrales
5. **No elimina** las limitaciones fundamentales de no capturar aspectos estructurales

Como menciona el texto: *"A diferencia del MSE, que mide el error absoluto entre las imágenes, el PSNR traduce esa diferencia en una escala logarítmica que **facilita la interpretación de la calidad percibida**; un valor más alto de PSNR corresponde a una mejor calidad de imagen filtrada."*

El PSNR no es una métrica fundamentalmente diferente al MSE, sino una **re-escala logarítmica** que hace el MSE más interpretable, comparable y alineado con la percepción humana de diferencias proporcionales.

---

## 8.5.3. Problemas Numéricos sobre Ruido en Imágenes

### 1. Calcule el Error Cuadrático Medio (MSE), el PSNR y el SSIM para las imágenes $I_r$ de referencia e $I_n$ con ruido de $2 \times 2$ píxeles:

$$I_r = \begin{bmatrix} 100 & 150 \\ 200 & 250 \end{bmatrix} \quad I_n = \begin{bmatrix} 105 & 148 \\ 202 & 245 \end{bmatrix}$$

#### **Solución:**

#### **Paso 1: Calcular el MSE**

La fórmula del MSE para imágenes de tamaño $M \times N$ es:

$$\text{MSE} = \frac{1}{M \times N} \sum_{i=1}^{M} \sum_{j=1}^{N} [I_r(i,j) - I_n(i,j)]^2$$

Para nuestras imágenes de $2 \times 2$:

$$\text{MSE} = \frac{1}{2 \times 2} [(100-105)^2 + (150-148)^2 + (200-202)^2 + (250-245)^2]$$

Calculando cada término:
- $(100-105)^2 = (-5)^2 = 25$
- $(150-148)^2 = (2)^2 = 4$
- $(200-202)^2 = (-2)^2 = 4$
- $(250-245)^2 = (5)^2 = 25$

$$\text{MSE} = \frac{1}{4}(25 + 4 + 4 + 25) = \frac{58}{4} = 14.5$$

**Respuesta:** $\boxed{\text{MSE} = 14.5}$

---

#### **Paso 2: Calcular el PSNR**

La fórmula del PSNR es:

$$\text{PSNR} = 10 \cdot \log_{10}\left(\frac{\text{MAX}_I^2}{\text{MSE}}\right)$$

Para una imagen de 8 bits: $\text{MAX}_I = 255$

$$\text{PSNR} = 10 \cdot \log_{10}\left(\frac{255^2}{14.5}\right) = 10 \cdot \log_{10}\left(\frac{65025}{14.5}\right)$$

$$\text{PSNR} = 10 \cdot \log_{10}(4484.48) = 10 \times 3.6517 = 36.517 \text{ dB}$$

**Respuesta:** $\boxed{\text{PSNR} = 36.52 \text{ dB}}$

**Interpretación:** Este valor está por encima del umbral mínimo de 30 dB, indicando una calidad aceptable.

---

#### **Paso 3: Calcular el SSIM**

El SSIM se calcula mediante:

$$\text{SSIM}(r, n) = l(r, n) \cdot c(r, n) \cdot s(r, n)$$

**a) Calcular las medias $\mu_r$ y $\mu_n$:**

$$\mu_r = \frac{1}{4}(100 + 150 + 200 + 250) = \frac{700}{4} = 175$$

$$\mu_n = \frac{1}{4}(105 + 148 + 202 + 245) = \frac{700}{4} = 175$$

**b) Calcular las desviaciones estándar $\sigma_r$ y $\sigma_n$:**

$$\sigma_r = \sqrt{\frac{1}{4}\sum_{i,j}[I_r(i,j) - \mu_r]^2}$$

$$\sigma_r = \sqrt{\frac{1}{4}[(100-175)^2 + (150-175)^2 + (200-175)^2 + (250-175)^2]}$$

$$\sigma_r = \sqrt{\frac{1}{4}[5625 + 625 + 625 + 5625]} = \sqrt{\frac{12500}{4}} = \sqrt{3125} = 55.90$$

$$\sigma_n = \sqrt{\frac{1}{4}[(105-175)^2 + (148-175)^2 + (202-175)^2 + (245-175)^2]}$$

$$\sigma_n = \sqrt{\frac{1}{4}[4900 + 729 + 729 + 4900]} = \sqrt{\frac{11258}{4}} = \sqrt{2814.5} = 53.05$$

**c) Calcular la covarianza $\sigma_{rn}$:**

$$\sigma_{rn} = \frac{1}{4}\sum_{i,j}[I_r(i,j) - \mu_r][I_n(i,j) - \mu_n]$$

$$\sigma_{rn} = \frac{1}{4}[(100-175)(105-175) + (150-175)(148-175)$$
$$+ (200-175)(202-175) + (250-175)(245-175)]$$

$$\sigma_{rn} = \frac{1}{4}[(-75)(-70) + (-25)(-27) + (25)(27) + (75)(70)]$$

$$\sigma_{rn} = \frac{1}{4}[5250 + 675 + 675 + 5250] = \frac{11850}{4} = 2962.5$$

**d) Definir las constantes para $L = 255$:**

$$C_1 = (0.01 \times 255)^2 = (2.55)^2 = 6.5025$$

$$C_2 = (0.03 \times 255)^2 = (7.65)^2 = 58.5225$$

$$C_3 = \frac{C_2}{2} = \frac{58.5225}{2} = 29.2612$$

**e) Calcular los componentes del SSIM:**

**Luminancia:**

$$l(r,n) = \frac{2\mu_r\mu_n + C_1}{\mu_r^2 + \mu_n^2 + C_1} = \frac{2(175)(175) + 6.5025}{175^2 + 175^2 + 6.5025}$$

$$l(r,n) = \frac{61250 + 6.5025}{30625 + 30625 + 6.5025} = \frac{61256.5025}{61256.5025} = 1.0000$$

**Contraste:**

$$c(r,n) = \frac{2\sigma_r\sigma_n + C_2}{\sigma_r^2 + \sigma_n^2 + C_2} = \frac{2(55.90)(53.05) + 58.5225}{55.90^2 + 53.05^2 + 58.5225}$$

$$c(r,n) = \frac{5930.79 + 58.5225}{3124.81 + 2814.30 + 58.5225} = \frac{5989.3125}{5997.6325} = 0.9986$$

**Estructura:**

$$s(r,n) = \frac{\sigma_{rn} + C_3}{\sigma_r\sigma_n + C_3} = \frac{2962.5 + 29.2612}{(55.90)(53.05) + 29.2612}$$

$$s(r,n) = \frac{2991.7612}{2965.4950 + 29.2612} = \frac{2991.7612}{2994.7562} = 0.9990$$

**f) SSIM total:**

$$\text{SSIM}(r,n) = l(r,n) \cdot c(r,n) \cdot s(r,n)$$

$$\text{SSIM}(r,n) = 1.0000 \times 0.9986 \times 0.9990 = 0.9976$$

**Respuesta:** $\boxed{\text{SSIM} = 0.9976}$

**Interpretación:** Un SSIM de 0.9976 (muy cercano a 1) indica una similitud estructural excelente entre las dos imágenes.

---

#### **Resumen de resultados:**

$$
\begin{array}{|c|c|c|}
\hline
\text{Métrica} & \text{Valor} & \text{Interpretación} \\
\hline
\text{MSE} & 14.5 & \text{Error promedio al cuadrado moderado} \\
\text{PSNR} & 36.52 \text{ dB} & \text{Calidad buena (> 30 dB)} \\
\text{SSIM} & 0.9976 & \text{Similitud estructural excelente} \\
\hline
\end{array}
$$

---

### 2. Dadas dos imágenes de 8 bits con un MSE calculado de 100, determine el valor de PSNR.

#### **Solución:**

**Datos:**
- MSE = 100
- Imágenes de 8 bits → $\text{MAX}_I = 2^8 - 1 = 255$

**Fórmula del PSNR:**

$$\text{PSNR} = 10 \cdot \log_{10}\left(\frac{\text{MAX}_I^2}{\text{MSE}}\right)$$

**Sustituyendo valores:**

$$\text{PSNR} = 10 \cdot \log_{10}\left(\frac{255^2}{100}\right)$$

$$\text{PSNR} = 10 \cdot \log_{10}\left(\frac{65025}{100}\right)$$

$$\text{PSNR} = 10 \cdot \log_{10}(650.25)$$

$$\text{PSNR} = 10 \times 2.8131 = 28.131 \text{ dB}$$

**Respuesta:** $\boxed{\text{PSNR} = 28.13 \text{ dB}}$

**Interpretación:** 
- Este valor está **ligeramente por debajo** del umbral mínimo aceptable de 30 dB
- Indica calidad **límite o pobre**
- Sería necesario mejorar el algoritmo o reducir el ruido para alcanzar el estándar de 30 dB

---

### 3. Calcule los componentes de luminancia $l$ y contraste $c$ del SSIM para las imágenes $I_r$ de referencia e $I_n$ con ruido de $2 \times 2$ píxeles:

$$I_r = \begin{bmatrix} 50 & 100 \\ 150 & 200 \end{bmatrix} \quad I_n = \begin{bmatrix} 55 & 98 \\ 152 & 195 \end{bmatrix}$$

Utilice $C_1 = 6.5025$ y $C_2 = 58.5225$.

#### **Solución:**

#### **Paso 1: Calcular las medias $\mu_r$ y $\mu_n$**

$$\mu_r = \frac{1}{2 \times 2}(50 + 100 + 150 + 200) = \frac{500}{4} = 125$$

$$\mu_n = \frac{1}{2 \times 2}(55 + 98 + 152 + 195) = \frac{500}{4} = 125$$

---

#### **Paso 2: Calcular el componente de luminancia $l(r,n)$**

**Fórmula:**

$$l(r,n) = \frac{2\mu_r\mu_n + C_1}{\mu_r^2 + \mu_n^2 + C_1}$$

**Sustituyendo valores:**

$$l(r,n) = \frac{2(125)(125) + 6.5025}{125^2 + 125^2 + 6.5025}$$

$$l(r,n) = \frac{31250 + 6.5025}{15625 + 15625 + 6.5025}$$

$$l(r,n) = \frac{31256.5025}{31256.5025} = 1.0000$$

**Respuesta:** $\boxed{l(r,n) = 1.0000}$

**Interpretación:** Las dos imágenes tienen exactamente la misma luminancia (brillo promedio), lo que indica una similitud perfecta en este aspecto.

---

#### **Paso 3: Calcular las desviaciones estándar $\sigma_r$ y $\sigma_n$**

**Para $\sigma_r$:**

$$\sigma_r = \sqrt{\frac{1}{4}\sum_{i,j}[I_r(i,j) - \mu_r]^2}$$

$$\sigma_r = \sqrt{\frac{1}{4}[(50-125)^2 + (100-125)^2 + (150-125)^2 + (200-125)^2]}$$

Calculando cada término:
- $(50-125)^2 = (-75)^2 = 5625$
- $(100-125)^2 = (-25)^2 = 625$
- $(150-125)^2 = (25)^2 = 625$
- $(200-125)^2 = (75)^2 = 5625$

$$\sigma_r = \sqrt{\frac{1}{4}(5625 + 625 + 625 + 5625)} = \sqrt{\frac{12500}{4}} = \sqrt{3125} = 55.90$$

**Para $\sigma_n$:**

$$\sigma_n = \sqrt{\frac{1}{4}\sum_{i,j}[I_n(i,j) - \mu_n]^2}$$

$$\sigma_n = \sqrt{\frac{1}{4}[(55-125)^2 + (98-125)^2 + (152-125)^2 + (195-125)^2]}$$

Calculando cada término:
- $(55-125)^2 = (-70)^2 = 4900$
- $(98-125)^2 = (-27)^2 = 729$
- $(152-125)^2 = (27)^2 = 729$
- $(195-125)^2 = (70)^2 = 4900$

$$\sigma_n = \sqrt{\frac{1}{4}(4900 + 729 + 729 + 4900)} = \sqrt{\frac{11258}{4}} = \sqrt{2814.5} = 53.05$$

---

#### **Paso 4: Calcular el componente de contraste $c(r,n)$**

**Fórmula:**

$$c(r,n) = \frac{2\sigma_r\sigma_n + C_2}{\sigma_r^2 + \sigma_n^2 + C_2}$$

**Sustituyendo valores:**

$$c(r,n) = \frac{2(55.90)(53.05) + 58.5225}{(55.90)^2 + (53.05)^2 + 58.5225}$$

Calculando el numerador:
$$2(55.90)(53.05) = 5930.79$$
$$\text{Numerador} = 5930.79 + 58.5225 = 5989.3125$$

Calculando el denominador:
$$(55.90)^2 = 3124.81$$
$$(53.05)^2 = 2814.30$$
$$\text{Denominador} = 3124.81 + 2814.30 + 58.5225 = 5997.6325$$

$$c(r,n) = \frac{5989.3125}{5997.6325} = 0.9986$$

**Respuesta:** $\boxed{c(r,n) = 0.9986}$

**Interpretación:** El contraste entre las dos imágenes es muy similar (0.9986 ≈ 1), indicando que la variabilidad de intensidades es casi idéntica.

---

#### **Resumen de resultados:**

$$
\begin{array}{|c|c|c|}
\hline
\text{Componente} & \text{Valor} & \text{Interpretación} \\
\hline
\textbf{Luminancia } l(r,n) & 1.0000 & \text{Brillo promedio idéntico} \\
\textbf{Contraste } c(r,n) & 0.9986 & \text{Variabilidad muy similar} \\
\hline
\end{array}
$$

**Nota:** Estos componentes forman parte del cálculo del SSIM total. Para obtener el SSIM completo, también se necesitaría calcular el componente de estructura $s(r,n)$ usando la covarianza.

---

### 4. Una imagen de $512 \times 512$ píxeles tiene un valor máximo de píxel de 255. Si el PSNR entre esta y una versión comprimida es de 30 dB, ¿cuál es el valor del MSE?

#### **Solución:**

**Datos:**
- Dimensiones: $512 \times 512$ píxeles (no relevante para el cálculo del MSE a partir del PSNR)
- $\text{MAX}_I = 255$ (imagen de 8 bits)
- $\text{PSNR} = 30$ dB

**Fórmula del PSNR:**

$$\text{PSNR} = 10 \cdot \log_{10}\left(\frac{\text{MAX}_I^2}{\text{MSE}}\right)$$

**Despejando MSE:**

$$30 = 10 \cdot \log_{10}\left(\frac{255^2}{\text{MSE}}\right)$$

Dividiendo ambos lados por 10:

$$3 = \log_{10}\left(\frac{255^2}{\text{MSE}}\right)$$

Aplicando la exponencial base 10 a ambos lados:

$$10^3 = \frac{255^2}{\text{MSE}}$$

$$1000 = \frac{65025}{\text{MSE}}$$

Despejando MSE:

$$\text{MSE} = \frac{65025}{1000} = 65.025$$

**Respuesta:** $\boxed{\text{MSE} = 65.025}$

**Interpretación:** 
- Un PSNR de **exactamente 30 dB** corresponde a un MSE de **65.025**
- Este es el **límite mínimo aceptable** de calidad para imágenes de 8 bits
- Un MSE mayor (y por tanto PSNR menor) indicaría calidad insuficiente

**Verificación:**

$$\text{PSNR} = 10 \cdot \log_{10}\left(\frac{65025}{65.025}\right) = 10 \cdot \log_{10}(1000) = 10 \times 3 = 30 \text{ dB} \quad \checkmark$$

---

### 5. Calcule el SSIM total para dos imágenes, dado que:

$$l(r, n) = 0.98, \quad c(r, n) = 0.95, \quad s(r, n) = 0.97$$

Asuma que $\alpha = \beta = \gamma = 1$ en la fórmula del SSIM.

#### **Solución:**

**Fórmula general del SSIM:**

$$\text{SSIM}(r, n) = [l(r, n)]^\alpha \cdot [c(r, n)]^\beta \cdot [s(r, n)]^\gamma$$

**Con los parámetros dados:** $\alpha = \beta = \gamma = 1$

La fórmula se simplifica a:

$$\text{SSIM}(r, n) = l(r, n) \cdot c(r, n) \cdot s(r, n)$$

**Sustituyendo los valores:**

$$\text{SSIM}(r, n) = 0.98 \times 0.95 \times 0.97$$

**Cálculo paso a paso:**

$$0.98 \times 0.95 = 0.931$$

$$0.931 \times 0.97 = 0.90307$$

**Respuesta:** $\boxed{\text{SSIM}(r, n) = 0.9031}$

**Interpretación:**

- **SSIM = 0.9031** indica una **alta similitud estructural** entre las imágenes
- Está relativamente cerca de 1 (similitud perfecta)
- Los tres componentes son altos (todos > 0.95), lo que sugiere:
  - **Luminancia** (0.98): Brillo muy similar
  - **Contraste** (0.95): Variabilidad similar
  - **Estructura** (0.97): Patrones y correlaciones muy similares

**Análisis del impacto de cada componente:**

$$
\begin{array}{|c|c|c|}
\hline
\text{Componente} & \text{Valor} & \text{Contribución al resultado} \\
\hline
\text{Luminancia } l & 0.98 & \text{Reduce SSIM en 2\%} \\
\text{Contraste } c & 0.95 & \text{Reduce SSIM en 5\%} \\
\text{Estructura } s & 0.97 & \text{Reduce SSIM en 3\%} \\
\hline
\textbf{Total} & \textbf{0.9031} & \textbf{Reducción acumulada }\sim\textbf{10\%} \\
\hline
\end{array}
$$

**Nota sobre el producto:** El SSIM usa el **producto** de los tres componentes, por lo que cualquier componente bajo afecta desproporcionadamente el resultado. En este caso, el contraste (0.95) es el componente más bajo y tiene el mayor impacto en reducir el SSIM final.

**Comparación con estándares:**

- SSIM > 0.90 generalmente indica **buena calidad**
- SSIM > 0.95 indica **muy buena calidad**
- SSIM > 0.98 indica **excelente calidad**

El valor de **0.9031** está justo por encima del umbral de buena calidad, sugiriendo que las imágenes son muy similares pero tienen algunas diferencias notables, principalmente en el contraste.

---