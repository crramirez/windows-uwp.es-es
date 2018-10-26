---
author: vladimp
Description: Windows desktop applications can pin secondary tiles thanks to the Desktop Bridge!
title: Anclar iconos secundarios desde una aplicación de escritorio
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.author: mijacobs
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, puente de dispositivo de escritorio, iconos secundarios, anclar, anclado, inicio rápido, muestra de código, ejemplo, secondarytile, aplicación de escritorio, win32, winforms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 44e37b47e22d10f509afd5d7503fa8f7a43ab365
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "5546468"
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>Anclar iconos secundarios desde una aplicación de escritorio


Gracias al [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop), las aplicaciones de escritorio de Windows (como Win32, Windows Forms y WPF) pueden anclar iconos secundarios.

![Captura de pantalla de iconos secundarios](images/secondarytiles.png)

> [!IMPORTANT]
> **Requiere la actualización de otoño Creators Update**: debes utilizar SDK 16299 y estar ejecutando la compilación 16299 o posterior para anclar iconos secundarios desde las aplicaciones de Puente de dispositivo de escritorio.

Agregar un icono secundario desde tu aplicación WPF o WinForms es muy similar a una aplicación para UWP pura. La única diferencia es que debes especificar el identificador de la ventana principal (HWND). Esto es porque al anclar un icono, Windows muestra un cuadro de diálogo modal que pide al usuario que confirme si quiere anclar el icono. Si la aplicación de escritorio no configura el objeto SecondaryTile con la ventana de propietario, Windows no sabe dónde se debe dibujar el cuadro de diálogo y se producirá un error en la operación.


## <a name="package-your-app-with-desktop-bridge"></a>Agregar tu aplicación con Puente de dispositivo de escritorio

Si no has empaquetado tu aplicación con el Puente de dispositivo de escritorio, [debes hacerlo primero](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) para poder usar las API de UWP.


## <a name="enable-access-to-iinitializewithwindow-interface"></a>Habilitar el acceso a la interfaz IInitializeWithWindow

Si tu aplicación está escrita en un lenguaje administrado, como C# o Visual Basic, declara la interfaz IInitializeWithWindow en el código de la aplicación con el atributo [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) y Guid, como se muestra en el siguiente ejemplo de C#. En este ejemplo se da por hecho que el archivo de código tiene una instrucción using para el espacio de nombres System.Runtime.InteropServices.

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

Alternativamente, si estás usando C++, agrega una referencia al archivo de encabezado **shobjidl.h** en el código. Este archivo de encabezado contiene la declaración de la interfaz *IInitializeWithWindow*.


## <a name="initialize-the-secondary-tile"></a>Inicializar el icono secundario

Inicializa un nuevo objeto de icono secundario exactamente como lo harías con una aplicación para UWP normal. Para obtener más información sobre la creación y anclaje de iconos secundarios, consulta [Anclar iconos secundarios](secondary-tiles-pinning.md).

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

Este es el paso clave para las aplicaciones de escritorio. Convertir el objeto en un objeto [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx). Después, llama al método [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) y pasa el identificador de la ventana que quieras que sea la propietaria del cuadro de diálogo modal. El siguiente ejemplo de C# muestra cómo pasar el identificador de la ventana principal de la aplicación al método.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>Anclar el icono

Por último, solicita anclar el icono como lo harías con una aplicación para UWP normal.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>Enviar notificaciones de iconos

> [!IMPORTANT]
> **Requiere la actualización de abril de 2018, versión 17134.81 o posterior**: debes ejecutar la versión 17134.81 o posterior para enviar notificaciones de icono o distintivo a ventanas secundarias desde aplicaciones del Puente de dispositivo de escritorio. Antes de esta actualización de mantenimiento .81, se producía una excepción 0x80070490 *Elemento no encontrado* al enviar notificaciones de icono o distintivo a iconos secundarios desde las aplicaciones del Puente de dispositivo de escritorio.

Enviar notificaciones de iconos o distintivos es los mismo que las aplicaciones para UWP. Consulta [Enviar una notificación de icono local](sending-a-local-tile-notification.md) para empezar.


## <a name="resources"></a>Recursos

* [Muestra de código completo](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Información general sobre iconos secundarios](secondary-tiles.md)
* [Anclar iconos secundarios (UWP)](secondary-tiles-pinning.md)
* [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop)
* [Muestras de código del Puente de dispositivo de escritorio](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)