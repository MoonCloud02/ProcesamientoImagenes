# Capítulo 6: Conversión a Escala de Grises - Respuestas

## 6.7.1. Preguntas de Hechos Indicados en el Texto

### 1. ¿Por qué las imágenes en escala de grises requieren solo una matriz bidimensional para su representación?

Las imágenes en escala de grises requieren solo una matriz bidimensional (2D) porque únicamente almacenan información de intensidad luminosa, sin considerar las diferentes fuentes espectrales de color. A diferencia de las imágenes RGB que necesitan tres matrices (una para cada canal: rojo, verde y azul), las imágenes en escala de grises representan cada píxel mediante un único valor que indica su nivel de intensidad o luminancia. Esta representación simplificada es suficiente para capturar la información de brillo de la imagen, lo que resulta en una estructura de datos más simple y eficiente computacionalmente.

### 2. ¿Cuáles son las técnicas de conversión a escala de grises enunciadas en el documento?

El documento enuncia las siguientes técnicas de conversión a escala de grises:

1. **Técnica del Promedio**: Calcula el promedio aritmético simple de los tres canales RGB con pesos iguales ($w_R = w_G = w_B = \frac{1}{3}$).

2. **Técnica del Midgray**: Se define como la media de las componentes de color máxima y mínima, proporcionando un tono intermedio de gris.

3. **Técnica de la Luminancia**: Utiliza un promedio ponderado de las componentes RGB que considera la percepción visual humana. Incluye dos estándares principales:
   - **NTSC 601**: Con pesos $w_R = 0.299$, $w_G = 0.587$, $w_B = 0.114$
   - **Rec. 709**: Con pesos $w_R = 0.2226$, $w_G = 0.7152$, $w_B = 0.0722$

### 3. ¿Qué problema se plantea al intentar convertir una imagen RGB a escala de grises?

El problema principal es que cada capa de color individual (roja, verde o azul) solo contiene información espectral de su propia tonalidad y no representa fielmente la imagen completa. Por ejemplo:

- La capa roja muestra claros los rojos y obscuros el azul
- El color amarillo aparece blanco en la capa roja (porque es mezcla de rojo y verde)
- La capa verde también muestra blanco el amarillo y obscuro los colores rojo y azul
- La capa azul muestra blanco solo el azul y negro los demás colores

Por lo tanto, simplemente tomar una de las capas no produce una representación adecuada de la imagen en escala de grises. Se requiere una combinación apropiada de las tres capas que considere tanto las características físicas del color como la percepción visual humana.

### 4. ¿Cuál de las técnicas le da más peso o ponderación a uno de los colores RGB, y por qué?

La **técnica de la luminancia** (tanto NTSC 601 como Rec. 709) le da más peso al color **verde**. Específicamente:

- **NTSC 601**: $w_G = 0.587$ (mayor que $w_R = 0.299$ y $w_B = 0.114$)
- **Rec. 709**: $w_G = 0.7152$ (mayor que $w_R = 0.2226$ y $w_B = 0.0722$)

**Razón**: Este mayor peso al verde se debe a que el ojo humano tiene un mayor número de conos sensibles al color verde en la retina. Por lo tanto, la percepción humana de la luminosidad está más influenciada por la componente verde que por las componentes roja y azul. Esto hace que la imagen resultante sea más brillante y se aproxime mejor a cómo los humanos perciben el brillo de la imagen.

### 5. Entre NTSC 601 y Rec. 709, ¿cuál es más antigua y cuál es su uso principal?

**NTSC 601 es más antigua** que Rec. 709.

**NTSC 601**:
- Desarrollada originalmente para la codificación de señales analógicas de televisión a color
- Es una norma técnica utilizada para la codificación de video analógico en televisión
- Especifica la forma en que se codifica la información de color en el formato YUV para televisión analógica
- Uso principal: Televisión analógica de definición estándar

