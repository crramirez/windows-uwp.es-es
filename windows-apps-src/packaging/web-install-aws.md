---
title: Hospedaje de paquetes de aplicación para UWP en AWS para instalación web
description: Tutorial de configuración de servidor web AWS para validar la instalación de la aplicación a través de la aplicación del instalador de aplicación
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, Windows 10, UWP, opcionales, establezca los paquetes, AWS relacionados con la instalación de prueba de instalador, AppInstaller, aplicación,
ms.localizationpriority: medium
ms.openlocfilehash: 53fe01a1c1a825377e886e042b4eef3868cbf5eb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628060"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>Hospedaje de paquetes de aplicación para UWP en AWS para instalación web

La aplicación del Instalador de aplicación permite a los desarrolladores y profesionales de TI distribuir aplicaciones de Windows 10 hospedándolas en su propia red de entrega de contenido (CDN). Estos es útil para empresas que no desean o necesitan publicar sus aplicaciones en Microsoft Store, pero quieres sacar provecho de la plataforma de implementación y el paquete de Windows 10.

En este tema se describe los pasos para configurar un sitio Web de Amazon Web Services (AWS) para hospedar los paquetes de aplicación UWP y cómo usar la aplicación del instalador de aplicación para instalar los paquetes de aplicación.

## <a name="setup"></a>Instalación

Para seguir correctamente este tutorial, necesitarás lo siguiente:
 
1. Suscripción de AWS 
2. Página Web
3. Paquete de la aplicación para UWP: el paquete de la aplicación que vas a distribuir

Opcional: [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp) en GitHub. Esto es útil si no tienes un paquete de la aplicación o página web con la que trabajar, pero aun así deseas obtener información acerca de cómo usar esta característica.

En este tutorial tratará cómo configurar una página web y hospedar paquetes en AWS. Esto requerirá una suscripción de AWS. Dependiendo de la escala de la operación, puede usar su suscripción gratuita para seguir este tutorial. 

## <a name="step-1---aws-membership"></a>Paso 1: suscripción AWS
Para obtener una suscripción AWS, visite la [página de detalles de la cuenta AWS](https://aws.amazon.com/free/). En el contexto de este tutorial, puedes usar una suscripción gratuita.

## <a name="step-2---create-an-amazon-s3-bucket"></a>Paso 2: creación de un depósito de Amazon S3

Amazon Simple Storage Service (S3) es una oferta para recopilar, almacenar y analizar datos AWS. S3 depósitos son una manera cómoda para hospedar los paquetes de aplicación UWP y las páginas web para su distribución. 

Después de iniciar sesión AWS con sus credenciales, en `Services` encontrar `S3`. 

Seleccione **crear depósitos**y escriba un **nombre del depósito** para su sitio Web. Siga las indicaciones del cuadro de diálogo para establecer las propiedades y permisos. Para asegurarse de que se puede distribuir su aplicación para UWP desde su sitio Web, habilitar **lectura** y **escribir** permisos para el cubo y seleccione **conceder acceso de lectura público a este cubo** .

![Establecer permisos en el depósito de Amazon S3](images/aws-permissions.png) 

Revise el resumen para asegurarse de que se reflejan las opciones seleccionadas. Haga clic en **crear depósitos** para finalizar este paso. 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>Paso 3: cargar el paquete de aplicación UWP y las páginas web en un cubo de S3

Uno que ha creado un depósito de Amazon S3, podrá verlo en la vista de Amazon S3. Este es un ejemplo del aspecto de nuestra depósito de demostración:

![Vista de depósito de Amazon S3](images/aws-post-create.png)

Ahora estamos preparados cargar los paquetes de aplicaciones y páginas web que nos gustaría hospedar en el depósito de Amazon S3. 

Haga clic en el depósito para cargar contenido recién creado. El depósito está vacío, ya que nada se ha cargado aún. Haga clic en el **cargar** botón y seleccione los paquetes de aplicaciones y archivos de la página web que desea cargar.

> [!NOTE]
> Puedes usar el paquete de la aplicación que forma parte del repositorio del [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp) proporcionado de GitHub si no tienes un paquete de la aplicación disponible. El certificado (MySampleApp.cer) con el que se firmó el paquete también está con la muestra en GitHub. También debes tener el certificado instalado en el dispositivo antes de instalar la aplicación.

![Cargar paquete de aplicación](images/aws-upload-package.png)

También debe tener al igual que los permisos para la creación de un depósito de Amazon S3, el contenido en el depósito **leer**, **escribir**, y **conceder acceso de lectura público a este objeto u objetos** permisos.

Si le gustaría que la prueba de carga una página web, pero no tiene una, puede usar la página html de ejemplo (default.html) desde el [proyecto inicial](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html).

> [!IMPORTANT]
> Antes de cargar la página web, confirme que la referencia de paquete de aplicación en la página web es correcta. 

Para obtener la referencia de paquete de aplicación, cargar primero el paquete de aplicación y copie la dirección URL de paquete de aplicación. Editar la página web html para que refleje la ruta de acceso del paquete de aplicación correcta. Vea el ejemplo de código para obtener más detalles. 

Seleccione el archivo de paquete de aplicación cargados para obtener el vínculo de referencia para el paquete de aplicación, debe ser similar a este ejemplo:

![ruta de acceso de paquete cargado](images/aws-package-path.png)

**Copia** el vínculo a la aplicación del paquete y agregar la referencia en la página web. 

```html
<html>
    <head>
        <meta charset="utf-8" />
        <title> Install My Sample App</title>
    </head>
    <body>
        <a href="ms-appinstaller:?source=https://s3-us-west-2.amazonaws.com/appinstaller-aws-demo/MySampleApp.appxbundle"> Install My Sample App</a>
    </body>
</html>
```
Cargue el archivo html en el depósito de Amazon S3. Recuerde que debe establecer los permisos para permitir **leer** y **escribir** acceso.

## <a name="step-4---test"></a>Paso 4: prueba

Una vez que se carga la página web en el depósito de Amazon S3, obtiene el vínculo a la página web, seleccione el archivo html cargado.

Use el vínculo para abrir la página web. Puesto que se establecen permisos para conceder acceso público a la página de web y el paquete de aplicación, cualquier persona con el vínculo a la página web podrá tener acceso a él e instalar los paquetes de aplicaciones para UWP mediante el instalador de la aplicación. Tenga en cuenta que el instalador de la aplicación forma parte de la plataforma Windows 10. Como desarrollador, no es necesario agregar código adicional ni las características a la aplicación para habilitar el uso del instalador de la aplicación. 

## <a name="troubleshooting"></a>Solución de problemas

### <a name="app-installer-fails-to-install"></a>No se puede instalar el instalador de la aplicación 

Instalación de la aplicación se producirá un error si el certificado firmado con el paquete de aplicación no está instalado en el dispositivo. Para corregir esto, debes instalar el certificado antes de instalar la aplicación. Si hospeda un paquete de aplicación para la distribución pública, se recomienda para firmar el paquete de aplicación con un certificado de una entidad de certificación. 


