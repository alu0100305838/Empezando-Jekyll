---
layout: post
title: Usar tags y categorías en Jekyll (III)
date: 2020-01-30 12:00:00 -0300
category: jekyll
tags: [jekyll-enhancement, tagging]
---

Como ya anticipamos en el artículo anterior, la solución que tomaremos para la creación de un *sistema de tagging* en nuestro *blog Jekyll* requerirá de la creación de dos [*Jekyll Collections*](https://jekyllrb.com/docs/collections/) , característica que apareció con la versión 3 de *Jekyll*.

En esta entrada describiremos los pasos para conseguir esto...

## *Tags* en *Jekyll*: un caso práctico (*Collections*)

Por si no leíste [mi *post* previo]({{ site.baseurl }}/Usar-tags-y-categorias-en-Jekyll-II/): la estrategia que seguiremos comienza con **crear una colección para las *Tags*, y otra para las Categorías**.

Para ello, deberemos comenzar con *intervenir* el principal archivo de configuración de nuestro sistema...

### Cambios al  _config.yml

Como vamos a usar **dos colecciones** en la implementación, debemos comenzar **definiéndolas** en el archivo de configuración principal de nuestro *blog* (recomiendo *echar una mirada* a la info presentada en [la info oficial](https://jekyllrb.com/docs/collections/)).

{% highlight yaml %}
# Collections...
collections:
  my_categories:
    output: true
    permalink: /category/:name/
  my_tags:
    output: true
    permalink: /tag/:name/
{% endhighlight %}

Hay que notar que:

+ Los nombres asignados aquí a las colecciones ( `my_tags` y `my_categories` ) son arbitrarios, pero una vez elegidos tendrán que ser usados también para los nombres de las carpetas asignadas a cada colección, antecedidos por un *underscore* (_) .
+ Conviene que los *permalinks* incluyan en el *path* un nombre orientativo para el usuario del blog.
+ Es muy importante  que se incluya la opción `output: true` para que el generador de páginas procese los contenidos de cada colección de manera apropiada.

Veremos más adelante que las páginas que agruparán los *post* asociados a cada etiqueta necesitarán de plantillas *ad hoc*, que llamaremos `blog_by_category.html` y `blog_by_tag.html` , las que se almacenarán en la carpeta `/_layouts`. Conocido esto, en vez de escribir en el *Front Matter* de cada etiqueta la información del layout que le corresponde, podemos indicarlo en nuestro archivo `_config.yml`, en la sección `defaults:`, como sigue...

{% highlight yaml %}
defaults:
  -
    scope:
      path: ""
      type: pages
    values:
      layout: page
  -
    scope:
      path: ""
      type: posts
    values:
      layout: post
  -
    scope:
      path: ""
      type: my_categories
    values:
      layout: blog_by_category
  -
    scope:
      path: ""
      type: my_tags
    values:
      layout: blog_by_tag
{% endhighlight %}

La notación es específica de YAML, los detalles de estos *defaults* pueden consultarse en [Front Matter Defaults](https://jekyllrb.com/docs/configuration/front-matter-defaults/) , pero básicamente estamos indicando que para los contenidos relacionados a la colección `my_categories` (*ergo*, las páginas que listarán los *post* vinculados a cada una de nuestras categorías) le corresponderá un layout llamado `blog_by_category`; lo mismo para el caso de la colección `my_tags`.

### Creación de las entradas para cada etiqueta

Una particularidad de lo que sigue es que sólo necesitaremos **crear el *Front Matter* para cada etiqueta**, ya que los contenidos propiamente dichos de la página asociada a cada etiqueta serán creados enteramente por la propia *template* ( `blog_by_category.html` o `blog_by_tag.html`, según sea el caso), la que sólo necesitará leer las variables almacenadas en cada *Front Matter*.

Para dar un **ejemplo** , para el *tag* específico **Jekyll plugin** deberemos crear un archivo llamado `jekyll-plugin.md`, y guardarlo en la carpeta `_my_tags` correspondiente a la colección . Su contenido consistirá (al menos) de algo como lo siguiente:

{% highlight yaml %}
---
slug: jekyll-plugin
name: Jekyll plugin
---
{% endhighlight %}

> La variable `slug` sirve para indicar sel nombre que recibirá la página (suele escribirse en minúscula y reemplazarse espacios en blanco con guiones), y la variable `name` es el nombre con que aparecerá la etiqueta en pantalla.

Al momento de crear el sistema de *tagging*, conviene tener a mano un registro "prolijo" de las etiquetas que estemos utilizando —o sepamos que vamos a utilizar—, para así crear un archivo markdown (**.md**) para cada una de ellas, archivos que conformarán los elementos de las colecciones respectivas.

Desde ya, a medida que sigamos trabajando es posible que aparezca la necesidad de creación de nuevas etiquetas (generalmente *tags*), lo que exigirá que se acompañen de nuevos ingresos en la colección.

### Aclaraciones finales

+ Es importante señalar que a diferencia los sistemas de *tagging* que encontraremos en CMSs tipo WordPress,  nuestra solución para *Jekyll*  requerirá de otro nivel de "atención", ya que el etiquetado no está totalmente automatizado —deberemos recordar de hacer los ingresos de los elementos para cada nueva etiqueta en la/las colecciones—.

+ Es necesario anticipar que sucederá si intentamos correr *Jekyll* a modo local con esta nueva configuración:  en la terminal recibiremos avisos como el siguiente...

{% highlight shell %}
$ bundle exec jekyll serve
Configuration file: /home/theRaven/Shared/GitHub_repos/Empezando-Jekyll/_config.yml
   Build Warning: Layout 'blog_by_category' requested in _my_categories/github-pages.md does not exist.
   Build Warning: Layout 'blog_by_category' requested in _my_categories/jekyll.md does not exist.
   Build Warning: Layout 'blog_by_tag' requested in _my_tags/git.md does not exist.
   Build Warning: Layout 'blog_by_tag' requested in _my_tags/github.md does not exist.
   ...
{% endhighlight %}

​	Esto sucede porque en los defaults de `_config.yml` invocamos a dos *layouts* que todavía no creamos; para evitar esto, conviene que añadamos en la carpeta `_layouts` los archivos **blog_by_tag.html** y **blog_by_category.html**, por ahora sin contenido (esa tarea la dejaremos para más adelante).

El próximo post empezamos con cuestiones de programación usando Liquid, para conseguir los cambios necesarios en las *templates* y así conseguir que se muestren las etiquetas asociadas a cada página (con sus *links*).

*[continuará...]*

***

FUENTES

+ [ALTERNATIVE TAGS AND CATEGORIES ON GITHUB PAGES WITHOUT PLUGINS](http://www.minddust.com/post/alternative-tags-and-categories-on-github-pages/)

+ [Collections](https://jekyllrb.com/docs/collections/)

+ [Front Matter Defaults](https://jekyllrb.com/docs/configuration/front-matter-defaults/)
{: .fuentes}
