---
layout: post
title: Usar tags y categorías en Jekyll (I)
date: 2020-01-23 15:00:00 -0300
category: jekyll
tags: [jekyll-enhancement]
---
Una de las posibilidades que ofrece *GitHub Pages* —no la única— es la de poder generar un sitio de tipo **blog**, caracterizado por basarse en una **estructura de tipo cronológica** (de allí la importancia de la fecha de publicación de los artículos).

La navegación "interna" por un blog queda en parte atada a esta secuencia temporal de *posts*, a lo que se agrega algún sistema de busqueda interna, y también aquella navegación que se posibilita vinculando a los distintos contenidos con un *sistema de etiquetado*: básicamente estamos hablando del uso de **tags** y/o **categorías** como modo de estructurar los vínculos en nuestro blog.

## Implementar el Etiquetado en GitHub Pages

Posiblemente este debe ser uno de los temas relacionados a *Jekyll* más tratados en foros y sitios *más o menos* especializados en este generador de sitios estáticos. Como las distintas versiones de *Jekyll* han presentado cambios en el tiempo, una primera sugerencia para quien busque implementar prestaciones vinculadas al etiquetado en su blog, es que deberían *revisar la fecha* de la información analizada.

Suele leerse que, de base,  *Jekyll* no soporta el etiquetado: en realidad en el [Front Matter](https://jekyllrb.com/docs/front-matter/#predefined-variables-for-posts) de cada *post* del blog pueden añadirse datos asociados a 3 variables predefinidas, llamadas `tags` y  `category` ( o `categories` si corresponde) . Lo que es muy importante notar es que *Jekyll*  **no trae de base** la habilidad de generar contenidos donde se use la información de esas etiquetas al servicio del usuario.

> En un blog es deseable que junto con cada *post* se muestren las etiquetas que el autor del contenido asignó al mismo, y que cada una de dichas etiquetas se enlacen a páginas *automatizadas* donde se recopilen (*indexen*) todos los *posts* que compartan dicho **tag** o **categoría**.

Tal como ocurre con el caso que vimos de añadir la capacidad de *paginación* en *Jekyll* a través de un *plugin*, es posible implementar un sistema de *tagging* usando esta estrategia.

Pero a diferencia de lo anterior, para el caso específico de implementación de un blog a través del uso de *GitHub Pages* —que es el que nos ocupa en este artículo— no existe ningún *plugin* que nos permita esta prestación dentro del [grupo *safe*](https://pages.github.com/versions/) que están habilitados por defecto en GitHub.

Aún así, **es posible la implementación de un sistema de *tagging* sin necesidad de recurrir al uso de un *plugin***. De esta cuestión nos ocuparemos de aquí en adelante.

### ¿Usar *tags*, categoría, o ambas?

Parece conveniente echar un poco de luz sobre  estos términos, ya lo lo vamos a usar en nuestra implementación.

Quizás una fuente orientativa sobre la cuestión venga del modo que se toman estos términos en el ámbito de *WordPress*, el CMS más utilizado para la creación de blogs (como curiosidad, solo las etiquetas —*labels*— son contempladas en la organización de los blogs de *Blogger*).

Según *WordPress*:

> Las **categorías**  sirven para hacer una agrupación más amplia de tus entradas. Has de  verlas como temas generales o como una tabla de contenidos de WordPress. Están ahí para ayudar a identificar la temática de tu blog. Y para para ayudar a los visitantes a encontrar el tipo de contenido que  buscan. Y no menos importante, **las categorías son jerárquicas, por lo que puedes tener subcategorías**.  
>
> Por otro lado, las **etiquetas** tienen el propósito de  describir los detalles específicos de tus entradas. Has de verlas como a palabras clave que aparecen en un hipotético índice de tu web. Son la  micro-información que se puede utilizar para micro-clasificar tus  contenidos. Y a diferencia de las categorías, **las etiquetas no son jerárquicas**.  
>
> FTE: [Categorías y etiquetas en WordPress](https://neliosoftware.com/es/blog/categorias-y-etiquetas-en-wordpress/)

#### ¿Y en *Jekyll*?

En principio, al no estar presente el manejo del etiquetado en lo que el sistema básico ofrece, queda en nuestras manos seguir o no el criterio taxonómico antes expuesto.  Parece un buen criterio, dado que los visitantes de *innumerables* blogs ya tienen incorporada esa metodología, por la exposición a esa estrategia de categorización de contenidos, que lo sigamos en nuestro blog.
Entonces, **nuestro criterio** a seguir será:

+ cada *post* podrá pertenecer a una sola categoría
+ cada *post* podrá estar asociado a una indeterminada cantidad de *tags*

En cualquier caso, será importante que tengamos clara cuales serán las categorías con las que contará el blog (requiere una mirada de *planificación*): generalmente conviene mantener su número acotado.

#### Un ejemplo de Front Matter que incorpora etiquetado

No es más que un ejemplo acotado de un "encabezado" de un post; notar si que se usa el criterio de escribir la categoría y los *tags* en minúscula.

{% highlight yaml %}
---
layout: post
title: Git para crear modo local
date: '2020-01-09 22:00:00 -0300'
category: github-pages
tags: [github, jekyll-local, git]
---
{% endhighlight %}


## Formas de implementación

Del análisis de las formas de resolver *la creación de un sistema de tagging sin usar un plugin* que se muestran en la web, puede señalarse:

+ Existe una parte de la solución que es común a todos los casos, ya que es necesaria para cualquier sistema de tagging: la generación de un *link* (ya veremos *hacia dónde*) por cada uno de los ***tags*** asociados al *post*; lo mismo vale para la **categoría**. Visualmente quedaría algo como esto...

  ![tags]({{site.baseurl}}/images/tags.PNG)

  Lo normal es que estos links estén *invariablemente presentes* en las páginas donde se muestra el contenido completo de cada *post*, y *opcionalmente presentes* en la página donde se indexan todos los *posts* del blog (en nuestro caso en `index.html`, nuestra página principal).

  NOTA: en realidad, ya agregamos un sistema de paginación en nuestro sitio, así que la `index.html` original se encuentra "segmentada" en varias páginas, conteniendo hasta 5 *posts* cada una.

+ La segunda parte necesaria de la solución **aparece en dos variantes**, y tiene que ver con *hacia dónde* apuntan los *links* mencionados en el punto anterior:

  + En una de las variantes de solución se generan dos páginas —una para los *tags* y otra para las categorías— donde se mostrarán los títulos de cada *post* reagrupados siguiendo al *tag* (o categoría, según el caso) correspondiente; veamos un ejemplo visual:

  ![jekyll-categories]({{site.baseurl}}/images/jekyll-categories.svg)
  	FTE: ([3 Simple steps to setup Jekyll Categories and Tags](https://blog.webjeda.com/jekyll-categories/))  


  Estas páginas suelen estar acompañadas por una *nube de tags*, que sirve como un medio de alternar entre las distintas etiquetas (presentes en la misma página).

  Aunque es una manera práctica de solucionar la "muestra de resultados", especialmente si el blog es relativamente "modesto" en cuanto a cantidad de contenidos, no es la forma "tradicional" que se adopta en la mayoría de los blogs.

  + En la otra variante de solución nos encontraremos con una página dedicada (separada) para cada una de las etiquetas, con el conjunto de los *posts* asociados a veces agrupados por el año de publicación. Como es el tipo de respuesta más habitual de encontrar, no voy a abundar en detalles.

    En estas páginas el uso de la *nube de tags* es menos usual: es más probable que esta característica aparezca en otros lugares del blog (en el *sidebar*, por ejemplo).


***

A muy grandes rasgos, este es el panorama de soluciones que nos podemos encontrar en la web, gracias a la gentileza de desarrolladores con suficiente conocimiento de programación en *Liquid*, y de su uso dentro del entorno *Jekyll*.

## ¿Que implementación elegir?

Desde el punto de vista funcional, cualquiera de las dos *variantes de solución* son válidas, y por lo tanto decidir será más que nada una cuestión de gustos.

Desde el punto de vista de la implementación, la variante que contempla la generación de *sólo dos páginas* para indexar los *posts* (suponiendo que optemos por usar tanto las **categorías** como los ***tags*** en nuestro sitio) parece más simple, al menos de comprender, y es la que generalmente encontraremos en las búsquedas por la web.

De cualquier manera,  ambas maneras involucran una gran componente de programación en *Liquid* ([ver info](https://shopify.github.io/liquid/)), y entender las *minucias* o estrategias usadas por el programador puede no ser simple, aunque vale el intento... ya que así también se aprende.

> Como los detalles de la implementación que seguí los voy a dejar para un próximo *post* , cierro este con un *link* a un [ejemplo de la variante 1](http://pavdmyt.com/tags/), ya que en lo personal elegí la opción de generar una página por etiqueta (o sea, la variante 2).

***

Dejo a consideración alguna de las fuentes de información sobre la implementación del sistema de *tagging* que miré para escribir este *post*...



FUENTES

+ [Codinfox](https://codinfox.github.io/dev/2015/03/06/use-tags-and-categories-in-your-jekyll-based-github-pages/)
+ [How to implement Tags at Jekyll website](http://pavdmyt.com/how-to-implement-tags-at-jekyll-website/)
+ [3 Simple steps to setup Jekyll Categories and Tags](https://blog.webjeda.com/jekyll-categories/)
+ [Jekyll Tags on Github Pages](https://longqian.me/2017/02/09/github-jekyll-tag/)
+ [Use Tags and Categories in your Jekyll based Github (cached)](http://webcache.googleusercontent.com/search?q=cache:http://ju.outofmemory.cn/entry/132959)
+ [Alphabetizing Jekyll Page Tags In Pure Liquid (Without Plugins)](https://blog.lanyonm.org/articles/2013/11/21/alphabetize-jekyll-page-tags-pure-liquid.html)
