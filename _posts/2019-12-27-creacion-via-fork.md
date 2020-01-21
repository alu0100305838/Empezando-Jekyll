---
layout: post
title: Creación vía fork | Paso a paso
date: 2019-12-27 11:00:00 -0300
category: github-pages
tags: [github]
---


Comenzar con una bifurcación (*fork*) es excelente porque te dará una idea de cómo es Jekyll antes de que tengas que configurar un entorno de desarrollo local, instalar dependencias y descubrir el proceso de compilación de Jekyll.

La bifurcación –*fork*– significa copiar o clonar un repositorio, lo que permite experimentar libremente con los cambios sin afectar el proyecto original.

Por lo general, la gente bifurca repos para proponer cambios al proyecto original o *usar ese proyecto como punto de partida*. Este último caso es el que nos ocupará de aquí en adelante...

## Paso 1

+ Suponiendo que ya contamos con una cuenta en GitHub, lo primero es iniciar sesión en dicha cuenta y hacer un *fork* de todo el repositorio del tema elegido –[Jekyll Now](https://github.com/barryclark/jekyll-now)–
+ El nombre predeterminado de nuestro nuevo repositorio conviene cambiarlo (yo le puse **Empezando-Jekyll**), y habrá en este punto no confundirse y crear –como en muchos tutoriales se muestra– un nombre asociado a un *User Website*: de estos se permiten **solo uno** por cuenta GitHub.
+ Puede ser un buen momento para cambiar la descripción y URL de website (informativo) que está incluído en la *template* original

En *_config.yml* cambié algunas cosas:

{% highlight yaml %}
# Name of your site (displayed in the header)
name: Empezando con Jekyll

# Short bio or description (displayed in the header)
description: mis primeras notas y docs...
{% endhighlight %}

Si ahora visitamos la dirección de la subcarpeta donde se verá nuestro sitio, notaremos  que *no se vé* tal como lo esperábamos... las templates, como dijimos, vienen pensadas para trabajar pensando en un *User Website*.

Traduzco lo siguiente:

>**Problema común #3**: El uso de un *project website* para su sitio web de Jekyll [presenta cierta complejidad](http://jekyllrb.com/docs/github-pages/#project_page_url_structure) porque su sitio web se publicará en un subdirectorio. La URL se verá como `http://yourname.github.io/repository-name`, lo que causará problemas en las plantillas de Jekyll, como **romper referencias** a imágenes y no permitirle obtener una vista previa del sitio web localmente.

## Paso 2

+ Aunque desde hace un tiempo GitHub cambió el criterio de publicar el sitio desde una nueva rama –*branch*– a crear llamada la **rama *gh-pages***, y se puede hacer directamente desde la rama *master* (ver [aquí](https://blog.webjeda.com/create-jekyll-blog/#step-3-check-whether-you-are-on-the-right-branchnot-required) ), **voy a crearla igualmente** para evitarme confusiones con anteriores lecturas...
+ Luego de creada, hice a *gh-pages* la **rama por defecto**.
+ Existe otra rama de la cual no tengo aún mayores datos.

## Paso 3

En este paso vamos a trabajar sobre el archivo más determinante en el comportamiento de nuestro sitio Jekyll: **_config.yml**.

+ Hay cuestiones básicas de configuración sobre el sitio y su autor (o autores, vía *collections*) . Aquí se detallan el nombre del sitio, descripción, etc. Ya lo vimos antes.
+ Para solucionar los problemas de visualización que aparecieron por usar la opción *project website* , la opción **baseurl** [Base URL : Serve the website from the given base URL] debe cambiarse a una que incluya el subdirectorio.

{% highlight yaml %}
# If you're hosting your site at a Project repository on GitHub pages
# (http://yourusername.github.io/repository-name)
# and NOT your User repository (http://yourusername.github.io)
# then add in the baseurl here, like this: "/repository-name"
baseurl: "/Empezando-Jekyll"
{% endhighlight %}

> A diferencia de lo que sucedió con la plantilla *Emerald* (usada en un intento anterior, sin final feliz ), la definición de **baseurl** sin  *trailing slash* no generó problemas.

## Paso 4

Publicar el primer post, *like a Hello World!*

En mi caso consiste de un texto indicando que es el primer post, con muy poco más que destacar... sólo un post de prueba...

Sólo faltó crear un nuevo archivo markdown dentro de la carpeta **_posts**, con el formato de nombre de archivo `year-month-day-title.md` (yo reescribí el que traía el *theme*).

NOTA: Dejaremos para más adelante detalles sobre el **Front Matter** indispensable al comienzo del archivo markdown, sólo colocaremos el título del primer post.



***
FUENTES:

+ [Build a blog with Jekyll and Github Pages](http://andrewbtran.github.io/JRN-418/class13/jekyll/)
+ [Smashing Magazine - Build A Blog With Jekyll And GitHub Pages](https://www.smashingmagazine.com/2014/08/build-blog-jekyll-github-pages/)
+ [Jekyll Issues aren't as Bad as You Think](https://blog.webjeda.com/jekyll-issues/)
