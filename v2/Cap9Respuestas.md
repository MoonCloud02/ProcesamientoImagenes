# Capítulo 9: Filtros Espaciales - Respuestas

## 9.8. Preguntas

---

## 9.8.1. Preguntas sobre hechos indicados en el texto

### 1. ¿Qué es el filtrado espacial en el procesamiento de imágenes?

El **filtrado espacial** es una técnica esencial en el procesamiento de imágenes que emplea operaciones con ventanas para modificar las características locales de una imagen. 

**Características principales:**

- **Opera directamente en el dominio espacial**: Trabaja sobre los píxeles de la imagen sin necesidad de transformaciones a otros dominios (como el dominio de frecuencia).

- **Utiliza ventanas de tamaño variable**: Emplea matrices de tamaño específico (típicamente 3×3, 5×5, 7×7, etc.) que se desplazan por toda la imagen.

- **Procesamiento píxel por píxel**: La ventana recorre la imagen píxel por píxel, realizando operaciones aritméticas o estadísticas con los píxeles vecinos.

- **Opera sobre múltiples píxeles simultáneamente**: En cada posición, se consideran varios píxeles dentro de la ventana para calcular el nuevo valor del píxel central.

**Aplicaciones del filtrado espacial:**

1. **Eliminación de ruido**: Reducir el ruido presente en la imagen mediante filtros de suavizado
2. **Mejora de calidad**: Realzar características específicas de la imagen
3. **Enfatización o atenuación**: Destacar o reducir aspectos específicos como bordes o texturas
4. **Detección de bordes**: Identificar límites entre regiones usando operadores de gradiente
5. **Suavizado**: Reducir detalles finos y variaciones rápidas de intensidad

**Componentes fundamentales:**

- **Ventana**: Área local de la imagen que se selecciona y examina para aplicar una operación. Se desplaza por toda la imagen, abarcando diferentes regiones de píxeles en cada paso.

- **Kernel**: Matriz de valores que contiene los coeficientes o pesos que se aplican a las intensidades de los píxeles dentro de cada ventana para generar el valor filtrado.

**Resumen:** La ventana delimita el área que se procesa, y el kernel especifica cómo se transforma la información contenida en esa área. Esta metodología permite realizar transformaciones locales adaptadas a las características específicas de cada región de la imagen.

---

### 2. ¿Cuál es la diferencia principal entre la correlación y la convolución en dos dimensiones?

La diferencia principal entre **correlación** y **convolución** radica en la **orientación del kernel** durante la operación:

#### **Correlación en 2D:**

**Definición matemática:**

$$I'(x, y) = \sum_{i=-m}^{m} \sum_{j=-n}^{n} I(x + i, y + j) \cdot k(i, j)$$

También se puede escribir como:

$$I' = I \star k$$

**Características:**
- El kernel $k(i, j)$ se aplica **directamente** en su orientación original
- Los índices $i$ y $j$ recorren desde $-m$ hasta $m$ y desde $-n$ hasta $n$
- Se desplaza el kernel sobre la imagen sin rotarlo
- Realiza multiplicaciones elemento a elemento entre los valores del kernel y los píxeles correspondientes

#### **Convolución en 2D:**

**Definición matemática (forma estándar):**

$$I'(x, y) = \sum_{i=-m}^{m} \sum_{j=-n}^{n} I(x - i, y - j) \cdot k(i, j)$$

**Definición alternativa (con kernel rotado):**

$$I'(x, y) = \sum_{i=-m}^{m} \sum_{j=-n}^{n} I(x + i, y + j) \cdot k(-i, -j)$$

También se puede escribir como:

$$I' = I * k$$

**Características:**
- El kernel $k(i, j)$ se **rota 180 grados** antes de aplicarse
- La rotación se logra invirtiendo los signos: $k(-i, -j)$
- Equivale a aplicar primero una rotación horizontal y luego vertical del kernel
- La convolución puede resolverse como una correlación si se rota el kernel previamente

#### **Diferencia fundamental:**

$$
\begin{array}{|c|c|c|}
\hline
\text{Aspecto} & \text{Correlación } I \star k & \text{Convolución } I * k \\
\hline
\textbf{Orientación del kernel} & \text{Original} & \text{Rotado 180°} \\
\textbf{Fórmula con desplazamiento} & I(x + i, y + j) \cdot k(i, j) & I(x - i, y - j) \cdot k(i, j) \\
\textbf{Fórmula alternativa} & I(x + i, y + j) \cdot k(i, j) & I(x + i, y + j) \cdot k(-i, -j) \\
\textbf{Operador} & \star & * \\
\hline
\end{array}
$$

#### **Ejemplo ilustrativo:**

Para una imagen $I$ y kernel $k$:

$$I = \begin{bmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \\ 7 & 8 & 9 \end{bmatrix} \quad k = \begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix}$$

**Para el píxel central $I(2,2)$:**

**Correlación:**
$$I'(2,2) = 1a + 2b + 3c + 4d + 5e + 6f + 7g + 8h + 9i$$

**Convolución:**
$$I'(2,2) = 9a + 8b + 7c + 6d + 5e + 4f + 3g + 2h + 1i$$

O alternativamente (rotando el kernel):
$$I'(2,2) = 1i + 2h + 3g + 4f + 5e + 6d + 7c + 8b + 9a$$

#### **¿Cuándo son equivalentes?**

La correlación y la convolución son **equivalentes** si el kernel $k(i, j)$ es **simétrico**, es decir:

$$k(i, j) = k(-i, -j)$$

En este caso, rotar el kernel 180 grados produce el mismo kernel, por lo que:

$$I \star k = I * k$$

**Ejemplos de kernels simétricos:**
- Filtro Gaussiano
- Laplaciano de 4 u 8 puntos
- Filtro promedio

#### **¿Por qué se prefiere hablar de convolución?**

Aunque ambas operaciones son válidas, es más común hablar de **convolución** en procesamiento de imágenes por:

1. **Propiedades matemáticas útiles**:
   - Conmutatividad: $I * k = k * I$
   - Asociatividad: $(I * k_1) * k_2 = I * (k_1 * k_2)$
   - Distributividad: $I * (k_1 + k_2) = I * k_1 + I * k_2$

2. **Transformada de Fourier**:
   - La convolución en el dominio espacial se convierte en **multiplicación** en el dominio de frecuencia
   - Simplifica el análisis y procesamiento de señales

3. **Estándar en la teoría**:
   - Marco teórico más sólido para análisis de sistemas lineales
   - Facilita la comprensión y manipulación matemática

**Conclusión:** La diferencia principal es que la convolución implica una **rotación de 180 grados del kernel** respecto a la correlación. En la práctica, cuando los kernels son simétricos (como muchos filtros comunes), ambas operaciones producen el mismo resultado.

---

### 3. ¿Cuál es la principal diferencia entre el operador Laplaciano y otros operadores de detección de bordes como Prewitt y Sobel?

La principal diferencia radica en el **orden de la derivada** utilizada y en la **direccionalidad** de la detección de bordes:

#### **Orden de la derivada:**

**Operadores de Primera Derivada (Prewitt y Sobel):**
- Calculan la **primera derivada** de la imagen
- Aproximan el **gradiente** mediante diferencias finitas
- Producen dos componentes: $G_x$ (horizontal) y $G_y$ (vertical)

**Operador de Segunda Derivada (Laplaciano):**
- Calcula la **segunda derivada** de la imagen
- Utiliza la suma de las segundas derivadas parciales
- Produce un único resultado isotrópico

#### **Direccionalidad:**

**Prewitt y Sobel (Direccionales):**

- **Requieren dos kernels** para detectar bordes en diferentes direcciones:
  - Uno para dirección horizontal (detecta bordes verticales)
  - Otro para dirección vertical (detecta bordes horizontales)

**Kernels de Sobel:**

$$S_x = \begin{bmatrix} -1 & 0 & 1 \\ -2 & 0 & 2 \\ -1 & 0 & 1 \end{bmatrix} \quad S_y = \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{bmatrix}$$

**Kernels de Prewitt:**

$$P_x = \begin{bmatrix} -1 & 0 & 1 \\ -1 & 0 & 1 \\ -1 & 0 & 1 \end{bmatrix} \quad P_y = \begin{bmatrix} -1 & -1 & -1 \\ 0 & 0 & 0 \\ 1 & 1 & 1 \end{bmatrix}$$

- **Proporcionan información de magnitud Y dirección** del borde:

$$\text{Magnitud: } |G| = \sqrt{G_x^2 + G_y^2}$$

$$\text{Dirección: } \theta = \tan^{-1}\left(\frac{G_y}{G_x}\right)$$

**Laplaciano (Isotrópico):**

- **Usa un único kernel** para todas las direcciones
- **No distingue** la dirección del borde, solo su presencia
- Destaca bordes **hacia afuera o adentro**, dependiendo del signo

**Laplaciano de 4 puntos:**

$$\nabla^2 I(x,y)_4 = \begin{bmatrix} 0 & 1 & 0 \\ 1 & -4 & 1 \\ 0 & 1 & 0 \end{bmatrix}$$

**Laplaciano de 8 puntos:**

$$\nabla^2 I(x,y)_8 = \begin{bmatrix} 1 & 1 & 1 \\ 1 & -8 & 1 \\ 1 & 1 & 1 \end{bmatrix}$$

- **Es isotrópico**: Produce una magnitud del borde uniforme en todas las direcciones
- **Pierde información** sobre la orientación del borde

#### **Propiedades distintivas del Laplaciano:**

1. **Un solo kernel** vs. dos kernels (Prewitt/Sobel)
2. **Isotrópico** (responde igual en todas las direcciones)
3. **No proporciona** información de dirección del borde
4. **Mejor localización** de bordes en comparación con operadores de primer orden
5. **Muy sensible al ruido** (amplifica el ruido por ser segunda derivada)
6. **Requiere suavizado previo** (típicamente Gaussiano) para ser efectivo

#### **Sensibilidad al ruido:**

**Prewitt y Sobel:**
- Menos sensibles al ruido
- El peso central en Sobel (×2) reduce la sensibilidad al ruido
- Prewitt es más suave que Sobel al no ponderar el centro

**Laplaciano:**
- **Muy sensible al ruido** debido a la segunda derivada
- Se recomienda aplicar primero un filtro Gaussiano para suavizar
- Por esto surge el **LoG (Laplacian of Gaussian)** que combina ambos

#### **Definiciones matemáticas:**

**Gradiente (Primera derivada - Sobel/Prewitt):**

$$\nabla f = \begin{bmatrix} G_x \\ G_y \end{bmatrix} = \begin{bmatrix} \frac{\partial f}{\partial x} \\ \frac{\partial f}{\partial y} \end{bmatrix}$$

**Laplaciano (Segunda derivada):**

$$\nabla^2 I = \frac{\partial^2 I}{\partial x^2} + \frac{\partial^2 I}{\partial y^2}$$

Para el Laplaciano de 4 puntos discreto:

$$\nabla^2 I(x,y)_4 \approx I(x+1,y) + I(x-1,y) + I(x,y+1) + I(x,y-1) - 4I(x,y)$$

#### **Comparación visual de respuestas:**

$$
\begin{array}{|c|c|c|}
\hline
\text{Aspecto} & \text{Prewitt/Sobel} & \text{Laplaciano} \\
\hline
\textbf{Orden derivada} & \text{Primera} & \text{Segunda} \\
\textbf{Número de kernels} & 2 \text{ (horizontal y vertical)} & 1 \text{ (isotrópico)} \\
\textbf{Información de dirección} & \text{Sí (ángulo del gradiente)} & \text{No} \\
\textbf{Sensibilidad al ruido} & \text{Moderada} & \text{Alta} \\
\textbf{Localización de bordes} & \text{Buena} & \text{Excelente} \\
\textbf{Aplicación típica} & \text{Detección direccional} & \text{Detección omnidireccional} \\
\textbf{Preprocesamiento} & \text{Opcional} & \text{Necesario (Gaussiano)} \\
\hline
\end{array}
$$

#### **Ejemplo conceptual:**

**Transición de blanco a negro:**

- **Primera derivada (Prewitt/Sobel)**: Produce un **pico** en la ubicación del borde
- **Segunda derivada (Laplaciano)**: Produce un **cruce por cero** en la ubicación del borde

#### **Diferencia entre Prewitt y Sobel:**

Aunque ambos son de primera derivada, difieren en:

**Sobel:**
$$G_x \approx (c - a) + 2(f - d) + (i - g)$$
- Pondera **×2** la fila/columna central
- Más robusto ante ruido

**Prewitt:**
$$G_x \approx (c - a) + (f - d) + (i - g)$$
- Ponderación **uniforme** (×1)
- Más suave, menos sensible a cambios bruscos

#### **Conclusión:**

La **principal diferencia** es que:

1. **Laplaciano** es un operador de **segunda derivada** que usa **un único kernel isotrópico** y **no proporciona información direccional**, pero es **muy sensible al ruido**.

2. **Prewitt y Sobel** son operadores de **primera derivada** que usan **dos kernels direccionales**, proporcionan **información de magnitud y dirección** del borde, y son **menos sensibles al ruido**.

Esta diferencia fundamental hace que se elijan según la aplicación: Sobel/Prewitt cuando se necesita información direccional, y Laplaciano (típicamente como LoG) cuando se busca detección omnidireccional con buena localización.

---

### 4. ¿Qué es el filtro Laplacian of Gaussian (LoG) y para qué se utiliza?

El **Laplacian of Gaussian (LoG)**, o Laplaciano del Gaussiano, es un filtro compuesto que combina el suavizado del filtro Gaussiano con la detección de bordes del operador Laplaciano en una única operación.

#### **Definición:**

El LoG se obtiene aplicando el **operador Laplaciano** a un **filtro Gaussiano**, combinando matemáticamente ambas operaciones en un solo kernel.

#### **Proceso de dos etapas:**

**Etapa 1: Suavizado Gaussiano**
- Aplica un filtro Gaussiano a la imagen
- Suaviza la imagen eliminando detalles finos y ruido no deseado
- Produce una versión más homogénea, eliminando texturas y variaciones pequeñas

**Etapa 2: Aplicación del Laplaciano**
- Estima el Laplaciano sobre la imagen suavizada
- Resalta los cambios bruscos de intensidad
- Identifica los bordes de manera precisa

#### **Formulación matemática:**

**Gaussiano bidimensional:**

$$G(x, y) = \frac{1}{2\pi\sigma^2} e^{-\frac{x^2+y^2}{2\sigma^2}}$$

**Operador Laplaciano:**

$$\nabla^2 I = \frac{\partial^2 I}{\partial x^2} + \frac{\partial^2 I}{\partial y^2}$$

**Laplaciano del Gaussiano:**

$$\nabla^2 G(x, y) = \frac{\partial^2 G}{\partial x^2} + \frac{\partial^2 G}{\partial y^2}$$

Desarrollando las derivadas y simplificando, se llega a:

$$\nabla^2 G(x, y) = G(x, y) \cdot \frac{x^2 + y^2 - 2\sigma^2}{\sigma^4}$$

O de forma expandida:

$$\nabla^2 G(x, y) = \frac{1}{2\pi\sigma^2} e^{-\frac{x^2+y^2}{2\sigma^2}} \cdot \frac{x^2 + y^2 - 2\sigma^2}{\sigma^4}$$

#### **Normalización a cero:**

Para aplicaciones prácticas, es necesario **normalizar el kernel a 0** (como todo filtro pasa-alto). Esto se logra restando el promedio $k_l$ de todos los valores:

$$k_l = \frac{1}{nm} \sum_{x=1}^{n} \sum_{y=1}^{m} \nabla^2 G(x, y)$$

**LoG normalizado:**

$$\nabla^2_n G(x, y) = \nabla^2 G(x, y) - k_l$$

#### **Forma del kernel:**

El kernel LoG tiene forma de **"sombrero mexicano invertido"** (inverted Mexican hat):
- Valor negativo en el centro
- Valores positivos en un anillo alrededor
- Decrece hacia cero en la periferia

#### **¿Para qué se utiliza?**

**1. Detección de bordes:**
- Identifica bordes y contornos en imágenes
- Detecta transiciones rápidas de intensidad
- Proporciona **cruces por cero** en las ubicaciones de los bordes

**2. Detección de características:**
- Identifica regiones de interés
- Encuentra esquinas y puntos característicos
- Útil en visión por computadora

**3. Segmentación de imágenes:**
- Separa regiones con diferentes intensidades
- Delimita objetos en la escena

**4. Reducción de ruido con detección de bordes:**
- Suaviza el ruido antes de detectar bordes
- Evita falsos positivos causados por ruido

#### **Ventajas del LoG:**

**Combina dos operaciones en una**: Suavizado + Detección de bordes en una sola pasada

**Reduce la sensibilidad al ruido**: El suavizado Gaussiano previo atenúa el ruido antes de aplicar el Laplaciano

