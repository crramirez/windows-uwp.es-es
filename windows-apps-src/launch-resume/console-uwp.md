---
title: Crear una aplicación de consola de Plataforma universal de Windows
description: En este tema se describe cómo escribir una aplicación para UWP que se ejecuta en una ventana de consola.
keywords: UWP de la consola
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cab52a84e631404fc4cbd682d6cedef7e799d387
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162669"
---
# <a name="create-a-universal-windows-platform-console-app"></a>Crear una aplicación de consola de Plataforma universal de Windows

En este tema se describe cómo crear una aplicación de consola de [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) o c++/CX plataforma universal de Windows (UWP).

A partir de Windows 10, versión 1803, puede escribir C++/WinRT o C++/CX UWP apps que se ejecutan en una ventana de consola, como una ventana de consola de DOS o de PowerShell. Las aplicaciones de consola usan la ventana de la consola para la entrada y salida, y pueden usar funciones de [tiempo de ejecución universal C](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference) como **printf** y **getchar**. Las aplicaciones de consola de UWP se pueden publicar en el Microsoft Store. Tienen una entrada en la lista de aplicaciones y un icono principal que se puede anclar al menú Inicio. Las aplicaciones de consola de UWP se pueden iniciar desde el menú Inicio, aunque normalmente se inician desde la línea de comandos.

Para ver una acción, aquí se muestra un vídeo sobre la creación de una aplicación de consola de UWP.

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>Usar una plantilla de aplicación de consola de UWP 

Para crear una aplicación de consola de UWP, instale primero las **plantillas de proyecto de aplicación de consola (universal)**, disponibles en la [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal). Las plantillas instaladas están disponibles en **nuevo proyecto**  >  **instalar**  >  **otros lenguajes**  >  **Visual C++**  >  **Windows universal** como **aplicación de consola de c++/WinRT (Windows universal)** y la **aplicación de consola c++/CX (Windows universal)**.

## <a name="add-your-code-to-main"></a>Agregue el código a Main ()

Las plantillas agregan **Program. cpp**, que contiene la `main()` función. Aquí es donde se inicia la ejecución en una aplicación de consola de UWP. Obtenga acceso a los argumentos de la línea de comandos con los `__argc` `__argv` parámetros y. La aplicación de consola de UWP se cierra cuando el control vuelve de `main()` .

La plantilla **/WinRT de C++** de la aplicación de consola agrega el siguiente ejemplo de **Program. cpp** :

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>Comportamiento de la aplicación de consola de UWP

Una aplicación de consola de UWP puede tener acceso al sistema de archivos desde el directorio desde el que se ejecuta y a continuación. Esto es posible porque la plantilla agrega la extensión [AppExecutionAlias](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) al archivo package. appxmanifest de la aplicación. Esta extensión también permite al usuario escribir el alias desde una ventana de la consola para iniciar la aplicación. No es necesario que la aplicación esté en la ruta de acceso del sistema para iniciar.

Además, puede proporcionar un amplio acceso al sistema de archivos a la aplicación de consola de UWP agregando la funcionalidad restringida `broadFileSystemAccess` como se describe en [permisos de acceso a archivos](../files/file-access-permissions.md). Esta funcionalidad funciona con las API del espacio de nombres [**Windows. Storage**](/uwp/api/Windows.Storage) .

Se puede ejecutar más de una instancia de una aplicación de consola de UWP a la vez porque la plantilla agrega la funcionalidad [SupportsMultipleInstances](multi-instance-uwp.md) al archivo package. appxmanifest de la aplicación.

La plantilla también agrega la `Subsystem="console"` funcionalidad al archivo package. appxmanifest, que indica que esta aplicación de UWP es una aplicación de consola. Tenga en cuenta los `desktop4` `namespace` prefijos y iot2. Las aplicaciones de consola de UWP solo se admiten en proyectos de escritorio y Internet de las cosas (IoT).

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>Consideraciones adicionales para las aplicaciones de consola de UWP

- Solo las aplicaciones UWP de C++/WinRT y C++/CX pueden ser aplicaciones de consola.
- Las aplicaciones de consola de UWP deben tener como destino el tipo de proyecto de IoT o de escritorio.
- Es posible que las aplicaciones de la consola de UWP no creen una ventana. No pueden usar MessageBox () o Location (), ni ninguna otra API que pueda crear una ventana por cualquier motivo, como los mensajes de consentimiento del usuario.
- Las aplicaciones de consola de UWP no pueden consumir tareas en segundo plano ni servir como tarea en segundo plano.
- Con la excepción de la [activación desde la línea de comandos](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97), las aplicaciones de consola de UWP no admiten contratos de activación, incluida la Asociación de archivos, la Asociación de protocolos, etc.
- Aunque las aplicaciones de consola de UWP admiten la creación de instancias múltiples, no admiten [la redirección de varias instancias](multi-instance-uwp.md)
- Para obtener una lista de las API de Win32 que están disponibles para las aplicaciones para UWP, consulte [API de Win32 y com para aplicaciones de UWP](/uwp/win32-and-com/win32-and-com-for-uwp-apps) .