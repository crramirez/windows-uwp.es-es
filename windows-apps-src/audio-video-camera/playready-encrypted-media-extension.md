---
author: drewbatgit
ms.assetid: 79C284CA-C53A-4C24-807E-6D4CE1A29BFA
description: En esta sección se describe cómo modificar la aplicación web de PlayReady para admitir los cambios realizados en la versión anterior de Windows8.1 a la versión de Windows 10.
title: Encrypted Media Extension (EME) de PlayReady
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b73464ea10aa835b82df17605e983ebdfb9cd890
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5939597"
---
# <a name="playready-encrypted-media-extension"></a>Encrypted Media Extension (EME) de PlayReady



En esta sección se describe cómo modificar la aplicación web de PlayReady para admitir los cambios realizados en la versión anterior de Windows8.1 a la versión de Windows 10.

El uso de elementos multimedia de PlayReady en Internet Explorer permite a los desarrolladores crear aplicaciones web capaces de proporcionar contenido de PlayReady al usuario a la vez que aplica las reglas de acceso definidas por el proveedor de contenido. En esta sección se describe cómo agregar elementos multimedia de PlayReady a tus aplicaciones web existentes usando solo HTML5 y JavaScript.

## <a name="whats-new-in-playready-encrypted-media-extension"></a>Novedades de Encrypted Media Extension de PlayReady

En esta sección se proporciona una lista de los cambios realizados en PlayReady cifrados Media Extension (EME) para habilitar la protección de contenido de PlayReady en Windows 10.

La siguiente lista describe las nuevas características y los cambios realizados en Encrypted Media Extension de PlayReady para Windows 10:

-   Se ha agregado la administración de derechos digitales (DRM) de hardware.

    La compatibilidad con la protección de contenido basado en hardware permite la reproducción segura de contenido de alta definición (HD) y altísima definición (UHD) en diversas plataformas de dispositivo. Se protege el material de clave (incluyendo claves privadas, claves de contenido y cualquier otro material de clave que se use para derivar o desbloquear dichas claves), así como muestras de vídeo descifradas comprimidas y descomprimidas mediante el aprovechamiento de la seguridad de hardware.

