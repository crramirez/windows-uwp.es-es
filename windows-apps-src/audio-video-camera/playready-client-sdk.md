---
author: eliotcowley
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: En este tema se describe cómo agregar contenido multimedia protegido con PlayReady, a una aplicación para la Plataforma universal de Windows (UWP).
title: DRM de PlayReady
---

# DRM de PlayReady

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este tema se describe cómo agregar contenido multimedia protegido con PlayReady a una aplicación para la Plataforma universal de Windows (UWP).

DRM de PlayReady permite a los desarrolladores crear aplicaciones para UWP preparadas para proporcionar contenido de PlayReady al usuario mientras se aplican las reglas de acceso definidas por el proveedor de contenido. Esta sección describe los cambios realizados en la tecnología DRM de Microsoft PlayReady para Windows 10 y cómo modificar la aplicación para UWP de PlayReady para que admita los cambios realizados en la versión anterior de Windows 8.1 a la versión de Windows 10.
 
| Tema                                                                     | Descripción                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DRM de hardware](hardware-drm.md)                                           | En este tema se ofrece una descripción general sobre cómo agregar la administración de derechos digitales (DRM) basada en hardware de PlayReady a una aplicación para UWP.                                                                                                                                                                 |
| [Streaming adaptable con PlayReady](adaptive-streaming-with-playready.md) | En este artículo se describe el proceso para agregar streaming adaptable de contenido multimedia con protección de contenido de Microsoft PlayReady a una aplicación para Plataforma universal de Windows (UWP). Esta característica actualmente admite la reproducción de contenido de HTTP Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH). |

## Novedades de la tecnología DRM de PlayReady

En la siguiente lista se describen las nuevas características y los cambios realizados en la tecnología DRM de PlayReady para Windows 10.

-   Se ha agregado la administración de derechos digitales (DRM) de hardware.

    La compatibilidad con la protección de contenido basado en hardware permite la reproducción segura de contenido de alta definición (HD) y altísima definición (UHD) en diversas plataformas de dispositivo. Se protege el material de clave (incluyendo claves privadas, claves de contenido y cualquier otro material de clave que se use para derivar o desbloquear dichas claves), así como muestras de vídeo descifradas comprimidas y descomprimidas mediante el aprovechamiento de la seguridad de hardware. Cuando se usa DRM de hardware, los habilitadores desconocidos (reproducir en desconocido / reproducir en desconocido con downres) no tienen sentido, ya que la canalización HWDRM siempre sabe qué salida se está usando. Para más información, consulta [DRM de hardware](hardware-drm.md).

