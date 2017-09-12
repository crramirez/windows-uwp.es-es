---
author: seksenov
title: "Aplicaciones web hospedadas: Acceso a las características de la Plataforma universal de Windows (UWP) y a las API de tiempo de ejecución"
description: "Acceso a las características nativas de la Plataforma universal de Windows y a las API de tiempo de ejecución de Windows10, incluidos los comandos de voz de Cortana, los iconos dinámicos las ACUR para seguridad, OpenID y OAuth, todo ello desde JavaScript remoto."
kw: Hosted Web Apps, Accessing Windows 10 features from remote JavaScript, Building a Win10 Web Application, Windows JavaScript Apps, Microsoft Web Apps, HTML5 app for PC, ACUR URI Rules for Windows App, Call Live Tiles with web app, Use Cortana with web app, Access Cortana from website, msapplication-cortanavcd
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Hosted Web Apps, WinRT APIs for JavaScript, Win10 web app, Windows JavaScript app, ApplicationContentUriRules, ACURs, msapplication-cortanavcd, Cortana for web apps, aplicaciones web hospedadas,API de WinRT para JavaScript, aplicación web de Win10, aplicación de JavaScript de Windows, Cortana para aplicaciones web"
ms.openlocfilehash: 86661353916e64cb2ed4d7f0ca7b8830bfe95685
ms.sourcegitcommit: a704e3c259400fc6fbfa5c756c54c12c30692a31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2017
---
# <a name="accessing-uwp-features"></a>Acceder a las características de UWP

