---
author: Mtoepke
title: Recursos del sistema de aplicaciones para UWP y juegos en Xbox One
description: Recursos del sistema de UWP en Xbox
ms.author: mstahl
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: f4c96fd3c124f8853a91f8db4385f7d42e254834
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6209169"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Recursos del sistema de aplicaciones para UWP y juegos en Xbox One

Las aplicaciones para UWP que se ejecutan en Xbox One comparten recursos con el sistema y otras aplicaciones. Los recursos disponibles para una aplicación para UWP en Xbox One dependen de si envías como una aplicación o como un juego de Programa de creadores de Xbox Live.

* Memoria máxima disponible mientras se ejecuta en primer plano:
    * Aplicaciones: 1 GB
    * Juegos: 5 GB

La memoria máxima disponible para una aplicación que se ejecute en segundo plano es de 128MB. El modo en segundo plano solo se aplica a aplicaciones simultáneas, como reproductores de música en segundo plano.  Los juegos se suspenderán y terminarán en segundo plano.

Superar estas limitaciones provocará fallos en la asignación de memoria. Para obtener más información acerca de la supervisión de uso de memoria, consulta la referencia de la clase [MemoryManager](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx).
    
    > [!NOTE]
    > When running your app or game from the Visual Studio debugger, these memory constraints do not apply. This limit is only applicable when not running in debugging mode.

* CPU
    * Aplicaciones: comparte de 2 a 4 núcleos de CPU según el número de aplicaciones y juegos que se ejecutan en el sistema.
    * Juegos: 4 núcleos exclusivos y 2 de CPU compartidos.

* GPU
    * Aplicaciones: comparte el 45% de GPU según el número de aplicaciones y juegos que se ejecutan en el sistema.
    * Juegos: acceso total a ciclos de GPU disponibles.

* Compatibilidad con DirectX
    * Aplicaciones: nivel de características 10 de DirectX 11 Feature.
    * Juegos: nivel de características 10 de DirectX 12 y DirectX 11.

* Todas las aplicaciones y juegos deben tener como destino la arquitectura x64 para su desarrollo o su envío a la Store para Xbox.  

Para **desarrollo de aplicaciones**, los recursos disponibles pueden ser limitados en comparación con un PC estándar y puede variar según el número de aplicaciones y juegos que se ejecuten en el sistema.

Para el **desarrollo de juegos**, Xbox One, al igual que otras videoconsolas, es una parte especializada de hardware que requiere un kit de desarrollo específico basado en hardware para tener acceso al potencial completo. Si estás trabajando en un juego que requiere acceso al máximo potencial del hardware Xbox One, considere registrar con el programa [ID@Xbox](http://www.xbox.com/Developers/id) para acceder a los kits de desarrollo de Xbox One.


Para obtener más información acerca de los recursos del sistema para aplicaciones para UWP en Xbox One, consulta la primera parte de este vídeo.
</br>
</br>
<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developing-xbox-one-applications-16860/Video-What-s-Unique--vk0fOPf9C_2006218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="see-also"></a>Ver también
- [UWP en Xbox One](index.md)
- [Introducción al Programa de creadores de Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)
- [DirectX y UWP en Xbox One](https://blogs.msdn.microsoft.com/chuckw/2017/12/15/directx-and-uwp-on-xbox-one/)

