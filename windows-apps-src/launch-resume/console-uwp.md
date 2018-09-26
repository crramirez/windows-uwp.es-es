---
author: TylerMSFT
title: Crear una aplicación de consola con la Plataforma universal de Windows
description: En este tema, se describe cómo escribir una aplicación para UWP que se ejecuta en una ventana de la consola.
keywords: console uwp
ms.author: twhitney
ms.date: 08/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: e4c1b1df8ad29635f38ae5b373685d3504a4eb60
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "4208507"
---
# <a name="create-a-universal-windows-platform-console-app"></a>Crear una aplicación de consola de la Plataforma universal de Windows

En este tema se describe cómo crear un [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) o C++ / CX Universal Windows Platform (UWP) aplicación de consola.

A partir de Windows 10, versión 1803, puedes escribir C++ / WinRT o C++ / CX UWP las aplicaciones de consola que se ejecutan en una ventana de consola, como una ventana de consola DOS o PowerShell. Las aplicaciones de consola usan la ventana de consola para entrada y salida y pueden usar las funciones de [Universal en tiempo de ejecución de C](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference) como **printf** y **getchar**. Las aplicaciones de consola UWP pueden publicarse en la Microsoft Store. Tienen una entrada en la lista de aplicaciones y un icono principal que se puede anclar al menú Inicio. Aplicaciones de consola UWP se pueden iniciar desde el menú Inicio, aunque generalmente se inician desde la línea de comandos.

Para ver una en acción, este es un vídeo sobre cómo crear una aplicación de consola UWP.

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>Usar una plantilla de aplicación de consola UWP 

Para crear una aplicación de consola UWP, instala primero las **plantillas de proyecto de aplicación de consola (universal)**, que están disponibles en [Visual Studio Marketplace ](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal). Las plantillas instaladas, a continuación, están disponibles en el **Nuevo proyecto** > **instalado** > **Otros lenguajes** > **Visual C++** > **Universal de Windows** como **aplicación de consola c++ / WinRT (Universal Windows) **y **Console App C++ / CX (Universal Windows)**.

## <a name="add-your-code-to-main"></a>Agregar el código a main()

Las plantillas agregan **Program.cpp**, que contiene la función `main()`. Aquí es donde comienza la ejecución de una aplicación de consola UWP. Accede a los argumentos de línea de comandos con los parámetros `__argc` y `__argv`. La aplicación de la consola UWP se cierra cuando se devuelve el control desde `main()`.

El siguiente ejemplo de **Program.cpp** se agrega mediante la **aplicación de consola c++ / WinRT** plantilla:

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

## <a name="uwp-console-app-behavior"></a>Comportamiento de la aplicación de consola UWP

Una aplicación de consola UWP puede obtener acceso al sistema de archivos desde el directorio desde el que se ejecuta y por debajo. Esto es posible porque la plantilla agrega la extensión [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) al archivo Package.appxmanifest de tu aplicación. Esta extensión también permite al usuario escribir el alias desde una ventana de consola para iniciar la aplicación. La aplicación no tiene que estar en la ruta de acceso del sistema para iniciarse.

Además puede proporcionar acceso total al sistema de archivos a tu aplicación de consola UWP agregando la funcionalidad restringida `broadFileSystemAccess`, tal y como se describe en [Permisos de acceso de archivos](https://docs.microsoft.com/windows/uwp/files/file-access-permissions). Esta funcionalidad funciona con las API del espacio de nombres [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346).

Puedes ejecutar más de una instancia de una aplicación de consola UWP al mismo tiempo dado que la plantilla agrega la funcionalidad [SupportsMultipleInstances](multi-instance-uwp.md) al archivo Package.appxmanifest de tu aplicación.

La plantilla también agrega la funcionalidad `Subsystem="console"` al archivo Package.appxmanifest, que indica que esta aplicación para UWP es una aplicación de consola. Fíjate en los prefijos `desktop4` y iot2 `namespace`. Las aplicaciones de consola UWP solo se admiten en los proyectos de Internet de las cosas (IoT) y de escritorio.

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

## <a name="additional-considerations-for-uwp-console-apps"></a>Consideraciones adicionales sobre las aplicaciones de consola UWP

- Solo C++ / WinRT y C++ / CX UWP aplicaciones pueden ser aplicaciones de consola.
- Las aplicaciones de consola UWP deben tener como destino el escritorio o el tipo de proyecto de IoT.
- Aplicaciones de consola UWP no pueden crear una ventana. No se usan MessageBox(), o Location() o cualquier otra API que se puede crear una ventana por cualquier motivo, como avisos de consentimiento del usuario.
- Las aplicaciones de consola UWP no pueden usar tareas en segundo plano ni servir como una tarea en segundo plano.
- Con la excepción de la [activación de línea de comandos](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97), las aplicaciones de consola UWP no son compatibles con contratos de activación de soporte, incluida la asociación de archivos, la asociación de protocolos, etc.
- Aunque las aplicaciones de consola UWP admiten instancias múltiples, no admiten el [redireccionamiento de instancias múltiples](multi-instance-uwp.md)
- Para obtener una lista de las API de Win32 que están disponibles para las aplicaciones para UWP, consulta [Win32 and COM APIs for UWP apps](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) (API de Win32 y COM para las aplicaciones para UWP).