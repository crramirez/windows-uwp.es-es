---
author: drewbatgit
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: En este tema se ofrece una descripción general sobre cómo agregar la administración de derechos digitales (DRM) basada en hardware de PlayReady a una aplicación para la Plataforma universal de Windows (UWP).
title: DRM de hardware
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 160a4ab0ff5bdc40ea46ff6d8fb9fd8e47f560e3
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "3934196"
---
# <a name="hardware-drm"></a>DRM de hardware


En este tema se ofrece una introducción sobre cómo agregar la administración de derechos digitales (DRM) basada en hardware de PlayReady a una aplicación para la Plataforma universal de Windows (UWP).

> [!NOTE] 
> La DRM basada en hardware PlayReady se admite en una gran variedad de dispositivos, incluidos dispositivos Windows y que no son Windows, como televisores, teléfonos y tabletas. Para que un dispositivo Windows admita la DRM de hardware PlayReady, debe ejecutar Windows10 y tener una configuración de hardware compatible.

Cada vez más proveedores de contenido se encaminan al uso de protecciones basadas en hardware para conceder permiso para reproducir contenido completo de alto valor en las aplicaciones. Para cubrir esta necesidad, se ha agregado a PlayReady una compatibilidad sólida con la implementación de hardware del cifrado principal. Esta compatibilidad permite la reproducción segura de alta definición (1080p) y altísima definición (UHD) de contenido en varias plataformas de dispositivos. Se protege el material de clave (incluyendo claves privadas, claves de contenido y cualquier otro material de clave que se use para derivar o desbloquear dichas claves), así como muestras de vídeo descifradas comprimidas y descomprimidas mediante el aprovechamiento de la seguridad de hardware.

## <a name="windows-tee-implementation"></a>Implementación de Windows TEE

Este tema proporciona una breve introducción sobre cómo Windows 10 implementa el entorno de ejecución de confianza (TEE).

Los detalles sobre la implementación de Windows TEE están fuera del ámbito de este documento. Sin embargo, es de utilidad hacer una breve descripción de la diferencia entre el puerto del kit de migración estándar y el puerto de Windows. Windows implementa la capa de proxy de OEM y transfiere las llamadas a funciones serializadas PRITEE a un controlador de modo usuario en el subsistema de Windows Media Foundation. Este finalmente se enrutará bien al controlador de Windows TrEE (entorno de ejecución de confianza) o al controlador de elementos gráficos del OEM. Los detalles de cualquiera de estos enfoques están fuera del ámbito de este documento. El siguiente diagrama muestra la interacción del componente general para el puerto de Windows. Si quieres desarrollar una implementación de Windows PlayReady TEE, puedes ponerte en contacto con <WMLA@Microsoft.com>.

![windows tee component diagram, diagrama de componentes de Windows TEE](images/windowsteecomponentdiagram720.jpg)

## <a name="considerations-for-using-hardware-drm"></a>Consideraciones para el uso de DRM de hardware

