# 🐕 Clasificación de Razas de Perros - Stanford Dogs Dataset

## 📌 Descripción del Proyecto
Proyecto de Deep Learning para clasificar 120 razas de perros utilizando redes neuronales convolucionales (CNN) desde cero, implementando técnicas de preprocesamiento como boxing y resize.

## 🎯 Resultados Obtenidos

| Métrica | Valor |
|---------|-------|
| **Mejor Validation Accuracy** | **37.62%** (época 47) |
| **Train Accuracy** | **51.44%** (época 47) |
| **Mejora vs azar** | **45x superior** (0.83% → 37.62%) |
| **Total parámetros** | **~5M** (cumple <40M) |
| **Épocas entrenadas** | 47 de 80 (interrupción por fallo de kernel) |

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