**Rec. 709** (ITU-R BT.709):
- Desarrollada por la Unión Internacional de Telecomunicaciones (ITU) en 1990
- Es un estándar internacional más reciente
- Uso principal: Producción, transmisión y visualización de imágenes de televisión en color de **alta definición (HD)**
- Los valores de ponderación fueron establecidos experimentalmente para producir una imagen en escala de grises que parece más natural al ojo humano en contenido HD

---

## 6.7.2. Preguntas de Comprensión con Base en el Texto

### 1. ¿Por qué el verde tiene mayor peso en las técnicas de conversión con base en la luminancia?

El verde tiene mayor peso en las técnicas de conversión basadas en luminancia porque está directamente relacionado con la **fisiología del ojo humano**. La retina humana contiene tres tipos de conos responsables de la percepción del color, pero el número de conos no está distribuido uniformemente entre los tres colores primarios.

**Explicación detallada**:

1. **Mayor densidad de conos verdes**: La retina humana tiene una mayor cantidad de conos sensibles a las longitudes de onda del verde en comparación con los conos sensibles al rojo y al azul.

2. **Sensibilidad espectral**: El ojo humano es más sensible a las longitudes de onda correspondientes al color verde (aproximadamente 555 nm), que se encuentra en el centro del espectro visible.

3. **Percepción de luminancia**: Debido a esta mayor sensibilidad, la componente verde contribuye más significativamente a la percepción de brillo o luminancia de una escena.

4. **Resultado práctico**: Al asignar mayor peso al canal verde (0.587 en NTSC 601 o 0.7152 en Rec. 709), la imagen resultante en escala de grises se aproxima mejor a cómo los humanos perciben realmente el brillo de la imagen original, haciéndola más brillante y perceptualmente más precisa.

Esta ponderación no es arbitraria, sino que está basada en estudios experimentales de la respuesta del sistema visual humano.

### 2. ¿Es posible convertir imágenes en escala de grises de vuelta a color a partir de solo sus valores en gris?

**No, no es posible** realizar una conversión directa e inequívoca de una imagen en escala de grises de vuelta a sus colores originales utilizando únicamente los valores de gris.

**Razones fundamentales**:

1. **Pérdida de información**: La conversión a escala de grises es un proceso irreversible que implica pérdida de información. Por ejemplo, con NTSC 601:
   $$Y = 0.299R + 0.587G + 0.114B$$
   
   Esta ecuación tiene infinitas soluciones para $(R, G, B)$ dado un valor fijo de $Y$.

2. **Múltiples combinaciones**: Como ilustra la Figura 6.5 del texto, múltiples combinaciones de valores RGB pueden producir el mismo valor en escala de grises. Colores visualmente diferentes pueden tener la misma luminancia.

3. **Sistema subdeterminado**: Matemáticamente, tenemos una ecuación (el valor de gris) con tres incógnitas $(R, G, B)$, lo que resulta en un sistema subdeterminado con infinitas soluciones.

**Alternativas mencionadas en el texto**:

Sin embargo, existen técnicas de **colorización** o **falso color** que pueden agregar color a imágenes en escala de grises:

1. **Procesamiento de falso color**: Se usa para representar datos como gradientes de temperatura, información meteorológica, rayos X, etc.

2. **Colorización basada en conocimiento**: Si se conoce información adicional sobre la imagen original (por ejemplo, que contiene piel humana, cielo, mar, árboles), se pueden estimar aproximaciones a las tonalidades originales.

3. **Algoritmos de reconocimiento**: Se han desarrollado algoritmos que reconocen objetos específicos y los colorean según su naturaleza esperada (firmamento azul, mar azul, árboles verdes, etc.).

Es importante enfatizar que estas técnicas **no recuperan los colores originales exactos**, sino que generan aproximaciones o representaciones basadas en información adicional o suposiciones sobre el contenido de la imagen.

### 3. ¿Cómo afectan las técnicas de conversión a la percepción visual en aplicaciones prácticas?

Las técnicas de conversión a escala de grises tienen un impacto significativo en la percepción visual y su efectividad varía según la aplicación práctica:

#### **Impacto según la técnica utilizada:**

