---
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: En este tema se ofrece una descripción general sobre cómo agregar la administración de derechos digitales (DRM) basada en hardware de PlayReady a una aplicación para la Plataforma universal de Windows (UWP).
title: DRM de hardware
---

# DRM de hardware

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este tema se ofrece una descripción general sobre cómo agregar la administración de derechos digitales (DRM) basada en hardware de PlayReady a una aplicación para la Plataforma universal de Windows (UWP).

**Nota:** DRM basado en hardware solo es compatible con hardware determinado con una versión de firmware de Windows 10. Consulta las [reglas de solidez y cumplimiento de PlayReady](http://www.microsoft.com/playready/licensing/compliance/) para obtener más información sobre las garantías proporcionadas.

Cada vez más proveedores de contenido están avanzando hacia protecciones basadas en hardware para conceder permiso para reproducir contenido completo de alto valor en las aplicaciones. Para cubrir esta necesidad, se ha agregado a PlayReady una compatibilidad sólida con la implementación de hardware del cifrado principal. Esta compatibilidad permite la reproducción segura de alta definición (1080p) y altísima definición (UHD) de contenido en varias plataformas de dispositivos. Se protege el material de clave (incluyendo claves privadas, claves de contenido y cualquier otro material de clave que se use para derivar o desbloquear dichas claves), así como muestras de vídeo descifradas comprimidas y descomprimidas mediante el aprovechamiento de la seguridad de hardware.

## Implementación de Windows TEE

En este tema se ofrece una breve descripción general sobre la forma en que Windows 10 implementa el entorno de ejecución de confianza.

Los detalles sobre la implementación de Windows TEE está fuera del ámbito de este documento. Sin embargo, es de utilidad hacer una breve descripción de la diferencia entre el puerto del kit de migración estándar y el puerto de Windows. Windows implementa la capa de proxy de OEM y transfiere las llamadas a funciones serializadas PRITEE a un controlador de modo usuario en el subsistema de Windows Media Foundation. Este finalmente se enrutará bien al controlador de Windows TrEE (entorno de ejecución de confianza) o al controlador de elementos gráficos del OEM. Los detalles de cualquiera de estos enfoques están fuera del ámbito de este documento. El siguiente diagrama muestra la interacción del componente general para el puerto de Windows. Si quieres desarrollar una implementación de Windows PlayReady TEE, puedes ponerte en contacto con <WMLA@Microsoft.com>.

![diagrama de componentes de windows tee](images/windowsteecomponentdiagram720.jpg)

## Consideraciones para el uso de DRM de hardware.

En este tema se ofrece una lista breve de cosas que deben tenerse en cuenta para desarrollar aplicaciones diseñadas para usar DRM de hardware.

-   Proceso multimedia protegido (PMP) no es compatible.
-   Compatible con el nivel de protección de salida (OPL) 270 (no de menor resolución). Microsoft recomienda que el contenido de alta definición para DRM de hardware tenga un OPL mayor de 270 (aunque no es necesario). Un OPL mayor que 270 implica que sea necesario HDCP. Además, Microsoft recomienda establecer HDCP tipo 1 (versión 2.2 o posterior).
-   A diferencia de DRM de software, se aplican protecciones de salida a todos los monitores basadas en el monitor menos capacitado. Por ejemplo, si el usuario tiene dos monitores conectados y solo uno de ellos es compatible con HDCP, se producirá un error en la reproducción si la licencia requiere HDCP incluso si el contenido solamente se está representando en el monitor compatible con HDCP. En DRM de software, el contenido se podría reproducir siempre que solo se esté representando en el monitor compatible con HDCP.
-   DRM de hardware no está garantizado para su uso por parte del cliente ni en relación con su seguridad a menos que las claves de contenido y las licencias cumplan las siguientes condiciones:
    -   El audio debe estar desactivado o cifrado para una clave de contenido distinta que el vídeo. Microsoft recomienda que el audio esté desactivado para mejorar el rendimiento de la reproducción.
    -   La licencia que se usa para la clave de contenido de vídeo debe tener un nivel de seguridad de 3000.
-   No se admiten varias unidades de procesamiento gráfico (GPU) de licencias persistentes.

Para administrar las licencias persistentes en equipos con varias GPU, observa el siguiente escenario:

1.  Un cliente compra un nuevo equipo con una tarjeta gráfica integrada.
2.  El cliente usa una aplicación que adquiere licencias persistentes al usar DRM de hardware.
3.  La licencia permanente ahora está enlazada a las claves de hardware de esa tarjeta gráfica.
4.  Después, el cliente instala una nueva tarjeta gráfica.
5.  Todas las licencias en el almacén de datos con hash (HDS) se enlazan a la tarjeta de vídeo integrada, pero el cliente ahora quiere reproducir contenido protegido con la tarjeta gráfica recién instalada.

Para evitar que se produzca un error de reproducción porque el hardware no puede descifrar las licencias, PlayReady usa un almacén de datos con hash (HDS) independiente para cada tarjeta gráfica que encuentre. Esto provocará que PlayReady intente adquirir una licencia para una parte del contenido donde normalmente ya tendría una licencia (es decir, en el caso de DRM de software o en cualquier caso sin un cambio de hardware, PlayReady no necesitaría adquirir una licencia). Por lo tanto, si la aplicación adquiere una licencia permanente mientras usa DRM de hardware, la aplicación debe poder controlar el caso en que esa licencia se "pierda" si el usuario final instala (o desinstala) una tarjeta gráfica. Dado que no es un escenario común, en lugar de averiguar cómo tratar con un cambio de hardware en el código de cliente o servidor, puedes decidir controlar las llamadas de soporte técnico cuando ya no se reproduzca el contenido después de un cambio de hardware.

## Invalidar DRM de hardware

En esta sección se describe cómo invalidar DRM de hardware si el contenido reproducido no admite DRM de hardware.

De manera predeterminada, se usa DRM de hardware si el sistema lo admite. Sin embargo, algunos contenidos no son compatibles con DRM de hardware. Un ejemplo de esto es el contenido de Cocktail. Otro ejemplo es cualquier contenido que use un códec de vídeo que no sea H.264 y HEVC. Otro ejemplo es el contenido HEVC, ya que cierto DRM de hardware admitirá HEVC y otro no lo hará. Por lo tanto, si quieres reproducir algún contenido y DRM de hardware no lo admite en el sistema en cuestión, puedes desactivar DRM de hardware.

El siguiente ejemplo muestra cómo desactivar DRM de hardware. Solo necesitas hacer esto antes de cambiar. Además, asegúrate de no disponer de ningún objeto PlayReady en la memoria; de lo contrario, el comportamiento será indefinido.

``` syntax
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer(“PlayReady”, Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values[“SoftwareOverride”] = 1;
```

Para volver a DRM de hardware, establece el valor **SoftwareOverride** en **0**.

Para cada reproducción multimedia, debes establecer **MediaProtectionManager** en:

``` syntax
mediaProtectionManager.properties[“Windows.Media.Protection.UseSoftwareProtectionLayer”] = true;
```

La mejor manera de saber si estás en DRM de hardware o de software es echar un vistazo a C:\\Usuarios\\&lt;nombreusuario&gt;\\AppData\\Local\\Packages\\&lt;nombre de la aplicación&gt;\\LocalState\\PlayReady\\\*

-   Si hay un archivo mspr.hds, significa que te encuentras en DRM de software.
-   Si tienes otro archivo \*.hds, significa que te encuentras en DRM de hardware.
-   También puedes eliminar toda la carpeta PlayReady y volver a intentar la prueba.

## Detectar el tipo de DRM de hardware

En esta sección se describe cómo detectar qué tipo de DRM de hardware se admite en el sistema.

Puedes usar el método [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) para determinar si el sistema admite una característica específica de administración de derechos digitales (DRM) de hardware. Por ejemplo:

``` syntax
boolean PlayReadyStatics->CheckSupportedHardware(PlayReadyHardwareDRMFeatures enum);
```

La enumeración [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) contiene la lista válida de los valores de características de DRM de hardware que se pueden consultar. Para determinar si se admite DRM de hardware, usa el miembro **HardwareDRM** en la consulta. Para determinar si el hardware admite el códec de codificación de vídeo de alta eficiencia (HEVC)/H.265, usa el miembro **HEVC** en la consulta.

También puedes usar la propiedad [**PlayReadyStatics.PlayReadyCertificateSecurityLevel**](https://msdn.microsoft.com/library/windows/apps/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel.aspx) para conseguir el nivel de seguridad del certificado de cliente para determinar si se admite DRM de hardware. A menos que el nivel de seguridad del certificado devuelto sea mayor o igual a 3000, o bien el cliente no está individualizado o aprovisionado (en cuyo caso esta propiedad devuelve 0) o DRM de hardware no está en uso (en cuyo caso esta propiedad devuelve un valor que es menor de 3.000).



<!--HONumber=Mar16_HO1-->