**Mejor detección de bordes reales**: Elimina bordes falsos causados por ruido

**Isotrópico**: Detecta bordes en todas las direcciones uniformemente

**Precisión en localización**: Proporciona buena localización de bordes mediante cruces por cero

**Escalabilidad**: El parámetro $\sigma$ permite ajustar la escala de detección

#### **Desventajas del LoG:**

**Computacionalmente costoso**: Requiere más cálculos que filtros simples

**Dependencia de parámetros**: Requiere ajuste cuidadoso de:
   - Tamaño de la ventana
   - Desviación estándar $\sigma$
   - Umbral de detección

**Detección excesiva**: Puede generar detección excesiva si se superponen bordes en diferentes escalas

**Sin información direccional**: Como el Laplaciano, no proporciona la orientación del borde

#### **Parámetros clave:**

**Desviación estándar $\sigma$:**
- Controla el grado de suavizado
- Valores pequeños: Detecta detalles finos, sensible a ruido
- Valores grandes: Detecta estructuras grandes, insensible a detalles

**Tamaño del kernel:**
- Se recomienda al menos **6 veces $\sigma$** como tamaño mínimo
- Ejemplo: Para $\sigma = 0.8$, usar kernel de al menos $6 \times 0.8 = 4.8 \approx 5 \times 5$

#### **Ejemplo de cálculo:**

Para un kernel LoG de $5 \times 5$ con $\sigma = 0.5$:

**Posiciones únicas calculadas** (por simetría):

$$
\begin{array}{|c|c|c|c|}
\hline
(x,y) & \text{Cálculo} & \nabla^2 G(x,y) & \nabla^2_n G(x,y) \\
\hline
(0,0) & \text{Centro} & -4.9496 & -4.9048 \\
(0,1) & \text{Adyacente} & 0.6696 & 0.7146 \\
(1,1) & \text{Diagonal} & 0.2712 & 0.3167 \\
(0,2) & \text{Borde} & 0.0112 & 0.0564 \\
(1,2) & \text{Esquina diagonal} & 0.0003 & 0.0468 \\
(2,2) & \text{Esquina} & 1.79 \times 10^{-7} & 0.0448 \\
\hline
\end{array}
$$

**Kernel LoG normalizado resultante:**

$$\nabla^2_n G = \begin{bmatrix}
0.0448 & 0.0468 & 0.0564 & 0.0468 & 0.0448 \\
0.0468 & 0.3167 & 0.7146 & 0.3167 & 0.0468 \\
0.0564 & 0.7146 & -4.9048 & 0.7146 & 0.0564 \\
0.0468 & 0.3167 & 0.7146 & 0.3167 & 0.0468 \\
0.0448 & 0.0468 & 0.0564 & 0.0468 & 0.0448
\end{bmatrix}$$

#### **Aplicación típica:**

1. Cargar imagen $I$
2. Generar kernel LoG con parámetros apropiados ($\sigma$, tamaño)
3. Aplicar convolución: $I' = I * \nabla^2_n G$
4. Buscar **cruces por cero** en $I'$ para localizar bordes exactos
5. Aplicar umbralización si es necesario

#### **Diferencia con aplicación secuencial:**

**Método secuencial (menos eficiente):**
```
1. I_suave = I * G(x,y)
2. I_bordes = Laplaciano(I_suave)
```

**Método LoG (más eficiente):**
```
1. kernel_LoG = Laplaciano(G(x,y))
2. I_bordes = I * kernel_LoG
```

#### **Conclusión:**

El **Laplacian of Gaussian (LoG)** es un filtro fundamental que:

1. **Combina** suavizado Gaussiano con detección Laplaciana
2. **Se utiliza para** detectar bordes y características de manera robusta ante ruido
3. **Ofrece** una detección isotrópica con buena localización
4. **Requiere** ajuste cuidadoso de parámetros para resultados óptimos
5. **Es ideal** cuando se necesita detección de bordes en todas las direcciones con reducción de ruido incorporada

Su principal aplicación es la **detección de bordes robusta** en imágenes con ruido, siendo especialmente útil en visión por computadora, análisis de imágenes médicas y cualquier aplicación donde se requiera identificar contornos precisos sin los artefactos introducidos por el ruido.

---

### 5. ¿Cuál es la ventaja de usar el filtro Gaussiano?

El **filtro Gaussiano** ofrece múltiples ventajas que lo convierten en uno de los filtros más utilizados en procesamiento de imágenes:

#### **1. Suavizado isotrópico**

**Ventaja principal:**
- Realiza un **suavizado uniforme en todas las direcciones**
- No introduce sesgos direccionales en la imagen filtrada
- Preserva mejor las características angulares de la imagen

**Contraste con otros filtros:**
- El filtro promedio puede introducir artefactos cuadrados
- El Gaussiano produce transiciones más naturales y suaves

#### **2. Eliminación efectiva de ruido**

**Tipos de ruido que maneja bien:**

**a) Ruido Gaussiano:**
- **Muy eficaz** para eliminar ruido que sigue una distribución normal
- El suavizado atenúa las variaciones aleatorias de intensidad

**b) Ruido de "sal y pimienta":**
- Aunque no es el mejor para este tipo (la mediana es superior)
- Puede reducir su impacto visual significativamente

**Comparación con filtro promedio:**
- El Gaussiano pondera más los píxeles cercanos al centro
- Reduce mejor el ruido sin perder tanto detalle

#### **3. Ponderación adaptativa según distancia**

**Característica clave:**
$$G(x, y) = \frac{1}{2\pi\sigma^2} e^{-\frac{x^2+y^2}{2\sigma^2}}$$

- **Píxeles cercanos al centro**: Mayor peso en el cálculo
- **Píxeles alejados del centro**: Menor peso en el cálculo

**Ventaja sobre filtro promedio:**

$$
\begin{array}{|c|c|c|}
\hline
\text{Aspecto} & \text{Filtro Promedio} & \text{Filtro Gaussiano} \\
\hline
\text{Ponderación} & \text{Uniforme (todos igual)} & \text{Decreciente con distancia} \\
\text{Resultado} & \text{Puede introducir artefactos} & \text{Transiciones más suaves} \\
\text{Preservación de bordes} & \text{Regular} & \text{Mejor} \\
\hline
\end{array}
$$

El filtro promedio considera **todos los píxeles igualmente**, lo que puede ser problemático cuando píxeles alejados tienen valores significativamente diferentes, introduciendo artefactos. El Gaussiano soluciona esto.

#### **4. Comportamiento óptimo en el dominio de frecuencia**

**Propiedad única:**
- La **Transformada de Fourier** de una Gaussiana es **otra Gaussiana**
- Atenúa gradualmente las altas frecuencias sin introducir artefactos indeseados
- No produce el efecto "ringing" (oscilaciones) común en otros filtros

**Ventaja práctica:**
- Suavizado en dominio espacial = Atenuación controlada en dominio frecuencial
- Permite diseño y análisis teórico más sencillo

#### **5. Control preciso mediante parámetro σ**

**Desviación estándar $\sigma$:**

$$\text{Mayor } \sigma \rightarrow \text{Más suavizado, menos detalles}$$
$$\text{Menor } \sigma \rightarrow \text{Menos suavizado, más detalles}$$

**Flexibilidad:**
- Ajuste fino del nivel de suavizado según necesidad
- Un solo parámetro controla el comportamiento completo
- Escalabilidad para diferentes niveles de detalle

**Recomendación:**
- Tamaño del kernel: al menos $6\sigma$ (asegura que el kernel cubra la mayor parte de la distribución)

#### **6. Modelo natural de desenfoque**

**Aplicación en óptica:**
- El desenfoque por plano de formación en cámaras sigue aproximadamente una distribución Gaussiana
- Modela mejor los fenómenos físicos reales que el promedio
- Útil en:
  - Simulación de desenfoque de profundidad de campo
  - Corrección de aberraciones ópticas
  - Efectos de bokeh en fotografía

#### **7. Base para operaciones avanzadas**

**El Gaussiano es fundamental para:**

**a) Laplacian of Gaussian (LoG):**
- Combina suavizado + detección de bordes
- Esencial en visión por computadora

**b) Difference of Gaussians (DoG):**
- Aproxima el operador Laplaciano
- Usado en SIFT y otros descriptores de características

**c) Pirámides Gaussianas:**
- Representación multi-escala de imágenes
- Base para análisis jerárquico

**d) Filtros derivativos:**
- Derivadas de Gaussianas para detección de bordes direccionales
- Más robustas ante ruido que derivadas simples

#### **8. Preservación de bordes importantes**

**Balance óptimo:**
- Suaviza el ruido en regiones homogéneas
- **Preserva mejor los bordes** significativos en comparación con el promedio
- La ponderación decreciente evita mezclar regiones muy diferentes

**Comparación visual:**
- **Filtro promedio**: Puede difuminar excesivamente los bordes
- **Filtro Gaussiano**: Mantiene transiciones más nítidas

#### **9. Separabilidad**

**Propiedad matemática:**

$$G(x, y) = G(x) \cdot G(y)$$

donde:

$$G(x) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{x^2}{2\sigma^2}}$$

$$G(y) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-\frac{y^2}{2\sigma^2}}$$

**Ventaja computacional:**
- Permite aplicar el filtro en **dos pasadas 1D** en lugar de una pasada 2D
- Reduce complejidad de $O(n^2)$ a $O(2n)$
- Mucho más eficiente para kernels grandes

**Ejemplo:**
- Kernel $31 \times 31$: 961 operaciones por píxel en 2D, vs 62 operaciones en dos pasadas 1D

#### **10. Fundamentación matemática sólida**

**Teorema del Límite Central:**
- El Gaussiano surge naturalmente en muchos procesos naturales
- Tiene propiedades matemáticas bien estudiadas
- Facilita el análisis teórico y la predicción de resultados

**Propiedades útiles:**
- Convolución de Gaussianas es otra Gaussiana
- Permite análisis multi-escala riguroso
- Base teórica sólida para algoritmos avanzados

#### **11. No introduce artefactos artificiales**

**Comparación con otros filtros:**

$$
\begin{array}{|c|c|}
\hline
\text{Filtro} & \text{Posibles Artefactos} \\
\hline
\textbf{Promedio} & \text{Bordes cuadrados, transiciones abruptas} \\
\textbf{Mediana} & \text{Pérdida de gradientes suaves} \\
\textbf{Bilateral} & \text{Efecto "cartoon" en exceso} \\
\textbf{Gaussiano} & \text{Mínimos, principalmente desenfoque controlado} \\
\hline
\end{array}
$$

#### **12. Normalización automática a 1**

**Propiedad importante:**

$$\sum_{x} \sum_{y} G(x, y) = 1$$

Cuando se normaliza correctamente:

$$G_u(x, y) = \frac{G(x, y)}{k_s}$$

donde:

$$k_s = \sum_{x=1}^{n} \sum_{y=1}^{m} G(x, y)$$

**Ventaja:**
- Preserva el brillo promedio de la imagen
- No oscurece ni aclara artificialmente
- Mantiene el rango dinámico original

#### **Ejemplo de cálculo del kernel Gaussiano:**

Para un kernel $5 \times 5$ con $\sigma = 0.5$:

$$G(x, y) = \frac{1}{2\pi(0.5)^2} e^{-\frac{x^2+y^2}{2(0.5)^2}}$$

**Kernel normalizado resultante:**

$$G_u = \begin{bmatrix}
0.0000 & 0.0000 & 0.0002 & 0.0000 & 0.0000 \\
0.0000 & 0.0113 & 0.0837 & 0.0113 & 0.0000 \\
0.0002 & 0.0837 & 0.6187 & 0.0837 & 0.0002 \\
0.0000 & 0.0113 & 0.0837 & 0.0113 & 0.0000 \\
0.0000 & 0.0000 & 0.0002 & 0.0000 & 0.0000
\end{bmatrix}$$

Note cómo el **píxel central tiene el mayor peso** (0.6187) y **decrece hacia la periferia**.

#### **Aplicaciones prácticas:**

1. **Preprocesamiento**: Antes de detección de bordes (LoG)
2. **Reducción de ruido**: En imágenes con ruido Gaussiano
3. **Suavizado selectivo**: Con $\sigma$ variable según región
4. **Simulación óptica**: Modelado de desenfoque de lente
5. **Compresión**: Reducción de detalles antes de comprimir
6. **Mejora visual**: Efecto "soft focus" en fotografía

#### **Limitaciones a considerar:**

**Pérdida de detalles finos**: $\sigma$ grande elimina texturas importantes

**Desenfoque de bordes**: Puede difuminar bordes si no se controla

**Costo computacional**: Kernels grandes requieren más procesamiento (mitigado por separabilidad)

**Selección de $\sigma$**: Requiere ajuste según aplicación específica

#### **Conclusión:**

La **ventaja principal** del filtro Gaussiano es su **suavizado isotrópico** que:

1. **No introduce sesgos direccionales** ni artefactos artificiales
2. **Pondera adaptativamente** según distancia al centro
3. **Es eficiente** computacionalmente (separable)
4. **Tiene comportamiento óptimo** en frecuencia
5. **Ofrece control preciso** mediante $\sigma$
6. **Modela procesos naturales** (óptica, ruido)
7. **Sirve de base** para operaciones avanzadas (LoG, DoG, pirámides)
8. **Preserva mejor los bordes** que el filtro promedio
9. **Tiene fundamentación matemática sólida**

Estas características lo convierten en el **filtro de suavizado por excelencia** en procesamiento de imágenes, siendo especialmente valioso cuando se requiere eliminar ruido sin introducir artefactos y manteniendo las propiedades estructurales importantes de la imagen.

---

## 9.8.2. Preguntas de análisis y comprensión con base en el texto

### 1. Analice cómo la elección del tamaño de la ventana de un filtro de suavizado afecta el resultado final en una imagen.

El tamaño de la ventana de un filtro de suavizado es un parámetro fundamental que determina el balance entre la reducción de ruido y la preservación de detalles en la imagen resultante. Este parámetro tiene múltiples efectos interrelacionados:

#### **Efectos del tamaño de ventana:**

**Ventanas pequeñas (3×3, 5×5):**

**Ventajas:**
- **Preservación de detalles**: Mantiene mejor las características finas y texturas de la imagen
- **Bordes más nítidos**: Los contornos y transiciones se mantienen más definidos
- **Procesamiento rápido**: Menor costo computacional
- **Menos difuminación**: La imagen resultante mantiene mayor claridad visual

**Desventajas:**
- **Reducción limitada de ruido**: Menos píxeles contribuyen al promedio, limitando la capacidad de suavizado
- **Efecto local**: Solo afecta un vecindario muy pequeño
- **Puede requerir múltiples pasadas**: Para lograr mayor suavizado

**Ventanas grandes (11×11, 15×15, 21×21):**

**Ventajas:**
- **Mayor reducción de ruido**: Más píxeles participan en el promedio, atenuando mejor las variaciones aleatorias
- **Suavizado más uniforme**: Produce regiones más homogéneas
- **Mejor para ruido intenso**: Efectivo cuando el nivel de ruido es alto

**Desventajas:**
- **Pérdida significativa de detalles**: Las texturas finas desaparecen
- **Bordes difuminados**: Los contornos pierden nitidez y se vuelven difusos
- **Costo computacional alto**: Más operaciones por píxel
- **Distorsión geométrica**: Puede alterar las formas y estructuras pequeñas

#### **Análisis cuantitativo:**

**Para un filtro promedio aritmético:**

$$I'(x, y) = \frac{1}{p \times q} \sum_{i=-m}^{m} \sum_{j=-n}^{n} I(x + i, y + j)$$

donde $p = 2m + 1$ y $q = 2n + 1$ son las dimensiones de la ventana.

**Impacto del tamaño:**

$$
\begin{array}{|c|c|c|c|}
\hline
\text{Tamaño ventana} & \text{Número de píxeles} & \text{Influencia por píxel} & \text{Efecto general} \\
\hline
3 \times 3 & 9 & 11.11\% & \text{Suavizado leve} \\
5 \times 5 & 25 & 4\% & \text{Suavizado moderado} \\
7 \times 7 & 49 & 2.04\% & \text{Suavizado notable} \\
11 \times 11 & 121 & 0.83\% & \text{Suavizado fuerte} \\
15 \times 15 & 225 & 0.44\% & \text{Suavizado muy fuerte} \\
\hline
\end{array}
$$

**Observación clave:** A medida que aumenta el tamaño de la ventana, la contribución individual de cada píxel disminuye, pero el número total de píxeles que influyen aumenta, resultando en un promedio más estable pero menos sensible a características locales.

#### **Efectos sobre diferentes componentes de la imagen:**

**1. Ruido de alta frecuencia:**
- **Ventana pequeña**: Reducción parcial, el ruido puede permanecer visible
- **Ventana grande**: Reducción significativa, el ruido se promedia efectivamente

**2. Bordes y contornos:**
- **Ventana pequeña**: Bordes permanecen relativamente nítidos
- **Ventana grande**: Bordes se difuminan, transiciones se vuelven graduales

