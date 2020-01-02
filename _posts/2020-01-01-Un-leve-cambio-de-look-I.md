---
layout: post
title: Un leve cambio de look (I)
date: 2020-01-01 22:00:00 -0300
tag: GitHub
---

Nuestro sitio basado en *Jekyll Now* está funcionando por ahora de manera más que aceptable, así que es un buen momento de intentar un cambio visual: vamos modificar el logo del sitio, a agregar un favicon.ico  y a  personalizar las tipografías del sitio.

Empecemos por las imágenes...

## Cambiar el logo (avatar) del sitio

Desde ya que hay un proceso previo que es el de crear algún logo más o menos creativo para nuestro sitio (¿?) y habrá que subirlo, por ejemplo,  a nuestra carpeta *images*.

![logo](https://raw.githubusercontent.com/dsigno/Empezando-Jekyll/gh-pages/images/use11320.png)

Si examinamos el archivo *default.html* veremos que al principio del <body>  aparece esta línea:

```
<a href="{{ site.baseurl }}/" class="site-avatar"><img src="{{ site.avatar }}" /></a>
```

El theme Jekyll Now viene preparado para mostrar el llamado *avatar* al tope de nuestras páginas, y la expresión `{{ site.avatar }}` nos está indicando que está definido en realidad en algún archivo de configuración (en *_config.yml* más precisamente).

El valor original de la template era:

```
avatar: https://raw.githubusercontent.com/barryclark/jekyll-now/master/images/jekyll-logo.png
```

Y lo cambiaremos a la URL de nuestro nuevo logo:

```
avatar: https://raw.githubusercontent.com/dsigno/Empezando-Jekyll/gh-pages/images/use11320.png
```
Un detalle a considerar es que las URL de las imágenes se *sirven* desde raw.githubusercontent.com; cito:

> The `raw.githubusercontent.com` domain is used to serve  unprocessed versions of files stored in GitHub repositories. If you  browse to a file on GitHub and then click the **Raw** link, that's where you'll go.

## Agregar un favicon

1. Usando uno de los varios servicios online para generar archivos de íconos (usé [OnlineFavicon.com](https://onlinefavicon.com/) en mi caso), cargué la mencionada imagen creada para el logo, y generé el archivo **favicon.ico**.

2. Subí el archivo  .ico a la carpeta images de mi repositorio

3. Agregué esta línea al final del <head> del archivo *default.html*

   ```
     <!-- Update these with your own icon images -->
   	<link rel="shortcut icon" href="images/favicon.ico">
   </head>
   ```

***

En la segunda parte del post nos dedicaremos a la tipografía...

***

PD: la línea propuesta para el favicon sólo funciona para la página *home*... debe ser:

```
<link rel="shortcut icon" href="{{ site.baseurl }}/images/favicon.ico">
```

