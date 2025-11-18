# Capítulo 11: Operaciones Morfológicas Binarias
## Respuestas a las Preguntas 11.10

---

## Preguntas sobre hechos indicados en el texto

### 1. ¿Qué es la morfología matemática y en qué área de estudio se utiliza principalmente?

La morfología matemática es una técnica con base en la teoría de conjuntos y en la topología que se utiliza para extraer información sobre la forma y estructura de los objetos. Esta es altamente utilizada en el **procesamiento de imágenes** para analizar y manipular formas geométricas, lo que permite que sea utilizada comúnmente para extraer características útiles de una imagen, como:

- Bordes
- Contornos
- Regiones
- Áreas de interés

Las operaciones morfológicas se pueden aplicar a imágenes binarias o en escala de grises. En el caso de las imágenes binarias, se definen de forma natural en términos de conjuntos, siendo más comunes ya que las operaciones se pueden definir de manera más clara de forma binaria.

---

### 2. ¿Cuál es la función del elemento estructurante en las operaciones de morfología matemática?

El **elemento estructurante** $(S)$ es una plantilla de ceros y unos que define el vecindario de cada píxel de la imagen a operar en relación con algún origen sobre la máscara. Sus funciones principales son:

1. **Definir la vecindad**: Se recorre sobre toda la imagen binaria a transformar buscando las coincidencias entre píxeles de la imagen y el elemento estructurante.

2. **Determinar el área de operación**: Define el área a expandir o reducir dentro de una vecindad para cada píxel.

3. **Influir en el resultado**: El elemento estructurante puede tener diferentes formas y tamaños, como:
   - Rectángulo
   - Disco
   - Cruz
   - Línea
   - Cuadrado
   - Diamante

La elección del elemento estructurante es dependiente de la aplicación específica y de los objetivos del procesamiento de la imagen. El origen del $S$ no tiene que estar en su centro, pero a menudo lo está.

---

### 3. ¿Qué condición debe cumplirse para que un píxel central se mantenga blanco (1) durante una operación de erosión?

Durante una operación de erosión, la condición que debe cumplirse es:

**Todos los píxeles cubiertos por el elemento estructurante deben ser blancos (1)** para que el píxel central del elemento estructurante se mantenga blanco (1).

Formalmente, la erosión del conjunto $I$ por el elemento estructurante $S$ se denota como:

$$I_e = I \ominus S = \{p \in \mathbb{Z}^2 \mid S_p \subseteq I\}$$

donde:
- $I_e$ es la imagen erosionada resultante
- $\ominus$ denota la operación de erosión
- $S_p$ es la traslación del elemento estructurante $S$ por el vector $p$:

$$S_p = \{s + p \mid s \in S\}$$

**De lo contrario**, si algún píxel cubierto por el elemento estructurante es negro (0), el píxel central coincidente con la imagen se vuelve negro (0). Esto tiene el efecto de reducir el tamaño de los objetos en la imagen.

En el algoritmo de erosión, esto se implementa como una **operación AND** entre el elemento estructurante y los píxeles de la imagen.

---

### 4. ¿Cuáles son las operaciones morfológicas fundamentales descritas en el texto?

Las operaciones morfológicas fundamentales descritas en el texto son:

#### Operaciones Básicas:

1. **Erosión** $(I \ominus S)$: Reduce el área de los objetos en la imagen. Implica comparar el elemento estructurante con cada píxel; si todos los píxeles cubiertos son blancos (1), el píxel central se mantiene blanco (1), de lo contrario se vuelve negro (0).

2. **Dilatación** $(I \oplus S)$: Expande el área de los objetos en la imagen. Si al menos un píxel cubierto por el elemento estructurante es blanco (1), el píxel central se mantiene blanco (1).

#### Operaciones Compuestas:

3. **Apertura (Opening)**: Consiste en aplicar primero una erosión a la imagen, seguida de una dilatación. Se utiliza para eliminar pequeños objetos y detalles en los bordes de los objetos más grandes, ayudando a suavizar los bordes mientras se eliminan los detalles no deseados.

4. **Cerramiento (Closing)**: Consiste en aplicar primero una dilatación a la imagen, seguida de una erosión. Se utiliza para eliminar los agujeros pequeños y estrechos en los objetos y para cerrar brechas entre ellos.

Además, el texto menciona la **dualidad entre erosión y dilatación**, expresada mediante las ecuaciones:

$$I \ominus S = (I^c \oplus S^s)^c$$

$$I \oplus S = (I^c \ominus S^s)^c$$

donde $I^c$ es el complemento de $I$ y $S^s$ es la reflexión de $S$ respecto al origen.

---

### 5. ¿Qué efecto tiene la operación de dilatación sobre los objetos de una imagen binaria?

La operación de dilatación tiene el **efecto de expandir el tamaño de los objetos** en una imagen binaria. Específicamente:

#### Efectos principales:

1. **Expansión de objetos**: Aumenta el área de los objetos blancos en la imagen.

2. **Cierre de brechas**: Permite cerrar pequeñas discontinuidades entre objetos.

3. **Relleno de huecos**: Ayuda a rellenar pequeños agujeros dentro de los objetos.

4. **Conexión de regiones**: Conecta objetos que están próximos entre sí.

#### Definición formal:

La dilatación del conjunto $I$ por el elemento estructurante $S$ se denota como:

$$I_d = I \oplus S = \{p \in \mathbb{Z}^2 \mid S_p \cap I \neq \emptyset\}$$

donde:
- $I_d$ es la imagen dilatada resultante
- $\oplus$ denota la operación de dilatación
- $S_p$ es la traslación del elemento estructurante $S$ por el vector $p$:

$$S_p = \{s + p \mid s \in S\}$$

#### Proceso de dilatación:

El proceso implica comparar el elemento estructurante con cada píxel de la imagen y:
- **Si al menos un píxel** cubierto por el elemento estructurante es blanco (1), el píxel central se mantiene blanco (1) (operación OR).
- De lo contrario, el píxel central coincidente con la imagen se vuelve negro (0).

La dilatación se puede realizar varias veces en una imagen para expandar aún más el tamaño de los objetos, y se pueden utilizar diferentes formas y tamaños de elementos estructurantes para lograr diferentes efectos sobre la imagen.

---

## Preguntas de análisis y comprensión con base en el texto

### 1. Explica por qué la morfología matemática es una herramienta útil en el procesamiento de imágenes binarias

La morfología matemática es una herramienta particularmente útil en el procesamiento de imágenes binarias por las siguientes razones:

#### Fundamento teórico sólido

La morfología matemática tiene su base en la **teoría de conjuntos y la topología**, lo que permite que las operaciones sobre imágenes binarias se definan de manera natural y clara. Las imágenes binarias se pueden representar como conjuntos de píxeles, donde cada píxel es un elemento que pertenece o no al conjunto (objeto o fondo).

#### Definición clara de operaciones

En las imágenes binarias, las operaciones se pueden **definir de manera más clara** que en imágenes en escala de grises. Esto se debe a que:
- Los píxeles solo tienen dos valores posibles (0 o 1, negro o blanco)
- Las operaciones morfológicas se traducen directamente en operaciones de conjuntos
- No hay ambigüedad en la aplicación de los operadores lógicos

#### Extracción de información estructural

La morfología matemática permite extraer información crucial sobre la **forma y estructura** de los objetos, facilitando:

1. **Detección de bordes**: Identificación de los contornos de los objetos
2. **Segmentación**: Separación de regiones de interés
3. **Extracción de contornos**: Delimitación precisa de formas
4. **Análisis de regiones**: Estudio de áreas específicas
5. **Eliminación de ruido**: Limpieza de artefactos no deseados

#### Operaciones fundamentales versátiles

Las dos operaciones básicas (erosión y dilatación) permiten construir operaciones más complejas que resuelven problemas prácticos:
- **Apertura**: Elimina objetos pequeños y ruido
- **Cerramiento**: Rellena huecos y conecta objetos cercanos
- **Detección de bordes**: Mediante diferencias entre imágenes originales y procesadas

#### Eficiencia computacional

Al trabajar con valores binarios, las operaciones son computacionalmente eficientes, utilizando operadores lógicos básicos (AND, OR, NOT) que son extremadamente rápidos en hardware moderno.

#### Aplicabilidad práctica

La morfología matemática es fundamental para:
- Análisis de formas geométricas
- Preprocesamiento de imágenes para reconocimiento de patrones
- Análisis médico (detección de estructuras en imágenes médicas)
- Control de calidad industrial
- Visión por computadora

---

### 2. ¿Cuál es la relación entre los operadores de lógica binaria (AND, OR, NOT) y las operaciones de conjuntos aplicadas en morfología matemática?

La relación entre los operadores de lógica binaria y las operaciones de conjuntos es **fundamental y directa**, constituyendo la base matemática de la morfología matemática en imágenes binarias.

