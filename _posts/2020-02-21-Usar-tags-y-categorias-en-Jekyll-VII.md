---
layout: post
title: Usar tags y categorías en Jekyll (bonus track)
date: 2020-02-21 23:00:00 -0300
category: jekyll
tags: [jekyll-enhancement, tagging, liquid]
---
Habiendo conseguido ya poner en funcionamiento nuestro sistema de *tagging*, posibilitando así el indexado de nuestros posts tomando como base a sus etiquetas ¿qué nos queda agregarle?

Es común encontrar en diversos sistemas de blogs el uso de la llamada **nube de tags**, que no es más que la representación en un mismo lugar del conjunto de etiquetas definidas en el sitio, acompañadas con una manera de informar sobre su *frecuencia* (la cantidad de *posts* vinculadas a la misma).

Dentro del entorno de *Jekyll* la manera más rápida de resolver esto sería a través de algún *plugin*... pero como ya nos ha sucedido al tratar de ver como implementar el *tagging*, dentro del ámbito específico de las *GitHub Pages* no existe un plugin que lo posibilite —por eso de las *safe option* ([ver lista](https://pages.github.com/versions/))—.

En la presente entrada se mostrará una manera de crear una nube de tags muy simple para usar con nuestro sistema de etiquetado recién implementado... por ahora mostraremos esas nubes el pie de las páginas de indexado por *tag* o por categoría.

## Nube de *Tags* en *Jekyll* : una (muy simple) versión

Dado que vamos a usar el mismo código para mostrar la *susodicha* nube en las templates **blog_by_tag.html** y **blog_by_category.html** , nos convendrá crear un nuevo *include*, al que llamaremos **tags_cloud.html** , y que guardaremos —como corresponde— en la carpeta `_includes` de nuestro proyecto.

El código es (basado en parte en la solución propuesta en **superdevresources.com**):

{% highlight html %}
{% raw %}
{% comment %}
=======================
The purpose of this snippet is: crear una nube de tags
=======================
{% endcomment %}

{% if site.tags %}
<div class="container-l2">

<h3 class="cloud">Tag Cloud</h3>
{% assign tags = site.tags | sort %}
<ul class="taxonomy">
{% for tag in tags %}
  {% for var in site.my_tags %}
    {% if var.slug == tag[0] %}
      {% assign tagname = var.name %}
    {% endif %}
  {% endfor %}

  <li class="site-tag">
    <a href="{{ site.baseurl }}/tag/{{ tag | first }}/">
      {{ tagname }} ({{ tag | last | size }})
    </a>
  </li>
{% endfor %}
</ul>

</div>
{% endif %}
{% endraw %}
{% endhighlight %}

Y el resultado (convenientemente *estilizado* vía CSS) visualmente quedaría algo como...

![tag-cloud]({{site.baseurl}}/images/tag-cloud.png)

El código mostrado (y adaptado para su uso con nuestra colección `my_tags`) comienza con el ordenamiento alfabético de los contenidos de `site.tags` , seguido por la consulta dentro de la mencionada colección `my_tags` —recorriéndola con el `for var in site.my_tags`— para encontrar el texto con el que construiremos el *link* en el paso siguiente —que se almacenará en la variable  `tagname`—.

Junto a este texto se mostrará un número que indica el tamaño del *array* de *posts* asociados al *tag* en cuestión, que es la manera más simple de indicar la cantidad de ocurrencias de cada etiqueta.

***

Lo que resta es añadir al final de las *templates* **blog_by_tag.html** y **blog_by_category.html** el siguiente código *include*:

{% highlight liquid %}
{% raw %}
{% include tags_cloud.html %}
{% endraw %}
{% endhighlight %}

## Cierre del tema *tagging*

Este es el séptimo y último *post* vinculado a este tema, en donde traté de plasmar de manera lo más minuciosa posible (...) la implementación de un sistema de *tagging* siguiendo la idea propuesta en **minddust.com**... Nos vemos en la próxima entrada, hablando posiblente de la implementación de alguna mejora en nuestro sitio (*¡quedan varias..!*).

***


FUENTES

+ [Generating Tag cloud on Jekyll blog without plugin](https://superdevresources.com/tag-cloud-jekyll/)

+ [Mindust: ALTERNATIVE TAGS AND CATEGORIES ON GITHUB PAGES WITHOUT PLUGINS](http://www.minddust.com/post/alternative-tags-and-categories-on-github-pages/)
{: .fuentes}
