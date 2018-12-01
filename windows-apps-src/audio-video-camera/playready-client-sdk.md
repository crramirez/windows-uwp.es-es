---
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: En este tema se describe cómo agregar contenido multimedia protegido con PlayReady, a una aplicación para la Plataforma universal de Windows (UWP).
title: DRM de PlayReady
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4fac02f892c66a1bcf0b08986ae00a3a162b44ca
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335210"
---
# <a name="playready-drm"></a>DRM de PlayReady



En este tema se describe cómo agregar contenido multimedia protegido con PlayReady a una aplicación para la Plataforma universal de Windows (UWP).

DRM de PlayReady permite a los desarrolladores crear aplicaciones para UWP preparadas para proporcionar contenido de PlayReady al usuario mientras se aplican las reglas de acceso definidas por el proveedor de contenido. En esta sección se describe los cambios realizados en Microsoft DRM de PlayReady para Windows 10 y cómo modificar la aplicación de UWP de PlayReady para admitir los cambios realizados en la versión anterior de Windows8.1 a la versión de Windows 10.
 
| Tema                                                                     | Descripción                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DRM de hardware](hardware-drm.md)                                           | En este tema se ofrece una descripción general sobre cómo agregar la administración de derechos digitales (DRM) basada en hardware de PlayReady a una aplicación para UWP.                                                                                                                                                                 |
| [Streaming adaptable con PlayReady](adaptive-streaming-with-playready.md) | En este artículo se describe el proceso para agregar streaming adaptable de contenido multimedia con protección de contenido de Microsoft PlayReady a una aplicación para Plataforma universal de Windows (UWP). Actualmente, esta característica admite la reproducción de contenido HTTP Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH). |

## <a name="whats-new-in-playready-drm"></a>Novedades de la DRM de PlayReady

La siguiente lista describe las nuevas características y los cambios realizados en DRM de PlayReady para Windows 10.

-   Se ha agregado la administración de derechos digitales de hardware (HWDRM).

    La compatibilidad con la protección de contenido basada en hardware permite la reproducción segura de contenido de alta definición (HD) y altísima definición (UHD) en diversas plataformas de dispositivos. Se protege el material de clave (incluyendo claves privadas, claves de contenido y cualquier otro material de clave que se use para derivar o desbloquear dichas claves), así como muestras de vídeo descifradas comprimidas y descomprimidas mediante el aprovechamiento de la seguridad de hardware. Cuando se usa DRM de hardware, los habilitadores desconocidos (reproducir en desconocido / reproducir en desconocido con downres) no tienen sentido, ya que la canalización HWDRM siempre conoce qué salida se está usando. Para más información, consulta [DRM de hardware](hardware-drm.md).

