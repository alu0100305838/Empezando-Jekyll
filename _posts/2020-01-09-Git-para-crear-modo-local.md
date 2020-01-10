---
layout: post
title: Git para crear modo local
date: '2020-01-09 22:00:00 -0300'
tag: GitHub Jekyll-local Git
---

Siguiendo en nuestra idea de empezar a trabajar "localmente" en el desarrollo del sitio que ya tenemos funcionando en GitHub, es el momento de hacer una copia de dicho repositorio remoto en nuestra máquina: necesitaremos recurrir al uso de ***git***... 

## Sincronizando repositorios y el uso de *Git*

Una parte del trabajo con *git* está dedicada a los vínculos entre el **repositorio remoto** (en nuestro caso alojado en *GitHub*), y los distintos **repositorios locales** (en principio uno por cada máquina que esté colaborando en un proyecto).

Dado que no tenemos aún los archivos del proyecto en nuestra computadora, debemos **clonar** el repositorio remoto usando *git*. 

Pero primero...

### Configurar Git por primera vez

Es posible que no hayas usado *git* previamente, pero como el sistema está pensado para trabajos colaborativos es necesario la identificación de cada usuario con un nombre  y un email de contacto.

Es necesario hacer estas cosas **solamente una vez en tu computadora**, y se mantendrán entre actualizaciones. También puedes cambiarlas en cualquier momento volviendo a ejecutar los comandos correspondientes.

```
$ git config --global user.name "dsigno Mint"
$ git config --global user.email dsigno4all@gmail.com
$ git config --global color.ui auto
```

Esta última es la opción por defecto (que colorea la salida cuando va a un terminal).

### Clonando un repositorio existente

Si deseas obtener una copia de un repositorio *Git* existente —por ejemplo, un proyecto en el que te gustaría contribuir— el comando que  necesitas es `git clone`. 

Cada versión de cada archivo de la historia del proyecto es descargada  por defecto cuando ejecutas `git clone`. De hecho, si el disco de tu servidor se corrompe, puedes usar cualquiera de los clones en cualquiera de los clientes para devolver el servidor  al estado en el que estaba cuando fue clonado (puede que pierdas algunos hooks del lado del servidor y demás, pero toda la información acerca de las versiones estará ahí).

Para empezar, desde el terminal hay que posicionarse en el directorio donde almacenas/almacenarás tus repositorios (en mi caso es `~/Shared/GitHub_repos/`)

Puedes clonar un repositorio con `git clone [url]`. Por ejemplo, si quieres clonar la librería de *Git* llamada Empezando-Jekyll puedes hacer algo así:

```console
$ git clone https://github.com/dsigno/Empezando-Jekyll.git
```

Esto crea un directorio llamado `Empezando-Jekyll`, inicializa un directorio `.git` en su interior, descarga toda la información de ese repositorio y saca una copia de trabajo de la última versión. Si te metes en el directorio `Empezando-Jekyll`, verás que están los archivos del proyecto listos para ser utilizados. Si quieres clonar el repositorio a un directorio con otro nombre, puedes especificarlo con la siguiente opción de línea de comandos:

```console
$ git clone https://github.com/dsigno/Empezando-Jekyll.git mylibgit
```

Ese comando hace lo mismo que el anterior, pero el directorio de destino se llamará `mylibgit`.

Veamos el resultado del comando en acción:

```bash
theRaven@ravennest:~/Shared/GitHub_repos$ git clone https://github.com/dsigno/Empezando-Jekyll.git
Clonando en 'Empezando-Jekyll'...
remote: Enumerating objects: 1492, done.
remote: Total 1492 (delta 0), reused 0 (delta 0), pack-reused 1492
Recibiendo objetos: 100% (1492/1492), 8.44 MiB | 322.00 KiB/s, listo.
Resolviendo deltas: 100% (828/828), listo.
```

Esto es lo instalado...

![clone-results]({{site.baseurl}}/images/clone-results.PNG)

Lo primero que conviene es "mirar" con `git status`