En este tema se ofrece una lista breve de puntos que deben tenerse en cuenta para desarrollar aplicaciones diseñadas para usar DRM de hardware. Como se explica en [DRM PlayReady](playready-client-sdk.md#output-protection), con la HWDRM PlayReady para Windows10, todas las protecciones de salida se aplican desde dentro de la implementación de Windows TEE, lo que tiene algunas consecuencias en los comportamientos de la protección de salida:

-   **Compatibilidad con el nivel de protección de salida (OPL) para vídeo digital sin comprimir 270:** HWDRM PlayReady para Windows10 no admite una resolución menor y aplicará el uso de HDCP. Recomendamos que el contenido de alta definición para la HWDRM tenga un OPL mayor de 270 (aunque no es necesario). También recomendamos que se establezca una restricción de tipo HDCP en la licencia (HDCP versión 2.2 en Windows10).
-   **A diferencia de DRM de software (SWDRM), se aplican protecciones de salida a todos los monitores basadas en el monitor menos capacitado.** Por ejemplo, si el usuario tiene dos monitores conectados y solo uno de ellos es compatible con HDCP, se producirá un error en la reproducción si la licencia requiere HDCP incluso si el contenido solamente se está representando en el monitor compatible con HDCP. En DRM de software, el contenido se podría reproducir siempre que solo se esté representando en el monitor compatible con HDCP.
-   **No se garantiza que el cliente pueda usar la HWDRM ni que esta sea segura, a menos que las claves de contenido y las licencias cumplan las siguientes condiciones**:
    -   La licencia que se use para la clave de contenido de vídeo debe tener una propiedad de nivel de seguridad mínimo de 3000.
    -   El audio debe estar cifrado con una clave de contenido distinta que el vídeo, y la licencia que se use para el audio debe tener una propiedad de nivel de seguridad mínimo de 2000. Como alternativa, el audio podría dejarse sin cifrar.
    
Además, es necesario tener en cuenta los siguientes elementos al usar la HWDRM:

-   No se admite el Proceso multimedia protegido (PMP).
-   No se admite Windows Media Video (también conocido como VC-1) (consulta [Invalidar la DRM de hardware](#override-hardware-drm)).
-   No se admiten varias unidades de procesamiento gráfico (GPU) para las licencias persistentes.

Para administrar las licencias persistentes en equipos con varias GPU, observa el siguiente escenario:

1.  Un cliente compra un nuevo equipo con una tarjeta gráfica integrada.
2.  El cliente usa una aplicación que adquiere licencias persistentes al usar DRM de hardware.
3.  La licencia permanente ahora está enlazada a las claves de hardware de esa tarjeta gráfica.
4.  Después, el cliente instala una nueva tarjeta gráfica.
5.  Todas las licencias en el almacén de datos con hash (HDS) se enlazan a la tarjeta de vídeo integrada, pero el cliente ahora quiere reproducir contenido protegido con la tarjeta gráfica recién instalada.

Para evitar que se produzca un error de reproducción porque el hardware no pueda descifrar las licencias, PlayReady usa un HDS (almacén de datos con hash) independiente para cada tarjeta gráfica que encuentra. Esto provocará que PlayReady intente adquirir una licencia para una parte del contenido donde normalmente ya tendría una licencia (es decir, en el caso de la DRM de software o en cualquier caso en el que no haya ningún cambio de hardware, PlayReady no necesitaría adquirir una licencia). Por lo tanto, si la aplicación adquiere una licencia permanente mientras usa DRM de hardware, la aplicación debe poder controlar el caso en que esa licencia se "pierda" si el usuario final instala (o desinstala) una tarjeta gráfica. Dado que no es un escenario común, en lugar de averiguar cómo tratar con un cambio de hardware en el código de cliente o servidor, puedes decidir controlar las llamadas de soporte técnico cuando ya no se reproduzca el contenido después de un cambio de hardware.

## <a name="override-hardware-drm"></a>Invalidar DRM de hardware

En esta sección se describe cómo invalidar la DRM de hardware (HWDRM) si el contenido reproducido no admite dicha DRM de hardware.

De manera predeterminada, se usa DRM de hardware si el sistema lo admite. Sin embargo, algunos contenidos no son compatibles con DRM de hardware. Un ejemplo de esto es el contenido de Cocktail. Otro ejemplo es cualquier contenido que use un códec de vídeo que no sea H.264 y HEVC. Otro ejemplo es el contenido HEVC, ya que cierto DRM de hardware admitirá HEVC y otro no lo hará. Por lo tanto, si quieres reproducir algún contenido y DRM de hardware no lo admite en el sistema en cuestión, puedes desactivar DRM de hardware.

El siguiente ejemplo muestra cómo desactivar DRM de hardware. Solo necesitas hacer esto antes de cambiar. Además, asegúrate de no disponer de ningún objeto PlayReady en la memoria; de lo contrario, el comportamiento será indefinido.

```js
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer("PlayReady", Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values["SoftwareOverride"] = 1;
```

Para volver a DRM de hardware, establece el valor **SoftwareOverride** en **0**.

Para cada reproducción multimedia, debes establecer **MediaProtectionManager** en:

```js
mediaProtectionManager.properties["Windows.Media.Protection.UseSoftwareProtectionLayer"] = true;
```

La mejor manera de saber si te encuentras en DRM de hardware o de DRM de software es echar un vistazo a C:\\Users\\&lt;nombre de usuario&gt;\\AppData\\Local\\Packages\\&lt;nombre de la aplicación&gt;\\LocalCache\\PlayReady\\\*

-   Si hay un archivo mspr.hds, significa que te encuentras en DRM de software.
-   Si tienes otro archivo \*.hds, significa que te encuentras en DRM de hardware.
-   También puedes eliminar toda la carpeta PlayReady y volver a intentar la prueba.

## <a name="detect-the-type-of-hardware-drm"></a>Detectar el tipo de DRM de hardware

En esta sección se describe cómo detectar qué tipo de DRM de hardware se admite en el sistema.

Puedes usar el método [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) para determinar si el sistema admite una característica específica de DRM de hardware. Por ejemplo:

```csharp
bool isFeatureSupported = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.HEVC);
```

La enumeración [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) contiene la lista válida de los valores de características de DRM de hardware que se pueden consultar. Para determinar si se admite DRM de hardware, usa el miembro **HardwareDRM** en la consulta. Para determinar si el hardware admite el códec de codificación de vídeo de alta eficiencia (HEVC)/H.265, usa el miembro **HEVC** en la consulta.

También puedes usar la propiedad [**PlayReadyStatics.PlayReadyCertificateSecurityLevel**](https://msdn.microsoft.com/library/windows/apps/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel.aspx) para conseguir el nivel de seguridad del certificado de cliente para determinar si se admite DRM de hardware. A menos que el nivel de seguridad del certificado devuelto sea mayor o igual a 3000, o bien el cliente no está individualizado o aprovisionado (en cuyo caso esta propiedad devuelve 0) o DRM de hardware no está en uso (en cuyo caso esta propiedad devuelve un valor que es menor de 3.000).

### <a name="detecting-support-for-aes128cbc-hardware-drm"></a>Detección de la compatibilidad para DRM de hardware AES128CBC
A partir de Windows 10, versión 1709, para detectar la compatibilidad para el cifrado de hardware AES128CBC en un dispositivo, llama a **[PlayReadyStatics.CheckSupportedHardware](https://msdn.microsoft.com/library/windows/apps/dn986441)** y especifica el valor de enumeración [**PlayReadyHardwareDRMFeatures.Aes128Cbc**](https://msdn.microsoft.com/library/windows/apps/dn986265). En versiones anteriores de Windows 10, la especificación de este valor hará que se genere una excepción. Por este motivo, debes comprobar la presencia del valor de enumeración llamando a **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** y especificando la versión 5 de contrato principal antes de llamar a **CheckSupportedHardware**.

```csharp
bool supportsAes128Cbc = ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5);

if (supportsAes128Cbc)
{
    supportsAes128Cbc = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.Aes128Cbc);
}
```

## <a name="see-also"></a>Ver también
- [DRM de PlayReady](playready-client-sdk.md)
