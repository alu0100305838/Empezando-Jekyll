---
layout: post
title: Añadir paginación en posts
date: 2020-01-14 04:00:00 -0300
tag: Jekyll-enhancement Liquid
---

Culminada la primera etapa de creación del sitio en *GitHub Pages*, del clonado y "espejado" posterior del mismo en un *pacientemente* creado "entorno local" —léase, en nuestra máquina—, se abre un enorme campo de acción, prueba y desarrollo... y fundamentalmente de aprendizaje sobre *Jekyll*, *Git* y demás.

En este post nos ocuparemos de contar como agregar en nuestro muy simple *Theme* llamado *Jekyll Now!* la posibilidad de navegación entre posts, usando los habituales links PREV y NEXT, que suelen cerrar los contenidos propiamente dichos del artículo.

Y en la medida de lo posible, trataremos como siempre de aportar alguna nota conceptual o teórica a nuestros contenidos; en este caso aparecerán sobre el lenguaje de marcado **Liquid**, que es el que se usa para la creación de plantillas en *Jekyll*.

## Añadir *links Prev-Next* en los posts

Empecemos por un ejemplo de lo que queremos lograr, mostrando los resultados a conseguir en un *hipotético* post, y un posible código HTML asociado:

{% highlight html %}
<div class="post-nav">
 <p>
    <a href="/useful-emacs-shotcuts/">&#8672;&nbsp;Useful Emacs Shortcuts</a>
 </p>
 <p>
    <a href="/consume-less-create-more/">Consume Less, Create More&nbsp;&#8674;</a>
 </p>
</div>
{% endhighlight %}

*Desde ya que esto podría hacerse post tras post de manera "no automatizada"*, pero estamos trabajando con un generador de sitios como *Jekyll*: veremos como de manera muy simple puede generarse un código o  *snippet* que se agregará en las plantillas generadoras de los posts.

### ¿Por qué usar *Prev-Next navigation*?

Al tipo de Blogs que estamos creando los usuarios llegan casi con seguridad a través de buscadores como *Google Search*, *DuckDuckGo*, etc. Si el contenido les interesa,  agregar un *enlace de publicación siguiente-anterior* y/o  *enlaces de publicaciones relacionadas* es una buena manera de involucrar a su audiencia con más publicaciones y, si las publicaciones están segmentadas "en capítulos", les resulte más fácil llegar por una *vía interna* al nuevo contenido.

## ¿Qué elementos nos ofrece *Liquid* para conseguir esto?

