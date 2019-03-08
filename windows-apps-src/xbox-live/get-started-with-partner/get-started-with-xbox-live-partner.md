---
title: Introducción a Xbox Live como un asociado
description: Proporciona vínculos para ayudar a un socio administrado o ID@Xbox miembro empezar a trabajar con el desarrollo de Xbox Live.
ms.assetid: 69ab75d1-c0e7-4bad-bf8f-5df5cfce13d5
ms.date: 06/07/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, asociado, ID@Xbox
ms.localizationpriority: medium
ms.openlocfilehash: 1a219ee6f4e1dc80ead893c4e230d70e8caa2400
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659830"
---
# <a name="get-started-with-xbox-live-as-a-managed-partner-or-an-idxbox-developer"></a>Introducción a Xbox Live como un socio administrado o un ID@Xbox para desarrolladores

Esta sección describen cómo empezar a Xbox Live como un socio administrado o como un ID@Xbox para desarrolladores. Asociados y desarrolladores de identificador pueden tener acceso a las características completa gama de Xbox Live, incluidos los logros, participan varios jugadores y mucho más.

Administra los asociados y ID@Xbox los desarrolladores pueden desarrollar los títulos de Xbox Live para la plataforma Universal de Windows (UWP) o la plataforma Xbox Developer Kit (XDK).

Además del contenido disponible en este caso, también hay documentación adicional que está disponible para asociados con una cuenta autorizada del centro de partners. Puede tener acceso a ese contenido aquí: [Contenido del asociado de Xbox Live](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/partner-content).

## <a name="why-should-you-use-xbox-live"></a>¿Por qué debería usar Xbox Live?

Xbox Live ofrece una variedad de características diseñadas para ayudar a promover su juego y mantener los jugadores implicados:

- Xbox Live plataforma social permite jugadores conectarse con amigos y hablar sobre su juego.
- Logros de Xbox Live le ayuda a su juego populares proporcionando su juego promoción gratuita al gráfico social Xbox Live cuando jugadores desbloquean logros.
- Marcadores de Xbox Live le ayuda a controlar la participación de su juego al permitir que los jugadores competir para derrotar a sus amigos y subir los rangos.
- Xbox Live multijugador permite jugadores jugar con sus amigos o una operación get coincide con otros jugadores que compiten o cooperan en el juego.
- Xbox Live conectado ofertas de almacenamiento libre guardar juegos móviles entre dispositivos para que jugadores pueden seguir fácilmente juegos progreso entre Xbox One y PC Windows.

## <a name="1-choose-a-platform"></a>1. Elija una plataforma
Decidir entre un Kit de desarrollo de Xbox (XDK), plataforma Universal de Windows (UWP) o juego entre play:

- XDK en función de juegos que solo se ejecutan en la consola Xbox One
- Juegos UWP pueden tener como destino cualquier plataforma de Windows como PC de Windows, Windows Phone o Xbox One
  - Para Xbox One, consulte [UWP en Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) y específicamente [recursos del sistema para aplicaciones de UWP y juegos en Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- Entre jugar a juegos suelen ser juegos destinados a Xbox One y PC Windows con las rutas de acceso el XDK y UWP.

## <a name="2-ensure-that-you-have-a-title-created-in-partner-center-or-xdp"></a>2. Asegúrese de que tiene un título que se creó en el centro de partners o XDP
Cada título de Xbox Live debe definirse en el centro de partners o el Portal para desarrolladores de Xbox (XDP) antes de poder iniciar sesión y realizar llamadas de servicio de Xbox Live.  [Crear un nuevo título](create-a-new-title.md) le mostrará cómo hacerlo.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. Siga la guía correspondiente para configurar su motor de juego o IDE
Puede seguir a la Guía de introducción adecuada para su plataforma y el motor de y conozca los aspectos básicos de Xbox Live conforme vaya avanzando.

* [Introducción al uso de Visual Studio para juegos UWP](get-started-with-visual-studio-and-uwp.md) le mostrará cómo vincular el proyecto de Visual Studio con la configuración de Xbox Live en el centro de partners.
* [Introducción al uso de Unity para juegos UWP](partner-add-xbox-live-to-unity-uwp.md) mostrará cómo crear un nuevo Xbox Live habilitada la opción de título de Unity, agregue características como marcadores en el título y generar un proyecto nativo de Visual Studio.
* [Introducción al uso de Visual Studio para XDK juegos basados](xdk-developers.md) le mostrará cómo obtener la configuración de proyecto de Visual Studio si está realizando un Xbox One título con el XDK.
* [Al obtener iniciada diseñar un juego de entre-play](get-started-with-cross-play-games.md) explica cómo lograr que un producto es un juego XDK en función de Xbox One y un basadas en UWP juegos para PC de Windows 10.

## <a name="4-xbox-live-concepts"></a>4. Conceptos de Xbox Live
Una vez que tenga un título que se creó, debe obtener información sobre los conceptos de Xbox Live que afectan a su experiencia de desarrollo de títulos:

- [Los espacios aislados de Xbox Live](../xbox-live-sandboxes.md)
- [Cuentas de prueba de Xbox Live](../xbox-live-test-accounts.md)
- [Introducción a las API de Xbox Live](../introduction-to-xbox-live-apis.md)

## <a name="5-add-xbox-live-features"></a>5. Agregar características de Xbox Live

Agregar características de Xbox Live a su juego:

- [Xbox Live plataforma Social - perfil, amigos, presencia](../social-platform/social-platform.md)
- [Xbox Live datos plataforma: estadísticas, marcadores, logros](../data-platform/data-platform.md)
- [Xbox Live plataforma multijugador - torneos de invitación, contactos,](../multiplayer/multiplayer-intro.md)
- [Plataforma de almacenamiento en vivo de Xbox - conectado almacenamiento título](../storage-platform/storage-platform.md)
- [Búsqueda contextual](../contextual-search/introduction-to-contextual-search.md)

## <a name="6-release-your-title"></a>6. Versión del título

ID@Xbox y socios administrados deben pasar por un proceso de certificación antes de liberar sus títulos.  Esto es porque los títulos de tengan acceso a características adicionales, como varios jugadores en línea, logros y presencia enriquecida.  Cuando esté listo para liberar su título, póngase en contacto con su representante de Microsoft para obtener más detalles sobre el proceso de envío y de lanzamiento.