-   Proporciona la adquisición proactiva de licencias no persistentes.
-   Proporciona la adquisición de varias licencias en un mensaje.

    Puede usar un objeto PlayReady con varios identificadores de clave (ID) al igual que en Windows8.1 o usar los [datos de modelo de descifrado de contenido (CDMData)](https://go.microsoft.com/fwlink/p/?LinkID=626819) con varios Id.

    > [!NOTE]
    > En Windows 10, se admiten varios identificadores de clave en &lt;ID&gt; en CDMData.

-   Se ha agregado soporte para la caducidad en tiempo real o una licencia de duración limitada (LDL).

    Proporciona la capacidad de establecer la caducidad en tiempo real en las licencias.

-   Se ha agregado la compatibilidad con la directiva de HDCP de tipo 1 (versión 2.2).
-   Miracast ahora está implícito como salida.
-   Se ha agregado la detención segura.

    La detención segura proporciona los medios para que un dispositivo PlayReady se imponga con confianza a un servicio de transmisión por secuencias de multimedia que la reproducción multimedia ha detenido para un determinado contenido.

-   Se ha agregado la separación de la licencia de audio y vídeo.

    Pistas independientes impiden que el vídeo se descodifique como audio, lo cual permite una protección de contenido más sólida. Los estándares emergentes requieren claves independientes para pistas de audio y visuales.

-   Se ha agregado MaxResDecode.

    Esta función se agregó para limitar la reproducción de contenido a una resolución máxima incluso cuando se posee una clave más capaz (pero no una licencia). Admite casos en los que se codifican varios tamaños de flujo con una única clave.

## <a name="encrypted-media-extension-support-in-playready"></a>Compatibilidad con Encrypted Media Extension en PlayReady

En esta sección se describe la versión de Encrypted Media Extension de W3C compatible con PlayReady.

PlayReady para aplicaciones web está actualmente enlazado al [borrador de la extensión multimedia cifrada (EME) de W3C del 10 de mayo de 2013](http://www.w3.org/TR/2013/WD-encrypted-media-20130510/). En versiones futuras de Windows se cambiará esta compatibilidad de acuerdo con la especificación de EME actualizada.

## <a name="use-hardware-drm"></a>Usar el DRM de hardware

En esta sección se describe cómo la aplicación web puede usar DRM de hardware de PlayReady y cómo deshabilitar DRM de hardware si el contenido protegido no lo admite.

Para usar la tecnología DRM de hardware de PlayReady, la aplicación web de JavaScript debe usar el método EME **isTypeSupported** con un identificador de clave del sistema de `com.microsoft.playready.hardware` para poder consultar la compatibilidad del explorador con la tecnología DRM de hardware de PlayReady.

En ocasiones algunos contenidos no son compatibles con DRM de hardware. El contenido Cocktail nunca se admite en DRM de hardware; si quieres reproducir este tipo de contenido debes desactivar DRM de hardware. Cierto DRM de hardware admitirá HEVC y otro no lo hará. Si quieres reproducir contenido HEVC y DRM de hardware no lo admite, también tendrás que desactivarlo.

> [!NOTE]
> Para determinar si se admite el contenido HEVC, después de crear una instancia de `com.microsoft.playready`, usa el método [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441).

## <a name="add-secure-stop-to-your-web-app"></a>Agregar la detención segura a aplicaciones web

En esta sección se describe cómo agregar la detención segura a aplicaciones web.

La detención segura proporciona los medios para que un dispositivo PlayReady se imponga con confianza a un servicio de transmisión por secuencias de multimedia que la reproducción multimedia ha detenido para un determinado contenido. Esta funcionalidad garantiza que los servicios de transmisión por secuencias de multimedia proporcionen una aplicación precisa e informes de limitaciones de uso en diferentes dispositivos de una cuenta determinada.

Hay dos escenarios principales para el envío de un desafío de detención segura:

-   Cuando se detiene la presentación multimedia porque se ha alcanzado el final del contenido o cuando el usuario detiene la presentación multimedia en algún lugar en mitad del proceso.
-   Cuando la sesión anterior termina inesperadamente (por ejemplo, debido a un bloqueo del sistema o la aplicación). La aplicación tendrá que consultar, ya sea al inicio o apagado, las sesiones de detención segura pendientes y enviar los desafíos independientemente de otras reproducciones de multimedia.

En los siguientes procedimientos se describe cómo configurar la detención segura para diferentes escenarios.

Configurar la detención segura para conseguir un final normal de una presentación:

1.  Registra el evento **onEnded** antes de que comience la reproducción.
2.  El controlador de eventos **onEnded** debe llamar a `removeAttribute(“src”)` desde el objeto de elemento de vídeo o audio para establecer el origen en **NULL**, lo cual desencadenará que Media Foundation interrumpa la topología, destruya los descifradores y establezca el estado de parada.
3.  Puedes iniciar sesión de CDM de la detención segura dentro del controlador para enviar el desafío de detención segura al servidor para que notifique que la reproducción ha parado en ese momento, pero también se puede hacer más tarde.

Configurar la detención segura si el usuario abandona la página o cierra la pestaña o el explorador:

-   No se requiere ninguna acción por parte de la aplicación para registrar el estado de parada. Se registrará para ti.

Configurar la detención segura para los controles de página personalizados o las acciones del usuario (como los botones de navegación personalizados o iniciar una nueva presentación antes de que se complete la presentación actual):

-   Cuando se produce una acción de usuario personalizada, la aplicación debe establecer el origen en **NULL**, lo cual desencadenará que Media Foundation interrumpa la topología, destruya los descifradores y establezca el estado de parada.

El siguiente ejemplo muestra cómo usar la detención segura en la aplicación web:

```JavaScript
// JavaScript source code

var g_prkey = null;
var g_keySession = null;
var g_fUseSpecificSecureStopSessionID = false;
var g_encodedMeteringCert = 'Base64 encoded of your metering cert (aka publisher cert)';

// Note: g_encodedLASessionId is the CDM session ID of the proactive or reactive license acquisition 
//       that we want to initiate the secure stop process.
var g_encodedLASessionId = null;

function main()
{
    ...

    g_prkey = new MSMediaKeys("com.microsoft.playready");

    ...

    // add 'onended' event handler to the video element
    // Assume 'myvideo' is the ID of the video element
    var videoElement = document.getElementById("myvideo");
    videoElement.onended = function (e) { 

        //
        // Calling removeAttribute("src") will set the source to null
        // which will trigger the MF to tear down the topology, destroy the
        // decryptor(s) and set the stop state.  This is required in order
        // to set the stop state.
        //
        videoElement.removeAttribute("src");
        videoElement.load();

        onEndOfStream();
    };
}

function onEndOfStream()
{
    ...

    createSecureStopCDMSession();

    ...    
}

function createSecureStopCDMSession()
{
    try{    
        var targetMediaCodec = "video/mp4";
        var customData = "my custom data";

        var encodedSessionId = g_encodedLASessionId;
        if( !g_fUseSpecificSecureStopSessionID )
        {
            // Use "*" (wildcard) as the session ID to include all secure stop sessions
            // TODO: base64 encode "*" and place encoded result to encodedSessionId
        }

        var int8ArrayCDMdata = formatSecureStopCDMData( encodedSessionId, customData,  g_encodedMeteringCert );
        var emptyArrayofInitData = new Uint8Array();

        g_keySession = g_prkey.createSession(targetMediaCodec, emptyArrayofInitData, int8ArrayCDMdata);

        addPlayreadyKeyEventHandler();

    } catch( e )
    {
        // TODO: Handle exception
    }
}

function addPlayreadyKeyEventHandler()
{
    // add 'keymessage' eventhandler   
    g_keySession.addEventListener('mskeymessage', function (event) {

        // TODO: Get the keyMessage from event.message.buffer which contains the secure stop challenge
        //       The keyMessage format for the secure stop is similar to LA as below:
        //
        //            <PlayReadyKeyMessage type="SecureStop" >
        //              <SecureStop version="1.0" >
        //                <Challenge encoding="base64encoded">
        //                    secure stop challenge
        //                </Challenge>
        //                <HttpHeaders>
        //                    <HttpHeader>
        //                      <name>Content-Type</name>
        //                         <value>"content type data"</value>
        //                    </HttpHeader>
        //                    <HttpHeader>
        //                         <name>SOAPAction</name>
        //                         <value>soap action</value>
        //                     </HttpHeader>
        //                    ....
        //                </HttpHeaders>
        //              </SecureStop>
        //            </PlayReadyKeyMessage>
                
        // TODO: send the secure stop challenge to a server that handles the secure stop challenge

        // TODO: Recevie and response and call event.target.Update() to proecess the response
    });
    
    // add 'keyerror' eventhandler
    g_keySession.addEventListener('mskeyerror', function (event) {
        var session = event.target;
        
        ...

        session.close();
    });
    
    // add 'keyadded' eventhandler
    g_keySession.addEventListener('mskeyadded', function (event) {
        
        var session = event.target;

        ...

        session.close();             
    });
}

/**
* desc@ formatSecureStopCDMData
*   generate playready CDMData
*   CDMData is in xml format:
*   <PlayReadyCDMData type="SecureStop">
*     <SecureStop version="1.0">
*       <SessionID>B64 encoded session ID</SessionID>
*       <CustomData>B64 encoded custom data</CustomData>
*       <ServerCert>B64 encoded server cert</ServerCert>
*     </SecureCert>
* </PlayReadyCDMData>        
*/
function formatSecureStopCDMData(encodedSessionId, customData, encodedPublisherCert) 
{
    var encodedCustomData = null;

    // TODO: base64 encode the custom data and place the encoded result to encodedCustomData

    var CDMDataStr = "<PlayReadyCDMData type=\"SecureStop\">" +
                     "<SecureStop version=\"1.0\" >" +
                     "<SessionID>" + encodedSessionId + "</SessionID>" +
                     "<CustomData>" + encodedCustomData + "</CustomData>" +
                     "<ServerCert>" + encodedPublisherCert + "</ServerCert>" +
                     "</SecureStop></PlayReadyCDMData>";
    
    var int8ArrayCDMdata = null

    // TODO: Convert CDMDataStr to Uint8 byte array and palce the converted result to int8ArrayCDMdata

    return int8ArrayCDMdata;
}
```

> [!NOTE]
> El `<SessionID>B64 encoded session ID</SessionID>` de los datos de detención segura del ejemplo descrito anteriormente puede ser un asterisco (*), que es un carácter comodín para todas las sesiones de detención segura registradas. Es decir, la etiqueta **SessionID** puede ser una sesión específica o un carácter comodín (\*) para seleccionar todas las sesiones de detención segura.

## <a name="programming-considerations-for-encrypted-media-extension"></a>Consideraciones de programación para Encrypted Media Extension

En esta sección se enumera las consideraciones de programación que deberías tener en cuenta al crear la aplicación web habilitado de PlayReady para Windows 10.

Los objetos **MSMediaKeys** y **MSMediaKeySession** creados por la aplicación deben mantenerse activos hasta que se cierre la aplicación. Una manera de garantizar que estos objetos se mantengan activos, consiste en asignarlos como variables globales (las variables estarían fuera del ámbito y sujetas a la recolección de elementos no usados si se declaran como una variable local dentro de una función). Por ejemplo, en el siguiente ejemplo se asignan las variables *g\_msMediaKeys* y *g\_mediaKeySession* como variables globales que, a su vez, se asignan a los objetos **MSMediaKeys** y **MSMediaKeySession** de la función.

``` syntax
var g_msMediaKeys;
var g_mediaKeySession;

function foo() {
  ...
  g_msMediaKeys = new MSMediaKeys("com.microsoft.playready");
  ...
  g_mediaKeySession = g_msMediaKeys.createSession("video/mp4", intiData, null);
  g_mediaKeySession.addEventListener(this.KEYMESSAGE_EVENT, function (e) 
  {
    ...
    downloadPlayReadyKey(url, keyMessage, function (data) 
    {
      g_mediaKeySession.update(data);
    });
  });
  g_mediaKeySession.addEventListener(this.KEYADDED_EVENT, function () 
  {
    ...
    g_mediaKeySession.close();
    g_mediaKeySession = null;
  });
}
```

Para obtener más información, consulta las [aplicaciones de muestra](https://code.msdn.microsoft.com/windowsapps/PlayReady-samples-for-124a3738).

## <a name="see-also"></a>Consulta también
- [DRM de PlayReady](playready-client-sdk.md)




