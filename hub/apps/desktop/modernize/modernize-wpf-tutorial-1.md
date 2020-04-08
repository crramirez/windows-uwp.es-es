---
description: En este tutorial se muestra cómo agregar interfaces de usuario de XAML en UWP, crear paquetes MSIX e incorporar otros componentes actuales en la aplicación de WPF.
title: Migración de la aplicación Contoso Expenses a .NET Core 3
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 6a52e12f9d60ee4abb4b1aed3043a69c25845267
ms.sourcegitcommit: f34deba1d4460d85ed08fe9648999fe03ff6a3dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317108"
---
# <a name="part-1-migrate-the-contoso-expenses-app-to-net-core-3"></a>Parte 1: Migración de la aplicación Contoso Expenses a .NET Core 3

Esta es la primera parte de un tutorial que muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo denominada Contoso Expenses. Para obtener información general sobre el tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, consulta [Tutorial: Modernización de una aplicación WPF](modernize-wpf-tutorial.md).
  
En esta parte del tutorial, migrarás toda la aplicación de gastos de Contoso desde .NET Framework 4.7.2 a [.NET Core 3](modernize-wpf-tutorial.md#net-core-3). Antes de iniciar esta parte del tutorial, asegúrate de [abrir y compilar el ejemplo ContosoExpenses](modernize-wpf-tutorial.md#get-the-contoso-expenses-sample-app) en Visual Studio 2019.

> [!NOTE]
> Para obtener más información sobre cómo migrar una aplicación WPF desde .NET Framework a .NET Core 3, consulta [esta serie de blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

## <a name="migrate-the-contosoexpenses-project-to-net-core-3"></a>Migración del proyecto Contoso Expenses a .NET Core 3

En esta sección, migrarás el proyecto ContosoExpenses de la aplicación Contoso Expenses a .NET Core 3. Para ello, debes crear un nuevo archivo de proyecto que contenga los mismos archivos que el proyecto de ContosoExpenses existente, pero que tenga como destino .NET Core 3 en lugar de .NET Framework 4.7.2. Esto te permite mantener una única solución con las versiones de .NET Framework y .NET Core de la aplicación.

1. Comprueba que el proyecto ContosoExpenses tiene como destino actualmente .NET Framework 4.7.2. En el Explorador de soluciones, haz clic con el botón derecho en el proyecto **ContosoExpenses**, elige **Propiedades** y confirma que la propiedad **Marco de destino** de la pestaña **Aplicación** está establecida en .NET Framework 4.7.2.

    ![Versión 4.7.2 de .NET Framework para el proyecto](images/wpf-modernize-tutorial/NETFramework472.png)

3. En el Explorador de Windows, navega hasta la carpeta **C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses** y crea un nuevo archivo de texto denominado **ContosoExpenses.Core.csproj**.

4. Haz clic con el botón derecho en el archivo, elige **Abrir con** y, después, ábrelo en un editor de texto que prefieras, como el Bloc de notas, Visual Studio Code o Visual Studio.

5. Copia el texto siguiente en el archivo y guárdalo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.0</TargetFramework>
        <UseWPF>true</UseWPF>
     </PropertyGroup>

    </Project>
    ```

6. Cierra el archivo y vuelve a la solución **ContosoExpenses** en Visual Studio.

7. Haz clic con el botón derecho en la solución **ContosoExpenses** y elige **Agregar > Proyecto existente**. Selecciona el archivo **ContosoExpenses.Core.csproj** que acabas de crear en la carpeta `C:\WinAppsModernizationWorkshop\Lab\Exercise1\01-Start\ContosoExpenses` para agregarlo a la solución.

**ContosoExpenses.Core.csproj** incluye los siguientes elementos:

* El elemento **Project** especifica una versión del SDK de **Microsoft.NET.Sdk.WindowsDesktop**. Esto hace referencia a las aplicaciones .NET para Escritorio de Windows e incluye componentes para las aplicaciones de WPF y Windows Forms.
* El elemento **PropertyGroup** contiene elementos secundarios que indican que el resultado del proyecto es un ejecutable (no un archivo DLL), tiene como destino .NET Core 3 y usa WPF. En el caso de una aplicación de Windows Forms, usarías un elemento **UseWinForms** en lugar del elemento **UseWPF**.

> [!NOTE]
> Cuando se trabaja con el formato. csproj introducido con .NET Core 3,0, todos los archivos de la misma carpeta que el archivo. csproj se consideran parte del proyecto. Por lo tanto, no tienes que especificar todos los archivos incluidos en el proyecto. Debes especificar solo los archivos para los que deseas definir una acción de compilación personalizada o que desees excluir.

## <a name="migrate-the-contosoexpensesdata-project-to-net-standard"></a>Migración del proyecto ContosoExpenses.Data a .NET Standard

La solución **ContosoExpenses** incluye una biblioteca de clases **ContosoExpenses.Data** que contiene modelos e interfaces para servicios y tiene como destino .NET 4.7.2. Las aplicaciones de .NET Core 3.0 pueden usar bibliotecas de .NET Framework, siempre y cuando no utilicen las API que no están disponibles en .NET Core. Sin embargo, la mejor ruta de modernización consiste en trasladar las bibliotecas a .NET Standard. De este modo, tendrás seguro que la biblioteca es plenamente compatible con tu aplicación de .NET Core 3.0. Además, también puedes volver a usar la biblioteca con otras plataformas, como la Web (a través de ASP.NET Core) y móviles (a través de Xamarin).

Para migrar el proyecto **ContosoExpenses.Data** a .NET Standard:

1. En Visual Studio, haz clic con el botón derecho en el proyecto **ContosoExpenses.Data** y elige **Descargar el proyecto**. Vuelve a hacer clic con el botón derecho en el proyecto y, a continuación, elige la opción para **editar ContosoExpenses.Data.csproj**.

2. Elimina todo el contenido del archivo de proyecto.

3. Copia y pega el siguiente XML y guarda el archivo.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">

      <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
      </PropertyGroup>

    </Project>
    ```

4. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Data** y elige **Volver a cargar el proyecto**.

## <a name="configure-nuget-packages-and-dependencies"></a>Configuración de paquetes y dependencias NuGet

Al migrar los proyectos **ContosoExpenses.Core** y **ContosoExpenses.Data** en las secciones anteriores, quitaste las referencias del paquete NuGet de los proyectos. En esta sección, volverás a agregar estas referencias.

Para configurar paquetes NuGet para el proyecto **ContosoExpenses.Data**:

1. En el proyecto **ContosoExpenses.Data**, expande el nodo **Dependencias**. Observa que falta la sección **NuGet**.

    ![Paquetes NuGet](images/wpf-modernize-tutorial/NuGetPackages.png)

    Si abres **Packages.config** en el **Explorador de soluciones** encontrarás las referencias "antiguas" de los paquetes NuGet que usó el proyecto cuando usaba el .NET Framework completo.

    ![Dependencias y paquetes](images/wpf-modernize-tutorial/Packages.png)

    Este es el contenido del archivo **packages.config**. Observarás que todos los paquetes NuGet tienen como destino .NET Framework 4.7.2 completo:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="Bogus" version="26.0.2" targetFramework="net472" />
      <package id="LiteDB" version="4.1.4" targetFramework="net472" />
    </packages>
    ```

2. En el proyecto **ContosoExpenses.Data**, elimina el archivo **packages.config**.

4. En el proyecto **ContosoExpenses.Data**, haz clic con el botón derecho en el nodo **Dependencias** y elige **Administrar paquetes NuGet**.

  ![Administrar paquetes NuGet...](images/wpf-modernize-tutorial/ManageNugetNETCORE3.png)

5. En la ventana **Administrador de paquetes NuGet**, haz clic en **Examinar**. Busca el paquete `Bogus` e instala la versión estable más reciente.

    ![Paquete NuGet ficticio](images/wpf-modernize-tutorial/Bogus.png)

6. Busca el paquete `LiteDB` e instala la versión estable más reciente.

    ![Paquete NuGet LiteDB](images/wpf-modernize-tutorial/LiteDB.png)

    Es posible que te preguntes dónde se almacena esta lista de paquetes NuGet, ya que el proyecto ya no tiene un archivo packages.config. Los paquetes NuGet a los que se hace referencia se almacenan directamente en el archivo. csproj. Para comprobarlo, consulta el contenido del archivo de proyecto **ContosoExpenses.Data.csproj** en un editor de texto. Encontrarás las siguientes líneas agregadas al final del archivo:

    ```xml
    <ItemGroup>
       <PackageReference Include="Bogus" Version="26.0.2" />
       <PackageReference Include="LiteDB" Version="4.1.4" />
    </ItemGroup>
    ```

    > [!NOTE]
    > También puedes observar que estás instalando los mismos paquetes para este proyecto de .NET Core 3 que los que usan los proyectos de .NET Framework 4.7.2. Los paquetes NuGet admiten destinos múltiples. Los autores de bibliotecas pueden incluir diferentes versiones de una biblioteca en el mismo paquete, compiladas para distintas arquitecturas y plataformas. Estos paquetes admiten .NET Framework completo y .NET Standard 2.0, que es compatible con proyectos de .NET Core 3. Para obtener más información sobre las diferencias entre .NET Framework, .NET Core y .NET Standard, consulta [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard).

Para configurar paquetes NuGet para el proyecto **ContosoExpenses.Core**:

1. En el proyecto **ContosoExpenses.Core**, abre el archivo **packages.config**. Observa que actualmente contiene las siguientes referencias que tienen como destino .NET Framework 4.7.2.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <packages>
      <package id="CommonServiceLocator" version="2.0.2" targetFramework="net472" />
      <package id="MvvmLightLibs" version="5.4.1.1" targetFramework="net472" />
      <package id="System.Runtime.CompilerServices.Unsafe" version="4.5.2" targetFramework="net472" />
      <package id="Unity" version="5.10.2" targetFramework="net472" />
    </packages>
    ```

    En los pasos siguientes, usarás las versiones .NET Standard de los paquetes `MvvmLightLibs` y `Unity`. Los otros dos son dependencias que NuGet descarga automáticamente al instalar estas dos bibliotecas.

2. En el proyecto **ContosoExpenses.Core**, elimina el archivo **packages.config**.

3. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y selecciona **Administrar paquetes NuGet**.

4. En la ventana **Administrador de paquetes NuGet**, haz clic en **Examinar**. Busca el paquete `Unity` e instala la versión estable más reciente.

    ![Paquete de Unity](images/wpf-modernize-tutorial/UnityPackage.png)

5. Busca el paquete `MvvmLightLibsStd10` e instala la versión estable más reciente. Esta es la versión de .NET Standard del paquete de `MvvmLightLibs`. Para este paquete, el autor eligió empaquetar la versión .NET Standard de la biblioteca en un paquete independiente de la versión .NET Framework.

    ![Paquete MvvmLightsLibs](images/wpf-modernize-tutorial/MvvmLightsLibsPackage.png)

6. En el proyecto **ContosoExpenses.Core**, haz clic con el botón derecho en el nodo **Dependencias** y elige **Agregar referencia**.

7. En la categoría **Proyectos > Solución**, selecciona **ContosoExpenses.Data** y haz clic en **Aceptar**.

    ![Agregar referencia](images/wpf-modernize-tutorial/AddReference.png)

## <a name="disable-auto-generated-assembly-attributes"></a>Deshabilitación de atributos de ensamblado generados automáticamente

En este punto del proceso de migración, si intentas compilar el proyecto **ContosoExpenses.Core** verás algunos errores.

![Nuevos errores de compilación de .NET Core 3](images/wpf-modernize-tutorial/NETCORE3BuildNewErrors.png)

Este problema se debe a que el nuevo formato. csproj introducido con .NET Core 3.0 almacena la información de ensamblado en el archivo de proyecto en lugar de en el archivo **AssemblyInfo.cs**. Para corregir estos errores, deshabilita este comportamiento y deja que el proyecto siga usando el archivo **AssemblyInfo.cs**.

1. En Visual Studio, haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Descargar el proyecto**. Vuelve a hacer clic con el botón derecho en el proyecto y, a continuación, elige la opción para **editar ContosoExpenses.Core.csproj**.

1. Agrega el siguiente elemento en la sección **PropertyGroup** y guarda el archivo.

    ```XML
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Después de agregar este elemento, la sección **PropertyGroup** debe tener ahora el siguiente aspecto:

    ```XML
    <PropertyGroup>
      <OutputType>WinExe</OutputType>
      <TargetFramework>netcoreapp3.0</TargetFramework>
      <UseWPF>true</UseWPF>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

3. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Volver a cargar el proyecto**.

4. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Data** y elige **Descargar el proyecto**. Vuelve a hacer clic con el botón derecho en el proyecto y, a continuación, elige la opción para **editar ContosoExpenses.Data.csproj**.

5. Agrega la misma entrada en la sección **PropertyGroup** y guarda el archivo.

    ```xml
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    ```

    Después de agregar este elemento, la sección **PropertyGroup** debe tener ahora el siguiente aspecto:

    ```xml
    <PropertyGroup>
      <TargetFramework>netstandard2.0</TargetFramework>
      <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    </PropertyGroup>
    ```

6. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Data** y elige **Volver a cargar el proyecto**.

## <a name="add-the-windows-compatibility-pack"></a>Adición del paquete de compatibilidad de Windows

Si ahora intentas compilar los proyectos **ContosoExpenses.Core** y **ContosoExpenses.Data**, verás que los errores anteriores ahora se han corregido, pero todavía hay algunos errores en la biblioteca **ContosoExpenses.Data** similares a estos.

`Services\RegistryService.cs(9,26,9,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,26,12,34): error CS0103: The name 'Registry' does not exist in the current context`
`Services\RegistryService.cs(12,97,12,123): error CS0103: The name 'RegistryKeyPermissionCheck' does not exist in the current context`

Estos errores son el resultado de la conversión del proyecto **ContosoExpenses.Data** de una biblioteca de .NET Framework (que es específica de Windows) a una biblioteca de .NET Standard, que se puede ejecutar en varias plataformas, como Linux, Android, iOS, etc. El proyecto **ContosoExpenses.Data** contiene una clase denominada **RegistryService**, que interactúa con el Registro, un concepto solo de Windows.

Para resolver estos errores, instala el paquete NuGet de [compatibilidad de Windows](https://www.nuget.org/packages/Microsoft.Windows.Compatibility). Este paquete proporciona compatibilidad con muchas API específicas de Windows que se usarán en una biblioteca de .NET Standard. La biblioteca ya no será multiplataforma después de usar este paquete, pero seguirá teniendo como destino .NET Standard. 

1. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Data**.
2. Elige **Administrar paquetes de NuGet**.
3. En la ventana **Administrador de paquetes NuGet**, haz clic en **Examinar**. Busca el paquete `Microsoft.Windows.Compatibility` e instala la versión estable más reciente.

    ![](images/wpf-modernize-tutorial/WindowsCompatibilityPack.png)

4. Ahora intenta compilar el proyecto de nuevo; para ello, haz clic con el botón derecho en el proyecto **ContosoExpenses.Data** y elige **Compilar**.

Esta vez el proceso de compilación se completará sin errores.

## <a name="test-and-debug-the-migration"></a>Prueba y depuración de la migración

Ahora que los proyectos se compilan correctamente, está listo para ejecutar y probar la aplicación para ver si hay errores en tiempo de ejecución.

1. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Establecer como proyecto de inicio**.

2. Presiona F5 para iniciar el proyecto **ContosoExpenses.Core** en el depurador. Verás una excepción similar a la siguiente.

    ![Excepción mostrada en Visual Studio](images/wpf-modernize-tutorial/ExceptionNETCore3.png)

    Esta excepción se produce porque cuando eliminaste el contenido del archivo. csproj al principio de la migración, quitaste la información sobre la **acción de compilación** para los archivos de imagen. Los pasos siguientes corrigen este problema.

3. Detén el depurador.

4. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Descargar el proyecto**. Vuelve a hacer clic con el botón derecho en el proyecto y, a continuación, elige la opción para **editar ContosoExpenses.Core.csproj**.

5. Antes de cerrar el elemento **Project**, agrega la entrada siguiente:

    ```xml
    <ItemGroup>
      <Content Include="Images/*">
        <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
      </Content>
    </ItemGroup>
    ```

6. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Volver a cargar el proyecto**.

7. Para asignar Contoso.ico a la aplicación, haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Propiedades**. En la página abierta, haz clic en la lista desplegable situada en **Icono** y selecciona `Images\contoso.ico`.

    ![Icono de Contoso en las propiedades del proyecto](images/wpf-modernize-tutorial/ContosoIco.png)

8. Haga clic en **Guardar**.

9. Presiona F5 para iniciar el proyecto **ContosoExpenses.Core** en el depurador. Confirma que la aplicación se ejecuta ahora.

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, has migrado correctamente la aplicación Contoso Expenses a .NET Core 3. Ahora ya estás listo para continuar con la [Parte 2: Incorporación de un control InkCanvas en UWP mediante islas XAML](modernize-wpf-tutorial-2.md).

> [!NOTE]
> Si tienes una pantalla de alta resolución, puedes observar que la aplicación tiene un aspecto muy pequeño. Solucionarás este problema en el siguiente paso del tutorial.
