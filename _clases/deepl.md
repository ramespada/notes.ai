---
layout: lecture
title: Deep Learning
description: Que cambia cuando pasamos de una neurona a redes profundas
date: 2024-03-08
ready: true
---

> Deep learning es usar redes neuronales con multiples capas para aprender representaciones cada vez mas abstractas.

## 1. Idea central

- Una sola capa aprende fronteras simples.
- Varias capas componen funciones y aprenden patrones complejos.
- Profundidad = mas capacidad de representacion.

## 2. Arquitectura basica

Para una red de $$L$$ capas:

$$
z^{[l]} = W^{[l]}a^{[l-1]} + b^{[l]}
$$

$$
a^{[l]} = g^{[l]}(z^{[l]})
$$

- $$a^{[0]} = x$$
- $$a^{[L]} = \hat{y}$$

## 3. Flujo de entrenamiento

1. **Forward pass**: obtener $$\hat{y}$$.
2. **Loss**: medir error $$\mathcal{L}(\hat{y}, y)$$.
3. **Backprop**: calcular gradientes.
4. **Gradient descent**: actualizar parametros.

## 4. Por que funciona mejor que modelos poco profundos

- Aprende representaciones jerarquicas.
- Reusa features entre capas.
- Escala con datos y computo.

## 5. Componentes clave

- Activaciones: ReLU, GELU, sigmoid, tanh.
- Normalizacion: BatchNorm, LayerNorm.
- Regularizacion: dropout, weight decay.
- Optimizacion: SGD, Adam.

## 6. Riesgos comunes

- Overfitting.
- Vanishing/exploding gradients.
- Entrenamiento inestable por mala inicializacion o learning rate.

## 7. Metricas tipicas

- Clasificacion: accuracy, precision, recall, F1.
- Regresion: MAE, MSE, RMSE.

## 8. Resumen

- Deep learning = redes profundas + backprop + optimizacion.
- La profundidad permite modelar funciones complejas.
- El rendimiento depende de arquitectura, datos y entrenamiento.

## 9. Ejercicios sugeridos

1. Entrenar una MLP de 1 capa y otra de 3 capas en el mismo dataset.
2. Comparar ReLU vs sigmoid en velocidad de convergencia.
3. Agregar dropout y medir efecto en train vs validation.