-   PlayReady ya no es un componente de marco appX, sino que es un componente integrado en el sistema operativo. Por ello, se cambió el espacio de nombres de **Microsoft.Media.PlayReadyClient** a [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454).
-   Los siguientes encabezados que definen los códigos de error de PlayReady ahora forman parte del Kit de desarrollo de software de Windows (SDK): Windows.Media.Protection.PlayReadyErrors.h y Windows.Media.Protection.PlayReadyResults.h.
-   Proporciona la adquisición proactiva de licencias no persistentes.

    Las versiones anteriores de la tecnología DRM de PlayReady no admitían la adquisición proactiva de licencias no persistentes. Se ha agregado esta funcionalidad a esta versión. Esto puede reducir el tiempo para mostrar el primer fotograma. Para más información, consulta el apartado [Adquirir de forma proactiva una licencia no persistente antes de la reproducción](#proactively_acquire_a_non_persistent_license_before_playback).

-   Proporciona la adquisición de varias licencias en un mensaje.

    Permite que la aplicación cliente adquiera varias licencias no persistentes en un solo mensaje de adquisición de licencia. Esto puede reducir el tiempo para mostrar el primer fotograma al adquirir licencias de varias partes del contenido mientras el usuario sigue explorando la biblioteca de contenido. Esto impide que se produzca un retraso en la adquisición de licencia cuando el usuario selecciona el contenido que quiere reproducir. Además, permite que las secuencias de audio y vídeo se cifren en claves independientes al habilitar un encabezado de contenido que incluye varios identificadores de claves (KID). Esto permite que una única adquisición de licencia adquiera todas las licencias para todas las secuencias dentro de un archivo de contenido, en lugar de tener que usar la lógica personalizada y varias solicitudes de adquisición de licencia para lograr el mismo resultado.

-   Se ha agregado soporte para caducidad en tiempo real o una licencia de duración limitada (LDL).

    Proporciona la capacidad de establecer la caducidad en tiempo real en licencias y la transición sin problemas de una licencia que caduca a otra licencia (válida) en medio de una reproducción. Cuando se combina con la adquisición de varias licencias en un mensaje permite que una aplicación adquiera de forma asincrónica varias LDL mientras el usuario continúa explorando la biblioteca de contenido, y solo adquiere una licencia de mayor duración una vez que el usuario ha seleccionado el contenido que quiere reproducir. Por tanto, la reproducción comenzará más rápido (puesto que ya hay una licencia disponible) y, dado que la aplicación habrá adquirido una licencia de mayor duración, en el momento en que la LDL expire, continuará sin problemas la reproducción sin interrupciones hasta el final del contenido.

-   Se han agregado la cadenas de licencia no persistente.
-   Se ha agregado la compatibilidad con las restricciones basadas en tiempo (incluida caducidad, expirar después de primera reproducción y caducidad en tiempo real) en las licencias no persistentes.
-   Se ha agregado la compatibilidad con la directiva de HDCP tipo 1 (versión 2.2).

    Consulta [Cosas a tener en cuenta](#things_to_consider) para obtener más información.

-   Miracast ahora está implícito como salida.
-   Se ha agregado la detención segura.

    La detención segura proporciona los medios para que un dispositivo PlayReady se imponga con confianza a un servicio de transmisión por secuencias de multimedia que la reproducción multimedia ha detenido para un determinado contenido. Esta funcionalidad garantiza que los servicios de transmisión por secuencias de multimedia proporcionen una aplicación precisa e informes de limitaciones de uso en diferentes dispositivos de una cuenta determinada.

-   Se ha agregado la separación de la licencia de audio y vídeo.

    Pistas independientes impiden que el vídeo se descodifique como audio, lo cual permite una protección de contenido más sólida. Los estándares emergentes requieren claves independientes para pistas de audio y visuales.

-   Se ha agregado MaxResDecode.

    Esta función se agregó para limitar la reproducción de contenido a una resolución máxima incluso cuando se posee una clave más capaz (pero no una licencia). Admite casos en los que se codifican varios tamaños de flujo con una única clave.

Se agregaron a la tecnología DRM de PlayReady las siguientes nuevas interfaces, clases y enumeraciones:

-   Interfaz [
              **IPlayReadyLicenseAcquisitionServiceRequest**
            ](https://msdn.microsoft.com/library/windows/apps/dn986077)
-   Interfaz [
              **IPlayReadyLicenseSession**
            ](https://msdn.microsoft.com/library/windows/apps/dn986080)
-   Interfaz [
              **IPlayReadySecureStopServiceRequest**
            ](https://msdn.microsoft.com/library/windows/apps/dn986090)
-   Clase [
              **PlayReadyLicenseSession**
            ](https://msdn.microsoft.com/library/windows/apps/dn986309)
-   Clase [
              **PlayReadySecureStopIterable**
            ](https://msdn.microsoft.com/library/windows/apps/dn986371)
-   Clase [
              **PlayReadySecureStopIterator**
            ](https://msdn.microsoft.com/library/windows/apps/dn986375)
-   Enumerador [
              **PlayReadyHardwareDRMFeatures**
            ](https://msdn.microsoft.com/library/windows/apps/dn986265)

Se ha creado una nueva muestra para mostrar cómo usar las nuevas características de la tecnología DRM de PlayReady. Dicha muestra se puede descargar desde [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670).

## Cosas a tener en cuenta

-   DRM de PlayReady ahora admite la especificación HDCP de tipo 1 (versión 2.2 o posterior). PlayReady cumple con la directiva en la licencia que debe aplicar el dispositivo. Esta característica puede habilitarse en la licencia del SDK del servidor PlayReady v3.0 (el servidor controla esta directiva en la licencia mediante el habilitador de reproducción **GUID**). Para más información, consulta [PlayReady Compliance and Robustness Rules (Reglas de solidez y cumplimiento de PlayReady)](http://www.microsoft.com/playready/licensing/compliance/).
-   Windows Media Video (también conocido como VC-1) no es compatible con la tecnología DRM de hardware (consulta [Invalidar DRM de hardware](hardware-drm.md#override-hardware-drm)).
-   La tecnología DRM de PlayReady es ahora compatible con el estándar de compresión de vídeo Codificación de vídeo de alta eficiencia (HEVC /H.265). Para admitir HEVC, la aplicación debe usar contenido del esquema de cifrado común (CENC) versión 2 que incluye dejar desactivados los encabezados de división de contenido. Consulta el estándar "ISO/IEC 23001-7 Information technology -- MPEG systems technologies -- Part 7: Common encryption in ISO base media file format files" (Es necesaria la versión ISO/IEC 23001-7:2015 o posterior) para obtener más información. Microsoft también recomienda usar CENC versión 2 para todo el contenido de HWDRM. Además, varias tecnologías DRM de hardware admitirán HEVC y otras no lo harán (consulta [Invalidar DRM de hardware](hardware-drm.md#override-hardware-drm)).
-   Para aprovechar las ventajas de determinadas características nuevas de PlayReady 3.0 (lo que incluye, entre otras cosas, SL3000 para los clientes basados en hardware, la adquisición de varias licencias no persistentes en un mensaje de adquisición de licencia y restricciones basadas en el tiempo o licencias no persistentes); asimismo, es necesario que el servidor PlayReady sea la versión de lanzamiento del Kit de desarrollo de Software del servidor de Microsoft PlayReady v3.0.2769 o posterior.
-   En función de la directiva sobre protección de salida especificada en la licencia de contenido, los usuarios finales podrían encontrarse con errores en la reproducción de multimedia si la salida conectada no es compatible con estos requisitos. La siguiente tabla enumera el conjunto de errores comunes que se producen como consecuencia de lo anterior. Para más información, consulta [PlayReady Compliance and Robustness Rules (Reglas de solidez y cumplimiento de PlayReady)](http://www.microsoft.com/playready/licensing/compliance/).

| Error                                                   | Valor      | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | La Directiva de protección de salida de la licencia requiere que el monitor active la especificación HDCP, pero no se pudo activar.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | La Directiva de protección de salida de la licencia requiere que el monitor active la especificación HDCP de tipo 1, pero no se pudo activar.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | Este código de error solo se produce cuando se ejecuta en DRM de hardware. La directiva de protección de salida de la licencia requiere que el monitor active el HDCP o reduzca la resolución eficaz del contenido, pero no se puede activar el HDCP y la resolución eficaz del contenido no se puede reducir porque DRM de hardware no admite que se reduzca la resolución del contenido. En la tecnología DRM de software, el contenido sí se reproduce. Consulta [Consideraciones para el uso de DRM de hardware](hardware-drm.md#considerations-for-using-hardware-drm). |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | El controlador de elementos gráficos no admite la protección de salida. Por ejemplo, el monitor está conectado a través de VGA o no está instalado un controlador de elementos gráficos adecuado para la salida digital. En este último caso, el controlador típico que está instalado es el Adaptador de pantalla básico de Microsoft y el problema se resuelve instalando un controlador de elementos gráficos adecuado.                                                                                                                                                  |

## Requisitos previos

Antes de empezar a crear la aplicación para UWP protegida con PlayReady, el sistema debe tener instalado el siguiente software:

-   Windows 10.
-   Debes usar Microsoft Visual Studio 2015 o una versión posterior si pretendes compilar cualquiera de las muestras para aplicaciones para UWP para DRM de PlayReady. Todavía se puede usar Microsoft Visual Studio 2013 para compilar cualquiera de las muestras de la tecnología DRM de PlayReady para aplicaciones de la Tienda para Windows 8.1.

Si vas a reproducir contenido MPEG-2/H.262 en la aplicación, también debes descargar e instalar [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876).

## Guía de migración de la aplicación de la Tienda Windows PlayReady

Esta sección incluye información sobre cómo migrar las aplicaciones PlayReady de la Tienda Windows 8.x existentes a Windows 10.

El espacio de nombres de las aplicaciones para UWP en Windows 10 se cambió de **Microsoft.Media.PlayReadyClient** a [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454). Esto significa que tendrás que buscar y reemplazar el espacio de nombres antiguo por uno nuevo en el código. Seguirás haciendo referencia a un archivo winmd. Forma parte de windows.media.winmd en el sistema operativo Windows 10. Está en el windows.winmd como parte del Windows SDK de TH. Para UWP, se hace referencia en windows.foundation.univeralappcontract.winmd.

Para reproducir contenido protegido por PlayReady de alta definición (HD) (1080p) y contenido de altísima definición (UHD), tendrás que implementar DRM de hardware PlayReady. Para obtener información sobre cómo implementar la tecnología DRM de hardware PlayReady, consulta [DRM de hardware](hardware-drm.md).

Algunos contenidos no son compatibles con la tecnología DRM de hardware. Para obtener información acerca de cómo deshabilitar la tecnología DRM de hardware y habilitar DRM de software, consulta [Invalidar DRM de hardware](hardware-drm.md#override-hardware-drm).

En lo relacionado con el administrador de protección multimedia, asegúrate de que el código tenga la siguiente configuración si es que no la tiene ya:

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## Adquirir de forma proactiva una licencia no persistente antes de la reproducción

En esta sección se describe cómo obtener licencias no persistentes de forma proactiva antes de que empiece la reproducción.

En versiones anteriores del DRM de PlayReady, las licencias no persistentes solo se podían obtener de forma reactiva durante la reproducción. En esta versión, puedes adquirir licencias no persistentes de forma proactiva antes de que empiece la reproducción.

1.  Crear de forma proactiva una sesión de reproducción donde se pueda almacenar la licencia no persistente. Por ejemplo:

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  Vincula esa sesión de reproducción a la clase de adquisición de licencia. Por ejemplo:

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  Crea una solicitud de servicio de licencia. Por ejemplo:

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  Realiza la adquisición de licencia con la solicitud de servicio creada en el paso 3. La licencia se almacenará en la sesión de reproducción.
5.  Vincula la sesión de reproducción al origen multimedia de la reproducción. Por ejemplo:

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## Agregar detención segura

Esta sección describe cómo agregar detención segura a aplicaciones para UWP.

La detención segura proporciona los medios para que un dispositivo PlayReady se imponga con confianza a un servicio de transmisión por secuencias de multimedia que la reproducción multimedia ha detenido para un determinado contenido. Esta funcionalidad garantiza que los servicios de transmisión por secuencias de multimedia proporcionen una aplicación precisa e informes de limitaciones de uso en diferentes dispositivos de una cuenta determinada.

Hay dos escenarios principales para el envío de un desafío de detención segura:

-   Cuando se detiene la presentación multimedia porque se ha alcanzado el final del contenido o cuando el usuario detiene la presentación multimedia en algún lugar en mitad del proceso.
-   Cuando la sesión anterior termina inesperadamente (por ejemplo, debido a un bloqueo del sistema o la aplicación). La aplicación tendrá que consultar, ya sea al inicio o apagado, las sesiones de detención segura pendientes y enviar los desafíos independientemente de otras reproducciones de multimedia.

Para una implementación de muestra de detención segura, consulta el archivo securestop.cs que encontrarás en la muestra de PlayReady ubicada en [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670).

 

 






<!--HONumber=May16_HO2-->