**3. Texturas finas:**
- **Ventana pequeña**: Texturas preservadas en gran medida
- **Ventana grande**: Texturas eliminadas o severamente atenuadas

**4. Regiones homogéneas:**
- **Ventana pequeña**: Cambio mínimo
- **Ventana grande**: Mayor uniformidad, pequeñas variaciones eliminadas

#### **Consideraciones espaciales:**

**Soporte espacial del filtro:**

El tamaño de la ventana define el **soporte espacial** del filtro, es decir, la extensión de la región que contribuye al cálculo de cada píxel resultante.

**Relación con características de la imagen:**

- Si la ventana es **menor que el tamaño de las estructuras** importantes: Se preservan las características
- Si la ventana es **comparable al tamaño de las estructuras**: Se comienza a distorsionar
- Si la ventana es **mayor que el tamaño de las estructuras**: Las estructuras se pierden o difuminan severamente

#### **Ejemplo práctico:**

Considere una imagen con:
- Objetos pequeños de 5 píxeles de ancho
- Ruido con variaciones de 1-2 píxeles

**Con ventana 3×3:**
- Ruido: Parcialmente reducido
- Objetos: Bien preservados
- Resultado: Balance razonable

**Con ventana 7×7:**
- Ruido: Bien reducido
- Objetos: Comienzan a difuminarse (ventana comparable al tamaño)
- Resultado: Objetos pequeños pierden definición

**Con ventana 15×15:**
- Ruido: Eliminado completamente
- Objetos: Severamente difuminados o eliminados
- Resultado: Imagen muy suave pero sin detalles

#### **Para filtro Gaussiano:**

El impacto del tamaño de ventana se combina con el efecto de la desviación estándar $\sigma$:

$$G(x, y) = \frac{1}{2\pi\sigma^2} e^{-\frac{x^2+y^2}{2\sigma^2}}$$

**Recomendación práctica:**
- Tamaño de ventana: al menos $6\sigma$ (usualmente impar)
- Esto asegura que se capture la mayor parte de la distribución Gaussiana (≈99.7%)

**Relación tamaño-sigma:**

$$
\begin{array}{|c|c|c|}
\hline
\sigma & \text{Tamaño mínimo recomendado} & \text{Resultado} \\
\hline
0.5 & 3 \times 3 & \text{Suavizado muy leve} \\
1.0 & 7 \times 7 & \text{Suavizado moderado} \\
2.0 & 13 \times 13 & \text{Suavizado notable} \\
5.0 & 31 \times 31 & \text{Suavizado fuerte} \\
\hline
\end{array}
$$

#### **Efectos en bordes específicamente:**

**Transición de borde:**

Para un borde idealizado (función escalón), el efecto del tamaño de ventana es:

- **Ventana pequeña**: Transición en ~3 píxeles
- **Ventana mediana**: Transición en ~7 píxeles
- **Ventana grande**: Transición en ~15+ píxeles

**Pérdida de localización:**
- El centro del borde se mantiene, pero su nitidez disminuye
- La incertidumbre en la posición del borde aumenta con el tamaño de ventana

#### **Interacción con el tipo de filtro:**

**Filtro promedio aritmético:**
- Todos los píxeles en la ventana tienen igual peso
- Ventanas grandes pueden introducir artefactos de píxeles muy alejados

**Filtro Gaussiano:**
- Ponderación decreciente desde el centro
- Ventanas grandes son menos problemáticas debido a la ponderación
- Los píxeles lejanos tienen peso mínimo

**Filtro mediana:**
- Menos sensible al tamaño en términos de difuminación de bordes
- Ventanas grandes eliminan estructuras finas pero preservan bordes mejor que el promedio

#### **Criterios de selección del tamaño:**

**1. Según el nivel de ruido:**
- Ruido bajo: Ventanas pequeñas (3×3, 5×5)
- Ruido moderado: Ventanas medianas (7×7, 9×9)
- Ruido alto: Ventanas grandes (11×11, 15×15)

**2. Según la importancia de detalles:**
- Detalles críticos: Ventanas pequeñas
- Detalles menos importantes: Ventanas grandes

**3. Según el tiempo de procesamiento:**
- Tiempo limitado: Ventanas pequeñas
- Sin restricciones: Optimizar según calidad

**4. Según la aplicación:**
- Compresión: Ventanas moderadas a grandes (reducir información)
- Análisis: Ventanas pequeñas (preservar características)
- Visualización: Ventanas moderadas (balance)

#### **Estrategias adaptativas:**

**Filtrado con ventanas variables:**
1. Detectar regiones homogéneas vs. regiones con detalles
2. Aplicar ventanas grandes en regiones homogéneas
3. Aplicar ventanas pequeñas en regiones con estructuras

**Filtrado multi-escala:**
1. Aplicar múltiples ventanas de diferentes tamaños
2. Combinar resultados según criterios de calidad
3. Obtener mejor balance entre reducción de ruido y preservación de detalles

#### **Fenómeno de "sobre-suavizado":**

Cuando la ventana es excesivamente grande:
- La imagen pierde textura y profundidad
- Apariencia "plástica" o "artificial"
- Pérdida de información visual importante
- Degradación de calidad percibida

#### **Regla práctica general:**

**Tamaño óptimo de ventana:**

$$\text{Tamaño} \approx 2 \times \text{(escala del ruido)} + 1$$

Donde "escala del ruido" es el tamaño típico de las variaciones de ruido.

Para ruido con variaciones de 1-2 píxeles: ventana 3×3 o 5×5
Para ruido con variaciones de 3-4 píxeles: ventana 7×7 o 9×9

#### **Conclusión:**

La elección del tamaño de ventana en un filtro de suavizado es un **compromiso fundamental** entre:

**Por un lado:**
- Reducción efectiva de ruido
- Uniformidad en regiones homogéneas
- Estabilidad del resultado

**Por otro lado:**
- Preservación de detalles finos
- Nitidez de bordes
- Mantenimiento de estructuras pequeñas

**Principios guía:**

1. **Comenzar pequeño**: Usar ventanas pequeñas y aumentar si es necesario
2. **Considerar la aplicación**: Ajustar según si se prioriza reducción de ruido o preservación de detalles
3. **Evaluar visualmente**: El resultado final debe validarse visualmente
4. **Considerar alternativas**: Filtros adaptativos o métodos multi-escala pueden ofrecer mejores resultados
5. **Balancear costo-beneficio**: Ventanas más grandes requieren más procesamiento

El tamaño de ventana debe seleccionarse cuidadosamente considerando las características específicas de la imagen (nivel de ruido, escala de detalles importantes, tipo de estructuras) y los objetivos del procesamiento (reducción de ruido vs. preservación de información).

---

### 2. Compare las propiedades y aplicaciones de los filtros de Sobel y Prewitt en la detección de bordes.

Los operadores de **Sobel** y **Prewitt** son dos de los detectores de bordes más utilizados en procesamiento de imágenes. Ambos son operadores de **primera derivada** que calculan el **gradiente** de la imagen, pero difieren en sus ponderaciones y, por lo tanto, en sus propiedades y aplicaciones.

#### **Definiciones matemáticas:**

**Operador Sobel:**

$$S_x = \begin{bmatrix} -1 & 0 & 1 \\ -2 & 0 & 2 \\ -1 & 0 & 1 \end{bmatrix} \quad S_y = \begin{bmatrix} -1 & -2 & -1 \\ 0 & 0 & 0 \\ 1 & 2 & 1 \end{bmatrix}$$

**Deducción Sobel (dirección X):**

Para una ventana 3×3:

$$\begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix}$$

$$\frac{\partial f}{\partial x} \approx (c - a) + 2(f - d) + (i - g)$$

**Operador Prewitt:**

$$P_x = \begin{bmatrix} -1 & 0 & 1 \\ -1 & 0 & 1 \\ -1 & 0 & 1 \end{bmatrix} \quad P_y = \begin{bmatrix} -1 & -1 & -1 \\ 0 & 0 & 0 \\ 1 & 1 & 1 \end{bmatrix}$$

**Deducción Prewitt (dirección X):**

$$\frac{\partial f}{\partial x} \approx (c - a) + (f - d) + (i - g)$$

#### **Diferencia fundamental:**

**Sobel:**
- Pondera con **factor 2** la fila/columna central
- Mayor énfasis en los píxeles directamente adyacentes al centro

**Prewitt:**
- Ponderación **uniforme** (factor 1 para todos)
- Trata todos los píxeles de la ventana por igual

#### **Comparación de propiedades:**

$$
\begin{array}{|c|c|c|}
\hline
\textbf{Propiedad} & \textbf{Sobel} & \textbf{Prewitt} \\
\hline
\text{Ponderación central} & 2\times & 1\times \\
\hline
\text{Sensibilidad al ruido} & \text{Moderadamente baja} & \text{Más baja} \\
\hline
\text{Precisión en bordes nítidos} & \text{Alta} & \text{Moderada} \\
\hline
\text{Suavizado implícito} & \text{Moderado} & \text{Mayor} \\
\hline
\text{Respuesta a gradientes} & \text{Más fuerte} & \text{Más suave} \\
\hline
\text{Detección de bordes curvos} & \text{Buena} & \text{Regular} \\
\hline
\text{Complejidad computacional} & \text{Igual} & \text{Igual} \\
\hline
\text{Uso histórico} & \text{Muy común} & \text{Común} \\
\hline
\end{array}
$$

#### **Análisis detallado de propiedades:**

**1. Sensibilidad al ruido:**

**Sobel:**
- El factor 2 en el centro enfatiza los píxeles más cercanos
- Más sensible a cambios abruptos, incluyendo ruido
- Recomendado: aplicar suavizado Gaussiano previo en imágenes ruidosas

**Prewitt:**
- La ponderación uniforme actúa como un promedio implícito
- Más robusto ante ruido aleatorio
- Respuesta más "suave" a variaciones espurias

**Razón matemática:**

En Sobel, el píxel central contribuye con peso 2, mientras que en Prewitt todos contribuyen con peso 1. Esto hace que Prewitt sea efectivamente un promedio de tres diferencias, mientras que Sobel enfatiza la diferencia central.

**2. Precisión en localización de bordes:**

**Sobel:**
- Mejor localización de bordes nítidos
- El énfasis central (×2) refuerza la detección donde el cambio es más directo
- Produce respuestas más fuertes y definidas

**Prewitt:**
- Localización ligeramente menos precisa
- La ponderación uniforme "difumina" la respuesta
- Produce transiciones más graduales

**3. Respuesta a diferentes tipos de bordes:**

**Bordes nítidos (cambios abruptos):**
- **Sobel**: Respuesta fuerte y bien localizada
- **Prewitt**: Respuesta moderada, más distribuida

**Bordes graduales (rampas):**
- **Sobel**: Detecta bien, pero puede ser sensible a la pendiente
- **Prewitt**: Detecta de manera más uniforme

**Bordes curvos:**
- **Sobel**: Mejor seguimiento de curvaturas
- **Prewitt**: Puede "suavizar" las curvas excesivamente

**Bordes diagonales:**
- **Sobel**: Buena detección, el factor 2 ayuda en diagonales
- **Prewitt**: Detección aceptable pero menos enfática

#### **Cálculo del gradiente:**

Ambos operadores producen dos componentes de gradiente:

**Magnitud del gradiente:**

$$|G| = \sqrt{G_x^2 + G_y^2}$$

**Aproximación computacionalmente eficiente:**

$$|G| \approx |G_x| + |G_y|$$

o

$$|G| \approx \max(|G_x|, |G_y|)$$

**Dirección del gradiente:**

$$\theta = \tan^{-1}\left(\frac{G_y}{G_x}\right)$$

Esta información direccional es idéntica conceptualmente para ambos operadores, aunque las magnitudes difieren.

#### **Ejemplo comparativo numérico:**

Considere una imagen simple con un borde vertical:

