---
layout: lecture
title: Leyes de Escalado Neuronal
description: Como mejora un modelo al escalar parametros, datos y computo
date: 2024-03-07
ready: true
---

> Las leyes de escalado describen como cambia la perdida de un modelo cuando aumentamos tamano, datos y computo.

## 1. Idea central

En muchos modelos grandes, la perdida baja siguiendo una ley de potencia: mejoras pequenas pero predecibles al escalar.

## 2. Ley de escalado con parametros

$$
L(N) = L_{\infty} + A\,N^{-\alpha}
$$

Simbolos:
- $$L(N)$$: perdida esperada usando $$N$$ parametros.
- $$L_{\infty}$$: perdida limite (piso teorico al escalar mucho).
- $$A$$: constante de escala (depende de tarea/modelo).
- $$N$$: numero de parametros entrenables.
- $$\alpha$$: exponente de escalado (>0).

Lectura rapida: al aumentar $$N$$, la perdida baja, pero con retornos decrecientes.

## 3. Ley de escalado con datos

$$
L(D) = L_{\infty} + B\,D^{-\beta}
$$

Simbolos:
- $$L(D)$$: perdida esperada usando $$D$$ tokens (o ejemplos).
- $$L_{\infty}$$: perdida limite.
- $$B$$: constante de escala para datos.
- $$D$$: cantidad de datos de entrenamiento.
- $$\beta$$: exponente de escalado de datos (>0).

Lectura rapida: mas datos casi siempre ayuda, hasta saturar.

## 4. Costo de entrenamiento (aprox.)

$$
C \approx k\,N\,D
$$

Simbolos:
- $$C$$: costo computacional total (FLOPs aproximados).
- $$k$$: constante que resume arquitectura, secuencia y detalle de implementacion.
- $$N$$: numero de parametros.
- $$D$$: tokens (o ejemplos) usados en entrenamiento.

Lectura rapida: duplicar parametros o datos suele duplicar costo.

## 5. Regimen compute-optimo (intuicion)

Con presupuesto fijo $$C$$, no conviene escalar solo $$N$$ o solo $$D$$: conviene balancearlos.

Una regla practica usada en LLMs modernos es:

$$
D \approx r\,N
$$

Simbolos:
- $$D$$: tokens de entrenamiento.
- $$N$$: numero de parametros.
- $$r$$: razon tokens/parametro (heuristica del regimen optimo).

Lectura rapida: si $$N$$ crece, tambien deben crecer los datos para no subentrenar el modelo.

## 6. Implicancias practicas

- Escalar sin datos suficientes produce undertraining.
- Escalar datos sin capacidad suficiente limita la ganancia.
- Las leyes de escalado sirven para planificar presupuesto antes de entrenar.

## 7. Limites

- Son tendencias empiricas, no leyes fisicas exactas.
- Cambian con arquitectura, tokenizer, dominio de datos y objetivo.
- No sustituyen evaluacion real en tareas downstream.

## 8. Checklist para planificar un experimento

1. Fijar presupuesto $$C$$.
2. Elegir rango de $$N$$.
3. Estimar $$D$$ compatible (evitar desequilibrio).
4. Entrenar modelos pequenos piloto.
5. Ajustar exponente empirico y proyectar el modelo final.

## 9. Ejercicios sugeridos

1. Ajustar una ley $$L(N)=L_{\infty}+A N^{-\alpha}$$ con tus resultados.
2. Comparar dos corridas con mismo $$C$$ pero distinto balance $$N$$/$$D$$.
3. Medir cuando aparecen retornos decrecientes al escalar datos.
