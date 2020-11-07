---
description: En este tutorial se muestra cómo usar C#/WinRT para generar una proyección de .NET 5 para un componente/WinRT de C++.
title: Tutorial para generar una proyección de .NET 5 desde un componente/WinRT de C++ y distribuir NuGet
ms.date: 10/12/2020
ms.topic: article
keywords: Windows 10, c#, winrt, cswinrt, proyección
ms.localizationpriority: medium
ms.openlocfilehash: 817c4ec364040cbe64f8ab466a5bdf059d8c2dda
ms.sourcegitcommit: aaa72ddeb01b074266f4cd51740eec8d1905d62d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94339653"
---
# <a name="walkthrough-generate-a-net-5-projection-from-a-cwinrt-component-and-distribute-the-nuget"></a>Tutorial: generación de una proyección de .NET 5 desde un componente/WinRT de C++ y distribución de NuGet

En este tutorial se muestra cómo usar [C#/WinRT](index.md) para generar una proyección de .net 5 para un componente/WinRT de C++, crear el paquete de Nuget asociado y hacer referencia al paquete de Nuget desde una aplicación de consola de C# de .net 5.

Puede descargar el ejemplo completo de este tutorial en GitHub [aquí](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample).

> [!NOTE]
> Este tutorial está escrito para la versión preliminar más reciente de C#/WinRT (RC2). Esperamos que la próxima versión 1,0 tenga más actualizaciones y mejoras en la experiencia del desarrollador.

## <a name="prerequisites"></a>Requisitos previos

Este tutorial y el ejemplo correspondiente requieren las herramientas y los componentes siguientes:

- [Visual Studio 16,8 Preview 3](https://visualstudio.microsoft.com/vs/preview/) (o posterior) con la carga de trabajo de desarrollo de plataforma universal de Windows instalada. En **detalles de la instalación**  >  **plataforma universal de Windows desarrollo** , active la opción de **herramientas de plataforma universal de Windows de C++ (v14x)** .
- [SDK de .net 5,0 RC2](https://github.com/dotnet/installer).
- [Extensión VSIX de c++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) para plantillas de proyecto de/WinRT de c++.

## <a name="create-a-simple-cwinrt-runtime-component"></a>Crear un componente de tiempo de ejecución de C++/WinRT simple

Para seguir este tutorial, primero debe tener un componente/WinRT de C++ para el que crear una proyección de .NET 5. En este tutorial se usa el proyecto **SimpleMathComponent** en el ejemplo relacionado de github [aquí](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample/SimpleMathComponent). Se trata de un proyecto de **componente de Windows Runtime (C++/WinRT)** que se creó con la [extensión VSIX/WinRT de c++](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Después de copiar el proyecto en el equipo de desarrollo, abra la solución en Visual Studio 2019 versión preliminar.

El código de este proyecto proporciona la funcionalidad para las operaciones matemáticas básicas que se muestran en el archivo de encabezado siguiente. 

```cpp
// SimpleMath.h
...
namespace winrt::SimpleMathComponent::implementation
{
    struct SimpleMath: SimpleMathT<SimpleMath>
    {
        SimpleMath() = default;
        double add(double firstNumber, double secondNumber);
        double subtract(double firstNumber, double secondNumber);
        double multiply(double firstNumber, double secondNumber);
        double divide(double firstNumber, double secondNumber);
    };
}
```

Para obtener pasos más detallados sobre la creación de un componente/WinRT de C++ y la generación de un archivo. winmd, vea [Windows Runtime componentes con c++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md).

> [!NOTE]
> Si está implementando [IInspectable:: getruntimeclassname (](/windows/win32/api/inspectable/nf-inspectable-iinspectable-getruntimeclassname) en el componente, **debe** devolver un nombre de clase de WinRT válido. Dado que C#/WinRT usa la cadena de nombre de clase para la interoperabilidad, un nombre de clase en tiempo de ejecución incorrecto generará una **excepción InvalidCastException**.

## <a name="add-a-projection-project-to-the-component-solution"></a>Agregar un proyecto de proyección a la solución de componentes

Si ha clonado el ejemplo del repositorio, elimine primero el proyecto **SimpleMathProjection** para seguir el tutorial paso a paso.

1. Agregue un nuevo proyecto de **biblioteca de clases (.net Core)** a la solución.

    1. En **Explorador de soluciones** , haga clic con el botón secundario en el nodo de la solución y haga clic en **Agregar**  ->  **nuevo proyecto**.
    2. En el **cuadro de diálogo Agregar nuevo proyecto** , busque la plantilla de proyecto **biblioteca de clases (.net Core)** . Seleccione la plantilla y haga clic en **siguiente**.
    3. Asigne al nuevo proyecto el nombre **SimpleMathProjection** y haga clic en **crear**.

2. Elimine el archivo **Class1.CS** vacío del proyecto.

3. Instale el [paquete NuGet/WinRT de C#](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT).

    1. En **Explorador de soluciones** , haga clic con el botón derecho en el proyecto **SimpleMathProjection** y seleccione **administrar paquetes NuGet**. 
    2. Busque el paquete NuGet **Microsoft. Windows. CsWinRT** e instale la versión más reciente.

4. Agregue una referencia de proyecto al proyecto **SimpleMathComponent** . En **Explorador de soluciones** , haga clic con el botón secundario en el nodo **dependencias** en el proyecto **SimpleMathProjection** , seleccione **Agregar referencia de proyecto** y seleccione el proyecto **SimpleMathComponent** .

    > [!NOTE]
    > Si usa Visual Studio 16,8 Preview 4 o una versión posterior, ha terminado con esta sección después de completar el paso 4. Si usa Visual Studio 16,8 Preview 3, también debe completar el paso 5.

5. Si usa Visual Studio 16,8 Preview 3: en **Explorador de soluciones** , haga doble clic en el nodo **SimpleMathProjection** para abrir el archivo de proyecto en el editor, agregue los siguientes elementos al archivo y, a continuación, guarde y cierre el archivo.

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.Net.Compilers.Toolset" Version="3.8.0-4.20472.6" />
    </ItemGroup>

    <PropertyGroup>
      <RestoreSources>
        https://api.nuget.org/v3/index.json;
        https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json
      </RestoreSources>
    </PropertyGroup>
    ```

    Estos elementos instalan la versión necesaria del paquete de NuGet **Microsoft.net. compiladores. Toolset** , que incluye el último compilador de C#. En este tutorial se instala este paquete NuGet a través de estas referencias de archivo de proyecto porque es posible que la versión requerida de este paquete no esté disponible en la fuente de NuGet pública predeterminada.

Después de estos pasos, el **Explorador de soluciones** debe ser similar a este.

![Explorador de soluciones que muestra las dependencias del proyecto de proyección](images/projection-dependencies.png)

## <a name="edit-the-project-file-to-execute-cwinrt"></a>Editar el archivo de proyecto para ejecutar C#/WinRT

Antes de poder invocar **cswinrt.exe** y generar el ensamblado de proyección, debe editar el archivo de proyecto para el proyecto de proyección.

1. En **Explorador de soluciones** , haga doble clic en el nodo **SimpleMathProjection** para abrir el archivo de proyecto en el editor.

2. Actualice el `TargetFramework` elemento para hacer referencia al Windows SDK. Esto agrega depedencies de ensamblado que son necesarios para la compatibilidad con la interoperabilidad y proyección. Nuestro ejemplo tiene como destino la versión más reciente de Windows 10, tal como se muestra en este tutorial, **net 5.0-Windows 10.0.19041.0** (también conocido como SDK versión 2004).

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. Agregue un nuevo `PropertyGroup` elemento que establezca varias propiedades **cswinrt** .

    ```xml
    <PropertyGroup>
      <CsWinRTIncludes>SimpleMathComponent</CsWinRTIncludes>
      <CsWinRTGeneratedFilesDir>$(OutDir)</CsWinRTGeneratedFilesDir>
    </PropertyGroup>
    ```

    Estos son algunos detalles sobre la configuración de este ejemplo:

    - La `CsWinRTIncludes` propiedad especifica los espacios de nombres que se van a proyectar.
    - La `CsWinRTGeneratedFilesDir` propiedad establece el directorio de salida donde se generan los archivos de la proyección, que se establecen en la sección siguiente sobre la creación fuera del origen.

4. La versión más reciente de C#/WinRT en este tutorial puede requerir la especificación de los metadatos de Windows. Esto se corregirá en una versión futura de C#/WinRT. Esto se puede proporcionar con cualquiera de las siguientes opciones:

    - Una referencia de paquete, como a [Microsoft. Windows. SDK. Contracts]( https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts/), o
    - Un valor explícito establece con la `CsWinRTWindowsMetadata` propiedad:

      ```xml
      <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
      ```

5. Guarde y cierre el archivo **SimpleMathProjection. csproj** .

## <a name="build-projects-out-of-source"></a>Compilar proyectos fuera del origen

En el [ejemplo relacionado](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample), la compilación se configura con el archivo **Directory. Build. props** . Los archivos generados de la compilación de los proyectos **SimpleMathComponent** y **SimpleMathProjection** aparecen en la carpeta *_build* en el nivel de solución. Para configurar los proyectos para compilar fuera de origen, copie el archivo **Directory. Build. props** siguiente en el directorio que contiene el archivo de solución.

```xml
<Project>
  <PropertyGroup>
    <BuildOutDir>$([MSBuild]::NormalizeDirectory('$(SolutionDir)_build', '$(Platform)', '$(Configuration)'))</BuildOutDir>
    <OutDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'bin'))</OutDir>
    <IntDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'obj'))</IntDir>
  </PropertyGroup>
</Project>
```

Aunque este paso no es necesario para generar una proyección, proporciona simplicidad generando archivos de compilación desde ambos proyectos en el mismo directorio y facilitando la limpieza de la compilación. Tenga en cuenta que si no crea un origen, tanto **SimpleMathComponent. winmd** como el ensamblado de interoperabilidad **SimpleMathComponent.dll** se generarán en directorios diferentes en sus carpetas de proyecto respectivas. A continuación, se hace referencia a estos archivos en **SimpleMathProjection. nuspec** , por lo que las rutas de acceso tendrían que cambiarse según corresponda.

## <a name="create-a-nuget-package-from-the-projection"></a>Creación de un paquete NuGet desde la proyección

Para distribuir y usar el ensamblado de interoperabilidad, puede crear automáticamente un paquete NuGet al compilar la solución agregando algunas propiedades adicionales del proyecto. Este paquete incluirá el ensamblado de interoperabilidad y una dependencia en el paquete NuGet/WinRT de C# para el ensamblado en tiempo de ejecución de C#/WinRT requerido. Este ensamblado en tiempo de ejecución se denomina **winrt.runtime.dll** para los destinos de .net 5,0.

1. Agregue un archivo de especificación de NuGet (. nuspec) al proyecto **SimpleMathProjection** .

    1. En **Explorador de soluciones** , haga clic con el botón secundario en el nodo **SimpleMathProjection** , elija **Agregar**  ->  **nueva carpeta** y asigne a la carpeta el nombre **Nuget**. 
    2. Haga clic con el botón derecho en la carpeta **Nuget** , elija **Agregar**  ->  **nuevo elemento** , elija el archivo XML y asígnele el nombre **SimpleMathProjection. nuspec**. 

2. Agregue lo siguiente a **SimpleMathProjection. csproj** para generar automáticamente el paquete. Estas propiedades especifican el `NuspecFile` y el directorio para generar el paquete NuGet.

    ```xml
    <PropertyGroup>
      <GeneratedNugetDir>.\nuget\</GeneratedNugetDir>
      <NuspecFile>$(GeneratedNugetDir)SimpleMathProjection.nuspec</NuspecFile>
      <OutputPath>$(GeneratedNugetDir)</OutputPath>
      <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

3. Open the **SimpleMathProjection.nuspec** file to edit the package creation properties. Below is an example of a C++/WinRT component NuGet spec. Notice the `dependency` on CsWinRT for the `net5.0` target framework moniker, as well as the target for `lib\net5.0\SimpleMathProjection.dll`, which points to the projection assembly **SimpleMathComponent.dll** instead of **SimpleMathComponent.winmd**. This behavior is new in .NET 5.0 and enabled by C#/WinRT.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
      <metadata>
        <id>SimpleMathComponent</id>
        <version>0.1.0-prerelease</version>
        <authors>Contoso Math Inc.</authors>
        <description>A simple component with basic math operations</description>
        <dependencies>
          <group targetFramework=".NETCoreApp3.0" />
          <group targetFramework="UAP10.0" />
          <group targetFramework=".NETFramework4.6" />
          <group targetFramework="net5.0">
            <dependency id="Microsoft.Windows.CsWinRT" version="0.8.0" exclude="Build,Analyzers" />
          </group>
        </dependencies>
      </metadata>
      <files>
        <!--Support net46+, netcore3, net5, uap, c++ -->
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\netcoreapp3.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\uap10.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\net46\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathProjection\bin\SimpleMathProjection.dll" target="lib\net5.0\SimpleMathProjection.dll" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.dll" target="runtimes\win10-x64\native\SimpleMathComponent.dll" />
      </files>
    </package>
    ```

## <a name="build-the-solution-to-generate-the-projection-and-nuget-package"></a>Compilar la solución para generar la proyección y el paquete NuGet

En este momento ya puede compilar la solución: haga clic con el botón derecho en el nodo de la solución y seleccione **compilar solución**. En primer lugar, compilará el proyecto de componente y, a continuación, el proyecto de proyección. Los archivos Interop **. CS** y el ensamblado se generarán en el directorio de salida, además de los archivos de metadatos del proyecto de componente. También podrá ver el paquete NuGet generado **simplemathcomponent 0.1.0-prerelease. nupkg** en la carpeta **NuGet** .

![Explorador de soluciones que muestra la generación de proyección](images/projection-generated-files.png)

## <a name="reference-the-nuget-package-in-a-c-net-50-console-application"></a>Referencia al paquete de NuGet en una aplicación de consola de C# .NET 5,0

Para consumir el **SimpleMathComponent** proyectado, puede simplemente agregar una referencia al paquete de NuGet recién creado en la aplicación. En los pasos siguientes se muestra cómo hacerlo mediante la creación de una aplicación de consola simple en una solución independiente.

1. Cree una nueva solución con un proyecto de **aplicación de consola (.net Core)** .

    1. Abra Visual Studio, seleccione **Archivo** -> **Nuevo** -> **Proyecto**.
    2. En el **cuadro de diálogo Agregar nuevo proyecto** , busque la plantilla de proyecto **aplicación de consola (.net Core)** . Seleccione la plantilla y haga clic en **siguiente**.
    3. Asigne al nuevo proyecto el nombre **SampleConsoleApp** y haga clic en **crear**. La creación de este proyecto en una nueva solución le permite restaurar el paquete NuGet de **SimpleMathComponent** por separado.

2. En **Explorador de soluciones** , haga doble clic en el nodo **SampleConsoleApp** para abrir el archivo de proyecto **SampleConsoleApp. csproj** y actualice el moniker y la configuración de plataforma de destino, tal como se muestra en el ejemplo siguiente.

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. Agregue el paquete NuGet **SimpleMathComponent** al proyecto **SampleConsoleApp** . También necesitará el paquete NuGet [Microsoft. VCRTForwarders. 140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) , que es necesario en las aplicaciones que no están empaquetadas en un paquete MSIX. Para restaurar el **SimpleMathComponent** NuGet al compilar el proyecto, puede usar la `RestoreSources` propiedad con la ruta de acceso a la carpeta **NuGet** en la solución de componentes.

    ```xml
    <PropertyGroup>
      <RestoreSources>
          https://api.nuget.org/v3/index.json;
          ../../CppWinRTProjectionSample/SimpleMathProjection/nuget
      </RestoreSources>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.VCRTForwarders.140" Version="1.0.6" />
        <PackageReference Include="SimpleMathComponent" Version="0.1.0-prerelease" />
    </ItemGroup>
    ```

    Tenga en cuenta que para este tutorial, la ruta de acceso de restauración de NuGet para **SimpleMathComponent** supone que ambos archivos de solución están en el mismo directorio. Como alternativa, puede [Agregar una fuente de paquetes de NuGet local](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources) a la solución.

4. Edite el archivo **Program.CS** para usar la funcionalidad proporcionada por **SimpleMathComponent**.

    ```csharp
    static void Main(string[] args)
    {
        var x = new SimpleMathComponent.SimpleMath();
        Console.WriteLine("Adding 5.5 + 6.5 ...");
        Console.WriteLine(x.add(5.5, 6.5).ToString());
    }
    ```

5. Compilar y ejecutar la aplicación de consola. Debería ver la salida siguiente.

    ![Salida de NET5 de consola](images/console-output.png)

## <a name="resources"></a>Recursos

- [Ejemplo de código completo para este tutorial](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)