$$I = \begin{bmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \end{bmatrix} \rightarrow \begin{bmatrix} 255 & 255 & 255 \\ 255 & 255 & 255 \\ 255 & 255 & 255 \end{bmatrix}$$

**Aplicando Sobel en X:**
$$G_x^{Sobel} = -1(0) + 0(0) + 1(255) + (-2)(0) + 0(0) + 2(255) + (-1)(0) + 0(0) + 1(255)$$
$$G_x^{Sobel} = 255 + 510 + 255 = 1020$$

**Aplicando Prewitt en X:**
$$G_x^{Prewitt} = -1(0) + 0(0) + 1(255) + (-1)(0) + 0(0) + 1(255) + (-1)(0) + 0(0) + 1(255)$$
$$G_x^{Prewitt} = 255 + 255 + 255 = 765$$

**Observación:**
- Sobel produce una respuesta **1.33 veces más fuerte** que Prewitt para el mismo borde
- Esta diferencia se debe al factor 2 en la fila central

#### **Aplicaciones específicas:**

**Sobel es preferible cuando:**

1. **Bordes nítidos son prioritarios**
   - Imágenes de documentos
   - Códigos de barras, códigos QR
   - Texto y caracteres

2. **Se necesita alta precisión de localización**
   - Mediciones dimensionales
   - Control de calidad industrial
   - Alineación de imágenes

3. **Detección de bordes fuertes**
   - Segmentación de objetos con contornos definidos
   - Extracción de contornos principales

4. **Imágenes con bajo ruido**
   - Imágenes sintéticas
   - Gráficos generados por computadora
   - Imágenes pre-procesadas

**Prewitt es preferible cuando:**

1. **La imagen tiene ruido significativo**
   - Imágenes médicas (ultrasonido, algunas modalidades de MRI)
   - Fotografías con grano
   - Imágenes de baja calidad

2. **Se requiere robustez sobre precisión**
   - Aplicaciones en tiempo real con condiciones variables
   - Procesamiento de video con calidad inconsistente

3. **Bordes graduales son importantes**
   - Imágenes con transiciones suaves
   - Fotografías naturales con iluminación gradual

4. **Se busca un efecto de suavizado implícito**
   - Cuando no se puede aplicar pre-filtrado
   - Procesamiento con recursos limitados

#### **Comparación en aplicaciones comunes:**

**1. Detección de bordes en fotografías:**
- **Sobel**: Produce bordes más definidos pero puede ser sensible a textura
- **Prewitt**: Produce bordes más suaves, menos afectado por textura fina

**2. Visión por computadora:**
- **Sobel**: Ampliamente usado por su balance entre precisión y robustez
- **Prewitt**: Usado cuando se prioriza estabilidad sobre precisión

**3. Procesamiento de imágenes médicas:**
- **Sobel**: Para imágenes de alta calidad (CT, MRI de alta resolución)
- **Prewitt**: Para imágenes más ruidosas (ultrasonido, rayos X)

**4. Segmentación de imágenes:**
- **Sobel**: Cuando los objetos tienen bordes bien definidos
- **Prewitt**: Cuando los límites son más difusos

**5. Reconocimiento de patrones:**
- **Sobel**: Mayor discriminación entre características
- **Prewitt**: Mayor tolerancia a variaciones

#### **Análisis espectral:**

Ambos operadores actúan como **filtros pasa-altos** en el dominio de frecuencia:

**Sobel:**
- Respuesta de frecuencia ligeramente más selectiva
- Atenúa más suavemente las bajas frecuencias

**Prewitt:**
- Respuesta de frecuencia más uniforme
- Transición más gradual entre frecuencias

#### **Extensiones y variaciones:**

**Operadores relacionados:**

**Sobel extendido:**
- Ventanas de 5×5, 7×7 con ponderaciones similares
- Mayor supresión de ruido, menor precisión

**Prewitt modificado:**
- Algunas implementaciones usan ponderaciones intermedias
- Balance entre Sobel y Prewitt estándar

**Scharr (mejora de Sobel):**
- Ponderaciones optimizadas para rotación isotrópica
- Mejor respuesta en todas las direcciones

$$\text{Scharr}_x = \begin{bmatrix} -3 & 0 & 3 \\ -10 & 0 & 10 \\ -3 & 0 & 3 \end{bmatrix}$$

#### **Limitaciones compartidas:**

Ambos operadores tienen limitaciones similares:

1. **Tamaño fijo 3×3**: No adaptativos a diferentes escalas de bordes
2. **Sensibilidad a orientación**: Mejor respuesta en direcciones cardinales
3. **Sin supresión de no-máximos**: Producen bordes gruesos sin post-procesamiento
4. **Requieren umbralización**: La salida necesita umbral para imagen binaria
5. **No detectan bordes cerrados**: Solo producen magnitud y dirección

#### **Combinación con otros métodos:**

**Pre-procesamiento recomendado:**
1. **Conversión a escala de grises** (si la imagen es color)
2. **Suavizado Gaussiano** (especialmente para Sobel en imágenes ruidosas)
3. **Normalización de contraste** (opcional)

**Post-procesamiento típico:**
1. **Cálculo de magnitud** del gradiente
2. **Supresión de no-máximos** (adelgazar bordes)
3. **Umbralización** (simple, histéresis, adaptativa)
4. **Morfología** (cierre de gaps, limpieza)

#### **Consideraciones prácticas de implementación:**

**Eficiencia computacional:**
- Ambos requieren 18 operaciones por píxel (9 multiplicaciones + 9 sumas)
- Pueden optimizarse aprovechando los ceros en los kernels
- Implementaciones paralelas (GPU) son eficientes para ambos

**Separabilidad:**
- Ninguno es perfectamente separable
- Aproximaciones separables existen pero pierden precisión

**Manejo de bordes de imagen:**
- Requieren padding (típicamente simétrico o replicado)
- La elección del padding afecta los bordes de la imagen resultante

#### **Recomendaciones de uso:**

**Usar Sobel cuando:**
- Precisión de localización es crítica
- Bordes son nítidos y bien definidos
- La imagen tiene bajo ruido o se puede pre-filtrar
- Se necesita mayor sensibilidad a cambios

**Usar Prewitt cuando:**
- La imagen tiene ruido moderado a alto
- No se puede aplicar pre-filtrado
- Se requiere mayor estabilidad
- Los bordes son graduales
- Se prioriza robustez sobre precisión

**Regla general:**
En aplicaciones modernas, **Sobel es más común** debido a:
- Mejor balance general entre propiedades
- Amplia disponibilidad en bibliotecas
- Mayor cantidad de investigación y optimizaciones
- Mejor documentación y ejemplos

Sin embargo, **Prewitt sigue siendo relevante** en:
- Aplicaciones legacy
- Situaciones con ruido significativo
- Contextos donde se ha optimizado específicamente

#### **Conclusión:**

Sobel y Prewitt son operadores muy similares con diferencias sutiles pero importantes:

**Sobel:**
- Ponderación central 2× → Mayor énfasis en cambios directos
- Mejor precisión de localización
- Más sensible al ruido
- Preferido en aplicaciones generales

**Prewitt:**
- Ponderación uniforme 1× → Efecto de promedio implícito
- Mayor robustez al ruido
- Menor precisión de localización
- Preferido cuando la estabilidad es crítica

La elección entre ambos debe basarse en las características específicas de la aplicación: tipo de imagen, nivel de ruido, importancia de la precisión vs. robustez, y recursos computacionales disponibles. En muchos casos, la diferencia en resultados es mínima, pero en aplicaciones críticas, la elección correcta puede mejorar significativamente el desempeño del sistema de procesamiento de imágenes.

---

### 3. Explique por qué es importante la normalización en el uso del kernel Gaussiano.

La normalización del kernel Gaussiano es un paso crítico que asegura que el filtrado preserve propiedades fundamentales de la imagen. Sin normalización adecuada, el filtro Gaussiano puede introducir artefactos indeseables y alterar características esenciales de la imagen original.

#### **Definición del kernel Gaussiano sin normalizar:**

$$G(x, y) = \frac{1}{2\pi\sigma^2} e^{-\frac{x^2+y^2}{2\sigma^2}}$$

#### **Problema sin normalización:**

Cuando se calcula discretamente un kernel Gaussiano en una ventana finita (por ejemplo, 5×5 o 7×7), la suma de todos los valores del kernel **no necesariamente es igual a 1**:

$$k_s = \sum_{x=1}^{n} \sum_{y=1}^{m} G(x, y) \neq 1$$

Esto ocurre porque:
1. El kernel se trunca a un tamaño finito
2. Los valores periféricos del Gaussiano continúan hacia infinito
3. La discretización introduce errores de aproximación

#### **Proceso de normalización:**

Para corregir esto, se normaliza el kernel dividiéndolo por la suma de sus elementos:

$$G_u(x, y) = \frac{G(x, y)}{k_s} = \frac{G(x, y)}{\sum_{x=1}^{n} \sum_{y=1}^{m} G(x, y)}$$

Después de esta normalización:

$$\sum_{x=1}^{n} \sum_{y=1}^{m} G_u(x, y) = 1$$

#### **Razones por las que es importante la normalización:**

**1. Preservación del brillo (intensidad promedio):**

**Sin normalización:**

Cuando se aplica el filtro Gaussiano mediante convolución:

$$I'(x, y) = \sum_{i} \sum_{j} I(x+i, y+j) \cdot G(i, j)$$

Si $\sum G(i, j) < 1$, entonces:
- La imagen resultante será **más oscura** que la original
- Se pierde información de intensidad
- El rango dinámico se reduce artificialmente

Si $\sum G(i, j) > 1$, entonces:
- La imagen resultante será **más brillante** que la original
- Puede causar saturación (valores > 255 en imágenes de 8 bits)
- Introduce distorsión en el histograma

**Con normalización:**

Al asegurar que $\sum G_u(i, j) = 1$:
- El brillo promedio se mantiene constante
- No hay ganancia ni pérdida de energía en la imagen
- El histograma se preserva (solo se suaviza su distribución)

**Ejemplo numérico:**

Para una región uniforme con $I(x,y) = 128$:

**Sin normalización** (supongamos $k_s = 0.8$):
$$I'(x,y) = 128 \times 0.8 = 102.4 \approx 102$$
Pérdida de 26 niveles de intensidad

**Con normalización** ($k_s = 1.0$):
$$I'(x,y) = 128 \times 1.0 = 128$$
Intensidad preservada

**2. Comportamiento como filtro pasa-bajo:**

**Propiedad de filtros pasa-bajo:**

Los filtros de suavizado deben actuar como filtros pasa-bajo, lo que significa:
- Atenuar altas frecuencias (detalles, ruido)
- Preservar bajas frecuencias (estructuras generales, brillo promedio)

**Condición matemática:**

Para que un filtro sea pasa-bajo puro, debe cumplir:

$$H(0, 0) = \sum_{i} \sum_{j} h(i, j) = 1$$

donde $H(0, 0)$ es la respuesta del filtro en frecuencia cero (componente DC).

**Sin normalización:**
- No se cumple la condición de pasa-bajo puro
- El filtro atenúa o amplifica todas las frecuencias, incluyendo DC
- Comportamiento impredecible

**Con normalización:**
- Se cumple $H(0, 0) = 1$
- Las bajas frecuencias se preservan correctamente
- Solo se atenúan las altas frecuencias como se desea

**3. Consistencia en diferentes tamaños de kernel:**

**Problema sin normalización:**

Considere kernels Gaussianos de diferentes tamaños con el mismo $\sigma$:

- Kernel 3×3: $k_s = 0.75$ (pierde parte de la cola de la distribución)
- Kernel 5×5: $k_s = 0.92$ (captura más de la distribución)
- Kernel 7×7: $k_s = 0.98$ (captura casi toda la distribución)

Aplicando estos kernels sin normalizar:
- Diferentes tamaños producen diferentes brillos
- No hay consistencia entre implementaciones
- Resultados impredecibles

**Con normalización:**

Todos los kernels, independientemente de su tamaño, producen:
- El mismo brillo promedio
- Resultados consistentes
- Comportamiento predecible

**4. Conformidad con la definición teórica:**

**Distribución Gaussiana continua:**

La integral de la distribución Gaussiana sobre todo el dominio es 1:

$$\int_{-\infty}^{\infty} \int_{-\infty}^{\infty} G(x, y) \, dx \, dy = 1$$

Esta es una propiedad fundamental de las funciones de densidad de probabilidad.

**Discretización finita:**

Al discretizar en una ventana finita, perdemos esta propiedad a menos que normalicemos:

$$\sum_{x=-m}^{m} \sum_{y=-n}^{n} G_u(x, y) = 1$$

La normalización **restaura la propiedad teórica** en el dominio discreto.

**5. Correcta implementación del suavizado:**

**Interpretación del suavizado:**

El filtro Gaussiano es esencialmente un **promedio ponderado** de los píxeles vecinos:

$$I'(x, y) = \sum_{i} \sum_{j} w(i, j) \cdot I(x+i, y+j)$$

donde $w(i, j) = G_u(i, j)$ son los pesos.

**Propiedad de pesos:**

Para que sea un promedio válido, los pesos deben sumar 1:

$$\sum_{i} \sum_{j} w(i, j) = 1$$

**Sin normalización:**
- No es un promedio válido
- Es una combinación lineal arbitraria
- Puede amplificar o atenuar la señal

**Con normalización:**
- Es un promedio ponderado correcto
- Cada píxel contribuye proporcionalmente
- El resultado es interpretable como un suavizado

**6. Evitar saturación y clipping:**

**Problema de saturación:**

En imágenes digitales (por ejemplo, 8 bits con rango [0, 255]):

**Sin normalización** ($k_s > 1$):
- Los valores pueden exceder 255
- Se requiere clipping: $I'(x,y) = \min(I'(x,y), 255)$
- Se pierden detalles en regiones brillantes
- Introduce distorsión no lineal

**Con normalización** ($k_s = 1$):
- Los valores permanecen en el rango válido
- No se requiere clipping (excepto por errores de redondeo mínimos)
- Se preserva la información en toda la imagen

**7. Reversibilidad aproximada:**

**Propiedad deseable:**

Aplicar múltiples suavizados Gaussianos debe ser equivalente a aplicar un solo Gaussiano con $\sigma$ más grande:

$$G_{\sigma_1} * G_{\sigma_2} = G_{\sqrt{\sigma_1^2 + \sigma_2^2}}$$

**Sin normalización:**
- Esta propiedad no se cumple
- Cada aplicación altera el brillo
- No se pueden componer filtros correctamente

**Con normalización:**
- La propiedad se mantiene (aproximadamente, dada la discretización)
- Se pueden componer filtros de manera predecible
- Facilita análisis multi-escala

**8. Análisis y depuración:**

**Importancia práctica:**

**Sin normalización:**
- Difícil diagnosticar problemas
- ¿El oscurecimiento es por el algoritmo o por falta de normalización?
- Resultados difíciles de interpretar

**Con normalización:**
- Comportamiento predecible y estándar
- Problemas son atribuibles a otras fuentes
- Facilita la comparación con literatura y otras implementaciones

#### **Ejemplo de cálculo de normalización:**

**Kernel Gaussiano 5×5 con $\sigma = 0.5$:**

**Paso 1: Calcular valores sin normalizar**

$$
\begin{array}{|c|c|}
\hline
\textbf{Posición} & G(x,y) \\
\hline
(0,0) & 0.6366 \\
\hline
(0,1) & 0.0862 \\
\hline
(1,1) & 0.0117 \\
\hline
(0,2) & 0.0023 \\
\hline
... & ... \\
\hline
\end{array}
$$

**Paso 2: Sumar todos los valores**

$$k_s = \sum_{x,y} G(x,y) = 1.0291$$

**Observación:** $k_s > 1$, por lo que sin normalización la imagen se volvería más brillante.

**Paso 3: Normalizar**

$$G_u(x,y) = \frac{G(x,y)}{1.0291}$$

**Kernel normalizado:**

$$G_u = \begin{bmatrix}
0.0000 & 0.0000 & 0.0002 & 0.0000 & 0.0000 \\
0.0000 & 0.0113 & 0.0837 & 0.0113 & 0.0000 \\
0.0002 & 0.0837 & 0.6187 & 0.0837 & 0.0002 \\
0.0000 & 0.0113 & 0.0837 & 0.0113 & 0.0000 \\
0.0000 & 0.0000 & 0.0002 & 0.0000 & 0.0000
\end{bmatrix}$$

**Verificación:**

$$\sum_{x,y} G_u(x,y) = 1.0000$$

#### **Casos especiales:**

**1. Kernels muy pequeños (3×3 con $\sigma$ grande):**

Cuando el kernel es muy pequeño comparado con $\sigma$:
- Se captura solo una pequeña fracción de la distribución
- $k_s$ puede ser mucho menor que 1
- La normalización es **crítica** para evitar oscurecimiento severo

**2. Kernels grandes (31×31 con $\sigma$ pequeño):**

Cuando el kernel es muy grande comparado con $\sigma$:
- Se captura casi toda la distribución
- $k_s \approx 1.0$
- La normalización tiene poco efecto, pero sigue siendo necesaria para exactitud

**3. Regla práctica:**

**Tamaño recomendado:** $\text{Tamaño} \geq 6\sigma$

Esto asegura que se capture aproximadamente el 99.7% de la distribución, haciendo que $k_s \approx 1.0$ incluso antes de normalizar, pero la normalización sigue siendo necesaria para precisión.

#### **Normalización en filtros Laplacianos:**

**Contraste con filtros pasa-alto:**

Para filtros pasa-alto como el Laplaciano, la normalización es **diferente**:

$$\sum_{i,j} \nabla^2 G(i,j) = 0$$

Debe sumar a **cero**, no a uno, porque:
- Son filtros de alta frecuencia
- Deben eliminar la componente DC
- Responden solo a cambios, no a niveles absolutos

**Normalización del LoG:**

Para el Laplacian of Gaussian, se normaliza restando el promedio:

$$\nabla^2_n G(x,y) = \nabla^2 G(x,y) - k_l$$

donde:

$$k_l = \frac{1}{nm} \sum_{x=1}^{n} \sum_{y=1}^{m} \nabla^2 G(x,y)$$

Esto asegura que $\sum \nabla^2_n G = 0$.

#### **Implementación práctica:**

**Código conceptual:**

```python
# Calcular kernel Gaussiano
G = gaussian_kernel(size, sigma)

# Calcular suma
k_s = np.sum(G)

# Normalizar
G_normalized = G / k_s

# Verificar
assert abs(np.sum(G_normalized) - 1.0) < 1e-6
```

**Consideraciones de punto flotante:**

- Usar precisión suficiente (float32 o float64)
- Verificar que la suma normalizada sea muy cercana a 1.0
- Pequeñas desviaciones (<1e-6) son aceptables debido a errores de redondeo

#### **Conclusión:**

La normalización del kernel Gaussiano es **esencial** porque:

1. **Preserva el brillo promedio** de la imagen
2. **Asegura comportamiento como filtro pasa-bajo** puro
3. **Garantiza consistencia** entre diferentes tamaños de kernel
4. **Mantiene conformidad** con la definición teórica
5. **Implementa correctamente** el concepto de promedio ponderado
6. **Evita saturación** y clipping de valores
7. **Permite composición** correcta de filtros
8. **Facilita análisis** y depuración

**Sin normalización:**
- La imagen puede oscurecerse o aclararse arbitrariamente
- Los resultados son impredecibles e inconsistentes
- Se violan propiedades fundamentales del filtrado
- Se introduce distorsión indeseada

**Con normalización:**
- El filtrado funciona como se espera teóricamente
- Los resultados son predecibles y reproducibles
- Se preservan las propiedades esenciales de la imagen
- El algoritmo es robusto y confiable

Por lo tanto, **siempre** debe normalizarse el kernel Gaussiano dividiendo por la suma de sus elementos, asegurando que la suma total sea exactamente 1.0. Este paso simple pero crucial garantiza que el filtro Gaussiano cumpla su propósito de suavizar la imagen sin introducir artefactos de brillo y manteniendo todas sus propiedades teóricas deseadas.

---

### 4. Evalúe los efectos de aplicar un filtro de mediana en una imagen con ruido de "sal y pimienta".

El filtro de **mediana** es particularmente efectivo para eliminar el ruido de "sal y pimienta", que consiste en píxeles aleatorios con valores extremos (blancos y negros). A continuación se evalúan detalladamente los efectos de este filtro sobre este tipo específico de ruido.

#### **Definición del filtro mediana:**

$$I'(x, y) = \text{median}\{I(x+i, y+j) \mid (i,j) \in R\}$$

donde $R$ es la región de la ventana de filtrado (típicamente $3 \times 3$, $5 \times 5$, etc.).

**Proceso:**
1. Ordenar los valores de intensidad dentro de la ventana
2. Seleccionar el valor central (mediana)
3. Asignar este valor al píxel central

#### **Características del ruido "sal y pimienta":**

**Definición:**
- **Sal** (pepper): Píxeles negros aleatorios con valor 0
- **Pimienta** (salt): Píxeles blancos aleatorios con valor 255
- **Distribución**: Aleatoria, afecta solo algunos píxeles
- **Naturaleza**: Impulsivo, valores extremos

**Parámetros:**
- **Densidad**: Porcentaje de píxeles afectados (típicamente 0.01 a 0.10)
- **Proporción**: Puede ser simétrico (50-50) o asimétrico

#### **Efectos del filtro mediana sobre ruido sal y pimienta:**

**1. Eliminación efectiva del ruido:**

**Razón fundamental:**

El filtro mediana es **robusto ante valores atípicos** (outliers):
- El valor mediano no se ve afectado por valores extremos aislados
- Si la mayoría de píxeles en la ventana no son ruido, la mediana será un valor válido
- Los píxeles de ruido (0 o 255) se ignoran efectivamente al seleccionar la mediana

**Ejemplo numérico:**

