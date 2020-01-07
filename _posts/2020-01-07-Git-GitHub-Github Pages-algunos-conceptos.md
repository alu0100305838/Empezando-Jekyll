---
layout: post
title: Git, GitHub y Github Pages (algunos conceptos)
date: 2020-01-07 02:00:00 -0300
tag: GitHub Git
---

En el post anterior anticipamos lo necesario que es dar una breve conceptualización sobre algunos temas (poco más que una presentación) para poder avanzar en nuestro trabajo. 

Ya hablamos sobre *Jekyll* y sus opciones al momento de generar un sitio web estático; ahora le toca el turno a Git, GitHub y Github Pages y su importante papel en el *ecosistema Jekyll*.

***

NOTA: voy a transcribir/resumir parte del contenido del interesante  [artículo de Jonathan McGlone - Creating and Hosting a Personal Site on GitHub](http://jmcglone.com/guides/github-pages/)

## ¿Qué son Git, GitHub y GitHub Pages?

Tanto *Git*, *GitHub* y *GitHub Pages* están muy relacionados. Imagine a **Git** como el flujo de trabajo para hacer las cosas y **GitHub** y **GitHub Pages** como lugares para almacenar el trabajo que termina. Los proyectos que usan Git *se almacenan públicamente* en las páginas de GitHub y GitHub, por lo que de manera muy generalizada, Git es lo que hace principalmente en modo local (en su propia computadora) y GitHub es el lugar donde todo esto se almacena públicamente en un servidor (la parte remota).       

### Git

[*Git*](http://git-scm.com/) es un sistema de control de versiones que rastrea los cambios en los archivos de un proyecto a lo largo del tiempo. Por lo general, registra cuáles fueron los cambios (¿qué se agregó? ¿qué se eliminó del archivo?), quién realizó los cambios, notas y comentarios sobre los cambios realizados por el usuario, y en qué momento se realizaron los cambios. Se utiliza principalmente para proyectos de desarrollo de software que a menudo son colaborativos, por lo que, en este sentido, es una herramienta para ayudar a habilitar y mejorar la colaboración. Sin embargo, su naturaleza colaborativa lo ha llevado a ganar interés en la comunidad editorial como una herramienta para ayudar en los flujos de trabajo de autoría y editorial.

*Git* es para personas que desean mantener múltiples versiones de sus archivos de manera eficiente y *viajar en el tiempo* para visitar diferentes versiones sin hacer malabarismos con numerosos archivos junto con sus nombres confusos almacenados en diferentes ubicaciones. Piense en *Git* y el control de versiones como "un botón mágico de deshacer" (si hiciese falta).

En el diagrama a continuación, cada etapa representa un nuevo "**guardar**". Sin *Git*, no puede volver a ninguna de las etapas intermedias entre el borrador inicial y el borrador final. Si deseáramos cambiar el párrafo inicial en el borrador final, tendríamos que borrar datos que no podríamos recuperar. Para evitar esto, normalmente utilizamos la opción "**guardar como**", le asignamos al trabajo un nombre diferente, y ahí sí eliminamos el párrafo inicial y comenzamos a escribir uno nuevo.

![git basics]({{site.baseurl}}/images/git-basics.png)

Con *Git*, **el flujo es multidireccional**. Cada cambio que es significativo se marca como importante en una versión, y continúa. Si necesita volver a las etapas anteriores, puede hacerlo sin ninguna pérdida de datos. Actualmente, el "historial de revisiones" de *Google Docs* o el "historial de edición" de Wikipedia funcionan de esta manera. *Git* es mucho más detallado y puede ser mucho más complejo si es necesario. 

### GitHub

*GitHub* es un servicio de alojamiento web para el código fuente de software y proyectos de desarrollo web (u otros proyectos basados en texto) que usan *Git*. En muchos casos, la mayor parte del código está disponible públicamente, lo que permite a los desarrolladores investigar, colaborar, descargar, usar, mejorar y remezclar fácilmente ese código. El contenedor para el código de un proyecto específico se llama **repositorio**.

> Es importante notar que los proyectos almacenados en *GitHub* no tienen por qué estar vinculados a sitios web, mucho menos asociados a sitios web estáticos servidos a través de *Pages*: estas son prestaciones particulares del servicio GitHub... su función principal es el permitir contar con repositorios de trabajo en común entre varios desarrolladores, que elijen usar *git* como sistema de control de versiones.

NOTA: existen otros servicios de alojamiento de repositorios de projectos, entre los que se cuentan BitBucket y GitLab (los más conocidos).

### GitHub Pages

Las *GitHub Pages* son páginas web públicas alojadas de forma gratuita a través de *GitHub*. Los usuarios de *GitHub* pueden crear y alojar sitios web personales (**solo uno** permitido por usuario, llamados *user website*) y sitios web relacionados con proyectos específicos de *GitHub* (**varios** por cuenta, trabajando en subdirectorios, denominados *projects websites*). Pages le permite hacer lo mismo que *GitHub*, pero si el repositorio se nombra de cierta manera y los archivos que contiene son HTML o Markdown, puede ver el archivo como cualquier otro sitio web. *GitHub Pages* es la versión *auto-consciente* de *GitHub*. *Pages* también viene con un potente [generador de sitio estático](http://staticgen.com/) llamado [Jekyll](http://jekyllrb.com/), tal cual explicamos en el post anterior.

NOTA: Existen servicios similares (aunque menos conocidos) como *Netlify*, y la versión *Pages* de *GitLab*.

***

## Sólo un pantallazo...

Tanto este post como el anterior dedicado a *Jekyll*  tienen la finalidad de presentar las herramientas que estamos empleando y/o vamos a emplear para la construcción y transformación de este mismo blog. 

Especialmente lo vinculado al uso de ***git*** requiere desde lo conceptual a lo procedimental al menos recopilar algunas fuentes de aprendizaje de las muchas que hay en la web.

Cito una de las tres opciones de trabajo mencionada en el post previo...

> + Una “mejora” a la opción anterior es probar **en modo local** el citado conjunto de “archivos Jekyll”, siendo nuestra máquina la que genera una muestra del sitio web estático, para luego subir solo los archivos necesarios a GitHub. Sólo pueden usarse un número limitado de *plugins*.

Esta es la metodología de trabajo que vamos a seguir de aquí en más. A medida que vayamos implementando el *entorno de trabajo local* iremos describiendo los pormenores de esta empresa... hacia allí vamos.

FUENTES:

[Creating and Hosting a Personal Site on GitHub](http://jmcglone.com/guides/github-pages/)