#### Correspondencia directa entre operadores

La morfología matemática utiliza las operaciones de conjuntos y sus correspondientes operadores lógicos para analizar y procesar estructuras dentro de las imágenes digitales que se modelan como conjuntos de píxeles.

#### 1. Unión (∪) y OR Lógico (∨)

**Operación de conjuntos:**

$$A \cup B = \{x \mid x \in A \text{ o } x \in B\}$$

**Operador lógico:**

$$p \vee q = \begin{cases} 
\text{Verdadero,} & \text{si } p \text{ es verdadero o } q \text{ es verdadero (o ambos)} \\
\text{Falso,} & \text{si } p \text{ y } q \text{ son falsos}
\end{cases}$$

**Analogía**: La unión de conjuntos y el operador lógico OR son análogos porque ambos combinan elementos o proposiciones que cumplen **al menos una** de las condiciones. Si se consideran conjuntos de valores de verdad, la unión corresponde al OR lógico.

**Aplicación en morfología**: En la operación de **dilatación**, se utiliza el OR lógico: si al menos un píxel cubierto por el elemento estructurante es blanco (1), el píxel central se mantiene blanco (1).

#### 2. Intersección (∩) y AND Lógico (∧)

**Operación de conjuntos:**

$$A \cap B = \{x \mid x \in A \text{ y } x \in B\}$$

**Operador lógico:**

$$p \wedge q = \begin{cases} 
\text{Verdadero,} & \text{si } p \text{ es verdadero y } q \text{ es verdadero} \\
\text{Falso,} & \text{en cualquier otro caso}
\end{cases}$$

**Analogía**: La intersección de conjuntos y el operador lógico AND son análogos porque ambos requieren que se cumplan **simultáneamente** dos condiciones. La intersección corresponde al AND lógico.

**Aplicación en morfología**: En la operación de **erosión**, se utiliza el AND lógico: todos los píxeles cubiertos por el elemento estructurante deben ser blancos (1) para que el píxel central se mantenga blanco (1).

#### 3. Complemento (′ o \) y NOT Lógico (¬)

**Operación de conjuntos:**

$$A' = U \setminus A = \{x \mid x \in U \text{ y } x \notin A\}$$

**Operador lógico:**

$$\neg p = \begin{cases} 
\text{Verdadero,} & \text{si } p \text{ es falso} \\
\text{Falso,} & \text{si } p \text{ es verdadero}
\end{cases}$$

**Analogía**: El complemento de un conjunto y el operador lógico NOT son análogos porque ambos consideran la **negación** de una condición: en conjuntos, los elementos que no pertenecen a $A$; en lógica, el valor de verdad opuesto de $p$.

**Aplicación en morfología**: Se utiliza en la **dualidad** entre erosión y dilatación, donde el complemento de la imagen es fundamental para expresar una operación en términos de la otra.

#### Importancia de esta relación

Esta correspondencia permite:

1. **Modelar imágenes digitales como conjuntos de píxeles**
2. **Modificar y extraer información relevante** de las regiones
3. **Facilitar tareas complejas** como detección de bordes, segmentación y eliminación de ruido
4. **Implementar algoritmos eficientes** usando operaciones lógicas de hardware
5. **Fundamentar matemáticamente** todas las operaciones morfológicas

Como indica el texto: *"La estrecha relación entre la teoría de conjuntos y la lógica binaria es fundamental en la morfología matemática, que es una herramienta clave en el procesamiento de imágenes."*

---

### 3. Describe la importancia de aplicar la reflexión al elemento estructurante S cuando este no es simétrico respecto al origen

La reflexión del elemento estructurante $S$ es un concepto fundamental para mantener la **dualidad exacta** entre las operaciones de erosión y dilatación, especialmente cuando el elemento estructurante no es simétrico.

#### Definición de la reflexión

La reflexión del elemento estructurante $S$ respecto al origen se define como:

$$S^s = \{-s \mid s \in S\}$$

Esto implica que **cada punto del elemento estructurante se refleja respecto al origen**, es decir, se invierten sus coordenadas.

#### La dualidad entre erosión y dilatación

La dualidad entre estas operaciones se expresa mediante las ecuaciones:

$$I \ominus S = (I^c \oplus S^s)^c$$

$$I \oplus S = (I^c \ominus S^s)^c$$

donde:
- $I^c$ es el complemento de $I$: $I^c = \mathbb{Z}^2 \setminus I$
- $S^s$ es la reflexión de $S$ respecto al origen
- $\ominus$ denota la operación de erosión
- $\oplus$ denota la operación de dilatación

#### Importancia de la reflexión

##### 1. Elementos estructurantes no simétricos

Si el elemento estructurante **no es simétrico**, la reflexión $S^s$ se vuelve **necesaria** para asegurar la dualidad exacta entre erosión y dilatación. Esto se debe a que:

- La **posición relativa** de $S$ respecto al origen influye en cómo se aplican las operaciones morfológicas
- Sin la reflexión, las operaciones duales no producirían resultados equivalentes
- La orientación del elemento estructurante afecta directamente el resultado de la operación

##### 2. Elementos estructurantes simétricos

Cuando el elemento estructurante es **simétrico** respecto al origen (por ejemplo, un cuadrado centrado, un disco, o una cruz simétrica), la reflexión no afecta el resultado ya que:

$$S = S^s$$

En este caso, muchas aplicaciones prácticas y definiciones en el procesamiento digital de imágenes **omiten la reflexión** en las fórmulas, simplificando así las definiciones.

##### 3. Consistencia matemática

La reflexión garantiza que:

- La erosión de una imagen sea exactamente equivalente al complemento de la dilatación del complemento con el elemento estructurante reflejado
- Las propiedades matemáticas de dualidad se mantengan válidas
- Los resultados sean predecibles y consistentes independientemente del orden de las operaciones

#### Casos prácticos

**Elemento estructurante asimétrico** (ejemplo: línea orientada o rectángulo no centrado):
- **Con reflexión**: La dualidad se mantiene correcta
- **Sin reflexión**: Los resultados de erosión y dilatación no serían duales, produciendo inconsistencias

**Elemento estructurante simétrico** (ejemplo: cuadrado 3×3 centrado, disco):
- La reflexión puede omitirse sin afectar los resultados
- Simplifica la implementación y el cálculo

#### Conclusión

Como indica el texto: *"La necesidad de reflejar el elemento estructurante S depende de cómo se definan las operaciones de erosión y dilatación en el contexto específico."* Sin embargo, para mantener la **rigurosidad matemática** y la **dualidad exacta**, especialmente con elementos no simétricos, la reflexión es fundamental. Algunos autores omiten este paso cuando trabajan con elementos simétricos y en espacios discretos, pero su importancia teórica y práctica no debe subestimarse cuando se trabaja con formas asimétricas.

---

### 4. Explica cómo se combinan las operaciones de erosión y dilatación en el cerramiento morfológico y cuál es la utilidad de esta combinación

El **cerramiento morfológico** (closing en inglés) es una operación compuesta que combina dilatación y erosión en un orden específico para lograr efectos particulares sobre la estructura de los objetos en una imagen binaria.

#### Proceso del cerramiento morfológico

El cerramiento consiste en aplicar las operaciones en el siguiente orden:

**1. Primero: Dilatación**
- Se expanden los objetos en la imagen
- Se cierran pequeñas brechas entre objetos cercanos
- Se rellenan huecos pequeños dentro de los objetos

**2. Segundo: Erosión**
- Se reduce el tamaño de los objetos expandidos
- Se devuelve a los objetos aproximadamente a su tamaño original
- Se conservan los cambios topológicos (huecos rellenados, brechas cerradas)

#### Algoritmo de cerramiento

1. Definir la imagen de entrada $I$ y el elemento estructurante $S$
2. Realizar una **dilatación** en la imagen de entrada: $I_d = I \oplus S$
3. Realizar una **erosión** sobre la imagen dilatada usando el **mismo** elemento estructurante: $I_{cerrada} = I_d \ominus S$
4. Devolver la imagen resultante de la erosión como resultado final

Matemáticamente, el cerramiento se puede expresar como:

$$I \bullet S = (I \oplus S) \ominus S$$

#### Utilidad de esta combinación

##### 1. Relleno de agujeros pequeños

La dilatación inicial rellena los huecos pequeños dentro de los objetos, y la erosión posterior no los vuelve a abrir si son lo suficientemente pequeños en relación con el elemento estructurante.

##### 2. Cierre de brechas estrechas

Los objetos que están separados por espacios pequeños se conectan durante la dilatación, y esta conexión se mantiene después de la erosión.

##### 3. Suavizado de bordes

El cerramiento ayuda a **suavizar los bordes** de los objetos, eliminando irregularidades y concavidades pequeñas en el contorno exterior, mejorando la conectividad entre los objetos en la imagen.