Ventana $3 \times 3$ con un píxel de ruido sal (255):

$$\begin{bmatrix} 100 & 102 & 98 \\ 101 & 255 & 99 \\ 103 & 97 & 100 \end{bmatrix}$$

**Proceso:**
- Valores ordenados: [97, 98, 99, 100, 100, 101, 102, 103, 255]
- Mediana (posición 5): **100**
- Resultado: El píxel ruidoso (255) se reemplaza por 100

**Contraste con filtro promedio:**

El promedio sería:
$$\frac{100+102+98+101+255+99+103+97+100}{9} = \frac{1055}{9} \approx 117$$

El valor 117 está distorsionado por el píxel ruidoso (255), mientras que la mediana (100) es representativa de los píxeles reales.

**2. Preservación de bordes:**

**Ventaja clave:**

A diferencia de los filtros de promedio, el filtro mediana **preserva mejor los bordes** porque:
- Selecciona un valor que realmente existe en la vecindad
- No crea valores intermedios artificiales
- Mantiene transiciones abruptas cuando la mayoría de píxeles pertenecen a un lado del borde

**Ejemplo en un borde:**

Ventana $3 \times 3$ sobre un borde vertical (con ruido):

$$\begin{bmatrix} 0 & 0 & 250 \\ 0 & 255 & 250 \\ 5 & 0 & 255 \end{bmatrix}$$

Donde 255 en el centro es ruido sal.

**Proceso:**
- Valores ordenados: [0, 0, 0, 0, 5, 250, 250, 255, 255]
- Mediana: **5**
- Resultado: Borde preservado (valor bajo, consistente con el lado izquierdo)

**Filtro promedio:** Promedio ≈ 106, difuminaría el borde

**3. No introduce valores nuevos (ventanas impares):**

**Propiedad importante:**

Para ventanas de tamaño impar, el filtro mediana **siempre selecciona un valor que existe** en la imagen original:
- No crea intensidades artificiales
- Mantiene el rango de valores original
- Preserva la paleta de colores (en imágenes de color)

**Ventanas pares:**

Si la ventana tiene un número par de elementos (poco común), la mediana se define como:

$$\text{median} = \frac{T_{K-1} + T_K}{2}$$

En este caso, puede crear valores intermedios.

**4. Dependencia del tamaño de ventana:**

**Ventana pequeña (3×3):**

**Efectividad:**
- Elimina píxeles aislados de ruido sal y pimienta
- Preserva bien los detalles finos
- Requiere que la densidad de ruido sea baja (<30% aproximadamente)

**Limitación:**
- Si hay múltiples píxeles de ruido adyacentes en la ventana, puede fallar

**Ejemplo de fallo:**

$$\begin{bmatrix} 255 & 255 & 255 \\ 100 & 255 & 102 \\ 103 & 97 & 100 \end{bmatrix}$$

- Valores ordenados: [97, 100, 100, 102, 103, 255, 255, 255, 255]
- Mediana: **103**
- Resultado aceptable, pero si hay más ruido, la mediana podría ser 255

**Ventana grande (5×5, 7×7):**

**Efectividad:**
- Elimina clusters de ruido
- Más robusto ante densidades altas de ruido
- Puede manejar hasta ~40-50% de ruido en la ventana

**Limitación:**
- Mayor pérdida de detalles finos
- Puede suavizar texturas pequeñas
- Esquinas y puntos agudos pueden redondearse

**Criterio de selección:**

Para ruido sal y pimienta con densidad $d$:
- Si $d < 0.1$ (10%): Ventana $3 \times 3$ suficiente
- Si $0.1 \leq d < 0.3$: Ventana $5 \times 5$
- Si $d \geq 0.3$: Ventana $7 \times 7$ o filtro adaptativo

**5. Efecto sobre diferentes tipos de estructuras:**

**Regiones homogéneas:**

**Antes del filtrado:**
- Región uniforme con ruido sal y pimienta esparcido
- Apariencia granular

**Después del filtrado:**
- Región suave y uniforme
- Ruido completamente eliminado
- Intensidad consistente con el valor original de la región

**Bordes y contornos:**

**Ventaja:**
- Bordes nítidos se mantienen relativamente nítidos
- Mejor que promedio, pero puede haber alguna pérdida de nitidez

**Limitación:**
- Esquinas muy agudas pueden redondearse ligeramente
- Bordes de un píxel de ancho pueden ensancharse

**Texturas finas:**

**Efecto mixto:**
- Texturas más grandes que la ventana: Preservadas razonablemente
- Texturas comparables a la ventana: Parcialmente suavizadas
- Texturas más pequeñas que la ventana: Pueden perderse

**Detalles pequeños:**

**Riesgo:**
- Puntos pequeños legítimos pueden confundirse con ruido
- Líneas finas pueden romperse si tienen ancho de un píxel
- Se recomienda usar la ventana más pequeña posible

**6. Comparación con otros filtros:**

**vs. Filtro promedio:**

$$
\begin{array}{|c|c|c|}
\hline
\textbf{Aspecto} & \textbf{Mediana} & \textbf{Promedio} \\
\hline
\text{Eliminación de sal y pimienta} & \text{Excelente} & \text{Pobre} \\
\hline
\text{Preservación de bordes} & \text{Buena} & \text{Pobre} \\
\hline
\text{Suavizado general} & \text{Moderado} & \text{Bueno} \\
\hline
\text{Ruido Gaussiano} & \text{Regular} & \text{Bueno} \\
\hline
\text{Costo computacional} & \text{Mayor (ordenamiento)} & \text{Menor} \\
\hline
\end{array}
$$

**vs. Filtro mínimo:**

**Filtro mínimo:**
- Selecciona el valor más pequeño
- Elimina ruido sal (blanco) pero **enfatiza pimienta** (negro)
- No es adecuado para sal y pimienta mixto

**vs. Filtro máximo:**

**Filtro máximo:**
- Selecciona el valor más grande
- Elimina ruido pimienta (negro) pero **enfatiza sal** (blanco)
- No es adecuado para sal y pimienta mixto

**vs. Filtro Gaussiano:**

**Filtro Gaussiano:**
- Reduce ruido mediante promedio ponderado
- Los píxeles extremos (sal y pimienta) **distorsionan** el resultado
- Produce "manchas" donde había ruido
- No es efectivo para ruido impulsivo

**7. Efectos secundarios y limitaciones:**

**Pérdida de detalles en iteraciones múltiples:**

Si se aplica el filtro mediana repetidamente:
- Primera aplicación: Elimina la mayoría del ruido
- Aplicaciones adicionales: Comienzan a suavizar detalles legítimos
- Eventualmente: La imagen se vuelve "plástica" o "de dibujos animados"

**Ejemplo:**
- 1 pasada: Ruido eliminado, imagen clara
- 3 pasadas: Texturas suavizadas
- 5+ pasadas: Pérdida significativa de detalles, imagen muy simplificada

**Distorsión de gradientes suaves:**

En regiones con cambios graduales de intensidad:
- El filtro mediana puede crear **terrazas** (posterización)
- Los gradientes suaves se convierten en pasos discretos
- El efecto es más notable con ventanas grandes

**Costo computacional:**

**Complejidad:**
- Filtro promedio: O(n) operaciones por píxel
- Filtro mediana: O(n log n) por el ordenamiento

**Tiempo relativo:**
- Mediana es aproximadamente 3-5 veces más lento que promedio
- Existen algoritmos optimizados (mediana rápida) que mejoran esto

**8. Ventajas específicas para sal y pimienta:**

**Por qué es el mejor filtro para este ruido:**

1. **Naturaleza impulsiva**: El ruido sal y pimienta son outliers extremos, y la mediana es robusta ante outliers
2. **Distribución binaria**: Solo dos valores anómalos (0 y 255), fácilmente identificables al ordenar
3. **Afecta píxeles aislados**: La mayoría de píxeles en una ventana son válidos, permitiendo que la mediana sea representativa
4. **No correlacionado**: Los píxeles de ruido son independientes, no forman patrones

**Tasa de éxito:**

Para ruido sal y pimienta con densidad $d$ y ventana $n \times n$:

**Probabilidad de éxito** (mediana es un píxel válido):
- Depende de que más de la mitad de píxeles en la ventana no sean ruido
- Para $n = 3, d = 0.1$: ~99% de éxito
- Para $n = 3, d = 0.3$: ~90% de éxito
- Para $n = 5, d = 0.3$: ~99% de éxito

#### **Ejemplo completo de filtrado:**

**Imagen original (región $5 \times 5$):**

$$I = \begin{bmatrix}
100 & 102 & 0 & 101 & 103 \\
98 & 255 & 99 & 100 & 102 \\
101 & 99 & 100 & 255 & 98 \\
103 & 100 & 102 & 101 & 99 \\
0 & 101 & 98 & 103 & 255
\end{bmatrix}$$

Ruido sal y pimienta: valores 0 (sal) y 255 (pimienta)

**Aplicando filtro mediana $3 \times 3$ en píxel (2,2):**

Ventana:
$$\begin{bmatrix} 255 & 99 & 100 \\ 99 & 100 & 255 \\ 100 & 102 & 101 \end{bmatrix}$$

Valores ordenados: [99, 99, 100, 100, 101, 102, 255, 255]
Mediana: **100**

**Resultado después del filtrado completo:**

$$I' = \begin{bmatrix}
100 & 100 & 101 & 101 & 102 \\
100 & 99 & 100 & 100 & 100 \\
99 & 100 & 100 & 100 & 100 \\
100 & 100 & 100 & 101 & 101 \\
100 & 100 & 101 & 101 & 102
\end{bmatrix}$$

**Observaciones:**
- Todos los píxeles de ruido (0 y 255) han sido eliminados
- Los valores resultantes están en el rango normal (98-103)
- La imagen tiene apariencia uniforme y limpia

#### **Recomendaciones prácticas:**

**Para eliminar sal y pimienta:**

1. **Primera opción**: Filtro mediana con ventana $3 \times 3$
2. **Si persiste ruido**: Aumentar a $5 \times 5$
3. **Para ruido muy denso**: Considerar filtro mediana adaptativo
4. **Post-procesamiento**: Si se perdieron detalles, aplicar ligero sharpening

**Evitar:**
- Filtros de promedio para este tipo de ruido
- Ventanas excesivamente grandes (>7×7) excepto en casos extremos
- Múltiples aplicaciones innecesarias del filtro

**Variaciones avanzadas:**

**Filtro mediana adaptativo:**
- Ajusta el tamaño de ventana según la presencia local de ruido
- Mejor preservación de detalles
- Mayor complejidad computacional

**Filtro mediana pesado:**
- Asigna pesos a los píxeles antes de calcular la mediana
- Híbrido entre mediana y promedio ponderado

#### **Conclusión:**

El filtro de mediana es **altamente efectivo** para eliminar ruido de sal y pimienta debido a:

1. **Robustez ante outliers**: La mediana no se ve afectada por valores extremos aislados
2. **Preservación de bordes**: Mantiene transiciones abruptas mejor que filtros de promedio
3. **No crea valores artificiales**: Selecciona valores reales de la imagen
4. **Efectividad demostrada**: Tasa de éxito muy alta para densidades típicas de ruido (<30%)

**Efectos positivos:**
- Eliminación completa o casi completa del ruido sal y pimienta
- Mantenimiento de estructuras y bordes importantes
- Mejora significativa de la calidad visual
- No introduce artefactos nuevos

**Limitaciones:**
- Costo computacional mayor que filtros lineales
- Posible pérdida de detalles muy finos con ventanas grandes
- Puede suavizar texturas pequeñas
- Gradientes suaves pueden aparecer como terrazas

**Balance:**
Para ruido sal y pimienta, las ventajas del filtro mediana superan ampliamente sus limitaciones, convirtiéndolo en el **método de elección estándar** para este tipo específico de ruido. Es significativamente superior a filtros lineales como el promedio o Gaussiano, que fallan al intentar eliminar este ruido impulsivo.

---

### 5. Analice las diferencias entre el uso de un filtro Laplaciano de 4 puntos y uno de 8 puntos.

Los filtros Laplacianos de 4 y 8 puntos son operadores de **segunda derivada** utilizados para detectar bordes en imágenes. Ambos aproximan el operador Laplaciano continuo, pero difieren en cómo consideran los píxeles vecinos, lo que resulta en propiedades y aplicaciones distintas.

#### **Definiciones matemáticas:**

**Operador Laplaciano continuo:**

$$\nabla^2 I = \frac{\partial^2 I}{\partial x^2} + \frac{\partial^2 I}{\partial y^2}$$

**Laplaciano de 4 puntos:**

$$\nabla^2 I(x,y)_4 = \begin{bmatrix} 0 & 1 & 0 \\ 1 & -4 & 1 \\ 0 & 1 & 0 \end{bmatrix}$$

**Fórmula discreta:**
$$\nabla^2 I(x,y)_4 = I(x+1,y) + I(x-1,y) + I(x,y+1) + I(x,y-1) - 4I(x,y)$$

**Laplaciano de 8 puntos:**

$$\nabla^2 I(x,y)_8 = \begin{bmatrix} 1 & 1 & 1 \\ 1 & -8 & 1 \\ 1 & 1 & 1 \end{bmatrix}$$

**Fórmula discreta:**
$$\nabla^2 I(x,y)_8 = I(x+1,y) + I(x-1,y) + I(x,y+1) + I(x,y-1)$$
$$+ I(x+1,y+1) + I(x+1,y-1) + I(x-1,y+1) + I(x-1,y-1) - 8I(x,y)$$

#### **Diferencias estructurales:**

**1. Vecindad considerada:**

**Laplaciano de 4 puntos:**
- Considera solo los **4 vecinos directos** (arriba, abajo, izquierda, derecha)
- Vecindad de conectividad-4
- Distancia Manhattan = 1

**Laplaciano de 8 puntos:**
- Considera los **4 vecinos directos** + **4 vecinos diagonales**
- Vecindad de conectividad-8
- Incluye distancias Manhattan = 1 y distancia Euclidiana = √2

**Visualización:**

```
Laplaciano 4 puntos:       Laplaciano 8 puntos:
      [N]                       [NW] [N] [NE]
   [W][C][E]                    [W] [C] [E]
      [S]                       [SW] [S] [SE]
```

**2. Aproximación de derivadas:**

**Laplaciano de 4 puntos:**
- Aproxima **solo las segundas derivadas en x e y**
- No incluye derivadas mixtas

**Derivación:**

Segunda derivada en x:
$$\frac{\partial^2 I}{\partial x^2} \approx I(x+1,y) - 2I(x,y) + I(x-1,y)$$

Segunda derivada en y:
$$\frac{\partial^2 I}{\partial y^2} \approx I(x,y+1) - 2I(x,y) + I(x,y-1)$$

Suma:
$$\nabla^2 I_4 = [I(x+1,y) + I(x-1,y) + I(x,y+1) + I(x,y-1)] - 4I(x,y)$$

**Laplaciano de 8 puntos:**
- Aproxima **segundas derivadas en x e y** + **derivadas mixtas**
- Incluye información diagonal

**Derivación:**

Derivadas mixtas para diagonal $y = x$:
$$\frac{\partial^2 I}{\partial x \partial y}\bigg|_{y=x} \approx I(x+1,y+1) - 2I(x,y) + I(x-1,y-1)$$

Derivadas mixtas para diagonal $y = -x$:
$$\frac{\partial^2 I}{\partial x \partial y}\bigg|_{y=-x} \approx I(x+1,y-1) - 2I(x,y) + I(x-1,y+1)$$

Combinando todo:
$$\nabla^2 I_8 = \nabla^2 I_4 + \text{términos diagonales}$$

#### **Comparación de propiedades:**

$$
\begin{array}{|c|c|c|}
\hline
\textbf{Propiedad} & \textbf{Laplaciano 4 puntos} & \textbf{Laplaciano 8 puntos} \\
\hline
\text{Vecinos considerados} & \text{4 (cardinales)} & \text{8 (cardinales + diagonales)} \\
\hline
\text{Sensibilidad direccional} & \text{Alta (sesgo hacia ejes)} & \text{Baja (más isotrópico)} \\
\hline
\text{Detección de bordes diagonales} & \text{Débil} & \text{Fuerte} \\
\hline
\text{Respuesta a bordes horizontales/verticales} & \text{Fuerte} & \text{Muy fuerte} \\
\hline
\text{Isotrópía} & \text{Moderada} & \text{Mayor} \\
\hline
\text{Sensibilidad al ruido} & \text{Alta} & \text{Muy alta} \\
\hline
\text{Peso central} & -4 & -8 \\
\hline
\text{Suma del kernel} & 0 & 0 \\
\hline
\text{Complejidad computacional} & \text{5 operaciones} & \text{9 operaciones} \\
\hline
\end{array}
$$

#### **Análisis detallado de diferencias:**

**1. Sensibilidad direccional:**

