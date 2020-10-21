---
description: C#/WinRT es un conjunto de herramientas que proporciona compatibilidad con la proyección de WinRT para el código C#.
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, standard, c#, winrt, cswinrt, proyección
ms.localizationpriority: medium
ms.openlocfilehash: 844d8441777e7c95e2b562cf7dff748600a072e9
ms.sourcegitcommit: 861c381a31e4a5fd75f94ca19952b2baaa2b72df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92171139"
---
# <a name="cwinrt"></a>C#/WinRT

> [!IMPORTANT]
> C#/WinRT está en versión preliminar pública y puede que se modifique sustancialmente antes de la versión final. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

C#/WinRT es un kit de herramientas con paquetes NuGet que proporciona compatibilidad con la proyección de Windows Runtime (WinRT) para el lenguaje C#. Una *proyección* es una capa de traducción, como un ensamblado de interoperabilidad, que permite programar las API de WinRT de forma natural y familiar para el lenguaje de destino. Por ejemplo, la proyección de C#/WinRT oculta los detalles de la interoperabilidad entre las interfaces de C# y WinRT, y proporciona asignaciones de muchos tipos de WinRT a equivalentes adecuados de .NET, como cadenas, URI, tipos de valores comunes y colecciones genéricas.

Actualmente, C#/WinRT proporciona compatibilidad para consumir tipos de WinRT, y la versión preliminar actual te permite [crear](#create-an-interop-assembly) y [referenciar](#reference-an-interop-assembly) ensamblados de interoperabilidad de WinRT. En futuras versiones de C#/WinRT, agregaremos compatibilidad para la creación de tipos de WinRT en C#.

Para obtener más información sobre C#/WinRT, consulta el [repositorio de C#/WinRT en GitHub](https://aka.ms/cswinrt/repo).

## <a name="motivation-for-cwinrt"></a>Motivación para C#/WinRT

[.NET Core](/dotnet/core/) es el centro de la plataforma .NET, y .NET 5 es la próxima versión principal. Es un entorno de ejecución multiplataforma y de código abierto que se puede usar para compilar aplicaciones de dispositivo, nube e IoT.

Las versiones anteriores de .NET Framework y .NET Core tenían conocimientos integrados de WinRT, una tecnología específica de Windows. Para admitir los objetivos de portabilidad y eficiencia de .NET 5, hemos quitado la compatibilidad con la proyección de WinRT fuera del entorno de ejecución y el compilador de .NET, y la hemos trasladado al kit de herramientas de C#/WinRT. El objetivo de C#/WinRT es proporcionar paridad con la compatibilidad integrada de WinRT proporcionada por las versiones anteriores del compilador de C# y el entorno de ejecución de .NET. Para más detalles, consulta [Asignaciones de .NET de tipos de Windows Runtime](../winrt-components/net-framework-mappings-of-windows-runtime-types.md).

C#/WinRT también admite WinUI 3.0. Esta versión de WinUI quita del sistema operativo las características y los controles nativos de la interfaz de usuario de Microsoft. Esto permite a los desarrolladores de aplicaciones usar los controles y elementos visuales más recientes en Windows 10, versión 1803 y versiones posteriores.

Por último, C#/WinRT es un kit de herramientas general y está diseñado para admitir otros escenarios en los que la compatibilidad integrada con WinRT no está disponible en el compilador de C# o el entorno de ejecución de .NET. C#/WinRT admite versiones del entorno de ejecución de .NET compatibles con .NET Standard 2.0, como Mono 5.4.

## <a name="create-an-interop-assembly"></a>Creación de un ensamblado de interoperabilidad

Las API de WinRT se definen en archivos de metadatos de Windows (*.winmd). El paquete NuGet de C#/WinRT ([Microsoft.Windows.CsWinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)) incluye el compilador de C#/WinRT, **cswinrt**, que puede usar para procesar archivos de metadatos de Windows y generar código de .NET 5.0 en C#. Puedes compilar estos archivos de origen en ensamblados de interoperabilidad, de forma similar a como [C++/WinRT](../cpp-and-winrt-apis/index.md) genera encabezados para la proyección del lenguaje C++. Después, puedes distribuir los ensamblados de interoperabilidad de C#/WinRT a los que las aplicaciones hagan referencia, junto con el ensamblado de en tiempo de ejecución de C#/WinRT.

Para ver un tutorial que muestra cómo crear y distribuir un ensamblado de interoperabilidad como un paquete NuGet, consulte [Tutorial: Generación de una proyección de .NET 5 desde un componente de C++/WinRT y actualización del NuGet](net-projection-from-cppwinrt-component.md).

### <a name="invoke-cswinrtexe"></a>Invocación de cswinrt.exe

