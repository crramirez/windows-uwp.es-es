---
title: Iniciar el recorte de pantalla
description: En este tema se describen los esquemas de URI MS-screenclip y MS-screensketch. La aplicación puede usar estos esquemas de URI para iniciar el recorte & aplicación de boceto o para abrir un nuevo recorte.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP, Uri, recorte, boceto
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d9469dd6efd3598ab7abd9791a976385f4dfce49
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684664"
---
# <a name="launch-screen-snipping"></a>Iniciar el recorte de pantalla

Los esquemas **MS-screenclip:** y **MS-screensketch:** URI permiten iniciar recortes o editar capturas de pantallas.

## <a name="open-a-new-snip-from-your-app"></a>Abrir un nuevo recorte de la aplicación

El URI **MS-screenclip:** permite que la aplicación se abra automáticamente e inicie un nuevo recorte. El recorte resultante se copia en el Portapapeles del usuario, pero no se vuelve a pasar automáticamente a la aplicación de apertura.

**MS-screenclip:** toma los siguientes parámetros:

| Parámetro | Escribe | Necesario | Descripción |
| --- | --- | --- | --- |
| origen | cadena | no | Una cadena de forma libre para indicar el origen que inició el URI. |
| delayInSeconds | entero | no | Un valor entero, de 1 a 30. Especifica el retraso, en segundos completos, entre la llamada del URI y el momento en que se inician los recortes. |
| callbackformat | cadena | no | Este parámetro no está disponible. |

## <a name="launching-the-snip--sketch-app"></a>Inicio del recorte & aplicación de boceto

El URI **MS-screensketch:** permite iniciar mediante programación el recorte & aplicación de boceto y abrir una imagen específica en esa aplicación para la anotación.

**MS-screensketch:** toma los siguientes parámetros:

| Parámetro | Escribe | Necesario | Descripción |
| --- | --- | --- | --- |
| sharedAccessToken | cadena | no | Token que identifica el archivo que se va a abrir en el recorte & aplicación de boceto. Recuperado de [SharedStorageAccessManager. AddFile](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.sharedstorageaccessmanager.addfile). Si se omite este parámetro, la aplicación se iniciará sin abrir un archivo. |
| secondarySharedAccessToken | cadena | no | Una cadena que identifica un archivo JSON con metadatos sobre el recorte. Los metadatos pueden incluir un campo **clipPoints** con una matriz de coordenadas x, y y/o un [userActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity). |
| origen | cadena | no | Una cadena de forma libre para indicar el origen que inició el URI. |
| isTemporary | bool | no | Si se establece en true, el boceto de pantalla intentará eliminar el archivo después de abrirlo. |

En el ejemplo siguiente se llama al método [LaunchUriAsync](https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_) para enviar una imagen para recortar & boceto de la aplicación del usuario.

```csharp

bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-screensketch:edit?source=MyApp&isTemporary=false&sharedAccessToken=2C37ADDA-B054-40B5-8B38-11CED1E1A2D"));

```

En el ejemplo siguiente se muestra lo que un archivo especificado por el parámetro **secondarySharedAccessToken** de **MS-screensketch** podría contener:

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
  "userActivity": "{\"$schema\":\"http://activity.windows.com/user-activity.json\",\"UserActivity\":\"type\",\"1.0\":\"version\",\"cross-platform-identifiers\":[{\"platform\":\"windows_universal\",\"application\":\"Microsoft.MicrosoftEdge_8wekyb3d8bbwe!MicrosoftEdge\"},{\"platform\":\"host\",\"application\":\"edge.activity.windows.com\"}],\"activationUrl\":\"microsoft-edge:https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"contentUrl\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"visualElements\":{\"attribution\":{\"iconUrl\":\"https://www.microsoft.com/favicon.ico?v2\",\"alternateText\":\"microsoft.com\"},\"description\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"backgroundColor\":\"#FF0078D7\",\"displayText\":\"Use snipping tool to capture screenshots - Windows Help\",\"content\":{\"$schema\":\"http://adaptivecards.io/schemas/adaptive-card.json\",\"type\":\"AdaptiveCard\",\"version\":\"1.0\",\"body\":[{\"type\":\"Container\",\"items\":[{\"type\":\"TextBlock\",\"text\":\"Use snipping tool to capture screenshots - Windows Help\",\"weight\":\"bolder\",\"size\":\"large\",\"wrap\":true,\"maxLines\":3},{\"type\":\"TextBlock\",\"text\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\",\"size\":\"normal\",\"wrap\":true,\"maxLines\":3}]}]}},\"isRoamable\":true,\"appActivityId\":\"https://support.microsoft.com/help/13776/windows-use-snipping-tool-to-capture-screenshots\"}"
}

```
