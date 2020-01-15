---
layout: post
title: Instalando Jekyll en local (II)
date: 2020-01-12 15:00:00 -0300
tag: Jekyll-local
---

Continuamos ahora con la segunda parte de la descripción del proceso de instalación de *Jekyll* en nuestras computadoras, de manera de generar nuestro sitio estático en modo local, y permitir así probar los cambios antes de subirlos a *GitHub*.

En un *post* anterior comenzamos la descripción este proceso de intalación, sobre  un SO Linux Mint. Nos ocupamos allí en particular de los **requisitos previos**: deben estar instalados algunos programas que permitan ejecutar *Jekyll* en nuestra computadora, requisitos que repetimos aquí...

+ [Ruby](https://www.ruby-lang.org/en/downloads/) version **2.4.0** o superior.
+ [RubyGems](https://rubygems.org/pages/download)
+ [GCC](https://gcc.gnu.org/install/) y [Make](https://www.gnu.org/software/make/)

Añadimos a esta lista a  [**Bundler**](http://bundler.io/), ya que en la mayoría de la documentación que habla sobre la instalación de *Jekyll* —inclusive [la oficial](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/)— recomiendan el uso de la gema *Bundler*, ya que permite incluso usar distintas versiones de *Jekyll* y de sus *plugins* para diferentes proyectos, sin generar conflictos.

## Instalación de *Jekyll*

Esta simple cuestión me presentó (por falta de conocimiento y/o de información, quizás) algunas dudas y varios problemas en una instalación de prueba previa, donde entre otras cosas, no usé *Bundler*.

> Uno de los puntos importantes a definir es: ¿se van a  usar los servicios *standard* de *GitHub Pages*, o el sitio se alojará en un servidor de sitios estáticos común y corriente? Esto tiene implicancias definidas, [ya que la versión de Jekyll y la lista de *plugins* seguros que se habilitan en GitHub Pages](https://pages.github.com/versions/) es limitada.

El caso más genérico de implementación —y usando Bundler—, está detallado en el sitio oficial, en [Using Jekyll with Bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/) .  Voy a seguir paso a paso estas instrucciones

NOTA: ya tengo instalado el software citado como requisito.

### 1. Inicializar Bundler

Nuestro sitio ya fue clonado desde GitHub al directorio `~/Shared/GitHub_repos/Empezando-Jekyll` de mi Linux Mint. Debemos hacer dicho directorio nuestro directorio de trabajo.

Allí ejecutaremos `bundle init`

{% highlight shell %}
theRaven@ravennest:~$ cd ~/Shared/GitHub_repos/Empezando-Jekyll/
theRaven@ravennest:~/Shared/GitHub_repos/Empezando-Jekyll$ bundle init
Writing new Gemfile to /home/theRaven/Shared/GitHub_repos/Empezando-Jekyll/Gemfile
{% endhighlight %}

Se inicia un nuevo proyecto Bundler, creando un archivo llamado `Gemfile`.

### 2. Configurar Bundler

Este paso es opcional, pero se recomienda. Vamos a configurar *Bundler* para instalar gemas en el subdirectorio del proyecto ` ./vendor/bundle/` . Esto nos permite instalar nuestras dependencias en un **entorno aislado**, asegurando que no entren en conflicto con otras gemas en su sistema. Si omite este paso, *Bundler* instalará sus dependencias globalmente en su sistema.

{% highlight shell %}
bundle install --path vendor/bundle
{% endhighlight %}
Con esto se creará `~/Shared/GitHub_repos/Empezando-Jekyll/vendor/bundle/ruby/2.5.0/`

> ##### Bundler Config es persistente
>
> Este paso es *solo requerido una vez* por proyecto.  *Bundler* guarda su configuración en  `./.bundle/config`, de esa manera las futuras gemas serán instaladas en el mismo directorio (`BUNDLE_PATH: "vendor/bundle"`)

### 3. Agregar Jekyll (usando *gem 'github-pages'*)

Veamos. Las instrucciones originales indican:

> Ahora, vamos a usar Bundler para agregar Jekyll como una dependencia de nuestro nuevo proyecto. Este comando agregará la gema Jekyll a nuestro Gemfile y la instalará en la carpeta  `./vendor/bundle/` folder.  
>
> ​		`bundle add jekyll`

***

** Es en este punto donde voy a hacer cambios respecto a las instrucciones pensadas para instalar un *Jekyll* genérico, a una versión pensada para *GitHub Pages* **

***

#### Un modelo de gemfile

Cuando se genera un sitio Jekyll usando Bundler, desde cero, via `bundle exec jekyll VERSION new`  (se instala con la plantilla *minima*), se crea un `Gemfile` como el que sigue...

{% highlight ruby %}
source "https://rubygems.org"
# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
gem "jekyll", "~> 4.0.0"
# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", "~> 2.5"
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins
# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end
(...más líneas continúan)
{% endhighlight %}

Como vemos, la instalación genérica de *Jekyll* puede usarse con *Github Pages*, siguiendo las indicaciones del sitio oficial de *GitHub*, en el documento [Creating a GitHub Pages site with Jekyll](https://help.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll#creating-your-site).   

Allí se señala *entre otras cosas* hacer un cambio en el archivo `Gemfile`  tal como lo indican las instrucciones incluidas en forma de comentario en el mismo archivo...

Suscintamente: se pide cambiar el uso de la gema **jekyll** por la gema **github-pages** , aunque en el punto 9) se agrega el uso de una fórmula como la siguiente...

{% highlight shell %}
 gem "github-pages", "~> VERSION", group: :jekyll_plugins
{% endhighlight %}

reemplazando *VERSION* con la versión de dependencia actual para github-pages —`gem 'github-pages', '~> 203'` al momento de escribir este texto—


> Nota: en [otras documentaciones](https://help.github.com/en/enterprise/2.14/user/articles/setting-up-your-github-pages-site-locally-with-jekyll#step-2-install-jekyll-using-bundler) aparecen instrucciones similares...  
>
> If you already have a Gemfile, open your favorite text editor, such as [Atom](https://atom.io/), and add these lines to your Gemfile:  
>
>   {% highlight shell %}
    source 'https://rubygems.org'
    gem 'github-pages', group: :jekyll_plugins
    {% endhighlight %}

#### Ahora, en mi gemfile

Como ya tengo tema instalado *(Jekyll Now! , remember...*) voy a agregar al *Gemfile* que creó el `bundle init` lo siguiente:

{% highlight ruby %}
gem 'github-pages', '~> 203' , group: :jekyll_plugins
{% endhighlight %}

#### Por fin, a instalar...

Tan simple como correr  `bundle install`. El proceso demora un tiempo —son más de 50MB a instalar—, y requiere conexión a internet. Tal como configuramos *Bundler*, los archivos con las gems y demás se instalan en el *path* `vendor/bundle`

Esta imagen muestra las nuevas carpetas. En un momento veremos como agregar la carpeta vendor y algunas más a la lista del archivo `.gitignore` ya que es contenido vinculado a los datos o a la plataforma de trabajo específica del usuario (de hecho podrían haberse instalado en una carpeta de caracter más "global" en nuestro SO)

![Carpetas instaladas en vendor/]({{site.baseurl}}/images/carpetas-instaladas-en-vendor.PNG)

##### Importante

> La gema **github-pages** permite instalar *Jekyll* y los complementos de una sola vez.  
>
> Nos permite tener un *mirror* en nuestra máquina local de los complementos (*plugins*) utilizados por *GitHub Pages* , incluidos *Jekyll*, *Sass*, etc.  
>
> Nota 1: You are not required to install Jekyll separately
>
> Nota 2: La versión de *Jekyll* que se instaló fue la **3.8.5**, y la sugerida en el *gemfile* original era `jekyll (~> 4.0.0)`

### 4. Sirviendo el sitio

¡El nuevo sitio web está listo! Se puede servir el sitio web con `bundle exec jekyll serve` y visitarlo en [Server address]: http://127.0.0.1:4000/Empezando-Jekyll/ (recordemos que estamos trabajando en un *Project website*, no en *User website*, que no usa subdirectorio).

Desde aquí, está listo para continuar desarrollando el sitio por su cuenta. Todos los comandos normales de Jekyll están disponibles para usted, **pero debe prefijarlos con** `bundle exec` para que Bundler ejecute la versión de Jekyll que está instalada en su carpeta de proyecto.

#### Troubleshooting

Al intentar servir por primera vez el sitio , ocurrió un problema que impidió arrancar el servidor. El mensaje de error incluía:

{% highlight shell %}l
Generating...
ERROR: YOUR SITE COULD NOT BE BUILT:
------------------------------------
Invalid date '<%= Time.now.strftime('%Y-%m-%d %H:%M:%S %z') %>': Document 'vendor/cache/gems/jekyll-3.2.1/lib/site_template/_posts/0000-00-00-welcome-to-jekyll.markdown.erb' does not have a valid date in the YAML front matter.
{% endhighlight %}

Por suerte es un error ampliamente comentado, y la solución aparece en la misma web oficial de *Jekyll* en [Configuration problems](https://jekyllrb.com/docs/troubleshooting/#configuration-problems): solo basta agregar carpetas vinculadas a *vendor* en la sección `exclude:`  de *_config.yml*. A mí me quedó así...

{% highlight ruby %}
exclude:
  - Gemfile
  - Gemfile.lock
  - LICENSE
  - README.md
  - CNAME
  - node_modules
  - vendor/bundle/
  - vendor/cache/
  - vendor/gems/
  - vendor/ruby/
  # - any_additional_item # any user-specific listing goes at the end
{% endhighlight %}



### 5. *Commit* a sistemas de control de versiones

Si está almacenando su nuevo sitio en el control de versiones Git, querrá ignorar las carpetas `./vendor/` y `./.bundle/` ya que contienen información específica del usuario o de la plataforma. Los nuevos usuarios podrán instalar las dependencias correctas basadas en  `Gemfile` y `Gemfile.lock`, que deberían estar registradas. Puede usar este `.gitignore` para comenzar, si lo desea. [NOTA: *traducción de la info oficial*]

**.gitignore**

{% highlight shell %}
# Ignore metadata generated by Jekyll
_site/
.sass-cache/
.jekyll-cache/
.jekyll-metadata

# Ignore folders generated by Bundler
.bundle/
vendor/
{% endhighlight %}

Considerando el tamaño de los contenidos almacenados en `vendor/` este cambio no es menor...

***

## Para concluir

***¡Ya podemos probar nuestro sitio en local!***  Una vez conforme con los primeros cambios, hay que sincronizar con el repositorio remoto en GitHub tal como vimos en el post anterior, usando el comando  `git push origin gh-pages`

De cualquier manera, la forma de trabajo más adecuada es abrir un nuevo *branch* (rama) en nuestro *git* local —con algún nombre descriptivo como *development*—, probar las innovaciones en esa rama, para luego hacer un `merge` en local hacia la rama principal *gh-pages*...

Ya hablaremos del tema ramas en *git* en un próximo post.


***

FUENTES

[Using Jekyll with Bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/)

[Setting up your GitHub Pages site locally with Jekyll](https://help.github.com/en/enterprise/2.14/user/articles/setting-up-your-github-pages-site-locally-with-jekyll#step-2-install-jekyll-using-bundler)

[Creating a GitHub Pages site with Jekyll](https://help.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll#creating-your-site) —desde cero—

[HTML::Pipeline dependences](https://github.com/jch/html-pipeline#dependencies)
