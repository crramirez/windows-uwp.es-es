---
author: QuinnRadich
title: Iniciar recortes de pantalla
description: En este tema se describe los esquemas de URI ms-screenclip y ms-screensketch. La aplicación puede usar estos esquemas de URI para iniciar la aplicación de recorte & boceto o abrir un recorte nuevo.
ms.author: quradic
ms.date: 8/1/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, uri, recorte, boceto
ms.localizationpriority: medium
ms.openlocfilehash: e18662125ef72051a289b3f1d0f3dc09b452d256
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "4530997"
---
# <a name="launch-screen-snipping"></a>Iniciar recortes de pantalla

El **ms screenclip:** y **ms screensketch:** esquemas de URI te permite iniciar recortes o capturas de pantalla de edición.

## <a name="open-a-new-snip-from-your-app"></a>Abre un nuevo recorte desde la aplicación

El **ms screenclip:** URI permite que la aplicación automáticamente abrir e iniciar un nuevo recorte. El recorte resultante se copia en el Portapapeles del usuario, pero no se pasa automáticamente a la aplicación de apertura.

**ms screenclip:** toma los siguientes parámetros:

| Parámetro | Tipo | Obligatorio | Descripción |
| --- | --- | --- | --- |
| origen | string | no | Una cadena de forma libre para indicar el origen que inició el URI. |
| delayInSeconds | entero | no | Un valor entero de 1 a 30. Especifica el retraso en segundos completas, entre la llamada URI y cuando comienza la recortes. |

## <a name="launching-the-snip--sketch-app"></a>Iniciar el recorte y la aplicación boceto

El **ms screensketch:** URI te permite iniciar la aplicación de recorte & boceto mediante programación y abrir una imagen específica en esa aplicación para la anotación.

**ms screensketch:** toma los siguientes parámetros:

| Parámetro | Tipo | Obligatorio | Descripción |
| --- | --- | --- | --- |
| sharedAccessToken | string | no | Un token que identifica el archivo para abrirlo en la aplicación de recorte & boceto. Recupera [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Si se omite este parámetro, se iniciará la aplicación sin un archivo abierto. |
| origen | string | no | Una cadena de forma libre para indicar el origen que inició el URI. |
| isTemporary | bool | no | Si se establece en True, bocetos de pantalla intentará eliminar el archivo después de abrirla. |

En el ejemplo siguiente, se llama al método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) para enviar una imagen a recorte & boceto desde la aplicación de usuario.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```