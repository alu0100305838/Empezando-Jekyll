---
layout: post
title: Usar tags y categorías en Jekyll (IV)
date: '2020-02-02 13:00:00 -0300'
category: jekyll
tags:
  - jekyll-enhancement
  - tagging
  - liquid
published: true
---

Para que un sistema de *tagging* funcione es necesario que las etiquetas asignadas a cada *post* aparezcan disponibles al visitante del blog en forma de *links*.

En este artículo nos concentraremos en esta cuestión, y para ello tendremos que adentrarnos en la **programación con *Liquid***, que nos permitirá agregar nuevo código a dos de las *templates* que veníamos usando (`index.html` y `post.html`) , y así automatizar la visualización de los *links/etiquetas*... como ejemplo, para los *tags* deberíamos conseguir algo como esto:

![tags]({{site.baseurl}}/images/tags.PNG)

## *Tags* en *Jekyll*: un caso práctico (*Liquid coding*)

No es momento de escribir un tratado sobre *Liquid* —tampoco podría—, pero conviene que demos un marco orientativo sobre el ámbito en que nos moveremos.

+ Algunos se refieren a *Liquid* como un lenguaje de plantillas, mientras que otros lo conocen como motor. No importa el término se utilice, ambas formas son correctas en muchos sentidos.  Tiene una sintaxis (como en todos los lenguajes tradicionales de programación), tiene conceptos tales como salida, lógica y circuitos, e interactúa con variables (datos), tal y como se haría con un lenguaje centrado en la web, como con PHP.

+ Los diseñadores y desarrolladores de sitios web pueden usar un lenguaje de plantilla para crear páginas web que combinen **contenido estático**, que es el mismo en varias páginas, y **contenido dinámico**, que cambia de una página a la siguiente. Un lenguaje de plantilla permite reutilizar los elementos estáticos que definen el diseño de una página web, mientras que *llena dinámicamente la página con datos* de un sitio *Jekyll*.  

+ **Los elementos estáticos están escritos en HTML y los elementos dinámicos están escritos en *Liquid***. Los elementos *Liquid* en un archivo actúan como marcadores de posición: cuando el código en el archivo se compila y se envía al navegador, el *Liquid* se reemplaza por datos del sitio *Jekyll* donde está instalado.

Al final de este *post* pueden encontrarse entre las fuentes listadas documentación sobre los elementos básicos necesarios para programar la generación del contenido dinámico en *nuestro blog Jekyll.*

### Haciendo unos tests muy básicos

Voy a comenzar con el problema más simple: mostrar en la página correspondiente a un *post* la **categoría** a que pertenece. Esto implica que vamos a trabajar sobre la *template* `post.html`, para poder ver algo como lo siguiente:

![category]({{site.baseurl}}/images/category.PNG)

Un lugar donde podríamos buscar la información requerida es en el *Front Matter* del mismo *post* ; pongo un ejemplo usando una de las entradas de este *blog*...

{% highlight yaml %}
---
layout: post
title: Conociendo Prose
date: '2019-12-28 06:00:00 -0300'
category: github-pages
tags: [github]
---
{% endhighlight %}

Una tentación sería tratar de resolver la cuestión agregando el siguiente código en `post.html`, en el lugar donde queremos que aparezca la nueva información:

{% highlight html %}
{% raw %}
<p class="post-cat">
	{% if page.category %}Post en Categoría: {{ page.category }}{% endif %}
</p>
{% endraw %}
{% endhighlight %}

> En *Liquid* los *tags* de control aparecerán  con la notación `{% raw %}{% tag %}{% endraw %}`, y los atributos de los objetos aparecerán como salida de texto en la página si están encerrados entre llaves dobles... algo como `{% raw %}{{ objeto.atributo }}{% endraw %}`

La explicación del simple código propuesto es: **si existe** un valor para la variable `category` en el *Front Matter* de la página, aparecerá insertado en el HTML en el lugar señalado por el marcador de texto (`page.category` vale en el ejemplo **github-pages**).

