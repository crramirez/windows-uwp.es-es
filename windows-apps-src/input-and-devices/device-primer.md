---
author: mijacobs
Description: "Familiarizarte con los dispositivos que admiten aplicaciones para la Plataforma universal de Windows (UWP) te ayudará a ofrecer la mejor experiencia de usuario para cada factor de forma."
title: "Dispositivo básico para aplicaciones para la Plataforma universal de Windows (UWP)"
ms.assetid: 7665044E-F007-495D-8D56-CE7C2361CDC4
label: Device primer
template: detail.hbs
keywords: "dispositivo, entrada, interacción"
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 66afc2412323817f15594bb25e8f86475fc6f73f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
#  <a name="device-primer-for-universal-windows-platform-uwp-apps"></a>Dispositivo básico para aplicaciones para la Plataforma universal de Windows (UWP)

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

![dispositivos de Windows](images/device-primer/device-primer-ramp.png)

Familiarizarte con los dispositivos que admiten aplicaciones para la Plataforma universal de Windows (UWP) te ayudará a ofrecer la mejor experiencia de usuario para cada factor de forma. Al diseñar para un dispositivo en particular, hay que tener en cuenta principalmente cómo aparecerá la aplicación en el dispositivo, dónde, cuándo y cómo se usará en el dispositivo y cómo interactuará el usuario con dicho dispositivo.

## <a name="pcs-and-laptops"></a>PC y portátiles


Los portátiles y los PC Windows incluyen una amplia gama de dispositivos y tamaños de pantalla. En general, los PC y los portátiles pueden mostrar más de información que los teléfonos o tabletas.

Tamaños de pantalla
-   13 pulgadas y tamaños superiores

![un PC](images/device-primer/device-primer-desktop.png)

Uso típico
-   En las aplicaciones de los equipos de escritorio y de los portátiles, se puede ver el uso compartido, pero no por más de un usuario a la vez, normalmente por períodos más largos.

Consideraciones sobre la interfaz de usuario
-   Las aplicaciones pueden tener una vista de ventana, cuyo tamaño viene determinado por el usuario. Según el tamaño de ventana, puede haber entre uno y tres marcos. En los monitores más grandes, la aplicación puede tener más de tres marcos.

-   Al usar una aplicación en un equipo de escritorio o un portátil, el usuario tiene control sobre los archivos de la aplicación. Como diseñador de la aplicación, asegúrate de proporcionar los mecanismos para administrar su contenido. Piensa en incluir comandos y funciones como, por ejemplo, "Guardar como", "Archivos recientes", etc.

-   La función Atrás del sistema es opcional. Cuando un desarrollador de aplicaciones elige mostrarla, aparece en la barra de título de la aplicación.

Entradas
-   Mouse
-   Teclado
-   Entrada táctil en equipos portátiles y equipos de escritorio todo en uno.
-   A veces se usan mandos, como el controlador de Xbox.

Funcionalidades típicas del dispositivo
-   Cámara
-   Micrófono

## <a name="tablets-and-2-in-1s"></a>Tabletas y 2 en 1


Los Tablet PC ultraportátiles están equipados con pantallas táctiles, cámaras, micrófonos y acelerómetros. El tamaño de la pantalla de las tabletas varía de 7" a 13,3". Los dispositivos 2 en 1 pueden actuar como tableta o como portátil con teclado y mouse, en función de la configuración (por lo general implica doblar la pantalla hacia atrás o inclinarla en posición vertical).

Tamaños de pantalla
- De 7" a 13,3" para tableta
- 13,3" y superior para 2 en 1

![un dispositivo de tableta](images/device-primer/device-primer-tablet.png)

Uso típico
-   Aproximadamente el 80% del uso de la tableta es por parte del propietario, mientras que el 20% restante del uso es compartido.
-   Normalmente se usa en casa como un dispositivo complementario mientras se ve el televisor.
-   Se usa durante períodos más prolongados que los teléfonos y tabléfonos.
-   Se escribe texto en ráfagas cortas.

Consideraciones sobre la interfaz de usuario
-   En las orientaciones horizontal y vertical, las tabletas permiten dos marcos a la vez.
-   El botón Atrás del sistema se encuentra en la barra de navegación.

Entradas
-   Función táctil
-   Lápiz
-   Teclado externo (en ocasiones)
-   Mouse (en ocasiones)
-   Voz (en ocasiones)

Funcionalidades típicas del dispositivo
-   Cámara
-   Micrófono
-   Sensores de movimiento
-   Sensores de ubicación

