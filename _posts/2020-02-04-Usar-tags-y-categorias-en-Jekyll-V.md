---
layout: post
title: Usar tags y categorías en Jekyll (V)
date: 2020-02-04 15:00:00 -0300
category: jekyll
tags: [jekyll-enhancement, tagging, liquid]
---

Comenzamos repitiendo lo dicho al inicio del [post anterior]({{site.baseurl}}/Usar-tags-y-categorias-en-Jekyll-IV/) : "Para que un sistema de *tagging* funcione es necesario que las etiquetas asignadas a cada *post* aparezcan disponibles al visitante del blog en forma de *links*. "

En este artículo nos queda generar estos *links*, pero ahora les toca el turno a los *tags*: visualmente queremos que quede algo como lo que se muestra en la siguiente imagen...

![tags]({{site.baseurl}}/images/tags.PNG)

A diferencia que lo visto para las *categorías*, ahora buscaremos generar la muestra de estas etiquetas *tanto* en la página correspondiente a cada *post*,  *como* en aquella página/s donde se indexan los sumarios de cada uno de los mismos —concretamente se afectarán las templates `index.html` y `post.html`—

## *Tags* en *Jekyll*: un caso práctico (*Liquid coding II*)

Nuevamente tomaremos como referencia los 2 artículos del blog  [Mindust](http://www.minddust.com/) —que ya citamos oportunamente— , especialmente el más antiguo (aunque  en el mismo no se toma la aproximación a la solución usando *Collections*).

Es probable que nos encontremos con más de una etiqueta por *post :  en diseño web suele usarse el recurso de "meter" estos *links* (*anchors*, &lt;a&gt; ) dentro de una estructura de lista (unordered list, `ul`), aunque visualmente los ubiquemos un *tag* a continuación del otro.

{% highlight html %}
<ul>
    <li><a>link1</a></li>
    <li><a>link2</a></li>
    <li><a>link3</a></li>
</ul>
{% endhighlight %}

Pienso a ser mucho más sintético que en la entrada anterior... empiezo aclarando que voy a colocar la codificación en *__snippets__* escritos en archivos tipo  `[include].html`.

***

### *Tags* para index.html

Como sabemos, aquí se indexan los *posts*... en realidad se indexa un resumen o anticipo de cada *post*. En mi caso voy a ubicar las etiquetas justo antes del READ MORE (o *leer más* ), por lo que en `index.html` haré un pequeño cambio:

{% highlight html %}
{% raw %}
<footer class="excerpt-foot">
	{% include tags_en_index.html %}

	<a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Leer Más</a>
</footer>
{% endraw %}
{% endhighlight %}

NOTA: no confundir este `footer` con el de la página, este va a corresponder a cada *post* indexado. La parte de interés es el `include`.

#### Código para el *include* tags_en_index.html

OK, el nombre del archivo *no es de lo mejor...*, es arbitrario, pero recordemos que es importante ubicarlo dentro de la carpeta `_includes`. Aquí el código (inspirado en el [trabajo antes citado](http://www.minddust.com/post/tags-and-categories-on-github-pages/)):

{% highlight html %}
{% raw %}
{% comment %}
=======================
The purpose of this snippet: es listar las tags de cada post
=======================
{% endcomment %}

<ul class="tags">
  {% if post.tags.size > 0 %}
    {% for posta in post.tags %}
      {% assign tag = site.my_tags | where: "slug", posta %}
      {% if tag %}
        {% assign tag = tag[0] %}
        <li class="tag"><a href="{{ site.baseurl }}{{ tag.url }}">{{ tag.name }}</a></li>
      {% endif %}
    {% endfor %}
  {% endif %}
</ul>
{% endraw %}
{% endhighlight %}

Los detalles del "funcionamiento" del programa no difiere demasiado de lo explicado en el artículo previo... *lo dejo como ejercicio para el lector*.

***

### *Tags* para post.html

De nuevo usaré un *snippet* escrito dentro de un *include*, que llamaré `tags_en_post.html`. El lugar donde mostrar las etiquetas depende del diseño que querramos adoptar: en mi caso lo haré después de la fecha (*[remember this?](https://dsigno.github.io/Empezando-Jekyll/Mostrar-fechas-en-espanol/)*)

{% highlight liquid %}
{% raw %}
<h1>{{ page.title }}</h1>

  {% include date_es.html date=page.date time-attributes=' class="post-date"' %}

  {% include tags_en_post.html %}

  <div class="entry">
  	{{ content }}
  </div>
	...
{% endraw %}
{% endhighlight %}

#### Código para el *include* tags_en_post.html

El código de este snippet es un poco más complejo que el anterior; notar que aquí se trabajará con `page.tags`, y no con `post.tags`...

{% highlight html %}
{% raw %}
{% comment %}
=======================
The purpose of this snippet is: listar todos los tags de este post
=======================
{% endcomment %}

<ul class="tags">
{% if page.tags.size > 0 %}

  {% for post_tag in page.tags %}
    {% assign tag = site.my_tags | where: "slug", post_tag %}
    {% if tag %}
      {% assign tag = tag[0] %}
      {% capture tags_content_temp %}
      {{ tags_content }}<li class="tag"><a href="{{ site.baseurl }}{{ tag.url }}">{{ tag.name }}</a></li>
      {% endcapture %}
      {% assign tags_content = tags_content_temp %}
    {% endif %}
  {% endfor %}
{% endif %}

{{ tags_content }}
</ul>
{% endraw %}
{% endhighlight %}

***

### Lo que resta

Completamos con este artículo una de las etapas de la implementación de nuestro sistema de *tagging*. Nos queda crear las dos plantillas donde se muestren los resultados de esta suerte de "indexado por etiqueta", tarea que quedará documentada en el artículo siguiente...


FUENTES

+ [HOW TO USE TAGS AND CATEGORIES ON GITHUB PAGES WITHOUT PLUGINS](http://www.minddust.com/post/tags-and-categories-on-github-pages/)

+ [official Liquid Documentation](https://shopify.github.io/liquid/)
{: .fuentes}
