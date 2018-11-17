---
author: laurenhughes
title: Instalar una aplicación para UWP desde un servidor IIS
description: Este tutorial muestra cómo configurar un servidor IIS, comprueba que la aplicación web puede hospedar paquetes de aplicación e invocar y utilizar al instalador de aplicación de forma eficaz.
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, uwp, instalador de aplicación, AppInstaller, instalación de prueba, relacionados con los paquetes opcionales, Establece, servidor IIS
ms.localizationpriority: medium
ms.openlocfilehash: 2898a3450f75379492bae1ade5c85581cc8e5a4e
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7169128"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>Instalar una aplicación para UWP desde un servidor IIS

Este tutorial muestra cómo configurar un servidor IIS, comprueba que la aplicación web puede hospedar paquetes de aplicación e invocar y utilizar al instalador de aplicación de forma eficaz.

La aplicación del Instalador de aplicación permite a los desarrolladores y profesionales de TI distribuir aplicaciones de Windows 10 hospedándolas en su propia red de entrega de contenido (CDN). Estos es útil para empresas que no desean o necesitan publicar sus aplicaciones en Microsoft Store, pero quieres sacar provecho de la plataforma de implementación y el paquete de Windows 10. 

## <a name="setup"></a>Programa de instalación

Para ir correctamente a través de con este tutorial, necesitarás lo siguiente:

1. Visual Studio 2017  
2. Herramientas de desarrollo Web y de IIS 
3. Paquete de la aplicación para UWP: el paquete de la aplicación que vas a distribuir

Opcional: [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp) en GitHub. Esto es útil si no tienes paquetes de la aplicación para que funcione con, pero aun así deseas obtener información sobre cómo usar esta característica.

## <a name="step-1---install-iis-and-aspnet"></a>Paso 1: instalar IIS y ASP.NET 