**Laplaciano de 4 puntos:**
- **Sesgo hacia direcciones cardinales** (0°, 90°, 180°, 270°)
- Detecta mejor bordes horizontales y verticales
- Bordes diagonales (45°, 135°) se detectan más débilmente

**Laplaciano de 8 puntos:**
- **Más isotrópico** (responde similarmente en todas las direcciones)
- Detecta bordes diagonales con casi la misma intensidad que cardinales
- Mejor para detectar características en cualquier orientación

**Ejemplo:**

Para un borde diagonal perfecto (45°):

$$\begin{bmatrix} 0 & 0 & 255 \\ 0 & 255 & 255 \\ 255 & 255 & 255 \end{bmatrix}$$

**Lap4:**
$$= 0 + 255 + 255 + 255 - 4(255) = 765 - 1020 = -255$$

**Lap8:**
$$= 0 + 255 + 255 + 255 + 0 + 0 + 255 + 255 - 8(255)$$
$$= 1275 - 2040 = -765$$

**Observación:** Lap8 produce una respuesta más fuerte debido a la contribución diagonal.

**2. Respuesta isotrópica:**

**Isotrópía ideal:**
Un operador perfectamente isotrópico responde igual para un borde independientemente de su orientación.

**Laplaciano de 4 puntos:**
- **Anisótropo** (respuesta depende de la dirección)
- Respuesta máxima para bordes alineados con ejes
- Respuesta ~70% para bordes a 45°

**Laplaciano de 8 puntos:**
- **Más cercano a isotrópico**
- Respuesta más uniforme en todas las direcciones
- Mejor aproximación al Laplaciano continuo

**Razón matemática:**

El Laplaciano continuo es isotrópico por definición. El de 8 puntos aproxima mejor esta propiedad al incluir direcciones diagonales, que en el plano discreto están aproximadamente a la misma distancia (√2 ≈ 1.414) que las cardinales (1.0).

**3. Sensibilidad al ruido:**

**Laplaciano de 4 puntos:**
- **Alta sensibilidad al ruido**
- Solo 4 píxeles contribuyen, por lo que un píxel ruidoso tiene impacto significativo
- Peso central -4 amplifica diferencias

**Laplaciano de 8 puntos:**
- **Muy alta sensibilidad al ruido**
- 8 píxeles contribuyen, pero el peso central es -8
- Mayor amplificación de ruido
- **Más problemático en imágenes ruidosas**

**Comparación cuantitativa:**

Para ruido con varianza $\sigma^2$:
- Lap4 amplifica el ruido aproximadamente por un factor de $\sqrt{5} \approx 2.24$
- Lap8 amplifica el ruido aproximadamente por un factor de $\sqrt{9} = 3.0$

**Conclusión:** Lap8 es **más sensible al ruido** que Lap4.

**Recomendación:**
Ambos requieren **suavizado previo** (típicamente Gaussiano), pero Lap8 requiere más suavizado o un $\sigma$ mayor.

**4. Detección de diferentes tipos de bordes:**

**Bordes horizontales/verticales:**

Ejemplo: Borde horizontal

$$\begin{bmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 255 & 255 & 255 \end{bmatrix}$$

**Lap4:**
$$= 0 + 255 + 0 + 0 - 0 = 255$$
(Respuesta fuerte)

**Lap8:**
$$= 0 + 255 + 0 + 0 + 0 + 255 + 0 + 255 - 0 = 765$$
(Respuesta muy fuerte)

**Bordes diagonales:**

Ejemplo: Borde diagonal 45°

$$\begin{bmatrix} 0 & 0 & 255 \\ 0 & 255 & 255 \\ 255 & 255 & 255 \end{bmatrix}$$

**Lap4:**
Respuesta moderada (no captura bien la diagonal)

**Lap8:**
Respuesta fuerte (captura bien la diagonal por los términos diagonales)

**5. Localización de bordes:**

**Laplaciano de 4 puntos:**
- Localización precisa en direcciones cardinales
- Puede dar múltiples respuestas en bordes curvos
- Bordes diagonales pueden aparecer "dentados"

**Laplaciano de 8 puntos:**
- Localización más uniforme en todas las direcciones
- Bordes curvos mejor definidos
- Transiciones diagonales más suaves

**6. Detección de esquinas y puntos:**

**Esquinas:**

Las esquinas son características con alta curvatura en múltiples direcciones.

**Laplaciano de 4 puntos:**
- Detecta esquinas alineadas con ejes de manera excelente
- Esquinas rotadas se detectan más débilmente

**Laplaciano de 8 puntos:**
- Detecta esquinas en cualquier orientación
- Respuesta más fuerte y uniforme
- Mejor para detectores de características rotacionalmente invariantes

**7. Aplicaciones específicas:**

**Laplaciano de 4 puntos es preferible cuando:**

1. **Bordes principalmente horizontales/verticales**
   - Documentos escaneados
   - Arquitectura con líneas rectas
   - Imágenes de circuitos impresos

2. **Se requiere menor sensibilidad al ruido**
   - Menor amplificación comparada con Lap8

3. **Recursos computacionales limitados**
   - Menos operaciones por píxel

4. **Compatibilidad con procesamiento morfológico**
   - Conectividad-4 se alinea con operaciones morfológicas estándar

**Laplaciano de 8 puntos es preferible cuando:**

1. **Bordes en todas las direcciones**
   - Imágenes naturales con objetos variados
   - Escenas con elementos diagonales

2. **Se requiere isotrópía**
   - Análisis de texturas
   - Detección de características rotacionalmente invariantes

3. **Detección de puntos y esquinas**
   - Características SIFT, SURF
   - Seguimiento de puntos

4. **Imágenes con bordes curvos**
   - Objetos orgánicos
   - Formas no rectangulares

#### **Variante con factor de ajuste α:**

Ambos pueden modularse con un factor $\alpha$:

**Laplaciano de 4 puntos con α:**

$$\nabla^2 I(x,y)_4 = \frac{1}{4(\alpha + 1)} \begin{bmatrix} \frac{\alpha}{4} & \frac{1-\alpha}{4} & \frac{\alpha}{4} \\ \frac{1-\alpha}{4} & -1 & \frac{1-\alpha}{4} \\ \frac{\alpha}{4} & \frac{1-\alpha}{4} & \frac{\alpha}{4} \end{bmatrix}$$

Donde $\alpha$ controla la sensibilidad:
- $\alpha = 0$: Laplaciano estándar de 4 puntos
- $\alpha > 0$: Incluye contribución diagonal, acercándose al comportamiento de 8 puntos
- En MATLAB, el valor predeterminado es $\alpha = 0.2$

#### **Combinación con filtro Gaussiano (LoG):**

Ambos se suelen combinar con Gaussiano:

**LoG-4:**
$$\nabla^2[G(x,y) * I(x,y)]$$ usando Laplaciano de 4 puntos

**LoG-8:**
$$\nabla^2[G(x,y) * I(x,y)]$$ usando Laplaciano de 8 puntos

**Diferencia:**
- LoG-8 detecta bordes en más direcciones
- LoG-4 es computacionalmente más eficiente
- Ambos requieren normalización a cero

#### **Ejemplo práctico completo:**

**Imagen de entrada (región $5 \times 5$):**

$$I = \begin{bmatrix}
100 & 100 & 100 & 100 & 100 \\
100 & 100 & 100 & 100 & 100 \\
100 & 100 & 200 & 200 & 200 \\
100 & 100 & 200 & 200 & 200 \\
100 & 100 & 200 & 200 & 200
\end{bmatrix}$$

Borde horizontal en fila 3.

**Aplicando Lap4 en píxel (3,3):**

Ventana:
$$\begin{bmatrix} 100 & 100 & 100 \\ 100 & 200 & 200 \\ 100 & 200 & 200 \end{bmatrix}$$

$$\text{Lap4} = 100 + 100 + 200 + 200 - 4(200) = 600 - 800 = -200$$

**Aplicando Lap8 en píxel (3,3):**

$$\text{Lap8} = 100 + 100 + 200 + 200 + 100 + 100 + 100 + 200 - 8(200)$$
$$= 1100 - 1600 = -500$$

**Observación:**
- Ambos detectan el borde (valores negativos)
- Lap8 produce respuesta más fuerte (-500 vs -200)
- Lap8 sería más visible en la imagen de bordes resultante

#### **Recomendaciones prácticas:**

**Criterios de selección:**

1. **Naturaleza de la imagen:**
   - Estructuras rectilíneas → Lap4
   - Formas variadas → Lap8

2. **Nivel de ruido:**
   - Ruido moderado → Lap4 (con Gaussiano previo)
   - Ruido bajo → Lap8
   - Ruido alto → Considerar otros métodos (Sobel, Canny)

3. **Orientación de bordes:**
   - Principalmente H/V → Lap4
   - Todas las direcciones → Lap8

4. **Requisitos de isotrópía:**
   - No crítico → Lap4
   - Crítico → Lap8

**Post-procesamiento:**

Ambos benefician de:
1. **Suavizado Gaussiano previo** (crítico)
2. **Detección de cruces por cero** para localizar bordes
3. **Umbralización** para eliminar respuestas débiles
4. **Adelgazamiento** para obtener bordes de un píxel

#### **Conclusión:**

**Laplaciano de 4 puntos:**
- Más simple, menos costoso computacionalmente
- Sesgo hacia direcciones cardinales
- Menor sensibilidad al ruido (comparativamente)
- Adecuado para estructuras rectilíneas

**Laplaciano de 8 puntos:**
- Más completo, captura todas las direcciones
- Más isotrópico, mejor aproximación al operador continuo
- Mayor sensibilidad al ruido
- Adecuado para formas variadas y detección omnidireccional

**Elección:**
La decisión entre Lap4 y Lap8 depende del balance entre:
- **Isotrópía deseada** vs. **sensibilidad al ruido**
- **Costo computacional** vs. **completitud direccional**
- **Características de la imagen** vs. **requisitos de la aplicación**

En la práctica moderna, con preprocesamiento adecuado (Gaussiano) y poder computacional suficiente, **Lap8 suele preferirse** por su mejor comportamiento isotrópico. Sin embargo, **Lap4 sigue siendo válido** y a menudo suficiente para aplicaciones donde los bordes están predominantemente alineados con los ejes o donde se requiere mayor eficiencia.

---

## 9.8.3. Ejercicios numéricos

### 1. Calcule la correlación de una imagen I de 3×3 con un kernel k de 3×3 en el punto I(1,1) = a utilizando padding "Simétrico"

**Enunciado:**

Calcule la correlación de una imagen $I$ de $3 \times 3$ con un kernel $k$ de $3 \times 3$ con los valores mostrados en el punto $I(1,1) = a$ utilizando padding "Simétrico".

**Datos:**

$$I = \begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}$$

$$k = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}$$

**Solución:**

#### **Paso 1: Entender el padding simétrico**

El **padding simétrico** (Symmetric Padding) replica los valores de la imagen reflejándolos como en un espejo en los bordes. Esto mantiene la continuidad y simetría de la imagen.

Para calcular la correlación en el punto $I(1,1) = a$, necesitamos una ventana $3 \times 3$ centrada en ese píxel. Como $I(1,1)$ está en la esquina superior izquierda, necesitamos agregar padding simétrico.

#### **Paso 2: Construcción de la imagen con padding simétrico**

Para aplicar padding simétrico a una imagen $3 \times 3$:

$$I = \begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}$$

La imagen extendida con padding simétrico (1 píxel en cada dirección):

$$I_{\text{padded}} = \begin{bmatrix}
e & d & d & e & f \\
b & a & b & c & c \\
b & a & b & c & c \\
e & d & e & f & f \\
h & g & h & i & i
\end{bmatrix}$$

**Explicación del padding simétrico:**
- **Esquina superior izquierda**: Se refleja desde $e$ (el píxel más cercano al centro)
- **Borde superior**: Se refleja la fila superior ($a, b, c$) desde la fila inmediatamente inferior ($d, e, f$)
- **Borde izquierdo**: Se refleja la columna izquierda desde la columna inmediatamente a la derecha
- Las reflexiones siguen el patrón de espejo

Sin embargo, para simplificar y seguir el patrón estándar del libro, la imagen con padding simétrico es:

$$I_{\text{padded}} = \begin{bmatrix}
e & d & e & f & e \\
b & a & b & c & b \\
e & d & e & f & e \\
h & g & h & i & h \\
e & d & e & f & e
\end{bmatrix}$$

#### **Paso 3: Ventana 3×3 centrada en I(1,1) = a**

Para calcular la correlación en la posición original $I(1,1) = a$, que ahora está en la posición $(2,2)$ de la imagen con padding, extraemos la ventana $3 \times 3$:

$$W = \begin{bmatrix}
e & d & e \\
b & a & b \\
e & d & e
\end{bmatrix}$$

#### **Paso 4: Fórmula de correlación**

La correlación se calcula como:

$$I'(x,y) = \sum_{i=-1}^{1} \sum_{j=-1}^{1} W(x+i, y+j) \cdot k(i,j)$$

O de manera más explícita:

$$I'(1,1) = \sum \sum W \odot k$$

Donde $\odot$ representa la multiplicación elemento a elemento seguida de la suma.

#### **Paso 5: Cálculo paso a paso**

Multiplicamos cada elemento de la ventana $W$ con el kernel $k$ y sumamos:

$$\begin{bmatrix}
e & d & e \\
b & a & b \\
e & d & e
\end{bmatrix} \odot \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}$$

**Cálculo detallado:**

$$\begin{align}
I'(1,1) &= e \cdot 1 + d \cdot 2 + e \cdot 3 + \\
&\quad b \cdot 4 + a \cdot 5 + b \cdot 6 + \\
&\quad e \cdot 7 + d \cdot 8 + e \cdot 9
\end{align}$$

**Agrupando términos:**

$$\begin{align}
I'(1,1) &= 1e + 2d + 3e + 4b + 5a + 6b + 7e + 8d + 9e \\
&= 5a + (4+6)b + (2+8)d + (1+3+7+9)e \\
&= 5a + 10b + 10d + 20e
\end{align}$$

#### **Paso 6: Resultado**

$$\boxed{I'(1,1) = 5a + 10b + 10d + 20e}$$

#### **Paso 7: Verificación con ejemplo numérico**

Supongamos valores específicos:

$$I = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}$$

Entonces: $a=1, b=2, c=3, d=4, e=5, f=6, g=7, h=8, i=9$

$$I'(1,1) = 5(1) + 10(2) + 10(4) + 20(5) = 5 + 20 + 40 + 100 = 165$$

**Verificación directa:**

Ventana:
$$W = \begin{bmatrix}
5 & 4 & 5 \\
2 & 1 & 2 \\
5 & 4 & 5
\end{bmatrix}$$

Multiplicación:
$$I' = 5(1) + 4(2) + 5(3) + 2(4) + 1(5) + 2(6) + 5(7) + 4(8) + 5(9)$$
$$I' = 5 + 8 + 15 + 8 + 5 + 12 + 35 + 32 + 45 = 165$$ ✓

#### **Paso 8: Interpretación**

El resultado $I'(1,1) = 5a + 10b + 10d + 20e$ muestra que:

1. **El píxel central $a$** tiene peso 5 (del kernel)
2. **Los píxeles adyacentes $b$ y $d$** tienen peso combinado de 10 cada uno
3. **El píxel $e$ (diagonal)** tiene el mayor peso (20) debido a la simetría del padding y los valores del kernel

Esto ilustra cómo el padding simétrico afecta el resultado de la correlación en los bordes, dando mayor importancia a los píxeles cercanos al centro de la imagen original.

---

### 2. Determine el valor filtrado de un píxel I(2,2) y I(3,3) utilizando filtro de mediana con un kernel k de 3×3. Use padding "Symmetric"

**Enunciado:**

Determine el valor filtrado de un píxel $I(2,2)$ y $I(3,3)$ utilizando un filtro de mediana con un kernel $k$ de $3 \times 3$. Use padding de "Symmetric".

**Datos:**

$$I = \begin{bmatrix}
4 & 2 & 5 \\
6 & 3 & 4 \\
3 & 7 & 4
\end{bmatrix}$$

**Solución:**

#### **Paso 1: Entender el filtro de mediana**

El **filtro de mediana** reemplaza cada píxel por la **mediana** de los valores en su vecindad. La mediana es el valor central cuando los elementos se ordenan de menor a mayor.

**Fórmula:**

$$I'(x,y) = \text{mediana}\{I(x+i, y+j) \mid (i,j) \in R\}$$

Donde $R$ es la región de la ventana (típicamente $3 \times 3$).

#### **Paso 2: Construcción de la imagen con padding simétrico**

Imagen original:

$$I = \begin{bmatrix}
4 & 2 & 5 \\
6 & 3 & 4 \\
3 & 7 & 4
\end{bmatrix}$$

Con padding simétrico (1 píxel en cada dirección):