> [!NOTE]
> La mayoría de las consideraciones para los PC y portátiles también se aplican a los dispositivos 2 en 1.

## <a name="xbox-and-tv"></a>Xbox y televisión

La experiencia de estar sentado en tu sofá al otro lado de la sala, con un controlador para juegos o control remoto para interactuar con tu televisor, se llama la **experiencia de 10 pies**. Se denomina así porque el usuario generalmente está sentado aproximadamente a 10 pies de distancia de la pantalla. Esto proporciona desafíos que no están presentes, por ejemplo, en la experiencia de *2 pies* o interactuando con un PC. Si estás desarrollando una aplicación para Xbox One o cualquier otro dispositivo conectado a una pantalla de TV y que puede usar un controlador para juegos o un control remoto para la entrada, siempre debes tener esto en cuenta.

Diseñar tu aplicación para UWP para la experiencia de 10 pies es muy diferente que diseñarla para cualquiera de las otras categorías de dispositivos mencionadas aquí. Para obtener más información, consulta [Diseño para Xbox y televisión](designing-for-tv.md).

Tamaños de pantalla
- 24" y superior

![Xbox y televisión](images/device-primer/device-primer-tv-and-xbox.png)

Uso típico
- A menudo se comparte entre varias personas, aunque también suele usarse por parte de una.
- Se usa normalmente durante períodos más largos.
- Se usa normalmente en casa, por lo que permanece en un solo lugar.
- Rara vez pide entrada de texto porque se tarda más con un controlador para juegos o control remoto.
- Se corrige la orientación de la pantalla.
- Normalmente solo se ejecuta una aplicación cada vez, pero puede que sea posible acoplar aplicaciones a un lado (como en Xbox).

Consideraciones sobre la interfaz de usuario
- Por lo general, las aplicaciones conservan el mismo tamaño, a menos que se ajuste otra aplicación al lado.
- La funcionalidad Atrás del sistema es útil y se ofrece en la mayoría de aplicaciones de Xbox, accesible mediante el botón B del controlador para juegos.
- Dado que el cliente está sentado a aproximadamente 10 pies de la pantalla, asegúrate de que la interfaz de usuario es lo suficientemente grande y clara para que se vea.

Entradas
- Controlador para juegos (por ejemplo, un controlador de Xbox)
- Control remoto
- Voz (en ocasiones, si el cliente tiene un sensor Kinect o unos auriculares)

Funcionalidades típicas del dispositivo
- Cámara (en ocasiones, si el cliente tiene un sensor Kinect)
- Micrófono (en ocasiones, si el cliente tiene un sensor Kinect o unos auriculares)
- Sensores de movimiento (en ocasiones, si el cliente tiene un sensor Kinect)

## <a name="phones-and-phablets"></a>Teléfonos y tabléfonos


Los más usados de todos los dispositivos informáticos, los teléfonos pueden hacer muchas cosas con la superficie de pantalla limitada y las entradas básicas. Los teléfonos están disponibles en una amplia variedad de tamaños; los teléfonos más grandes se denominan tabléfonos. Las experiencias de la aplicación en tabléfonos son similares a las de los teléfonos, pero debido a que la superficie de pantalla de los tabléfonos es mayor, se pueden producir algunos cambios clave en el consumo de contenido.

