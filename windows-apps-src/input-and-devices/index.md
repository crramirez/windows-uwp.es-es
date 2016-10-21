---
description: "Personaliza tu aplicación para UWP para tipos específicos de entrada y de dispositivos. Aprovecha los comandos táctiles y de voz. Ejecuta las aplicaciones en Xbox, el teléfono e incluso el televisor."
title: "Diseño para entradas y dispositivos de la aplicación para UWP: desarrollo de aplicaciones de Windows"
author: mijacobs
keywords: "información básica de dispositivos, entradas de la aplicación, personalizar la aplicación para UWP"
translationtype: Human Translation
ms.sourcegitcommit: 5a6666d4e706d4d49d646b5bb2e43b82394eb215
ms.openlocfilehash: 85bcd15d4b9262188f0821642faf0d3d0cb7dbad

---
# Entradas y dispositivos

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Las aplicaciones para UWP admiten una amplia variedad de entradas y se ejecutan en multitud de dispositivos automáticamente; no es necesario que hagas nada para habilitar la entrada táctil o ejecutar la aplicación en un teléfono, por ejemplo. 

Sin embargo, hay ocasiones en las que quizás te interese optimizar la aplicación para determinados tipos de entradas o dispositivos. Por ejemplo, si quieres crear una aplicación para pintar, quizás te interese personalizar la manera de controlar la entrada mediante el lápiz. 

Las instrucciones de diseño y codificación de esta sección te ayudan a personalizar la aplicación para UWP para determinados tipos de entradas y dispositivos. 

## Entradas e interacciones

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Información básica de entradas](input-primer.md)</b><br/> Familiarízate con cada tipo de dispositivo de entrada y sus comportamientos, capacidades y limitaciones con determinados factores de forma.   
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Cortana](cortana-interactions.md) </b><br/> Amplía la funcionalidad básica de Cortana con comandos de voz que inicien y ejecuten una acción única en una aplicación externa.   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<b>[Controlador para juegos y control remoto](gamepad-and-remote-interactions.md)</b><br/>Las aplicaciones para UWP admiten ahora un controlador para juegos y entrada de control remoto. Los controladores para juegos y los controles remotos son los dispositivos de entrada principales para las experiencias de Xbox y TV.  
  </div>
  <div class="side-by-side-content-right">
