---
layout: lecture
title: Backpropagation
description: Calculo eficiente de gradientes en redes neuronales
date: 2024-03-08
ready: true
---

> Backpropagation calcula los gradientes de una red multicapa usando la regla de la cadena.

## Idea principal

Para entrenar con gradient descent necesitamos:

$$
\nabla_{\theta}\mathcal{L}
$$

Backprop calcula ese gradiente de forma eficiente, desde la salida hacia la entrada.

## Red basica

Para cada capa $$l$$:

$$
z^{[l]} = W^{[l]}a^{[l-1]} + b^{[l]}
$$

$$
a^{[l]} = g(z^{[l]})
$$

## Paso hacia adelante (forward)

1. Calcular activaciones capa por capa.
2. Obtener prediccion $$\hat{y}$$.
3. Calcular perdida $$\mathcal{L}(\hat{y}, y)$$.

## Paso hacia atras (backward)

Definimos:

$$
\delta^{[l]} = \frac{\partial \mathcal{L}}{\partial z^{[l]}}
$$

Gradientes de la capa:

$$
\frac{\partial \mathcal{L}}{\partial W^{[l]}} = \delta^{[l]}(a^{[l-1]})^T
$$

$$
\frac{\partial \mathcal{L}}{\partial b^{[l]}} = \delta^{[l]}
$$

Propagacion hacia atras:

$$
\delta^{[l-1]} = (W^{[l]})^T\delta^{[l]} \odot g'(z^{[l-1]})
$$

## Caso muy usado

Con **softmax + cross-entropy** en la salida:

$$
\delta^{[L]} = \hat{y} - y
$$

Eso simplifica mucho el entrenamiento en clasificacion.

## Actualizacion

$$
W^{[l]} \leftarrow W^{[l]} - \alpha \frac{\partial \mathcal{L}}{\partial W^{[l]}}
$$

$$
b^{[l]} \leftarrow b^{[l]} - \alpha \frac{\partial \mathcal{L}}{\partial b^{[l]}}
$$

## Pseudocodigo

```text
forward -> loss
backward -> dW, db
update -> W, b
repetir
```

## Resumen

- Backprop no optimiza por si solo: calcula gradientes.
- Gradient descent usa esos gradientes para actualizar parametros.
- Juntos son la base del entrenamiento de redes neuronales.
