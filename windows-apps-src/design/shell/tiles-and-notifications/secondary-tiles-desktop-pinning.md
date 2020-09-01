---
Description: Las aplicaciones de escritorio de Windows pueden anclar iconos secundarios gracias al puente de escritorio.
title: Anclar iconos secundarios desde la aplicación de escritorio
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, puente de escritorio, iconos secundarios, PIN, anclaje, Inicio rápido, ejemplo de código, ejemplo, secondarytile, aplicación de escritorio, Win32, WinForms, WPF
ms.localizationpriority: medium
ms.openlocfilehash: 111d66e69ddb9cff56f36a26bd8094429fe808ef
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172379"
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>Anclar iconos secundarios desde la aplicación de escritorio


Gracias al [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), las aplicaciones de escritorio de Windows (como Win32, Windows Forms y WPF) pueden anclar iconos secundarios.

![Captura de pantalla de iconos secundarios](images/secondarytiles.png)

> [!IMPORTANT]
> **Requiere Fall Creators Update**: debe tener como destino el SDK 16299 y ejecutar la compilación 16299 o posterior para anclar los iconos secundarios de las aplicaciones de puente de escritorio.

Agregar un icono secundario desde la aplicación de WPF o WinForms es muy similar a una aplicación UWP pura. La única diferencia es que debe especificar el identificador de ventana principal (HWND). Esto se debe a que al anclar un icono, Windows muestra un cuadro de diálogo modal que pide al usuario que confirme si desea anclar el icono. Si la aplicación de escritorio no configura el objeto SecondaryTile con la ventana propietaria, Windows no sabrá dónde dibujar el cuadro de diálogo y se producirá un error en la operación.


## <a name="package-your-app-with-desktop-bridge"></a>Empaquetar la aplicación con el puente de escritorio

Si no ha empaquetado la aplicación con el puente de escritorio, [primero debe hacerlo](/windows/msix/desktop/source-code-overview) antes de poder usar las api de Windows Runtime.


## <a name="enable-access-to-iinitializewithwindow-interface"></a>Habilitar el acceso a la interfaz IInitializeWithWindow

Si la aplicación está escrita en un lenguaje administrado como C# o Visual Basic, declare la interfaz IInitializeWithWindow en el código de la aplicación con el atributo [ComImport](/dotnet/api/system.runtime.interopservices.comimportattribute) y GUID, tal como se muestra en el siguiente ejemplo de C#. En este ejemplo se da por hecho que el archivo de código tiene una instrucción using para el espacio de nombres System.Runtime.InteropServices.

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

Como alternativa, si usa C++, agregue una referencia al archivo de encabezado **shobjidl. h** en el código. Este archivo de encabezado contiene la declaración de la interfaz *IInitializeWithWindow*.


## <a name="initialize-the-secondary-tile"></a>Inicializar el icono secundario

Inicialice un nuevo objeto de icono secundario exactamente igual que haría con una aplicación de UWP normal. Para obtener más información sobre la creación y el anclaje de iconos secundarios, consulte [anclar iconos secundarios](secondary-tiles-pinning.md).

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>Asignar el identificador de ventana

Este es el paso clave para las aplicaciones de escritorio. Convierta el objeto en un objeto [IInitializeWithWindow](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow) . A continuación, llame al método [IInitializeWithWindow.Initialize](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize) y pase el identificador de la ventana que desee que sea el propietario del cuadro de diálogo modal. En el siguiente ejemplo de C# se muestra cómo pasar el identificador de la ventana principal de la aplicación al método.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>Anclar el icono

Por último, solicite el anclaje del icono como lo haría con una aplicación de UWP normal.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>Enviar notificaciones de icono

> [!IMPORTANT]
> **Requiere la versión 17134,81 de abril de 2018 o posterior**: debe ejecutar la compilación 17134,81 o posterior para enviar notificaciones de icono o de distintivo a los mosaicos secundarios desde las aplicaciones de puente de escritorio. Antes de esta actualización de servicio. 81, se produciría una excepción 0x80070490 *elemento no encontrado* al enviar notificaciones de icono o distintivo a mosaicos secundarios desde aplicaciones de puente de escritorio.

El envío de notificaciones de icono o de distintivo es igual que las aplicaciones de UWP. Consulte [envío de una notificación de icono local](sending-a-local-tile-notification.md) para comenzar.


## <a name="resources"></a>Recursos

* [Ejemplo de código completo](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Información general sobre mosaicos secundarios](secondary-tiles.md)
* [Anclar iconos secundarios (UWP)](secondary-tiles-pinning.md)
* [Puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop)
* [Ejemplos de código de puente de escritorio](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)