<b>[Teclado](keyboard-interactions.md)</b><br/>La entrada de teclado es una parte importante de la experiencia de interacción del usuario global en las aplicaciones. El teclado es indispensable para personas con ciertas discapacidades o para los usuarios que, simplemente, lo consideran una manera más eficaz de interactuar con una aplicación.  
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Mouse](mouse-interactions.md)</b><br/>La entrada de mouse es ideal para las interacciones del usuario que requieren precisión al apuntar y hacer clic. Desde luego que la interfaz de usuario de Windows admite esta precisión inherente, ya que se ha optimizado para la imprecisa naturaleza de la entrada táctil.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Lápiz](pen-and-stylus-interactions.md)</b><br/>Optimiza tu aplicación para UWP para que las entradas realizadas con la pluma proporcionen tanto la funcionalidad del dispositivo señalador como la mejor experiencia de Windows Ink para tus usuarios.   
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Voz](speech-interactions.md)</b><br/>Integra el reconocimiento de voz y texto a voz (también denominado TTS o síntesis de voz) directamente en la experiencia del usuario de la aplicación.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Función táctil](touch-interactions.md)</b><br/>UWP incluye una serie de mecanismos diferentes para administrar la entrada táctil que te permiten crear una experiencia envolvente que los usuarios de tus aplicaciones pueden explorar con confianza.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Panel táctil](touchpad-interactions.md)  </b><br/>Diseña tu aplicación de modo que los usuarios puedan interactuar con ella a través de un panel táctil. Un panel táctil combina la entrada multitáctil indirecta con la entrada precisa de un dispositivo señalador, como un mouse. Esta combinación hace que el panel táctil sea ideal tanto para una interfaz de usuario optimizada para entrada táctil como para los destinos de menor tamaño de las aplicaciones de productividad.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Varias entradas](multiple-input-design-guidelines.md)  </b><br/>Para acoger al máximo número de usuarios y dispositivos posibles, te recomendamos que diseñes tus aplicaciones para que funcionen con el máximo posible de tipos de entrada (gestos, voz, entrada táctil, panel táctil, mouse y teclado). De este modo, se maximizarán la flexibilidad, la facilidad de uso y la accesibilidad.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Zoom óptico y cambio de tamaño](guidelines-for-optical-zoom.md)</b><br/>En este artículo se describen los elementos de zoom y cambio de tamaño de Windows. También se ofrecen instrucciones de experiencia de usuario para que uses estos nuevos mecanismos de interacción en las aplicaciones.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Movimiento panorámico](guidelines-for-panning.md)</b><br/>El movimiento panorámico o el desplazamiento permiten a los usuarios navegar dentro de una única vista para mostrar el contenido de la vista que no cabe en la ventanilla.  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Rotación](guidelines-for-rotation.md)</b><br/> En este artículo se describe la nueva interfaz de usuario de Windows para realizar una rotación. También se ofrecen directrices sobre la experiencia del usuario que debes tener presente cuando uses este nuevo mecanismo de interacción en una aplicación para UWP.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Selección de texto e imágenes](guidelines-for-textselection.md)</b><br/>En este artículo se describe la selección y manipulación de texto, imágenes y controles, y se ofrecen instrucciones de experiencia del usuario que se deben tener en cuenta al usar estos mecanismos en las aplicaciones.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Selección del destino](guidelines-for-targeting.md)</b><br/>La selección táctil del destino en Windows usa toda el área de contacto de cada dedo que es detectado por un digitalizador táctil. El conjunto más grande y más complejo de datos de entrada notificado por el digitalizador se usa para aumentar la precisión cuando se determina el destino previsto (o con más probabilidades) del usuario.
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Comentarios visuales](guidelines-for-visualfeedback.md)</b><br/>Usa la información visual para mostrar a los usuarios cuándo se detectan, se interpretan y se controlan sus interacciones. La información visual puede ayudar a los usuarios al promover la interacción. Indica si una interacción se ha realizado correctamente, lo que mejora la sensación de control del usuario. También transmite los estados del sistema y reduce los errores.  
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Identificar dispositivos de entrada](identify-input-devices.md)</b><br/>Identifica los dispositivos de entrada que se conectan a un dispositivo de la Plataforma universal de Windows (UWP) e identifica sus capacidades y atributos. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Entrada de texto personalizado](custom-text-input.md)</b><br/>Las API de texto principales en el espacio de nombres Windows.UI.Text.Core habilitan una aplicación para UWP para recibir una entrada de texto desde cualquier servicio de texto compatible con dispositivos Windows. Esto permite que la aplicación reciba texto en cualquier idioma y de cualquier tipo de entrada, como el teclado, voz o lápiz.
</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Controlar la entrada de puntero](handle-pointer-input.md)</b><br/>Recibe, procesa y administra los datos de entrada de dispositivos señaladores, como la función táctil, el mouse, la pluma o el lápiz y el panel táctil, en aplicaciones para la Plataforma universal de Windows (UWP).
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b><br/>   
</p>
  </div>
</div>
</div>


## Dispositivos

El hecho de conocer los dispositivos que admiten aplicaciones para UWP te ayudará a ofrecer la mejor experiencia del usuario para cada factor de forma. Al diseñar un dispositivo en particular, hay que tener en cuenta principalmente cómo aparecerá la aplicación en el dispositivo dónde, cuándo y cómo se usará la aplicación en el dispositivo y cómo interactuará el usuario con el dispositivo.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[Información básica de dispositivos](device-primer.md)</b><br/>El hecho de conocer los dispositivos que admiten aplicaciones para UWP te ayudará a ofrecer la mejor experiencia del usuario para cada factor de forma. 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[Diseño para Xbox y televisión](designing-for-tv.md)</b><br/>Aprende a diseñar tu aplicación para la Plataforma universal de Windows (UWP) de manera que se vea y funcione bien en la Xbox One y las pantallas de televisión.
</p>
  </div>
</div>
</div>




<!--HONumber=Aug16_HO5-->