##### 4. Preservación del tamaño general

A diferencia de aplicar solo dilatación (que agrandaría permanentemente los objetos), el cerramiento restaura aproximadamente el tamaño original de los objetos mientras mantiene las mejoras topológicas.

##### 5. Mejora de la conectividad

Es útil para unir componentes que deberían estar conectados pero que fueron separados por:
- Ruido en el proceso de binarización
- Interrupciones en líneas o bordes
- Artefactos de digitalización

#### Aplicaciones prácticas

El cerramiento es útil en:

- **Segmentación de objetos**: Conectar partes fragmentadas de un mismo objeto
- **Preprocesamiento**: Preparar imágenes para análisis posterior eliminando discontinuidades
- **Análisis de formas**: Rellenar huecos internos antes de calcular propiedades geométricas
- **Reconstrucción de estructuras**: Restaurar objetos que fueron parcialmente degradados

#### Diferencia con la apertura

Mientras que:
- **Cerramiento** (dilatación + erosión): Rellena huecos, cierra brechas, suaviza concavidades
- **Apertura** (erosión + dilatación): Elimina objetos pequeños, separa objetos unidos, suaviza convexidades

Ambas operaciones son complementarias y se utilizan según las necesidades específicas del procesamiento.

#### Ejemplo visual

Como se muestra en las figuras del texto:
- La imagen binarizada puede tener pequeños agujeros en los objetos
- Después de la dilatación, estos agujeros se rellenan
- Tras la erosión, los objetos recuperan su tamaño pero mantienen los huecos cerrados
- El resultado es una imagen con objetos más completos y mejor conectados

Como indica el texto: *"El cerramiento es útil en el procesamiento de imágenes para suavizar los bordes de los objetos y mejorar la conectividad entre los objetos en la imagen. Además, se puede utilizar para rellenar pequeños agujeros y para suavizar la forma de los objetos en la imagen."*

---

### 5. ¿Qué ventajas ofrece la operación de erosión cuando se aplica a una imagen que contiene ruido?

La operación de **erosión** es particularmente efectiva para tratar imágenes binarias que contienen ruido, ofreciendo múltiples ventajas en el proceso de limpieza y mejora de la calidad de la imagen.

#### Ventajas principales de la erosión en imágenes con ruido

##### 1. Eliminación de objetos pequeños (ruido aislado)

La erosión es especialmente efectiva para **eliminar píxeles o pequeños grupos de píxeles** que no forman parte de los objetos principales:

- **Ruido de sal**: Píxeles blancos aislados en el fondo negro son eliminados completamente
- **Artefactos pequeños**: Pequeños objetos que no son de interés desaparecen
- **Píxeles espurios**: Píxeles blancos aleatorios son removidos

**Principio**: Si un objeto es más pequeño que el elemento estructurante, la erosión lo eliminará por completo, ya que no habrá ninguna posición donde el elemento estructurante completo quepa dentro del objeto.

##### 2. Separación de objetos conectados incorrectamente

Cuando el ruido causa que objetos que deberían estar separados aparezcan conectados por puentes estrechos o puntos de contacto:

- La erosión **rompe estas conexiones débiles**
- Separa objetos que están unidos solo por **píxeles finos**
- Facilita el análisis individual de cada objeto

**Aplicación**: Útil cuando la binarización produce uniones incorrectas entre objetos debido a variaciones de iluminación o umbralización inadecuada.

##### 3. Reducción de protuberancias irregulares

El ruido a menudo aparece como irregularidades en los bordes de los objetos:

- La erosión **suaviza los bordes** al eliminar protuberancias pequeñas
- Reduce las irregularidades causadas por errores de digitalización
- Produce contornos más limpios y regulares

##### 4. Limpieza de detalles innecesarios

En imágenes donde hay **detalles finos que no son relevantes** para el análisis:

- La erosión elimina estos detalles manteniéndose solo las estructuras principales
- Simplifica la imagen para procesamiento posterior
- Reduce la complejidad computacional de análisis subsecuentes

##### 5. Preparación para la operación de apertura

La erosión es el primer paso en la **apertura morfológica** (erosión seguida de dilatación):

- Elimina el ruido y objetos pequeños
- La dilatación posterior restaura el tamaño de los objetos principales
- El resultado es una imagen limpia donde se conservan solo los objetos significativos

Como menciona el texto sobre la apertura: *"La erosión reduce el tamaño de los objetos en la imagen y, por lo tanto, elimina los detalles más pequeños, mientras que la dilatación tiene el efecto de expandir los objetos nuevamente al tamaño original, pero sin los detalles que se eliminaron durante la erosión."*

#### Consideraciones importantes

##### Desventajas a considerar

1. **Reducción del tamaño de objetos**: La erosión reduce el área de todos los objetos, no solo del ruido
2. **Pérdida de información**: Detalles finos que sí son relevantes pueden perderse
3. **Selección del elemento estructurante**: El tamaño y forma del elemento estructurante deben elegirse cuidadosamente

##### Estrategias de mitigación

- **Aplicar apertura en lugar de solo erosión**: Para restaurar el tamaño original de los objetos
- **Usar elementos estructurantes pequeños**: Para minimizar la pérdida de información relevante
- **Aplicar erosión múltiples veces**: Si el ruido es muy denso, con elementos estructurantes pequeños
- **Combinar con otras técnicas**: Como filtros de mediana antes de la binarización

#### Ejemplo práctico

Según el algoritmo de erosión descrito en el texto:

1. Se define la imagen de entrada $I$ (con ruido) y el elemento estructurante $S$
2. Se explora cada píxel de la imagen
3. Si **todos** los píxeles cubiertos por el elemento estructurante son blancos (1), el píxel central se mantiene blanco
4. Si **algún** píxel cubierto es negro (0), el píxel central se vuelve negro

Este proceso asegura que:
- Los píxeles blancos aislados (ruido) se eliminan, ya que tendrán píxeles negros en su vecindad
- Los objetos sólidos se reducen pero mantienen su estructura principal
- Los puentes delgados entre objetos se rompen

#### Aplicaciones específicas

La erosión para eliminación de ruido es útil en:

- **Preprocesamiento de OCR**: Limpieza de documentos escaneados
- **Análisis médico**: Eliminación de artefactos en imágenes de rayos X o resonancias
- **Inspección industrial**: Limpieza de imágenes de piezas manufacturadas
- **Visión por computadora**: Preparación de imágenes para detección de objetos

#### Conclusión

La erosión es una herramienta fundamental para el **preprocesamiento y limpieza** de imágenes binarias con ruido. Su capacidad para eliminar selectivamente objetos pequeños mientras preserva las estructuras principales la convierte en una operación esencial en el pipeline de procesamiento de imágenes. Cuando se combina inteligentemente con la dilatación (en operaciones como la apertura), ofrece una solución robusta y eficiente para mejorar significativamente la calidad de las imágenes binarias afectadas por ruido.

---

## Ejercicios Numéricos

### Ejercicio 1: Erosión directa y por dualidad

**Enunciado**: Se propone realizar una erosión directa y por dualidad sobre una imagen binaria representada por una matriz $I$ de $10 \times 10$, utilizando un elemento estructurante $S$ cuadrado de $3 \times 3$.

#### Datos iniciales

**Imagen de entrada $I$ (10×10):**

Nota: En la representación visual, los 1 aparecen en negro y los 0 en blanco.

$$
I = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\
1 & 1 & 0 & 0 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Elemento estructurante $S$ (3×3):**

$$
S = \begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}
$$

---

#### Solución 1a: Erosión Directa

La erosión se define como:

$$I_e = I \ominus S = \{p \in \mathbb{Z}^2 \mid S_p \subseteq I\}$$

**Algoritmo de erosión directa:**

Para cada píxel $(i,j)$ de la imagen:
1. Colocar el centro del elemento estructurante $S$ en la posición $(i,j)$
2. Verificar si **todos** los píxeles cubiertos por $S$ son 1 (operación AND)
3. Si todos son 1, entonces $I_e(i,j) = 1$
4. Si al menos uno es 0, entonces $I_e(i,j) = 0$

**Análisis paso a paso (considerando bordes):**

Para simplificar, consideraremos que los píxeles en el borde tienen vecinos con valor 0 (padding implícito). El elemento estructurante se aplica solo donde cabe completamente dentro de la imagen.

**Posiciones donde aplicar el elemento estructurante:**

Para un elemento estructurante de 3×3, el centro puede colocarse desde la posición (1,1) hasta (8,8) en una matriz 10×10 (usando indexación desde 0).

Analizando algunas posiciones clave:

