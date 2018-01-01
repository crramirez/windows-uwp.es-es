---
author: laurenhughes
title: "Instalación de aplicaciones para UWP desde una página web"
description: "En esta sección, revisaremos los pasos que debes llevar a cabo para permitir que los usuarios instalen tus aplicaciones directamente desde la página web."
ms.author: lahugh
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, instalador de aplicación, AppInstaller, instalación de prueba, conjunto relacionado, paquetes opcionales"
localizationpriority: medium
ms.openlocfilehash: 00301a61bf4e47659707af883739de57e238c681
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2017
---
# <a name="installing-uwp-apps-from-a-web-page"></a>Instalación de aplicaciones para UWP desde una página web

Por lo general, una app debe estar disponible de forma local en un dispositivo para que se puede instalar con el Instalador de aplicación. Para el escenario web, esto significa que el usuario debe descargar el paquete de la aplicación desde el servidor web, tras lo cual se puede instalar con el Instalador de aplicación. Esto resulta ineficaz y desperdicia espacio en el disco, motivo por el cual el Instalador de aplicación tiene ahora características integradas para agilizar el proceso.

El Instalador de aplicación puede instalar una app directamente desde un servidor web. Cuando el usuario hace clic en un vínculo web hospedado de paquete de la aplicación, el Instalador de aplicación se invoca automáticamente. Al usuario se le lleva entonces a la vista de información de la aplicación del Instalador de aplicación y solo tiene que hacer clic para interactuar directamente con la aplicación. 

La instalación de la aplicación directa solo está disponible en Windows 10 Fall Creators Update y versiones más recientes. Las versiones anteriores de Windows (volviendo a la Actualización de aniversario de Windows 10) se admitirán por la [experiencia de instalación web en versiones anteriores de Windows 10](#web-install-experience). Esta experiencia no es tan fluida como la instalación de aplicación directa, pero proporciona mejoras significativas para el procedimiento de instalación de aplicaciones existente.
  
> [!NOTE]
> Para admitir las nuevas características, se requiere una versión del Instalador de aplicación superior a la 1.0.12271.0.

### <a name="protocol-activation-scheme"></a>Esquema de activación de protocolos
En este mecanismo, el Instalador de aplicación se registra con con el sistema operativo para un esquema de activación de protocolo. Cuando el usuario hace clic en un vínculo web, el navegador comprueba con el sistema operativo las aplicaciones que están registradas para ese vínculo web. Si el esquema coincide con el esquema de activación de protocolo especificado por el Instalador de aplicación, se invoca el Instalador de aplicación. Es importante tener en cuenta que este mecanismo es independiente del navegador. Esto resulta útil para los administradores de sitios que, por ejemplo, no deben tener en cuenta diferencias de navegador al incorporarlo en una página web. 

#### <a name="requirements-for-protocol-activation-scheme"></a>Requisitos para el esquema de activación de protocolos
   - Servidores web que admiten solicitudes de intervalos de bytes (HTTP/1.1)
   - Los paquetes de aplicaciones deben estar hospedados en servidores que admiten el protocolo HTTP/1.1.   

#### <a name="how-to-enable-this-on-a-webpage"></a>Cómo habilitar esto en una página web 
Los desarrolladores de aplicaciones que quieran hospedar paquetes de aplicaciones en sus sitios web deben seguir este paso:

Asigne un prefijo de sus URI de paquetes de aplicaciones al esquema de activación `'ms-appinstaller:?source='` para el que se registra el Instalador de aplicación cuando hace referencia a ellos en tu página web. Consulta el ejemplo de la **página web de MyApp** para obtener detalles. 
``` html
<html>
    <body>
        <h1> MyApp Web Page </h1>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubApp.appx"> Install app package </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppBundle.appxbundle"> Install app bundle  </a>
        <a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/HubAppSet.appinstaller"> Install related set </a>
    </body>
</html>
```

## La experiencia de instalación web en versiones anteriores de Windows 10<a name="web-install-experience"></a>

La invocación del Instalador de aplicación desde el navegador se admite en todas las versiones de Windows 10 donde el Instalador de aplicación está disponible (a partir de la Actualización de aniversario). Sin embargo, la funcionalidad para instalar directamente desde la web sin necesidad de descargar el paquete primero solo está disponible en Windows 10 Fall Creators Update.  

Los usuarios de versiones anteriores de Windows 10 (con el Instalador de aplicación disponible) también pueden sacar provecho de la instalación web de aplicaciones para UWP a través del Instalador de aplicación, pero tendrán una experiencia de usuario diferente. Cuando estos usuarios hacen clic en el vínculo web, el Instalador de aplicación solicitará **Descargar** el paquete en lugar de **Instalar**. Después de la descarga, el Instalador de aplicación iniciará el lanzamiento del paquete descargado automáticamente. Dado que el paquete de la aplicación se descarga en la web, estos archivos pasarán por Microsoft SmartScreen para una comprobación de seguridad. Una vez que el usuario concede permiso para continuar y se hace clic una vez más en **Instalar**, ¡la aplicación estará lista para usarla!

Aunque este flujo no es tan perfecto como la instalación directa en Windows 10 Fall Creators Update, los usuarios pueden interactuar rápidamente con la aplicación. Además, con este flujo, el usuario no tiene por qué preocuparse de que los archivos del paquete de la aplicación ocupen espacio en las unidades de disco de manera innecesaria. El Instalador de aplicación administra de manera eficaz el espacio al descargar el paquete en su carpeta de datos de la aplicación y eliminar paquetes cuando ya no son necesarios. 

Esta es una comparación rápida de la versión de actualización de Windows 10 Fall Creator del Instalador de aplicación y la versión anterior del Instalador de aplicación:

| Instalador de aplicación, Versión más reciente | Instalador de aplicación, Versión anterior |
|------------------------------|----------------------------------|
| El Instalador de aplicación muestra información de la aplicación antes de que comience la descarga. | El navegador pide al usuario que elija descargar.  |
| El Instalador de aplicación realiza la descarga. | El usuario debe iniciar manualmente el lanzamiento del paquete de la aplicación. |
| Después de la descarga del paquete, el Instalador de aplicación inicia automáticamente el paquete de la aplicación. | El usuario debe hacer clic en **Instalar** e iniciar manualmente el paquete de la aplicación. |
| El Instalador de aplicación se encargará de la eliminación de los paquetes descargados. | El usuario debe eliminar manualmente los archivos descargados. |

## <a name="web-install-experience-on-previous-versions-of-windows-10"></a>La experiencia de instalación web en versiones anteriores de Windows 10
En las versiones anteriores de Windows 10 Fall Creators Update, el Instalador de aplicación no puede instalar directamente una app desde la web. En estas versiones, el Instalador de aplicación solo puede instalar paquetes de la aplicación que están disponibles de forma local. En su lugar, el Instalador de aplicación descargará el paquete y requerirá que el usuario haga doble clic en el paquete descargado que se instalará.


## <a name="microsoft-smartscreen-integration"></a>Integración con MicrosoftSmartScreen

Microsoft SmartScreen siempre ha formado parte del proceso de la instalación para instalar aplicaciones a través del Instalador de aplicación. SmartScreen garantiza que los usuarios están protegidos del contenido malintencionado que puede abrirse camino hasta sus dispositivos. Con la actualización más reciente del Instalador de aplicación, la integración de SmartScreen es más sencilla y eficaz, y ofrece advertencias al instalar aplicaciones desconocidas y proteger los dispositivos de daños. 