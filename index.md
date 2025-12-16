---
layout: page
title: Introducción a las bases de Inteligencia Artificial
nositetitle: true
---

El objetivo del curso es exponer las bases para entender como funcionan los modelos de inteligencia artificial con enfasis en los aspectos teóricos de los modelos de lenguage (LLM).

Se asumen conocimientos previos de algebra lineal, cálculo, lógica y nociones de teoría de la información. 

Acá podés encontrár [referencias](./biblio/) que usé para armar el curso.

# Programa del curso

<ul>
{% assign clases = site['clases'] | sort: 'date' %}
{% for clase in clases %}
    {% if clase.phony != true %}
      {% if clase.ready %}
        <li>
        <strong>{{ clase.date | date: 'C%-m.%-d' }}</strong>:
            <a href="{{site.baseurl}}{{ clase.url }}">{{ clase.title }}</a>
        </li>
        {% else %}
        {% comment %}
        	 <li>  {{ clase.title }} {% if clase.noclass %}[no class]{% endif %}</li> 
        {%endcomment%}
      {% endif %}
    {% endif %}
{% endfor %}
</ul>

<div class="small center">
<p><a href="https://github.com/ramespada/notes.ai">Repositorio</a>.</p>
<p></p>
<p>Ramiro A. Espada</p>
</div>
