---
layout: post
title: Usar tags y categorías en Jekyll (VI)
date: 2020-02-10 00:00:00 -0300
category: jekyll
tags: [jekyll-enhancement, tagging, liquid]
---

La codificación que permite generar las **etiquetas/links** que agregamos en las *templates* `index.html` y `post.html` deben complementarse con la creación de dos nuevas *templates*, encargadas de posibilitar en nuestro entorno *Jekyll*  que se generen las páginas adonde  apuntarán los *links* antes mencionados.

Recordemos lo ya [escrito en otro *post*]({{site.baseurl}}/Usar-tags-y-categorias-en-Jekyll-III/) (referido al método de *tagging* a implementar):

> Una particularidad de lo que sigue es que sólo necesitaremos **crear el *Front Matter* para cada etiqueta**, ya que los contenidos propiamente dichos de la página asociada a cada etiqueta serán creados enteramente por la propia *template* ( `blog_by_category.html` o `blog_by_tag.html`, según sea el caso), la que sólo necesitará leer las variables almacenadas en cada *Front Matter*.

Estas dos nuevas *templates* son las que ya referenciamos en `_config.yml` como los *layouts* asociados a las colecciones **my_tags** y **my_categories**.  (Al final del mismo *post* explicamos por qué teníamos que crear estas plantillas —por el momento, y hasta este momento, en blanco— para evitar errores en el funcionamiento local de *Jekyll*).

Manos a la obra, pues.

## *Tags* en *Jekyll*: un caso práctico (*Liquid coding III*)

Acabamos de recordar que los contenidos *propiamente dichos* de las nuevas páginas que listarán los *posts* correspondientes a cada etiqueta son generados enteramente por las *templates*: sólo toman data de los *Front Matter* que encabezan los elementos de las colecciones  **my_tags** y **my_categories**.