Desde el sitio oficial de *Jekyll* se ofrece una demasiado suscinta [información sobre Liquid](https://jekyllrb.com/docs/step-by-step/02-liquid/) , destaquemos que presenta los tres elementos que permiten la elaboración y control de las plantillas: los tres ingredientes básicos son los **objetos**, las **etiquetas** y los **filtros**.

> *Liquid*, al igual que cualquier lenguaje de plantillas, crea un puente entre un archivo HTML y un registro de datos. En nuestro contexto, desde luego que el dato es generado por *Jekyll*. Esto es posible al permitirnos acceder a variables desde el interior de una plantilla con una sintaxis fácil de leer y de utilizar.

Podemos hablar de dos grupos de elementos de marcado (*markup*) en *Liquid*: elementos *de salida* y elementos *de Tag*.

+ Los elementos de salida (o resultado, *output*) son aquellos que se expresan como texto y van entre llaves dobles: `{% raw %}{{ elemento }}{% endraw %}`
+ Los elementos de tag, que contienen la lógica de programación, son aquellos que no pueden ser expresados como texto y van entre: `{% raw %}{%  %}{% endraw %} `


> Otra forma de interpretar los delimitadores es como "marcadores". Un marcador puede ser apreciado como un **segmento de código** que finalmente será sustituido por **datos** en cuanto la plantilla compilada se envíe al explorador. Este dato lo define completamente el diseñador de la interfaz como resultado del código Liquid en la plantilla. Así, las plantillas Liquid, al igual que las plantillas que entrelazan PHP y HTML, sirven como representaciones de lo que se mostrará gráficamente.

### Objetos/Variables

Los **objetos** son marcas del tipo `{% raw %}{{ nombre }}{% endraw %}` que permiten sustituir variables por su valor en una página. Por ejemplo, el código `{% raw %}{{ site.title }}{% endraw %}` en una plantilla resultaría en una cabecera de primer nivel conteniendo el título del sitio web.

Los principales objetos que se pueden usar al componer plantillas para *Jekyll* son `site` y `page`.

+ El primero contiene las variables que forman parte de la configuración del sitio y el contenido (por ejemplo, los *posts*).
+ El segundo alberga los metadatos de la página que se está renderizando, de forma que cuando una plantilla se aplique a varias páginas, `page` **tendrá diferentes datos para cada una**. Este objeto tiene algunos datos de uso común, como `page.title` o `page.date`. Además, en los datos iniciales o *front matter* de cada página se pueden establecer variables personalizadas que serán accesibles como propiedades de `page`.

Puedes encontrar la [referencia completa de variables disponibles por defecto](https://jekyllrb.com/docs/variables/) en la documentación de Jekyll.

## Uniendo todo

De la documentación previamente citada, tomamos información vinculada el objeto `page`

| variable        | descripción                                                  |
| --------------- | ------------------------------------------------------------ |
| `page.next`     | The next post relative to the position of the current post in `site.posts`. Returns `nil` for the last entry. |
| `page.previous` | The previous post relative to the position of the current post in `site.posts`. Returns `nil` for the first entry. |

Traducido: para cada *post* incluido dentro de la lista almacenada en `site.posts` —una lista cronológicamente inversa de todos los posts— se dispone de estas 2 variables que referencian los posts anterior y posterior (si existieran). Es justo lo que necesitamos.

Si volvemos al código HTML del principio, notamos que necesitaremos conocer:

+ dos nombres de página: los conoceremos con `page.previous.title` y `page.next.title`
+ dos links: los sabremos con `page.previous.url` y `page.next.url`



Usando nuestros nuevos conocimientos relacionados a Liquid, el código necesario quedaría:

{% highlight html %}
{% raw %}
<div class="post-nav">
 {% if page.previous.url %}  
 <p>
    <a href="{{page.previous.url}}">&larr;&nbsp;{{ page.previous.title }}</a>
 </p>
 {% endif %}
 {% if page.next.url %}
 <p>
    <a href="{{page.next.url}}">{{ page.next.title }}&nbsp;&rarr;</a>
 </p>
 {% endif %}
</div>
{% endraw %}
{% endhighlight %}



Aparecen los códigos de control `{% raw %}{% if  algo %}{% endraw %}` y `{% raw %}{% endif %}{% endraw %}`, usados para determinar que los *links* aparezcan sólo si ese `algo` resulta `TRUE`... son códigos condicionales.

### Código HTML y CSS

Como estoy siguiendo la solución planteada en [Bytedude](https://www.bytedude.com/jekyll-previous-and-next-posts/) (que usa estilos con *flexbox* en los CSS) voy a mostrar el código allí propuesto:

A) código HTML a insertar en su *post layout file*

{% highlight html %}
{% raw %}
<div class="post-nav">
  <p>
    {% if page.previous.url %}
    <a href="{{page.previous.url}}">&#8672;&nbsp;{{page.previous.title}}</a>
    {% endif %}
  </p>
  <p>
    {% if page.next.url %}
    <a href="{{page.next.url}}">{{page.next.title}}&nbsp;&#8674;</a>
    {% endif %}
  </p>
</div>
{% endraw %}
{% endhighlight %}


B) código CSS para los links

{% highlight css %}
.post-nav {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
}

.post-nav p {
    flex: 1 1 0;
    width: 45%;
}

.post-nav p:first-child {
    padding-right: 0.5em;
}

.post-nav p:last-child {
    padding-left: 0.5em;
    text-align: right;
}
{% endhighlight %}

### Probando la solución

Como suele pasar, *anda pero no del todo*.  La solución no contempla la variable  `{{ site.baseurl }}`, necesaria para *github pages* de proyectos...


{% highlight html %}
{% raw %}
<div class="post-nav">
  <p>
    {% if page.previous.url %}
    <a href="{{ site.baseurl }}{{page.previous.url}}">&#8672;&nbsp;{{page.previous.title}}</a>
    {% endif %}
  </p>
  <p>
    {% if page.next.url %}
    <a href="{{ site.baseurl }}{{page.next.url}}">{{page.next.title}}&nbsp;&#8674;</a>
    {% endif %}
  </p>
</div>
{% endraw %}
{% endhighlight %}


Por último, un leve cambio visual en la parte superior de los links, y el cambio de CSS a SCSS en los estilos:

{% highlight scss %}
.post-nav {
	display: flex;
	justify-content: space-between;
	flex-wrap: wrap;
	margin-top: 2em;
	border-top: 1px solid $lightGray;
	p {
		flex: 1 1 0;
		width: 45%;
		&:first-child {
			padding-right: 0.5em;
		}
		&:last-child {
			padding-left: 0.5em;
			text-align: right;
		}
	}
}
{% endhighlight %}

**¡Eureka!** Ya tenemos los links PREV y NEXT funcionando en nuestros posts...

***

FUENTES

[Liquid](https://shopify.github.io/liquid/)

[Qué es Liquid, el lenguaje para la creación de plantillas en Shopify](https://es.shopify.com/blog/liquid-lenguaje-para-la-creacion-de-plantillas-en-shopify)

[Jekyll – how to link to next/previous post on your blog](http://david.elbe.me/jekyll/2015/06/20/how-to-link-to-next-and-previous-post-with-jekyll.html)

[Bytedude - Jekyll: Previous And Next Posts](https://www.bytedude.com/jekyll-previous-and-next-posts/)

[How to Create a Previous and Next Post Link Within a Category Using Jekyll and Liquid](https://jttyeung.github.io/2017/11/12/how-to-create-a-previous-and-next-post-link-within-a-category-using-jekyll-and-liquid.html)

[Webjeda - How to Add Related or Previous Next Pagination in Jekyll?](https://blog.webjeda.com/related-post-jekyll/)