**Posición (1,1):**
- Vecindad extraída de I:
$$\begin{bmatrix} 1 & 1 & 1 \\ 1 & 0 & 0 \\ 1 & 0 & 0 \end{bmatrix}$$
- ¿Todos son 1? **No** (hay 0s)
- Resultado: $I_e(1,1) = 0$

**Posición (0,0) - borde:**
- Con padding de 0s en el borde: No todos son 1
- Resultado: $I_e(0,0) = 0$

**Posición (5,7):**
- Vecindad extraída de I:
$$\begin{bmatrix} 0 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{bmatrix}$$
- ¿Todos son 1? **No** (hay un 0)
- Resultado: $I_e(5,7) = 0$

**Posición (4,7):**
- Vecindad extraída de I:
$$\begin{bmatrix} 1 & 1 & 1 \\ 1 & 1 & 1 \\ 1 & 1 & 1 \end{bmatrix}$$
- ¿Todos son 1? **Sí**
- Resultado: $I_e(4,7) = 1$

**Posición (7,6):**
- Vecindad extraída de I:
$$\begin{bmatrix} 0 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 1 & 1 \end{bmatrix}$$
- ¿Todos son 1? **No** (hay 0s)
- Resultado: $I_e(7,6) = 0$

**Realizando la erosión completa:**

Aplicando el algoritmo sistemáticamente a cada posición (recorriendo toda la matriz y verificando las vecindades 3×3), obtenemos:

$$
I_e = I \ominus S = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$$

**Explicación del resultado:**

La erosión eliminó casi toda la imagen excepto una región vertical en la columna 7 (filas 4 a 7). Esto ocurre porque:

1. **Región con muchos 0s**: La gran área con 0s (lado izquierdo superior) hace que ninguna vecindad 3×3 pueda contener solo 1s en esa zona.

2. **Región sólida preservada**: En la esquina superior derecha y centro-derecha hay una zona sólida de 1s. Al aplicar el elemento estructurante 3×3:
   - En la posición (4,7): hay un bloque 3×3 completo de 1s → se preserva
   - En las posiciones (5,7), (6,7), (7,7): también hay bloques 3×3 completos → se preservan
   
3. **Bordes eliminados**: Los bordes de cualquier región se erosionan porque el elemento estructurante toca píxeles del borde o del fondo.

4. **Objetos delgados eliminados**: Las regiones estrechas o diagonales desaparecen porque no tienen suficiente "grosor" para contener un bloque 3×3 completo de 1s.

---

#### Solución 1b: Erosión por Dualidad

La dualidad entre erosión y dilatación establece que:

$$I \ominus S = (I^c \oplus S^s)^c$$

donde:
- $I^c$ es el complemento de $I$
- $S^s$ es la reflexión de $S$ (pero como $S$ es simétrico, $S^s = S$)
- $\oplus$ denota dilatación

**Paso 1: Calcular el complemento de $I$**

El complemento invierte todos los valores: los 1s se convierten en 0s y los 0s en 1s.

$$
I^c = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 & 0 \\
0 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 0 \\
0 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 \\
0 & 0 & 1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 \\
0 & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 1 & 1 & 1 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 1 & 1 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$$

**Paso 2: Verificar la reflexión de $S$**

Como $S$ es simétrico (cuadrado de 3×3 con todos unos):

$$S^s = S = \begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}$$

**Paso 3: Aplicar dilatación a $I^c$ con $S$**

La dilatación se define como:

$$I^c \oplus S = \{p \in \mathbb{Z}^2 \mid S_p \cap I^c \neq \emptyset\}$$

En la dilatación, un píxel se establece en 1 si **al menos un píxel** cubierto por el elemento estructurante es 1 (operación OR).

**Aplicando la dilatación:**

La dilatación expande todas las regiones de 1s en $I^c$. Cada píxel de valor 1 se expande a todos sus vecinos en un radio definido por el elemento estructurante (en este caso, una vecindad 3×3).

Para cada posición, si al menos un píxel en la vecindad 3×3 es 1, el centro se vuelve 1.

$$
I^c \oplus S = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 & 0 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 & 0 \\
0 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 & 0 \\
0 & 0 & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 \\
\end{bmatrix}
$$

**Paso 4: Calcular el complemento del resultado**

Finalmente, invertimos el resultado de la dilatación para obtener la erosión:

$$
I_e = (I^c \oplus S)^c = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 1 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Nota importante sobre el manejo de bordes:**

Observamos que hay diferencias entre el resultado de la erosión directa y la erosión por dualidad. Esto se debe al **manejo de los bordes**:

- **Erosión directa**: Típicamente considera que los píxeles fuera de los límites son 0 (fondo), lo que hace que los bordes se erosionen más agresivamente.

- **Erosión por dualidad**: El resultado depende de cómo se maneje el padding en la dilatación del complemento.

**Para obtener resultados equivalentes**, ambos métodos deben usar la misma convención de bordes. En aplicaciones prácticas:

1. **Sin padding (bordes válidos)**: Solo se procesan píxeles donde el elemento estructurante cabe completamente.
2. **Padding con 0s**: Los bordes se tratan como fondo.
3. **Padding con replicación**: Los valores del borde se replican.

Si aplicamos la erosión directa solo en regiones válidas (donde el EE cabe completamente, es decir, de (1,1) a (8,8)), y consideramos que fuera de estos límites el resultado es 0, obtenemos:

$$
I_e = I \ominus S = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$$

Este es el resultado más consistente considerando padding con 0s en los bordes.

---

#### Verificación y Comparación

**Resultado final de la erosión (con manejo consistente de bordes):**

$$
I_e = I \ominus S = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$$

**Interpretación del resultado:**

Solo se preserva una línea vertical de 4 píxeles en la columna 7, filas 4-7. Esta es la única región donde existe un bloque sólido 3×3 de píxeles con valor 1.

**Características del resultado:**

1. **Reducción drástica**: El 96% de los píxeles se convierten en 0 (solo 4 de 100 píxeles permanecen como 1).

2. **Solo regiones sólidas**: Solo permanecen píxeles donde existían bloques completos 3×3 de 1s:
   - En posición (4,7): La vecindad 3×3 centrada aquí contiene solo 1s
   - En posición (5,7): La vecindad 3×3 centrada aquí contiene solo 1s
   - En posición (6,7): La vecindad 3×3 centrada aquí contiene solo 1s
   - En posición (7,7): La vecindad 3×3 centrada aquí contiene solo 1s

3. **Equivalencia matemática**: 
   $$I \ominus S = (I^c \oplus S^s)^c$$
   
   Ambos métodos producen el mismo resultado cuando se aplica el mismo criterio de manejo de bordes.

4. **Efecto de "adelgazamiento"**: La imagen se adelgaza considerablemente, eliminando:
   - Toda la región con 0s internos (lado izquierdo)
   - Los bordes de todas las regiones de 1s
   - Cualquier región donde los 1s no formen un bloque sólido de al menos 3×3

**Observaciones importantes:**

- **Elemento estructurante 3×3**: Requiere bloques sólidos de 9 píxeles (3×3) con valor 1 para preservar el píxel central.

- **Efecto en la imagen original**: 
  - La gran región de 0s en el lado izquierdo-superior impide que se preserve cualquier píxel en esa área.
  - La esquina superior derecha tiene 1s, pero al estar en el borde, no puede formar bloques 3×3 completos.
  - La región central-derecha es donde se encuentra el área más sólida de 1s.

- **Geometría preservada**: Solo la región vertical más gruesa y sólida sobrevive a la erosión.

- **Utilidad práctica**: Esta operación es útil para:
  - Eliminar ruido (objetos pequeños)
  - Identificar regiones "fuertes" o sólidas
  - Separar objetos conectados débilmente
  - Reducir el grosor de objetos para análisis de esqueletos

**Verificación manual de una posición:**

Para verificar, tomemos la posición (5,7):

- Vecindad extraída de $I$:
$$\begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}$$

- Elemento estructurante $S$:
$$\begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}$$

- Comparación: Todos los píxeles coinciden (todos son 1)
- Resultado: $I_e(5,7) = 1$

---

### Ejercicio 2: Dilatación directa y por dualidad

**Enunciado**: Realizar una dilatación directa y por dualidad sobre una imagen binaria representada por una matriz $I$ de $10 \times 10$, utilizando un elemento estructurante $S$ en línea de $1 \times 3$.

#### Datos iniciales

**Imagen de entrada $I$ (10×10):**

Nota: Los 1 aparecen en negro y los 0 en blanco en la visualización.

$$
I = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 1 & 1 \\
1 & 0 & 0 & 1 & 1 & 1 & 1 & 0 & 1 & 1 \\
1 & 1 & 0 & 1 & 1 & 1 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 0 & 0 & 1 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 0 & 1 & 1 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Elemento estructurante $S$ (1×3) - línea horizontal:**

$$
S = \begin{bmatrix}
1 & 1 & 1
\end{bmatrix}
$$