1. **Técnica del Promedio**:
   - **Ventaja**: Computacionalmente simple y rápida
   - **Desventaja**: No refleja fielmente la percepción humana de luminosidad
   - **Impacto visual**: Puede producir imágenes que parecen menos naturales o con menor contraste de lo esperado
   - **Aplicaciones**: Útil cuando la velocidad es prioritaria sobre la fidelidad visual

2. **Técnica del Midgray**:
   - **Ventaja**: Mantiene un nivel básico de contraste
   - **Desventaja**: Pérdida considerable de información y detalle
   - **Impacto visual**: Proporciona un tono intermedio uniforme pero puede no adaptarse bien a imágenes con distribuciones de color complejas
   - **Aplicaciones**: Soluciones simples donde se requiere contraste básico

3. **Técnica de Luminancia (NTSC 601 / Rec. 709)**:
   - **Ventaja**: Refleja mejor la percepción humana al ponderar más el verde
   - **Impacto visual**: Produce imágenes más brillantes y naturales que se aproximan a cómo el ojo humano percibe el brillo
   - **Aplicaciones**: Preferida para procesamiento profesional de imagen y video

#### **Efectos en aplicaciones prácticas:**

1. **Detección de bordes y segmentación**:
   - La luminancia ponderada preserva mejor los gradientes de intensidad que corresponden a características visuales importantes
   - Mejora la efectividad de algoritmos como Canny y Sobel al mantener información perceptualmente relevante

2. **Reconocimiento de patrones**:
   - La representación en escala de grises que refleja la percepción humana facilita el reconocimiento de objetos
   - El contraste apropiado es crucial para distinguir características discriminantes

3. **Compresión y transmisión**:
   - Las técnicas que preservan la percepción visual permiten mayor compresión sin pérdida perceptual significativa
   - Importante en aplicaciones de videoconferencia y streaming

4. **Análisis médico**:
   - En rayos X y resonancias magnéticas, la representación fiel de la luminancia puede ser crítica para diagnósticos precisos
   - El contraste adecuado facilita la identificación de anomalías

5. **Robótica y visión artificial**:
   - Como menciona el texto, en robótica móvil para seguimiento de líneas, la escala de grises reduce información no relevante
   - La simplificación mejora el rendimiento sin sacrificar funcionalidad

### 4. ¿Qué desafíos presenta la conversión a escala de grises en términos de la preservación de detalles?

La conversión a escala de grises enfrenta varios desafíos significativos relacionados con la preservación de detalles:

#### **1. Pérdida de información cromática distintiva**:

- **Problema**: Como ilustra el texto con la bandera (Fig. 6.1), colores diferentes pueden tener luminancias similares
- **Ejemplo práctico**: El rojo y el verde pueden aparecer con el mismo tono de gris a pesar de ser colores completamente distintos
- **Impacto**: Objetos que se distinguen claramente por color pueden volverse indistinguibles en escala de grises
- **Consecuencia**: Pérdida de información crítica en aplicaciones como segmentación de objetos por color

#### **2. Reducción del espacio de representación**:

- **Problema matemático**: Se pasa de un espacio tridimensional RGB (posiblemente $256^3 \approx 16.7$ millones de colores) a un espacio unidimensional (256 niveles de gris)
- **Pérdida de dimensionalidad**: Dos grados de libertad se eliminan completamente
- **Impacto**: Múltiples combinaciones RGB colapsan en el mismo valor de gris (como muestra la Fig. 6.5)

#### **3. Colapso de texturas y patrones de color**:

- **Desafío**: Texturas que dependen de variaciones cromáticas pueden desaparecer
- **Ejemplo**: Un patrón de rayas rojas y verdes de igual luminancia aparecería como una superficie uniforme
- **Aplicaciones afectadas**: Análisis de texturas, reconocimiento de materiales, clasificación de superficies

#### **4. Sensibilidad a las capas individuales**:

- **Problema**: Como menciona el texto, cada capa de color (R, G, B) individual no representa fielmente la imagen
- **Capa roja**: Muestra claros los rojos y obscuros el azul; el amarillo aparece blanco
- **Capa verde**: Muestra blanco el amarillo, obscuros el rojo y azul
- **Capa azul**: Solo el azul se ve blanco, los demás lucen negros
- **Desafío**: Elegir la técnica apropiada es crucial para preservar los detalles relevantes

