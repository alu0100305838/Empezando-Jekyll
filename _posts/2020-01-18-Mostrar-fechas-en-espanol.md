---
layout: post
title: Mostrar fechas en español
date: 2020-01-18 08:00:00 -0300
category: jekyll
tags: [jekyll-enhancement]
---
Quizás pueda parecer un tema menor o accesorio, pero lo cierto es que en la implementación de las *GitHub Pages* (y su limitado juego de *plugins*) no se ha tenido suficiente consideración con los idiomas —que no sea el inglés, *of course*—, y particularmente sobre un punto: **el formato en que se muestran las fechas de los posts**...

Vamos a intentar de corregir esto, y de paso presentaremos otra de las prestaciones de gran utilidad el el lenguaje *Liquid*: el uso de `includes`.

## Las fechas en *Jekyll*

Empecemos por lo obvio: la documentación oficial de *Jekyll* nos dá la info sobre los dos atributos de fecha sobre los que vamos a trabajar, asociadas a las variables `page` y `post`.

#### `page.date`

FUENTE:  [Jekyllrb - Page Variables](https://jekyllrb.com/docs/variables/#page-variables)

> **The Date assigned to the Post**. This can be overridden in a Post’s front matter by specifying a new date/time in the format `YYYY-MM-DD HH:MM:SS` (assuming UTC), or `YYYY-MM-DD HH:MM:SS +/-TTTT` (to specify a time zone using an offset from UTC. e.g. `2008-12-14 10:30:00 +0900`).

#### `post.date`

FUENTE:  [Jekyllrb - Predefined Variables for Posts](https://jekyllrb.com/docs/front-matter/#predefined-variables-for-posts)

> **A date here overrides the date from the name of the post**. This can be used to ensure correct sorting of posts. A date is specified in the format `YYYY-MM-DD HH:MM:SS +/-TTTT`; hours, minutes, seconds, and timezone offset are optional.

No es casualidad que las fechas (*dates*) estén asociadas al formato *post* como tipo de contenidos, ya los *posts* son los   elementos básicos constitutivos de los sitios del tipo **Blog**... y en un blog todo *post* tiene asociado —al menos— una **fecha** (*timestamp*), un array con la lista de *tags*, y un autor.

De hecho los nombres de los posts en Jekyll tienen la forma `YEAR-MONTH-DAY-titulo.MARKUP`

### Ejemplos

Tomando la idea del trabajo de Alan W. Smith /  [Jekyll Date Formatting Examples](http://alanwsmith.com/jekyll-liquid-date-formatting-examples) mostraremos algunos casos del uso de *Outputs* de fechas... para esta misma página.  Salvo el primer *simple* caso, en los otros se apela al uso de filtros —más adelante diremos algo sobre esta cuestión—

<table>
  <thead>
    <tr>
      <th>Marcador [Object | Filter]</th>
      <th>resultado</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>{% raw %} {{ page.date }} {% endraw %}</td>
      <td> {{ page.date }} </td>
    </tr>
    <tr>
      <td> {% raw %} {{ page.date | date: '%B %d, %Y' }} {% endraw %} </td>
      <td> {{ page.date | date: '%B %d, %Y' }} </td>
    </tr>
    <tr>
      <td> {% raw %} {{ page.date | date_to_string }} {% endraw %} </td>
      <td> {{ page.date | date_to_string }} </td>
    </tr>
    <tr>
      <td> {% raw %} {{ page.date | date: "%A, %B %-d, %Y" }} {% endraw %} </td>
      <td> {{ page.date | date: "%A, %B %-d, %Y" }} </td>
    </tr>
    <tr>
      <td> {% raw %} {{ page.date | date: "%-d/%-m/%Y" }} {% endraw %} </td>
      <td> {{ page.date | date: "%-d/%-m/%Y" }} </td>
    </tr>
  </tbody>
</table>

De los casos mostrados, únicamente el último (que usa sólo números) puede llegar a servirnos.

Pero también aparece una opción en principio más compleja, pensada para el idioma alemán, que paso a mostrar:

{% highlight liquid %}
{% raw %}

<!-- Whitespace added for readability -->
{% assign m = page.date | date: "%-m" %}
{{ page.date | date: "%-d" }}
{% case m %}
  {% when '1' %}Januar
  {% when '2' %}Februar
  {% when '3' %}M&auml;rz
  {% when '4' %}April
  {% when '5' %}Mai
  {% when '6' %}Juni
  {% when '7' %}Juli
  {% when '8' %}August
  {% when '9' %}September
  {% when '10' %}Oktober
  {% when '11' %}November
  {% when '12' %}Dezember
{% endcase %}
{{ page.date | date: "%Y" }}

{% endraw %}
{% endhighlight %}

Que dará como resultado: {% assign m = page.date | date: "%-m" %}
{{ page.date | date: "%-d" }}
{% case m %}
  {% when '1' %}Januar
  {% when '2' %}Februar
  {% when '3' %}M&auml;rz
  {% when '4' %}April
  {% when '5' %}Mai
  {% when '6' %}Juni
  {% when '7' %}Juli
  {% when '8' %}August
  {% when '9' %}September
  {% when '10' %}Oktober
  {% when '11' %}November
  {% when '12' %}Dezember
{% endcase %} {{ page.date | date: "%Y" }}

...esto ya puede ser más útil.

#### Explicación

En la línea 2 se vé  el uso del tag de creación de una variable (**m** en este caso) al que se le asigna un valor numérico que es el del mes correspondiente a la fecha del post: esto se logra **filtrando** la información presente en el atributo `page.date` , con uno de los filtros que ofrece *Liquid*, llamado `date:` —[ver info](https://shopify.github.io/liquid/filters/date/)—

Los 3 componentes de la fecha se construyen con 3 etiquetas de texto <code>{% raw %} {{  text output  }} {% endraw %}</code>: la primera y la tercera nos dan los números del día y del año, que se obtienen de nuevo con el filtro `date:`; para el nombre del mes se apela al *tag* `case`, que usa {% raw %} {%  when 'x'  %} {% endraw %}  donde se compara la variable creada **m** con los 12 valores numéricos posibles.

## Solución que implementaré

Como ya alguien escribió sobre una solución que toma este acercamiento, y con una explicación muy didáctica, empiezo mencionando la fuente: [Conversión de fecha a formato español sin plugins](https://bujonodi.github.io/jekyll/jekyll/update/2017/05/07/date-es.html)

Tal como se aclara allí, se usará la etiqueta `<time></time>` para insertar las fechas, ya que es una etiqueta de HTML5 utilizada para escribir fechas; —es muy recomendable usarla para optimizar el SEO—.

Se intenta conseguir algo como:

{% highlight html %}
  {% raw%}
<time datetime="2020-01-08" attr1="value1" attr2="value2">8 de enero del 2020</time>
  {% endraw%}
{% endhighlight %}

### ¿Dónde debería aparecer la fecha?

En principio, cada vez que aparezcan los contenidos de cada *post*... ya dijimos que es uno de los atributos fundamentales de este tipo de textos: es importante para el lector poder *ubicarse* con respecto al momento en que fue escrito el artículo.

Tendremos por lo tanto —al menos— dos situaciones:

+ En la página correspondiente a cada *post*: generalmente acompaña al título, aunque en *Jekyll Now!* el creador de la plantilla optó por ponerla como cierre del artículo —yo *por ahora* voy a optar por ofrecer las dos opciones—. Quedaría algo así...

  {% highlight html %}
    {% raw %}
  <article class="post">
    <h1>{{ page.title }}</h1>

    {% include date_es.html date=page.date time-attributes=' class="post-date"' %}

    <div class="entry">
      {{ content }}
    </div>

    <div class="date">
      Escrito el {% include date_es.html date=page.date %}
    </div>
    (...)
    {% endraw %}
  {% endhighlight %}

  > En estos casos se trabaja con los valores que obtenemos desde el atributo `page.date`

  NOTA: ya explicaré lo de  <code>{% raw %}{% include...%}{% endraw %}</code>, pero es donde aparecerá el contenido `{% raw %}<time> ... </time>{% endraw %}`  

+ En las páginas que indexan grupos de *posts* (o de sus *excerpt* o extractos) , como son las páginas productos de la paginación del `index.html` inicial, el autor de la plantilla  *Jekyll Now!* no contempló incluir la fecha. Nosotros corregiremos esta situación, pero invocaremos el include con otro atributo...

	{% highlight html %}
  {% raw %}
  <div class="posts">
  {% for post in paginator.posts %}
  <article class="post">

    <h1><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h1>

    {% include date_es.html date=post.date time-attributes='class="post-date"' %}

    <div class="entry">
      {{ post.excerpt }}
    </div>
    (...)
  {% endraw %}
  {% endhighlight %}

***

> En estos casos se trabaja con los valores que obtenemos desde el atributo `post.date`

### Ahora sí, hablemos del include...

El tipo de solución del que hablamos al principio (la pensada para el idioma alemán), dado que se va a usar al menos en **dos** de las templates *Jekyll* (en `_layouts/post.html` y en `index.html`), conviene que se resuelva usando un archivo del tipo **include**...

> En *Jekyll*, la etiqueta **include** le permite incluir el contenido de otro archivo almacenado en la carpeta `_includes`:

En nuestro caso el archivo —que deberemos crear— será `_includes/date_es.html` , y en los archivos se incluirá usando una etiqueta como  <code>{% raw %} {% include date_es.html %} {% endraw %}</code> —en nuestro caso es un poco más complejo—

> *Jekyll* buscará el archivo referenciado (en este caso, `date_es.html`) en el directorio `_include` en la raíz de su directorio fuente e insertará su contenido.

Es buen momento de presentar el contenido del archivo `date_es.html`, que se encargará de generar el contenido encerrado entre etiquetas `time`, como ya vimos:

{% highlight liquid %}
{% raw %}
<time
  datetime="{{ include.date }}"

  {% if include.time-attributes %}
    {{ include.time-attributes}}
  {% endif %}
>

  {{  include.date | date: '%-d' }} de

  {% assign m = include.date | date: '%-m' %}
  {% case m %}
    {% when '1' %}enero
    {% when '2' %}febrero
    {% when '3' %}marzo
    {% when '4' %}abril
    {% when '5' %}mayo
    {% when '6' %}junio
    {% when '7' %}julio
    {% when '8' %}agosto
    {% when '9' %}septiembre
    {% when '10' %}octubre
    {% when '11' %}noviembre
    {% when '12' %}diciembre
  {% endcase %}

  del

  {{  include.date | date: '%Y' }}
</time>
{% endraw %}
{% endhighlight %}

### ...aunque sea un include especial

El caso que usamos es un poco *atípico* con respecto a los que generalmente se presentan, que tienen la forma simple <code>{% raw %} {% include nombre_de_archivo.html %} {% endraw %} </code>

Tal como se explica en el artículo [Conversión de fecha a formato español sin plugins](https://bujonodi.github.io/jekyll/jekyll/update/2017/05/07/date-es.html) :

> En este fichero tenemos dos variables que debemos pasarle al usar <code>{% raw %}{% include %}{% endraw %} </code> :
>
> + `include.date`, que es la fecha que queremos formatear. Se debe pasar en el formato `YYYY-MMMM-DD HH:mm:ss`.
> + `include.time-attributes`, que son una serie de atributos extra que llevará la etiqueta `` (opcional).

Este tipo de uso de los *includes* es muy potente, ya que permite implementar en *Liquid* algo parecido a lo que son las funciones... (se pasan parámetros, y se obtienen distintas respuestas).

Recordemos que en nuestros dos casos de uso, variará el atributo  asignado al `date` del *include*, y que es *recibido* en el `date_es.html` , donde se invoca a través de `include.date` (releer hasta entender).

***

Para una completa comprensión de lo expuesto en este *post*, sugiero visitar los *links* citados en el transcurso de su desarrollo (aunque el idioma a veces dificulte las cosas)...



***

FUENTES

+ [Liquid](https://shopify.github.io/liquid/)

+ [ES Liquid para diseñadores](https://github.com/Shopify/liquid/wiki/ES-Liquid-para-diseñadores)

+ [Jekyllrb - Liquid](https://jekyllrb.com/docs/liquid/)

+ [Jekyll Date Formatting Examples](http://alanwsmith.com/jekyll-liquid-date-formatting-examples)

+ [Conversión de fecha a formato español sin plugins](https://bujonodi.github.io/jekyll/jekyll/update/2017/05/07/date-es.html)

+ [Jekyll - fechas en español](https://dmts.es/post/jekyll-fechas-en-espaol/)

+ [Using Jekyll _includes as custom liquid functions](https://hamishwillee.github.io/2014/11/13/jekyll-includes-are-functions/)
{: .fuentes}