```bash
theRaven@ravennest:~/Shared/GitHub_repos/Empezando-Jekyll$ git status
En la rama gh-pages
Tu rama está actualizada con 'origin/gh-pages'.

nada para hacer commit, el árbol de trabajo esta limpio
```

En este momento ambos repositorios están sincronizados...

### Probando generar "cambios" locales y *subirlos* al remoto

Aún sin instalar *Jekyll* en local ya podemos probar si las cosas van a funcionar. Recordemos que instalar *Jekyll* nos facilitará desarrollar nuestro sitio estático: generar cambios en el contenido y/o *layout* y poder ver como será el resultado antes de subirlo a *GitHub*.

Una manera de hacer esto es escribir un post en *markdown* tal como vinimos haciendo hasta ahora (*Front Matter* incluído), guardarlo en la carpeta `_post` en el repositorio local, y luego hacer el trabajo con *git* que culmine con un  `git push gh-pages`...

Voy a probar, en cambio, agregar una imagen (la que ves un poco más arriba en este post), copiándola a la carpeta `images`, y procesándola con *git*

+ Copio la imagen `clone-results.png` a la carpeta `images`

+ Realizo un nuevo `git status`  

    ```bash
$ git status
    En la rama gh-pages
    Tu rama está actualizada con 'origin/gh-pages'.
    
    Archivos sin seguimiento:
      (usa "git add <archivo>..." para incluirlo a lo que se será confirmado)
    
    	images/clone-results.PNG
    
    no hay nada agregado al commit pero hay archivos sin seguimiento presentes (usa "git add" para hacerles seguimiento)
    ```

+ Lo bueno de *git* es que nos dá muchos tips de ayuda... nos avisa que tendremos que agregar el nuevo archivo al grupo de los que llevan seguimiento (normalmente si no hay necesidad de seguimiento se los indica en `.gitignore`)

    ```bash
$ git add images/clone-results.PNG
    $ git status
    En la rama gh-pages
    Tu rama está actualizada con 'origin/gh-pages'.
    
    Cambios a ser confirmados:
      (usa "git reset HEAD <archivo>..." para sacar del área de stage)
    
    	nuevo archivo:  images/clone-results.PNG
    ```
	El `git add`  trabaja "silenciosamente" ; para ver los resultados repetimos `git status` y nos avisa que ya está en la *staging area* (área de preparación). Falta confirmar vía `git commit`  nuestra decisión.
	
+ Confirmamos entonces al archivo imagen... junto con cualquier otro cambio presente en la *staging area* (en nuestro caso no hay nada más).
	
	```bash
	$ git commit -m "agrego imagen sobre clonado"
	[gh-pages 6307318] agrego imagen sobre clonado
	 1 file changed, 0 insertions(+), 0 deletions(-)
	 create mode 100644 images/clone-results.PNG
	$ git status
	En la rama gh-pages
	Tu rama está adelantada a 'origin/gh-pages' por 1 commit.
	  (usa "git push" para publicar tus commits locales)
	
	nada para hacer commit, el árbol de trabajo esta limpio
	```
	
+ Hasta este momento, todo lo que ha hecho está en su sistema local pero invisible para su repositorio de GitHub hasta que empuje (*push*) esos cambios (diríamos *subir* los cambios).  

    ```bash
    $ git push origin gh-pages
    Username for 'https://github.com': dsigno
    Password for 'https://dsigno@github.com': 
    Contando objetos: 4, listo.
    Delta compression using up to 4 threads.
    Comprimiendo objetos: 100% (4/4), listo.
    Escribiendo objetos: 100% (4/4), 95.39 KiB | 10.60 MiB/s, listo.
    Total 4 (delta 2), reused 0 (delta 0)
    remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
    To https://github.com/dsigno/Empezando-Jekyll.git
       bbdd084..6307318  gh-pages -> gh-pages
    ```

    Su *commit* ahora está en el repositorio remoto (origen), en la rama *gh-pages*.