[Internet Information Services](https://www.iis.net/) es una característica de Windows que se puede instalar a través del menú Inicio. En el **menú Inicio** busca **las características de Windows de activar o desactivar**.

Buscar y seleccionar **Internet Information Services** para instalar IIS.

> [!NOTE]
> No es necesario seleccionar todas las casillas de verificación en Internet Information Services. Solo los que hayas seleccionados al comprobar **Internet Information Services** son suficientes.

También debes instalar ASP.NET 4.5 o superior. Para instalarla, busque **Internet Information Services -> World Wide Web Services -> características de desarrollo de aplicaciones**. Selecciona una versión de ASP.NET que sea mayor o igual a ASP.NET 4.5.

![Instalar ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>Paso 2: instalar Visual Studio 2017 y herramientas de desarrollo Web 

[Instalar Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) si todavía no se ha instalado. Si ya tienes Visual Studio 2017, asegúrate de que se instalen las cargas de trabajo siguientes. Si no están presentes en la instalación de las cargas de trabajo, sigue a lo largo de mediante el instalador de Visual Studio (que se encuentra en el menú Inicio).  

Durante la instalación, seleccione **Web ASP.NET y desarrollo** y otras cargas de trabajo que le interesa. 

Una vez completada la instalación, inicia Visual Studio y crea un nuevo proyecto (**archivo** -> **Nuevo proyecto**).

## <a name="step-3---build-a-web-app"></a>Paso 3: crear una aplicación Web

Inicia Visual Studio 2017 como **Administrador** y crea un nuevo proyecto de **Aplicación Web Visual C#** con una plantilla de proyecto **vacío** . 

![Nuevo proyecto](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>Paso 4: configurar IIS con nuestra aplicación Web 

Desde el Explorador de soluciones, haz clic en el proyecto raíz y selecciona **Propiedades**.

En las propiedades de aplicación web, selecciona la pestaña de la **Web** . En la sección de **servidores** , elija **IIS Local** en el menú desplegable y haz clic en **Crear directorio Virtual**. 

![pestaña Web](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>Paso 5: agregar un paquete de la aplicación a una aplicación web 

Agregar el paquete de aplicación que vas a distribuir en la aplicación web. Puedes usar el paquete de la aplicación que forma parte de los [paquetes de proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) de proporcionado en GitHub si no tienes un paquete de aplicación disponible. El certificado (MySampleApp.cer) con el que se firmó el paquete también está con la muestra en GitHub. Debe tener el certificado instalado en el dispositivo antes de instalar la aplicación (paso 9).

En la aplicación web de proyecto de inicio, se agregó una nueva carpeta a la aplicación web denominada `packages` que contiene los paquetes de aplicaciones a distribuirse. Para crear la carpeta en Visual Studio, haz clic en la raíz del explorador de soluciones, selecciona **Agregar** -> **Nueva carpeta** y asígnale `packages`. Para agregar paquetes de la aplicación a la carpeta, haz clic en el `packages` carpeta y selecciona **Agregar** -> ubicación del paquete de**Elemento existente** y busca la aplicación. 

![Agregar paquete](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>Paso 6: crear una página Web

Esta aplicación web de muestra usa HTML sencillo. Eres libre para crear la aplicación web según sea necesario por sus necesidades. 

Haz clic en el proyecto de la raíz del explorador de soluciones, selecciona **Agregar** -> **Nuevo elemento**, y agrega una nueva **Página HTML** de la sección de la **Web** .

Una vez que se crea la página HTML, haga clic en la página HTML en el Explorador de soluciones y seleccione **Establecer como página de inicio**.  

Haz doble clic en el archivo HTML para abrirlo en la ventana del editor de código. En este tutorial, se usará solo los elementos de los campos en la página web para invocar la aplicación de instalador de aplicación correctamente para instalar una aplicación de Windows 10. 

Incluir el siguiente código HTML en la página web. La clave para invocar correctamente el instalador de aplicación es usar el esquema personalizado que registra el instalador de aplicación con el sistema operativo: `ms-appinstaller:?source=`. Consulta el ejemplo de código siguiente para obtener más detalles.

> [!NOTE]
> Asegúrate de que la ruta de acceso de dirección URL especificada después de que el esquema personalizado coincide con la dirección Url de proyecto en la pestaña web de la solución de VS.
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>Paso 7: configurar la aplicación web para tipos MIME de paquete de aplicación

Abre el archivo **Web.config** desde el Explorador de soluciones y agrega las siguientes líneas en la `<configuration>` elemento. 

```xml
<system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
</system.webServer>
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>Paso 8: agregar la exención de bucle invertido de instalador de aplicación

Debido al aislamiento de red, aplicaciones para UWP como instalador de aplicación están restringidas a usar direcciones de bucle invertido IP como http://localhost/. Al usar el servidor IIS local, el instalador de aplicación debe agregarse a la lista de exención de bucle invertido. 

Para ello, abre el **símbolo del sistema** como **Administrador** y escribe lo siguiente: ''' línea de comandos CheckNetIsolation.exe LoopbackExempt - a-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

Debe buscar `microsoft.desktopappinstaller_8wekyb3d8bbwe` en la lista.

Una vez completada la validación local de la instalación de la aplicación a través del instalador de aplicación, puedes quitar la exención de bucle invertido que agregaste en este paso por:

''' CheckNetIsolation.exe LoopbackExempt -d de línea de comandos-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

## Step 9 - Run the Web App 

Build and run the web application by clicking on the run button on the VS Ribbon as shown in the image below:

![run](images/run.png)

A web page will open in your browser:

![web page](images/web-page.png)

Click on the link in the web page to launch the App Installer app and install your Windows 10 app package.


## Troubleshooting issues

### Not sufficient privilege 

If running the web app in Visual Studio displays an error such as "You do not have sufficient privilege to access IIS web sites on your machine", you will need to run Visual Studio as an administrator. Close the current instance of Visual Studio and reopen it as an admin.

### Set start page 

If running the web app causes the browser to load with an HTTP 403.14 - Forbidden error, it's because the web app doesn't have a defined start page. Refer to Step 6 in this tutorial to learn how to define a start page.
