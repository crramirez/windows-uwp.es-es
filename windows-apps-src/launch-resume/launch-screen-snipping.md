---
title: Iniciar recortes de pantalla
description: En este tema se describe los esquemas de URI ms-screenclip y ms-screensketch. La aplicación puede usar estos esquemas de URI para iniciar la aplicación de recorte & boceto o abrir un recorte nuevo.
ms.date: 8/1/2017
ms.topic: article
keywords: Windows 10, uwp, uri, recorte, boceto
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a90772e01885a7361cd51b54fc6e5ea9930bfbd
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8920564"
---
# <a name="launch-screen-snipping"></a>Iniciar recortes de pantalla

El **ms-screenclip:** y **ms-screensketch:** esquemas de URI te permite iniciar recortes o capturas de pantalla de edición.

## <a name="open-a-new-snip-from-your-app"></a>Abre un nuevo recorte desde la aplicación

El **ms-screenclip:** URI permite que la aplicación automáticamente abrir e iniciar un nuevo recorte. El recorte resultante se copia en el Portapapeles del usuario, pero no se pasa automáticamente a la aplicación de apertura.

**ms-screenclip:** toma los siguientes parámetros:

| Parámetro | Tipo | Obligatorio | Descripción |
| --- | --- | --- | --- |
| origen | string | no | Una cadena de forma libre para indicar el origen que inició el URI. |
| delayInSeconds | entero | no | Un valor entero de 1 a 30. Especifica el retraso en segundos completas, entre la llamada URI y cuando comienza la recortes. |

## <a name="launching-the-snip--sketch-app"></a>Iniciar el recorte y aplicaciones de bocetos

El **ms-screensketch:** URI te permite iniciar la aplicación de recorte & boceto mediante programación y abrir una imagen específica en esa aplicación para la anotación.

**ms-screensketch:** toma los siguientes parámetros:

| Parámetro | Tipo | Obligatorio | Descripción |
| --- | --- | --- | --- |
| sharedAccessToken | string | no | Un token que identifica el archivo para abrirlo en la aplicación de recorte & boceto. Recupera [SharedStorageAccessManager.AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Si se omite este parámetro, se iniciará la aplicación sin un archivo abierto. |
| secondarySharedAccessToken | string | no | Una cadena que identifica un archivo JSON con metadatos sobre el recorte. Los metadatos pueden incluir un campo de **clipPoints** con una matriz de coordenadas x e y o un [userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity). |
| origen | string | no | Una cadena de forma libre para indicar el origen que inició el URI. |
| isTemporary | bool | no | Si se establece en True, bocetos de pantalla intentará eliminar el archivo después de abrirla. |

En el ejemplo siguiente, se llama al método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) para enviar una imagen a recorte & boceto desde la aplicación de usuario.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

El siguiente ejemplo muestra lo que podría contener un archivo especificado por el parámetro **secondaryFileAccessToken** de **ms-captura de pantalla** :

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