#### **5. Pérdida de contraste selectivo**:

- **Problema**: Objetos con colores complementarios que tienen alto contraste cromático pero similar luminancia pierden separación
- **Ejemplo**: Texto azul sobre fondo rojo puede volverse casi ilegible en escala de grises
- **Impacto**: Afecta la legibilidad y distinguibilidad en interfaces y documentos

#### **6. Dependencia del contexto y aplicación**:

- **Desafío**: No existe una técnica universal óptima
- **Técnica del promedio**: Puede perder detalles en regiones con predominancia de un color
- **Midgray**: Puede no adaptarse a características específicas de la imagen
- **Luminancia**: Mejor para percepción general, pero puede no ser óptima para análisis técnico específico

#### **7. Variaciones en iluminación**:

- **Problema**: La conversión puede amplificar o atenuar efectos de iluminación no uniforme
- **Desafío práctico**: En el ejemplo del mango (Fig. 6.3 del texto), se menciona que la técnica puede fracasar cuando no hay luz homogénea
- **Impacto**: Dificulta la segmentación y el procesamiento en condiciones reales

#### **Estrategias de mitigación mencionadas en el texto**:

1. **Selección de capa apropiada**: En la Fig. 6.3, la capa azul separa mejor el mango del fondo porque los mangos carecen de componente azul
2. **Análisis en espacio HSV**: Como muestra el ejemplo de segmentación del auto amarillo (Fig. 6.6), trabajar en HSV permite mantener separación por color antes de convertir a grises
3. **Segmentación selectiva**: Mantener ciertos elementos en color mientras el resto se convierte a grises (técnica de falso color)

### 5. ¿Cuál es la relación entre los estándares NTSC 601 y Rec. 709 y la evolución de la tecnología?

La relación entre NTSC 601 y Rec. 709 refleja claramente la evolución tecnológica en sistemas de video y televisión:

#### **Evolución cronológica y tecnológica:**

**1. NTSC 601 - Era analógica de televisión**:

- **Contexto histórico**: Desarrollada para la televisión analógica a color de definición estándar (SD)
- **Tecnología**: Sistemas de transmisión analógica con limitaciones de ancho de banda
- **Resolución**: Diseñada para televisión de definición estándar (aproximadamente 480i/576i)
- **Formato de color**: Especifica la codificación en formato YUV para señales analógicas
- **Coeficientes de luminancia**: $Y = 0.299R + 0.587G + 0.114B$

**2. Rec. 709 (ITU-R BT.709) - Era digital HD**:

- **Contexto histórico**: Desarrollada en 1990 por la Unión Internacional de Telecomunicaciones (ITU)
- **Tecnología**: Sistemas digitales de alta definición (HD)
- **Resolución**: Diseñada para HD (720p, 1080i, 1080p)
- **Coeficientes actualizados**: $Y = 0.2226R + 0.7152G + 0.0722B$
- **Objetivo**: Producir una imagen que parece más natural al ojo humano en displays modernos

#### **Diferencias clave y su significado tecnológico:**

**1. Mayor peso al verde en Rec. 709**:
- NTSC 601: $w_G = 0.587$ (58.7%)
- Rec. 709: $w_G = 0.7152$ (71.52%)
- **Razón evolutiva**: Investigación más refinada sobre la respuesta del sistema visual humano y displays digitales más precisos

**2. Reducción del peso del rojo**:
- NTSC 601: $w_R = 0.299$ (29.9%)
- Rec. 709: $w_R = 0.2226$ (22.26%)
- **Implicación**: Mejor aproximación a la sensibilidad espectral real del ojo humano

**3. Reducción del peso del azul**:
- NTSC 601: $w_B = 0.114$ (11.4%)
- Rec. 709: $w_B = 0.0722$ (7.22%)
- **Significado**: El ojo humano es menos sensible al azul de lo que se consideraba originalmente

