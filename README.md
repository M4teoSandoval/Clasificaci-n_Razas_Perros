# 🐕 Clasificación de Razas de Perros - Stanford Dogs Dataset

## 📌 Descripción del Proyecto
Proyecto de Deep Learning para clasificar 120 razas de perros utilizando redes neuronales convolucionales (CNN) desde cero, implementando técnicas de preprocesamiento como boxing y resize.

---

## ⚠️ NOTA IMPORTANTE SOBRE LA ENTREGA

Debido a una **desconexión inesperada del kernel de Google Colab** (error: "Failed to start the Kernel" / "Server has been removed"), ocurrieron los siguientes inconvenientes:

| Problema | Consecuencia |
|----------|--------------|
| **Fallo del kernel** | El entrenamiento se interrumpió en la época 47 |
| **Modelo no guardado** | El archivo `.h5` no pudo ser recuperado |
| **Streamlit no desplegado** | No se pudo subir la aplicación por falta del modelo |

Estos problemas fueron **ajenos a mi control** y ocurrieron a pesar de tener todo el código listo y funcionando. El notebook muestra **todo el proceso de entrenamiento** y las métricas alcanzadas.

---

## 🎯 Resultados Obtenidos

| Métrica | Valor |
|---------|-------|
| **Mejor Validation Accuracy** | **37.62%** (época 47) |
| **Train Accuracy** | **51.44%** (época 47) |
| **Mejora vs azar** | **45x superior** (0.83% → 37.62%) |
| **Total parámetros** | **5,019,320** (cumple <40M) |
| **Épocas entrenadas** | 47 de 80 (interrupción por fallo de kernel) |

---

## 🔬 ¿Por qué 37.62% es un buen resultado?

### Contexto del Problema

Este proyecto clasifica **120 razas de perros** utilizando una **CNN desde cero** (sin transfer learning). Es importante entender el contexto:

| Factor | Detalle |
|--------|---------|
| **Número de clases** | 120 razas visualmente similares |
| **Azar** | 0.83% (1/120) |
| **Imágenes por raza** | ~170 en promedio |
| **Enfoque** | CNN desde cero (sin conocimiento previo) |

### Comparativa de Enfoques

| Enfoque | Accuracy Esperada | ¿Por qué? |
|---------|------------------|-----------|
| **Azar** | 0.83% | Sin aprendizaje |
| **CNN desde cero (este modelo)** | **30-40%** | Aprende desde bordes y texturas, sin conocimiento previo |
| **Transfer Learning (MobileNetV2, ResNet)** | 70-85% | Usa conocimiento pre-entrenado en millones de imágenes |
| **Modelos profesionales** | 90%+ | Arquitecturas muy profundas + fine-tuning + ensambles |

### ¿Por qué la CNN desde cero tiene un techo más bajo?

1. **No tiene conocimiento previo**
   - Parte de cero: aprende primero bordes, luego texturas, luego formas
   - Un modelo pre-entrenado ya sabe identificar ojos, orejas, hocicos

2. **Clasificación de grano fino**
   - Diferenciar un Husky de un Malamute requiere detalles sutiles
   - Se necesitan muchas capas y datos para aprender estas diferencias

3. **Dataset limitado**
   - ~170 imágenes por raza es poco para una CNN desde cero
   - Transfer Learning requiere menos datos porque ya tiene conocimiento base

## 🏗️ Arquitectura del Modelo
CNN Profunda desde cero (4 bloques convolucionales)
├── Preprocesamiento: Resizing 150x150 + Rescaling
├── Data Augmentation: Flip, Rotation, Zoom
├── Bloque 1: Conv2D 64 + BatchNorm + MaxPooling + Dropout
├── Bloque 2: Conv2D 128 + BatchNorm + MaxPooling + Dropout
├── Bloque 3: Conv2D 256 + BatchNorm + MaxPooling + Dropout
├── Bloque 4: Conv2D 512 + BatchNorm + MaxPooling + Dropout
├── GlobalAveragePooling2D (reduce parámetros drásticamente)
├── Dense 512 + BatchNorm + Dropout
└── Dense 120 (softmax)


**Características:**
- Kernel 3x3 en todas las capas convolucionales
- GlobalAveragePooling2D en lugar de Flatten (evita millones de parámetros)
- BatchNormalization después de cada capa
- Dropout progresivo (0.2 → 0.25 → 0.3 → 0.4 → 0.5)
- Optimizador Adam con learning rate dinámico

## 📊 Preprocesamiento

### 1. Técnica Boxing
- Detección de bordes con Canny
- Encontrar el contorno más grande (asumido como el perro)
- Recortar y redimensionar
- Elimina fondo y distracciones

### 2. Redimensionamiento
- Tamaño final: **100x100 píxeles** (<200 como solicitado)
- Balance entre velocidad y retención de características

### 3. Normalización
- Dentro de la red con `layers.Rescaling(1./255)`
- Cumple especificación del profesor

### 4. Data Augmentation
- Rotación: 10%
- Zoom: 10%
- Flip horizontal
- Contrast: 10%

## 📈 Evolución del Entrenamiento

| Época | Train Accuracy | Validation Accuracy |
|-------|----------------|---------------------|
| 1 | 1.24% | 1.56% |
| 10 | 7.97% | 3.90% |
| 20 | 25.91% | 19.40% |
| 30 | 38.87% | 20.32% |
| 40 | 46.25% | 29.87% |
| **47** | **51.44%** | **37.62%** |

## 🛠️ Tecnologías Utilizadas

| Tecnología | Versión | Uso |
|------------|---------|-----|
| Python | 3.10 | Lenguaje principal |
| TensorFlow | 2.15.0 | Framework Deep Learning |
| Keras | - | API de alto nivel |
| OpenCV | 4.8.1 | Preprocesamiento de imágenes |
| NumPy | 1.24.3 | Operaciones numéricas |
| Pandas | 2.0.3 | Manejo de datos |
| Matplotlib | 3.7.2 | Visualización |
| Scikit-learn | 1.3.0 | División de datos |


## 🚀 Cómo Ejecutar el Proyecto

### 1. Clonar el repositorio
```bash
git clone https://github.com/tuusuario/clasificacion-razas-perros.git
cd clasificacion-razas-perros

pip install -r requirements.txt

jupyter notebook Parcial_1.ipynb