$$I_{\text{padded}} = \begin{bmatrix}
3 & 6 & 3 & 4 & 3 \\
2 & 4 & 2 & 5 & 2 \\
3 & 6 & 3 & 4 & 3 \\
7 & 3 & 7 & 4 & 7 \\
3 & 6 & 3 & 4 & 3
\end{bmatrix}$$

**Nota:** En la imagen con padding, las posiciones originales se desplazan:
- $I(1,1) = 4$ está ahora en posición $(2,2)$
- $I(2,2) = 3$ está ahora en posición $(3,3)$
- $I(3,3) = 4$ está ahora en posición $(4,4)$

#### **Parte A: Calcular I'(2,2) - píxel central original**

**Paso 3: Extraer ventana 3×3 centrada en I(2,2)**

En la imagen original, $I(2,2) = 3$ está en el centro. Su ventana $3 \times 3$ es:

$$W_{(2,2)} = \begin{bmatrix}
4 & 2 & 5 \\
6 & 3 & 4 \\
3 & 7 & 4
\end{bmatrix}$$

Esta ventana incluye todos los píxeles de la imagen original (no necesita padding para el centro).

**Paso 4: Ordenar los valores**

Valores en la ventana: $\{4, 2, 5, 6, 3, 4, 3, 7, 4\}$

Ordenados de menor a mayor: $\{2, 3, 3, 4, 4, 4, 5, 6, 7\}$

**Paso 5: Encontrar la mediana**

Para 9 valores, la mediana es el **valor en la posición 5** (el del medio):

$$\text{Posiciones: } \underbrace{2, 3, 3, 4}_{4 \text{ menores}}, \boxed{4}_{\text{mediana}}, \underbrace{4, 5, 6, 7}_{4 \text{ mayores}}$$

**Resultado:**

$$\boxed{I'(2,2) = 4}$$

#### **Parte B: Calcular I'(3,3) - esquina inferior derecha**

**Paso 6: Extraer ventana 3×3 centrada en I(3,3) con padding**

En la imagen original, $I(3,3) = 4$ está en la esquina inferior derecha. En la imagen con padding (posición $(4,4)$), su ventana $3 \times 3$ es:

Extraemos desde la posición $(3,3)$ hasta $(5,5)$ en $I_{\text{padded}}$:

$$W_{(3,3)} = \begin{bmatrix}
3 & 4 & 3 \\
7 & 4 & 7 \\
3 & 4 & 3
\end{bmatrix}$$

**Verificación de las posiciones:**
- Esquina superior izquierda de la ventana: $I_{\text{padded}}(3,3) = 3$ (valor original $I(2,2)$)
- Centro de la ventana: $I_{\text{padded}}(4,4) = 4$ (valor original $I(3,3)$)
- Por simetría, los bordes se reflejan

**Paso 7: Ordenar los valores**

Valores en la ventana: $\{3, 4, 3, 7, 4, 7, 3, 4, 3\}$

Ordenados de menor a mayor: $\{3, 3, 3, 3, 4, 4, 4, 7, 7\}$

**Paso 8: Encontrar la mediana**

Para 9 valores, la mediana es el **valor en la posición 5**:

$$\text{Posiciones: } \underbrace{3, 3, 3, 3}_{4 \text{ menores}}, \boxed{4}_{\text{mediana}}, \underbrace{4, 4, 7, 7}_{4 \text{ mayores}}$$

**Resultado:**

$$\boxed{I'(3,3) = 4}$$

#### **Paso 9: Tabla resumen de resultados**

$$
\begin{array}{|c|c|c|c|c|c|}
\hline
\textbf{Posición} & \textbf{Valor original} & \textbf{Ventana } 3\times3 & \textbf{Valores ordenados} & \textbf{Mediana} & \textbf{Valor filtrado} \\
\hline
I(2,2) & 3 & \{4,2,5,6,3,4,3,7,4\} & \{2,3,3,4,4,4,5,6,7\} & 4 & 4 \\
\hline
I(3,3) & 4 & \{3,4,3,7,4,7,3,4,3\} & \{3,3,3,3,4,4,4,7,7\} & 4 & 4 \\
\hline
\end{array}
$$

#### **Paso 10: Interpretación**

**Para I(2,2):**
- Valor original: 3
- Valor filtrado: 4
- El filtro aumentó el valor en 1, acercándolo al valor más común (moda = 4)

**Para I(3,3):**
- Valor original: 4
- Valor filtrado: 4
- El filtro mantuvo el valor, indicando que 4 es un valor robusto en su vecindad

**Efecto del filtro de mediana:**
- **Elimina valores atípicos** sin crear nuevos valores
- **Preserva bordes** mejor que filtros de promedio
- **Reduce ruido impulsivo** (sal y pimienta) efectivamente

El filtro de mediana es especialmente útil porque:
1. No crea valores nuevos que no existan en la imagen
2. Es robusto ante valores extremos
3. Preserva mejor las transiciones bruscas (bordes)

---

### 3. Calcule la convolución para el punto I(1,1) = a de una imagen I de 3×3 con un kernel k de 3×3 utilizando padding "Circular"

**Enunciado:**

Calcule la convolución para el punto $I(1,1) = a$ de una imagen $I$ de $3 \times 3$ con un kernel $k$ de $3 \times 3$ utilizando padding "Circular".

**Datos:**

$$I = \begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}$$

$$k = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}$$

**Solución:**

#### **Paso 1: Entender el padding circular**

El **padding circular** (Circular Padding o Wrap) trata la imagen como si fuera periódica, conectando los bordes opuestos. Es como si la imagen estuviera enrollada en un toro (dona).

**Principio:**
- El borde derecho se conecta con el borde izquierdo
- El borde inferior se conecta con el borde superior
- Las esquinas se conectan diagonalmente

#### **Paso 2: Construcción de la imagen con padding circular**

Imagen original:

$$I = \begin{bmatrix}
a & b & c \\
d & e & f \\
g & h & i
\end{bmatrix}$$

Con padding circular (1 píxel en cada dirección):

$$I_{\text{padded}} = \begin{bmatrix}
i & g & h & i & g \\
c & a & b & c & a \\
f & d & e & f & d \\
i & g & h & i & g \\
c & a & b & c & a
\end{bmatrix}$$

**Explicación del patrón:**
- **Esquina superior izquierda** (posición (1,1)): Es $i$ (esquina opuesta de la imagen)
- **Borde superior**: Son los valores del borde inferior ($g, h, i$)
- **Borde izquierdo**: Son los valores del borde derecho ($c, f, i$)
- Los valores se "envuelven" (wrap around) desde los lados opuestos

#### **Paso 3: Fórmula de convolución**

La convolución se define como:

$$I'(x,y) = \sum_{i=-1}^{1} \sum_{j=-1}^{1} I(x-i, y-j) \cdot k(i,j)$$

Para el punto $I(1,1) = a$ (que está en posición $(2,2)$ en la imagen con padding):

$$I'(1,1) = \sum_{i=-1}^{1} \sum_{j=-1}^{1} I(1-i, 1-j) \cdot k(i,j)$$

#### **Paso 4: Ventana 3×3 centrada en I(1,1) = a**

En la imagen con padding, extraemos la ventana centrada en la posición $(2,2)$ que corresponde a $a$:

$$W = \begin{bmatrix}
i & g & h \\
c & a & b \\
f & d & e
\end{bmatrix}$$

**Verificación de las posiciones usando índices circulares:**
- Posición $(-1,-1)$ relativa a $(1,1)$: $I(0,0) \rightarrow I(3,3) = i$ (circular)
- Posición $(-1,0)$ relativa a $(1,1)$: $I(0,1) \rightarrow I(3,1) = g$ (circular)
- Posición $(-1,1)$ relativa a $(1,1)$: $I(0,2) \rightarrow I(3,2) = h$ (circular)
- Y así sucesivamente...

#### **Paso 5: Cálculo de la convolución**

En convolución, usamos el kernel **invertido** (rotado 180°) o cambiamos los índices. Aquí aplicamos la definición estándar:

$$\begin{align}
I'(1,1) &= I(1-(-1), 1-(-1)) \cdot k(-1,-1) + I(1-(-1), 1-0) \cdot k(-1,0) + \cdots \\
&= I(2,2) \cdot k(-1,-1) + I(2,1) \cdot k(-1,0) + I(2,0) \cdot k(-1,1) + \cdots
\end{align}$$

Usando el padding circular para evaluar:

$$\begin{align}
I'(1,1) &= i \cdot k(-1,-1) + g \cdot k(-1,0) + h \cdot k(-1,1) + \\
&\quad c \cdot k(0,-1) + a \cdot k(0,0) + b \cdot k(0,1) + \\
&\quad f \cdot k(1,-1) + d \cdot k(1,0) + e \cdot k(1,1)
\end{align}$$

**Nota:** En convolución, el kernel se aplica rotado 180°:

$$k_{\text{rotado}} = \begin{bmatrix}
9 & 8 & 7 \\
6 & 5 & 4 \\
3 & 2 & 1
\end{bmatrix}$$

Entonces, el cálculo correcto es:

$$\begin{align}
I'(1,1) &= i \cdot 9 + g \cdot 8 + h \cdot 7 + \\
&\quad c \cdot 6 + a \cdot 5 + b \cdot 4 + \\
&\quad f \cdot 3 + d \cdot 2 + e \cdot 1
\end{align}$$

#### **Paso 6: Simplificación**

$$\begin{align}
I'(1,1) &= 9i + 8g + 7h + 6c + 5a + 4b + 3f + 2d + 1e \\
&= 5a + 4b + 6c + 2d + 1e + 3f + 8g + 7h + 9i
\end{align}$$

**Ordenando alfabéticamente:**

$$\boxed{I'(1,1) = 5a + 4b + 6c + 2d + e + 3f + 8g + 7h + 9i}$$

#### **Paso 7: Verificación con ejemplo numérico**

Supongamos:

$$I = \begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}$$

Donde: $a=1, b=2, c=3, d=4, e=5, f=6, g=7, h=8, i=9$

$$\begin{align}
I'(1,1) &= 5(1) + 4(2) + 6(3) + 2(4) + 1(5) + 3(6) + 8(7) + 7(8) + 9(9) \\
&= 5 + 8 + 18 + 8 + 5 + 18 + 56 + 56 + 81 \\
&= 255
\end{align}$$

**Verificación directa con ventana circular:**

Ventana con padding circular para $I(1,1)$:
$$W = \begin{bmatrix}
9 & 7 & 8 \\
3 & 1 & 2 \\
6 & 4 & 5
\end{bmatrix}$$

Kernel rotado 180°:
$$k_{\text{rot}} = \begin{bmatrix}
9 & 8 & 7 \\
6 & 5 & 4 \\
3 & 2 & 1
\end{bmatrix}$$

Multiplicación:
$$I' = 9(9) + 7(8) + 8(7) + 3(6) + 1(5) + 2(4) + 6(3) + 4(2) + 5(1)$$
$$I' = 81 + 56 + 56 + 18 + 5 + 8 + 18 + 8 + 5 = 255$$ ✓

#### **Paso 8: Comparación con otros tipos de padding**

Para el mismo punto $I(1,1)$ con diferentes paddings:

$$
\begin{array}{|c|c|c|}
\hline
\textbf{Tipo de Padding} & \textbf{Ventana } 3\times3 & \textbf{Características} \\
\hline
\text{Circular} & \text{Usa } \{i,g,h,c,a,b,f,d,e\} & \text{Conecta bordes opuestos} \\
\hline
\text{Simétrico} & \text{Usa } \{e,d,e,b,a,b,e,d,e\} & \text{Refleja como espejo} \\
\hline
\text{Replicado} & \text{Usa } \{a,a,a,a,a,b,a,d,e\} & \text{Repite bordes} \\
\hline
\text{Cero} & \text{Usa } \{0,0,0,0,a,b,0,d,e\} & \text{Rellena con ceros} \\
\hline
\end{array}
$$

#### **Paso 9: Interpretación**

El resultado $I'(1,1) = 5a + 4b + 6c + 2d + e + 3f + 8g + 7h + 9i$ muestra que:

1. **Los píxeles de la esquina opuesta** ($i=9$, $g=8$, $h=7$) tienen los **pesos más altos** debido al padding circular
2. **El píxel central** $a$ tiene peso 5 (del kernel)
3. **El píxel más distante** $e$ tiene el peso más bajo (1)

**Ventajas del padding circular:**
- Útil para **imágenes periódicas** o **texturas repetitivas**
- **Mantiene la continuidad** en los bordes
- Apropiado para **señales cíclicas**

**Desventajas:**
- Puede introducir **artefactos** en imágenes no periódicas
- No es natural para la mayoría de fotografías

---

### 4. Utilice el operador Laplaciano de 4 puntos para detectar bordes en una imagen I de 3×3 utilizando padding "Replicate"

**Enunciado:**

Utilice el operador Laplaciano de 4 puntos para detectar bordes en una imagen $I$ de $3 \times 3$ utilizando padding "Replicate".

**Datos:**

$$I = \begin{bmatrix}
1 & 4 & 5 \\
6 & 5 & 4 \\
3 & 7 & 2
\end{bmatrix}$$

**Solución:**

#### **Paso 1: Operador Laplaciano de 4 puntos**

El Laplaciano de 4 puntos es un operador de segunda derivada que detecta cambios bruscos de intensidad. Su kernel es:

$$\nabla^2 I_4 = \begin{bmatrix}
0 & 1 & 0 \\
1 & -4 & 1 \\
0 & 1 & 0
\end{bmatrix}$$

**Fórmula directa:**

$$\nabla^2 I(x,y)_4 = I(x,y+1) + I(x,y-1) + I(x+1,y) + I(x-1,y) - 4I(x,y)$$

O en términos de direcciones:

$$\nabla^2 I(x,y)_4 = I_{\text{arriba}} + I_{\text{abajo}} + I_{\text{izquierda}} + I_{\text{derecha}} - 4I_{\text{centro}}$$

#### **Paso 2: Entender el padding "Replicate"**

El **padding replicado** (Replicate Padding) repite el valor del borde más cercano:

Imagen original:
$$I = \begin{bmatrix}
1 & 4 & 5 \\
6 & 5 & 4 \\
3 & 7 & 2
\end{bmatrix}$$

Con padding replicado (1 píxel):

$$I_{\text{padded}} = \begin{bmatrix}
1 & 1 & 4 & 5 & 5 \\
1 & 1 & 4 & 5 & 5 \\
6 & 6 & 5 & 4 & 4 \\
3 & 3 & 7 & 2 & 2 \\
3 & 3 & 7 & 2 & 2
\end{bmatrix}$$

**Explicación:**
- **Esquina superior izquierda**: Se replica el valor $I(1,1) = 1$
- **Borde superior**: Se replica la fila superior
- **Borde izquierdo**: Se replica la columna izquierda
- **Bordes opuestos**: Similar

#### **Paso 3: Calcular el Laplaciano para todos los píxeles**

Aplicaremos el operador Laplaciano a cada píxel de la imagen original.

**Para I(1,1) = 1 (esquina superior izquierda):**

Ventana con padding:
$$W_{(1,1)} = \begin{bmatrix}
1 & 1 & 4 \\
1 & 1 & 4 \\
6 & 6 & 5
\end{bmatrix}$$

Usando la fórmula (en imagen con padding, posición (2,2)):
$$\nabla^2 I(1,1) = I(1,2) + I(3,2) + I(2,1) + I(2,3) - 4I(2,2)$$
$$\nabla^2 I(1,1) = 1 + 6 + 1 + 4 - 4(1) = 12 - 4 = 8$$

**Para I(1,2) = 4 (borde superior centro):**

Vecinos: arriba=4 (replicado), abajo=5, izquierda=1, derecha=5
$$\nabla^2 I(1,2) = 4 + 5 + 1 + 5 - 4(4) = 15 - 16 = -1$$

**Para I(1,3) = 5 (esquina superior derecha):**

Vecinos: arriba=5 (replicado), abajo=4, izquierda=4, derecha=5 (replicado)
$$\nabla^2 I(1,3) = 5 + 4 + 4 + 5 - 4(5) = 18 - 20 = -2$$

**Para I(2,1) = 6 (borde izquierdo centro):**

Vecinos: arriba=1, abajo=3, izquierda=6 (replicado), derecha=5
$$\nabla^2 I(2,1) = 1 + 3 + 6 + 5 - 4(6) = 15 - 24 = -9$$

**Para I(2,2) = 5 (centro - sin padding necesario):**

Vecinos: arriba=4, abajo=7, izquierda=6, derecha=4
$$\nabla^2 I(2,2) = 4 + 7 + 6 + 4 - 4(5) = 21 - 20 = 1$$

**Para I(2,3) = 4 (borde derecho centro):**

Vecinos: arriba=5, abajo=2, izquierda=5, derecha=4 (replicado)
$$\nabla^2 I(2,3) = 5 + 2 + 5 + 4 - 4(4) = 16 - 16 = 0$$

**Para I(3,1) = 3 (esquina inferior izquierda):**

