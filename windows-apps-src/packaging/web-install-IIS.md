---
title: Instalar una aplicación para UWP desde un servidor IIS
description: Este tutorial muestra cómo configurar un servidor IIS, compruebe que la aplicación web puede hospedar paquetes de aplicaciones e invocar y usar de forma eficaz el instalador de la aplicación.
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, uwp, instalador de la aplicación, AppInstaller, paquetes establecidos, opcionales, IIS Server relacionados con la carga de prueba,
ms.localizationpriority: medium
ms.openlocfilehash: 6a4512229a29a7adc59d6b61edd596eaeb56a5a8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623570"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>Instalar una aplicación para UWP desde un servidor IIS

Este tutorial muestra cómo configurar un servidor IIS, compruebe que la aplicación web puede hospedar paquetes de aplicaciones e invocar y usar de forma eficaz el instalador de la aplicación.

La aplicación del Instalador de aplicación permite a los desarrolladores y profesionales de TI distribuir aplicaciones de Windows 10 hospedándolas en su propia red de entrega de contenido (CDN). Estos es útil para empresas que no desean o necesitan publicar sus aplicaciones en Microsoft Store, pero quieres sacar provecho de la plataforma de implementación y el paquete de Windows 10. 

## <a name="setup"></a>Instalación

Para ver correctamente a través con este tutorial, necesita lo siguiente:

1. Visual Studio 2017  
2. Herramientas de desarrollo Web e IIS 
3. Paquete de la aplicación para UWP: el paquete de la aplicación que vas a distribuir

Opcional: [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp) en GitHub. Esto resulta útil si no tiene que trabajar con los paquetes de aplicaciones, pero aún así desea obtener información sobre cómo usar esta característica.

## <a name="step-1---install-iis-and-aspnet"></a>Paso 1: instalar IIS y ASP.NET 

[Internet Information Services](https://www.iis.net/) es una característica de Windows que se puede instalar mediante el menú Inicio. En **menú Inicio** buscar **o desactivar las características de Windows Active**.

Busque y seleccione **Internet Information Services** para instalar IIS.

> [!NOTE]
> No es necesario seleccionar todas las casillas de verificación en Internet Information Services. Solo los que se seleccionaron al proteger **Internet Information Services** son suficientes.

También necesitará instalar ASP.NET 4.5 o superior. Para instalarlo, busque **Internet Information Services -> World Wide Web Services -> Application Development Features**. Seleccione una versión de ASP.NET que es mayor o igual que ASP.NET 4.5.

![Instalar ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>Paso 2: instalar Visual Studio 2017 y herramientas de desarrollo Web 

[Instale Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) si aún no lo ya instalado. Si ya tiene Visual Studio 2017, asegúrese de que las cargas de trabajo siguientes están instalados. Si las cargas de trabajo no están presentes en la instalación, seguir con el instalador de Visual Studio (que se encuentra en el menú Inicio).  

Durante la instalación, seleccione **ASP.NET y desarrollo Web** y otras cargas de trabajo que le interesa. 

Una vez completada la instalación, inicie Visual Studio y cree un nuevo proyecto (**archivo** -> **nuevo proyecto**).

## <a name="step-3---build-a-web-app"></a>Paso 3: crear una aplicación Web

Inicie Visual Studio 2017 como **administrador** y cree un nuevo **Visual C# aplicación Web** el proyecto con un **vacía** plantilla de proyecto. 

![Nuevo proyecto](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>Paso 4: configurar IIS con nuestra aplicación Web 

En el Explorador de soluciones, haga clic con el botón derecho en el proyecto raíz y seleccione **propiedades**.

En las propiedades de la aplicación web, seleccione el **Web** ficha. En el **servidores** sección, elija **IIS Local** en la lista desplegable de menú y haga clic en **crear directorio Virtual**. 

![pestaña Web](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>Paso 5: agregar un paquete de aplicación a una aplicación web 

Agregue el paquete de aplicación que se va a distribuir en la aplicación web. Puede usar el paquete de aplicación que forma parte de la proporcionada [paquetes del proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) en GitHub si no tiene un paquete de aplicación disponible. El certificado (MySampleApp.cer) con el que se firmó el paquete también está con la muestra en GitHub. Debe tener el certificado instalado en el dispositivo antes de instalar la aplicación (paso 9).

En la aplicación web del proyecto de inicio, se agregó una nueva carpeta a la aplicación web denominada `packages` que contiene los paquetes de aplicación que se distribuyan. Para crear la carpeta en Visual Studio, haga clic en la raíz del explorador de soluciones, seleccione **agregar** -> **nueva carpeta** y asígnele el nombre `packages`. Para agregar paquetes de aplicaciones a la carpeta, haga clic en el `packages` carpeta y seleccione **agregar** -> **elemento existente...**  y vaya a la ubicación del paquete de aplicación. 

![Agregar paquete](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>Paso 6: creación de una página Web

Esta aplicación web de ejemplo usa HTML simple. Es libre para compilar la aplicación web según sea necesario según sus necesidades. 

Haga clic con el botón derecho en el proyecto raíz del explorador de soluciones, seleccione **agregar** -> **nuevo elemento**y agregue un nuevo **página HTML** desde el **Web**sección.

Una vez creada la página HTML, haga clic con el botón derecho en la página HTML en el Explorador de soluciones y seleccione **establecer como página principal**.  

Haga doble clic en el archivo HTML para abrirlo en la ventana del editor de código. En este tutorial, se usará sólo los elementos de los necesarios en la página web para invocar la aplicación del instalador de la aplicación correctamente para instalar una aplicación de Windows 10. 

Incluir el siguiente código HTML en la página web. La clave para invocar correctamente el instalador de la aplicación es usar el esquema personalizado que se registra el instalador de la aplicación con el sistema operativo: `ms-appinstaller:?source=`. Vea el ejemplo de código siguiente para obtener más detalles.

> [!NOTE]
> Asegúrese de que la ruta de dirección URL especificada después de que el esquema personalizado coincide con la dirección Url del proyecto en la pestaña web de la solución de VS.
 
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

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>Paso 7: configurar la aplicación web para los tipos MIME del paquete de aplicación

Abra el **Web.config** archivo desde el Explorador de soluciones y agregue las siguientes líneas en el `<configuration>` elemento. 

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

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>Paso 8: agregar exención de bucle invertido para el instalador de la aplicación

Debido al aislamiento de red, las aplicaciones UWP como instalador de la aplicación están restringidas para utilizar direcciones de bucle invertido IP, como http://localhost/. Cuando se usa el servidor IIS local, instalador de la aplicación debe agregarse a la lista de exención de bucle invertido. 

Para ello, abra **símbolo** como un **administrador** y escriba lo siguiente: ''' línea de comandos CheckNetIsolation.exe LoopbackExempt - a-n="microsoft.desktopappinstaller_8wekyb3d8bbwe"
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

Debería encontrar `microsoft.desktopappinstaller_8wekyb3d8bbwe` en la lista.

Una vez completada la validación local de la instalación de la aplicación a través del instalador de la aplicación, puede quitar la exención de bucle invertido que agregó en este paso a paso por:

```Command Line CheckNetIsolation.exe LoopbackExempt -d -n="microsoft.desktopappinstaller_8wekyb3d8bbwe"
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
