---
author: normesta
Description: "Ejecuta la aplicación empaquetada y revisa su aspecto sin tener que iniciar sesión en ella. A continuación, establece los puntos de interrupción y revisa el código. Cuando estés listo para probar la aplicación en un entorno de producción, firma la aplicación e instálala."
Search.Product: eADQiWindows 10XVcnh
title: "Ejecutar, depurar y probar una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)"
ms.author: normesta
ms.date: 06/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.openlocfilehash: c160fecc530a6366de48f4f2ecc24df2463c0469
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="run-debug-and-test-a-packaged-desktop-app-desktop-bridge"></a>Ejecutar, depurar y probar una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)

Ejecuta la aplicación empaquetada y revisa su aspecto sin tener que iniciar sesión en ella. A continuación, establece los puntos de interrupción y revisa el código. Cuando estés listo para probar la aplicación en un entorno de producción, firma la aplicación e instálala. En este tema se explica cómo realizar cada uno de estos pasos.

<span id="run-app" />
## <a name="run-your-app"></a>Ejecutar la aplicación

Puedes ejecutar la aplicación para probarla de forma local sin tener que obtener un certificado y firmarlo.

Si has creado el paquete mediante un proyecto de UWP en Visual Studio, establece el proyecto de empaquetado como proyecto de inicio y luego presiona CTRL + F5 para iniciar la aplicación.

Si usaste Desktop App Converter o empaquetaste la aplicación de forma manual, abre el símbolo del sistema de Windows PowerShell y en la subcarpeta **PackageFiles** de la carpeta de resultados, ejecuta este cmdlet:

```
Add-AppxPackage –Register AppxManifest.xml
```
Para iniciar la aplicación, búscala en el menú Inicio de Windows.