Vecinos: arriba=6, abajo=3 (replicado), izquierda=3 (replicado), derecha=7
$$\nabla^2 I(3,1) = 6 + 3 + 3 + 7 - 4(3) = 19 - 12 = 7$$

**Para I(3,2) = 7 (borde inferior centro):**

Vecinos: arriba=5, abajo=7 (replicado), izquierda=3, derecha=2
$$\nabla^2 I(3,2) = 5 + 7 + 3 + 2 - 4(7) = 17 - 28 = -11$$

**Para I(3,3) = 2 (esquina inferior derecha):**

Vecinos: arriba=4, abajo=2 (replicado), izquierda=7, derecha=2 (replicado)
$$\nabla^2 I(3,3) = 4 + 2 + 7 + 2 - 4(2) = 15 - 8 = 7$$

#### **Paso 4: Imagen resultante del Laplaciano**

$$I'_{\text{Laplaciano}} = \begin{bmatrix}
8 & -1 & -2 \\
-9 & 1 & 0 \\
7 & -11 & 7
\end{bmatrix}$$

#### **Paso 5: Tabla resumen de cálculos**

$$
\begin{array}{|c|c|c|c|c|c|}
\hline
\textbf{Posición} & \textbf{Valor original} & \textbf{Vecinos } (\uparrow,\downarrow,\leftarrow,\rightarrow) & \textbf{Suma vecinos} & 4\times\text{ centro} & \textbf{Laplaciano} \\
\hline
(1,1) & 1 & 1,6,1,4 & 12 & 4 & 8 \\
\hline
(1,2) & 4 & 4,5,1,5 & 15 & 16 & -1 \\
\hline
(1,3) & 5 & 5,4,4,5 & 18 & 20 & -2 \\
\hline
(2,1) & 6 & 1,3,6,5 & 15 & 24 & -9 \\
\hline
(2,2) & 5 & 4,7,6,4 & 21 & 20 & 1 \\
\hline
(2,3) & 4 & 5,2,5,4 & 16 & 16 & 0 \\
\hline
(3,1) & 3 & 6,3,3,7 & 19 & 12 & 7 \\
\hline
(3,2) & 7 & 5,7,3,2 & 17 & 28 & -11 \\
\hline
(3,3) & 2 & 4,2,7,2 & 15 & 8 & 7 \\
\hline
\end{array}
$$

#### **Paso 6: Interpretación de los valores**

**Valores positivos (indican bordes o puntos brillantes):**
- $I'(1,1) = 8$: **Borde fuerte** - esquina con transición significativa
- $I'(2,2) = 1$: Transición leve
- $I'(3,1) = 7$: **Borde moderado**
- $I'(3,3) = 7$: **Borde moderado**

**Valores negativos (indican valles o puntos oscuros):**
- $I'(2,1) = -9$: **Valle moderado** - píxel brillante rodeado de valores menores
- $I'(3,2) = -11$: **Valle fuerte** - píxel muy brillante (7) en contraste con vecinos

**Valor cero:**
- $I'(2,3) = 0$: **Sin cambio** - región homogénea

#### **Paso 7: Detección de bordes por umbralización**

Para detectar bordes, aplicamos un **umbral** a los valores absolutos:

**Opción 1: Umbral absoluto**

$$\text{Borde} = \begin{cases}
1 & \text{si } |\nabla^2 I(x,y)| > \text{umbral} \\
0 & \text{en otro caso}
\end{cases}$$

Con umbral = 5:

$$I'_{\text{bordes}} = \begin{bmatrix}
1 & 0 & 0 \\
1 & 0 & 0 \\
1 & 1 & 1
\end{bmatrix}$$

**Opción 2: Escalar a rango [0, 255]**

Para visualización, escalamos:

$$I'_{\text{scaled}} = \frac{|\nabla^2 I| - \min}{\ max - \min} \times 255$$

Con $\min = -11$ y $\max = 8$:

$$I'_{\text{scaled}} = \begin{bmatrix}
255 & 134 & 121 \\
34 & 161 & 148 \\
242 & 0 & 242
\end{bmatrix}$$

#### **Paso 8: Análisis de bordes detectados**

**Bordes fuertes detectados:**

1. **Posición (1,1)**: Transición de 1 → 4,6 (cambio de 3-5 unidades)
2. **Posición (3,2)**: Píxel 7 rodeado de valores menores (valle pronunciado)
3. **Posición (2,1)**: Píxel 6 con transiciones significativas

**Regiones homogéneas:**

- **Posición (2,3)**: Valor 4 bien equilibrado con vecinos

#### **Paso 9: Efecto del padding "Replicate"**

El padding replicado afecta los resultados en los bordes:

**Ventajas:**
- No introduce valores artificiales (0 o valores extraños)
- Mantiene los valores dentro del rango de la imagen
- Apropiado cuando los bordes de la imagen son representativos

**Desventajas:**
- Puede subestimar cambios reales en los bordes de la imagen
- Los píxeles de borde tienen vecinos duplicados, reduciendo la magnitud del Laplaciano

#### **Paso 10: Resultado final**

$$\boxed{
I'_{\text{Laplaciano}} = \begin{bmatrix}
8 & -1 & -2 \\
-9 & 1 & 0 \\
7 & -11 & 7
\end{bmatrix}
}$$

**Bordes principales detectados:**
- **Esquina superior izquierda** (1,1): Valor 8
- **Borde izquierdo** (2,1): Valor -9
- **Borde inferior** (3,1), (3,2), (3,3): Valores 7, -11, 7

El operador Laplaciano ha detectado exitosamente las regiones con cambios bruscos de intensidad, siendo especialmente sensible al píxel (3,2) = 7 que es significativamente más brillante que sus vecinos.

---

### 5. Calcule el Kernel Laplaciano de la Gaussiana (LoG) para una desviación estándar σ = 0.8

**Enunciado:**

Calcule el Kernel Laplaciano de la Gaussiana (LoG) para una desviación estándar $\sigma = 0.8$.

**Solución:**

#### **Paso 1: Fórmulas fundamentales**

**Gaussiano bidimensional:**

$$G(x,y) = \frac{1}{2\pi\sigma^2} e^{-\frac{x^2+y^2}{2\sigma^2}}$$

**Laplaciano:**

$$\nabla^2 I = \frac{\partial^2 I}{\partial x^2} + \frac{\partial^2 I}{\partial y^2}$$

**Laplaciano del Gaussiano (LoG):**

$$\nabla^2 G(x,y) = \frac{\partial^2 G}{\partial x^2} + \frac{\partial^2 G}{\partial y^2}$$

Que se simplifica a:

$$\nabla^2 G(x,y) = G(x,y) \cdot \frac{x^2 + y^2 - 2\sigma^2}{\sigma^4}$$

O de forma expandida:

$$\nabla^2 G(x,y) = \frac{1}{2\pi\sigma^2} e^{-\frac{x^2+y^2}{2\sigma^2}} \cdot \frac{x^2 + y^2 - 2\sigma^2}{\sigma^4}$$

#### **Paso 2: Cálculo de parámetros para σ = 0.8**

$$\sigma = 0.8$$

$$\sigma^2 = (0.8)^2 = 0.64$$

$$\sigma^4 = (0.64)^2 = 0.4096$$

$$2\sigma^2 = 2(0.64) = 1.28$$

$$2\pi\sigma^2 = 2\pi(0.64) = 4.021238$$

#### **Paso 3: Determinación del tamaño del kernel**

La recomendación es usar un kernel de al menos $6\sigma$:

$$\text{Tamaño mínimo} = 6\sigma = 6(0.8) = 4.8 \approx 5$$

Usaremos un kernel de $5 \times 5$ centrado en $(0,0)$, con coordenadas desde $-2$ hasta $2$.

#### **Paso 4: Cálculo de posiciones únicas (por simetría)**

Por simetría, solo calculamos posiciones únicas en un cuadrante:

$$
\begin{array}{|c|c|}
\hline
\textbf{Posición} & \textbf{Descripción} \\
\hline
(0,0) & \text{Centro} \\
\hline
(0,1) & \text{Adyacente horizontal} \\
\hline
(1,1) & \text{Diagonal cercana} \\
\hline
(0,2) & \text{Borde horizontal} \\
\hline
(1,2) & \text{Diagonal intermedia} \\
\hline
(2,2) & \text{Esquina} \\
\hline
\end{array}
$$

#### **Paso 5: Cálculo paso a paso para cada posición**

**Para (0,0) - Centro:**

$$x^2 + y^2 = 0^2 + 0^2 = 0$$

$$\alpha = \frac{0 - 1.28}{0.4096} = \frac{-1.28}{0.4096} = -3.125$$

$$G(0,0) = \frac{1}{4.021238} e^{-\frac{0}{1.28}} = \frac{1}{4.021238} \cdot 1 = 0.248697$$

$$\nabla^2 G(0,0) = 0.248697 \times (-3.125) = -0.777179$$

**Para (0,1) - Adyacente:**

$$x^2 + y^2 = 0^2 + 1^2 = 1$$

$$\alpha = \frac{1 - 1.28}{0.4096} = \frac{-0.28}{0.4096} = -0.683594$$

$$G(0,1) = \frac{1}{4.021238} e^{-\frac{1}{1.28}} = \frac{1}{4.021238} \cdot 0.456537 = 0.113518$$

$$\nabla^2 G(0,1) = 0.113518 \times (-0.683594) = -0.077597$$

**Para (1,1) - Diagonal cercana:**

$$x^2 + y^2 = 1^2 + 1^2 = 2$$

$$\alpha = \frac{2 - 1.28}{0.4096} = \frac{0.72}{0.4096} = 1.757813$$

$$G(1,1) = \frac{1}{4.021238} e^{-\frac{2}{1.28}} = \frac{1}{4.021238} \cdot 0.208463 = 0.051846$$

$$\nabla^2 G(1,1) = 0.051846 \times 1.757813 = 0.091143$$

**Para (0,2) - Borde horizontal:**

$$x^2 + y^2 = 0^2 + 2^2 = 4$$

$$\alpha = \frac{4 - 1.28}{0.4096} = \frac{2.72}{0.4096} = 6.640625$$

$$G(0,2) = \frac{1}{4.021238} e^{-\frac{4}{1.28}} = \frac{1}{4.021238} \cdot 0.043183 = 0.010741$$

$$\nabla^2 G(0,2) = 0.010741 \times 6.640625 = 0.071329$$

**Para (1,2) - Diagonal intermedia:**

$$x^2 + y^2 = 1^2 + 2^2 = 5$$

$$\alpha = \frac{5 - 1.28}{0.4096} = \frac{3.72}{0.4096} = 9.082031$$

$$G(1,2) = \frac{1}{4.021238} e^{-\frac{5}{1.28}} = \frac{1}{4.021238} \cdot 0.019726 = 0.004906$$

$$\nabla^2 G(1,2) = 0.004906 \times 9.082031 = 0.044554$$

**Para (2,2) - Esquina:**

$$x^2 + y^2 = 2^2 + 2^2 = 8$$

$$\alpha = \frac{8 - 1.28}{0.4096} = \frac{6.72}{0.4096} = 16.406250$$

$$G(2,2) = \frac{1}{4.021238} e^{-\frac{8}{1.28}} = \frac{1}{4.021238} \cdot 0.001845 = 0.000459$$

$$\nabla^2 G(2,2) = 0.000459 \times 16.406250 = 0.007530$$

#### **Paso 6: Tabla resumen de cálculos**

$$
\begin{array}{|c|c|c|c|c|c|c|}
\hline
\textbf{Posición} & x^2+y^2 & \alpha & G(x,y) & \nabla^2 G(x,y) & \textbf{Cantidad} & \textbf{Suma parcial} \\
\hline
(0,0) & 0 & -3.125 & 0.248697 & -0.777179 & 1 & -0.777179 \\
\hline
(0,\pm1), (\pm1,0) & 1 & -0.684 & 0.113518 & -0.077597 & 4 & -0.310388 \\
\hline
(\pm1,\pm1) & 2 & 1.758 & 0.051846 & 0.091143 & 4 & 0.364572 \\
\hline
(0,\pm2), (\pm2,0) & 4 & 6.641 & 0.010741 & 0.071329 & 4 & 0.285316 \\
\hline
(\pm1,\pm2), (\pm2,\pm1) & 5 & 9.082 & 0.004906 & 0.044554 & 8 & 0.356432 \\
\hline
(\pm2,\pm2) & 8 & 16.406 & 0.000459 & 0.007530 & 4 & 0.030120 \\
\hline
\end{array}
$$

#### **Paso 7: Suma total y normalización**

**Suma total sin normalizar:**

$$k_l = -0.777179 - 0.310388 + 0.364572 + 0.285316 + 0.356432 + 0.030120 = -0.051127$$

**Para normalizar a suma cero (requisito de filtros pasa-alto):**

$$\bar{k}_l = \frac{k_l}{25} = \frac{-0.051127}{25} = -0.002045$$

**Valores normalizados:**

$$\nabla^2_n G(x,y) = \nabla^2 G(x,y) - \bar{k}_l$$

$$
\begin{array}{|c|c|c|}
\hline
\textbf{Posición} & \nabla^2 G(x,y) & \nabla^2_n G(x,y) \\
\hline
(0,0) & -0.777179 & -0.775134 \\
\hline
(0,1) & -0.077597 & -0.075552 \\
\hline
(1,1) & 0.091143 & 0.093188 \\
\hline
(0,2) & 0.071329 & 0.073374 \\
\hline
(1,2) & 0.044554 & 0.046599 \\
\hline
(2,2) & 0.007530 & 0.009575 \\
\hline
\end{array}
$$

#### **Paso 8: Kernel LoG 5×5 completo normalizado**

$$\nabla^2_n G = \begin{bmatrix}
0.0096 & 0.0466 & 0.0734 & 0.0466 & 0.0096 \\
0.0466 & 0.0932 & -0.0756 & 0.0932 & 0.0466 \\
0.0734 & -0.0756 & -0.7751 & -0.0756 & 0.0734 \\
0.0466 & 0.0932 & -0.0756 & 0.0932 & 0.0466 \\
0.0096 & 0.0466 & 0.0734 & 0.0466 & 0.0096
\end{bmatrix}$$

#### **Paso 9: Verificación de suma cero**

$$\begin{align}
\text{Suma} &= 1(-0.7751) + 4(-0.0756) + 4(0.0932) + 4(0.0734) + 8(0.0466) + 4(0.0096) \\
&= -0.7751 - 0.3024 + 0.3728 + 0.2936 + 0.3728 + 0.0384 \\
&\approx 0.0001 \approx 0
\end{align}$$

La pequeña diferencia se debe al redondeo.

#### **Paso 10: Visualización conceptual**

El kernel LoG tiene forma de **"sombrero mexicano invertido"**:

- **Centro negativo** (-0.7751): Detecta picos brillantes
- **Primer anillo negativo** (-0.0756): Transición
- **Anillo positivo** (0.0932, 0.0734, 0.0466): Contrasta con el centro
- **Periferia baja** (0.0096): Decae hacia cero

#### **Paso 11: Propiedades del kernel resultante**

**1. Suma cero:**
$$\sum \nabla^2_n G(x,y) \approx 0$$
(Filtro pasa-alto)

**2. Simetría isotrópica:**
$$\nabla^2_n G(x,y) = \nabla^2_n G(-x,-y) = \nabla^2_n G(y,x)$$

**3. Valor central dominante:**
El centro (-0.7751) tiene la mayor magnitud

**4. Decrecimiento radial:**
Los valores disminuyen al alejarse del centro

#### **Paso 12: Aplicaciones del kernel LoG**

**Detección de bordes:**
- Encuentra bordes mediante **cruces por cero**
- Combina suavizado y detección en una operación

**Detección de características:**
- Identifica **blobs** (regiones circulares)
- Usado en detectores SIFT, SURF

**Realce de bordes:**
- Acentúa transiciones de intensidad
- Reduce el efecto del ruido

#### **Resultado final:**

$$\boxed{
\nabla^2_n G_{\sigma=0.8} = \begin{bmatrix}
0.010 & 0.047 & 0.073 & 0.047 & 0.010 \\
0.047 & 0.093 & -0.076 & 0.093 & 0.047 \\
0.073 & -0.076 & -0.775 & -0.076 & 0.073 \\
0.047 & 0.093 & -0.076 & 0.093 & 0.047 \\
0.010 & 0.047 & 0.073 & 0.047 & 0.010
\end{bmatrix}
}$$

**Parámetros del kernel:**
- **Tamaño**: $5 \times 5$
- **Desviación estándar**: $\sigma = 0.8$
- **Suma total**: $\approx 0$ (normalizado)
- **Rango de valores**: $[-0.775, 0.093]$
- **Características**: Isotrópico, simétrico, pasa-alto

Este kernel está listo para ser aplicado a una imagen mediante convolución para detectar bordes y características con reducción de ruido incorporada.

---