#### **Drivers de la evolución tecnológica:**

1. **Mejores displays**:
   - Transición de CRTs analógicos a pantallas digitales (LCD, LED, OLED)
   - Mayor precisión en reproducción de color y luminancia
   - Rec. 709 optimizada para características de displays digitales modernos

2. **Procesamiento digital**:
   - NTSC 601 diseñada para limitaciones analógicas
   - Rec. 709 aprovecha capacidades de procesamiento digital sin restricciones analógicas

3. **Investigación en visión humana**:
   - Estudios experimentales más precisos sobre percepción visual
   - Comprensión mejorada de la distribución y sensibilidad de conos en la retina
   - Rec. 709 incorpora este conocimiento actualizado

4. **Ancho de banda y compresión**:
   - Mayor ancho de banda disponible en era digital
   - Técnicas de compresión más sofisticadas (MPEG, H.264, H.265)
   - Rec. 709 diseñada para trabajar eficientemente con estos códecs

5. **Resolución y calidad**:
   - Transición de SD (~480p) a HD (~1080p) y más allá
   - Necesidad de estándares que preserven mejor detalles en altas resoluciones

#### **Impacto en aplicaciones prácticas:**

1. **Compatibilidad retroactiva**: Sistemas modernos a menudo soportan ambos estándares para contenido legacy
2. **Flujos de trabajo profesionales**: Producción de video moderna usa Rec. 709 como estándar
3. **Evolución continua**: Rec. 709 es base para estándares aún más avanzados (Rec. 2020 para UHD/4K con mayor gamut)

#### **Lecciones sobre evolución tecnológica:**

1. **Los estándares evolucionan con la tecnología**: NTSC 601 fue óptima para su época, Rec. 709 para la siguiente
2. **Conocimiento científico mejorado**: Mejor comprensión del sistema visual humano impulsó cambios
3. **Trade-offs diferentes**: Cada generación tecnológica tiene diferentes limitaciones y posibilidades
4. **Coexistencia necesaria**: Ambos estándares persisten para soportar contenido de diferentes épocas

**Conclusión**: La relación entre NTSC 601 y Rec. 709 ejemplifica cómo los estándares técnicos evolucionan respondiendo a avances en tecnología de displays, procesamiento digital, y comprensión científica de la percepción humana, manteniendo siempre el objetivo de representar imágenes de la manera más natural posible para el espectador.

---

## 6.7.3. Ejercicios Numéricos con Base en el Texto

### 1. Dada una imagen con píxeles RGB $(R, G, B) = (120, 200, 80)$, calcule el valor en escala de grises usando las técnicas del promedio y Midgray.

**Datos:**
- $R = 120$
- $G = 200$
- $B = 80$

#### a) Técnica del Promedio

La técnica del promedio utiliza pesos iguales para las tres componentes:

$$Y_{\text{promedio}} = w_R \cdot R + w_G \cdot G + w_B \cdot B$$

donde $w_R = w_G = w_B = \frac{1}{3}$

$$Y_{\text{promedio}} = \frac{1}{3} \cdot 120 + \frac{1}{3} \cdot 200 + \frac{1}{3} \cdot 80$$

$$Y_{\text{promedio}} = \frac{120 + 200 + 80}{3} = \frac{400}{3} = 133.33$$

**Resultado:** $Y_{\text{promedio}} = 133.33$ (redondeando a $133$ para valores enteros)

#### b) Técnica del Midgray

La técnica del Midgray se define como la media de las componentes máxima y mínima:

$$Y_{\text{Midgray}} = \frac{\max(R, G, B) + \min(R, G, B)}{2}$$

Identificando los valores extremos:
- $\max(120, 200, 80) = 200$
- $\min(120, 200, 80) = 80$

$$Y_{\text{Midgray}} = \frac{200 + 80}{2} = \frac{280}{2} = 140$$

**Resultado:** $Y_{\text{Midgray}} = 140$

#### Comparación:
- Promedio: $133.33$
- Midgray: $140$

La técnica Midgray produce un valor ligeramente mayor en este caso porque solo considera los valores extremos, mientras que el promedio considera también el valor intermedio.

