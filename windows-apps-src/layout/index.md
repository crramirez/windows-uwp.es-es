---
description: "Aprende a diseñar y codificar una aplicación para UWP en la que resulte fácil navegar y cuyo aspecto sea perfecto en varios dispositivos y tamaños de pantalla."
title: "Diseño de la distribución de una aplicación para UWP: desarrollo de aplicaciones de Windows"
author: mijacobs
keywords: "diseño de aplicaciones para UWP, Plataforma universal de Windows, diseño de aplicaciones, interfaz"
label: Layout
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 1aa12606-8a99-4db3-8311-90e02fde9cf1
ms.openlocfilehash: 1034588565032301cb0746d79a122e8388dad8f9
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="layout-for-uwp-apps"></a>Diseño de aplicaciones para UWP
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


La estructura de la aplicación, el diseño de la página y la navegación son la base de la experiencia del usuario de tu aplicación. Los artículos de esta sección te ayudarán a crear una aplicación en la que resulte fácil navegar y que quede bien en varios dispositivos y tamaños de pantalla.

## <a name="intro"></a>Introducción

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p><b>[Introducción al diseño de interfaces de usuario para aplicaciones](design-and-ui-intro.md)</b><br />
Al diseñar una aplicación de UWP, se crea una interfaz de usuario que se adapta a una gran variedad de dispositivos con diferentes tamaños de pantalla. En este artículo se ofrece una descripción general de las características y las ventajas relacionadas con la interfaz de usuario de las aplicaciones para UWP, así como sugerencias y trucos para diseñar una interfaz de usuario dinámica. </p>
  </div>
  <div class="side-by-side-content-right">
    ![Una aplicación que se ejecuta en varios dispositivos](images/rspd-reposition-type1-sm.png)
  </div>
</div>
</div>

## <a name="app-layout-and-structure"></a>Estructura y distribución de la aplicación
Echa un vistazo a estas recomendaciones para estructurar la aplicación y usar los tres tipos de elementos de interfaz de usuario: navegación, comandos y contenido.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p>
<b>[Conceptos básicos de navegación](navigation-basics.md)</b><br/>
La navegación en las aplicaciones para UWP se basa en un modelo flexible de estructuras de navegación, elementos de navegación y características de nivel del sistema. Este artículo es una introducción a estos componentes y te muestra cómo usarlos juntos para crear una buena experiencia de navegación.
</p>
<p>
<b>[Conceptos básicos de contenido](content-basics.md)</b><br/>
El propósito principal de cualquier aplicación es proporcionar acceso a contenido: en una aplicación de edición de fotos, la fotografía es el contenido; en una aplicación de viajes, los mapas y la información acerca de los destinos de viaje son el contenido; y así sucesivamente. Este artículo proporciona recomendaciones de diseño de contenido para los tres escenarios de contenido: consumo, creación e interacción.
</p> 
  </div>
  <div class="side-by-side-content-right">
<p><b>[Conceptos básicos de comandos](commanding-basics.md)</b> <br />
Los elementos de comandos son los elementos interactivos de la interfaz de usuario que permiten al usuario realizar acciones como enviar un correo electrónico, eliminar un elemento o enviar un formulario. En este artículo se describen los elementos de comandos (como los botones y las casillas), las interacciones que admiten y las superficies de comandos (como las barras de comandos y los menús contextuales) para hospedarlos.</p>
  </div>
</div>
</div>

## <a name="page-layout"></a>Diseño de página 
Estos artículos te ayudan a crear una interfaz de usuario flexible que quede bien en distintos tamaños de pantalla, tamaños de ventana, resoluciones y orientaciones. 


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Tamaños de pantalla y puntos de interrupción](screen-sizes-and-breakpoints-for-responsive-design.md)</b><br/>
El número de destinos de dispositivo y tamaños de pantalla en el ecosistema de Windows 10 es demasiado grande como para preocuparse de optimizar la interfaz de usuario para cada uno de ellos. En su lugar, se recomienda el diseño de unos anchos claves (también denominados "puntos de interrupción"): 360, 640, 1024 y 1366 epx.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[Definir diseños con XAML](layouts-with-xaml.md)</b> <br/>
Cómo usar las propiedades de XAML y los paneles de diseño para hacer que la aplicación tenga capacidad de respuesta y sea adaptable.</p>
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Paneles de diseño](layout-panels.md)</b> <br />
Obtén información sobre cada tipo de diseño de cada panel y muestra cómo usarlos para diseñar elementos de interfaz de usuario XAML.</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[Alineación, márgenes y relleno](alignment-margin-padding.md)</b> <br />
Además de las propiedades de dimensiones (ancho, alto y restricciones), los elementos también pueden tener propiedades de alineación, margen y espaciado interno que influyan en el comportamiento del diseño cuando un elemento pasa por un cálculo de diseño y se representa en una interfaz de usuario.</p> 
  </div>
</div>
</div>


