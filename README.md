# Clasificación de Enfermedades en Hojas de Café mediante Deep Learning

> **Proyecto final del curso de Deep Learning.**
>
> Este repositorio contiene el código, modelos y experimentación para la detección automática de enfermedades en cultivos de café utilizando visión artificial.

## Tabla de Contenidos

1. [El Problema](#1-el-problema)
2. [Base de Datos](#2-base-de-datos)
3. [Arquitectura y Metodología](#3-arquitectura-y-metodología)
4. [Integrantes](#4-integrantes)

## 1. El Problema

Las enfermedades y plagas de las plantas causan pérdidas importantes en los cultivos, y su diagnóstico suele depender de la inspección visual de un experto. Este recurso no siempre está disponible y la evaluación puede ser subjetiva. En el caso del café, enfermedades como **la roya, el minador, la phoma y la cercospora** reducen drásticamente la productividad y la calidad del grano.

Este proyecto busca clasificar automáticamente imágenes de hojas de café en "sanas" o "afectadas" por alguna de estas enfermedades usando técnicas de **Deep Learning**. El objetivo a largo plazo es apoyar la detección temprana y el diagnóstico en campo a partir de fotografías tomadas con un teléfono celular.

## 2. Base de Datos

Se utilizó el conjunto de datos **BRACOL**, compuesto por imágenes tomadas con smartphones en condiciones reales de iluminación y fondo.

- **Nombre:** BRACOL – Brazilian Arabica Coffee Leaf images dataset
- **Fuente:** Krohling, Esgario y Ventura (2019), Mendeley Data
- **Enlace:** [Mendeley Data - BRACOL](https://data.mendeley.com/datasets/yy2k5y8mxg/1)
- **DOI:** `10.17632/yy2k5y8mxg.1`
- **Tamaño:** 1.747 imágenes de hojas de café arábica.
- **Clases (5):**
  *  Hoja sana
  *  Minador
  *  Roya
  *  Phoma (mancha parda)
  *  Cercospora

## 3. Arquitectura y Metodología

En el proyecto implementamos y comparamos dos redes neuronales muy diferentes. Esto nos permitió evaluar la ventaja de utilizar un modelo robusto y preentrenado frente a diseñar una arquitectura liviana desde cero. Se aplicaron técnicas de preprocesamiento y aumento de datos (Data Augmentation) en ambas aproximaciones.

### 3.1 Arquitectura 1: ResNet-50 (Transfer Learning)
Se implementó una estrategia de **Transferencia de Aprendizaje (Transfer Learning)** cargando directamente el modelo desde PyTorch (`torchvision.models`). El uso de conexiones residuales en esta arquitectura permite entrenar redes profundas mitigando el problema del desvanecimiento del gradiente.
* **Características:** Cuenta con 23.52 millones de parámetros y fue preentrenada en la base de datos ImageNet, la cual contiene más de un millón de fotos.
* **Backbone intacto:** Se dejó congelada toda la primera parte de la red, aprovechando su capacidad ya aprendida para extraer características universales como bordes, formas, colores y texturas.
* **Cabeza Clasificadora (Adaptación):** Se reemplazó únicamente la última capa (la cabeza clasificadora `fc`) por una nueva adaptada a nuestro problema: consta de un **Dropout del 30 %** (para evitar que memorice, apagando neuronas al azar) seguido de una capa lineal con 5 salidas.
* **Interpretabilidad:** Se conectó la técnica **Grad-CAM** a la última capa convolucional de la red (`layer4[-1]`). Esto genera mapas de calor sobre la imagen que permiten visualizar en qué zonas de la hoja se activó más el modelo para tomar su decisión, aportando explicabilidad clínica al diagnóstico[cite: 1].

### 3.2 Arquitectura 2: Custom CNN (Red convolucional hecha desde cero)
Es una red propia, armada desde cero en PyTorch sin ningún conocimiento previo, diseñada para ser eficiente en el consumo de recursos.
* **Características:** Es un modelo sumamente liviano que cuenta con solo 0.25 millones de parámetros (unas 94 veces más pequeña que la ResNet-50).
* **Extractor de características (features):** Está compuesto por 4 bloques secuenciales. Cada bloque cuenta con una capa convolucional (`Conv2d` con 32, 64, 128 y 128 filtros respectivamente), una función de activación `ReLU` y una capa `MaxPool2d` (que reduce el tamaño espacial de la imagen a la mitad en cada paso). Este flujo finaliza con un promedio global (`AdaptiveAvgPool2d`) que resume la información en una lista de 128 números.
* **Clasificador (classifier):** Toma estos 128 valores, los pasa por una capa totalmente conectada de 64 neuronas con activación `ReLU`, aplica un **Dropout del 30 %** para regularización y termina en la capa lineal final de 5 salidas para realizar la predicción[cite: 1].

## 4. Integrantes

- **David García Zapata**
- **Mariana Carrasquilla**
- **Juan Manuel Gallo López**
- **Valentina Zapata**
