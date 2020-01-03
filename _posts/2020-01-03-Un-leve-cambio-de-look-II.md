---
layout: post
title: Un leve cambio de look (II)
date: 2020-01-03 12:00:00 -0300
tag: GitHub
---

Ya empezamos un leve cambio estético en nuestro blog basado en *Jekyll Now* ; nuestro próximo objetivo  —y más relevante— es personalizar las tipografías del sitio.

## Eligiendo *pairing fonts*

Siendo un tema que tiene más que ver con el diseño gráfico, conviene auxiliarse visitando sitios donde opine gente experta: hay varios y muy buenos...

Menciono un par de ellos, con ejemplos donde se incluyen las fuentes que voy a usar:

+ [Canva - font combinations : Bevas Neue](https://www.canva.com/font-combinations/bebas-neue/)
+ [typ.io - Bebas with Lato](http://typ.io/s/1zw5)
+ [Pair & Compare ](https://www.pairandcompare.net/)

En el sitio **typ.io** ofrecen además numéricos para el uso de *pairings fonts*; transcribo valores CSS sugeridos para *Bebas* y *Lato*, pero cambiaré a *Bebas Neue* para tener dos fuentes existentes en *GoogleFonts*.

```css
.header-1 {
    font-family: Bebas, sans-serif;
    font-size: 44px;
    font-style: normal;
    font-weight: 700;
    letter-spacing: normal;
    line-height: 48.4px;
    text-transform: uppercase;
    color: #000000;
    }
    .header-2 {
    font-family: Bebas, sans-serif;
    font-size: 24px;
    font-style: normal;
    font-weight: 700;
    letter-spacing: normal;
    line-height: 24px;
    text-transform: uppercase;
    color: #000000;
    }
    .content-1 {
    font-family: Bebas, sans-serif;
    font-size: 16px;
    font-style: normal;
    font-weight: 400;
    letter-spacing: normal;
    line-height: 21.6px;
    text-transform: none;
    color: #25B7FF;
    }
    .content-2 {
    font-family: Lato;
    font-size: 16px;
    font-style: normal;
    font-weight: 400;
    letter-spacing: normal;
    line-height: 21.6px;
    text-transform: none;
    color: #000000;
    }
    .content-3 {
    font-family: Lato;
    font-size: 13px;
    font-style: normal;
    font-weight: 400;
    letter-spacing: normal;
    line-height: 20.8px;
    text-transform: none;
    color: #000000;
    }
```

NOTA: Quizás use alguno de esos valores como referencia.

Por el momento, visito Google Fonts y genero la siguiente información de enlace a las fuentes, que deberá incluirse en `_layout/default.html`

<link href="https://fonts.googleapis.com/css?family=Bebas+Neue|Lato:400,400i&display=swap" rel="stylesheet">

> font-family: 'Bebas Neue', cursive;
> font-family: 'Lato', sans-serif;

## *Conjeturando* SASS

Jekyll usa SASS para procesar los estilos CSS del maquetado del sitio, *tema que no domino aún*. Pero SASS (como LESS y otros) se crearon para simplificar el mantenimiento de los sitios, usando entre otras cosas **variables** que se definen al principio de los archivos **.scss**; la template *Jekyll Now* usa el siguiente archivo `_sass/_variables.scss`

```scss

//
// VARIABLES
//

// Colors
$blue: #4183C4;

// Grays
$black: #000;
$darkerGray: #222;
$darkGray: #333;
$gray: #666;
$lightGray: #eee;
$white: #fff;

// Font stacks
$helvetica: Helvetica, Arial, sans-serif;
$helveticaNeue: "Helvetica Neue", Helvetica, Arial, sans-serif;
$georgia: Georgia, serif;

// Mobile breakpoints
@mixin mobile {
  @media screen and (max-width: 640px) {
    @content;
  }
}
```

En la sección `Font stacks`  vemos donde deberemos redefinir no sólo las fuentes (pensadas para el entorno Mac OS yo creo), sino cambiar sus nombres... cito lo encontrado en esta página de la que tomo buena parte de mi información =>  [dirk lüsebrink - Google Web Fonts for my Jekyll Sass](http://sebrink.de/Google-Webfonts-for-my-Jekyll/)

> I don’t understand the original logic in the `style.css` file of [Jekyll Now](http://github.com/barryclark/jekyll-now). For example, the `h1` headlines font is set to `font-family: $helveticaNeue;`. You have a sass variable for the font family, but it is named after the content of the variable. So when now changing the font-family you would either need to put another font value into the `$helveticaNeue` variable, which is confusing, or you need to change all references to `$helveticaNeue` var to references to a new font variable. Also not a smart move.

#### redefino variables en _variables.scss

```scss
// google web fonts
$Headers: 'Bebas Neue', sans-serif; 
$SansSerif: 'Lato', sans-serif;
// fuentes de sistema
$Monospace: Monaco, Consolas, "Lucida Console", monospace; 
```
#### cambio definición de code en _highlights.scss

```
code {
  font-family: $Monospace;
  font-size: 0.8em;
}
```



## Ahora el turno de style.scss

El archivo donde habrá que tener más cuidado es style.scss, ya que tienen que mostrarse las nuevas variables creadas en reemplazo de `$helveticaNeue`  (para títulos principalmente) y  ​`$helvetica`.

Por ahora no pienso ir mucho más allá con el rediseño... no es el ámbito ideal de pruebas el trabajar modificando archivos en Github y esperar la "reelaboración" del sitio: para este tipo de trabajo es conveniente hacer cambios en un "entorno local", y luego subirlos a Github.

Para ser concreto: además de dar los nuevos nombres a las variables de familia de fuente (`$Headers` y `$SansSerif`), voy a "tocar" algún que otro estilo, nada de mucha consideración:

```
h1, h2, h3, h4, h5, h6 {
  ...
  font-weight: normal;
  ...
}
  
.site-name {
  ...
  font-size: 44px;
  ...
}

.post {
  blockquote {
    ...    
	  em { 
	    font-style: normal;
	  }
  }
```

Hasta aquí, los cambios visuales que vamos a aplicar directamente desde Github...

***

FUENTE:

+ dirk lüsebrink - Google Web Fonts for my Jekyll Sass



