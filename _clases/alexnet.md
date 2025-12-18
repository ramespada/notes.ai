---
layout: lecture
title: Perceptron
description: El perceptron (la unidad base de una red neuronal)
date: 2024-03-08
ready: true
---

> El *perceptrón*, es un modelo simplificado del funcionamiento de una neurona biológica, fue creado por Frank Rosenblatt en la decada de los 50', y es la base de las redes neuronales. 


Componentes de un perceptron:
- Entradas/Input (x): una señal que consiste en un arreglo 1-D de valores
- Salida/Output  (y): una salida binaria (0-1)
- Pesos: (w): valores que ponderan la importancia de la señalde ajuste del perceptron

Para que un perceptron quede definido tambien es importante determinar:
- Algoritmo de ajuste de los w
- Tasa de aprendizaje (alpha)
- Función de activación



## Algoritmos de ajuste:

### Perceptron learning rule

$$ w_j'=w_j - \alpha x_j $$


### Regla de ajuste por cuadrados mínimos (LMS): 

Bernard Widrow y Ted Hoff propusieron una regla de ajuste que depende del error:

$$ w_j'=w_j + \alpha x_j (y - y*) $$


### Backpropagation




## Función de activación:

Antes de producir la salida, una función suma todas las señales multiplicadas por sus factores de peso y produce un valor único (salida). La función que interviene para generar este valor puede variar, a continuación veremos algunas de las más utilizadas.

### Función de activación de McCulloch-Pitts 


### Función de activación Sigmoideal
