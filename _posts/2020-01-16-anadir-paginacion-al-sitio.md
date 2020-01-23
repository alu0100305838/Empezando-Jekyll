---
layout: post
title: Añadir paginación al sitio
date: 2020-01-16 03:00:00 -0300
category: jekyll
tags: [jekyll-plugin]
---

La plantilla (o *Theme*) *Jekyll Now!* es una plantilla muy simple, ideal para quien empieza un blog sin demasiados conocimientos sobre *Jekyll*. Pero al empezar a acumularse varios *posts* se empieza a extrañar una característica bastante común en cualquier página principal de un Blog: la división en páginas de la lista total de artículos del blog... estamos hablando de **paginación**.

Por suerte uno de los *plugins* seguros permitidos en GitHub es **jekyll-paginate**. En base a él construiremos el sistema de paginación en *index.html*

## Habilitar la paginación

El *post* previo mencionamos *resumidamente* algo sobre *Liquid* y sus objetos, que permiten crear plantillas básicas y no tanto para los distintos tipos de páginas de un sitio.

Desde la aparición de *Jekyll 3* es necesario el uso de un *plugin*  para poder generar la paginación, y como dijimos, existe uno en particular que opera sin problemas con *GitHub Pages*: **jekyll-paginate**... pero es necesario seguir algunos pasos para habilitarlo.

> Es importante señalar que al habilitar **jekyll-paginate** una nueva variable `paginator` se agregará a otras variables globales ya disponibles como `site`, `page`, etc.

### 1er paso

Suponiendo que se esté usando Bundler para la instalación de *Jekyll* y sus *gems* asociadas (*plugins maybe)*, habría que agregar la línea `gem 'jekyll-paginate'` al archivo Gemfile, y luego ejecutar:

{% highlight shell %}
$ bundle
{% endhighlight %}

Esto según aparece, entre otras, en el [README de  jekyll-paginate](https://github.com/jekyll/jekyll-paginate#installation).

*Pero esto presupone una instalación genérica de Jekyll, donde se usa `gem Jekyll` en el Gemfile.*

Nosotros [arrancamos nuestra instalación usando una gema particular, llamada github-pages](https://dsigno.github.io/Empezando-Jekyll/instalando-jekyll-en-local-II/#3-agregar-jekyll-usando-gem-github-pages), que aparte de *Jekyll* (versión 3.8.5 según recuerdo), instala los *plugins* denominados "seguros" por GitHub... entre ellos, *jekyll-paginate* (1.1.0 al día de la fecha).

> Conclusión: no tendremos que instalar *jekyll-paginate* (ya lo está), sólo habilitarlo.

NOTA: ver [en Stackoverflow](https://stackoverflow.com/a/35432329)

### 2do paso (para mí el primero)

Hay que escribir en el archivo `_config.yml` de manera de conseguir:

+ habilitar formalmente el *plugin*, que aparecerá en la lista `plugins:`
+ configurar la cantidad máxima  de ítems que se mostrarán por página.
+ el *path* donde aparecerán las páginas, por ejemplo, si la página inicialmente está en `/index.html`, se generará el producto de la paginación en  `/page2/`, `/page3/` y así sucesivamente.

{% highlight yaml %}
plugins:
  (...plugins previos...)
  - jekyll-paginate # lo agregué for enhancement

# Pagination - https://jekyllrb.com/docs/pagination/
paginate: 5
paginate_path: /page:num/
{% endhighlight %}

Ahora, cuando Jekyll compile nuestro sitio proporcionará una variable `site.paginator` de donde podremos extraer cada página de posts.

### 3er paso: cambiar index.html

En nuestro Blog las páginas se basarán en el archivo `index.html` de la raíz del sitio. Sin paginación aún, vemos que se genera con la variable `site.posts`.

{% highlight html %}
{% raw %}
---
layout: default
---

<div class="posts">
  {% for post in site.posts %}
    <article class="post">

      <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>

      <div class="entry">
        {{ post.excerpt }}
      </div>

      <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Read More</a>
    </article>
  {% endfor %}
</div>
{% endraw %}
{% endhighlight %}

El **nuevo** archivo `index.html`  que usaremos necesitará la nueva variable `paginator.post` ( *= posts available for the current page*); para aprovecharla, usaremos una estructura similar al del index anterior, como el siguiente:

{% highlight html %}
{% raw %}
---
layout: default
---

<div class="posts">
  {% for post in paginator.posts %}
  <article class="post">

    <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>

    <div class="entry">
      {{ post.excerpt }}
    </div>

    <a href="{{ site.baseurl }}{{ post.url }}" class="read-more">Leer Más</a>
  </article>
  {% endfor %}

  <nav class="post-nav">
    {% if paginator.next_page %}
      <p><a href="{{ site.baseurl }}{{ paginator.next_page_path }}">Más antiguo</a></p>
    {% endif %}

    {% if paginator.previous_page %}
      <p><a href="{{ site.baseurl }}{{ paginator.previous_page_path }}">Más nuevo</a></p>
    {% endif %}
  </nav>
</div>
{% endraw %}
{% endhighlight %}

Por supuesto que ahora las nuevas páginas *productos de la paginación* terminarán en una sección **nav** con dos links de navegación, que nos vinculan a los anteriores 5 posts, y a los siguientes 5 posts (si existieran)... *ya hemos visto esto en funcionamiento, ¡vamos!*

> Mi estrategia en particular es no cambiar la info presentada para cada *post* (sigo usando `post.excerpt`), y usar un esquema HTML para los links similar a lo ya usado para la *paginación entre posts* descripta en nuestra entrada anterior (con uso de flex-box).

Los estilos SCSS fueron cambiados muy poco a lo ya usado, igualmente vuelvo a presentarlo aquí...

{% highlight scss %}
.post-nav {
	display: flex;
	justify-content: space-between;
	flex-wrap: wrap;
    float: none;
	margin-top: 2em;
	border-top: 1px solid $lightGray;
	p {
		flex: 1 1 0;
		width: 45%;
    font-family: $Headers;
    font-size: 1.222em;
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

> #### Importante: la paginación solo funciona dentro de archivos HTML
>
> La paginación no funciona desde los archivos Markdown de su sitio Jekyll. La paginación funciona cuando se llama desde el archivo HTML, llamado index.html, que opcionalmente puede residir y producir paginación desde un subdirectorio, a través del valor de configuración paginate_path.

Esto es todo por ahora, faltaría mejorar lo mostrado agregando al menos la fecha de los posts... *quedará para otra entrada.*

***



FUENTES

+ [Liquid](https://shopify.github.io/liquid/)

+ [Jekyllrb - Pagination](https://jekyllrb.com/docs/pagination/)

+ [Jekyll Pagination](https://blog.webjeda.com/jekyll-pagination/)

+ [Crea tu propia página o blog gratis con Jekyll y Github: Parte 2 - Publicando posts](https://www.campusmvp.es/recursos/post/crea-tu-propia-pagina-o-blog-gratis-con-jekyll-y-github-parte-2-publicando-posts.aspx)
