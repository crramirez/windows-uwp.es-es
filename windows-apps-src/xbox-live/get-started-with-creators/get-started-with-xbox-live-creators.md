---
title: Introducción al Programa de creadores de Xbox Live
description: Proporciona vínculos que te ayudarán a empezar a trabajar con el Programa de creadores de Xbox Live.
ms.assetid: 2a744405-7ee4-42b4-8f36-9916e8c3a530
ms.date: 12/13/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cce9d34679884d48475b7a7ae0fa8286204a6289
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615880"
---
# <a name="get-started-with-the-xbox-live-creators-program"></a>Introducción al Programa de creadores de Xbox Live
 
El Programa de creadores de Xbox Live te permite publicar rápidamente y directamente tus juegos para Xbox One y Windows 10, con un proceso de certificación simplificado y sin necesidad de aprobación conceptual. Si tu juego integra Xbox Live y sigue nuestras [directivas estándar de Store](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx), estás listo para publicar. En este artículo se describen los pasos necesarios para poner en funcionamiento tu juego con la integración de Xbox Live. 

El Programa de creadores de Xbox Live debe ser una aplicación de Plataforma universal de Windows (UWP). En el caso de Xbox One, consulta [UWP en Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) y, en concreto, [Recursos del sistema de aplicaciones para UWP y juegos en Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation). Los juegos publicados a través del Programa de creadores de Xbox Live no tienen acceso a los logros o servicios de multijugador en línea. Para obtener una lista completa de los servicios admitidos, consulta la [Tabla de características de información general del programa de desarrolladores](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview#feature-table).

## <a name="1-ensure-you-have-a-title-created-in-partner-center"></a>1. Asegúrese de que tiene un título que se creó en el centro de partners
Se debe definir cada título de Xbox Live en [centro de partners](https://partner.microsoft.com/dashboard) antes de poder iniciar sesión y realizar llamadas de servicio de Xbox Live.  [Crear un nuevo título de creadores](create-and-test-a-new-creators-title.md) te mostrará cómo hacerlo.

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. Siga la guía correspondiente para configurar su motor de juego o IDE
Puedes seguir la "guía de introducción" adecuada para tu plataforma y motor y descubre los aspectos básicos de Xbox Live mientras avanzas:

* [Desarrollar un título de creadores con Visual Studio](develop-creators-title-with-visual-studio.md) le mostrará cómo vincular el proyecto de Visual Studio con la configuración de Xbox Live en el centro de partners.
* [Desarrollar un título de creadores con Unity](develop-creators-title-with-unity.md) te mostrará como crear un nuevo juego Unity habilitado para Xbox Live, controlar el inicio de usuario de un único usuario y varios usuarios, añadir características tales como marcadores y estadísticas y generar un proyecto nativo de Visual Studio.

Aunque Unity es el único motor de juego de terceros para el que ofrecemos documentación, lo motores de juego [Construct (2 y 3)](https://www.scirra.com/construct2) y [Game Maker Studio](https://www.yoyogames.com/gamemaker) también tienen documentación para ayudarte a integrar Xbox Live en tu juego Construct o Game Maker Studio respectivamente.

* [Game Maker Studio 2 UWP ahora admite el Programa de creadores de Xbox Live](https://www.yoyogames.com/gamemaker/xblc) te mostrará como exportar tus proyectaros de Game Maker Studio para jugar en Xbox One y un equipo con Windows 10.
* [Usar Xbox Live en aplicaciones UWP: Construct](https://www.scirra.com/tutorials/9540/using-xbox-live-in-uwp-apps) te mostrará como usar Xbox Live en tus juegos Construct 2 y 3.

En el caso de otros motores de desarrollo de juegos sin integración Xbox Live documentada, puedes seguir usando las API de Xbox Live para añadir Xbox Live a tu título. Para usar la API de Xbox Live desde tu proyecto, puedes agregar referencias a los archivos binarios con paquetes de NuGet o agregar el origen de la API. Agregar paquetes de NuGet agiliza la compilación mientras que agregar el origen hace que la depuración sea más sencilla.

Para obtener soporte técnico para los servicios de Xbox Live con motores de juegos de terceros que no son Unity, ponte en contacto con el personal de motor de juego adecuado para que responda a tus preguntas.

## <a name="3-xbox-live-concepts--testing"></a>3. Conceptos de Xbox Live y pruebas
Una vez que tengas un título creado, debes conocer los conceptos de Xbox Live que afectarán a tu experiencia de desarrollo de títulos. También es importante que pruebes tu juego en todas las plataformas compatibles para asegurarte de que se comporta según lo esperado.

- [Configuración del servicio de Xbox Live para el programa de creadores](xbox-live-service-configuration-creators.md)
- [Entorno de prueba de Xbox Live](../xbox-live-sandboxes.md)
- [Autorizar a las cuentas de Xbox Live](authorize-xbox-live-accounts.md)

## <a name="4-enable-xbox-live-sign-in"></a>4. Habilitar la sesión Xbox Live
Todos los juegos del Programa de creadores de Xbox Live deben integrar el inicio de sesión de Xbox Live y mostrar la identidad del usuario (Gamertag, imagen de jugador, etc.). Puedes elegir iniciar sesión automáticamente en el usuario o que se necesario presionar un botón para iniciarlo. El Gamertag debe mostrarse una vez iniciada sesión para que el jugador pueda validar que está utilizando el perfil correcto.

- [Xbox Live plataforma Social - perfil, amigos, presencia](../social-platform/social-platform.md)

## <a name="5-add-optional-xbox-live-features"></a>5. Agregar características opcionales de Xbox Live

El Programa de creadores de Xbox Live ofrece un conjunto de características diseñadas para ayudar a promocionar tu juego y mantener a los jugadores interesados:

- [Plataforma de datos de Xbox Live: estadísticas, marcadores](../data-platform/data-platform.md) permite fomentar la participación en tu juego ya que permite a los jugadores competir para vencer a sus amigos y subir en la clasificación.
- [Plataforma de almacenamiento de Xbox Live: Almacenamiento conectado, Almacenamiento de títulos](../storage-platform/storage-platform.md) ofrece itinerancia de juegos guardada de forma gratuita entre dispositivos para que los jugadores puedan seguir fácilmente el progreso del juego entre Xbox One y un PC Windows.
- [Plataforma social de Xbox Live: perfil, amigos, presencia](../social-platform/social-platform.md) permite a los jugadores conectarse con amigos y hablar sobre el juego.

Es importante tener en cuenta que el Programa de creadores de Xbox Live no admite puntuación de jugador, logros o juego multijugador en línea.

## <a name="6-release-your-game"></a>6. Versión del juego

Si estás usando el Programa de creadores de Xbox Live, el proceso es similar al lanzamiento de cualquier otra aplicación para UWP:

- [Directivas de Microsoft Store](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx), incluidas [Directivas de juegos y Xbox](https://msdn.microsoft.com/en-us/library/windows/apps/dn764944.aspx#pol_10_13)
- [Publicar aplicaciones de Windows](https://developer.microsoft.com/en-us/store/publish-apps)