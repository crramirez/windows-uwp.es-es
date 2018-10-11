---
author: c-don
title: Instalación de aplicación para UWP desde un Azure Web Server
description: Este tutorial muestra cómo configurar un servidor web de Azure, cómo comprobar que la aplicación web puede hospedar los paquetes de la aplicación, e invocar y utilizar el Instalador de aplicación de forma eficaz.
ms.author: cdon
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, instalador de aplicación, AppInstaller, instalación de prueba, conjunto relacionado, paquetes opcionales, servidor web de Azure
ms.localizationpriority: medium
ms.openlocfilehash: b98ca6316f733210dbdbc5201178b3a89a2b5982
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "4509135"
---
# <a name="install-a-uwp-app-from-an-azure-web-app"></a>Instalar una aplicación para UWP desde una aplicación Web de Azure

La aplicación del Instalador de aplicación permite a los desarrolladores y profesionales de TI distribuir aplicaciones de Windows 10 hospedándolas en su propia red de entrega de contenido (CDN). Estos es útil para empresas que no desean o necesitan publicar sus aplicaciones en Microsoft Store, pero quieres sacar provecho de la plataforma de implementación y el paquete de Windows 10.

En este tema se describen los pasos para configurar un Azure Web Server para que hospede paquetes de la aplicación para UWP y cómo usar la aplicación del Instalador de aplicación para instalar los paquetes de aplicación.

En este tutorial, analizaremos la configuración de un servidor IIS para comprobar localmente que tu aplicación web puede hospedar correctamente los paquetes de aplicaciones e invocar y usar de manera eficaz la aplicación del Instalador de aplicación. También ofreceremos tutoriales para hospedar tus aplicaciones web de manera adecuadas en los servicios web de nube populares en el campo (Azure y AWS) para garantizar que cumplen los requisitos de instalación web del Instalador de aplicación. Este tutorial paso a paso no requiere experiencia y es muy sencillo de seguir. 

## <a name="setup"></a>Instalación

Para seguir correctamente este tutorial, necesitarás lo siguiente:
 
1. Una suscripción a MicrosoftAzure 
2. Paquete de la aplicación para UWP: el paquete de la aplicación que vas a distribuir

Opcional: [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp) en GitHub. Esto es útil si no tienes un paquete de la aplicación o página web con la que trabajar, pero aun así deseas obtener información acerca de cómo usar esta característica.

### <a name="step-1---get-an-azure-subscription"></a>Paso 1: obtener una suscripción de Azure
Para obtener una suscripción de Azure, visita la [página de la cuenta de Azure](https://azure.microsoft.com/free/). En el contexto de este tutorial, puedes usar una suscripción gratuita.

### <a name="step-2---create-an-azure-web-app"></a>Paso 2: crear una aplicación web de Azure 
En la página de Azure Portal, haz clic en el botón **+ Crear un recurso** y, a continuación, selecciona **Aplicación web**

![crear](images/azure-create-app.png)

Crea un **Nombre de la aplicación** único y deja el resto de los campos de manera predeterminada. Haz clic en **Crear** para finalizar el Asistente para la creación de aplicaciones web. 

![crear aplicación web](images/azure-create-app-2.png)

### <a name="step-3---hosting-the-app-package-and-the-web-page"></a>Paso 3: hospedar el paquete de la aplicación y la página web 
Una vez que se ha creado la aplicación web, puedes obtener acceso a ella desde el panel del Azure Portal. En este paso, vamos a crear una página web sencilla con la GUI del Azure Portal.

Tras seleccionar la aplicación web recién creada en el panel, usa el campo de búsqueda para buscar y abrir el **Editor de App Service**. 

En el editor, hay un archivo `hostingstart.html` predeterminado. Haz clic con el botón derecho en el espacio vacío del panel del Explorador de archivos y selecciona **Cargar archivos** para empezar a cargar los paquetes de la aplicación.

> [!NOTE]
> Puedes usar el paquete de la aplicación que forma parte del repositorio del [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp) proporcionado de GitHub si no tienes un paquete de la aplicación disponible. El certificado (MySampleApp.cer) con el que se firmó el paquete también está con la muestra en GitHub. También debes tener el certificado instalado en el dispositivo antes de instalar la aplicación.

![cargar](images/azure-upload-file.png)

Haz clic con el botón derecho en el espacio vacío del panel del Explorador de archivos y selecciona **Nuevos archivos** para crear un archivo nuevo. Asigna el nombre `default.html` al archivo.

Si estás usando el paquete de la aplicación proporcionado en el [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp), copia el siguiente código HTML en la página web `default.html` recién creada. Si estás usando tu propio paquete de la aplicación, modifica la dirección URL del servicio de la aplicación (la dirección URL después de `source=`). Puedes obtener la dirección URL del servicio de la aplicación de la página de introducción de la aplicación en el Azure Portal.

```html
<html>
<head>
    <meta charset="utf-8" />
    <title> Install My Sample App</title>
</head>
<body>
    <a href="ms-appinstaller:?source=https://appinstaller-azure-demo.azurewebsites.net/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

### <a name="step-4---configure-the-web-app-for-app-package-mime-types"></a>Paso 4: configurar la aplicación web para tipos MIME del paquete de aplicación

Agrega un nuevo archivo a la aplicación web: `Web.config`. Abre el archivo `Web.config` desde el explorador y agrega las siguientes líneas. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
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
</configuration>
```

### <a name="step-5---run-and-test"></a>Paso 5: ejecutar y probar

Para iniciar la página web que has creado, usa la dirección URL del paso 3 en el explorador seguido de `/default.html`. 

![Edge](images/edge.png)

Haz clic en la opción para instalar mi aplicación de muestra para iniciar el Instalador de aplicación e instalar el paquete de la aplicación. 

## <a name="troubleshooting-issues"></a>Solución de problemas

### <a name="app-installer-app-fails-to-install"></a>No se puede instalar la aplicación Instalador de aplicación 
Se producirá un error en la instalación de la aplicación si el certificado con el que se firmó el paquete de la aplicación no está instalado en el dispositivo. Para corregir esto, debes instalar el certificado antes de instalar la aplicación. Si hospedas un paquete de la aplicación para distribución pública, te recomendamos firmar el paquete de la aplicación con un certificado de una entidad de certificación. 

![certificado de la aplicación](images/aws-app-cert.png)

### <a name="nothing-happens-when-you-click-the-link"></a>No sucede nada al hacer clic en el vínculo. 
Asegúrate de que la aplicación Instalador de aplicación está instalada. Ve a **Configuración** -> **Aplicaciones y características** y busca Instalador de aplicación en la lista de aplicaciones instaladas. 

