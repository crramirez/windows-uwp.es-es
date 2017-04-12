---
author: normesta
Description: "Implementa y depura una aplicación para Plataforma universal de Windows (UWP) convertida a partir de una aplicación de escritorio de Windows (Win32, WPF y Windows Forms) mediante el uso del puente de aplicación de escritorio a UWP."
Search.Product: eADQiWindows 10XVcnh
title: "Puente de dispositivo de escritorio a UWP, depuración"
ms.author: normesta
ms.date: 03/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.openlocfilehash: d1ce3054df19b0b51c8203e7fa7296efde848c41
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-debug"></a>Puente de dispositivo de escritorio a UWP: depuración

En este tema se proporciona información destinada a ayudarte a depurar correctamente tu aplicación y luego convertirla con el Puente de dispositivo de escritorio a UWP. Tienes varias opciones para depurar la aplicación convertida.

## <a name="attach-to-process"></a>Asociar al proceso

Cuando Microsoft Visual Studio se ejecuta "como administrador", los comandos *Iniciar depuración* e *Iniciar sin depurar* funcionarán para el proyecto de una aplicación convertida, pero la aplicación iniciada se ejecutará en un [nivel de integridad medio](https://msdn.microsoft.com/library/bb625963) (es decir, no tendrá privilegios elevados). Para otorgar privilegios de administrador a la aplicación iniciada, primero tienes que iniciar "como administrador" a través de un acceso directo o un icono. Una vez que la aplicación se ejecute desde una instancia de Microsoft Visual Studio "como administrador", invoca __Asociar al proceso__ y selecciona el proceso de la aplicación en el cuadro de diálogo.

## <a name="f5-debug"></a>Depuración con F5

Visual Studio admite ahora un nuevo proyecto de empaquetado. El nuevo proyecto permite copiar automáticamente las actualizaciones al compilar la aplicación en el paquete de la aplicación de Windows creado desde el convertidor en el instalador de la aplicación. Tras configurar el proyecto de empaquetado, ahora también puedes usar F5 para depurar directamente en el paquete aplicación de Windows.

>Nota: También puedes usar la opción de depurar un paquete aplicación de Windows existente, con la opción Depurar -> Otros destinos de depuración -> Depurar paquete de aplicaciones instalado.

Puedes empezar de este modo:

1. Primero, asegúrate de que estás preparado para usar Desktop App Converter. Para obtener instrucciones, consulta [Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md).

2. Ejecuta el convertidor y luego el instalador para la aplicación de Win32. El convertidor captura el diseño y los cambios realizados en el registro y produce un paquete de la aplicación de Windows con manifiesto y registery.dat para virtualizar el registro:

![alt](images/desktop-to-uwp/debug-1.png)

3. Instalar e iniciar [Visual Studio 2017 RC](https://www.visualstudio.com/downloads/#visual-studio-community-2017-rc).

4. Instalar el proyecto VSIX de empaquetado de escritorio a UWP desde la [Galería de Visual Studio](http://go.microsoft.com/fwlink/?LinkId=797871).

5. Abre la solución de Win32 correspondiente que se convirtió en Visual Studio.

6. Agrega el nuevo proyecto de empaquetado a la solución haciendo clic en la solución y eligiendo "Agregar nuevo proyecto". A continuación, elige el proyecto de empaquetado de escritorio a UWP en Instalación e implementación:

    ![alt](images/desktop-to-uwp/debug-2.png)

    El proyecto resultante se agregará a la solución:

    ![alt](images/desktop-to-uwp/debug-3.png)

    En el proyecto de empaquetado, AppXFileList proporciona una asignación de archivos en el diseño de paquete de la aplicación de Windows. Las referencias están vacías al principio, pero se deben establecer manualmente en el proyecto .exe para la ordenación de la compilación.

7. El proyecto DesktopToUWPPackaging tiene una página de propiedades que te permite configurar la raíz del paquete de la aplicación de Windows y qué icono ejecutar:

    ![alt](images/desktop-to-uwp/debug-4.png)

    Establece PackageLayout en la ubicación raíz del paquete de la aplicación de Windows que creó el convertidor (arriba). A continuación, elige qué icono ejecutar.

8.    Abre y edita AppXFileList.xml. Este archivo define cómo copiar el resultado de la compilación de depuración de Win32 en el diseño del paquete de la aplicación de Windows que el convertidor compiló. De manera predeterminada, tenemos un marcador de posición en el archivo con una etiqueta y comentario de ejemplo:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <ItemGroup>
    <!— Use the following syntax to copy debug output to the AppX layout
       <AppxPackagedFile Include="$(outdir)\App.exe">
          <PackagePath>App.exe</PackagePath>
        </AppxPackagedFile>
        See http://etc...
    -->
      </ItemGroup>
    </Project>
    ```

    A continuación, te mostramos un ejemplo de cómo crear la asignación. En este caso, copiaremos el .exe y el .dll de la ubicación de la compilación de Win32 en la ubicación del diseño del paquete.

    ```XML
    <?xml version="1.0" encoding=utf-8"?>
    <Project ToolsVersion=14.0" xmlns="http://scehmas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>{relativepath}</MyProjectOutputPath>
        </PropertyGroup>
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

    El archivo se define del siguiente modo:

    En primer lugar, definimos *MyProjectOutputPath* para que señale a la ubicación donde se compilará el proyecto de Win32:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>..\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
        </PropertyGroup>
    ```

    A continuación, cada *LayoutFile* especifica un archivo para copiar desde la ubicación de la compilación de Win32 en el diseño del paquete de la aplicación de Windows. En este caso, se copian primero un .exe y, luego, un .dll.

    ```XML
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

9. Establece el proyecto de empaquetado como proyecto de inicio. De este modo se copian los archivos de Win32 en el paquete de la aplicación de Windows y luego se inicia el depurador cuando el proyecto se genera y se ejecuta.  

    ![alt](images/desktop-to-uwp/debug-5.png)

10.    Por último, ahora puedes establecer un punto de interrupción en el código de Win32 y presionar la tecla F5 para iniciar el depurador. Copiará las actualizaciones que realizaste en tu aplicación de Win32 en el paquete de la aplicación de Windows y te permitirá depurar directamente desde dentro de Visual Studio.

11.    Si actualizas tu aplicación, necesitarás usar MakeAppX para volver a empaquetar la aplicación. Para obtener más información, consulta [App packager (MakeAppx.exe) (Empaquetador de aplicaciones [MakeAppx.exe])](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx).

Si tienes varias configuraciones de compilación (por ejemplo, para lanzamiento y depuración), puedes agregar lo siguiente al archivo AppXFileList.xml para copiar la compilación de Win32 desde distintas ubicaciones:

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'DesktopUWP'">C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP>
    </MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'ReleaseDesktopUWP'"> C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\ReleaseDesktopUWP</MyProjectOutputPath>
</PropertyGroup>
```

También puedes usar la compilación condicional para habilitar rutas de código en particular si actualizas tu aplicación para UWP pero sigues queriendo compilarla para Win32.

1.    En el ejemplo siguiente, el código se compilará solo para DesktopUWP y mostrará un icono con la API de WinRT.

    ```C#
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

2.    Puedes usar Configuration Manager para agregar la nueva configuración de compilación:

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.    A continuación, en las propiedades del proyecto, agrega compatibilidad con símbolos de compilación condicional:

    ![alt](images/desktop-to-uwp/debug-8.png)

4.    Ahora puedes alternar el destino de compilación a DesktopUWP si quieres que la compilación esté destinada a la API de UWP que agregaste.

## <a name="plmdebug"></a>PLMDebug

La opción Asociar a proceso y F5 de Visual Studio son útiles para depurar la aplicación mientras se ejecuta. Sin embargo, en algunos casos quizás necesites un control más preciso del proceso de depuración, incluida la posibilidad de depurar la aplicación antes de que se inicie. En estos escenarios más avanzados, usa [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx). Esta herramienta te permite depurar la aplicación convertida mediante el depurador de Windows y ofrece control total sobre el ciclo de vida de aplicación, lo que incluye la suspensión, la reanudación y la terminación.

PLMDebug se incluye en Windows SDK. Para obtener más información, consulta [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx).

## <a name="run-another-process-inside-the-full-trust-container"></a>Ejecutar otro proceso dentro del contenedor de plena confianza

Puedes invocar procesos personalizados dentro del contenedor de un paquete de la aplicación especificada. Esto puede ser útil para los escenarios de prueba (por ejemplo, si tienes una herramienta de ejecución de pruebas personalizada y quieres probar la salida de la aplicación). Para ello, usa el cmdlet ```Invoke-CommandInDesktopPackage``` de PowerShell:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```
