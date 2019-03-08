---
title: Empezar a trabajar con entre jugar a juegos
description: Obtenga información sobre cómo desarrollar entre jugar a juegos que se ejecutan en las consolas de PC y Xbox One.
ms.assetid: 6c8e9d08-a3d2-4bfc-90ee-03c8fde3e66d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, cross-play, jugar desde cualquier lugar
ms.localizationpriority: medium
ms.openlocfilehash: 79b0c211291d8a27456126ea0378f9093d1dccab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646730"
---
# <a name="get-started-with-cross-play-games"></a>Empezar a trabajar con entre jugar a juegos

## <a name="summary"></a>Resumen

Con el lanzamiento de Windows 10, los desarrolladores de juegos será capaz de lanzar productos single tanto en Xbox One (como un juego XDK) y Windows 10 (como un juego para UWP). En algunos casos, los programadores desearán habilitar las play entre estos juegos, donde las versiones de su juego de Xbox One y Windows 10 están unificadas a través de servicios de Xbox Live, como varios jugadores, guardar juegos, logros y mucho más. Para habilitar entre-play, estos juegos compartirán una única configuración de servicio de Id. de título y Xbox Live en las versiones el XDK y UWP del juego.

Ingesta en un juego hoy XDK + UWP requiere 4 pasos principales:

1.  Creación de su producto UWP en el centro de partners

2.  Crear tu juego XDK en XDP, seleccionar las plataformas que desea compartir configuración XBL entre

3.  Enlazar la información del producto para UWP a su producto XDK en XDP

4.  Configurar y publicar Xbox Live a través de XDP

El propósito de este documento es detallar aún más estos 4 pasos que resulte tan fácil como sea posible para los empleados de Xbox para la ingesta de juegos de cross-plat XDK + UWP

## <a name="terminology"></a>Terminología

### <a name="scenario-terms"></a>Términos de escenario

1.  Cross-Play: Un juego que se está liberando en más de una plataforma, pero comparte una Xbox única Id. del título y configuración del servicio. El resultado final es que ambas versiones del juego comparten la misma Xbox Live configuración - logros, tablas de clasificación, juegos guardados, participan varios jugadores y mucho más.