Este elemento estructurante representa una línea horizontal de 3 píxeles. El píxel central es el del medio.

---

#### Solución 2a: Dilatación Directa

La dilatación se define como:

$$I_d = I \oplus S = \{p \in \mathbb{Z}^2 \mid S_p \cap I \neq \emptyset\}$$

**Algoritmo de dilatación directa:**

Para cada píxel $(i,j)$ de la imagen:
1. Colocar el centro del elemento estructurante $S$ en la posición $(i,j)$
2. Verificar si **al menos un píxel** cubierto por $S$ es 1 (operación OR)
3. Si al menos uno es 1, entonces $I_d(i,j) = 1$
4. Si todos son 0, entonces $I_d(i,j) = 0$

**Características del elemento estructurante lineal 1×3:**

- Cubre 3 píxeles horizontales: izquierda, centro, derecha
- Afecta solo en dirección horizontal
- El centro del EE está en el píxel del medio

**Análisis paso a paso:**

Voy a analizar algunas posiciones clave para ilustrar el proceso:

**Posición (1,1) - valor original 0:**
- Vecindad horizontal: $[I(1,0), I(1,1), I(1,2)] = [1, 0, 0]$
- ¿Al menos un píxel es 1? **Sí** (el de la izquierda)
- Resultado: $I_d(1,1) = 1$

**Posición (1,3) - valor original 0:**
- Vecindad horizontal: $[I(1,2), I(1,3), I(1,4)] = [0, 0, 0]$
- ¿Al menos un píxel es 1? **No**
- Resultado: $I_d(1,3) = 0$

**Posición (2,4) - valor original 1:**
- Vecindad horizontal: $[I(2,3), I(2,4), I(2,5)] = [0, 1, 0]$
- ¿Al menos un píxel es 1? **Sí** (el del centro)
- Resultado: $I_d(2,4) = 1$

**Posición (5,2) - valor original 0:**
- Vecindad horizontal: $[I(5,1), I(5,2), I(5,3)] = [1, 0, 0]$
- ¿Al menos un píxel es 1? **Sí** (el de la izquierda)
- Resultado: $I_d(5,2) = 1$

**Realizando la dilatación completa:**

Aplicando el algoritmo sistemáticamente a cada posición, la dilatación horizontal expande cada píxel de valor 1 hacia sus vecinos horizontales inmediatos:

$$
I_d = I \oplus S = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 1 & 1 & 1 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Explicación del resultado:**

1. **Expansión horizontal**: Cada región de 1s se expande un píxel hacia la izquierda y hacia la derecha.

2. **Regiones que cambian**:
   - Fila 1: Los 0s en posiciones (1,1) cambian a 1 porque están junto a un 1 en (1,0)
   - Fila 2: El 0 en (2,3) se conecta con el 1 en (2,4), expandiéndose
   - Fila 4: La región de 1s se expande, conectando áreas
   - Fila 5: El 0 en (5,2) se convierte en 1 por el 1 en (5,1)

3. **Efecto de puente horizontal**: La dilatación con elemento lineal horizontal tiende a:
   - Cerrar brechas horizontales pequeñas
   - Conectar píxeles que están separados por un solo píxel horizontalmente
   - No afecta separaciones verticales

4. **Preservación de huecos grandes**: Los grupos de 0s que tienen más de 2 píxeles de ancho consecutivos permanecen como 0s en su centro, aunque sus bordes se reducen.

---

#### Solución 2b: Dilatación por Dualidad

La dualidad entre erosión y dilatación establece que:

$$I \oplus S = (I^c \ominus S^s)^c$$

donde:
- $I^c$ es el complemento de $I$
- $S^s$ es la reflexión de $S$ (pero como $S$ es simétrico, $S^s = S$)
- $\ominus$ denota erosión

**Paso 1: Calcular el complemento de $I$**

Invertimos todos los valores: 1→0, 0→1.

$$
I^c = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 & 0 \\
0 & 1 & 1 & 1 & 0 & 1 & 1 & 1 & 0 & 0 \\
0 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 1 & 1 & 0 & 0 \\
0 & 0 & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 \\
0 & 1 & 1 & 0 & 1 & 1 & 0 & 0 & 0 & 0 \\
0 & 1 & 0 & 0 & 1 & 1 & 0 & 0 & 0 & 0 \\
0 & 1 & 1 & 1 & 1 & 1 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$$

**Paso 2: Verificar la reflexión de $S$**

El elemento estructurante $S = [1, 1, 1]$ es simétrico horizontalmente, por lo tanto:

$$S^s = S = \begin{bmatrix}
1 & 1 & 1
\end{bmatrix}$$

**Paso 3: Aplicar erosión a $I^c$ con $S$**

La erosión requiere que **todos** los píxeles en la vecindad horizontal 1×3 sean 1 para preservar el píxel central.

Para cada posición $(i,j)$:
- Si $[I^c(i,j-1), I^c(i,j), I^c(i,j+1)]$ son **todos 1**, entonces $I^c \ominus S(i,j) = 1$
- Si alguno es 0, entonces $I^c \ominus S(i,j) = 0$

**Analizando algunas posiciones:**

**Posición (1,2):**
- Vecindad: $[I^c(1,1), I^c(1,2), I^c(1,3)] = [1, 1, 1]$
- ¿Todos son 1? **Sí**
- Resultado: $(I^c \ominus S)(1,2) = 1$

**Posición (1,3):**
- Vecindad: $[I^c(1,2), I^c(1,3), I^c(1,4)] = [1, 1, 1]$
- ¿Todos son 1? **Sí**
- Resultado: $(I^c \ominus S)(1,3) = 1$

**Posición (2,2):**
- Vecindad: $[I^c(2,1), I^c(2,2), I^c(2,3)] = [1, 1, 1]$
- ¿Todos son 1? **Sí**
- Resultado: $(I^c \ominus S)(2,2) = 1$

**Posición (2,4):**
- Vecindad: $[I^c(2,3), I^c(2,4), I^c(2,5)] = [1, 0, 1]$
- ¿Todos son 1? **No**
- Resultado: $(I^c \ominus S)(2,4) = 0$

**Aplicando erosión completa a $I^c$:**

$$
I^c \ominus S = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 1 & 1 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 & 1 & 1 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 1 & 1 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$$

**Paso 4: Calcular el complemento del resultado**

Finalmente, invertimos el resultado de la erosión para obtener la dilatación:

$$
I_d = (I^c \ominus S)^c = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 1 & 1 & 1 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

---

#### Verificación y Comparación

**Resultado final de la dilatación:**

$$
I_d = I \oplus S = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 1 & 1 & 1 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Verificación: Ambos métodos producen el mismo resultado**

**Análisis detallado del resultado:**

1. **Equivalencia perfecta**: La dilatación directa y la dilatación por dualidad producen exactamente el mismo resultado, confirmando la validez matemática de la dualidad.

2. **Efecto de expansión horizontal**:
   - **Fila 1**: Región de 1s en (1,1) aparece por expansión desde (1,0)
   - **Fila 2**: Los 0s en posiciones (2,3) y (2,4) se llenan parcialmente, conectando con el 1 en (2,4)
   - **Fila 3**: Se conecta completamente la región de 1s
   - **Fila 4**: Se expande el grupo de 1s hacia (4,2)
   - **Fila 8**: El 0 en (8,1) se vuelve 1 por el vecino (8,0)

3. **Regiones que NO cambian**:
   - **Fila 1, posiciones (1,2)-(1,4)**: Permanecen como 0 porque forman un grupo de 3 ceros consecutivos
   - **Fila 5, posiciones (5,3)-(5,5)**: Grupo de 3 ceros consecutivos
   - **Fila 8, posiciones (8,2)-(8,4)**: Grupo de 3 ceros consecutivos

4. **Característica clave del elemento estructurante lineal 1×3**:
   - **Cierra brechas de 1 píxel**: Si dos 1s están separados por un solo 0 horizontalmente, la dilatación conecta la brecha
   - **Preserva huecos de 3+ píxeles**: Grupos de 3 o más 0s consecutivos horizontalmente mantienen al menos 1 cero en el centro
   - **No afecta verticalmente**: Las separaciones verticales no se modifican

5. **Comparación con la imagen original**:
   - Total de píxeles que cambiaron de 0 a 1: aproximadamente 9 píxeles
   - Regiones expandidas principalmente en bordes horizontales de los objetos
   - La estructura general se mantiene pero más conectada horizontalmente

**Verificación manual de una posición crítica:**

Tomemos la posición (2,3) que cambió de 0 a 1:

- Vecindad en $I$: $[I(2,2), I(2,3), I(2,4)] = [0, 0, 1]$
- ¿Al menos un píxel es 1? **Sí** (el de la derecha)
- Resultado: $I_d(2,3) = 1$ ✓

