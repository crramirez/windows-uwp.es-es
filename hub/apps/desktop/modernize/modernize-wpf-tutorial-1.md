---
description: Este tutorial muestra cómo agregar interfaces de usuario de UWP XAML y crear paquetes MSIX para incorporar otros componentes modernos en su aplicación WPF.
title: Migración de la aplicación Contoso Expenses a .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, formularios windows forms, wpf, Islas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6e303e7059edd72fcdeb5455f450e6ece9d58e02
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141848"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Parte 1: Migración de la aplicación Contoso Expenses a .NET Core 3

Se trata de la primera parte de un tutorial que muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo denominada Contoso gastos. Para obtener información general del tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, vea [Tutorial: Modernizar una aplicación de WPF](modernize-wpf-tutorial.md).
  
En esta parte del tutorial, migrará toda la aplicación de gastos de Contoso desde .NET Framework 4.7.2 a [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Antes de iniciar esta parte del tutorial, asegúrese de haga lo siguiente:

* [Abra y compile el ejemplo ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) en Visual Studio de 2019.
* Si usa una versión de lanzamiento de Visual Studio de 2019, habilite las versiones preliminares de .NET Core SDK. En Visual Studio, vaya a **Herramientas > opciones**, escriba "Vista previa" en el cuadro de búsqueda y seleccione **utilice las vistas previas de .NET Core SDK**. Si usas un [versión preliminar de Visual Studio de 2019](https://visualstudio.microsoft.com/vs/preview/), no es necesario seleccionar esta opción porque las versiones preliminares de .NET Core están habilitadas de forma predeterminada.

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migrar el proyecto ContosoExpenses a .NET Core 3

En esta sección, podrá migrar el proyecto ContosoExpenses en la aplicación de gastos de Contoso para .NET Core 3. Para ello, deberá crear un nuevo archivo de proyecto que contiene los mismos archivos que el proyecto ContosoExpenses existente pero destinos de .NET Core 3 en lugar de .NET Framework 4.7.2. Esto le permite mantener una única solución con las versiones de .NET Framework y .NET Core de la aplicación.

1. Compruebe que el ContosoExpenses actualmente proyecto destinada a .NET Framework 4.7.2. En el Explorador de soluciones, haga clic en el **ContosoExpenses** del proyecto, elija **propiedades**y confirme que la **.NET framework de destino** propiedad en el  **Aplicación** ficha se establece en .NET Framework 4.7.2.

    ![La versión 4.7.2 para el proyecto de .NET framework](images/wpf-modernize-tutorial/NETFramework472.png)

3. En el Explorador de Windows, navegue hasta la **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** carpeta y cree un nuevo archivo de texto denominado **ContosoExpenses.Core.csproj**.

4. Haga clic con el botón derecho en el archivo, elija **abrir con**y, a continuación, ábralo en un editor de texto que prefiera, como el Bloc de notas, Visual Studio Code o Visual Studio.

5. Copie el texto siguiente en el archivo y guárdelo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Cierre el archivo y volver a la **ContosoExpenses** solución en Visual Studio.

7. Haga clic en el **ContosoExpenses** solución y elija **Agregar -> proyecto existente**. Seleccione el **ContosoExpenses.Core.csproj** de archivo que acaba de crear en el `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` carpeta para agregarlo a la solución.

El **ContosoExpenses.Core.csproj** incluye los siguientes elementos:

* El **proyecto** elemento especifica una versión del SDK de **Microsoft.NET.Sdk.WindowsDesktop**. Esto se hace referencia a las aplicaciones de .NET para escritorio de Windows, e incluye componentes para aplicaciones de Windows Forms y WPF.
* El **PropertyGroup** elemento contiene elementos secundarios que indican el resultado del proyecto es un archivo ejecutable (no una DLL), tiene como destino .NET Core 3 y usa WPF. Para una aplicación de Windows Forms, se usaría un **UseWinForms** elemento en lugar de la **UseWPF** elemento.

> [!NOTE]
> Cuando se trabaja con el formato .csproj precedido con .NET Core 3.0, todos los archivos en la misma carpeta que el archivo .csproj se consideran parte del proyecto. Por lo tanto, no debe especificar todos los archivos incluidos en el proyecto. Debe especificar sólo los archivos para el que desea definir una acción de compilación personalizada o que va a excluir.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migrar el proyecto ContosoExpenses.Data a .NET Standard

El **ContosoExpenses** solución incluye un **ContosoExpenses.Data** biblioteca de clases que contiene las interfaces para los servicios y los modelos y tiene como destino .NET 4.7.2. Las aplicaciones de .NET core 3.0 pueden usar las bibliotecas de .NET Framework, siempre y cuando no usan las API que no están disponibles en .NET Core. Sin embargo, la mejor ruta de acceso de modernización es mover las bibliotecas para .NET Standard. Esto asegurará que la biblioteca es totalmente compatible con la aplicación de .NET Core 3.0. Además, puede volver a usar la biblioteca también con otras plataformas, como web (a través de ASP.NET Core) y mobile (a través de Xamarin).

Para migrar el **ContosoExpenses.Data** proyecto a .NET Standard:

1. En Visual Studio, haga clic en el **ContosoExpenses.Data** proyecto y elija **descargar el proyecto**. Haga clic en el proyecto nuevo y, a continuación, elija **ContosoExpenses.Data.csproj editar**.

2. Eliminar todo el contenido del archivo del proyecto.

3. Copie y pegue el siguiente código XML y guarde el archivo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Haga clic en el **ContosoExpenses.Data** proyecto y elija **recargar el proyecto**.

## <a name="configure-nuget-packages-and-dependencies"></a>Configurar las dependencias y los paquetes de NuGet

Cuando se ha migrado el **ContosoExpenses.Core** y **ContosoExpenses.Data** proyectos en las secciones anteriores, ha quitado las referencias del paquete NuGet de los proyectos. En esta sección, agregará estas referencias de vuelta.

Para configurar los paquetes de NuGet para la **ContosoExpenses.Data** proyecto:

1.  En el **ContosoExpenses.Data** del proyecto, expanda el **dependencias** nodo. Tenga en cuenta que el **NuGet** falta sección.

    ![Paquetes de NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    Si abre el **Packages.config** en el **el Explorador de soluciones** encontrará las referencias de los paquetes de NuGet 'old' utilizan el proyecto al que estaba utilizando la versión completa de .NET Framework.

    ![Paquetes y dependencias](images/wpf-modernize-tutorial/Packages.png)

    Este es el contenido de la **Packages.config** archivo. Observará que todos los paquetes de NuGet como destino la versión completa de .NET Framework 4.7.2:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. En el **ContosoExpenses.Data** del proyecto, elimine el **Packages.config** archivo.

4. En el **ContosoExpenses.Data** del proyecto, haga clic en el **dependencias** nodo y elija **administrar paquetes de NuGet**.

  ![Administrar paquetes NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. En el **Administrador de paquetes de NuGet** ventana, haga clic en **examinar**. Busque el `Bogus` paquete e instalarlo.

    ![Paquete de NuGet fantasma](images/wpf-modernize-tutorial/Bogus.png)

6. Busque el `LiteDB` paquete e instalarlo.

    ![Paquete de LiteDB NuGet](images/wpf-modernize-tutorial/LiteDB.png)

    Quizás se pregunte donde se almacena estos lista de paquetes de NuGet, puesto que el proyecto ya no tiene un archivo packages.config. Los paquetes de NuGet que se hace referencia se almacenan directamente en el archivo .csproj. Para comprobarlo, ver el contenido de la **ContosoExpenses.Data.csproj** archivo de proyecto en un editor de texto. Encontrará las siguientes líneas que se agrega al final del archivo:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > También es posible que tenga en cuenta que va a instalar los mismos paquetes para este proyecto de .NET Core 3 como los que se usan los proyectos de .NET Framework 4.7.2. Paquetes de NuGet es compatible con múltiples versiones. Los creadores de bibliotecas pueden incluir diferentes versiones de una biblioteca en el mismo paquete, compilado para las plataformas y arquitecturas diferentes. Estos paquetes admiten la versión completa de .NET Framework, así como .NET Standard 2.0, que es compatible con proyectos de .NET Core 3. Para obtener más información acerca de las diferencias de .NET Framework, .NET Core y .NET Standard, consulte [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Para configurar los paquetes de NuGet para la **ContosoExpenses.Core** proyecto:

1. En el **ContosoExpenses.Core** proyecto, abra el **packages.config** archivo. Tenga en cuenta que actualmente contiene las siguientes referencias que tienen como destino .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    En los pasos siguientes obtendrá las versiones de .NET Standard el `MvvmLightLibs` y `Unity` paquetes. Los otros dos son las dependencias descargadas automáticamente mediante NuGet cuando se instalación estas dos bibliotecas.

2. En el **ContosoExpenses.Core** del proyecto, elimine el **Packages.config** archivo.

3. Haga clic en el **ContosoExpenses.Core** proyecto y elija **administrar paquetes de NuGet**.

4. En el **Administrador de paquetes de NuGet** ventana, haga clic en **examinar**. Busque el `Unity` paquete e instalarlo.

    ![Paquete de Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. Busque el `MvvmLightLibsStd10` paquete e instalarlo. Esta es la versión de .NET Standard el `MvvmLightLibs` paquete. Este paquete, el autor ha elegido empaquetar la versión de .NET Standard de la biblioteca en un paquete independiente de la versión de .NET Framework.

    ![Paquete MvvmLightsLibs](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. En el **ContosoExpenses.Core** del proyecto, haga clic en el **dependencias** nodo y elija **Agregar referencia**.

7. En el **proyectos > solución** categoría, seleccione **ContosoExpenses.Data** y haga clic en **Aceptar**.

    ![Agregar referencia](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Desactivar los atributos de ensamblado generado automáticamente

En este punto del proceso de migración, si intenta compilar el **ContosoExpenses.Core** proyecto, verá algunos errores.

![Nuevos errores de compilación de .NET core 3](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Este problema ocurre porque el nuevo formato .csproj introdujo con .NET Core 3.0 almacena la información de ensamblado en el archivo de proyecto en lugar de **AssemblyInfo.cs** archivo. Para corregir estos errores, deshabilitar este comportamiento y permitir que el proyecto a seguir usando la **AssemblyInfo.cs** archivo.

1. En Visual Studio, haga clic en el **ContosoExpenses.Core** proyecto y elija **descargar el proyecto**. Haga clic en el proyecto nuevo y, a continuación, elija **ContosoExpenses.Core.csproj editar**.

1. Agregue el siguiente elemento en el **PropertyGroup** sección y guarde el archivo.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Después de agregar este elemento, el **PropertyGroup** sección ahora debería parecerse a esto:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Haga clic en el **ContosoExpenses.Core** proyecto y elija **recargar el proyecto**.

4. Haga clic en el **ContosoExpenses.Data** proyecto y elija **descargar el proyecto**. Haga clic en el proyecto nuevo y, a continuación, elija **ContosoExpenses.Data.csproj editar**.

5. Agregar la misma entrada de la **PropertyGroup** sección y guarde el archivo.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Después de agregar este elemento, el **PropertyGroup** sección ahora debería parecerse a esto:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Haga clic en el **ContosoExpenses.Data** proyecto y elija **recargar el proyecto**.

## <a name="add-the-windows-compatibility-pack"></a>Agregue el paquete de compatibilidad de Windows

Si ahora intenta compilar el **ContosoExpenses.Core** y **ContosoExpenses.Data** proyectos, verá que ahora se solucionen los errores anteriores, pero todavía hay algunos errores en el  **ContosoExpenses.Data** biblioteca similar a éstos.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Estos errores son el resultado de convertir el **ContosoExpenses.Data** proyecto desde una biblioteca de .NET Framework (que es específica de Windows) en una biblioteca .NET Standard, que puede ejecutar en varias plataformas como Linux, Android, iOS, y mucho más. El **ContosoExpenses.Data** proyecto contiene una clase denominada **RegistryService**, que interactúa con el registro, un concepto de Windows.

Para resolver estos errores, instale el [compatibilidad de Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) paquete NuGet. Este paquete proporciona compatibilidad con muchas API específicas de Windows que se usará en una biblioteca .NET Standard. La biblioteca ya no será multiplataforma después de usar este paquete, pero todavía destino .NET Standard. 

1. Haga doble clic en el **ContosoExpenses.Data** proyecto.
2. Elija **administrar paquetes de NuGet**.
3. En el **Administrador de paquetes de NuGet** ventana, haga clic en **examinar**. Busque el `Microsoft.Windows.Compatibility` paquete e instalarlo. 

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Ahora, vuelva a intentarlo compilar el proyecto, por ello, haga clic en el **ContosoExpenses.Data** proyecto y elegir **compilar**.

Esta vez el proceso de compilación se completará sin errores.

## <a name="test-and-debug-the-migration"></a>Probar y depurar la migración

Ahora que los proyectos son compilar correctamente, está listo para ejecutar y probar la aplicación para ver si hay errores en tiempo de ejecución.

1. Haga clic en el **ContosoExpenses.Core** proyecto y elija **establecer como proyecto de inicio**.

2. Presione F5 para iniciar el **ContosoExpenses.Core** proyecto en el depurador. Verá una excepción similar al siguiente.

    ![Excepción que se muestra en Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Se provoca esta excepción debido a que cuando se elimina el contenido del archivo .csproj al principio de la migración, quita la información sobre la **acción de compilación** para los archivos de imagen. Los pasos siguientes corrección este problema.

3. Detenga al depurador.

4. Haga clic en el **ContosoExpenses.Core** proyecto y elija **descargar el proyecto**. Haga clic en el proyecto nuevo y, a continuación, elija **ContosoExpenses.Core.csproj editar**.

5. Antes del cierre **proyecto** elemento, agregue la siguiente entrada:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Haga clic en el **ContosoExpenses.Core** proyecto y elija **recargar el proyecto**.

7. Para asignar el Contoso.ico a la aplicación, haga clic en el **ContosoExpenses.Core** proyecto y elija **propiedades**. En la página abierta, haga clic en la lista desplegable bajo **icono** y seleccione `Images\contoso.ico`.

    ![Icono de Contoso en las propiedades del proyecto](images/wpf-modernize-tutorial/ContosoIco.png)

8. Haga clic en **Guardar**.

9. Presione F5 para iniciar el **ContosoExpenses.Core** proyecto en el depurador. Confirme que la aplicación ahora se ejecuta.

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, ha migrado correctamente la aplicación de gastos de Contoso para .NET Core 3. Ahora está listo para [parte 2: Agregar un control de UWP InkCanvas mediante XAML Islas](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Si tiene una pantalla de alta resolución, es posible que tenga en cuenta que la aplicación parece muy pequeña. Abordaré este problema en el paso siguiente del tutorial.
