---
author: normesta
Description: "En esta guía se explica cómo configurar la solución de Visual Studio para editar, depurar y empaquetar la aplicación de escritorio para el Puente de dispositivo de escritorio."
Search.Product: eADQiWindows 10XVcnh
title: "Empaquetar una aplicación mediante Visual Studio (Puente de dispositivo de escritorio a UWP)"
ms.author: normesta
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.openlocfilehash: d8919448b965f18ff7f8fdaeda325889e495ef85
ms.sourcegitcommit: f6dd9568eafa10ee5cb2b849c0d82d84a1c5fb93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2017
---
# <a name="package-an-app-by-using-visual-studio-desktop-bridge"></a>Empaquetar una aplicación mediante Visual Studio (Puente de dispositivo de escritorio a UWP)

Puedes usar Visual Studio para generar un paquete para tu aplicación de escritorio. Luego, puedes publicar que ese paquete en la Tienda Windows o transferirlo localmente a uno o más equipos.

Esta guía le muestra cómo configurar la solución y generar luego un paquete de la aplicación de escritorio.

## <a name="first-consider-how-youll-distribute-your-app"></a>En primer lugar, piensa la manera de distribuir la aplicación.

Si vas a publicar la aplicación en la [Tienda Windows](https://www.microsoft.com/store/apps), comienza rellenando [este formulario](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft se pondrá en contacto contigo para iniciar el proceso de incorporación. Como parte de este proceso, podrás reservar un nombre en la tienda y obtener la información que necesitas para empaquetar la aplicación.

## <a name="add-a-packaging-project-to-your-solution"></a>Agregar un proyecto de empaquetado en tu solución

1. En Visual Studio, abre la solución que contiene tu proyecto de aplicación de escritorio.

2. Agrega un proyecto **Aplicación vacía (Windows universal)** de JavaScript a la solución.

   No tendrás que agregarle ningún código. Solo está ahí para generar un paquete para ti. Nos referiremos a este proyecto como el "proyecto de empaquetado".

   ![proyecto de UWP de JavaScript](images/desktop-to-uwp/javascript-uwp-project.png)

   >[!IMPORTANT]
   >En general, debes usar la versión de JavaScript de este proyecto.  Las versiones de C#, VB.NET y C++ tienen algunos problemas, pero si quieres usar alguna de ellas, consulta la guía [Problemas conocidos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor) antes de hacerlo.

## <a name="add-the-desktop-application-binaries-to-the-packaging-project"></a>Agregar los archivos binarios de aplicación de escritorio al proyecto de empaquetado

Agrega los archivos binarios directamente al proyecto de empaquetado.

1. En el **Explorador de soluciones**, amplía la carpeta del proyecto de empaquetado, crea una subcarpeta y asígnale el nombre que quieras (por ejemplo: **win32**).

2. Haz clic con el botón derecho en la subcarpeta y elige **Agregar elemento existente**.

3. En el cuadro de diálogo **Agregar elemento existente**, busca y agrega los archivos de la carpeta de resultados de la aplicación de escritorio. Esto incluye no solo los archivos ejecutables, sino también los archivos .dll o .config que se encuentran en esa carpeta.

   ![Archivo ejecutable de referencia](images/desktop-to-uwp/cpp-exe-reference.png)

   Cada vez que realices un cambio en el proyecto de aplicación de escritorio, deberás copiar una nueva versión de esos archivos en el proyecto de empaquetado. Puede automatizar esto agregando un evento posterior a la compilación en el archivo de proyecto del proyecto de empaquetado. A continuación te mostramos un ejemplo.

   ```XML
   <Target Name="PostBuildEvent">
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe.config"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.pdb"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.dll"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.pdb"
       DestinationFolder="win32" />
   </Target>
   ```

## <a name="modify-the-package-manifest"></a>Modificar el manifiesto del paquete

El proyecto de empaquetado contiene un archivo que describe la configuración del paquete. De manera predeterminada, este archivo describe una aplicación para UWP, de modo que deberás modificarlo para que el sistema sepa que el paquete incluye una aplicación de escritorio que se ejecuta en plena confianza.  

1. En el **Explorador de soluciones**, amplía el proyecto de empaquetado, haz clic con el botón derecho en el archivo **package.appxmanifest** y luego elige **Ver código**.

   ![Proyecto de dotnet de referencia](images/desktop-to-uwp/reference-dotnet-project.png)

2. Agrega este espacio de nombres a la parte superior del archivo, y el prefijo del espacio de nombres a la lista de ``IgnorableNamespaces``.

   ```XML
   xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
   ```
   Cuando hayas terminado, las declaraciones del espacio de nombres tendrán un aspecto similar al siguiente:

   ```XML
   <Package
     xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
     xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
     xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
     xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
     IgnorableNamespaces="uap mp rescap">
   ```

3. Busca el elemento ``TargetDeviceFamily`` y establece el atributo ``Name``en **Windows.Desktop**, el atributo ``MinVersion`` en la versión mínima del proyecto de empaquetado y la ``MaxVersionTested`` en la versión de destino del proyecto de empaquetado.

   ```XML
   <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.10586.0" MaxVersionTested="10.0.15063.0" />
   ```

   Encontrarás la versión mínima y la versión de destino en las páginas de propiedades del proyecto de empaquetado.

   ![Configuración de versión mínima y de destino](images/desktop-to-uwp/min-target-version-settings.png)


4. Quita el atributo ``StartPage`` del elemento ``Application``. Luego agrega los atributos ``Executable`` y ``EntryPoint``.

   El elemento ``Application`` tendrá el siguiente aspecto.

   ```XML
   <Application Id="App"  Executable=" " EntryPoint=" ">
   ```

5. Establece el atributo ``Executable`` en el nombre del archivo ejecutable de tu aplicación de escritorio. Luego establece el atributo ``EntryPoint`` en **Windows.FullTrustApplication **.

   El elemento ``Application`` tendrá un aspecto similar la siguiente.

   ```XML
   <Application Id="App"  Executable="win32\MyWindowsFormsApplication.exe" EntryPoint="Windows.FullTrustApplication">
   ```
6. Agrega la funcionalidad ``runFullTrust`` al elemento ``Capabilities``.

   ```XML
     <rescap:Capability Name="runFullTrust"/>
   ```
   Pueden aparecer marcas onduladas azules debajo de esta declaración, pero puedes ignorarlas sin problemas.

   >[!IMPORTANT]
   Si está creando un paquete para una aplicación de escritorio de C++, tendrás que realizar algunos cambios adicionales en el archivo de manifiesto para poder implementar los tiempos de ejecución de Visual C++ junto con la aplicación. Consulta [Usar tiempos de ejecución de Visual C++ en un proyecto de Puente de dispositivo de escritorio](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

7. Compila el proyecto de empaquetado para garantizar que no aparece ningún error.

8. Si quieres probar el paquete, consulta [Ejecutar, depurar y probar una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-debug.md).

   A continuación, vuelve a esta guía y consulta la siguiente sección para generar el paquete.

## <a name="generate-a-package"></a>Generar un paquete

Para generar un paquete de la aplicación, sigue las instrucciones que se describen en este tema: [Empaquetado de aplicaciones para UWP](..\packaging\packaging-uwp-apps.md).

Cuando llegues a la pantalla **Seleccionar y configurar paquetes**, dedica un momento a pensar qué tipo de archivos binarios vas a incluir en el paquete antes de seleccionar cualquiera de las casillas.

* Si has [ampliado](desktop-to-uwp-extend.md) la aplicación de escritorio mediante la adición de un proyecto de Plataforma universal de Windows basado en C#, C++ o VB.NET-sa la solución, activas las casillas **x86** y **x64**.  

* De lo contrario, elige la casilla **Neutro**.

>[!NOTE]
El motivo por el que tendrías que elegir de manera explícita cada plataforma compatible es porque una solución que has ampliado contiene dos tipos de archivos binarios: uno para el proyecto de UWP y otro para el proyecto de escritorio. Dado que estos son diferentes tipos de archivos binarios, .NET Native debe producir explícitamente archivos binarios nativos para cada plataforma.

Si recibes errores al intentar generar el paquete, consulta la guía [Problemas conocidos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor) y si el problema no aparece en la lista, comparte el problema con nosotros [aquí](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

## <a name="next-steps"></a>Pasos siguientes

**Ejecutar la aplicación o buscar y corregir problemas**

Consulta [Ejecutar, depurar y probar una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-debug.md)

**Mejorar tu aplicación de escritorio agregando las API de UWP**

Consulta [Mejorar tu aplicación de escritorio para Windows 10](desktop-to-uwp-enhance.md).

**Ampliar tu aplicación de escritorio agregando los componentes de UWP**

Consulta [Ampliar tu aplicación de escritorio con componentes de UWP modernos](desktop-to-uwp-extend.md).

**Distribuir la aplicación**

Consulta [Distribuir una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-distribute.md)

**Encuentra respuestas a preguntas específicas**

Nuestro equipo supervisa estas [etiquetas de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envíanos tus comentarios acerca de este artículo**

Usa la sección comentarios que tienes a continuación.