Para invocar cswinrt.exe desde un proyecto, instale el [paquete NuGet de C#/WinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/) más reciente. A continuación, puede establecer las propiedades del proyecto específicas de C#/WinRT en un proyecto de **biblioteca de C#** para generar un ensamblado de interoperabilidad. El siguiente fragmento de proyecto muestra una invocación simple de **cswinrt** para generar orígenes de proyección para los tipos del espacio de nombres Contoso. Estos orígenes se incluyen en la compilación del proyecto.

```xml
<PropertyGroup>
  <CsWinRTIncludes>Contoso</CsWinRTIncludes>
</PropertyGroup>
```

En este proyecto también tendría que hacer referencia al paquete NuGet CsWinRT y a los archivos .winmd específicos del proyecto que quiere proyectar, ya sea a través de un paquete NuGet, una referencia de proyecto o una referencia directa. De forma predeterminada, los espacios de nombres de **Windows** y **Microsoft** no se proyectan. Para obtener una lista completa de las propiedades del proyecto CsWinRT, consulte la [documentación de NuGet de CsWinRT](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md).

### <a name="distribute-the-interop-assembly"></a>Distribución del ensamblado de interoperabilidad

Un ensamblado de interoperabilidad normalmente se distribuye como paquete de NuGet, junto con una dependencia en el paquete NuGet de C#/WinRT para el ensamblado de tiempo de ejecución de C#/WinRT necesario, **winrt.runtime.dll**.

Para asegurarse de que la versión correcta del tiempo de ejecución de C#/WinRT está implementado para las aplicaciones de .NET 5.0, incluya una condición `targetFramework` en el archivo .nuspec con una dependencia en el paquete NuGet de C#/WinRT.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework="net5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.9.0" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> El moniker de la plataforma de destino para .NET 5.0 se está moviendo de ".NETCoreApp5.0" a ".net5.0". 

## <a name="reference-an-interop-assembly"></a>Referencia a un ensamblado de interoperabilidad

Normalmente, los proyectos de aplicación hacen referencia a los ensamblados de interoperabilidad de C#/WinRT. Pero, a su vez, también pueden hacer referencia a ellos los ensamblados de interoperabilidad intermedios. Por ejemplo, el ensamblado de interoperabilidad WinUI haría referencia al ensamblado de interoperabilidad de Windows SDK.

Para consumir los tipos de C#/WinRT proyectados, agrega al proyecto una referencia al paquete NuGet de interoperabilidad de C#/WinRT adecuado. Esto hace que tanto el ensamblado de interoperabilidad como el ensamblado en tiempo de ejecución de C#/WinRT se agreguen al proyecto.

Si distribuyes un componente WinRT de terceros sin un ensamblado de interoperabilidad oficial, un proyecto de aplicación puede seguir el procedimiento para [crear un ensamblado de interoperabilidad](#create-an-interop-assembly) para generar sus propios orígenes de proyección privados. No recomendamos este enfoque, ya que puede producir proyecciones en conflicto del mismo tipo dentro de un proceso. El empaquetado de NuGet, según el esquema de [Semantic Versioning](https://semver.org), está diseñado para evitar esto. Se prefiere un ensamblado de interoperabilidad de terceros oficial.

### <a name="winrt-type-activation"></a>Activación de tipos de WinRT

C#/WinRT admite la activación de tipos de WinRT hospedados por el sistema operativo, así como componentes de terceros, como [Win2D](https://www.nuget.org/packages/Win2D.uwp/). La compatibilidad con la activación de componentes de terceros en una aplicación de escritorio se habilita con la [activación de WinRT sin registro](https://blogs.windows.com/windowsdeveloper/2019/04/30/enhancing-non-packaged-desktop-apps-using-windows-runtime-components/), disponible en Windows 10, versión 1903 y versiones posteriores. Esto también puede requerir el uso del paquete [VCRT Forwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/), si el componente se compiló para aplicaciones para UWP como destino.

C#/WinRT también proporciona una ruta de reserva de activación si Windows no puede activar el tipo tal y como se ha descrito anteriormente. En este caso, C#/WinRT intenta encontrar un archivo DLL de implementación nativo en función del nombre de tipo completo, quitando progresivamente los elementos. Por ejemplo, la lógica de reserva intentaría activar el tipo Contoso.Controls.Widget desde los módulos siguientes, en secuencia:

1. Contoso.Controls.Widget.dll
2. Contoso.Controls.dll
3. Contoso.dll

C#/WinRT usa el [orden de búsqueda alternativo LoadLibrary](/windows/win32/dlls/dynamic-link-library-search-order#alternate-search-order-for-desktop-applications) para buscar un archivo DLL de implementación. Una aplicación que se base en este comportamiento de reserva debe empaquetar el archivo DLL de implementación junto con el módulo de la aplicación.

## <a name="known-issues"></a>Problemas conocidos

Hay algunos problemas de rendimiento conocidos relacionados con la interoperabilidad en la versión preliminar actual de C#/WinRT. Estos se abordarán antes de la versión final a finales del 2020. Otros problemas conocidos y cambios importantes se anotarán en el [repositorio de GitHub de C#/WinRT](https://aka.ms/cswinrt/repo).

Si tienes algún problema funcional con el paquete NuGet de C#/WinRT, el compilador cswinrt.exe o los orígenes de proyección generados, envíanos los problemas a través de la [página de problemas de C#/WinRT](https://github.com/microsoft/CsWinRT/issues).

## <a name="additional-resources"></a>Recursos adicionales

* [Repositorio de C#/WinRT en GitHub](https://aka.ms/cswinrt/repo)
