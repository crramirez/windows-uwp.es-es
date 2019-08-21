---
description: En este tutorial se muestra cómo agregar interfaces de usuario XAML de UWP, crear paquetes de MSIX e incorporar otros componentes modernos en la aplicación WPF.
title: Migración de la aplicación Contoso Expenses a .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, Islas XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 97488a913605916c067861b5941d7aa127b00917
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643410"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Parte 1: Migración de la aplicación Contoso Expenses a .NET Core 3

Esta es la primera parte de un tutorial que muestra cómo modernizar una aplicación de escritorio WPF de ejemplo denominada gastos de contoso. Para obtener información general sobre el tutorial, los requisitos previos y las instrucciones para descargar la aplicación de [ejemplo, vea Tutorial: Modernizar una aplicación](modernize-wpf-tutorial.md)de WPF.
  
En esta parte del tutorial, migrará toda la aplicación de gastos de Contoso del .NET Framework 4.7.2 a [.net Core 3](modernize-wpf-tutorial.md#net-core-3). Antes de comenzar esta parte del tutorial, asegúrese de hacer lo siguiente:

* [Abra y compile el ejemplo ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) en Visual Studio 2019.
* Si usa una versión de lanzamiento de Visual Studio 2019, habilite las versiones preliminares del SDK de .NET Core. En Visual Studio, vaya a **herramientas > opciones**, escriba "vista previa" en el cuadro de búsqueda y seleccione **usar vistas previas del SDK de .net Core**. Si usa una [versión preliminar de Visual Studio 2019](https://visualstudio.microsoft.com/vs/preview/), no es necesario que seleccione esta opción, ya que las vistas previas de .net Core están habilitadas de forma predeterminada.

> [!NOTE]
> Para obtener más información sobre cómo migrar una aplicación WPF desde el .NET Framework a .NET Core 3, vea [esta serie de blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migración del proyecto ContosoExpenses a .NET Core 3

En esta sección, migrará el proyecto ContosoExpenses de la aplicación de gastos de Contoso a .NET Core 3. Para ello, debe crear un nuevo archivo de proyecto que contenga los mismos archivos que el proyecto de ContosoExpenses existente, pero tiene como destino .NET Core 3 en lugar de la .NET Framework 4.7.2. Esto le permite mantener una única solución con las versiones de .NET Framework y .NET Core de la aplicación.

1. Compruebe que el proyecto ContosoExpenses tiene como destino actualmente el .NET Framework 4.7.2. En Explorador de soluciones, haga clic con el botón derecho en el proyecto **ContosoExpenses** , elija **propiedades**y confirme que la propiedad **plataforma de destino** de la pestaña **aplicación** está establecida en el .NET Framework 4.7.2.

    ![.NET Framework de la versión 4.7.2 del proyecto](images/wpf-modernize-tutorial/NETFramework472.png)

3. En el explorador de Windows, vaya a la carpeta **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** y cree un nuevo archivo de texto denominado **ContosoExpenses. Core. csproj**.

4. Haga clic con el botón derecho en el archivo, elija **abrir con**y, a continuación, ábralo en un editor de texto de su elección, como el Bloc de notas, Visual Studio Code o Visual Studio.

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

6. Cierre el archivo y vuelva a la solución **ContosoExpenses** en Visual Studio.

7. Haga clic con el botón derecho en la solución **ContosoExpenses** y elija **Agregar > proyecto existente**. Seleccione el archivo **ContosoExpenses. Core. csproj** que acaba de crear en `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` la carpeta para agregarlo a la solución.

**ContosoExpenses. Core. csproj** incluye los siguientes elementos:

* El elemento **Project** especifica una versión del SDK de **Microsoft. net. SDK. WindowsDesktop**. Esto hace referencia a las aplicaciones .NET para escritorio de Windows e incluye componentes para las aplicaciones de WPF y Windows Forms.
* El elemento **PropertyGroup** contiene elementos secundarios que indican que el resultado del proyecto es un ejecutable (no un archivo dll), tiene como destino .net Core 3 y usa WPF. En el caso de una aplicación Windows Forms, usaría un elemento **UseWinForms** en lugar del elemento **UseWPF** .

> [!NOTE]
> Cuando se trabaja con el formato. csproj introducido con .NET Core 3,0, todos los archivos de la misma carpeta que el archivo. csproj se consideran parte del proyecto. Por lo tanto, no tiene que especificar todos los archivos incluidos en el proyecto. Debe especificar solo los archivos para los que desea definir una acción de compilación personalizada o que desee excluir.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migre el proyecto ContosoExpenses. Data a .NET Standard

La solución **ContosoExpenses** incluye una biblioteca de clases de **ContosoExpenses. Data** que contiene modelos e interfaces para servicios y tiene como destino .net 4.7.2. Las aplicaciones de .NET Core 3,0 pueden usar bibliotecas de .NET Framework, siempre y cuando no utilicen las API que no están disponibles en .NET Core. Sin embargo, la mejor ruta de modernización consiste en trasladar las bibliotecas a .NET Standard. Esto garantizará que la aplicación de .NET Core 3,0 admite totalmente la biblioteca. Además, también puede volver a usar la biblioteca con otras plataformas, como Web (a través de ASP.NET Core) y móviles (a través de Xamarin).

Para migrar el proyecto **ContosoExpenses. Data** a .net Standard:

1. En Visual Studio, haga clic con el botón derecho en el proyecto **ContosoExpenses. Data** y elija **descargar el proyecto**. Vuelva a hacer clic con el botón derecho en el proyecto y elija **Editar ContosoExpenses. Data. csproj**.

2. Elimine todo el contenido del archivo de proyecto.

3. Copie y pegue el siguiente código XML y guarde el archivo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Data** y elija **volver a cargar el proyecto**.

## <a name="configure-nuget-packages-and-dependencies"></a>Configuración de paquetes y dependencias de NuGet

Al migrar los proyectos **ContosoExpenses. Core** y **ContosoExpenses. Data** en las secciones anteriores, quitó las referencias del paquete NuGet de los proyectos. En esta sección, volverá a agregar estas referencias.

Para configurar paquetes NuGet para el proyecto **ContosoExpenses. Data** :

1. En el proyecto **ContosoExpenses. Data** , expanda el nodo dependencias. Tenga en cuenta que falta la sección **NuGet** .

    ![Paquetes NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    Si abre el **archivo packages. config** en el **Explorador de soluciones** encontrará las referencias ' antiguas ' de los paquetes NuGet que usó el proyecto cuando usaba el .NET Framework completo.

    ![Dependencias y paquetes](images/wpf-modernize-tutorial/Packages.png)

    Este es el contenido del archivo **packages. config** . Observará que todos los paquetes NuGet tienen como destino el .NET Framework 4.7.2 completo:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. En el proyecto **ContosoExpenses. Data** , elimine el archivo **packages. config** .

4. En el proyecto **ContosoExpenses. Data** , haga clic con el botón derecho en el nodo dependencias y elija **administrar paquetes NuGet**.

  ![Administrar paquetes de NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. En la ventana **Administrador de paquetes NuGet** , haga clic en **examinar**. Busque el `Bogus` paquete e instale la versión estable más reciente.

    ![Paquete NuGet ficticio](images/wpf-modernize-tutorial/Bogus.png)

6. Busque el `LiteDB` paquete e instale la versión estable más reciente.

    ![Paquete NuGet de LiteDB](images/wpf-modernize-tutorial/LiteDB.png)

    Es posible que se pregunte dónde se almacena esta lista de paquetes NuGet, ya que el proyecto ya no tiene un archivo packages. config. Los paquetes NuGet a los que se hace referencia se almacenan directamente en el archivo. csproj. Para comprobarlo, consulte el contenido del archivo de proyecto **ContosoExpenses. Data. csproj** en un editor de texto. Encontrará las siguientes líneas agregadas al final del archivo:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > También puede observar que está instalando los mismos paquetes para este proyecto de .NET Core 3 que los que usan los proyectos de .NET Framework 4.7.2. Los paquetes NuGet admiten la compatibilidad con múltiples versiones. Los autores de bibliotecas pueden incluir diferentes versiones de una biblioteca en el mismo paquete, compiladas para distintas arquitecturas y plataformas. Estos paquetes admiten la .NET Framework completa y .NET Standard 2,0, que es compatible con proyectos de .NET Core 3. Para obtener más información sobre las diferencias .NET Framework, .NET Core y .NET Standard, vea [.net Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Para configurar paquetes NuGet para el proyecto **ContosoExpenses. Core** :

1. En el proyecto **ContosoExpenses. Core** , abra el archivo **packages. config** . Observe que actualmente contiene las siguientes referencias que tienen como destino el .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    En los pasos siguientes, .net Standard las versiones de los `MvvmLightLibs` paquetes `Unity` y. Los otros dos son dependencias que NuGet descarga automáticamente al instalar estas dos bibliotecas.

2. En el proyecto **ContosoExpenses. Core** , elimine el archivo **packages. config** .

3. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Core** y elija **administrar paquetes NuGet**.

4. En la ventana **Administrador de paquetes NuGet** , haga clic en **examinar**. Busque el `Unity` paquete e instale la versión estable más reciente.

    ![Paquete de Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. Busque el `MvvmLightLibsStd10` paquete e instale la versión estable más reciente. Esta es la versión de .net Standard del `MvvmLightLibs` paquete. Para este paquete, el autor eligió empaquetar la versión .NET Standard de la biblioteca en un paquete independiente que la versión .NET Framework.

    ![Paquete MvvmLightsLibs](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. En el proyecto **ContosoExpenses. Core** , haga clic con el botón derecho en el nodo dependencias y elija **Agregar referencia**.

7. En la categoría **proyectos > solución** , seleccione **ContosoExpenses. Data** y haga clic en **Aceptar**.

    ![Agregar referencia](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Deshabilitar atributos de ensamblado generados automáticamente

En este punto del proceso de migración, si intenta compilar el proyecto **ContosoExpenses. Core** , verá algunos errores.

![Errores de compilación de .NET Core 3](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Este problema se produce porque el nuevo formato. csproj introducido con .NET Core 3,0 almacena la información de ensamblado en el archivo de proyecto en lugar del archivo **AssemblyInfo.CS** . Para corregir estos errores, deshabilite este comportamiento y deje que el proyecto siga usando el archivo **AssemblyInfo.CS** .

1. En Visual Studio, haga clic con el botón derecho en el proyecto **ContosoExpenses. Core** y elija **descargar el proyecto**. Vuelva a hacer clic con el botón derecho en el proyecto y, a continuación, elija **Editar ContosoExpenses. Core. csproj**.

1. Agregue el siguiente elemento en la sección **PropertyGroup** y guarde el archivo.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Después de agregar este elemento, la sección **PropertyGroup** debe tener el siguiente aspecto:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Core** y elija **volver a cargar el proyecto**.

4. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Data** y elija **descargar el proyecto**. Vuelva a hacer clic con el botón derecho en el proyecto y elija **Editar ContosoExpenses. Data. csproj**.

5. Agregue la misma entrada en la sección **PropertyGroup** y guarde el archivo.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Después de agregar este elemento, la sección **PropertyGroup** debe tener el siguiente aspecto:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Data** y elija **volver a cargar el proyecto**.

## <a name="add-the-windows-compatibility-pack"></a>Agregar el paquete de compatibilidad de Windows

Si ahora intenta compilar los proyectos **ContosoExpenses. Core** y **ContosoExpenses. Data** , verá que los errores anteriores ahora se han corregido, pero todavía hay algunos errores en la biblioteca **ContosoExpenses. Data** , similares a estos.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Estos errores son el resultado de la conversión del proyecto **ContosoExpenses. Data** de una biblioteca .NET Framework (que es específica de Windows) a una biblioteca .net Standard, que se puede ejecutar en varias plataformas, como Linux, Android, iOS, etc. El proyecto **ContosoExpenses. Data** contiene una clase denominada **RegistryService**, que interactúa con el registro, un concepto solo de Windows.

Para resolver estos errores, instale el paquete NuGet de [compatibilidad de Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) . Este paquete proporciona compatibilidad con muchas API específicas de Windows que se usarán en una biblioteca de .NET Standard. La biblioteca ya no será multiplataforma después de usar este paquete, pero seguirá teniendo como destino .NET Standard. 

1. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Data** .
2. Elija **administrar paquetes NuGet**.
3. En la ventana **Administrador de paquetes NuGet** , haga clic en **examinar**. Busque el `Microsoft.Windows.Compatibility` paquete e instale la versión estable más reciente.

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Ahora intente compilar el proyecto de nuevo; para ello, haga clic con el botón derechoen el proyecto **ContosoExpenses. Data** y elija compilar.

Esta vez el proceso de compilación se completará sin errores.

## <a name="test-and-debug-the-migration"></a>Prueba y depuración de la migración

Ahora que los proyectos se compilan correctamente, está listo para ejecutar y probar la aplicación para ver si hay errores en tiempo de ejecución.

1. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Core** y elija **establecer como proyecto de inicio**.

2. Presione F5 para iniciar el proyecto **ContosoExpenses. Core** en el depurador. Verá una excepción similar a la siguiente.

    ![Excepción mostrada en Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Esta excepción se produce porque cuando eliminó el contenido del archivo. csproj al principio de la migración, quitó la información sobre la **acción de compilación** para los archivos de imagen. En los pasos siguientes se soluciona este problema.

3. Detenga el depurador.

4. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Core** y elija **descargar el proyecto**. Vuelva a hacer clic con el botón derecho en el proyecto y, a continuación, elija **Editar ContosoExpenses. Core. csproj**.

5. Antes del elemento de **proyecto** de cierre, agregue la entrada siguiente:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Core** y elija **volver a cargar el proyecto**.

7. Para asignar el archivo contoso. ICO a la aplicación, haga clic con el botón derecho en el proyecto **ContosoExpenses. Core** y elija **propiedades**. En la página abierta, haga clic en la lista desplegable en `Images\contoso.ico`icono y seleccione.

    ![Icono de Contoso en las propiedades del proyecto](images/wpf-modernize-tutorial/ContosoIco.png)

8. Haga clic en **Guardar**.

9. Presione F5 para iniciar el proyecto **ContosoExpenses. Core** en el depurador. Confirme que la aplicación se ejecuta ahora.

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, ha migrado correctamente la aplicación de gastos de Contoso a .NET Core 3. Ya está listo para [la parte 2: Agregue un control InkCanvas de UWP con islas](modernize-wpf-tutorial-2.md)XAML.

> [!NOTE]
> Si tiene una pantalla de alta resolución, puede observar que la aplicación tiene un aspecto muy pequeño. Solucionará este problema en el siguiente paso del tutorial.
