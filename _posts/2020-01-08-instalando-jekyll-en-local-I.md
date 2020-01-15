---
layout: post
title: Instalando Jekyll en local (I)
date: 2020-01-08 23:00:00 -0300
tag: Jekyll-local
---

El hecho de trabajar "en local" nos significará mucha comodidad y aumento de velocidad al momento de desarrollar nuestro sitio *Jekyll*, pero como contrapartida, deberemos lidiar con la instalación de determinados componentes de software: de esta cuestión hablaremos en estos posts.

Como es un detalle importante vinculado a la descripción paso a paso que voy a encarar, aclaro que voy a trabajar en un "entorno local" consistente en una máquina virtual de *Virtual Box* corriendo **Linux Mint**, aunque existe mucha información sobre implementaciones de *Jekyll* sobre otros Sistemas Operativos.

## ¿Qué se necesita para correr *Jekyll* localmente?

Aunque ya lo hemos mencionado, aquí lo repetimos: *Jekyll* está escrito en **Ruby**, que es un lenguaje de programación. *Jekyll* es el código (programa) que permite crear nuestro website en base a *templates*, *includes*, documentos markdown, estilos SASS, imágenes y demás.

Este código (como otros que que deberemos cargar en nuestra máquina) recibe el nombre de ***Gem*** (gema). **Será necesario instalar software que *interprete* esas gemas escritas en *ruby*** para que puedan ejecutar su acción en nuestra computadora; la manera rápida de decirlo es: debemos [instalar *Ruby*](https://www.ruby-lang.org/en/documentation/installation/) (en la versión correspondiente a nuestro sistema operativo).

> [Ruby](http://www.ruby-lang.org/) es un lenguaje de programación orientado a objetos, interpretado y reflexivo. Fue creado por un japonés llamado Yukihiro Matsumoto en el año 1993 pero lo sacó al público en el '95.  
>
> Se dice que un lenguaje de programación es interpretado cuando su ejecución depende de un intérprete, en contraste con los lenguajes compilados. También se hacen llamar lenguajes de “script”.

Debe señalarse que las *Gems* son administradas a través de un gestor de paquetes específico (independiente de los gestores de paquetes del SO) llamado ***RubyGems***, que también debe ser instalado.

### Requisitos previos para correr Jekyll

Antes de instalar *Jekyll* propiamente dicho deberemos contar entonces con:

[Ruby](https://www.ruby-lang.org/en/downloads/) versión  2.4.0  o superior, incluidos todos los encabezados de desarrollo (la  instalación de ruby se puede comprobar ejecutando `ruby -v` , los encabezados de desarrollo pueden verificarse en Ubuntu ejecutando `apt list --installed ruby-dev` )

[RubyGems](https://rubygems.org/pages/download) (que puedes verificar ejecutando `gem -v` )

[GCC](https://gcc.gnu.org/install/) y [Make](https://www.gnu.org/software/make/) (en caso de que su sistema no los tenga instalados, que puede verificar ejecutando `gcc -v` , `g++ -v` y `make -v` en la interfaz de línea de comandos de su sistema)

En mi sistema:

{% highlight shell %}
$ ruby -v
ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux-gnu]
$ apt list --installed ruby-dev
Listando... Hecho
ruby-dev/bionic,now 1:2.5.1 amd64 [instalado]
$ gem -v
2.7.6
$ gcc -v
(...)
gcc version 7.4.0 (Ubuntu 7.4.0-1ubuntu1~18.04.1)
$ g++ -v
(...)
gcc version 7.4.0 (Ubuntu 7.4.0-1ubuntu1~18.04.1)
$ make -v
GNU Make 4.1
(...)
{% endhighlight %}

## Usando *Bundler* para instalar *gems*

El modo tradicional de instalar *gems* es con el comando `gem`, disponible al ser instalado *RubyGems*. Sin embargo, todas las documentaciones recomiendan usar [**Bundler**](http://bundler.io/) para instalar y ejecutar *Jekyll*. *Bundler* administra las dependencias de gems de Ruby, reduce los errores de construcción de Jekyll y evita los errores relacionados con el entorno.

### Un poco de teoría sobre Bundler

Transcribo lo que encontré en [Lo que he aprendido: temas de Jekyll y GitHub Pages](https://ondahostil.wordpress.com/2017/02/24/lo-que-he-aprendido-temas-de-jekyll-y-github-pages/)... es una explicación corta del uso de *Bundler*.

***

Está muy bien saber los pasos para conseguir nuestro objetivo, pero está mejor saber por qué hacemos las cosas, creo yo. Para ello necesitamos saber qué es eso de `bundle cosas` y el archivo `Gemfile`.

Veamos, los comandos `bundle cosas` pertenecen al programa [Bundler](http://bundler.io/) que es un programa que sirve para instalar y gestionar las gemas que necesita un proyecto. Podemos entender las gemas como si fueran paquetes, *Bundler* mira a ver si los *paquetes* necesarios están instalados en las versiones necesarias para que nuestro proyecto funcione.

Para que *Bundler* haga su trabajo, nosotros le decimos en un archivo llamado `Gemfile` qué gemas vamos a usar y en qué versión. Por ejemplo, un `Gemfile` para usar el tema Minima en Jekyll tiene esta pinta:

{% highlight ruby %}
source "https://rubygems.org"
gem "jekyll", "3.3.1"
gem "minima", "~> 2.0"
{% endhighlight %}

Esto le dice a Bundler dónde buscar las gemas y que use Jekyll 3.3.1 y Minima en una versión [mayor que la 2.0 y menor que la 3.0](https://robots.thoughtbot.com/rubys-pessimistic-operator).

Cuando hacemos `bundle install`, Bundle mira el **Gemfile** e instala lo que debe. Además, cuando usamos *Bundle*, se crea un archivo `Gemfile.lock` en el que se guarda la información de las gemas que hemos utilizado, no solo las que nosotros hemos puesto en el `Gemfile`, sino *todas de las que depende nuestro proyecto* con sus versiones correspondientes. Este archivo `Gemfile.lock` puede ser muy largo.

***

### Instalar Bundler

Si ya fue instalado Ruby y RubyGems, simplemente hay que usar, como con cualquier gema, el comando `gem install`

{% highlight shell %}
# Instala Bundler gem
$ gem install bundler
$ bundle version
Bundler version 2.1.3 (2020-01-02 commit 14c5584664)
{% endhighlight %}

De la documentación oficial de Jekyll, extractamos lo siguiente:

> `gem install bundler` installs [Bundler](https://rubygems.org/gems/bundler). You only need to install it once — not every time you create a new Jekyll project. Here are some additional details:
>
> If you’re using a `Gemfile` you would first run `bundle install` to install the gems, then `bundle exec jekyll serve` to build your site. This guarantees you’re using the gem versions set in the `Gemfile`. If you’re not using a `Gemfile` you can just run `jekyll serve`.

### Gemfile

Es necesario para el uso de *Bundler* un archivo **Gemfile** en el directorio raíz de su repositorio local: en el se indicará la lista de gems (y sus versiones) que se usarán al hacer `bundle install`. Un ejemplo de Gemfile puede ser:

{% highlight ruby %}
source "https://rubygems.org"

gem "jekyll"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
end
{% endhighlight %}

***

## Una pausa...

Antes de seguir con la instalación de *Jekyll* en sí (para lo cual se necesita lo descripto en esta página), en el próximo post contaremos como **clonar** nuestro repositorio existente en GitHub... necesitaremos hablar entonces un poco sobre ***git***.



***



FUENTES

[Jekyllrb - installation](https://jekyllrb.com/docs/installation/)

[Jekyll 3.7 - Installation](https://code-examples.net/es/docs/jekyll/installation/index)

[Wikipedia - RubyGems](https://es.wikipedia.org/wiki/RubyGems)

[Guía Hispana de Ruby](http://www.maestrosdelweb.com/ruby/ )

[Lo que he aprendido: temas de Jekyll y GitHub Pages](https://ondahostil.wordpress.com/2017/02/24/lo-que-he-aprendido-temas-de-jekyll-y-github-pages/)
