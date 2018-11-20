---
author: laurenhughes
title: Hospedaje de paquetes de aplicación para UWP en AWS para instalación web
description: Tutorial para configurar el servidor web AWS validar la instalación de la aplicación a través de la aplicación del instalador de aplicación
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
keywords: Windows 10, Windows 10, UWP, paquetes opcionales, Establece, AWS relacionadas con la instalación de prueba de instalador, AppInstaller, aplicación,
ms.localizationpriority: medium
ms.openlocfilehash: f24abac93e2444a3c9f454c8883902e5db4d31be
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7281830"
---
# <a name="hosting-uwp-app-packages-on-aws-for-web-install"></a>Hospedaje de paquetes de aplicación para UWP en AWS para instalación web

La aplicación del Instalador de aplicación permite a los desarrolladores y profesionales de TI distribuir aplicaciones de Windows 10 hospedándolas en su propia red de entrega de contenido (CDN). Estos es útil para empresas que no desean o necesitan publicar sus aplicaciones en Microsoft Store, pero quieres sacar provecho de la plataforma de implementación y el paquete de Windows 10.

Este tema describen los pasos para configurar un sitio Web de Amazon Web Services (AWS) para hospedar paquetes de aplicaciones para UWP y cómo usar la aplicación de instalador de aplicación para instalar los paquetes de aplicaciones.

## <a name="setup"></a>Instalación

Para seguir correctamente este tutorial, necesitarás lo siguiente:
 
1. Suscripción AWS 
2. Página Web
3. Paquete de la aplicación para UWP: el paquete de la aplicación que vas a distribuir

Opcional: [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp) en GitHub. Esto es útil si no tienes un paquete de la aplicación o página web con la que trabajar, pero aun así deseas obtener información acerca de cómo usar esta característica.

Este tutorial te redirigirá sobre cómo configurar una página web y paquetes de host en AWS. Esto requiere una suscripción AWS. Según la escala de la operación, puedes usar su suscripción gratuita a seguir este tutorial. 

## <a name="step-1---aws-membership"></a>Paso 1: pertenencia AWS
Para obtener una suscripción AWS, visita la [página de detalles de la cuenta AWS](https://aws.amazon.com/free/). En el contexto de este tutorial, puedes usar una suscripción gratuita.

## <a name="step-2---create-an-amazon-s3-bucket"></a>Paso 2: crear un cubo de Amazon S3

El servicio de almacenamiento sencillo de Amazon (S3) es un AWS oferta para recopilar, almacenar y analizar los datos. S3 depósitos son una forma cómoda para hospedar paquetes de aplicaciones para UWP y las páginas web para su distribución. 

Después de iniciar sesión AWS con sus credenciales en `Services` encontrar `S3`. 

Selecciona el **cubo de crear**y escribe un **nombre de cubo** de tu sitio Web. Sigue las instrucciones del cuadro de diálogo para establecer propiedades y los permisos. Para garantizar que tu aplicación para UWP puede distribuirse desde tu sitio Web, habilite **leer** y **escribir** permisos para el cubo y seleccione **conceder acceso público de lectura a este cubo**.

![Establecer permisos en Amazon S3 cubo](images/aws-permissions.png) 

Revisa el resumen para asegurarse de que se reflejan las opciones seleccionadas. Haz clic en **Crear cubo** para completar este paso. 

## <a name="step-3---upload-uwp-app-package-and-web-pages-to-an-s3-bucket"></a>Paso 3: cargar el paquete de la aplicación para UWP y las páginas web a un cubo de S3

Uno has creado un cubo de Amazon S3, podrás ver esto en la vista de Amazon S3. Este es un ejemplo del aspecto de nuestro cubo de demostración:

![Vista de cubo de Amazon S3](images/aws-post-create.png)

Ahora estamos listos para cargar los paquetes de aplicaciones y páginas web que nos gustaría para hospedar en nuestro cubo de Amazon S3. 

Haz clic en el cubo para cargar contenido recién creado. El cubo está vacía, ya que nada se han cargado aún. Haz clic en el botón de **carga** y selecciona los paquetes de aplicaciones y archivos de la página web que desea volver a cargar.

> [!NOTE]
> Puedes usar el paquete de la aplicación que forma parte del repositorio del [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp) proporcionado de GitHub si no tienes un paquete de la aplicación disponible. El certificado (MySampleApp.cer) con el que se firmó el paquete también está con la muestra en GitHub. También debes tener el certificado instalado en el dispositivo antes de instalar la aplicación.

![cargar el paquete de la aplicación](images/aws-upload-package.png)

Al igual que los permisos para la creación de un cubo de Amazon S3, el contenido en el sector de almacenamiento también debes tener permisos de **concesión de acceso de lectura público a este objeto** , **escribir**y **leer**.

Si quieres probar la carga de una página web, pero no tienes uno, puedes usar la página html de muestra (default.html) desde el [Proyecto de inicio](https://github.com/AppInstaller/MySampleWebApp/blob/master/MySampleWebApp/default.html).

> [!IMPORTANT]
> Antes de cargar la página web, confirma que la referencia de paquete de la aplicación en la página web es correcta. 

Para obtener la referencia del paquete de aplicación, carga el paquete de la aplicación en primer lugar y copia la dirección URL del paquete. Editar la página web de html para reflejar la ruta de acceso del paquete de aplicación correcto. Vea el ejemplo de código para obtener más detalles. 

Selecciona el archivo de paquete de aplicación cargados para obtener el vínculo de referencia para el paquete de la aplicación, debe ser similar a este ejemplo:

![ruta de acceso del paquete cargados](images/aws-package-path.png)

**Copia** el vínculo a la aplicación del paquete y agrega la referencia en la página web. 

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
Carga el archivo html en el período de Amazon S3. Recuerda que tienes que establecer los permisos para permitir el acceso de **lectura** y **escritura** .

## <a name="step-4---test"></a>Paso 4: prueba

Una vez que se carga la página web en el período de Amazon S3, Obtén el vínculo a la página web seleccionando el archivo html cargados.

Usa el vínculo para abrir la página web. Dado que establecemos permisos para conceder acceso público a la página de web y el paquete de la aplicación, cualquier persona que tenga el vínculo a la página web podrán tener acceso a él e instalar los paquetes de aplicación para UWP mediante el instalador de aplicación. Ten en cuenta que el instalador de aplicación es parte de la plataforma de Windows 10. Como desarrollador, no es necesario agregar código adicional o características a tu aplicación para habilitar el uso del instalador de aplicación. 

## <a name="troubleshooting"></a>Solución de problemas

### <a name="app-installer-fails-to-install"></a>No se puede instalar el instalador de aplicación 

Instalación de la aplicación se producirá un error si el certificado que se firmó el paquete de aplicación con no está instalado en el dispositivo. Para corregir esto, debes instalar el certificado antes de instalar la aplicación. Si hospedas un paquete de la aplicación para distribución pública, se recomienda para firmar el paquete de la aplicación con un certificado de una entidad de certificación. 