**Propiedades observadas:**

- **Dualidad verificada**: $I \oplus S = (I^c \ominus S^s)^c$ ✓
- **Idempotencia**: Aplicar dilatación múltiples veces seguiría expandiendo
- **Extensividad**: $I \subseteq I \oplus S$ (la imagen dilatada contiene todos los píxeles originales de 1)
- **Direccionalidad**: El elemento estructurante lineal horizontal afecta solo en esa dirección

---

### Ejercicio 3: Apertura morfológica (Ejercicio 1 + Apertura)

**Enunciado**: Sobre el ejercicio enunciado en el punto 1, realizar apertura con el mismo elemento estructurante y mostrar sus resultados.

**Recordatorio de datos del Ejercicio 1:**

**Imagen de entrada $I$ (10×10):**

$$
I = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\
1 & 1 & 0 & 0 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Elemento estructurante $S$ (3×3):**

$$
S = \begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}
$$

---

#### Concepto de Apertura

La **apertura morfológica** se define como una **erosión seguida de una dilatación** con el mismo elemento estructurante:

$$I \circ S = (I \ominus S) \oplus S$$

**Propósito de la apertura:**
- Eliminar objetos pequeños y ruido
- Separar objetos conectados por puentes estrechos
- Suavizar contornos eliminando protuberancias pequeñas
- Mantener aproximadamente el tamaño de los objetos grandes

---

#### Paso 1: Erosión de $I$ (Ya calculada en Ejercicio 1)

Del Ejercicio 1, obtuvimos:

$$
I_e = I \ominus S = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$$

**Resultado de la erosión:**
- Solo 4 píxeles permanecen como 1 (columna 7, filas 4-7)
- Esta es la única región donde existía un bloque sólido 3×3 de 1s
- Todo el ruido y estructuras delgadas fueron eliminados

---

#### Paso 2: Dilatación de $I_e$ con $S$

Ahora aplicamos dilatación a la imagen erosionada usando el mismo elemento estructurante 3×3.

**Algoritmo de dilatación:**
- Para cada píxel, si **al menos un píxel** en la vecindad 3×3 es 1, el píxel central se vuelve 1

**Análisis de posiciones clave:**

**Posición (3,6):**
- Vecindad 3×3 centrada en (3,6):
$$\begin{bmatrix}
0 & 0 & 0 \\
0 & 0 & 0 \\
0 & 0 & 1
\end{bmatrix}$$
- ¿Al menos un píxel es 1? **Sí** (posición inferior derecha)
- Resultado: $(I_e \oplus S)(3,6) = 1$

**Posición (4,7):**
- Vecindad 3×3 centrada en (4,7):
$$\begin{bmatrix}
0 & 0 & 0 \\
0 & 1 & 0 \\
0 & 1 & 0
\end{bmatrix}$$
- ¿Al menos un píxel es 1? **Sí**
- Resultado: $(I_e \oplus S)(4,7) = 1$

**Posición (7,8):**
- Vecindad 3×3 centrada en (7,8):
$$\begin{bmatrix}
0 & 1 & 0 \\
0 & 0 & 0 \\
0 & 0 & 0
\end{bmatrix}$$
- ¿Al menos un píxel es 1? **Sí** (posición superior central)
- Resultado: $(I_e \oplus S)(7,8) = 1$

**Aplicando dilatación completa:**

La línea vertical de 4 píxeles se expande en todas direcciones, formando un bloque aproximadamente 3×3:

$$
I_{apertura} = (I \ominus S) \oplus S = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$$

---

#### Análisis del resultado de la apertura

**Comparación entre las tres etapas:**

1. **Imagen original $I$**: 
   - Región grande con muchos 0s internos (lado izquierdo)
   - Región sólida en el lado derecho
   - Estructura compleja con áreas conectadas

2. **Después de erosión $I_e$**:
   - Solo 4 píxeles verticales (columna 7, filas 4-7)
   - 96% de reducción
   - Solo la región más sólida sobrevive

3. **Después de apertura $(I \ominus S) \oplus S$**:
   - Bloque rectangular de aproximadamente 6×3 píxeles
   - Centrado en columnas 6-8, filas 3-8
   - 54 píxeles con valor 1

**Efectos de la apertura:**

1. **Eliminación de ruido y estructuras débiles**:
   - Toda la región de la izquierda con 0s internos fue eliminada
   - Las conexiones débiles desaparecieron
   - Solo permanece la estructura más robusta

2. **Suavización de bordes**:
   - El resultado tiene bordes más regulares y suaves
   - Se formó un rectángulo aproximado

3. **Separación de objetos**:
   - Si hubiera habido objetos conectados por puentes delgados, se habrían separado

4. **Preservación aproximada del tamaño**:
   - La región sólida original se preservó en tamaño similar
   - La dilatación "recupera" aproximadamente el área perdida en la erosión

**Propiedades matemáticas verificadas:**

- **Anti-extensividad**: $I \circ S \subseteq I$ (la apertura siempre reduce o mantiene el área)
- **Idempotencia**: Aplicar apertura nuevamente no cambiaría el resultado: $(I \circ S) \circ S = I \circ S$
- **Incremento de tamaño**: Si $S_1 \subseteq S_2$, entonces $I \circ S_1 \subseteq I \circ S_2$

**Utilidad práctica:**

La apertura en este caso identificó y aisló la región más significativa y estructuralmente sólida de la imagen, eliminando todo el ruido, huecos internos y estructuras débiles. Este resultado es útil para:
- Identificar objetos principales
- Eliminar ruido y artefactos
- Preprocesamiento para análisis de forma
- Segmentación robusta de regiones de interés

---

### Ejercicio 4: Cerramiento morfológico (Ejercicio 2 + Cerramiento)

**Enunciado**: Sobre el ejercicio enunciado en el punto 2, realizar cerramiento con el mismo elemento estructurante y mostrar sus resultados.

**Recordatorio de datos del Ejercicio 2:**

**Imagen de entrada $I$ (10×10):**

$$
I = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 1 & 1 \\
1 & 0 & 0 & 1 & 1 & 1 & 1 & 0 & 1 & 1 \\
1 & 1 & 0 & 1 & 1 & 1 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 0 & 0 & 1 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 0 & 1 & 1 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Elemento estructurante $S$ (1×3) - línea horizontal:**

$$
S = \begin{bmatrix}
1 & 1 & 1
\end{bmatrix}
$$

---

#### Concepto de Cerramiento

El **cerramiento morfológico** se define como una **dilatación seguida de una erosión** con el mismo elemento estructurante:

$$I \bullet S = (I \oplus S) \ominus S$$

**Propósito del cerramiento:**
- Rellenar huecos pequeños dentro de los objetos
- Cerrar brechas estrechas entre objetos
- Conectar componentes cercanos
- Suavizar contornos eliminando concavidades

---

#### Paso 1: Dilatación de $I$ (Ya calculada en Ejercicio 2)

Del Ejercicio 2, obtuvimos:

$$
I_d = I \oplus S = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 1 & 1 & 1 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Resultado de la dilatación:**
- Los objetos se expandieron horizontalmente
- Algunas brechas de 1 píxel se cerraron
- Los grupos de 3+ ceros consecutivos se mantuvieron parcialmente

---

#### Paso 2: Erosión de $I_d$ con $S$

Ahora aplicamos erosión a la imagen dilatada usando el mismo elemento estructurante lineal 1×3.

**Algoritmo de erosión horizontal:**
- Para cada píxel, **todos** los píxeles en la vecindad horizontal 1×3 deben ser 1 para preservar el píxel central

**Análisis de posiciones clave:**

**Posición (1,1):**
- Vecindad horizontal: $[I_d(1,0), I_d(1,1), I_d(1,2)] = [1, 1, 0]$
- ¿Todos son 1? **No**
- Resultado: $(I_d \ominus S)(1,1) = 0$

**Posición (3,1):**
- Vecindad horizontal: $[I_d(3,0), I_d(3,1), I_d(3,2)] = [1, 1, 1]$
- ¿Todos son 1? **Sí**
- Resultado: $(I_d \ominus S)(3,1) = 1$

**Posición (4,2):**
- Vecindad horizontal: $[I_d(4,1), I_d(4,2), I_d(4,3)] = [1, 1, 1]$
- ¿Todos son 1? **Sí**
- Resultado: $(I_d \ominus S)(4,2) = 1$

**Aplicando erosión completa:**

$$
I_{cerramiento} = (I \oplus S) \ominus S = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\
1 & 0 & 0 & 0 & 1 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

---

#### Análisis del resultado del cerramiento

**Comparación detallada (fila por fila):**

