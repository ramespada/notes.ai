---
layout: lecture
title: Perceptron
description: El perceptron (la unidad base de una red neuronal)
date: 2024-03-08
ready: true
---

> El *perceptron* es un modelo simple de neurona artificial propuesto por Frank Rosenblatt en 1957-1958. Aun hoy sigue siendo la unidad conceptual base de las redes neuronales modernas.

## Idea central

Un perceptron toma una entrada vectorial, calcula una suma ponderada y decide una clase binaria.

- Entrada: $$x = [x_1, x_2, ..., x_n]$$
- Pesos: $$w = [w_1, w_2, ..., w_n]$$
- Sesgo (*bias*): $$b$$
- Salida: $$\hat{y} \in \{-1, +1\}$$ (o alternativamente $$\{0,1\}$$)

Ecuacion del modelo:

$$
z = w^T x + b = \sum_{j=1}^{n} w_j x_j + b
$$

Regla de decision (activacion tipo escalon):

$$
\hat{y} =
\begin{cases}
+1 & \text{si } z \ge 0 \\
-1 & \text{si } z < 0
\end{cases}
$$

## Componentes del perceptron

- Entradas: variables del problema.
- Pesos: controlan la importancia de cada entrada.
- Bias: desplaza el umbral de decision.
- Funcion de activacion: transforma $$z$$ en clase.
- Regla de aprendizaje: ajusta $$w$$ y $$b$$ usando ejemplos etiquetados.

## Regla de aprendizaje de Rosenblatt

Para clasificacion binaria con etiquetas $$y \in \{-1,+1\}$$:

Si el perceptron se equivoca con un ejemplo $$x$$, se actualiza:

$$
w \leftarrow w + \alpha y x
$$

$$
b \leftarrow b + \alpha y
$$

Donde $$\alpha > 0$$ es la tasa de aprendizaje.

Una forma equivalente (si usas etiquetas $$\{0,1\}$$) es:

$$
w_j \leftarrow w_j + \alpha (y - \hat{y}) x_j
$$

$$
b \leftarrow b + \alpha (y - \hat{y})
$$

## Algoritmo (entrenamiento)

1. Inicializar $$w$$ y $$b$$ (por ejemplo en cero).
2. Recorrer el dataset varias epocas.
3. Para cada ejemplo $$x^{(i)}$$:
   - calcular $$\hat{y}^{(i)}$$
   - si hay error, actualizar $$w, b$$ con la regla anterior.
4. Detener cuando no haya errores o se alcance un maximo de epocas.

## Geometria: el perceptron piensa en lineas (o hiperplanos)

La frontera de decision es:

$$
w^T x + b = 0
$$

- En 2D: es una recta.
- En 3D: es un plano.
- En dimensiones mayores: un hiperplano.

Por eso el perceptron solo puede resolver problemas **linealmente separables**.

## Ejemplos clasicos

### Problemas que si puede aprender

- Compuerta OR
- Compuerta AND

En ambos casos existe una linea que separa las clases positivas de las negativas.

### Problema que no puede aprender (XOR)

La compuerta XOR no es linealmente separable en 2D. No hay una sola recta que separe perfectamente los puntos.

Resultado practico: el perceptron entra en ciclos de actualizacion y no converge a solucion perfecta.

## Teorema de convergencia (idea)

Si los datos son linealmente separables, el algoritmo del perceptron converge en una cantidad finita de actualizaciones (Novikoff, 1962).

Si no son separables, no hay garantia de convergencia.

## Del perceptron a ADALINE, LMS y backpropagation

Historicamente:

- **Perceptron (Rosenblatt):** actualiza por error de clasificacion.
- **ADALINE / LMS (Widrow-Hoff):** minimiza error cuadratico medio con una regla tipo gradiente.
- **Backpropagation (Rumelhart, Hinton, Williams, 1986):** generaliza la idea de gradiente a redes multicapa.

Punto clave: para entrenar redes profundas se usan activaciones diferenciables (sigmoide, tanh, ReLU), no el escalon duro de McCulloch-Pitts.

## Mini implementacion en Python

```python
import numpy as np

class Perceptron:
    def __init__(self, lr=0.1, epochs=50):
        self.lr = lr
        self.epochs = epochs
        self.w = None
        self.b = 0.0

    def fit(self, X, y):
        # y en {-1, +1}
        n_features = X.shape[1]
        self.w = np.zeros(n_features)

        for _ in range(self.epochs):
            errors = 0
            for xi, yi in zip(X, y):
                y_hat = 1 if (np.dot(xi, self.w) + self.b) >= 0 else -1
                if y_hat != yi:
                    self.w += self.lr * yi * xi
                    self.b += self.lr * yi
                    errors += 1
            if errors == 0:
                break

    def predict(self, X):
        z = X @ self.w + self.b
        return np.where(z >= 0, 1, -1)
```

## Resumen

- El perceptron es la neurona artificial mas simple.
- Aprende moviendo pesos y bias para corregir errores.
- Su poder esta limitado a separaciones lineales.
- Fue la base historica para llegar a redes multicapa y al deep learning moderno.

## Ejercicios sugeridos

1. Implementar OR y AND con el perceptron y verificar convergencia.
2. Intentar entrenar XOR y observar que no converge.
3. Graficar la frontera de decision durante el entrenamiento en 2D.
4. Comparar regla de Rosenblatt vs. LMS en un dataset con ruido.