2.  Centro de partners: El [portal](https://partner.microsoft.com/dashboard) donde puede reservar las identidades de aplicación para su uso en UWP hoy y el programa de instalación de Xbox Live configuración de desarrollo para UWP.

3.  XDP: El Xbox Portal para desarrolladores, que existe hoy en día para la ingesta, configurar y publicar juegos Xbox uno XDK y SRA y verá el uso se ha agregado a ingerir, configurar y publicar XDK + UWP entre-jugar a juegos.

### <a name="identity-terms"></a>Términos de identidad

1.  Id. de título: Esto es el identificador de título de Xbox, utilizado para identificar cada juego en Xbox Live. Un identificador de título que se asigna a un solo producto, que puede abarcar varias plataformas.

2.  Id. de configuración de servicio (¿SCID): Cada título de Xbox (identificado por un identificador de título) tiene un identificador de configuración de servicio correspondiente (también conocido como ¿SCID). Este identificador permite Xbox Live identificar las reglas o configuración que se utiliza para interactuar con el título.

3.  Nombre de familia de paquete (PFN): Se trata de una identidad asignada a cada producto que creó en el centro de partners. Una vez enlazada la UWP a la identidad de este producto del centro de partners, tardará en este PFN. PFNs son identificadores de producto único, que pueden abarcar varias plataformas. PFNs son 1:1 con los identificadores de título de Xbox.

4.  Id. de aplicación MSA: También conocido como identificador de cliente de MSA, esto es otra identidad de aplicación asignado por MSA en tiempo de creación del producto en el centro de partners. Esta identidad ayuda a los servicios de Microsoft a identificar la aplicación. Los identificadores de aplicación de MSA son 1:1 con PFNs (y en consecuencia con identificadores de título de Xbox).

## <a name="scenario-overview"></a>Información general sobre el escenario

### <a name="what-is-cross-play"></a>¿Qué es entre Play?

Presentación de una experiencia de Windows 10; Cross-play es entre dispositivos juegos entre el PC y Xbox One con uso compartido de una sola configuración de Xbox Live en las versiones de dispositivo para iluminar escenarios como varios jugadores entre dispositivos, logros & marcadores, de juegos y guarda el juego.

### <a name="what-are-the-pros-and-cons-of-cross-play"></a>¿Cuáles son las ventajas y desventajas de entre-Play?

Cross-Play es probablemente el enfoque adecuado para usted si desea que las versiones XDK y UWP de su juego para:

-   Entable entre dispositivos participan varios jugadores (frente a Xbox One. PC) en al menos un modo de juego multijugador

-   Compartir una única operación de guardar juegos que el usuario puede usar en ambos dispositivos

-   Tener un único conjunto de logros & puntuación / desafíos / marcadores que aditivamente puede progresar contra en ambos dispositivos

Es probable que entre-Play no es el enfoque adecuado para usted si:

-   Desea evitar que los jugadores de PC y Xbox One de su juego comprometidos con varios jugadores en todos los dispositivos en todos los modos de juego multijugador

-   Desea mantener Xbox One y juego PC guardar independiente (quizás por motivos de seguridad o de confianza)

-   Desea que las versiones de Xbox One y PC de su juego tener puntuación independiente (también conocido como los usuarios que compran Xbox One y PC pueden recibir 1000 puntuación para cada plataforma en lugar de un 1000 compartido)

En general, Cross-Play agrega el mayor valor para:

-   Gratis para jugar / Xbox jugar desde cualquier lugar, que hacen hincapié en la continuidad entre las versiones del juego de Xbox One y PC

-   Juegos con varios jugadores entre dispositivos entre Xbox One y PC

**NOTA**: Cross-Play está disponible tanto para los juegos nuevos que se están lanzando las versiones XDK y UWP de su juego al mismo tiempo, así como juegos que ya se han enviado un XDK, pero están agregando una versión de UWP.

### <a name="what-are-the-restrictions-of-cross-play"></a>¿Cuáles son las restricciones de entre-Play?

Hasta que Xbox One es compatible con los juegos UWP, entre jugar a juegos requiere una versión UWP (para equipos Windows 10) del juego y XDK (para la consola Xbox One).

Cualquier XDK + juego para UWP entre-Play viene con algunas restricciones importantes:

1.  **Títulos XDK se deben ingerir en XDP**. Tanto de una experiencia de publicación principal y la configuración del servicio, centro de partners no está equipado para admitir títulos XDK basado.

2.  **Puede ser una configuración de servicio único creada en XDP utilizado por la versión de un juego XDK y UWP**. Hemos agregado nuevas características para XDP para permitir un juego compartir una configuración de servicio única entre las versiones XDK y UWP. La versión de UWP aun así deberá publicarse en el centro de partners para los paquetes / catalog, pero la publicación de configuración de todos los servicios que puede hacerse en XDP.

3.  **Configuración del servicio no se puede dividir entre XDP y centro de partners**. XDP y centro de partners no están enterados de entre sí – publicar en una sobrescrituras cualquier existente publica desde la otra. Esto tiene el potencial de irreparablemente interrumpir la configuración del servicio y crear experiencias de usuario terrible (fuga logros perdido juegos guardados, etc.) y, como resultado, se requiere el 100% de la configuración del servicio para realizarse en XDP entre-jugar a juegos XDK + UWP.

### <a name="create-your-uwp-product-in-partner-center"></a>Creación de su producto UWP en el centro de partners

Crear un producto UWP en el centro de partners, siga a la guía: [Adición de Xbox Live a un proyecto UWP nuevo o existente](get-started-with-visual-studio-and-uwp.md)

## <a name="setup-your-xdk-product-in-xdp"></a>Programa de instalación de su producto XDK en XDP

Ahora que se crea la UWP, está listo para configurar su producto XDK en XDP. Si no dispone de un título XDK, debe crear uno.

### <a name="create-your-xdp-product"></a>Creación de su producto XDP

Trabajar con su administrador de cuentas para crear un nuevo producto en el publicador en XDP ([https://xdp.xboxlive.com/](https://xdp.xboxlive.com/User/Publisher)).

Al crear el producto en XDP, asegúrese de que se desplaza a la parte inferior de la sección izquierda de la interfaz de usuario para seleccionar las plataformas. Compruebe todas las plataformas que **pretende algún día** liberar el juego en con la integración de entre-play Xbox Live.

Una vez haya seleccionado las plataformas, especifique el tipo de acceso a los recursos para su juego (probablemente un título XDK) y los mecanismos de lanzamiento prevista para este producto.


![](../images/ingesting_crossplay_games_xdp/image4.png)
### <a name="update-your-xdp-product-platforms"></a>Actualizar las plataformas de producto XDP

Si ya tiene un producto XDK existente en XDP, debe actualizarse para admitir la plataforma de PC. Para ello, una vez en el producto, vaya a configuración del producto &gt; tipo de plataforma.

![](../images/ingesting_crossplay_games_xdp/image10.png)

En esta página, seleccione las plataformas que desea admitir (las opciones son Windows Mobile, umbral de PC y Xbox One). Cuando esté satisfecho con la selección, seleccione el botón "Enviar plataformas".

Este cambio inmediatamente se realizarán en vivo (sin necesidad de una configuración del servicio, el catálogo o el binario de publicación para que esto surta efecto). Tenga en cuenta que esta configuración abarca los espacios aislados, no puede tener tipos de plataforma diferente por espacio aislado para su juego.

### <a name="enter-your-msa-app-id"></a>Escriba el identificador de la aplicación MSA

Una vez creado el producto XDP, vaya a la página de configuración del producto para el producto introducir el identificador de aplicación de MSA que creó anteriormente. Puede ponerse en configuración del producto, haga clic en el extremo izquierdo "cuadro de estado" en la barra de tareas para el producto en XDP, como se muestra a continuación.

![](../images/ingesting_crossplay_games_xdp/image11.png)

Una vez realizada a la página de configuración del producto, seleccione la sección "Configuración de Id. de aplicación". En esta área, puede escribir el identificador de aplicación de MSA que recuperó y colóquelo en el campo "Id. de aplicación", como se muestra a continuación.

![](../images/ingesting_crossplay_games_xdp/image12.png)

**Le** **no es necesario que escriba el nombre y el publicador atribución**, **y específicamente no debe usar el vínculo "Obtener el identificador de aplicación" en la página**, tal y como ya tiene un identificador de aplicación de MSA necesita que escriba en este campo y no desea que uno nuevo generado para la aplicación.

Después de escribir el identificador de la aplicación de MSA en el campo "Id. de aplicación", haga clic en el botón "Enviar la configuración de Id. de aplicación". Esto le permitirá ahorrar la información de Id. de aplicación de MSA con seguridad de Xbox Live: siempre que realice una solicitud para recuperar un XToken de la UWP, dentro de la notificación de título asignará ahora a este producto XDP (siempre y cuando la UWP utiliza la identidad del manifiesto AppX se crea correctamente desarrollo en el centro de partners).

### <a name="enter-the-partner-center-pfn-into-xdp"></a>Escriba el PFN de centro de partners en XDP

Mientras que los pasos anteriores son suficientes para obtener su juego para UWP autenticado y el uso de Xbox Live con la configuración del servicio cree y publique en XDP, ciertas características de Xbox Live (por ejemplo, invitaciones para varios jugadores) deben ser consciente de PFN de su juego UWP para poder funcionar correctamente.

Para ello, vaya a configuración del producto &gt; enlace del centro de desarrollo

![](../images/ingesting_crossplay_games_xdp/image13.png)

En esta página, escriba el PFN de la aplicación UWP (tal y como se recuperó en la sección 4.1.1) y luego seleccione el botón "Guardar".

Esta configuración no se realiza en directo inmediatamente. Que se ponga en vivo a través de futuro configuración del servicio se publica en un espacio aislado. Por lo tanto, esta información es un espacio aislada y debe publicarse para cada espacio aislado esté disponible.

### <a name="flag-your-app-for-xbox-cert-in-partner-center"></a>Marca la aplicación para Xbox certificado en el centro de partners

En la actualidad, obtener su juego reconoce como Xbox Live habilitado para Xbox Cert requiere alguna intervención manual. Trabajo con el Administrador de versión para marcar la aplicación es que Xbox Live habilitada en el centro de partners para la detección de SmartCert.

### <a name="update-your-uwp-game-in-the-xbox-app"></a>Actualizar su juego para UWP en la aplicación de Xbox

Normalmente, el App Xbox usa un identificador de título generado automáticamente para todos los juegos UWP a la potencia de que experiencias de Xbox Live en el App Xbox. Para tener su juego para UWP use correctamente el identificador de título XDP generado en el App Xbox, una actualización de datos debe realizarse desde el centro de partners, **antes de enviar su juego para la versión UWP.**

Para ello, póngase en contacto con su madre y dígales que desea actualizar el identificador de título de la aplicación de Xbox para el nombre del título.  No olvide incluir el identificador de título que se creó en XDP (visible en XDP bajo Configuración del producto &gt; detalles del producto) y la dirección URL para Windows 10 para su UWP (visible en el centro de partners en administración de aplicaciones &gt; identidad de aplicación).

## <a name="configure-xbox-live-in-xdp"></a>Configurar Xbox Live en XDP

### <a name="service-configuration"></a>Configuración del servicio

Con nuestros productos XDP y centro de partners configurada correctamente y enlazado, es libre para configurar la configuración compartida de Xbox Live en XDP como lo haría normalmente para un título XDK.

**RECORDATORIO** : enlazado juegos XDK + UWP deben, bajo ninguna circunstancia, habilitar, configurar o publicar la configuración del servicio Xbox Live a través del centro de partners. Si no se sigue esta guía puede dañar permanentemente la configuración de Xbox Live para su juego.

### <a name="catalog-configuration"></a>Configuración de catálogo

Para un juego XDK + UWP, configuración de catálogo se debe configurar dos veces: una vez en XDP para el XDK y también en el centro de partners para la UWP.

Para la configuración XDP, esto es exactamente igual que un producto XDK normal. Para la configuración del centro de partners, se pueden encontrar pasos más detallados [aquí](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      Para juegos solo UWP, debe instalarse sólo un conjunto limitado de configuración de catálogo. En concreto, información de Marketing debe rellenarse para UWP solo juegos, como la configuración del servicio consume las cadenas especificadas aquí para cualquier XDP configure juego de Xbox Live. Además, availabilities deben ser el programa de instalación con "no disponible" para asegurarse de que el juego nunca aparecerán en el catálogo de Xbox One cuando la información de catálogo para el juego se publica.
    </td>
  </tr>
</table>

### <a name="binary-configuration"></a>Configuración binaria

Para un juego XDK + UWP, configuración binario se debe configurar dos veces: una vez en XDP para el XDK y también en el centro de partners para la UWP.

Para la configuración XDP, esto es exactamente igual que un producto XDK normal. Para la configuración del centro de partners, se pueden encontrar pasos más detallados [aquí](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      Para UWP solo juegos, no hay ninguna necesidad para la configuración binario en XDP. Puede omitir este paso completamente.
    </td>
  </tr>
</table>

## <a name="publish-in-xdp"></a>Publicar en XDP

Por lo general, la publicación de un juego XDK + UWP sigue el mismo proceso que la publicación por separado de un título XDK en XDP y su UWP en el centro de partners. Por lo tanto el debajo se centra en un proceso único o consideraciones que debe realizar para entre jugar a juegos.

### <a name="dev-sandbox-publishing"></a>Publicación de espacio aislado de desarrollo

### <a name="no-dev-sandbox-catalog-equivalent-for-microsoft-store"></a>Ningún equivalente de catálogo de espacio aislado de desarrollo para Microsoft Store

Aunque XDP permite publicar el catálogo de & binario a la versión de espacio aislado de desarrollo del catálogo de Xbox One, el catálogo de Microsoft Store no tiene se admite espacio aislado. Por lo tanto, las pruebas de la UWP en un espacio aislado de desarrollo requiere instalación de prueba que UWP y reproducción directamente. Esto no afecta a las pruebas de Xbox Live, pero puede alterar el estándar de los procesos de prueba.

<table>
  <tr>
    <td>
      Para un juego para UWP solo, sigue siendo necesario para publicar el XDK publicar información del catálogo de título para desbloquear la configuración del servicio, aunque el juego para UWP solo tiene presencia de catálogo no Xbox One.
    </td>
  </tr>
</table>

### <a name="cert-sandbox-publishing"></a>Publicación de espacio aislado CERT

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>Coordinación entre el centro de partners y la publicación de XDP

En XDP, pasar de CERT Retail es dos acciones independientes y explícitas. Sin embargo, en el centro de partners, el proceso de envío automáticamente mueve tu juego a través de la certificación y comercial. Por lo tanto, hay un orden de operaciones para respetar.

Cuando esté listo para ir a la certificación, debe seguir los pasos siguientes en orden:

1.  Publicar su producto XDK en XDP a CERT (incluido el catálogo, binario y configuración de servicio)

2.  Iniciar el envío del centro de partners para su producto UWP

    1.  **No olvide seleccionar "Publicar esta aplicación manualmente" o "No antes que \[fecha\]" en el campo de fecha de publicación.** Si no lo hace, se pudo liberar su juego para UWP para la venta por menor automáticamente sin intervención del usuario

<table>
  <tr>
    <td>
      Para un juego para UWP solo, sigue siendo necesario para publicar la configuración del servicio y el catálogo en CERT en XDP antes de iniciar el envío del centro de partners, incluso aunque no tenga un binario de título XDK que va a publicar.
    </td>
  </tr>
</table>

### <a name="retail-sandbox-publishing"></a>Publicación de espacio aislado de minoristas

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>Coordinación entre el centro de partners y la publicación de XDP

Una vez que se ha completado la certificación de su título XDK y ha dejado la certificación de la UWP y está listo para publicar, está listo para publicar la aplicación a la venta directa. Nuevamente, es importante seguir este proceso en un orden específico para asegurarse de que su título XDK y UWP estén alineados.

1.  Una vez que esté listo, publique su producto XDK en XDP Retail (incluido el catálogo, binario y configuración de servicio)

2.  En el centro de partners, en la página de certificación de su producto, seleccione "Publicar ahora" si previamente se ha seleccionado "Publicar esta aplicación manualmente" o esperar a que la publicación que se produzca si ha seleccionado "No antes que \[fecha\]".

Una vez completados estos pasos, debe tener el título XDK y juego para UWP publicado en el mundo, con una configuración de servicio compartido en el sector MINORISTA. Enhorabuena.

<table>
  <tr>
    <td>
      Para un juego para UWP solo, aún es necesaria para publicar el catálogo y configuración de servicio Retail en XDP antes de publicación complete en el centro de partners, en caso contrario, la fecha de publicación UWP no podrá acceder a Xbox Live.
    </td>
  </tr>
</table>