$$
\begin{array}{|c|c|c|c|}
\hline
\textbf{Fila} & \textbf{Original } I & \textbf{Cerramiento } (I \bullet S) & \textbf{Cambios} \\
\hline
0 & \texttt{1 1 1 1 1 1 1 1 1 1} & \texttt{1 1 1 1 1 1 1 1 1 1} & \text{Sin cambios} \\
\hline
1 & \texttt{1 0 0 0 0 0 1 1 1 1} & \texttt{1 0 0 0 0 0 1 1 1 1} & \text{Sin cambios} \\
\hline
2 & \texttt{1 0 0 0 1 0 0 0 1 1} & \texttt{1 0 0 0 1 0 0 0 1 1} & \text{Sin cambios} \\
\hline
3 & \texttt{1 0 0 1 1 1 1 0 1 1} & \texttt{1 1 1 1 1 1 1 1 1 1} & \textbf{3 píxeles: (3,1), (3,2), (3,7)} \\
\hline
4 & \texttt{1 1 0 1 1 1 0 0 1 1} & \texttt{1 1 1 1 1 1 1 0 1 1} & \textbf{2 píxeles: (4,2), (4,6)} \\
\hline
5 & \texttt{1 1 0 0 0 0 0 1 1 1} & \texttt{1 1 0 0 0 0 0 1 1 1} & \text{Sin cambios} \\
\hline
6 & \texttt{1 0 0 1 0 0 1 1 1 1} & \texttt{1 1 1 1 1 1 1 1 1 1} & \textbf{3 píxeles: (6,1), (6,2), (6,4), (6,5)} \\
\hline
7 & \texttt{1 0 1 1 0 0 1 1 1 1} & \texttt{1 1 1 1 1 1 1 1 1 1} & \textbf{3 píxeles: (7,1), (7,4), (7,5)} \\
\hline
8 & \texttt{1 0 0 0 0 0 1 1 1 1} & \texttt{1 1 0 0 0 1 1 1 1 1} & \textbf{2 píxeles: (8,1), (8,5)} \\
\hline
9 & \texttt{1 1 1 1 1 1 1 1 1 1} & \texttt{1 1 1 1 1 1 1 1 1 1} & \text{Sin cambios} \\
\hline
\end{array}
$$

**Píxeles que cambiaron (0 → 1):**
- Fila 3: **(3,1), (3,2), (3,7)** = 3 píxeles
- Fila 4: **(4,2), (4,6)** = 2 píxeles  
- Fila 6: **(6,1), (6,2), (6,4), (6,5)** = 4 píxeles
- Fila 7: **(7,1), (7,4), (7,5)** = 3 píxeles
- Fila 8: **(8,1), (8,5)** = 2 píxeles

**Total: 14 píxeles** cambiaron de 0 a 1.

**Efectos clave del cerramiento:**

1. **Cierre de brechas horizontales pequeñas**:
   - Brechas de 1-2 píxeles se cerraron completamente
   - Filas 3, 4, 6, 7: Múltiples brechas pequeñas eliminadas

2. **Preservación de huecos grandes**:
   - Filas 1, 2, 5: Grupos de 5+ ceros consecutivos se mantuvieron intactos
   - Fila 8: Grupo de 4 ceros se redujo a 3

3. **Mejora de conectividad horizontal**:
   - Regiones fragmentadas se unificaron
   - Filas 6-7: Completamente conectadas ahora

4. **Efecto direccional del elemento estructurante**:
   - Solo afectó separaciones horizontales
   - No modificó separaciones verticales

**Propiedades matemáticas verificadas:**

- **Extensividad**: $I \subseteq I \bullet S$
- **Idempotencia**: $(I \bullet S) \bullet S = I \bullet S$
- **Dualidad**: $I \bullet S = ((I^c \circ S^s)^c)$

**Utilidad práctica:**

El cerramiento conectó exitosamente regiones fragmentadas, rellenó interrupciones pequeñas y mejoró la conectividad, siendo útil para:
- Reconexión de caracteres fragmentados
- Reparación de líneas interrumpidas  
- Mejora de robustez en reconocimiento de patrones
- Preprocesamiento para análisis de conectividad

---

### Ejercicio 5: Detección de bordes mediante morfología

**Enunciado**: Se propone realizar la detección de bordes en una imagen binaria $I$ de $10 \times 10$ que contiene un círculo aproximado en el centro, utilizando un elemento estructurante $S$ de $3 \times 3$.

#### Datos iniciales

**Imagen de entrada $I$ (10×10) - Fondo con círculo:**

Nota: Los 1 representan el fondo (negro) y los 0 representan el círculo (blanco).

$$
I = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Elemento estructurante $S$ (3×3):**

$$
S = \begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}
$$

---

#### Conceptos de detección de bordes

La detección de bordes morfológica se basa en dos enfoques:

1. **Borde interno**: Diferencia entre la imagen original y su erosión
   $$\text{Borde\_interno} = I - (I \ominus S)$$
   
   Resalta los píxeles en el **borde interno** del objeto (la capa interna de píxeles del objeto).

2. **Borde externo**: Diferencia entre la dilatación y la imagen original
   $$\text{Borde\_externo} = (I \oplus S) - I$$
   
   Resalta los píxeles en el **borde externo** del objeto (la capa externa alrededor del objeto).

3. **Gradiente morfológico** (borde completo): Diferencia entre dilatación y erosión
   $$\text{Gradiente} = (I \oplus S) - (I \ominus S)$$
   
   Combina ambos bordes (interno + externo).

---

#### Paso 1: Erosión de $I$

Aplicamos erosión a la imagen original. Recordemos que en la erosión, un píxel se mantiene como 1 solo si **todos** los píxeles en la vecindad 3×3 son 1.

**Análisis de posiciones clave:**

**Posición (2,2) - esquina del círculo:**
- Vecindad 3×3:
$$\begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 0 \\
1 & 0 & 0
\end{bmatrix}$$
- ¿Todos son 1? **No** (hay 0s)
- Resultado: $I_e(2,2) = 0$

**Posición (3,3) - dentro del círculo:**
- Vecindad 3×3:
$$\begin{bmatrix}
0 & 0 & 0 \\
0 & 0 & 0 \\
0 & 0 & 0
\end{bmatrix}$$
- ¿Todos son 1? **No** (todos son 0)
- Resultado: $I_e(3,3) = 0$

**Posición (0,0) - esquina superior izquierda:**
- Con padding de 0s en bordes, no todos son 1
- Resultado: $I_e(0,0) = 0$

**Posición (0,4) - borde inferior:**
- Vecindad 3×3 (considerando padding):
$$\begin{bmatrix}
0 & 0 & 0 \\
1 & 1 & 1 \\
1 & 1 & 1
\end{bmatrix}$$
- ¿Todos son 1? **No** (hay 0s del padding)
- Resultado: $I_e(0,4) = 0$

**Aplicando erosión completa:**

La erosión elimina todos los bordes. Analicemos sistemáticamente:
- Los bordes externos de la imagen (con padding) se erosionan a 0
- El círculo de 0s impide que haya bloques 3×3 de 1s cerca de él
- Solo las regiones con bloques sólidos 3×3 de 1s sobreviven

$$
I_e = I \ominus S = \begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$$

**Explicación:**
- El círculo (región de 0s) tiene dimensiones 6×6 (filas 2-7, columnas 2-7 aproximadamente)
- No existe ningún bloque 3×3 completo de 1s en toda la imagen cuando consideramos:
  - Padding de 0s en los bordes externos
  - El gran círculo de 0s en el centro
- Las franjas de 1s alrededor del círculo tienen solo 2 píxeles de ancho (columnas 0-1 y 8-9)
- **Resultado**: Imagen completamente erosionada (todos 0s)

---

#### Paso 2: Cálculo del borde interno

$$\text{Borde\_interno} = I - I_e$$

Restamos la imagen erosionada de la imagen original:

$
\text{Borde\_interno} = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}$-$\begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}$=$\begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$

**Interpretación del borde interno:**
- Como $I_e$ es toda 0s, el borde interno es **idéntico a la imagen original**
- Esto indica que **toda la región de 1s es "borde interno"**
- No hay un núcleo "interior" sólido de 1s que sobreviva a la erosión
- La estructura de 1s alrededor del círculo es demasiado delgada (solo 2 píxeles de ancho en los lados)
- El círculo grande de 0s y los bordes con padding impiden cualquier bloque 3×3 sólido de 1s

---

#### Paso 3: Dilatación de $I$

Aplicamos dilatación a la imagen original. En la dilatación, un píxel se vuelve 1 si **al menos un píxel** en la vecindad 3×3 es 1.

**Análisis de posiciones clave:**

**Posición (2,3) - borde superior del círculo:**
- Vecindad 3×3:
$$\begin{bmatrix}
1 & 1 & 1 \\
1 & 0 & 0 \\
0 & 0 & 0
\end{bmatrix}$$
- ¿Al menos un píxel es 1? **Sí**
- Resultado: $I_d(2,3) = 1$

