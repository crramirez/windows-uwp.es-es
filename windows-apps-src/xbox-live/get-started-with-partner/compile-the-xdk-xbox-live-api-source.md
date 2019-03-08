---
title: Compile el código fuente de la API de XDK Xbox Live
description: Obtenga información sobre cómo compilar el origen de la API de Xbox Live que se distribuye con el Kit de desarrollo de Xbox (XDK).
ms.assetid: 78425e82-c132-4f6b-9db3-2536862f1ce5
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, xdk
ms.localizationpriority: medium
ms.openlocfilehash: 9a98af637c8c60449cd2005c4fc6f83f9b0719cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661000"
---
# <a name="compile-the-xbox-developer-kit-xdk-xbox-live-api-source"></a>Compile el código fuente de la API de Xbox Developer Kit (XDK) Xbox Live

El Kit de desarrollo de Xbox (XDK) incluye código fuente para la creación de Microsoft.Xbox.Services.dll (XSAPI). Los desarrolladores pueden seguir estas instrucciones para actualizar sus proyectos para usar una compilación local del archivo DLL.

Es posible que desee compilar XSAPI si:
1. Si desea depurar un problema para comprender de dónde procede un código de error.
1. Si se proporciona una revisión de código de origen para solucionar un problema, antes de que se puede distribuir entre un QFE.

## <a name="to-compile-the-xdk-c-xsapi-project-for-yourself"></a>Para compilar el proyecto XDK C++ XSAPI usted mismo

<ol>
  <li> Obtiene el origen Microsoft.Xbox.Services. Para ello, extraiga todos los archivos de "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox API\8.0\SourceDist\Xbox.Services.zip Services" en una carpeta grabable fuera "C:\Program Files (x86)", o se puede clonar el origen desde el <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> Si el proyecto hace referencia a la DLL de anterior a la compilación, deberá quitar la referencia</li>
    <ul>
      <li> Para Visual Studio 2012: Elija "Proyecto -> referencias..." en Visual Studio. Si aparece como una referencia de API de servicios de Xbox, selecciónelo y haga clic en "Quitar la referencia". Haga clic en "Aceptar" y guarde el archivo de proyecto.</li>
      <li> Para Visual Studio 2015 o 2017: Elija "proyecto -> Agregar referencias..." en Visual Studio. Si se activa la API de servicios de Xbox, desactívela. Haga clic en "Aceptar" y guarde el archivo de proyecto.</li>
    </ul>
  <li> Si va a compilar con el XDK, elija "archivo -> Agregar -> Existing Project..." en Visual Studio para agregar los siguientes dos proyectos a la solución de la aplicación. Los archivos vcxproj se ubicará en la carpeta que ha extraído el origen.</li>
Para Visual Studio 2017: <ul>
      <li>\Build\Microsoft.Xbox.Services.141.XDK.Cpp\Microsoft.Xbox.Services.141.XDK.Cpp.vcxproj</li>   <li>\External\cpprestsdk\Release\src\build\vs15.xbox\casablanca141.Xbox.vcxproj</li>
    </ul>
Para Visual Studio 2015: <ul>
      <li>\Build\Microsoft.Xbox.Services.140.XDK.Cpp\Microsoft.Xbox.Services.140.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs14.xbox\casablanca140.Xbox.vcxproj</li>
    </ul>
Para Visual Studio 2012: <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.Cpp\Microsoft.Xbox.Services.110.XDK.Cpp.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
    <li> Agregar los proyectos de código fuente como una referencia al elegir proyecto -> referencias... y seleccione "Agregar referencia". En "Solución -> proyectos", compruebe las entradas para ambos proyectos anteriores, a continuación, haga clic en Aceptar.</li>
    <li> Agregue el archivo de propiedades al proyecto haciendo clic en "Ver -> otras Windows -> Administrador de propiedades" y haciendo clic en el proyecto, seleccione "Agregar hoja de propiedades existente" y, por último, seleccione el archivo de xsapi.staticlib.props en la raíz del SDK sourch.  Si el sistema de compilación no es compatible con archivos de propiedades, debe agregar manualmente las bibliotecas tal como se muestra en %XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\ y definiciones del preprocesador Xbox.Services.API.Cpp.props</li>
    <li> Agregue el archivo services.h a su origen de la aplicación haciendo clic en el proyecto, agregar -> elemento existente y elija el {SDK root}\Include\xsapi\services.h archivo de código fuente</li>
    <li> Asegúrese de que la "carpeta de salida" del proyecto de aplicación y el proyecto de servicios de Xbox son los mismos. Esta configuración puede encontrarse en el proyecto de Visual Studio -> propiedades propiedades de configuración -> General -> directorio de salida.</li>
    <li> Recompile la solución de Visual Studio</li>
</ol>

## <a name="to-compile-the-xdk-winrt-xsapi-project-for-yourself"></a>Para compilar el proyecto XDK WinRT XSAPI usted mismo

<ol>
  <li> Obtiene el origen Microsoft.Xbox.Services. Para ello, se extraen todos los archivos de "%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox API\8.0\SourceDist\Xbox.Services.zip Services" en una carpeta grabable fuera "C:\Program Files (x86)", o se puede clonar el origen desde el <a href ="https://github.com/Microsoft/xbox-live-api">https://github.com/Microsoft/xbox-live-api</a></li>
  <li> Si el proyecto hace referencia a la DLL de anterior a la compilación, deberá quitar la referencia</li>
    <ul>
      <li> Para Visual Studio 2012: Elija "Proyecto -> referencias..." en Visual Studio. Si aparece como una referencia de API de servicios de Xbox, selecciónelo y haga clic en "Quitar la referencia". Haga clic en "Aceptar" y guarde el archivo de proyecto.</li>
      <li> Para Visual Studio 2015 o 2017: Elija "proyecto -> Agregar referencias..." en Visual Studio. Si se activa la API de servicios de Xbox, desactívela. Haga clic en "Aceptar" y guarde el archivo de proyecto.</li>
    </ul>
  <li> Si va a compilar con el XDK, elija "archivo -> Agregar -> Existing Project..." en Visual Studio para agregar los siguientes dos proyectos a la solución de la aplicación. Los archivos vcxproj se ubicará en la carpeta que ha extraído el origen.  Para Visual Studio 2015, los proyectos se actualizarán automáticamente al formato de VS2015.</li>
    <ul>
      <li>\Build\Microsoft.Xbox.Services.110.XDK.WinRT\Microsoft.Xbox.Services.110.XDK.WinRT.vcxproj</li> <li>\External\cpprestsdk\Release\src\build\vs11.xbox\casablanca110.Xbox.vcxproj</li>
    </ul>
  <li> En Visual Studio, agregue las referencias:</li>
    <ul>
      <li> Para Visual Studio 2012: Elija "Proyecto -> referencias..." y seleccione "Agregar referencia" en Visual Studio. En la solución -> proyectos, chceck las entradas para ambos proyectos anteriores y haga clic en Aceptar.</li>
      <li> Para Visual Studio 2015 o 2017: Elija "proyecto -> Agregar referencias..." en Visual Studio. En los proyectos, compruebe las entradas para ambos proyectos anteriores y haga clic en Aceptar.</li>
    </ul>
  <li> Asegúrese de que la "carpeta de salida" del proyecto de aplicación y el proyecto de servicios de Xbox son los mismos. Esta configuración puede encontrarse en el proyecto de Visual Studio -> propiedades propiedades de configuración -> General -> directorio de salida.</li>
  <li> Recompile la solución de Visual Studio</li>
</ol>