-   PlayReady ya no es un componente de marco appX, sino que es un componente integrado en el sistema operativo. Se ha cambiado el espacio de nombres de **Microsoft.Media.PlayReadyClient** a [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454).
-   Los siguientes encabezados que definen los códigos de error de PlayReady ahora forman parte del Kit de desarrollo de software de Windows (SDK): Windows.Media.Protection.PlayReadyErrors.h y Windows.Media.Protection.PlayReadyResults.h.
-   Proporciona la adquisición proactiva de licencias no persistentes.

    Las versiones anteriores de la tecnología DRM de PlayReady no admitían la adquisición proactiva de licencias no persistentes. Se ha agregado esta funcionalidad a esta versión. Esto puede reducir el tiempo para mostrar el primer fotograma. Para más información, consulta [Adquirir de forma proactiva una licencia no persistente antes de la reproducción](#proactively-acquire-a-non-persistent-license-before-playback).

-   Proporciona la adquisición de varias licencias en un mensaje.

    Permite que la aplicación cliente adquiera varias licencias no persistentes en un solo mensaje de adquisición de licencia. Esto puede reducir el tiempo para mostrar el primer fotograma al adquirir licencias de varias partes del contenido mientras el usuario sigue explorando la biblioteca de contenido. Esto impide que se produzca un retraso en la adquisición de licencia cuando el usuario selecciona el contenido que quiere reproducir. Además, permite que las secuencias de audio y vídeo se cifren en claves independientes al habilitar un encabezado de contenido que incluye varios identificadores de claves (KID). Esto permite que una única adquisición de licencia adquiera todas las licencias para todas las secuencias dentro de un archivo de contenido, en lugar de tener que usar la lógica personalizada y varias solicitudes de adquisición de licencia para lograr el mismo resultado.

-   Se ha agregado soporte para caducidad en tiempo real o una licencia de duración limitada (LDL).

    Proporciona la capacidad de establecer la caducidad en tiempo real en licencias y la transición sin problemas de una licencia que caduca a otra licencia (válida) en medio de una reproducción. Cuando se combina con la adquisición de varias licencias en un mensaje permite que una aplicación adquiera de forma asincrónica varias LDL mientras el usuario continúa explorando la biblioteca de contenido, y solo adquiere una licencia de mayor duración una vez que el usuario ha seleccionado el contenido que quiere reproducir. Por tanto, la reproducción comenzará más rápido (puesto que ya hay una licencia disponible) y, dado que la aplicación habrá adquirido una licencia de mayor duración, en el momento en que la LDL expire, continuará sin problemas la reproducción sin interrupciones hasta el final del contenido.

-   Se han agregado la cadenas de licencia no persistente.
-   Se ha agregado la compatibilidad con las restricciones basadas en tiempo (incluida la caducidad, la expiración después de la primera reproducción y la caducidad en tiempo real) en las licencias no persistentes.
-   Se ha agregado la compatibilidad con la directiva de HDCP de tipo 1 (versión 2.2 en Windows10).

    Consulta [Cosas a tener en cuenta](#things-to-consider) para obtener más información.

-   Miracast ahora está implícito como salida.
-   Se ha agregado la detención segura.

    La detención segura proporciona los medios para que un dispositivo PlayReady se imponga con confianza a un servicio de transmisión por secuencias de multimedia que la reproducción multimedia ha detenido para un determinado contenido. Esta funcionalidad garantiza que los servicios de transmisión por secuencias de multimedia proporcionen una aplicación precisa e informes de limitaciones de uso en diferentes dispositivos de una cuenta determinada.

-   Se ha agregado la separación de la licencia de audio y vídeo.

    Pistas independientes impiden que el vídeo se descodifique como audio, lo cual permite una protección de contenido más sólida. Los estándares emergentes requieren claves independientes para pistas de audio y visuales.

-   Se ha agregado MaxResDecode.

    Esta función se agregó para limitar la reproducción de contenido a una resolución máxima incluso cuando se posee una clave más capaz (pero no una licencia). Admite casos en los que se codifican varios tamaños de flujo con una única clave.

Se agregaron a la tecnología DRM de PlayReady las siguientes nuevas interfaces, clases y enumeraciones:

-   Interfaz [**IPlayReadyLicenseAcquisitionServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986077)
-   Interfaz [**IPlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986080)
-   Interfaz [**IPlayReadySecureStopServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986090)
-   Clase [**PlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986309)
-   Clase [**PlayReadySecureStopIterable**](https://msdn.microsoft.com/library/windows/apps/dn986371)
-   Clase [**PlayReadySecureStopIterator**](https://msdn.microsoft.com/library/windows/apps/dn986375)
-   Enumerador [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265)

Se ha creado una nueva muestra para ilustrar cómo usar las nuevas características de la tecnología DRM de PlayReady. La muestra puede descargarse en [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670).

## <a name="things-to-consider"></a>Cosas a tener en cuenta

-   La DRM de PlayReady ahora admite HDCP de tipo 1 (admitida en HDCP versión 2.1 o posterior). PlayReady incluye en la licencia una directiva de restricción de tipo de HDCP que el dispositivo debe aplicar. En Windows10, esta directiva exigirá que se use HDCP 2.2 o posterior. Esta característica puede habilitarse en la licencia del SDK del servidor PlayReady v3.0 (el servidor controla esta directiva en la licencia mediante el GUID de restricción de tipo de HDCP). Para obtener más información, consulta [PlayReady Compliance and Robustness Rules](http://www.microsoft.com/playready/licensing/compliance/) (Reglas de solidez y cumplimiento de PlayReady).
-   Windows Media Video (también conocida como VC-1) no es compatible con DRM de hardware (consulta [Invalidar DRM de hardware](hardware-drm.md#override-hardware-drm)).
-   La tecnología DRM de PlayReady es ahora compatible con el estándar de compresión de vídeo Codificación de vídeo de alta eficiencia (HEVC /H.265). Para admitir HEVC, la aplicación debe usar contenido del esquema de cifrado común (CENC) versión 2 que incluye dejar desactivados los encabezados de división de contenido. Consulta el estándar "ISO/IEC 23001-7 Information technology -- MPEG systems technologies -- Part 7: Common encryption in ISO base media file format files" (Es necesaria la versión ISO/IEC 23001-7:2015 o posterior) para obtener más información. Microsoft también recomienda usar CENC versión 2 para todo el contenido de HWDRM. Además, cierto DRM de hardware admitirá HEVC y otro no lo hará (consulta [Invalidar DRM de hardware](hardware-drm.md#override-hardware-drm)).
-   Para aprovechar las ventajas de determinadas características nuevas de PlayReady 3.0 (lo que incluye, entre otros, SL3000 para los clientes basados en hardware, adquisición de varias licencias no persistentes en un mensaje de adquisición de licencia y restricciones basadas en el tiempo o licencias no persistentes), es necesario que el servidor PlayReady sea la versión de lanzamiento del Kit de desarrollo de Software del servidor de Microsoft PlayReady v3.0.2769 o posterior.
-   En función de la directiva sobre protección de salida especificada en la licencia de contenido, los usuarios finales podrían encontrarse con errores en la reproducción de multimedia si la salida conectada no es compatible con estos requisitos. La siguiente tabla enumera el conjunto de errores comunes que se producen como consecuencia de lo anterior. Para obtener más información, consulta [PlayReady Compliance and Robustness Rules (Reglas de solidez y cumplimiento de PlayReady)](http://www.microsoft.com/playready/licensing/compliance/).

| Error                                                   | Valor      | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | La Directiva de protección de salida de la licencia requiere que el monitor active la especificación HDCP, pero no se pudo activar.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | La Directiva de protección de salida de la licencia requiere que el monitor active la especificación HDCP de tipo 1, pero no se pudo activar.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | Este código de error solo se produce cuando se ejecuta en DRM de hardware. La directiva de protección de salida de la licencia requiere que el monitor active el HDCP o reduzca la resolución eficaz del contenido, pero no se puede activar el HDCP y la resolución eficaz del contenido no se puede reducir porque DRM de hardware no admite que se reduzca la resolución del contenido. En DRM de software, el contenido sí se reproduce. Consulta [Consideraciones para el uso de DRM de hardware](hardware-drm.md#considerations-for-using-hardware-drm). |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | El controlador de elementos gráficos no admite la protección de salida. Por ejemplo, el monitor está conectado a través de VGA o no está instalado un controlador de elementos gráficos adecuado para la salida digital. En este último caso, el controlador habitual que se instala es el Adaptador de pantalla básico de Microsoft y el problema se resuelve con la instalación de un controlador de gráficos adecuado.                                                                                                                                                  |

## <a name="output-protection"></a>Protección de salida

En la siguiente sección se describe el comportamiento al usar la DRM de PlayReady para Windows10 con las directivas de protección de salida en una licencia de PlayReady.

La DRM de PlayReady admite los niveles de protección de salida que contiene la **especificación de derechos de multimedia extensible de Microsoft PlayReady**. Este documento puede encontrarse en el paquete de la documentación que se entrega con los productos con licencia de PlayReady.

> [!NOTE]
> Los valores permitidos para los niveles de protección de salida que un servidor de licencias puede establecer se rigen por las [PlayReady Compliance Rules (Reglas de cumplimiento de PlayReady)](https://www.microsoft.com/playready/licensing/compliance/).

La DRM de PlayReady permite la reproducción de contenido con directivas de protección de salida únicamente en conectores de salida del modo especificado en las reglas de cumplimiento de PlayReady. Para obtener más información sobre las condiciones de los conectores de salida especificadas en las reglas de cumplimiento de PlayReady, consulta [Defined Terms for PlayReady Compliance and Robustness Rules](https://www.microsoft.com/playready/licensing/compliance/) (Términos definidos para las reglas de solidez y cumplimiento de PlayReady).

Esta sección se centra en los escenarios de protección de salida con DRM de PlayReady para Windows10 y de DRM de hardware de PlayReady para Windows10, que también está disponible en algunos clientes de Windows. Con la HWDRM de PlayReady, todas las protecciones de salida se aplican desde dentro de la implementación de Windows TEE (consulta [DRM de hardware](hardware-drm.md)). Como resultado, algunos comportamientos varían respecto a cuando se usa la SWDRM (DRM de software) de PlayReady:

* Compatibilidad con el nivel de protección de salida (OPL) para vídeo digital sin comprimir 270: La HWDRM de PlayReady para Windows10 no admite una resolución menor y aplicará el uso de esa HDCP (protección de contenido digital de ancho de banda elevado). Recomendamos que el contenido de alta definición para la HWDRM tenga un OPL mayor de 270 (aunque no es necesario). Además, deberías establecer una restricción de tipo de HDCP en la licencia (HDCP versión 2.2 o posterior).
* A diferencia de la SWDRM, con la HWDRM se aplican protecciones de salida a todos los monitores basadas en el monitor menos capacitado. Por ejemplo, si el usuario tiene dos monitores conectados y uno de ellos es compatible con HDCP y el otro no, se producirá un error en la reproducción si la licencia requiere HDCP, incluso si el contenido solamente se representa en el monitor compatible con HDCP. En el caso de la SWDRM, el contenido se reproducirá siempre que solo se represente en el monitor compatible con HDCP.
* No se garantiza que el cliente pueda usar la HWDRM ni que esta sea segura, a menos que las claves de contenido y las licencias cumplan las siguientes condiciones:
    * La licencia que se use para la clave de contenido de vídeo debe tener un nivel de seguridad mínimo de 3000.
    * El audio debe estar cifrado con una clave de contenido distinta que el vídeo, y la licencia que se use para el audio debe tener un nivel de seguridad mínimo de 2000. Como alternativa, el audio podría dejarse sin cifrar.
* Todos los escenarios de la SWDRM requieren que el nivel mínimo de seguridad de la licencia de PlayReady usado para la clave de contenido de audio o vídeo sea menor o igual a 2000.

### <a name="output-protection-levels"></a>Niveles de protección de salida

En la siguiente tabla se muestran las asignaciones entre varios OPL en la licencia de PlayReady y cómo las aplica la DRM de PlayReady para Windows10.

#### <a name="video"></a>Video

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Vídeo digital comprimido</th>
        <th colspan="2">Vídeo digital sin comprimir</th>
        <th>TV analógica</th>
    </tr>
    <tr>
        <th>Cualquiera</th>
        <th colspan="2">HDMI, DVI, DisplayPort, MHL</th>
        <th>Componente, compuesto</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="6">N/A\*</td>
        <td colspan="2">Pasa contenido.</td>
        <td>Pasa contenido.</td>
    </tr>
    <tr>
        <th>150</th>
        <td colspan="2" rowspan="2">N/A\*</td>
        <td>Pasa contenido cuando CopyNever de CGMS-A está ocupado o si no se puede usar CGMS A.</td>
    </tr>
    <tr>
        <th>200</th>
        <td>Pasa contenido cuando se usa CopyNever de CGMS-A.</td>
    </tr>
    <tr>
        <th>250</th>
        <td colspan="2">Intenta usar HDCP, pero pasa contenido independientemente del resultado.</td>
        <td rowspan="5">N/A\*</td>
    </tr>
    <tr>
        <th>270</th>
        <td><b>SWDRM</b>: intenta usar HDCP. Si no se puede usar HDCP, el equipo restringirá la resolución eficaz a 520 000 píxeles por fotograma y pasará el contenido.</td>
        <td><b>HWDRM</b>: pasa contenido con HDCP. Si no se puede usar HDCP, se bloquea la reproducción en los puertos HDMI/DVI.</td>
    </tr>
    <tr>
        <th>300</th>
        <td colspan="2">
            <p>
                **Cuando la restricción de tipo HDCP NO está definida:** pasa contenido con HDCP. Si no se puede usar HDCP, se bloquea la reproducción en los puertos HDMI/DVI.
            </p>
            <p>
                **Cuando la restricción de tipo de HDCP ESTÁ definida**: pasa contenido con HDCP 2.2 y el tipo de secuencia de contenido establecido en 1. Si no se puede usar HDCP o el tipo de secuencia de contenido no se puede establecer en 1, se bloquea la reproducción a los puertos HDMI/DVI.
            </p>
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2">Windows10 nunca pasa contenido de vídeo digital comprimido a salidas, independientemente del valor del OPL posterior. Para obtener más información sobre el contenido de vídeo digital comprimido, consulta <a href="https://www.microsoft.com/playready/licensing/compliance/">Compliance Rules for PlayReady Products</a> (Reglas de cumplimiento para productos PlayReady).</td>
        <td colspan="2" rowspan="2">N/A\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* Un servidor de licencias no puede establecer todos los valores de los niveles de protección de salida. Para obtener más información, consulta [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) (Reglas de cumplimiento de PlayReady).

#### <a name="audio"></a>Audio

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Audio digital comprimido</th>
        <th>Audio digital sin comprimir</th>
        <th>Audio analógico o USB</th>
    </tr>
    <tr>
        <th>HDMI, DisplayPort, MHL</th>
        <th>HDMI, DisplayPort, MHL</th>
        <th>Cualquiera</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="3">Pasa contenido.</td>
        <td>Pasa contenido.</td>
        <td rowspan="5">Pasa contenido.</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="4">No pasa contenido.</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td>Pasa contenido cuando se usa HDCP en HDMI, DisplayPort o MHL, o si se usa SCMS y se establece en CopyNever.</td>
    </tr>
    <tr>
        <th>300</th>
        <td>Pasa contenido cuando se usa HDCP en HDMI, DisplayPort o MHL.</td>
    </tr>
</table>
<br/>

### <a name="miracast"></a>Miracast

La DRM de PlayReady permite reproducir contenido a través de la salida Miracast en cuanto se usa HDCP 2.0 o posterior. Sin embargo, en Windows10, Miracast se considera una salida *digital*. Para obtener más información acerca de los escenarios de Miracast, consulta [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) (Reglas de cumplimiento de PlayReady). La siguiente tabla delinea las asignaciones entre varios OPL en la licencia de PlayReady y cómo la DRM de PlayReady las aplica en salidas de Miracast.

<table>
    <tr>
        <th>OPL</th>
        <th>Audio digital comprimido</th>
        <th>Audio digital sin comprimir</th>
        <th>Vídeo digital comprimido</th>
        <th>Vídeo digital sin comprimir</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="4">Pasa contenido cuando se usa HDCP 2.0 o posterior. Si no se puede usar, no pasa contenido</td>
        <td>Pasa contenido cuando se usa HDCP 2.0 o posterior. Si no se puede usar, no pasa contenido</td>
        <td rowspan="6">N/A\*</td>
        <td>Pasa contenido cuando se usa HDCP 2.0 o posterior. Si no se puede usar, no pasa contenido</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="3">No pasa contenido.</td>
        <td rowspan="2">N/A\*</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td rowspan="2">Pasa contenido cuando se usa HDCP 2.0 o posterior. Si no se puede usar, no pasa contenido</td>
    </tr>
    <tr>
        <th>270</th>
        <td colspan="2">N/A\*</td>
    </tr>
    <tr>
        <th>300</th>
        <td>Pasa contenido cuando se usa HDCP 2.0 o posterior. Si no se puede usar, no pasa contenido</td>
        <td>No pasa contenido.</td>
        <td>
            <p>
                **Cuando la restricción de tipo HDCP NO está definida:** pasa contenido cuando se usa HDCP 2.0 o posterior. Si no se puede usar, NO pasa contenido.
            </p>
            <p>
                **Cuando la restricción de tipo de HDCP ESTÁ definida:** pasa contenido con HDCP 2.2 y el tipo de secuencia de contenido establecido en 1. Si no se puede usar HDCP o el tipo de secuencia de contenido no se puede establecer en 1, no pasa contenido.
            </p>        
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2" colspan="2">N/A\*</td>
        <td rowspan="2">Windows10 nunca pasa contenido de vídeo digital comprimido a salidas, independientemente del valor del OPL posterior. Para obtener más información sobre el contenido de vídeo digital comprimido, consulta <a href="https://www.microsoft.com/playready/licensing/compliance/">Compliance Rules for PlayReady Products</a> (Reglas de cumplimiento para productos PlayReady).</td>
        <td rowspan="2">N/A\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* Un servidor de licencias no puede establecer todos los valores de los niveles de protección de salida. Para obtener más información, consulta [PlayReady Compliance Rules](https://www.microsoft.com/playready/licensing/compliance/) (Reglas de cumplimiento de PlayReady).

### <a name="additional-explicit-output-restrictions"></a>Restricciones de salida explícitas adicionales

La siguiente tabla describe la implementación de la DRM de PlayReady para Windows10 de las restricciones explícitas de protección de salida de vídeo digital.

<table>
    <tr>
        <th>Situación</th>
        <th>GUID</th>
        <th>Si...</th>
        <th>En ese caso...</th>
    </tr>
    <tr>
        <th>Tamaño máximo de descodificación de resolución efectiva</th>
        <td>9645E831-E01D-4FFF-8342-0A720E3E028F</td>
        <td>La salida conectada es: Salida de vídeo digital, Miracast, HDMI, DVI, etc.</td>
        <td>
            <p>
                Pasa contenido cuando está restringido a:  
            </p>
            <ul>
                <li>(a) El ancho del fotograma debe ser menor o igual que el ancho máximo del fotograma en píxeles, y el alto del fotograma menor o igual que el alto máximo del fotograma en píxeles, o</li>
                <li>(b) El alto del fotograma debe ser menor o igual que el ancho máximo del fotograma en píxeles, y el ancho del fotograma menor o igual que el alto máximo del fotograma en píxeles.</li>
            </ul>                   
        </td>
    </tr>
    <tr>
        <th>Restricción del tipo de HDCP</th>
        <td>ABB2C6F1-E663-4625-A945-972D17B231E7</td>
        <td>La salida conectada es: Salida de vídeo digital, Miracast, HDMI, DVI, etc.</td>
        <td>Pasa contenido con HDCP 2.2 y el tipo de secuencia de contenido establecido en 1. Si no se puede usar HDCP 2.2 o el tipo de secuencia de contenido no se puede establecer en 1, no pasa contenido. También debe especificarse el nivel de protección de salida del vídeo digital sin comprimir de un valor mayor o igual a 271</td>
    </tr>
</table>
<br/>

La siguiente tabla describe la implementación de la DRM de PlayReady para Windows10 de las restricciones explícitas de protección de salida de vídeo analógico.

<table>
    <tr>
        <th>Situación</th>
        <th>GUID</th>
        <th>Si...</th>
        <th colspan="2">En ese caso...</th>
    </tr>
    <tr>
        <th>Monitor de PC analógico</th>
        <td>D783A191-E083-4BAF-B2DA-E69F910B3772</td>
        <td>La salida conectada es: VGA, DVI&ndash;analógica, etc..</td>
        <td><b>SWDRM:</b> el equipo restringirá la resolución eficaz a 520 000epx por fotograma y pasará contenido.</td>
        <td><b>HWDRM:</b> NO pasa contenido.</td>
    </tr>
    <tr>
        <th>Componente analógico</th>
        <td>811C5110-46C8-4C6E-8163-C0482A15D47E</td>
        <td>La salida conectada es: Componente</td>
        <td><b>SWDRM:</b> el equipo restringirá la resolución eficaz a 520 000epx por fotograma y pasará contenido.</td>
        <td><b>HWDRM:</b> NO pasa contenido.</td>
    </tr>
    <tr>
        <th rowspan="2">Salidas de TV analógica</th>
        <td>2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td>El OPL de la TV analógica es menor que 151.</td>
        <td colspan="2">Debe usarse CGMS-A.</td>
    </tr>
    <tr>
        <td>225CD36F-F132-49EF-BA8C-C91EA28E4369</td>
        <td>El OPL de la TV analógica es menor que 101 y la licencia no contiene 2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3.</td>
        <td colspan="2">Debe intentarse el uso de CGMS-A, pero el contenido se puede reproducir independientemente del resultado.</td>
    </tr>
    <tr>
        <th>Control de la ganancia y franjas de color automáticos</th>
        <td>C3FD11C6-F8B7-4D20-B008-1DB17D61F2DA</td>
        <td>Pasar contenido con una resolución menor o igual a 520 000px a una salida de TV analógica</td>
        <td colspan="2">Establece AGC solo para el vídeo de componente y el modo PAL cuando la resolución es inferior a 520 000px y establece la información de AGC y franjas de color para NTSC cuando la resolución es inferior a 520 000px, de acuerdo con lo indicado en la tabla 3.5.7.3. en las reglas de cumplimiento</td>
    </tr>
    <tr>
        <th>Solo salida digital</th>
        <td>760AE755-682A-41E0-B1B3-DCDF836A7306</td>
        <td>La salida conectada es analógica</td>
        <td colspan="2">No pasa contenido.</td>
    </tr>
</table>
<br/>

> [!NOTE]
> Si se usa una llave de adaptador, como "Mini DisplayPort a VGA" para la reproducción, Windows 10 ve el resultado como salida de vídeo digital y no puede aplicar las directivas de vídeo analógico.

La siguiente tabla describe la implementación de la DRM de PlayReady para Windows10 que permite reproducir en otras circunstancias.

<table>
    <tr>
        <th>Situación</th>
        <th>GUID</th>
        <th>Si...</th>
        <th colspan="2">En ese caso...</th>
    </tr>
    <tr>
        <th>Salida desconocida</th>
        <td>786627D8-C2A6-44BE-8F88-08AE255B01A7</td>
        <td>Si no se puede determinar la salida de un modo razonable o no se puede establecer OPM con el controlador de gráficos.</td>
        <td><b>SWDRM:</b> pasa contenido.</td>
        <td><b>HWDRM:</b> NO pasa contenido.</td>
    </tr>
    <tr>
        <th>Salida desconocida con restricción.</th>
        <td>B621D91F-EDCC-4035-8D4B-DC71760D43E9</td>
        <td>Si no se puede determinar la salida de un modo razonable o no se puede establecer OPM con el controlador de gráficos.</td>
        <td><b>SWDRM:</b> el equipo restringirá la resolución eficaz a 520 000epx por fotograma y pasará contenido.</td>
        <td><b>HWDRM:</b> NO pasa contenido.</td>
    </tr>
</table>
<br/>

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar a crear la aplicación para UWP protegida con PlayReady, es necesario instalar el siguiente software en el sistema:

-   Windows 10.
-   Si Pretendes compilar cualquiera de las muestras para DRM de PlayReady para aplicaciones para UWP, debes usar Microsoft Visual Studio2015 o posterior para compilar los ejemplos. Puedes seguir usando Microsoft Visual Studio2013 para compilar cualquiera de las muestras de DRM de PlayReady para aplicaciones de la tienda de Windows8.1.

<!--This is no longer available-->
<!--If you are planning to play back MPEG-2/H.262 content on your app, you must also download and install [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876).-->

## <a name="playready-uwp-app-migration-guide"></a>Guía de migración de la aplicación para UWP de PlayReady

Esta sección incluye información sobre cómo migrar las aplicaciones de la tienda de PlayReady Windows 8.x existentes a Windows 10.

El espacio de nombres PlayReady para aplicaciones para UWP en Windows 10 se cambió de **Microsoft.Media.PlayReadyClient** a [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454). Esto significa que tendrás que buscar y reemplazar el espacio de nombres antiguo por uno nuevo en el código. Seguirás haciendo referencia a un archivo winmd. Es parte de windows.media.winmd en el sistema operativo de Windows 10. Está en el windows.winmd como parte del Windows SDK de TH. Para UWP, se hace referencia en windows.foundation.univeralappcontract.winmd.

Para reproducir contenido protegido por PlayReady de alta definición (HD) (1080p) y contenido de altísima definición (UHD), tendrás que implementar DRM de hardware PlayReady. Para obtener información sobre cómo implementar DRM de hardware PlayReady, consulta [DRM de hardware](hardware-drm.md).

Algunos contenidos no son compatibles con DRM de hardware. Para obtener información acerca de cómo deshabilitar DRM de hardware y habilitar DRM de software DRM, consulta [Invalidar DRM de hardware](hardware-drm.md#override-hardware-drm).

En lo relacionado con el administrador de protección multimedia, asegúrate de que el código tenga la siguiente configuración si es que no la tiene ya:

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## <a name="proactively-acquire-a-non-persistent-license-before-playback"></a>Adquirir de forma proactiva una licencia no persistente antes de la reproducción

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
    
## <a name="query-for-protection-capabilities"></a>Consultar las capacidades de protección
A partir de Windows 10, versión 1703, puedes consultar las funcionalidades de DRM de hardware, como descodificar códecs, la resolución y las protecciones de salida (HDCP). Las consultas se realizan con el método [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported) que toma una cadena que representa las funcionalidades para las que se consulta al soporte técnico y una cadena que especifica el sistema de claves al que se aplica la consulta. Para una lista de los valores de cadena compatibles, consulta la página de referencia de la API para [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported). En el siguiente ejemplo de código se muestra el uso de este método.  

    ```cs
    using namespace Windows::Media::Protection;

    ProtectionCapabilities^ sr = ref new ProtectionCapabilities();

    ProtectionCapabilityResult result = sr->IsTypeSupported(
    L"video/mp4; codecs=\"avc1.640028\"; features=\"decode-bpp=10,decode-fps=29.97,decode-res-x=1920,decode-res-y=1080\"",
    L"com.microsoft.playready");

    switch (result)
    {
        case ProtectionCapabilityResult::Probably:
        // Queue up UHD HW DRM video
        break;

        case ProtectionCapabilityResult::Maybe:
        // Check again after UI or poll for more info.
        break;

        case ProtectionCapabilityResult::NotSupported:
        // Do not queue up UHD HW DRM video.
        break;
    }
    ```
## <a name="add-secure-stop"></a>Agregar una detención segura

Esta sección describe cómo agregar una detención segura a una aplicación para UWP.

La detención segura proporciona los medios para que un dispositivo PlayReady se imponga con confianza a un servicio de transmisión por secuencias de multimedia que la reproducción multimedia ha detenido para un determinado contenido. Esta funcionalidad garantiza que los servicios de transmisión por secuencias de multimedia proporcionen una aplicación precisa e informes de limitaciones de uso en diferentes dispositivos de una cuenta determinada.

Hay dos escenarios principales para el envío de un desafío de detención segura:

-   Cuando se detiene la presentación multimedia porque se ha alcanzado el final del contenido o cuando el usuario detiene la presentación multimedia en algún lugar en mitad del proceso.
-   Cuando la sesión anterior termina inesperadamente (por ejemplo, debido a un bloqueo del sistema o la aplicación). La aplicación tendrá que consultar, ya sea al inicio o apagado, las sesiones de detención segura pendientes y enviar los desafíos independientemente de otras reproducciones de multimedia.

Para una implementación de muestra de detención segura, consulta el archivo securestop.cs en la muestra de PlayReady ubicada en [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670).

## <a name="use-playready-drm-on-xbox-one"></a>Usar DRM de PlayReady en Xbox One

Para usar DRM de PlayReady en una aplicación para UWP en Xbox One, primero debes registrar tu cuenta del [Centro de partners](https://partner.microsoft.com/dashboard) que estés usando para publicar la aplicación para la autorización de uso de PlayReady. Puedes hacerlo de dos maneras distintas:

* Hacer que tu contacto de Microsoft solicite permiso.
* Solicitar autorización mediante el envío de tu nombre de empresa y cuenta de centro de partners para [pronxbox@microsoft.com](mailto:pronxbox@microsoft.com).

Una vez que recibas la autorización, tendrás que agregar un elemento `<DeviceCapability>` adicional en el manifiesto de la aplicación. Tendrás que hacerlo de forma manual porque actualmente no hay ninguna opción disponible en el Diseñador de manifiestos de aplicaciones. Sigue estos pasos para configurarlo:

1. Con el proyecto abierto en Visual Studio, abre el **Explorador de soluciones** y haz clic en **Package.appxmanifest**.
2. Selecciona **Abrir con...**, elige **Editor XML (texto)** y haz clic en **Aceptar**.
3. Entre las etiquetas `<Capabilities>`, agrega la siguiente `<DeviceCapability>`:

    ```xml
    <DeviceCapability Name="6a7e5907-885c-4bcb-b40a-073c067bd3d5" />
    ```

4. Guarda el archivo.

Por último, hay una última consideración al usar PlayReady en Xbox One: en los kits de desarrollo, existe un límite de SL150 (es decir, no pueden reproducir contenido de SL2000 o SL3000). Los dispositivos comerciales pueden reproducir contenido con mayor niveles de seguridad, pero para probar la aplicación en un kit de desarrollo, tendrás que usar contenido de SL150. Puedes probar este contenido de las siguientes formas:

* Usa el contenido de prueba protegido que requiere licencias de SL150.
* Implementa la lógica para que solo determinadas cuentas de prueba autenticadas puedan adquirir licencias de SL150 para cierto contenido.

Usa el enfoque que tiene más sentido para tu empresa y el producto.


## <a name="see-also"></a>Consulta también
- [Reproducción de contenido multimedia](media-playback.md)




