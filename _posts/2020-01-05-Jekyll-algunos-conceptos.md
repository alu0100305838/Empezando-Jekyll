---
layout: post
title: Jekyll (algunos conceptos)
date: '2020-01-05 22:00:00 -0300'
category: jekyll
tags: [github, jekyll-local, git]
published: true
---

Hasta ahora hemos estado trabajando directamente desde GitHub (esto es, *online*), todo esto facilitado por el hecho de haber partido con un *fork* de una plantilla llamada *Jekyll Now*. En todo caso nuestro sitio siempre corre bajo el entorno llamado *GitHub Pages* (GitHub "sirve" nuestro sitio web, funciona como "servidor web").

Para seguir avanzando y llegar a personalizar y potenciar nuestra plantilla, o incluso llegar a crear una desde cero, necesitaremos contar con un "entorno de desarrollo", y con nuevas herramientas tanto de software como conceptuales...

Por lo tanto en lo que sigue necesitaremos empezar a ver:

+ como trabajar con Jekyll desde un **entorno *local*** (nuestra PC)... esto incluye tener instalada en la misma algo de software extra.
+ como "enlazar" tanto los sitios **local** como **remoto** (el repositorio en GitHub será el remoto), de manera de experimentar con el desarrollo del sitio en un entorno aislado para luego subir los cambios a la web... esto implica manejar conceptos de **Git**.

Pero es momento de conceptualizar un poco (*muy poco*) para echar luz sobre la materia de nuestro trabajo.

## ¿Qué es Jekyll?

Jekyll es un **generador de sitios estáticos** muy poderoso. En algunos sentidos, es una vuelta  a los días del HTML estático antes de que las bases de datos se usaran para almacenar el contenido del sitio web. Para sitios simples sin arquitecturas complejas, como un sitio web personal, esta es una gran ventaja.  

Cuando se usa junto con GitHub, Jekyll volverá a generar automáticamente todas las páginas HTML de su sitio web **cada vez** que envíe un archivo.

> Cuando trabaja como parte de GitHub Pages, Jekyll es *consciente de sí mismo*, por lo que si agrega carpetas y archivos siguiendo convenciones de nomenclatura específicas, cuando se haga un **commit** con GitHub, Jekyll **reconstruirá** mágicamente su sitio web.

Jekyll facilita la administración de su sitio web porque depende de las plantillas. Las plantillas (o *layouts* en la nomenclatura de Jekyll) son su mejor amigo cuando usa un generador de sitio estático. En lugar de repetir el mismo marcado de navegación en cada página que creo, que tendría que editar en cada página si agrego, elimino o cambio la ubicación del elemento de navegación, puedo crear lo que Jekyll llama un *layout* que se utiliza en todos las páginas.

### Tres formas de trabajar con Jekyll:

+ Trabajando directamente en *GitHub Pages* hacemos que en GitHub se "procese" nuestro conjunto de plantillas, documentos markdown, estilos SASS o CSS, y allí se "cree" nuestro sitio web. Sólo pueden usarse un número limitado de *plugins*. No necesito instalar nada especial.
+ Una "mejora" a la opción anterior es probar **en modo local** el citado conjunto de "archivos Jekyll", siendo nuestra máquina la que genera una muestra del sitio web estático, para luego subir solo los archivos necesarios a GitHub. Sólo pueden usarse un número limitado de *plugins*.
+ Otra opción **en modo local** es crear el sitio, y **subir ese sitio** a algún proveedor, incluyendo a las mismísimas GitHub Pages, *pero esta vez sin que tenga que procesar y crear las páginas del sitio*. Pueden usarse *plugins* sin limitación.

### Anticipemos los requisitos:

La segunda y tercera opción necesitarán de la instalación de algo de software...

+ Para que *Jekyll* tome los distintos archivos y pueda generar el sitio estático, deberemos tener *Ruby* instalado en nuestra computadora (*Jekyll* fue escrito en lenguaje de programación *Ruby*, es lo que se dice una *Ruby Gem*). Dependiendo del SO podrá necesitarse algún software adicional.
+ Para poder vincular el entorno **remoto** con el **local**, es necesario trabajar con un sistema de control de versiones llamado **Git** (tema del que hablaremos en el próximo post). El uso de **Git** necesitará de la instalación del correspondiente software para su manejo, que generalmente se hace directamente desde una terminal de texto (*bien Unix la cosa*).

***

En lo personal voy a trabajar en un "entorno local" que implimentaré en una máquina virtual corriendo Linux Mint, aunque existen instaladores tanto de Git como de Ruby para los Sistemas operativos Windows y MacOs.

***



FUENTES:

+ [Creating and Hosting a Personal Site on GitHub](http://jmcglone.com/guides/github-pages/)

+ [Jekyll Plugins on GitHub](https://www.sitepoint.com/jekyll-plugins-github/)
