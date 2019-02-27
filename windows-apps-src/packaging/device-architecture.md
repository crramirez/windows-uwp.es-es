---
title: Arquitecturas de paquetes de aplicaciones
description: Obtén más información sobre qué arquitecturas de procesador debes usar al compilar el paquete de aplicación para UWP.
ms.date: 07/13/2017
ms.topic: article
keywords: windows 10, uwp, empaquetado, arquitectura, configuración de paquete
ms.localizationpriority: medium
ms.openlocfilehash: 338dac1d43e08257fa00b51c0c311a090f3d95c0
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116114"
---
# <a name="app-package-architectures"></a>Arquitecturas de paquetes de aplicaciones

Los paquetes de la aplicación se configuran para ejecutarse en una arquitectura de procesador específica. Al seleccionar una arquitectura, especifica en qué dispositivos quieres que se ejecute la aplicación. Las aplicaciones de la Plataforma universal de Windows (UWP) se pueden configurar para ejecutarse en las siguientes arquitecturas:
- x86
- x64
- ARM
- ARM64

Es **muy** recomendable que generes el paquete de la aplicación para todas las arquitecturas. Al anular la selección de una arquitectura de dispositivo, limitas el número de dispositivos en los que se puede ejecutar tu aplicación que, a su vez, ¡limitará la cantidad de personas que pueden usar la aplicación!

## <a name="windows-10-devices-and-architectures"></a>Dispositivos con Windows 10 y arquitecturas

> [!div class="mx-tableFixed"]
| Arquitectura de UWP | Escritorio (x86)      | Escritorio (x64)      | Escritorio (ARM)      | Móvil             | HoloLens y Windows Mixed Reality           | Xbox               | IoT Core (en función del dispositivo) | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | :x:                | :heavy_check_mark: | :x:                | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM y ARM64              | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :x:                | :x:                | :heavy_check_mark:          | :x:                |


Hablemos de estas arquitecturas con más detalle.

### <a name="x86"></a>x86
Elegir x86 suele ser la configuración más segura para un paquete de la aplicación, dado que se ejecutará en casi todos los dispositivos. En algunos dispositivos, no se ejecutará un paquete de la aplicación con la configuración x86, como Xbox o algunos dispositivos con IoT Core. Sin embargo, para un equipo, un paquete x86 es la opción más segura y tiene el mayor alcance para la implementación de dispositivos. Una parte importante de dispositivos con Windows 10 seguirá ejecutando la versión x86 de Windows.

### <a name="x64"></a>x64
Esta configuración se usa con menos frecuencia que la configuración x86. Debe tenerse en cuenta que esta configuración está reservada para escritorios con las versiones de 64 bits de Windows 10, [aplicaciones para UWP en Xbox](https://docs.microsoft.com/windows/uwp/xbox-apps/system-resource-allocation) y Windows 10 IoT Core en Intel Joule.

### <a name="arm-and-arm64"></a>ARM y ARM64
La configuración de Windows 10 en ARM incluye equipos de escritorio, dispositivos móviles y algunos dispositivos con IoT Core (Rasperry Pi 2, Raspberry Pi 3 y DragonBoard). Los equipos de escritorio con Windows 10 en ARM son una novedad de la familia de Windows, por lo que si eres desarrollador de aplicaciones para UWP, debes enviar los paquetes de ARM a la Tienda para obtener la mejor experiencia posible en estos equipos.

>[!NOTE]
> Para compilar la aplicación para UWP de destino de forma nativa la plataforma de ARM64, debe tener Visual Studio 2017 versión 15,9 o posterior. Para obtener más información, consulta [esta entrada de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).

Para obtener más información, consulta [Windows 10 en ARM](../porting/apps-on-arm.md). Consulta esta charla de //Build para ver una demostración de [Windows 10 en ARM](https://channel9.msdn.com/Events/Build/2017/P4171) y obtener más información acerca de cómo funciona.

Para obtener más información sobre temas específicos de IoT, consulta [implementar una aplicación con Visual Studio](https://developer.microsoft.com/windows/iot/Docs/AppDeployment).