---

### 2. Para un píxel RGB con valores $(80, 160, 100)$, calcule el valor en escala de grises usando NTSC 601 y Rec. 709, y discuta los resultados.

**Datos:**
- $R = 80$
- $G = 160$
- $B = 100$

#### a) NTSC 601

Los coeficientes NTSC 601 son:
- $w_R = 0.299$
- $w_G = 0.587$
- $w_B = 0.114$

$$Y_{601} = 0.299 \cdot R + 0.587 \cdot G + 0.114 \cdot B$$

$$Y_{601} = 0.299 \cdot 80 + 0.587 \cdot 160 + 0.114 \cdot 100$$

$$Y_{601} = 23.92 + 93.92 + 11.40 = 129.24$$

**Resultado:** $Y_{601} = 129.24$ (redondeando a $129$ para valores enteros)

#### b) Rec. 709

Los coeficientes Rec. 709 son:
- $w_R = 0.2226$
- $w_G = 0.7152$
- $w_B = 0.0722$

$$Y_{709} = 0.2226 \cdot R + 0.7152 \cdot G + 0.0722 \cdot B$$

$$Y_{709} = 0.2226 \cdot 80 + 0.7152 \cdot 160 + 0.0722 \cdot 100$$

$$Y_{709} = 17.808 + 114.432 + 7.22 = 139.46$$

**Resultado:** $Y_{709} = 139.46$ (redondeando a $139$ para valores enteros)

#### Discusión de Resultados:

1. **Diferencia en luminancia**: Rec. 709 produce un valor más alto ($139.46$) que NTSC 601 ($129.24$), una diferencia de aproximadamente $10.22$ unidades o $7.9\%$.

2. **Mayor peso al verde**: Rec. 709 asigna un peso significativamente mayor al canal verde ($0.7152$ vs $0.587$), lo que explica el valor más alto dado que $G = 160$ es la componente dominante en este píxel.

3. **Menor peso al rojo**: Rec. 709 reduce el peso del rojo ($0.2226$ vs $0.299$), lo que tiene menos impacto en este caso específico donde el rojo es el valor más bajo.

4. **Compensación en azul**: El peso del azul disminuye ligeramente en Rec. 709 ($0.0722$ vs $0.114$), pero el impacto es menor dado el valor intermedio de $B = 100$.

5. **Percepción visual**: Rec. 709, al ser un estándar más moderno diseñado para HD, produce una imagen que parece más natural al ojo humano, haciendo la imagen más brillante debido al mayor peso del verde, que es donde el ojo humano tiene mayor sensibilidad.

6. **Aplicación práctica**: Para contenido HD moderno, se debe preferir Rec. 709, mientras que NTSC 601 es más apropiado para contenido de video analógico o de definición estándar.

---

### 3. Dadas las intensidades $R = 10$ como valor mínimo, $Y_{\text{Midgray}} = 45$ y $Y_{\text{Promedio}} = 40$, determine los valores faltantes de $B$ y $G$, sabiendo que esta última no es el máximo.

**Datos:**
- $R = 10$ (valor mínimo)
- $Y_{\text{Midgray}} = 45$
- $Y_{\text{Promedio}} = 40$
- $G$ no es el máximo

**Análisis:**

Dado que $R = 10$ es el mínimo, entonces $G \geq 10$ y $B \geq 10$.

Como $G$ no es el máximo, entonces $B$ debe ser el máximo: $B \geq G \geq R = 10$.

#### Ecuación 1: Midgray

$$Y_{\text{Midgray}} = \frac{\max(R, G, B) + \min(R, G, B)}{2}$$

Como $R = 10$ es el mínimo y $B$ es el máximo:

$$45 = \frac{B + 10}{2}$$

$$90 = B + 10$$

$$B = 80$$

#### Ecuación 2: Promedio

$$Y_{\text{Promedio}} = \frac{R + G + B}{3}$$

$$40 = \frac{10 + G + 80}{3}$$

$$120 = 10 + G + 80$$

