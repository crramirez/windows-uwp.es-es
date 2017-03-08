---
author: awkoren
Description: "En esta guía se explica cómo configurar la solución de Visual Studio para editar, depurar y empaquetar tu aplicación de Puente de dispositivo de escritorio convertida."
Search.Product: eADQiWindows 10XVcnh
title: "Guía de empaquetado de Puente de dispositivo de escritorio para aplicaciones de escritorio de .NET con Visual Studio"
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 8aa68312d6ce81c809c79ddcafe7732944a628be
ms.lasthandoff: 02/08/2017

---

# <a name="desktop-bridge-packaging-guide-for-net-desktop-apps-with-visual-studio"></a>Guía de empaquetado de Puente de dispositivo de escritorio para aplicaciones de escritorio de .NET con Visual Studio

La Actualización de aniversario de Windows 10 permite a los desarrolladores usar el Puente de dispositivo de escritorio para empaquetar aplicaciones de Win32 existentes con el nuevo modelo de paquete (.appx), que permite realizar la publicación en la Tienda y la instalación de prueba fácilmente. Esta guía explica cómo configurar la solución de Visual Studio para que puedas editar, depurar y empaquetar tu aplicación. 

Para empezar, rellena el formulario en [Bring your existing apps and games to the Windows Store with the Desktop Bridge (Llevar las aplicaciones y los juegos existentes a la Tienda Windows con el Puente de dispositivo de escritorio)](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft se pondrá en contacto contigo para iniciar el proceso de incorporación. Una vez que tu cuenta esté aprobada para enviar aplicaciones de Puente de dispositivo de escritorio, sigue las instrucciones de este documento para preparar el paquete appxupload para la carga. 

> ¿Tienes comentarios o encuentras problemas a medida que examinas el Puente de dispositivo de escritorio? El mejor lugar para realizar sugerencias de características es en [Windows Developer UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial). Para realizar preguntas y consultar informes de errores, dirígete a los [foros de desarrollo de aplicaciones universales de Windows](https://social.msdn.microsoft.com/Forums/home?forum=wpdevelop).

## <a name="default-universal-windows-platform-packages"></a>Paquetes predeterminados de la Plataforma universal de Windows

Visual Studio te permite generar paquetes de depuración y lanzamiento que pueden distribuirse mediante la Tienda Windows o la transferencia local de aplicaciones. Para facilitar la creación de paquetes, Visual Studio te ayuda a crear uno. archivo .appxupload listo para enviarse a la tienda. Para más información, consulta [Empaquetado de aplicaciones para UWP](..\packaging\packaging-uwp-apps.md).

## <a name="desktop-bridge-packages"></a>Paquetes del Puente de dispositivo de escritorio

El [Puente de dispositivo de escritorio](desktop-to-uwp-root.md) permite que las distintas configuraciones integren archivos binarios de Win32 en el paquete de aplicación (appx). Podemos considerar la progresión del Puente de dispositivo de escritorio como un proceso con cuatro pasos clave. 

- **Paso 1 - Convertir**: empaquetar archivos binarios Win32 existentes sin ningún cambio o pocos cambios en el código.
- **Paso 2 - Mejorar**: incluir algunas características básicas de UWP (por ejemplo, un icono dinámico) en la aplicación existente haciendo referencia a Windows.winmd desde el código existente de Win32.
- **Paso 3 - Ampliar**: incluir funcionalidades de UWP avanzadas (como tareas en segundo plano) con la aplicación existente. Si los componentes UWP y Win32 se crean mediante lenguajes administrados (como C# o VB.Net), el paquete resultante tendrá archivos binarios mixtos que deberán procesarse detenidamente para garantizar el procesamiento de .NET Native correcto. 
- **Paso 4 - Migrar**: has migrado la interfaz de usuario a C#/VB.NET y XAML moderno, pero aún tienes código heredado de Win32. El punto de entrada ahora es un archivo ejecutable de .NET de UWP, pero aún tienes archivos binarios en el paquete que usan algunas API de Win32.

En la siguiente tabla se resumen algunas de las diferencias de la aplicación en cada uno de los cuatro pasos. 

| Paso | Binarios | Punto de entrada | .NET Native | Depuración con F5 |
|---|---|---|---|---|
| 1 (Convertir) | Win32 | Win32 | N/A | Extensión de VS |
| 2 (Mejorar) | Refs WinMD | Win32 | N/A | Extensión de VS |
| 3 (Ampliar) | Win32 + CoreCLR (*) | Win32 | Por usuario (**) | Extensión de VS |
| 4 (Migrar)    | CoreCLR (*) + Win32 | UWP | Por usuario (**) | VS |
| 5 (UWP) | CoreCLR | UWP |Por la Tienda | VS |

(*) [CoreCLR](https://github.com/dotnet/coreclr) hace referencia al tiempo de ejecución de .NET Core de que dependen los componentes de UWP escritos en un lenguaje administrado (C# / VB.NET). Estos componentes también requerirán el procesamiento .NET Native.

(**) En los pasos 3 y 4, el usuario debe procesar los ensamblados CoreCLR para generar los archivos binarios nativos de .NET y los símbolos correspondientes antes de realizar publicaciones en la tienda.

## <a name="configure-your-visual-studio-solution"></a>Configurar la solución de Visual Studio

Visual Studio incluye las herramientas que necesitas para configurar el paquete de aplicación, como el editor de manifiestos y el Asistente para la creación de paquetes. Para usar estas herramientas, necesitas un proyecto de UWP que actuará como el contenedor de appx para tu aplicación. Aunque puedes usar cualquier proyecto de UWP (como C#, VB.NET, C++ o JavaScript), hay algunos problemas conocidos de proyectos de C#, VB.NET y C++ (consulta la sección [Problemas conocidos](#known-issues-anchor) más adelante en este documento), de modo que vamos a usar JavaScript para este ejemplo. 

Si quieres depurar la aplicación en el contexto del modelo de aplicación de appx, deberás agregar otro proyecto que permitirá la depuración de appx F5. Para obtener más información, consulta la sección [Depurar la aplicación de Puente de dispositivo de escritorio](#debugging-anchor).

Empecemos con el paso 1 del proceso.

### <a name="step-1-convert"></a>Paso 1: convertir

Este paso muestra cómo crear una aplicación de Puente de dispositivo de escritorio a partir de un proyecto existente de Win32. En este ejemplo, usaremos un proyecto WinForms básico que realiza operaciones de lectura y escritura en el registro.

![](images/desktop-to-uwp/net-1.png)

#### <a name="add-the-uwp-project"></a>Agregar el proyecto UWP 

Para crear el paquete de Puente de dispositivo de escritorio, agrega un proyecto de UWP de JavaScript a la misma solución.

> Nota: Aunque estamos usando una plantilla de UWP de JavaScript, no vamos a escribir código JavaScript. Solo usamos el proyecto como una herramienta.

![](images/desktop-to-uwp/net-2.png)

#### <a name="add-the-win32-binaries-to-the-win32-folder"></a>Agregar los archivos binarios de Win32 a la carpeta de Win32

Todos los binarios de Win32 se almacenarán en el proyecto UWP en una carpeta denominada Win32 (aunque no es necesario este nombre exacto; puedes usar el nombre que quieras).

Si usas Visual Studio, puedes automatizar el proyecto para que copie los archivos después de cada compilación y, así, mejorar el flujo de trabajo de desarrollo. Edita el archivo de proyecto (.csproj en este ejemplo) para que incluya el destino AfterBuild que copiará todos los archivos de salida de Win32 a la carpeta Win32 del proyecto UWP, tal y como se muestra a continuación: 

```xml
  <Target Name="AfterBuild">
    <PropertyGroup>
      <TargetUWP>..\MyDesktopApp.Package\win32\</TargetUWP>
    </PropertyGroup>
     <ItemGroup>
       <Win32Binaries Include="$(TargetDir)\*" />
     </ItemGroup>
    <Copy SourceFiles="@(Win32Binaries)" DestinationFolder="$(TargetUWP)" />
  </Target>
```

Si usas otra herramienta para producir los archivos binarios de Win32, simplemente copia todos los archivos necesarios en tiempo de ejecución a la carpeta Win32. 

#### <a name="edit-the-app-manifest-to-enable-the-desktop-bridge-extensions"></a>Editar el manifiesto de la aplicación para habilitar las extensiones de Puente de dispositivo de escritorio

Esta plantilla incluye el archivo package.appxmanifest, que puedes usar para agregar las extensiones de Puente de dispositivo de escritorio. Para editar este archivo, haz clic y selecciona "Ver código" y agrega o modifica estos elementos: 

- `<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities" IgnorableNamespaces="uap rescap">`

- `<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" MaxVersionTested="10.0.14393.0" />`

- `<rescap:Capability Name="runFullTrust" />`

- `<Application Id="MyDesktopAppStep1" Executable="win32\MyDesktopApp.exe" EntryPoint="Windows.FullTrustApplication">`

Este es un ejemplo completo del archivo de manifiesto: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"

        xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
        xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
        IgnorableNamespaces="uap rescap mp">
  <Identity Name="MyDesktopAppStep1"
            ProcessorArchitecture="x64"
            Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US"
            Version="1.0.0.5" />
  <mp:PhoneIdentity PhoneProductId="6f6600a4-6da1-4d91-b493-35808d01f8de" PhonePublisherId="00000000-0000-0000-0000-000000000000" />
  <Properties>
    <DisplayName>MyDesktopAppStep1</DisplayName>
    <PublisherDisplayName>CN=Test</PublisherDisplayName>
    <Logo>Assets\SampleAppx.150x150.png</Logo>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" 
                        MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
  <Applications>
    <Application Id="MyDesktopAppStep1" 
                 Executable="win32\MyDesktopApp.exe" 
                 EntryPoint="Windows.FullTrustApplication">
      <uap:VisualElements DisplayName="MyDesktopAppStep1" 
                          Description="MyDesktopAppStep1" 
                          BackgroundColor="#777777" 
                          Square150x150Logo="Assets\SampleAppx.150x150.png" 
                          Square44x44Logo="Assets\SampleAppx.44x44.png">
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

#### <a name="configure-the-win32-binaries"></a>Configurar los archivos binarios de Win32

Para incluir los archivos binarios que necesita tu aplicación en el paquete de salida, selecciona cada archivo en Visual Studio. Establece sus propiedades como "Contenido" y el comportamiento de la compilación en "Copiar si es posterior". 

![](images/desktop-to-uwp/net-3.png)

Si quieres evitar la confirmación de los archivos binarios en el repositorio de código fuente, puedes usar el archivo .gitignore para excluir todos los archivos de la carpeta Win32. 

#### <a name="optional-use-wildcards-to-specify-the-files-in-your-win32-folder"></a>Opcional: Usa caracteres comodín para especificar los archivos en tu carpeta Win32

Si la aplicación Win32 necesita varios archivos, puedes editar el archivo de proyecto para que especifique un carácter comodín que indique qué archivos se deben marcar como "Contenido" en función de una expresión con caracteres comodín. Debes abrir el  archivo .jsproj con un editor de texto e incluir los archivos que necesites como se muestra a continuación:

```xml
<Content Include="win32\*.dll">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.exe">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.config">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
<Content Include="win32\*.pdb">
  <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
</Content>
```

### <a name="step-2-enhance"></a>Paso 2: mejorar

Si quieres llamar a las API de UWP disponibles desde el código de Win32, deberás agregar una referencia a `\Program Files (x86)\Windows Kits\10\UnionMetadata\Windows.winmd`. La lista completa de API de UWP disponibles para tu aplicación se encuentra en el artículo [API de UWP admitidas para aplicaciones convertidas con Puente de dispositivo de escritorio](desktop-to-uwp-supported-api.md).  

Dado que este archivo no es necesario en Windows 10, no hace falta que lo distribuyas. En las propiedades de referencia, establece la propiedad "Copia Local" en false.

![](images/desktop-to-uwp/net-4.png)

Para agregar los archivos binarios de Win32, usa las mismas instrucciones que se indican en el Paso 1. 

### <a name="step-3-extend"></a>Paso 3: ampliar 

Para este ejemplo, ampliamos una aplicación Win32 con una tarea en segundo plano. Esto requiere registrar la tarea en segundo plano en package.appxmanifest de la aplicación para UWP y agregar una referencia al proyecto que implemente la tarea en segundo plano, como se muestra a continuación.

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" 
              EntryPoint="BackgroundTasks.MyBackgroundTask">
    <BackgroundTasks>
      <Task Type="timer" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

Si la tarea en segundo plano se implementa con C# o VB.NET, la salida resultante contendrá archivos binarios CoreCLR que debe procesar la cadena de herramientas de .NET Native antes del envío a la tienda, como se describe en los pasos 3 y 4. Crea appxupload con archivos binarios mixtos.

### <a name="step-4-migrate"></a>Paso 4: migrar

Este escenario ya tiene un punto de entrada de UWP de C#, por lo que no es necesario agregar un proyecto UWP adicional. Sin embargo, debes seguir los pasos descritos en el Paso 1 para incluir y configurar los archivos binarios de Win32.

Para ejecutar el proceso de Win32, usa las API [**FullTrustProcessLauncher**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.FullTrustProcessLauncher). Deberás agregar la extensión de escritorio y la funcionalidad *fullTrustProcess* en el manifiesto de la aplicación para usar las API, como se muestra a continuación: 

```xml
..
xmlns:desktop=http://schemas.microsoft.com/appx/manifest/desktop/windows10
..
<desktop:Extension Category="windows.fullTrustProcess" 
                    Executable="win32\MyDesktopApp.exe" />
```

## <a name="generate-packages-for-your-desktop-bridge-app"></a>Generar paquetes de la aplicación de Puente de dispositivo de escritorio

Una vez hayas seguido las instrucciones anteriores, debes estar preparado para generar los paquetes con Visual Studio, como se describe en [Empaquetado de aplicaciones para UWP](..\packaging\packaging-uwp-apps.md). 

### <a name="steps-1-and-2-create-appxupload-with-win32-binaries"></a>Pasos 1 y 2: crear appxupload con archivos binarios de Win32

Para enviar paquetes con la funcionalidad *fullTrust*, debes generar un archivo appxupload que incluya los símbolos de cada plataforma en un archivo appxsym, y un lote que contenga los paquetes de la plataforma de appx.

En los pasos 1 y 2, el paquete no contiene ningún archivo binario CoreCLR, por lo que no es necesario que te preocupes por la plataforma que debes elegir. Selecciona "Neutral" y "Release (Any CPU)", como se muestra en la figura siguiente.

![](images/desktop-to-uwp/net-5.png)

Después de seleccionar la opción "Generate Store packages", el asistente generará el archivo appxupload listo para enviarse a la tienda.

### <a name="step-3-and-4-create-appxupload-with-mixed-binaries"></a>Pasos 3 y 4: crear appxupload con archivos binarios mixtos

También debes compilar para el lanzamiento y, en este caso, es obligatorio especificar qué plataformas queremos usar como destino, dado que es necesario que .NET Native produzca los archivos binarios nativos para cada plataforma.

![](images/desktop-to-uwp/net-6.png)

Para crear el nuevo archivo appxupload, creamos un nuevo archivo ZIP que incluya los archivos appxsym y appxbundle generados desde la carpeta _Prueba.

Crea un nuevo archivo ZIP que contenga los archivos appxsym y appxbundle y, a continuación, cambia el nombre de la extensión a appxupload.

![](images/desktop-to-uwp/net-7.png)

<span id="debugging-anchor" />
## <a name="debugging-your-desktop-bridge-app"></a>Depurar la aplicación de Puente de dispositivo de escritorio

Aunque puedes iniciar los proyectos desde Visual Studio sin depuración (Ctrl + F5), existe un problema conocido que causa que Visual Studio no pueda conectarse automáticamente al proceso en ejecución. Sin embargo, puedes establecer la conexión más adelante mediante uno de los siguientes métodos de conexión:

### <a name="attach-to-the-running-app"></a>Conectar con la aplicación en ejecución

#### <a name="attach-to-an-existing-process"></a>Conectar con el proceso existente

Una vez hayas iniciado correctamente la aplicación con Ctrl + F5, puedes establecer conexión con el proceso de Win32; sin embargo, no podrás depurar los módulos .NET Native. 

![](images/desktop-to-uwp/net-8.png)

#### <a name="attach-to-an-installed-app"></a>Conectar con una aplicación instalada

También puedes establecer conexión con cualquier paquete Appx existente mediante la opción Depurar -> Otros destinos de depuración -> Depurar paquete de aplicaciones instalado.

![](images/desktop-to-uwp/net-9.png)

Aquí puedes seleccionar la máquina local o conectarte a una remota.

![](images/desktop-to-uwp/net-10.png)

Con esta opción, podrás depurar código .NET Native.

### <a name="use-visual-studio-extension-to-debug-your-desktop-bridge-app"></a>Usar la extensión de Visual Studio para depurar la aplicación de Puente de dispositivo de escritorio 

Si prefieres ejecutar la opción de depurar tu aplicación con F5, debes instalar la extensión de Visual Studio 2017 [Desktop Bridge Debugging Project](https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.DesktoptoUWPPackagingProject) desde la galería de Visual Studio.

Este proyecto te permite depurar cualquier aplicación de Win32 que se esté migrando a UWP con Visual Studio (tal y como se describe en este documento) o mediante Desktop App Converter.

#### <a name="add-the-debugging-project-to-your-solution"></a>Agregar el proyecto de depuración a tu solución

Para empezar, agrega un nuevo proyecto Desktop Bridge Debugging Project al proyecto de tu solución.

![](images/desktop-to-uwp/net-11.png)

Para configurar este proyecto, debes definir la propiedad PackageLayout en la ventana de propiedades de cada configuración/plataforma que quieras usar para la depuración.
Para configurar la depuración/x86, estableceremos la propiedad de diseño del paquete a la carpeta papelera\x86\depuración del proyecto UWP con una ruta de acceso relativa: `..\MyDesktopApp.Package\bin\x86\Debug`. 

![](images/desktop-to-uwp/net-12.png)

Y editaremos el archivo AppXFileLayout.xml para especificar el punto de entrada:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="14.0" 
         xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <MyProjectOutputPath>$(PackageLayout)</MyProjectOutputPath>
  </PropertyGroup>
  <ItemGroup>
    <LayoutFile Include="$(MyProjectOutputPath)\win32\MyDesktopApp.exe">
      <PackagePath>$(PackageLayout)\win32\MyDesktopApp.exe</PackagePath>
    </LayoutFile>
  </ItemGroup>
</Project>
```

Por último, debes configurar las dependencias de la solución para asegurarte de que los proyectos están integrados en el orden correcto. 

Por ejemplo, vamos a revisar la solución creada para el paso 3.

![](images/desktop-to-uwp/net-13.png)

Para configurar el orden de compilación, puedes usar la configuración Dependencias del proyecto. Haz clic con el botón derecho en la solución y selecciona la opción Dependencias del proyecto. Una vez establezcas las dependencias correctas, puedes validar el orden de compilación como se muestra a continuación (para el paso 3):

![](images/desktop-to-uwp/net-14.png)

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>Problemas conocidos con proyectos de C++ UWP y C#/VB.NET

Si prefieres usar un proyecto de C# para empaquetar tu aplicación, debes tener en cuenta los siguientes problemas conocidos. 

- **La compilación de la aplicación en la depuración genera el error: Microsoft.Net.CoreRuntime.targets(235,5), que causa que no se admitan aplicaciones con archivos ejecutables de punto de entrada personalizados. Comprueba el atributo Executable del elemento Application en el manifiesto del paquete.** Como solución alternativa, usa el modo Lanzamiento.

- **Los archivos binarios de Win32 que se almacenan en la carpeta raíz del proyecto UWP se eliminan en el Lanzamiento**. Si no usas una carpeta para almacenar los archivos binarios de Win32, el compilador de .NET Native quitará los del paquete final, lo que dará como resultado un error de validación del manifiesto, porque no se encuentra el punto de entrada del archivo ejecutable.


