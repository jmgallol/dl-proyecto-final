# Clasificación de enfermedades en hojas de café mediante Deep Learning

Proyecto final del curso de Deep Learning.

## 1. Problema

Las enfermedades y plagas de las plantas causan pérdidas importantes en los cultivos, y su diagnóstico suele depender de la inspección visual de un experto, que no siempre está disponible y puede ser subjetiva. En el café, enfermedades como la roya, el minador, la phoma y la cercospora reducen la productividad y la calidad del grano.

Este proyecto busca clasificar automáticamente imágenes de hojas de café en sanas o afectadas por alguna de estas enfermedades, usando técnicas de Deep Learning. Así se podría apoyar la detección temprana a partir de fotografías tomadas con un celular.

## 2. Base de datos

- **Nombre:** BRACOL – Brazilian Arabica Coffee Leaf images dataset
- **Fuente:** Krohling, Esgario y Ventura (2019), Mendeley Data
- **Enlace:** https://data.mendeley.com/datasets/yy2k5y8mxg/1
- **DOI:** 10.17632/yy2k5y8mxg.1
- **Tamaño:** 1.747 imágenes de hojas de café arábica, tomadas con smartphones en condiciones reales
- **Clases:** hoja sana y hojas afectadas por minador, roya, phoma (mancha parda) y cercospora

## 3. Arquitectura propuesta

Transferencia de aprendizaje con EfficientNet-B0 (Tan & Le, 2019), preentrenada en ImageNet. Se reemplazará la capa clasificadora final por una adaptada a las clases del problema y se aplicará fine-tuning. Para interpretar las predicciones se usará Grad-CAM, que muestra en qué zonas de la hoja se fija el modelo.

## 4. Integrantes:
- David García Zapata
- Mariana Carrasquilla
- Juan Manuel Gallo López
- Valentina Zapata
