---
Description: La Microsoft Store aplica ciertas reglas relacionadas con los números de versión, que funcionan de forma ligeramente diferente en diferentes versiones del sistema operativo.
title: Numeración de la versión del paquete
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cfab93064cf2fa2edd91798167a2c2fbc368ecb1
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468899"
---
# <a name="package-version-numbering"></a>Numeración de la versión del paquete

Cada paquete que proporciones debe tener un número de versión (proporcionado como un valor en el atributo **Version** del elemento [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) en el manifiesto de la aplicación). La Microsoft Store aplica ciertas reglas relacionadas con los números de versión, que funcionan de forma ligeramente diferente en diferentes versiones del sistema operativo.

> [!NOTE]
> En este tema hace referencia a "paquetes", pero a menos que se indique, las mismas reglas se aplican a los números de versión de los archivos.msix/.appx y.msixbundle/.appxbundle.


## <a name="version-numbering-for-windows10-packages"></a>Números de versión para los paquetes de Windows 10

> [!IMPORTANT]
> Para los paquetes de Windows 10 (UWP), la última sección (cuarta) del número de versión está reservada para uso Store y debe dejarse como 0 cuando se compila el paquete (aunque el Store puede cambiar el valor de esta sección). Las demás secciones deben establecerse en un entero entre 0 y 65535 (excepto la primera sección, donde no puede ser 0).

Al elegir un paquete UWP desde su presentación publicada, la Microsoft Store siempre usará el paquete con mayor control de versiones que se aplica a dispositivos de Windows 10 del cliente. Esto te da más flexibilidad y te permite controlar qué paquetes se proporcionan a los clientes en tipos específicos de dispositivo. Es importante señalar que puedes enviar los paquetes en cualquier orden; no es necesario que los paquetes de un envío posterior tengan un número de versión más alto.

