---
title: Iniciar recortes de pantalla
description: En este tema se describe los esquemas de URI ms-screenclip y ms-screensketch. La aplicación puede usar estos esquemas de URI para iniciar la aplicación de recorte & boceto o para abrir un recorte nuevo.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, uwp, uri, recorte, boceto
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 06e988387f574b74d511b14a2ebca24b0a149158
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595390"
---
# <a name="launch-screen-snipping"></a>Iniciar recortes de pantalla

El **ms screenclip:** y **screensketch de ms:** Esquemas de URI le permite iniciar recortes o capturas de pantalla de edición.

## <a name="open-a-new-snip-from-your-app"></a>Abra un recorte nuevo desde la aplicación

El **screenclip de ms:** URI permite que la aplicación para abrir e iniciar un recorte nuevo automáticamente. El recorte resultante se copia en el Portapapeles del usuario, pero no se pasa automáticamente a la aplicación de apertura.

**MS-screenclip:** toma los parámetros siguientes:

| Parámetro | Tipo | Requerido | Descripción |
| --- | --- | --- | --- |
| Código fuente | string | no | Una cadena de formato libre para indicar el origen que se inicia el URI. |
| delayInSeconds | entero | no | Un valor entero entre 1 y 30. Especifica el retardo, en segundos completos, entre la llamada URI y recortes cuando comienza. |
| callbackformat | string | no | Este parámetro no está disponible. |

## <a name="launching-the-snip--sketch-app"></a>Iniciar la aplicación de boceto y recorte

El **screensketch de ms:** URI le permite iniciar la aplicación de recorte & boceto mediante programación y abrir una imagen específica en esa aplicación para la anotación.

**MS-screensketch:** toma los parámetros siguientes:

| Parámetro | Tipo | Requerido | Descripción |
| --- | --- | --- | --- |
| sharedAccessToken | string | no | Un token que identifica el archivo para abrirlo en la aplicación de recorte & boceto. Recuperan [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Si se omite este parámetro, se iniciará la aplicación sin un archivo abierto. |
| secondarySharedAccessToken | string | no | Cadena que identifica un archivo JSON con metadatos sobre el recorte. Los metadatos pueden incluir un **clipPoints** campo con una matriz de coordenadas x, y, o un [userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity). |
| Código fuente | string | no | Una cadena de formato libre para indicar el origen que se inicia el URI. |
| IsTemporary | bool | no | Si se establece en True, el boceto de pantalla intentará eliminar el archivo después de abrirla. |

El ejemplo siguiente se llama el [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) método para enviar una imagen al recorte & boceto desde la aplicación del usuario.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

El ejemplo siguiente muestra lo que un archivo especificado por el **secondarySharedAccessToken** parámetro de **ms screensketch** podría contener:

```json
{
  "clipPoints": [
    {
      "x": 0,
      "y": 0
    },
    {
      "x": 2080,
      "y": 0
    },
    {
      "x": 2080,
      "y": 780
    },
    {
      "x": 0,
      "y": 780
    }
  ],
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/en-us/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