La aplicación web puede tener acceso completo a la Plataforma universal de Windows (UWP), activar las características nativas en dispositivos Windows [que se benefician de la seguridad de Windows](#keep-your-app-secure--setting-application-content-uri-rules-acurs), [llamar a API de Windows Runtime](#call-windows-runtime-apis) directamente desde script hospedado en un servidor, aprovechar [la integración de Cortana](#integrate-cortana-voice-commands)y usar un [proveedor de autenticación en línea](#web-authentication-broker). También se admiten [aplicaciones híbridas](##create-hybrid-apps--packaged-web-apps-vs-hosted-web-apps), ya que puedes incluir código local para llamarlo desde el script hospedado y administrar la navegación en aplicaciones entre páginas locales y remotas.

## <a name="keep-your-app-secure--setting-application-content-uri-rules-acurs"></a>Mantener tu aplicación segura: Configuración de las reglas de URI de contenido de la aplicación (ACUR)

A través de las ACUR, también conocidas como una lista de URL permitidas, es posible proporcionar acceso directo remoto de direcciones URL a las API de Windows Universal desde HTML, CSS y JavaScript remotos. En el nivel del sistema operativo Windows, los límites de la directiva de derechos se han establecido para permitir que el código hospedado en el servidor web llame directamente a las API de la plataforma. Estos límites se definen en el manifiesto del paquete de la aplicación al colocar el conjunto de direcciones URL que componen la aplicación web hospedada en las reglas de URI de contenido de aplicación (ACUR). Las reglas deben incluir la página de inicio de la aplicación y cualquier otra página que desees incluir como páginas de la aplicación. Opcionalmente, también puedes excluir determinadas direcciones URL.

Hay varias formas de especificar una coincidencia de URL en tus reglas:

- Un nombre de host exacto
- Se incluye un nombre de host para el que se incluya o excluya un URI con un subdominio cualquiera de ese nombre de host
- Un URI exacto
- Un URI exacto que pueda contener una propiedad de consulta
- Una ruta de acceso parcial y un comodín para indicar una extensión de archivo concreta para una regla de inclusión.
- Rutas de acceso relativas para reglas de exclusión

Si el usuario navega a una dirección URL que no se incluya en las reglas, Windows abrirá dicha dirección URL de destino en un explorador.

Estos son algunos ejemplos de ACUR:

```HTML
<Application
Id="App"
StartPage="https://contoso.com/home">
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="https://contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="include" Match="https://*.contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="exclude" Match="https://contoso.com/excludethispage.aspx" />
</uap:ApplicationContentUriRules>
```

## <a name="call-windows-runtime-apis"></a>Llamar a API de Windows Runtime

Si se ha definido una dirección URL dentro de los límites de la aplicación (ACUR), esta puede llamar a las API de Windows Runtime con JavaScript mediante el atributo "WindowsRuntimeAccess". El espacio de nombres de Windows se insertará y estará presente en el motor de scripts cuando en el host de la aplicación se cargue una dirección URL con el acceso adecuado. Gracias a esto, las API universales de Windows están disponibles para que los scripts de la aplicación las llamen directamente. Como desarrollador, solo necesitas detectar la API de Windows que te gustaría llamar y, si está disponible, pasar a destacar las características de la plataforma.

Para habilitar esto, debes especificar el atributo `(WindowsRuntimeAccess="<<level>>")` en las ACUR con uno de estos valores:

- **all**: el código de JavaScript remoto tiene acceso a todas las API de la UWP y a cualquier componente local empaquetado.
- **allowForWeb**: el código de JavaScript remoto tiene acceso únicamente al código personalizado en el paquete. Acceso local a componentes C++/C# personalizados.
- **none**: valor predeterminado. La dirección URL especificada no tiene acceso a la plataforma.

A continuación se facilita un tipo de regla de ejemplo:

```HTML
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="http://contoso.com/" WindowsRuntimeAccess="all"  />
</uap:ApplicationContentUriRules>
```

Esto proporciona al script que se ejecuta en https://contoso.com/ acceso a espacios de nombres de Windows Runtime y a componentes empaquetados personalizados del paquete. Consulta el ejemplo [Windows.UI.Notifications.js](https://gist.github.com/Gr8Gatsby/3d471150e5b317eb1813#file-windows-ui-notifications-js) en GitHub para obtener notificaciones del sistema.

Este es un ejemplo de cómo implementar un icono dinámico y actualizarlo desde JavaScript remoto:

```Javascript
function updateTile(message, imgUrl, imgAlt) {
    // Namespace: Windows.UI.Notifications

    if (typeof Windows !== 'undefined'&&
            typeof Windows.UI !== 'undefined' &&
            typeof Windows.UI.Notifications !== 'undefined') {  
        var notifications = Windows.UI.Notifications,
        tile = notifications.TileTemplateType.tileSquare150x150PeekImageAndText01,
        tileContent = notifications.TileUpdateManager.getTemplateContent(tile),
        tileText = tileContent.getElementsByTagName('text'),
        tileImage = tileContent.getElementsByTagName('image');  
        tileText[0].appendChild(tileContent.createTextNode(message || 'Demo Message'));
        tileImage[0].setAttribute('src', imgUrl || 'https://unsplash.it/150/150/?random');
        tileImage[0].setAttribute('alt', imgAlt || 'Random demo image');    
        var tileNotification = new notifications.TileNotification(tileContent);
        var currentTime = new Date();
        tileNotification.expirationTime = new Date(currentTime.getTime() + 600 * 1000);
        notifications.TileUpdateManager.createTileUpdaterForApplication().update(tileNotification);
    } else {
        //Alternative behavior

    }
}
```

Este código producirá un icono que se parecerá al siguiente:

![Windows10 llama a un icono dinámico](images/hwa-to-uwp/hwa_livetile.png)

Llama a API de Windows Runtime con el entorno o la técnica que te resulte más familiar manteniendo los recursos en una característica de servidor que detecte las funcionalidades de Windows antes de llamarlas. Si las funcionalidades de la plataforma no están disponibles y la aplicación web se ejecuta en otro host, puedes proporcionar al usuario una experiencia predeterminada estándar que funcione en el explorador.

## <a name="integrate-cortana-voice-commands"></a>Integrar comandos de voz de Cortana

Puedes aprovechar la integración de Cortana si especificas un archivo de definición de comandos de voz (VCD) en la página html. El archivo VCD es un archivo xml que asigna comandos a frases concretas. Por ejemplo, un usuario podría tocar el botón Inicio y decir “Contoso Books, mostrar más vendidos” para iniciar la aplicación llamada Contoso Books e ir a una página de “más vendidos”.

Cuando agregas una etiqueta de elemento `<meta>` que muestra la ubicación del archivo VCD, Windows automáticamente descarga y registra el archivo de definición de comandos de voz.

A continuación se muestra un ejemplo del uso de la etiqueta en una página html de una aplicación web hospedada:

```HTML
<meta name="msapplication-cortanavcd" content="https:// contoso.com/vcd.xml"/>
```

Para obtener más información sobre la integración de Cortana y los archivos VCD, consulta Interacciones de Cortana y elementos y atributos de definición de comandos de voz (VCD) v1.2.

## <a name="create-hybrid-apps--packaged-web-apps-vs-hosted-web-apps"></a>Creación de aplicaciones híbridas: Aplicaciones web empaquetadas frente a aplicaciones web hospedados

Tienes opciones para crear la aplicación para UWP. La aplicación puede diseñarse para que se descargue desde la Tienda Windows y se hospede en su totalidad en el cliente local. Esto normalmente se denomina **aplicación web empaquetada**. Esto te permite ejecutar la aplicación sin conexión en cualquier plataforma compatible. La aplicación también puede ser una aplicación web completamente hospedada que se ejecute en un servidor web remoto. Esto normalmente se conoce como una **aplicación web hospedada**. Pero también hay una tercera opción: la aplicación puede hospedarse parcialmente en el cliente local y parcialmente en un servidor web remoto. A esta tercera opción la denominamos una **aplicación híbrida** y normalmente se usa el componente **WebView** para hacer que el contenido remoto parezca contenido local. Las aplicaciones híbridas pueden incluir el código de HTML5, CSS y Javascript que se ejecute como un paquete dentro del cliente de la aplicación local y conservar la posibilidad de interactuar con el contenido remoto.

## <a name="web-authentication-broker"></a>Agente de autenticación web

Puedes usar al agente de autenticación web para controlar el flujo de inicio de sesión para los usuarios si tienes un proveedor de identidad en línea que use protocolos de autorización y autenticación de Internet como OpenID o OAuth. Los URI de inicio y final se especifican en una etiqueta `<meta>` en una página html de la aplicación. Windows detecta estos URI y los pasa al agente de autenticación web para completar el flujo de inicio de sesión. El URI de inicio consta de la dirección desde la que envías la solicitud de autenticación a tu proveedor en línea anexada con otra información obligatoria, como un identificador de la aplicación, un URI de redirección donde se envía al usuario después de completar la autenticación, y el tipo de respuesta esperado. Puedes preguntarle al proveedor qué parámetros se requieren. A continuación se muestra un ejemplo del uso de la etiqueta `<meta>`:

```HTML
<meta name="ms-webauth-uris" content="https://<providerstartpoint>?client_id=<clientid>&response_type=token, https://<appendpoint>"/>
```

Para obtener más instrucciones, consulta [Consideraciones sobre el agente de autenticación web para proveedores en línea](../security/web-authentication-broker.md).

## <a name="app-capability-declarations"></a>Declaraciones de funcionalidades de las aplicaciones

Si tu aplicación necesita acceso mediante programación a recursos del usuario, como imágenes o música, o a dispositivos como una cámara o un micrófono, debes declarar la funcionalidad apropiada. Existen 3 categorías de declaración de funcionalidad de aplicación: 

- [Funcionalidades de uso general](https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#general-use-capabilities) que se aplican a la los escenarios de aplicaciones más habituales. 
- [Funcionalidades de dispositivo](https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#device-capabilities) que permiten a tu aplicación acceder a periféricos y dispositivos internos. 
- [Funcionalidades de uso especial](https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations#special-and-restricted-capabilities) que requieren una cuenta de empresa especial para su envío a la Tienda para usarlas. 

Para obtener más información sobre las cuentas de empresa, consulta [Tipos de cuenta, ubicaciones y precios](https://docs.microsoft.com/en-us/windows/uwp/publish/account-types-locations-and-fees).

> [!NOTE]
> Es importante saber que cuando los clientes compren tu aplicación en la Tienda Windows, se les notificarán todas las funcionalidades que la aplicación declara. Por lo tanto, no uses funcionalidades que la aplicación no necesite.

El acceso se solicita mediante la declaración de funcionalidades en el [manifiesto del paquete](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest) de la aplicación. Para obtener más información, consulta estos artículos en el artículo [Packaging for Universal Windows Platform (UWP) apps [Empaquetado de aplicaciones de la Plataforma universal de Windows (UWP)]](https://docs.microsoft.com/en-us/windows/uwp/packaging/index).

Algunas funcionalidades proporcionan a las aplicaciones acceso a un recurso con información confidencial. Estos recursos se consideran confidenciales porque pueden acceder a datos personales del usuario o costarle dinero. La configuración de privacidad, administrada por la aplicación Configuración, permite al usuario controlar dinámicamente el acceso a recursos con información confidencial. Por lo tanto, es importante que la aplicación no suponga que un recurso con información confidencial está siempre disponible. Para obtener más información sobre cómo acceder a recursos confidenciales, consulta las [Directrices para aplicaciones compatibles con la privacidad](https://msdn.microsoft.com/library/windows/apps/hh768223.aspx).

## <a name="manifoldjs-and-the-app-manifest"></a>manifoldjs y el manifiesto de la aplicación

Una manera sencilla de convertir tu sitio web en una aplicación para UWP es usar un **manifiesto de la aplicación** y **manifoldjs**. El manifiesto de la aplicación es un archivo xml que contiene metadatos sobre la aplicación. Especifica puntos como el nombre de la aplicación, vínculos a recursos, el modo de visualización, las direcciones URL y otros datos que describen cómo se debería implementar y ejecutar la aplicación. manifoldjs hace que este proceso sea muy sencillo, incluso en los sistemas que no admitan las aplicaciones web. Para obtener más información sobre su funcionamiento, visita [manifoldjs.com](http://www.manifoldjs.com/). También puedes ver una demostración de manifoldjs como parte de esta [presentación de aplicaciones web para Windows10](http://channel9.msdn.com/Events/WebPlatformSummit/2015/Hosted-web-apps-and-web-platform-innovations?wt.mc_id=relatedsession).

## <a name="related-topics"></a>Temas relacionados
- [API de Windows Runtime: Muestras de código JavaScript](https://microsoft.github.io/WindowsRuntimeAPIs_Javascript_snippets/)
- [Codepen: Espacio aislado que usar para llamar a las API de Windows Runtime](http://codepen.io/seksenov/pen/wBbVyb/)
- [Interacciones de Cortana](https://developer.microsoft.com/en-us/cortana)
- [Elementos y atributos de definición de comandos de voz (VCD) v1.2](https://msdn.microsoft.com/library/windows/apps/dn954977.aspx)
- [Consideraciones sobre el agente de autenticación web para proveedores en línea](https://docs.microsoft.com/en-us/windows/uwp/security/web-authentication-broker)
- [Declaraciones de funcionalidades de las aplicaciones](https://docs.microsoft.com/en-us/windows/uwp/packaging/app-capability-declarations)