![Aplicación empaquetada en el menú Inicio](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> Una aplicación empaquetada se ejecuta siempre como un usuario interactivo, por lo que cualquier unidad en la que instales la aplicación empaquetada debe tener un formato NTFS.

## <a name="debug-your-app"></a>Depurar la aplicación

Selecciona el paquete en un cuadro de diálogo cada vez que depures la aplicación o instala una extensión y depura la aplicación sin tener que seleccionar el paquete cada vez que inicies la sesión.

### <a name="debug-your-app-by-selecting-the-package"></a>Depurar la aplicación seleccionando el paquete

Esta opción es la que menos dura a la hora de realizar la instalación, pero es necesario que realices un paso adicional cada vez que quieras iniciar la sesión de depuración.


1. Asegúrate de que inicias tu aplicación empaquetada al menos una vez para que se instale en el equipo local.

   Consulta la sección anterior [Ejecutar la aplicación](#run-app).

2. Inicia Visual Studio.

   Si quieres depurar la aplicación con permisos elevados, inicia Visual Studio mediante la opción **Ejecutar como administrador**.

3. En Visual Studio, elige **Depurar**->**Otros destinos de depuración**->**Depurar paquete de aplicaciones instalado**.

4. En la lista **Paquetes de aplicación instalados**, selecciona el paquete de la aplicación y luego elige el botón **Adjuntar**.


### <a name="debug-your-app-without-having-to-select-the-package"></a>Depurar la aplicación sin tener que seleccionar el paquete

Con esta opción se tarda más tiempo en realizar la instalación, pero no tendrás que seleccionar el paquete instalado cada vez que la inicies. Necesitarás instalar [Visual Studio 2017](https://www.visualstudio.com/vs/whatsnew/) para usar este método.

1. En primer lugar, instala [Desktop Bridge Debugging Project (Proyecto de depuración del Puente de dispositivo de escritorio)](http://go.microsoft.com/fwlink/?LinkId=797871).

2. Inicia Visual Studio y abre el proyecto de la aplicación de escritorio.

6. Agrega un proyecto denominado **Depuración del Puente de dispositivo de escritorio** a la solución.

   Puedes encontrar la plantilla del proyecto en el grupo **Otros tipos de proyectos** de plantillas instaladas.

    ![alt](images/desktop-to-uwp/debug-2.png)

    El proyecto **Depuración del Puente de dispositivo de escritorio** aparecerá en la solución.

    ![alt](images/desktop-to-uwp/debug-3.png)

7. Abre las páginas de propiedades del proyecto **Depuración del Puente de dispositivo de escritorio**.

8. Establece el campo **Diseño del paquete** en la ubicación del archivo del manifiesto de paquete (AppxManifest.xml) y elige el archivo ejecutable de la aplicación de la lista desplegable **Start Up Tile (Icono de inicio)**.

     ![alt](images/desktop-to-uwp/debug-4.png)

8. Abre el archivo AppXPackageFileList.xml en el editor de código.

9. Quita los comentarios del bloque de XML y agrega los valores de estos elementos:

   **MyProjectOutputPath**: es la ruta de acceso relativa para depurar la carpeta de la aplicación de escritorio.

   **LayoutFile**: es el archivo ejecutable que se encuentra en la carpeta de depuración de la aplicación de escritorio.

   **PackagePath**: es el nombre de archivo completo del archivo ejecutable de la aplicación de escritorio que copiaste en la carpeta del paquete de la aplicación de Windows durante el proceso de conversión.

    A continuación te mostramos un ejemplo:

    ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
     <MyProjectOutputPath>..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    </PropertyGroup>
    <ItemGroup>
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.exe">
        <PackagePath>$(PackageLayout)\MyDesktopApp.exe</PackagePath>
      </LayoutFile>
    </ItemGroup>
  </Project>
    ```

  Si la aplicación usa los archivos DLL que se generan a partir de otros proyectos en la solución y quieres revisar el código que se encuentra en esos archivos DLL, incluye un elemento **LayoutFile** en cada uno de esos archivos DLL.

  ```XML
  ...
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.Models.dll">
      <PackagePath>$(PackageLayout)\MyDesktopApp.Models.dll</PackagePath>
      </LayoutFile>
  ...
  ```

10. Establece el proyecto de empaquetado como proyecto de inicio.  

    ![alt](images/desktop-to-uwp/debug-5.png)

11. Establece los puntos de interrupción en el código de la aplicación de escritorio y, a continuación, inicia el depurador.

  ![Botón de depuración](images/desktop-to-uwp/debugger-button.png)

  Visual Studio copia los archivos ejecutables y DLL que especificaste en el archivo XML en el paquete de la aplicación de Windows y, a continuación, inicia el depurador.

#### <a name="handle-multiple-build-configurations"></a>Controlar varias configuraciones de compilación

Si has definido varias configuraciones de compilación (por ejemplo: Liberar y Depurar), puedes modificar el archivo AppXPackageFileList.xml para que se copien solo los archivos que coincidan con la configuración de compilación que elegiste en Visual Studio al iniciar el depurador.

Observa el siguiente ejemplo.

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'Debug'">..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'Release'"> ..\MyDesktopApp\bin\Release</MyProjectOutputPath>
</PropertyGroup>
```

#### <a name="debug-uwp-enhancements-to-your-app"></a>Depurar las mejoras de UWP en la aplicación

Es posible que quieras mejorar la aplicación e incluir experiencias tan modernas como los iconos dinámicos. Si haces esto, puedes usar la compilación condicional para habilitar rutas de código con configuraciones de compilación específicas.

1. En primer lugar, en Visual Studio, define una configuración de compilación y asígnale un nombre como "DesktopUWP".

2. En la ficha **Compilación** de las propiedades del proyecto, agrega ese nombre en el campo **Símbolos de compilación condicional**.

     ![alt](images/desktop-to-uwp/debug-8.png)

3. Agrega los bloques de código condicional. Este código se compila solo para la configuración de compilación **DesktopUWP**.

    ```csharp
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

### <a name="debug-the-entire-app-lifecycle"></a>Depurar el ciclo de vida de toda la aplicación

en algunos casos quizás necesites un control más preciso del proceso de depuración, incluida la posibilidad de depurar la aplicación antes de que se inicie.

Puedes usar [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) para obtener el control total sobre el ciclo de vida de la aplicación (por ejemplo, puedes suspenderlo, reanudarlo o finalizarlo).

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) se incluye en Windows SDK.


### <a name="modify-your-app-in-between-debug-sessions"></a>Modificar la aplicación entre sesiones de depuración

Si realizas cambios en la aplicación para corregir errores, vuelve a empaquetarla mediante la herramienta MakeAppx. Consulta [Ejecutar la herramienta MakeAppX](desktop-to-uwp-manual-conversion.md#make-appx)

## <a name="test-your-app"></a>Probar la aplicación

Para probar la aplicación en una configuración realista mientras la preparas para su distribución, lo mejor es firmar la aplicación e instalarla.

Si empaquetaste la aplicación con Visual Studio, puedes ejecutar un script para firmarla e instalarla. Consulta [Realizar la instalación de prueba del paquete de la aplicación](../packaging/packaging-uwp-apps.md#sideload-your-app-package).

Si empaquetaste la aplicación mediante Desktop App Converter, puedes usar el parámetro ``sign`` para firmar la aplicación automáticamente con un certificado generado. Tendrás que instalar el certificado y, a continuación, instalar la aplicación. Consulta [Ejecutar la aplicación empaquetada](desktop-to-uwp-run-desktop-app-converter.md#run-app).   

También puedes firmar la aplicación manualmente. A continuación te indicamos cómo

1. Crea un certificado. Consulta [Crear un certificado](../packaging/create-certificate-package-signing.md).

2. Instala el certificado en el almacén de certificados **Raíz de confianza** o **Personas de confianza** del sistema.

3. Firma la aplicación con ese certificado; para ello, consulta [Firmar un paquete de aplicación con SignTool](../packaging/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Asegúrate de que el nombre del publicador del certificado coincide con el de la aplicación.

### <a name="related-sample"></a>Muestra relacionada

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-app-for-windows-10-s"></a>Probar la aplicación en Windows 10 S

Antes de publicar tu aplicación, asegúrate de que funcionará correctamente en dispositivos que ejecutan Windows 10S. De hecho, si vas a publicar la aplicación en la tienda Windows, debes hacerlo porque es un requisito de la tienda. Las aplicaciones que no funcionan correctamente en dispositivos que ejecutan Windows 10S no estarán certificadas. 

Consulta [test your Windows app for Windows 10 S (Probar la aplicación de Windows en Windows 10 S)](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s).

### <a name="run-another-process-inside-the-full-trust-container"></a>Ejecutar otro proceso dentro del contenedor de plena confianza

Puedes invocar procesos personalizados dentro del contenedor de un paquete de la aplicación especificada. Esto puede ser útil para los escenarios de prueba (por ejemplo, si tienes una herramienta de ejecución de pruebas personalizada y quieres probar la salida de la aplicación). Para ello, usa el cmdlet ```Invoke-CommandInDesktopPackage``` de PowerShell:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a preguntas específicas**

Nuestro equipo supervisa estas [etiquetas de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envíanos tus comentarios acerca de este artículo**

Usa la sección comentarios que tienes a continuación.