$$120 = 90 + G$$

$$G = 30$$

#### Verificación:

Verificamos que se cumplan las condiciones:
1. $R = 10$ es el mínimo: $10 < 30 < 80$ 
2. $G = 30$ no es el máximo: $B = 80$ es el máximo 
3. Midgray: $\frac{80 + 10}{2} = \frac{90}{2} = 45$ 
4. Promedio: $\frac{10 + 30 + 80}{3} = \frac{120}{3} = 40$ 

**Resultado:**
- $G = 30$
- $B = 80$

El píxel completo es $(R, G, B) = (10, 30, 80)$, donde predomina el azul.

---

### 4. Dado un valor en escala de grises $Y = 128$ y los coeficientes de NTSC 601 y Rec. 709, determine el valor de $R$ si $G = 100$ y $B = 50$ en cada caso. Comente si la solución es única.

**Datos:**
- $Y = 128$
- $G = 100$
- $B = 50$

#### a) NTSC 601

La ecuación de luminancia es:

$$Y_{601} = 0.299 \cdot R + 0.587 \cdot G + 0.114 \cdot B$$

Sustituyendo los valores conocidos:

$$128 = 0.299 \cdot R + 0.587 \cdot 100 + 0.114 \cdot 50$$

$$128 = 0.299 \cdot R + 58.7 + 5.7$$

$$128 = 0.299 \cdot R + 64.4$$

$$128 - 64.4 = 0.299 \cdot R$$

$$63.6 = 0.299 \cdot R$$

$$R = \frac{63.6}{0.299} = 212.71$$

**Resultado NTSC 601:** $R \approx 213$ (redondeado)

#### b) Rec. 709

La ecuación de luminancia es:

$$Y_{709} = 0.2226 \cdot R + 0.7152 \cdot G + 0.0722 \cdot B$$

Sustituyendo los valores conocidos:

$$128 = 0.2226 \cdot R + 0.7152 \cdot 100 + 0.0722 \cdot 50$$

$$128 = 0.2226 \cdot R + 71.52 + 3.61$$

$$128 = 0.2226 \cdot R + 75.13$$

$$128 - 75.13 = 0.2226 \cdot R$$

$$52.87 = 0.2226 \cdot R$$

$$R = \frac{52.87}{0.2226} = 237.48$$

**Resultado Rec. 709:** $R \approx 237$ (redondeado)

#### Verificación de validez:

- NTSC 601: $R = 213$ está en el rango válido $[0, 255]$ 
- Rec. 709: $R = 237$ está en el rango válido $[0, 255]$ 

#### Comentario sobre unicidad de la solución:

**La solución ES única** en cada caso, dado que:

1. **Sistema determinado**: Tenemos una ecuación lineal con una incógnita ($R$) y todos los demás valores son conocidos ($Y$, $G$, $B$ y los coeficientes).

2. **Diferencia entre estándares**: Los dos estándares producen valores diferentes de $R$ para el mismo valor de $Y$:
   - NTSC 601: $R = 213$
   - Rec. 709: $R = 237$
   
   Esta diferencia de $24$ unidades es significativa y se debe a los diferentes coeficientes de ponderación.

3. **No hay solución única general**: Sin embargo, si **no** se especifican los valores de $G$ y $B$, entonces el problema tendría infinitas soluciones, ya que tendríamos una ecuación con tres incógnitas.

4. **Restricciones físicas**: La solución es única solo dentro del rango válido $[0, 255]$. Si la solución calculada estuviera fuera de este rango, indicaría que no existe una combinación RGB válida que produzca ese valor de luminancia con los valores dados de $G$ y $B$.

**Conclusión**: Cuando se fijan dos de las tres componentes RGB ($G$ y $B$ en este caso) y se conoce el valor de luminancia $Y$, la solución para la tercera componente ($R$) es **única y determinada** para cada estándar específico (NTSC 601 o Rec. 709).

---

### 5. Dado un píxel con luminancias conocidas $Y_{601} = 150$ (según NTSC 601) y $Y_{709} = 145$ (según Rec. 709), determine los valores de $R$ y $G$ si el valor de $B = 80$. Asegúrese de que los valores calculados para $R$ y $G$ estén en el rango entero entre $[0, 255]$.