Es importante mencionar que hay [otras variables asociadas a cada página](https://jekyllrb.com/docs/variables/#page-variables) que no necesitan figurar en el Front Matter, y que pueden usarse dentro de una determinada codificación.

***

Ahora bien, ¿obtuvimos la salida especificada en la imagen? El texto mostrado es **github-pages** y no **GitHub Pages**  ¿Dónde encontrar el texto "GitHub Pages"?

#### A mirar la colección my_categories

En el post previo explicamos como crear dos colecciones donde almacenar la información de las distintas etiquetas a usar en el blog; en particular la colección **my_categories**  contendrá la info relacionada a las categorías.

Al momento de escribir este *post* estoy probando el uso de dos categorías, a saber:

+ GitHub Pages
+ Jekyll

Y por tanto en la carpeta `_my_categories` almaceno dos elementos para esta colección,

+ github-pages.md

{% highlight yaml %}
---
slug: github-pages
name: GitHub Pages
---
{% endhighlight %}

+ jekyll.md

{% highlight yaml %}
---
slug: jekyll
name: Jekyll
---
{% endhighlight %}

Como vemos, lo que debería hacer es tomar la información almacenada en la variable `category` del *Front Matter* del *post* (por ej. **github-pages**) , buscar en la colección *my_categories* que categoría tiene su variable  `slug` almacenando ese texto, y de allí obtendremos el texto **GitHub Pages**, nombre con que se mostrará la categoría en la página (¿que lío, no?).

#### Una estructura característica en *Liquid*

Por razones que quizás tengan que ver con su origen ligado a las "tiendas en línea", en *Liquid* es común encontrar codificación usando un *for loop* como el siguiente:

{% highlight liquid%}
{% raw %}
{% for product in collection.products %}
	{{ product.title}}
{% endfor %}
{% endraw %}
{% endhighlight %}

El bucle anterior recorrerá todos los *productos* de la *colección* y ejecutará lo que esté dentro del bloque de código. En este caso, generará el título del producto.

Aquí aparece la cuestión importante: existen los objetos (*products*), que pertenecen a una *colección*, y que poseen **atributos** (en nuestro caso, *title*). La documentación de Liquid señala que  "los objetos contienen atributos que se utilizan para mostrar contenido dinámico en una página", **pero los objetos también pueden contener otros objetos**. Los objetos en *Liquid* a veces son iterables (casi como una matriz) y, a veces, se puede llamar un objeto sin un atributo y recibir un solo valor.

#### Volvamos a lo nuestro

En nuestro caso tenemos una colección de objetos (llamados categorías), objetos cuyos datos están almacenados en el *Front Page* de (por ahora) dos archivos. Estos datos junto con otros valores generados por el sistema constituirán los atributos de dichos objetos.

***

Para probar el funcionamiento de esto vamos a agregar *temporariamente* este código en la *template* `post.html`:

{% highlight html%}
{% raw %}
{% for var in site.my_categories %}
    <ol>
      <li> {{ var.name }} </li>
      <li> {{ var.slug }} </li>
      <li> {{ var.url }} </li>
	</ol>
{% endfor %}
{% endraw %}
{% endhighlight %}

Lo que veremos como resultado del trabajo de este código será:

{% raw %}
1- GitHub Pages  
2- github-pages  
3- /category/github-pages


1- Jekyll  
2- jekyll  
3- /category/jekyll/
{% endraw %}
Lo que hace el pequeño *snippet* es tomar cada elemento de la colección con que estamos trabajando (se invoca usando `site.my_categories`)  y listar 3 de sus atributos: `name`, `slug` y `url` (los dos primeros son los datos que cargamos en los archivos de la colección).

Ya podemos responder dónde encontrar el texto "GitHub Pages" —es el primer item de la primera lista—, que es un atributo del objeto que también tiene como atributo el texto `github-pages`... el segundo item de la primera lista.

El truco que necesitamos es ejecutar la escritura necesaria **solo si** el atributo `slug` coincide con el valor que hayamos puesto en el  *Front Matter* de la página a la variable category (o sea `page.category`).

La nueva prueba a realizar en la *template*  `post.html` será entonces:

{% highlight html%}
{% raw %}
{% for var in site.my_categories %}
    {% if var.slug == page.category %}
    {% assign category_name = var.name %}
	<p>{{ category_name }}</p>
	{% endif %}
{% endfor %}
{% endraw %}
{% endhighlight %}

El uso de la nueva  variable `category_name` es opcional, es sólo otra manera de asignar una variable de texto...

### Un cierre adecuado

El objetivo de este artículo es generar  la información de la **categoría** en la cual enrolamos nuestros *posts*, con su *link* correspondiente... algo como esto:

![category]({{site.baseurl}}/images/category.PNG)

Con los recursos que ya hemos explorado nos alcanzará para armar la codificación necesaria, sería algo así:

{% highlight html%}
{% raw %}
{% for var in site.my_categories %}
  {% if var.slug == page.category %}
    {% capture category_link %}
    <a class="label" href="{{ site.baseurl }}{{ var.url }}">{{ var.name }}</a>
    {% endcapture %}
  {% endif %}  
{% endfor %}
<p class="post-cat">
  {% if category_link %}Post en Categoría: {{ category_link }}{% endif %}
</p>
{% endraw %}
{% endhighlight %}

El lugar donde insertar este *snippet* en nuestra *template* `post.html` quedaría a nuestra consideración —incluso podría insertarse a través de un *include* [como vimos en otro post](https://dsigno.github.io/Empezando-Jekyll/Mostrar-fechas-en-espanol/)—

***

Como detalle final, el estilo CSS aplicado a las etiquetas fue tomado de [este CodePen](https://codepen.io/wbeeftink/pen/dIaDH).

Para el artículo siguiente nos quedará el etiquetado para los *tags*...



FUENTES

+ [ALTERNATIVE TAGS AND CATEGORIES ON GITHUB PAGES WITHOUT PLUGINS](http://www.minddust.com/post/alternative-tags-and-categories-on-github-pages/)

+ [Blogs de Shopify. Qué es Liquid, el lenguaje para...](https://es.shopify.com/blog/liquid-lenguaje-para-la-creacion-de-plantillas-en-shopify)

+ [Jekyllrb - Liquid](https://jekyllrb.com/docs/liquid/)

+ [official Liquid Documentation](https://shopify.github.io/liquid/)

+ [Shopify Liquid – The Ultimate Guide](https://www.christhefreelancer.com/shopify-liquid-guide/)
{: .fuentes}
