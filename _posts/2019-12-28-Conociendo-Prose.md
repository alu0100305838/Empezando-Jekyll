---
layout: post
title: Conociendo Prose
date: '2019-12-28 06:00:00 -0300'
tag: GitHub
---
Hay 3 formas diferentes de realizar cambios en los archivos de tu blog:

+ Editando archivos dentro de su nuevo repositorio en *username.github.io*, mediante el navegador, en GitHub.com (muy práctico si se conoce markdown, y el post es simple).
+ Usando un editor de contenido de GitHub de terceros, como [Prose by Development Seed](http://prose.io/). Está optimizado para usar con Jekyll, lo que hace que la edición de markdown, la redacción de borradores y la carga de imágenes sean realmente fáciles.
+ Clonando su repositorio, y realizando actualizaciones en modo local, para luego subirlos (*push*) a su repositorio GitHub. Se necesita instalar algo de nuevo software en su computadora.

Voy a probar esta vez con la segunda opción, es decir, trabajar con **Prose**, para editar algunas cosas que ya traía la plantilla _Jekyll Now_.

## ¿Cómo usar Prose?

Por ser una herramienta "de terceros", deberemos darle permisos para acceder a nuestros datos en GitHub, tanto en lectura como en edición. 

![Prose01]({{site.baseurl}}/images/Prose.PNG)

Luego de dar la aprobación de acceso a la herramienta, podemos acceder a nuestros archivos de manera similar a lo que hacemos desde nuestra cuenta en Github, pero con mayores facilidades de edición, como ya veremos.

Vamos a usar Prose para editar la **404 - Page not found**, que viene escrita en inglés...

## Edición markdown en Prose

Al seleccionar la edición de *404.md*, nos aparece la siguiente ventana con las opciones básicas de edición markdown:

![edicion Prose]({{site.baseurl}}/images/edicion Prose.PNG)

El trabajo por hacer es bastante intuitivo: la única posibilidad de confusión puede darse en la presencia de variables YAML como `{{ site.baseurl }}`,  variables sobre la que hablaremos más adelante (espero...).

Cuando estemos conformes con los cambios, y por trabajar con la filosofía de Git, no se confirmarán directamente sino que nos mostrará una ventana mostrando las diferencias entre contenido previo y nuevo, y la opción de acompañar al *commit* con un mensaje personalizado:

![commit Prose]({{site.baseurl}}/images/commit Prose.PNG)

## Post con imágenes

Si en nuestros posts necesitamos insertar imágenes, `Prose` lo facilita mediante la opción de drag & drop desde la carpeta de nuestra máquina al cuadro de diálogo de inserción, donde además podemos incluir información ALT de texto.

![insert image]({{site.baseurl}}/images/insert-image-Prose.PNG)


> Es recomendable editar la carpeta donde se alojará la imagen para que las mismas no aparezcan mezcladas con los documentos markdown; yo escribí la carpeta **images**, que ya traía la template Jekyll Now

## Para terminar

**Prose** parece una muy buena opción para quienes no conozcan aún los entresijos de markdown, y facilita el workflow  involucrado con la escritura de posts.