**Datos:**
- $Y_{601} = 150$
- $Y_{709} = 145$
- $B = 80$
- Incógnitas: $R$ y $G$

#### Sistema de ecuaciones:

Tenemos dos ecuaciones con dos incógnitas:

**Ecuación 1 (NTSC 601):**
$$Y_{601} = 0.299R + 0.587G + 0.114B$$
$$150 = 0.299R + 0.587G + 0.114(80)$$
$$150 = 0.299R + 0.587G + 9.12$$
$$140.88 = 0.299R + 0.587G \quad \text{...(1)}$$

**Ecuación 2 (Rec. 709):**
$$Y_{709} = 0.2226R + 0.7152G + 0.0722B$$
$$145 = 0.2226R + 0.7152G + 0.0722(80)$$
$$145 = 0.2226R + 0.7152G + 5.776$$
$$139.224 = 0.2226R + 0.7152G \quad \text{...(2)}$$

#### Resolución del sistema:

**Método de eliminación:**

Multiplicamos la ecuación (1) por $0.2226$ y la ecuación (2) por $0.299$:

$$0.2226 \times 140.88 = 0.2226 \times (0.299R + 0.587G)$$
$$31.364 = 0.066577R + 0.130666G \quad \text{...(1')}$$

$$0.299 \times 139.224 = 0.299 \times (0.2226R + 0.7152G)$$
$$41.628 = 0.066557R + 0.213845G \quad \text{...(2')}$$

Restamos (1') de (2'):

$$41.628 - 31.364 = (0.066557 - 0.066577)R + (0.213845 - 0.130666)G$$
$$10.264 = -0.00002R + 0.083179G$$

Dado que el coeficiente de $R$ es prácticamente cero (error de redondeo), podemos aproximar:

$$10.264 \approx 0.083179G$$
$$G = \frac{10.264}{0.083179} = 123.43$$

**Resultado:** $G \approx 123$

Ahora sustituimos $G = 123.43$ en la ecuación (1):

$$140.88 = 0.299R + 0.587(123.43)$$
$$140.88 = 0.299R + 72.453$$
$$68.427 = 0.299R$$
$$R = \frac{68.427}{0.299} = 228.85$$

**Resultado:** $R \approx 229$

#### Verificación:

Con $R = 229$, $G = 123$, $B = 80$:

**NTSC 601:**
$$Y_{601} = 0.299(229) + 0.587(123) + 0.114(80)$$
$$Y_{601} = 68.471 + 72.201 + 9.12 = 149.79 \approx 150$$ 

**Rec. 709:**
$$Y_{709} = 0.2226(229) + 0.7152(123) + 0.0722(80)$$
$$Y_{709} = 50.975 + 87.970 + 5.776 = 144.72 \approx 145$$ 

#### Validación del rango:

- $R = 229 \in [0, 255]$ 
- $G = 123 \in [0, 255]$ 
- $B = 80 \in [0, 255]$ 

**Resultado final:**
- $R = 229$
- $G = 123$

El píxel completo es $(R, G, B) = (229, 123, 80)$, donde predomina el rojo, seguido del verde.

**Observaciones:**

1. **Sistema determinado**: Al tener dos ecuaciones lineales independientes con dos incógnitas, el sistema tiene solución única.

2. **Consistencia física**: Los valores de luminancia diferentes para los dos estándares ($Y_{601} = 150$ y $Y_{709} = 145$) son consistentes con un mismo píxel RGB, lo cual es esperado ya que cada estándar utiliza diferentes coeficientes de ponderación.

3. **Valores válidos**: Todos los valores calculados están dentro del rango entero válido $[0, 255]$, lo que confirma que existe una solución física para este problema.

4. **Predominancia del rojo**: El píxel resultante tiene un alto componente rojo ($229$), un componente medio de verde ($123$) y un componente relativamente bajo de azul ($80$), lo que produciría un color naranja-rojizo.