Con Continuum para teléfonos, una nueva experiencia de dispositivos móviles compatibles con Windows 10, los usuarios pueden conectar sus teléfonos a un monitor e incluso usar un mouse y un teclado para que los teléfonos funcionen como un portátil. (Para obtener más información, consulta el [artículo Continuum para teléfonos](http://go.microsoft.com/fwlink/p/?LinkID=699431)).

Tamaños de pantalla
-   Entre 4 y 5 pulgadas para teléfono
-   Entre 5,5 y 7 pulgadas para tabléfono

![windows phone](images/device-primer/device-primer-phablet.png)

Uso típico
-   Se usan principalmente en orientación vertical, principalmente debido a la facilidad para sostener el teléfono con una mano y poder interactuar completamente interactuar con él de esta forma, pero hay algunas experiencias que funcionan bien en horizontal, como ver fotos y vídeo, leer un libro y escribir texto.
-   Usados principalmente por tan solo una persona, el propietario del dispositivo.
-   Siempre al alcance, normalmente se colocan en un bolsillo o una bolsa.
-   Se usan durante períodos breves.
-   A menudo, los usuarios realizan varias tareas cuando usan el teléfono.
-   Se escribe texto en ráfagas cortas.

Consideraciones sobre la interfaz de usuario
-   Las dimensiones reducidas de la pantalla de un teléfono solo permite ver un marco cada vez tanto en orientación vertical como en horizontal. Todos los patrones de navegación jerárquica de un teléfono usan el modelo de "exploración", donde el usuario navega a través de capas de interfaz de usuario de un solo marco.

-   De manera similar a los teléfonos, en modo vertical, los tabléfonos solo pueden ver un marco a la vez. Sin embargo, puesto que la superficie de pantalla disponible de un tabléfono es mayor, los usuarios pueden girar hacia la orientación horizontal y mantenerla de manera que haya dos marcos visibles a la vez.

-   En las orientaciones horizontales y verticales, asegúrate de que haya suficiente superficie de pantalla para la barra de la aplicación cuando el teclado en pantalla está arriba.

Entradas
-   Función táctil
-   Voz

Funcionalidades típicas del dispositivo
-   Micrófono
-   Cámara
-   Sensores de movimiento
-   Sensores de ubicación

 

## <a name="surface-hub-devices"></a>Dispositivos de Surface Hub


Microsoft Surface Hub es un dispositivo de colaboración en equipo de pantalla grande diseñado para el uso simultáneo por parte de varios usuarios.

Tamaños de pantalla
-   55 y 84 pulgadas

![un Surface Hub](images/device-primer/device-primer-surfacehub3.png)

Uso típico
-   Las aplicaciones de Surface Hub ven el uso compartido durante períodos cortos de tiempo, como en las reuniones.

-   Los dispositivos de Surface Hub suelen estar inmóviles y raras veces se mueven.

Consideraciones sobre la interfaz de usuario
-   Las aplicaciones de Surface Hub pueden aparecer en uno de cuatro estados: lleno (vista en pantalla completa estándar), en segundo plano (oculto mientras la aplicación sigue funcionando, disponible en el conmutador de tareas), relleno (vista fija que ocupa el área de ensayo disponible) y acoplado (vista variable que ocupa los lados izquierdo o derecho del área).
-   En modo acoplado o en los modos de relleno, el sistema muestra la barra lateral de Skype y reduce la aplicación horizontalmente.
-   La función Atrás del sistema es opcional. Cuando un desarrollador de aplicaciones elige mostrarla, aparece en la barra de título de la aplicación.

Entradas
-   Función táctil
-   Lápiz
-   Voz
-   Teclado (en pantalla/remoto)
-   Panel táctil (remoto)

Funcionalidades típicas del dispositivo
-   Cámara
-   Micrófono

 

## <a name="windows-iot-devices"></a>Dispositivos de Windows IoT


Los dispositivos Windows IoT son una nueva clase de dispositivos que se centran en la incrustación de dispositivos electrónicos pequeños, sensores y conectividad dentro de los objetos físicos. Por lo general, estos dispositivos están conectados a través de una red o Internet para informar sobre los datos reales que detectan y, en algunos casos, actúan sobre ellos. Es posible que los dispositivos no tengan ninguna pantalla (también conocidos como dispositivos "sin periféricos") o que estén conectados a una pantalla pequeña (conocidos como dispositivos "con periféricos") con un tamaño de pantalla generalmente de 3,5 pulgadas o inferior.

Tamaños de pantalla
-   3,5 pulgadas o inferior
-   Algunos dispositivos no tienen ninguna pantalla

![dispositivo IoT](images/device-primer/device-primer-iot-device.png)

Uso típico
-   Por lo general, estos dispositivos están conectados a través de una red o Internet para informar sobre los datos reales que detectan y, en algunos casos, actúan sobre ellos.
-   Estos dispositivos solo pueden ejecutar una aplicación de cada vez, a diferencia de los teléfonos u otros dispositivos más grandes.
-   No es algo con lo que se interactúe todo el tiempo, sino que, en su lugar, está disponible cuando se necesita y desaparece cuando no.
-   La aplicación no tiene una prestación Atrás específica. Esto es responsabilidad de los desarrolladores.

Consideraciones sobre la interfaz de usuario
-   Los dispositivos "sin periféricos" no tienen ninguna pantalla.
-   La pantalla de los dispositivos "con periféricos" es mínima y solo muestra lo necesario debido a la funcionalidad y espacio en pantalla limitados.
-   La mayoría de veces, la orientación está bloqueada, por lo que tu aplicación solo necesita tener en cuenta una dirección de la pantalla.

Entradas
-   Variable, en función del dispositivo

Funcionalidades típicas del dispositivo
-   Variable, en función del dispositivo