Trataré en esta entrada ir un poco más allá de sólo transcribir el código de las nuevas *templates* para nuestro sitio... intentaré dar algunas pistas de como funciona el  *Liquid code*  que encontraremos en las mismas —es conveniente señalar que las plantillas que incorporaremos aquí serán prácticamente similares a las que pueden encontrarse en los repositorios [de su propio autor)](https://github.com/minddust/minddust.github.io/tree/master/_layouts) —.

> Importante: voy a intentar describir cómo funcionan estas dos nuevas *templates* tomando a sólo una de ellas como caso de estudio (ya que difieren mínimamente, y no en lo funcional). La metodología que usaré será ir probando estructuras más simples que la del resultado final, pero que respeten la idea del autor del programa.

### En camino a la *template* blog_by_tag.html

Al final de este *post*  aparecerán los códigos de las plantillas que estoy usando al día de la fecha, pero el objetivo de aquí en adelante será ir creando códigos "de prueba" que ayuden a entender como funcionan dichas plantillas.

#### Primera estructura "if / else"

El primer código a testear para  `blog_by_tag.html` es el siguiente:

{% highlight html %}
{% raw %}
---
layout: default
---

<header class="">
  <h1 class=""><small>Entradas con el TAG: </small>{{ page.name }}</h1>
</header>

<div class="container">
  {% if site.tags[page.slug] %}
    <!-- bucle principal -->
    {% for post in site.tags[page.slug] %}    
      <p>(aquí va un post...)</p>    
    {% endfor %}
    <!-- fin bucle principal -->
  {% else %}
    <p>No existen posts para este TAG.</p>
  {% endif %}
</div>
{% endraw %}
{% endhighlight %}

+ Aparece una estructura `header` conteniendo un `h1` destinado al título de la página. Recordemos que un ejemplo de lo que es un elemento de esta colección puede ser:

{% highlight liquid %}
  ---
  slug: github
  name: GitHub
  ---
{% endhighlight %}

El objeto `{% raw %}{{ page.name }}{% endraw %}` arrojará aquí el texto **GitHub** (la página corresponderá a este *tag*, *of course*).

+ El `div` siguiente encierra la estructura de decisión donde trabaja con el objeto  `site.tags[page.slug]`. (Según la [documentación de Jekyll](https://jekyllrb.com/docs/variables/#site-variables) , `site.tags.TAG` **lista todos los posts** con un *tag* igual a TAG).   

Entonces lo primero a evaluar es si  existe (si es *true*) algún post con el `page.slug` de la colección —podría existir el elemento en la colección y no haber ningún *post* que actualmente lo utilice— Si **no es** así conviene que exista un mensaje del tipo *"No existen posts para este TAG."*

+ En los casos en que exista al menos un elemento, se ejecuta un bucle `for` de manera de recorrer uno a uno los *posts* que hayan sido etiquetados con un *tag* que coincida con el `page.slug` —de aquí lo importante de no tener errores de escritura—

Como estamos en un proceso de prueba, dentro del bucle —que de aquí en más llamaremos "bucle principal"—  el contenido puede ser *por ahora* un párrafo cualquiera... Listo para probarse.

#### Una primera prueba ya funcional

Trabajaremos de aquí en más adentro del bucle principal. Lo que buscaremos es mostrar un listado con todos los *posts* asociados al *tag* que está siendo indexado por la nueva *template*.

+ Lo que se mostrará en la lista es una serie de *links* con el **título** de cada uno de estos *posts*, acompañado de la **fecha** de publicación que le corresponda. Por lo tanto, al ejecutarse el bucle va tomando cada objeto/post, y con los atributos **url**, **title** y **date** se puede armar cada uno de los *links* (*encapsulados* en un `div` que es, en principio, opcional).
+ En la expresión `{% raw %}{{ post.date | date: "%-d/%-m/%Y" }}{% endraw %}` vemos en acción un filtro pensado para ser usado con fechas, que permite mostrar una versión "corta" (consta sólo de números).

{% highlight html %}
{% raw %}
<!-- bucle principal -->
{% for post in site.tags[page.slug] %}
    <div class="list-group">
        <a href="{{ site.baseurl }}{{ post.url }}" class="list-group-item">
            <h4 class="list-group-item-heading">{{ post.title }}</h4>
            <p class="list-group-item-date">{{ post.date | date: "%-d/%-m/%Y" }}</p>
        </a>
    </div>
{% endfor %}
<!-- fin bucle principal -->
{% endraw %}
{% endhighlight %}

Probando nuestra *template* "en proceso" con este nuevo bucle principal, ya veremos un resultado que podría usarse sin mayores modificaciones como una versión funcional.

En mi caso trabajé con los estilos CSS para que quede algo como lo siguiente:

![listado de posts by tags]({{site.baseurl}}/images/listado-tags.PNG)

### Siguientes tests (muy poco funcionales)

En la *template* que vamos a implementar aparecerá el listado de posts pero *segmentado* de acuerdo al año en que fueron publicados... algo así:

![división por año]({{site.baseurl}}/images/listado-tags2.PNG)

Este *aparentemente simple* cambio complicará bastante la estructura que mostramos en el punto previo: la solución necesitará de nuevos *Liquid Tags*, y de implementar una estrategia extra para detectar en qué momento dividir la lista correspondiente a un año, para así comenzar una nueva lista para un año distinto.

#### Conociendo nuevos *Liquid tags*

Junto con operadores de  [Control de flujo](https://shopify.github.io/liquid/tags/control-flow/) más usuales en la mayoría de los lenguajes de programación, en *Liquid*  puede usarse `unless` , que opera de manera opuesta a un `if` —en `unless` el código encerrado se ejecutará si la condición analizada es **falsa**—

Mucho más versátil es la introducción en *Liquid* del **[objeto forloop](https://help.shopify.com/en/themes/liquid/objects/for-loops)** —que solo puede ser usado dentro de `for` tags—. La estructura que sigue es similar a la que usaremos en nuestra  *template*, y podríamos probar su funcionamiento reemplazando el anterior código usado en el bucle principal por éste, y ver los resultados sobre alguna página que indexe una etiqueta que tenga varios *post* asociados...

{% highlight html %}
{% raw %}
<!-- bucle principal -->
    {% for post in site.tags[page.slug] %}

      {% if forloop.first %}
        <p> => se muestra solo en el primer ciclo del loop</p>
      {% endif %}

      {% unless forloop.first %}
        <p> => se muestra si no es el primer ciclo del loop</p>
      {% endunless %}

		<p> => se muestra en todos los ciclos</p>

      {% if forloop.last %}
        <p> => se muestra solo en el último ciclo del loop</p>
      {% endif %}

    {% endfor %}
    <!-- fin bucle principal -->
{% endraw %}
{% endhighlight %}

Un caso particular que usaremos en breve es el atributo `forloop.index0` , que retorna el número de iteración en la que se esté trabajando, empezando la cuenta por 0 (ya veremos un ejemplo de uso).

#### Los posts y site.tags[TAG]

Ya comentamos que en el objeto del que hablamos se listan todos los *posts* con un *tag* determinado: es una de las *site variables* que *Jekyll* nos ofrece para poder trabajar. Un par de precisiones más sobre esto:

+ Los post estarán ordenados (en la lista) del más nuevo al más antiguo.
+ En ese ordenamiento recibirán un número que irá desde 0 hasta (n-1), por lo que una referencia como `site.tags[etiqueta][0]` nos permitirá trabajar con los atributos del primer objeto, y así sucesivamente.

Voy a mostrar ahora un nuevo código para el bucle principal que *me sirvió para entender* el funcionamiento de la *template* propuesta en [Mindust.com](http://www.minddust.com), donde se usan estos nuevos *Liquid tags*:

{% highlight liquid %}
{% raw %}
    <!-- bucle principal -->
    {% for post in site.tags[page.slug] %}
      {% capture post_year %}{{ post.date | date: '%Y' }}{% endcapture %}

      {% if forloop.first %}
        <p> => se muestra solo en el primer ciclo del loop</p>
        <p>index de este post: {{ forloop.index0 }}</p>
        <p>Resultado de [% raw %] {{ site.tags[page.slug][0].title }} [% endraw %] :{{ site.tags[page.slug][0].title }}</p>
      {% endif %}

      {% unless forloop.first %}
        {% assign previous_index = forloop.index0 | minus: 1 %}
        <p>index post anterior: {{ previous_index }}</p>
        {% capture previous_post_year %}{{ site.tags[page.slug][previous_index].date | date: '%Y' }}{% endcapture %}
        <p>año post listado previamente ({{ site.tags[page.slug][previous_index].title }}): {{ previous_post_year }}</p>
        <p>año este post: {{ post_year }}</p>
        {% if post_year != previous_post_year %}
          <p>años distintos; se debería cerrar la lista del año {{ previous_post_year }}, y abrirse la del año {{ post_year }}</p>
        {% else %}
          <p>seguimos en la lista del año {{ previous_post_year }}</p>
        {% endif %}
      {% endunless %}

        <h4 class="list-group-item-heading">{{ post.title }}</h4>
        <hr />

      {% if forloop.last %}
        <p> => se muestra solo en el último ciclo del loop</p>
      {% endif %}

    {% endfor %}
    <!-- fin bucle principal -->
{% endraw %}
{% endhighlight %}

+ En `post_year` se almacena el año del *post* que se esté "procesando" en cada uno de los *loops* del `for`: se consigue filtrando el atributo `.date`
+ En el primer ciclo probé algunos de los conceptos antes expuestos...
+ A partir del segundo ciclo comienza la comparación entre el año de publicación del *post actual* con la del *post* previamente analizado en el `for` —que es, a la sazón, "más nuevo"—.  Para ello se crea primero la variable `previous_index`  con la fórmula `forloop.index0 | minus: 1`, que resta 1 al `forloop.index0` de cada ciclo... para obtener el n° anterior.
+ Con la nueva variable podemos conseguir el año del *post* anterior (para el *loop*) y almacenarla en `previous_post_year`, usando `site.tags[page.slug][previous_index].date | date: '%Y'`
+ Comparando `post_year` y `previous_post_year` sabremos si hay un cambio o no.

Con las pistas dadas hasta aquí creo que puede entenderse el código... la página generada debería tener un aspecto similar al siguiente (está recortado para ver la parte donde hay cambio de año):

![coding4test]({{site.baseurl}}/images/coding4test.PNG)

Desde ya que lo mostrado no tiene nada que ver con el objetivo inicialmente buscado; la finalidad de este último código es experimental y pedagógica, ya que le permitirá entender al lector (creo) el funcionamiento de la plantilla definitiva que sigue...

### Por fin: la template blog_by_tag.html

Está casi todo dicho, solo recuerdo que la template `blog_by_category.html` es prácticamente similar a la que vamos a mostrar aquí —los cambios más obvios son pasar de `site.tags[page.slug]` a  `site.categories[page.slug]`—

Desde ya que aparecerá una estructura html similar a la usada en la primera versión —simple— que mostramos antes en el subítem **Una primera prueba ya funcional**

#### blog_by_tag.html

{% highlight liquid %}
{% raw %}
---
layout: default
---

<header class="">
  <h1 class=""><small>Entradas con el TAG: </small>{{ page.name }}</h1>
</header>

<div class="container">
  {% if site.tags[page.slug] %}
  	<!-- bucle principal -->
    {% for post in site.tags[page.slug] %}
      {% capture post_year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% if forloop.first %}
        <h3 class="year">{{ post_year }}</h3>
        <div class="list-group">
      {% endif %}
      {% unless forloop.first %}
        {% assign previous_index = forloop.index0 | minus: 1 %}
        {% capture previous_post_year %}{{ site.tags[page.slug][previous_index].date | date: '%Y' }}{% endcapture %}
        {% if post_year != previous_post_year %}
        </div>
        <h3 class="year">{{ post_year }}</h3>
        <div class="list-group">
        {% endif %}
      {% endunless %}
        <a href="{{ site.baseurl }}{{ post.url }}" class="list-group-item">
          <h4 class="list-group-item-heading">{{ post.title }}</h4>
          <p class="list-group-item-date">{{ post.date | date: "%-d/%-m/%Y" }}</p>
        </a>
      {% if forloop.last %}
        </div>
      {% endif %}
    {% endfor %}
    <!-- fin bucle principal -->
  {% else %}
    <p>No existen posts para este TAG.</p>
  {% endif %}
</div>
{% endraw %}
{% endhighlight %}

***


FUENTES

+ [HOW TO USE TAGS AND CATEGORIES ON GITHUB PAGES WITHOUT PLUGINS](http://www.minddust.com/post/tags-and-categories-on-github-pages/)

+ [Variables - Jekyll](https://jekyllrb.com/docs/variables/#site-variables)

+ [official Liquid Documentation](https://shopify.github.io/liquid/)

+ [Control de flujo](https://shopify.github.io/liquid/tags/control-flow/)
{: .fuentes}
