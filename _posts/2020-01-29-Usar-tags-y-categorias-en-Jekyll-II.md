---
layout: post
title: Usar tags y categorías en Jekyll (II)
date: 2020-01-29 18:00:00 -0300
category: jekyll
tags: [jekyll-enhancement, tagging]
---
Este artículo tratará específicamente de cómo implementar en *Jekyll* —versión *GitHub Pages*— un sistema de *tagging* similar al encontrado en la mayoría de los Blogs que encontramos en la web.

Tal como vimos en la [entrada previa]({{ site.baseurl }}/Usar-tags-y-categorias-en-Jekyll-I/), existen distintas maneras de conseguir dicha implementación **sin apelar a un *plugin***; en lo particular, **mi elección fue crear una página individual por etiqueta**.

> Al momento de escribir esta entrada, tengo implementado —en parte— el sistema de *tagging* sobre el que me referiré de aquí en adelante... con el tiempo es probable que aparezcan cambios que trataré de dejar documentados (si las diferencias conceptuales así lo ameritan).

## *Tags* en *Jekyll*: un caso práctico (INTRO)

Durante mis búsquedas en la web para interiorizarme sobre el problema que nos ocupa, encontré diversas "variantes": la mayoría resolvían procedimentalmente la cuestión generando **una única página** para todos los *tags* (lo mismo para quien optara por una página asociada a las *categorías*). Muchas de ellas ya las mencioné como fuentes en el artículo previo, el que quizás merezca una *lectura explorativa*.

En mi caso quería ver si podía conseguir una *solución clásica*: una página por etiqueta. Por suerte encontré  un blog  —[Mindust](http://www.minddust.com/)— **no con una, sino dos soluciones a mi dilema...**

### Solución 1  /  [[LINK]](http://www.minddust.com/post/tags-and-categories-on-github-pages/)

> Esta solución fue propuesta en agosto de 2014, y **_no es_ la que terminé adoptando**.

Pueden mencionarse algunos detalles de esta solución, propuesta antes de la salida de Jekyll 3:

+ Las etiquetas seteadas en el *Front Matter* de los posts deberán ser *manualmente* ingresadas en un archivo llamado  `tags.yml`  a añadir en el directorio `_data`; podría lucir a algo parecido a esto:

{% highlight yaml %}
- slug: git
  name: Git

- slug: github-pages
  name: GitHub Pages

- slug: jekyll-plugin
  name: Jekyll Plugin  
{% endhighlight %}

  (Puede consultarse la [documentación oficial sobre Data files](https://jekyllrb.com/docs/datafiles/) )

+ Deben crearse las plantillas correspondientes a las páginas que van a mostrar los resultados de las consultas a las etiquetas —sean *tags* o categorías—. Están nombradas como **blog_by_tag** y **blog_by_category** (la otra solución también necesita de esto).

+ Para cada una de las etiquetas es necesario crear una *template* vacía, por ejemplo  **/tag/github-pages.md**

{% highlight yaml %}
---
layout: blog_by_tag
tag: github-pages
permalink: /tag/github-pages/
---
{% endhighlight %}

Si bien el artículo en cuestión está escrito de manera *escueta*, creo que es suficiente como para que alguien con algún conocimiento del funcionamiento de *Jekyll*, y de lenguaje *Liquid*,  pueda implementarlo (comprender detalles de la programación es otra cosa).



### Solución 2  /  [[LINK]](http://www.minddust.com/post/alternative-tags-and-categories-on-github-pages/)

El mismo autor —y en el mismo blog— propone en septiembre del 2016 una **solución alternativa** (a la que cataloga de [DRY: Don't Repeat Yourself (¡No Te Repitas!)](http://joaquin.medina.name/web2008/documentos/informatica/documentacion/logica/OOP/Principios/2012_07_30_OopNoTeRepitas.html) ).

Tal como se menciona en ese nuevo artículo, con el advenimiento de Jekyll 3 apareció una nueva característica llamada *[Jekyll Collections](https://jekyllrb.com/docs/collections/)* (sobre la cual me dedicaré en más detalle, posiblemente, en una futura entrada).

La estrategia que se sigue en este caso **es crear una colección para las *Tags*, y otra para las Categorías**... ¿Cual es la ventaja de ésta aproximación..?  En palabras del mismo autor:

> Instead of storing all information inside a data file + template I moved them to the front-matters section of their respective collection entry. A big advantage here is that there is just one place left to store the data.
>
> The magic comes from `output: true` which causes Jekyll to generate pages according to their `permalink` value.

En nuestras palabras:  con esta estrategia de solución se concentra la —de todas maneras necesaria— estructuración de datos en el *Front Matter* asociado a cada etiqueta. Al determinar que estamos trabajando con **Colecciones** (esto se hace en `_config.yml`), y al guardar cada `[any_tag].md`  en la **carpeta** dedicada a esa *Collection* específica (`_my_tags` o `_my_categories`,  usando la misma notación *arbitraria* que usa el autor del artículo ), conseguiremos facilitar la implementación del sistema de *Tagging*.

***

Hasta aquí *la presentación* de la problemática... para este blog voy a seguir esta segunda manera de resolver la creación de un sistema de *tagging* sin *plugins*.

En los artículos siguientes me ocuparé de detallar los cambios necesarios en la configuración del *blog*, a la creación de las entidades correspondientes a las dos colecciones, a los cambios necesarios en las *templates* para que se muestren las etiquetas necesarias (con sus *links*), y finalmente crear dos *templates* nuevas: una para las páginas asociadas a cada *Tag*, y lo mismo para cada *Categoría*.

*[continuará...]*

***

FUENTES

+ [ALTERNATIVE TAGS AND CATEGORIES ON GITHUB PAGES WITHOUT PLUGINS](http://www.minddust.com/post/alternative-tags-and-categories-on-github-pages/)

+ [Collections](https://jekyllrb.com/docs/collections/)
{: .fuentes}
