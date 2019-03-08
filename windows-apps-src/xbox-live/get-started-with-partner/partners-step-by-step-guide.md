---
title: Guía paso a paso para partners de Xbox Live
description: Proporciona una directriz de pasos para integrar Xbox Live para socios administrados.
ms.assetid: f0c9db8f-f492-4955-8622-e1736f0a4509
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9a2d0b9e7d97adfb02281e7bae34ed51afd44f7f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660850"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-for-managed-partners-and-idxbox-members"></a>Guía paso a paso para integrar Xbox Live para socios administrados y ID@Xbox miembros

En esta sección le ayudarán a ponerse en marcha con Xbox Live:

## <a name="1-choose-a-platform"></a>1. Elija una plataforma
Decidir entre un Kit de desarrollo de Xbox (XDK), plataforma Universal de Windows (UWP) o juego entre-play.

- XDK en función de juegos que solo se ejecutan en la consola Xbox One
- Juegos UWP pueden tener como destino cualquier plataforma de Windows como PC de Windows, Windows Phone o Xbox One
  - Para Xbox One, consulte [UWP en Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/index) y específicamente [recursos del sistema para aplicaciones de UWP y juegos en Xbox One](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/system-resource-allocation)
- Entre jugar a juegos suelen ser juegos destinados a Xbox One y PC Windows con las rutas de acceso el XDK y UWP.

## <a name="2-ensure-you-have-a-title-created-in-partner-center-or-xdp"></a>2. Asegúrese de que tiene un título que se creó en el centro de partners o XDP
Cada título de Xbox Live debe definirse en el centro de partners o el Portal para desarrolladores de Xbox (XDP) antes de poder iniciar sesión y realizar llamadas de servicio de Xbox Live.  [Crear un nuevo título](create-a-new-title.md) le mostrará cómo hacerlo.

## <a name="3-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>3. Siga la guía correspondiente para configurar su motor de juego o IDE
Puede seguir a la Guía de introducción adecuada para su plataforma y el motor de y conozca los aspectos básicos de Xbox Live conforme vaya avanzando.

* [Introducción al uso de Visual Studio para juegos UWP](get-started-with-visual-studio-and-uwp.md) le mostrará cómo vincular el proyecto de Visual Studio con la configuración de Xbox Live en el centro de partners.

* [Introducción al uso de Unity para juegos UWP](partner-add-xbox-live-to-unity-uwp.md) mostrará cómo crear un nuevo Xbox Live habilitada la opción de título de Unity, agregue características como marcadores en el título y generar un proyecto nativo de Visual Studio.

* [Introducción al uso de Visual Studio para XDK juegos basados](xdk-developers.md) le mostrará cómo obtener la configuración de proyecto de Visual Studio si está realizando un Xbox One título con el XDK.

* [Al obtener iniciada diseñar un juego de entre-play](get-started-with-cross-play-games.md) explica cómo lograr que un producto es un juego XDK en función de Xbox One y un basadas en UWP juegos para PC de Windows 10.

## <a name="4-xbox-live-concepts"></a>4. Conceptos de Xbox Live
Una vez que tengas un título creado, debes conocer los conceptos de Xbox Live que afectarán a tu experiencia de desarrollo de títulos.

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

ID@Xbox títulos y liberado por el fabricante del juego es un Partner de Microsoft deben pasar por un proceso de certificación antes del lanzamiento.  Esto es porque estos títulos de tengan acceso a características adicionales como participan varios jugadores, logros y presencia enriquecida.  Cuando esté listo para liberar su título, póngase en contacto con su representante de Microsoft para obtener más detalles sobre el proceso de envío y la versión.