> Si visualizamos el repositorio en *GitHub* veremos reflejado este trabajo que realizamos primero en "local": no sólo aparece el archivo de la imagen sino también el mensaje del *commit* y el *commit ID*.

***



## PD: ¿Y si ahora agrego un *post* *con el viejo método*? (desde el sitio de *GitHub*)

De hecho, acabo se hacer esto con este mismísimo post —lo que antecede a esta post-data, *of course*—

Sabemos, pues, que hay un archivo en *GitHub* que no tenemos en nuestro repo local... la manera **rápida** de sincronizar este repositorio es a través del comando `git pull` para *bajar* y *fusionar* los cambios remotos.

> Usando `pull` en realidad estaremos haciendo un `fetch` y un `merge` (ya lo detallaremos más adelante)

Usaremos un método que separe por pasos la sincronización...

### Git fetch + Git merge

El comando `git fetch ` nos va a permitir recuperar todos los ficheros de un repositorio remoto que hayan sido modificados por otros colaboradores del proyecto y que actualmente no disponemos de ellos.

[`Git fetch`](https://git-scm.com/docs/git-fetch) tan sólo recupera la información del repositorio remoto y **la ubica en una rama oculta de tu repositorio local**, por lo que no la fusionará automáticamente con tu repositorio local. En este caso tenemos que saber que por cada repositorio remoto que tengamos configurado también tendremos una rama oculta de este.

```bash
$ git fetch
remote: Enumerating objects: 12, done.
remote: Counting objects: 100% (12/12), done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 12 (delta 6), reused 0 (delta 0), pack-reused 0
Desempaquetando objetos: 100% (12/12), listo.
Desde https://github.com/dsigno/Empezando-Jekyll
   6307318..524dc14  gh-pages   -> origin/gh-pages
$ git status
En la rama gh-pages
Tu rama está detrás de 'origin/gh-pages' por 3 commits, y puede ser avanzada rápido.
  (usa "git pull" para actualizar tu rama local)

nada para hacer commit, el árbol de trabajo esta limpio
```

Y como último paso vamos a fusionar esta rama  con la rama local en la que estamos trabajando actualmente y para ello necesitamos hacer uso del comando `git merge`.

```bash
$ git merge origin/gh-pages
Actualizando 6307318..524dc14
Fast-forward
 _posts/2020-01-09-Git-para-crear-modo-local.md | 164 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 164 insertions(+)
 create mode 100644 _posts/2020-01-09-Git-para-crear-modo-local.md
```



## Como actualizar y sincronizar este post (con PD), y traerlo a local con pull

Toda esta parte que estoy agregando al post original la voy a insertar usando *Prose* (directamente desde *GitHub*), por lo que tendré luego que ***sincronizar* nuevamente** mi sitio local: esta vez usaré el comando `pull`

### Git pull

El comando `git pull` es un atajo o una forma de abreviar todos los procesos que realizamos usando los comandos `git fetch` y `git merge` por lo que nos permite ahorrar tiempo.

> Cuando realizamos un `git pull` estamos sincronizando y trayéndonos todos los cambios del repositorio remoto a la rama en la que estemos trabajando actualmente, sin necesidad de ejecutar ningún comando extra.


NOTA: También nos puede ocasionar algún disgusto por lo que en proyectos grandes quizás sea mejor comprobar antes dichos cambios hacia una rama alternativa.



FUENTES

[2.1 Fundamentos de Git - Obteniendo un repositorio Git](https://git-scm.com/book/es/v2/Fundamentos-de-Git-Obteniendo-un-repositorio-Git)

[What is the difference between pull and clone in git?](https://stackoverflow.com/questions/3620633/what-is-the-difference-between-pull-and-clone-in-git)

[git fetch y git pull, Diferencias y formas de uso](https://blog.artegrafico.net/git-fetch-y-git-pull-diferencias-y-formas-de-uso)

[¿Por qué “git diff” no funciona sin antes hacer un “git fetch”?](https://es.stackoverflow.com/questions/196396/por-qué-git-diff-no-funciona-sin-antes-hacer-un-git-fetch)
