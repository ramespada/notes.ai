---
layout: lecture
title: Interpretabilidad mecanística
description: Entender y controlar comportamientos internos de LLMs
date: 2024-03-08
ready: true
---

> Basado en el capitulo 7 del libro de referencia (Welch Labs): la interpretabilidad mecanistica busca explicar que circuitos internos producen una conducta del modelo.

## 1. Problema central

Alinear salidas con ejemplos no implica entender el mecanismo interno.

El capitulo muestra que:
- un modelo puede parecer "honesto" en superficie,
- pero mantener informacion interna y comportamientos no transparentes,
- por eso necesitamos abrir el modelo y medir sus representaciones internas.

## 2. Residual stream (idea operativa)

En transformers, cada bloque agrega una correccion al estado actual.

$$
r_{l+1} = r_l + \mathrm{Attn}_l(r_l) + \mathrm{MLP}_l(r_l)
$$

Simbolos:
- $$r_l$$: estado del *residual stream* en la capa $$l$$.
- $$r_{l+1}$$: estado actualizado para la siguiente capa.
- $$\mathrm{Attn}_l(\cdot)$$: salida del bloque de atencion de la capa $$l$$.
- $$\mathrm{MLP}_l(\cdot)$$: salida del bloque MLP de la capa $$l$$.

Lectura rapida: el modelo transforma el mismo "canal" residual capa por capa hasta producir el siguiente token.

## 3. Salida a probabilidades

En el final, el estado se proyecta a logits y luego a probabilidades:

$$
\mathbf{p} = \mathrm{softmax}(\mathbf{z})
$$

$$
p_i = \frac{e^{z_i}}{\sum_j e^{z_j}}
$$

Simbolos:
- $$\mathbf{z}$$: vector de logits (uno por token del vocabulario).
- $$\mathbf{p}$$: vector de probabilidades de siguiente token.
- $$p_i$$: probabilidad del token $$i$$.
- $$z_i$$: logit del token $$i$$.
- $$j$$: indice que recorre todos los tokens del vocabulario.

## 4. Polysemanticidad y superposicion

Del capitulo: una neurona individual suele mezclar varios conceptos (*polysemanticity*).

Hipotesis de superposicion: el modelo codifica mas conceptos que dimensiones disponibles, superponiendolos en la misma base neuronal.

Implicancia: inspeccionar neuronas sueltas no alcanza para entender conceptos limpios.

## 5. Sparse Autoencoder (SAE)

Se usa un modelo auxiliar para mapear activaciones densas a features mas interpretables y dispersas.

Codificacion:

$$
\mathbf{f} = E\mathbf{x}
$$

Sparsificacion (idea):

$$
\tilde{\mathbf{f}} = \mathrm{sparse}(\mathbf{f})
$$

Reconstruccion:

$$
\hat{\mathbf{x}} = D\tilde{\mathbf{f}}
$$

Objetivo de entrenamiento:

$$
\mathcal{L}_{SAE} = \|\mathbf{x}-\hat{\mathbf{x}}\|_2^2 + \lambda\|\tilde{\mathbf{f}}\|_1
$$

Simbolos:
- $$\mathbf{x}$$: vector de activaciones del modelo original (por ejemplo, residual stream de una capa).
- $$E$$: matriz de codificacion (encoder).
- $$\mathbf{f}$$: features antes de forzar dispersidad.
- $$\tilde{\mathbf{f}}$$: features dispersas (la mayoria cerca de cero).
- $$D$$: matriz de decodificacion (decoder).
- $$\hat{\mathbf{x}}$$: reconstruccion de $$\mathbf{x}$$.
- $$\mathcal{L}_{SAE}$$: perdida total del autoencoder disperso.
- $$\|\cdot\|_2^2$$: error cuadratico de reconstruccion.
- $$\|\cdot\|_1$$: penalizacion de sparsidad.
- $$\lambda$$: peso de la penalizacion de sparsidad.

## 6. Steering de features (resultado clave del capitulo)

Una vez identificado un feature, se puede aumentar o reducir su activacion y observar cambios en salida.

Forma simple de intervención:

$$
\tilde{f}_k \leftarrow c
$$

Simbolos:
- $$\tilde{f}_k$$: feature disperso numero $$k$$.
- $$k$$: indice del feature intervenido.
- $$c$$: valor fijado manualmente (*clamp*).

En el capitulo, al amplificar un feature asociado a escepticismo (ejemplo en Gemma), sube la probabilidad de continuaciones tipo "questionable".

## 7. Limitaciones que marca el libro

- Extraemos solo una fraccion de los conceptos internos.
- Muchos features son raros y costosos de descubrir.
- Puede haber superposicion entre capas, no solo dentro de una capa.
- Aun con millones de features, seguimos lejos de una interpretacion completa.

## 8. Resumen

- La interpretabilidad mecanistica estudia mecanismos internos, no solo outputs.
- El residual stream es el canal central de representacion en transformers.
- SAE ayuda a desmezclar conceptos y habilita *steering* controlado.
- El enfoque es prometedor, pero hoy sigue siendo parcial.

## 9. Ejercicios sugeridos

1. Elegir una capa y registrar activaciones del residual stream para un prompt fijo.
2. Entrenar un SAE pequeno y medir reconstruccion vs sparsidad.
3. Identificar un feature activado por "duda/escepticismo" y probar *clamp*.
4. Comparar outputs antes y despues de la intervencion.