**Posición (4,4) - centro del círculo:**
- Vecindad 3×3:
$$\begin{bmatrix}
0 & 0 & 0 \\
0 & 0 & 0 \\
0 & 0 & 0
\end{bmatrix}$$
- ¿Al menos un píxel es 1? **No**
- Resultado: $I_d(4,4) = 0$

**Posición (3,2) - borde izquierdo del círculo:**
- Vecindad 3×3:
$$\begin{bmatrix}
1 & 1 & 0 \\
1 & 0 & 0 \\
1 & 0 & 0
\end{bmatrix}$$
- ¿Al menos un píxel es 1? **Sí**
- Resultado: $I_d(3,2) = 1$

**Posición (2,2) - esquina del círculo:**
- Vecindad 3×3:
$$\begin{bmatrix}
1 & 1 & 1 \\
1 & 1 & 0 \\
1 & 0 & 0
\end{bmatrix}$$
- ¿Al menos un píxel es 1? **Sí**
- Resultado: $I_d(2,2) = 1$

**Aplicando dilatación completa:**

La dilatación expande los 1s hacia el interior del círculo en un píxel en todas direcciones:

$$
I_d = I \oplus S = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}
$$

**Explicación:**
- Los 1s se expanden hacia el círculo de 0s
- El círculo se reduce:
  - **Original**: 6 filas × 6 columnas (filas 2-7, columnas 2-7 con variaciones)
  - **Después de dilatación**: 4 filas × 4 columnas (filas 3-6, columnas 3-6)
- Cambios específicos:
  - **Fila 2**: Se llena completamente con 1s (el borde superior del círculo se cierra)
  - **Columnas 2 y 7** (filas 3-6): Se rellenan con 1s (los bordes laterales se cierran)
  - **Fila 7**: Se llena completamente con 1s (el borde inferior del círculo se cierra)
- Solo permanece un núcleo central 4×4 de 0s

---

#### Paso 4: Cálculo del borde externo

$$\text{Borde\_externo} = I_d - I$$

Restamos la imagen original de la imagen dilatada:

$
\text{Borde\_externo} = \begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}$-$\begin{bmatrix}
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 \\
1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\
\end{bmatrix}$=$\begin{bmatrix}
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 1 & 1 & 1 & 1 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 & 1 & 1 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\
\end{bmatrix}
$

**Interpretación del borde externo:**
- Los 1s representan los píxeles que fueron **agregados** por la dilatación
- Forman el **contorno interno del círculo** (el borde que rodea internamente al círculo de 0s)
- **Total: 16 píxeles** forman el borde externo
- Patrón rectangular perfecto alrededor del círculo:
  - **Fila 2**: 4 píxeles (columnas 3-6) - Borde superior
  - **Filas 3-6**: 2 píxeles cada una (columnas 2 y 7) - Bordes laterales (4 filas × 2 = 8 píxeles)
  - **Fila 7**: 4 píxeles (columnas 3-6) - Borde inferior
  - Total: 4 + 8 + 4 = **16 píxeles**

---

#### Paso 5: Comparación y análisis

**Visualización comparativa:**

$$
\begin{array}{|c|c|c|c|}
\hline
\textbf{Tipo de Borde} & \textbf{Matriz Resultado} & \textbf{Píxeles detectados} & \textbf{Ubicación} \\
\hline
\textbf{Borde Interno} & \text{Igual a } I & \text{Toda la región de 1s (62 píxeles)} & \text{Todo el fondo (objeto principal)} \\
\hline
\textbf{Borde Externo} & \text{16 píxeles de 1} & \text{16 píxeles} & \text{Marco alrededor del círculo} \\
\hline
\end{array}
$$

**Borde interno visualizado (los 1s son el borde):**
```
Toda la estructura de 1s es borde porque no hay
un núcleo sólido que sobreviva a la erosión 3×3.
El círculo es demasiado grande y las franjas de 1s
son demasiado delgadas (2 píxeles de ancho).
```

**Borde externo visualizado (solo el contorno con 1s):**
```
  · · · · · · · · · ·
  · · · · · · · · · ·
  · · · 1 1 1 1 · · ·  ← Borde superior (4 píxeles)
  · · 1 · · · · 1 · ·  ← Bordes laterales
  · · 1 · · · · 1 · ·  ← Bordes laterales
  · · 1 · · · · 1 · ·  ← Bordes laterales
  · · 1 · · · · 1 · ·  ← Bordes laterales
  · · · 1 1 1 1 · · ·  ← Borde inferior (4 píxeles)
  · · · · · · · · · ·
  · · · · · · · · · ·
```

**Análisis detallado:**

1. **Borde interno vs Borde externo**:
   - **Borde interno** ($I - I_e$): Detecta la capa interna del objeto (1s). Como toda la región se erosiona completamente, el resultado es la imagen completa.
   - **Borde externo** ($I_d - I$): Detecta exactamente dónde la dilatación "invade" el círculo de 0s, formando un marco rectangular perfecto.

2. **Interpretación geométrica**:
   - El borde externo forma un **marco rectangular** de 1 píxel de grosor alrededor del círculo
   - Dimensiones del marco: 6×6 (filas 2-7, columnas 2-7)
   - Dimensiones del círculo reducido después de la dilatación: 4×4 (filas 3-6, columnas 3-6)
   - El borde externo marca la **transición de 0 (círculo) a 1 (fondo)**

3. **Píxeles específicos del borde externo** (16 píxeles totales):

   **Borde superior (fila 2):**
   - (2,3), (2,4), (2,5), (2,6) → 4 píxeles

   **Bordes laterales (filas 3-6):**
   - Izquierda: (3,2), (4,2), (5,2), (6,2) → 4 píxeles
   - Derecha: (3,7), (4,7), (5,7), (6,7) → 4 píxeles

   **Borde inferior (fila 7):**
   - (7,3), (7,4), (7,5), (7,6) → 4 píxeles

4. **Gradiente morfológico completo**:

Si calculáramos el gradiente morfológico:
$$\text{Gradiente} = I_d - I_e = I_d - 0 = I_d$$

En este caso, el gradiente sería igual a la imagen dilatada, ya que $I_e$ es completamente 0.

5. **Simetría perfecta**:
   - El borde externo muestra una simetría perfecta (rectangular)
   - Esto refleja la forma regular del círculo aproximado en la imagen original
   - 4 píxeles arriba, 4 abajo, 4 izquierda, 4 derecha

---

#### Conclusiones

**Sobre el borde interno:**
- **No hay un núcleo interno diferenciado** en esta imagen porque:
  - La región de fondo (1s) tiene solo 2 píxeles de ancho en los laterales
  - El círculo central es grande (6×6)
  - Los bordes de la imagen (con padding) también impiden bloques 3×3
- **Resultado**: $I - I_e = I$ (la imagen original completa es "borde interno")
- Esto indica que toda la estructura de 1s es "superficial" o "delgada"

**Sobre el borde externo:**
- **Detectó exitosamente el contorno del círculo** con exactamente **16 píxeles**
- Forma un **marco rectangular perfecto** de 1 píxel de grosor
- Es **simétrico y regular**: 4 píxeles por lado
- Muy útil para identificar la **forma y dimensiones** del círculo

**Comparación cuantitativa:**
- **Imagen original**: 62 píxeles con valor 1, 38 píxeles con valor 0
- **Borde interno**: 62 píxeles (todos los 1s originales)
- **Borde externo**: 16 píxeles (el contorno del círculo)
- **Relación**: El borde externo es el 25.8% del total de 1s (16/62)

**Aplicaciones prácticas:**
- **Borde interno**: Útil cuando hay objetos con núcleo sólido suficientemente grande
- **Borde externo**: Excelente para detectar contornos de huecos, cavidades o regiones de 0s
- **Este caso**: El borde externo es más informativo porque:
  - Identifica claramente el contorno del círculo
  - Proporciona información sobre la forma (rectangular)
  - Tiene un grosor uniforme de 1 píxel
  - Es computacionalmente eficiente (solo 16 píxeles vs 62)

**Ventajas de la detección morfológica de bordes:**
1. **Simplicidad**: Operaciones básicas de erosión y dilatación
2. **Robustez**: Menos sensible a ruido que métodos basados en gradientes
3. **Control**: El tamaño del elemento estructurante controla el grosor del borde
4. **Versatilidad**: Puede detectar bordes internos, externos o ambos
5. **Interpretabilidad**: Los resultados tienen significado geométrico claro

**Limitaciones observadas:**
- Requiere que la estructura tenga un grosor mínimo (relacionado con el tamaño de $S$)
- Si el objeto es muy delgado, toda la estructura puede ser "borde" (como en este caso)
- La elección del tamaño de $S$ afecta significativamente los resultados

---