Puede proporcionar varios paquetes UWP con el mismo número de versión. Sin embargo, los paquetes con el mismo número de versión no pueden tener también la misma arquitectura, ya que la identidad completa que la Tienda usa para cada paquete debe ser exclusiva. Para obtener más información, consulta [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Al proporcionar varios paquetes UWP que usan el mismo número de versión, la arquitectura (en el orden x64, x 86, ARM, neutral) se usará para decidir cuál es la de rango superior (cuando el Store determina qué paquete va a proporcionar a los dispositivos del cliente). Al clasificar lotes de aplicaciones que usan el mismo número de versión, se detiene en cuenta la arquitectura de mayor rango. Un lote de aplicaciones con un paquete x64 tiene un rango superior a otro que solo contenga un paquete x86.

Esto ofrece mucha flexibilidad al evolucionar la aplicación a lo largo del tiempo. Puede cargar y enviar nuevos paquetes que utilizan números de versión inferior para agregar compatibilidad para dispositivos Windows 10 que no admitía anteriormente, puede agregar paquetes con mayor control de versiones que tienen dependencias más estrictas para aprovechar las ventajas de hardware o características del sistema operativo o bien Puede agregar paquetes con mayor control de versiones que sirven las actualizaciones de algunos o todos los clientes existentes de bases.

El siguiente ejemplo muestra cómo se puede usar la numeración de versión a lo largo de varios envíos para ofrecer los paquetes previstos a los clientes.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Por ejemplo: Mover a un único paquete a través de varios envíos

Windows 10 le permite escribir un código base único que se ejecuta en todas partes. Esto hace mucho más fácil iniciar un nuevo proyecto en varias plataformas. Sin embargo, por diversas razones, es posible que no quieras combinar código base ya existente para crear un único proyecto inmediatamente.

Puede usar las reglas de control de versiones de paquete para mover gradualmente a los clientes a un único paquete para la familia de dispositivos Universal, al enviar un número de actualizaciones provisionales para familias de dispositivos específicos (incluidas las que se benefician de las API de Windows 10). El ejemplo siguiente muestra cómo se aplican las mismas reglas de manera coherente a través de una serie de presentaciones para la misma aplicación.

| Envío | Contenido                                                  | Experiencia del cliente                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -Versión package: 1.1.10.0 <br> -Familia de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -Versión package: 1.1.0.0 <br> -Familia de dispositivos: Windows.Mobile, minVersion 10.0.10240.0     | -   Los dispositivos de Windows 10 Desktop con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.10.0 <br> -   Los dispositivos de Windows 10 Mobile con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.0.0 <br> -   Las demás familias de dispositivos no podrán comprar e instalar la aplicación |
| 2          | -Versión package: 1.1.10.0 <br> -Familia de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -Versión package: 1.1.0.0 <br> -Familia de dispositivos: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -Versión package: 1.0.0.0 <br> -Familia de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Los dispositivos de Windows 10 Desktop con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.10.0 <br> -   Los dispositivos de Windows 10 Mobile con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.0.0 <br> -   Cuando otras familias de dispositivos (distintas de los tipos de escritorio y móvil) estén disponibles, obtendrán la versión 1.0.0.0 <br> -   Los dispositivos móviles y de escritorio que ya tengan instalada la aplicación no tendrán ninguna actualización (porque ya tienen la mejor versión disponible: 1.1.10.0 y 1.1.0.0 son mayores que 1.0.0.0) |
| 3          | -Versión package: 1.1.10.0 <br> -Familia de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -Versión package: 1.1.5.0 <br> -Familia de dispositivos: Windows.Universal, minVersion 10.0.10250.0 <br> <br> -Versión package: 1.0.0.0 <br> -Familia de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Los dispositivos de Windows 10 Desktop con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.10.0 <br> -   Los dispositivos de Windows 10 Mobile con la compilación 10.0.10250.0 o superior obtienen la versión 1.1.5.0 <br> -   Los dispositivos de Windows 10 Mobile con número de compilación >=10.0.10240.0 y <10.010250.0 obtienen la versión 1.1.0.0 
| 4          | -Versión package: 2.0.0.0 <br> -Familia de dispositivos: Windows.Universal, minVersion 10.0.10240.0   | -   Todos los clientes de todas las familias de dispositivos de Windows 10 con la compilación v10.0.10240.0 o superior obtienen el paquete 2.0.0.0 | 

> [!NOTE]
>  En todos los casos, los dispositivos de cliente recibirán el paquete que tiene el número de versión posible más alto que sean aptos. Por ejemplo, en el tercer envío antes descrito, todos los dispositivos de escritorio obtendrán la versión v1.1.10.0, aunque tengan la versión 10.0.10250.0 o superior del sistema operativo y, por lo tanto, también pudieran aceptar la versión v1.1.5.0. Dado que 1.1.10.0 es el número de versión más alto disponible para ellos, ese es el paquete que obtienen.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Usar el número de versión para revertir a un paquete distribuido anteriormente para las nuevas adquisiciones

Si decide mantener copias de los paquetes, tendrá la opción de revertir el paquete de la aplicación en el Store a un paquete de Windows 10 anterior si debe detectar problemas con una versión. Se trata de forma temporal para limitar las interrupciones para los clientes mientras toma tiempo para corregir el problema.

Para ello, cree un nuevo [envío](app-submissions.md). Quita el paquete problemático y carga el paquete anterior que quieras proporcionar en la Tienda. Los clientes que ya hayan recibido el paquete al que estás revirtiendo seguirán teniendo el paquete problemático (ya que el antiguo tiene un número de versión anterior). Sin embargo, así se evita que nadie pueda comprar el paquete problemático y mantiene la aplicación disponible en la Tienda.

Para corregir el problema para los clientes que ya se han recibido el paquete problemático, puede enviar un nuevo paquete de Windows 10 que tiene un número de versión mayor que el paquete incorrecto tan pronto como pueda. Cuando el envío supere el proceso de certificación, todos los clientes se actualizarán al nuevo paquete, ya que su número de versión es mayor.


## <a name="version-numbering-for-windows81-and-earlier-and-windows-phone-81-packages"></a>Versión de numeración para Windows 8.1 (y versiones anteriores) y los paquetes de Windows Phone 8.1

> [!IMPORTANT]
> A partir del 31 de octubre de 2018, los productos recién creada no pueden incluir los paquetes destinados a 8.x/Windows Windows Phone 8.x o versiones anteriores. Para obtener más información, consulte este [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Para los paquetes .appx destinados a Windows Phone 8.1, el número de versión del paquete en un nuevo envío siempre debe ser mayor que el del paquete incluido en el último envío (o cualquier envío anterior).

Paquetes .appx que tienen como destino Windows 8 y Windows 8.1, la misma regla se aplica según la arquitectura: el número de versión del paquete en un nuevo envío siempre debe ser mayor que el del paquete se publicó por última vez que el Store para la misma arquitectura.

Además, el número de versión de los paquetes de Windows 8.1 siempre debe ser mayor que los números de versión de cualquiera de los paquetes de Windows 8 para la misma aplicación. En otras palabras, el número de versión de cualquier paquete de Windows 8 que se envía debe ser menor que el número de versión de cualquier paquete de Windows 8.1 que se ha enviado para la misma aplicación.

> [!NOTE]
> Si la aplicación también tiene paquetes de Windows 10, el número de versión de los paquetes de Windows 10 debe ser mayor que los de cualquiera de los paquetes de Windows 8, Windows 8.1 o Windows Phone 8.1. Para obtener más información, consulte [agregar paquetes para Windows 10 a una aplicación publicada previamente](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Estos son algunos ejemplos de lo que sucede en escenarios de actualización números de versión diferentes para los paquetes destinados a Windows 8 y Windows 8.1.

| Con esta versión de tu aplicación en la Tienda  | Y cargas esta versión | Una vez que la nueva versión esté en Store, se instalará como nueva adquisición | Una vez que la nueva versión esté en Store, se actualizará si algún cliente ya tiene la aplicación |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Nada                                     | x86, v1.0.0.0               | x86, v1.0.0.0 en ambos equipos, x86 y x64                                                | Nada. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 para la arquitectura del cliente                                                   | Nada. Los números de versión son los mismos. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 para clientes con x86 <br> v1.0.0.1 para clientes con x64                 | Nada para clientes que ejecutan la aplicación en un equipo x86. <br> v1.0.0.0 se actualizará a v1.0.0.1 para clientes que ejecuten la aplicación en un equipo x64. <br> **Tenga en cuenta**  si la versión de la aplicación se está ejecutando en un x64 de x86 equipo, la aplicación no se actualizan a la x64 versión a menos que el cliente desinstala y vuelve a instalar. |
| Nada                                     | independiente, v1.0.0.1           | independiente, v1.0.0.1 en todos los equipos                                                         | Nada. |
| independiente, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 para la arquitectura del equipo del cliente          | Nada. Aquellos que tengan la versión independiente v1.0.0.1 de la aplicación, podrán seguir usándola. |
| independiente, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 para la arquitectura del equipo del cliente | Nada para clientes que ejecutan la versión independiente v1.0.0.1 de la aplicación. <br> v1.0.0.0 se actualizará a v1.0.0.1 para clientes que ejecutan la versión v1.0.0.0 de la aplicación desarrollada para las arquitecturas específicas de sus equipos. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 para la arquitectura del equipo del cliente  | v1.0.0.1 se actualizará a v1.0.0.2 para los clientes que ejecuten la versión v1.0.0.1 de la aplicación compilada para la arquitectura específica de su equipo. |

> [!NOTE]
> A diferencia de los paquetes .appx, no se tienen en cuenta los números de versión de ningún paquete .xap al especificar qué paquete proporcionar a un cliente determinado. Para proporcionar a un cliente un paquete .xap a uno más reciente, asegúrate de quitar el antiguo paquete .xap del nuevo envío.
