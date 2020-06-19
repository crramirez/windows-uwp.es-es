---
Description: El Microsoft Store exige ciertas reglas relacionadas con los números de versión, que funcionan de forma ligeramente diferente en diferentes versiones del sistema operativo.
title: Numeración de la versión del paquete
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d5ac50cdba542de56ffb1daee01da12d4979ac45
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945983"
---
# <a name="package-version-numbering"></a>Numeración de la versión del paquete

Cada paquete que proporciones debe tener un número de versión (proporcionado como un valor en el atributo **Version** del elemento [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) en el manifiesto de la aplicación). El Microsoft Store exige ciertas reglas relacionadas con los números de versión, que funcionan de forma ligeramente diferente en diferentes versiones del sistema operativo.

> [!NOTE]
> En este tema se hace referencia a "packages", pero a menos que se indique lo contrario, las mismas reglas se aplican a los números de versión de los archivos. msix/. appx y. msixbundle/. appxbundle.


## <a name="version-numbering-for-windows10-packages"></a>Números de versión para paquetes de Windows 10

> [!IMPORTANT]
> En el caso de los paquetes de Windows 10 (UWP), la última sección (cuarta) del número de versión se reserva para el uso de la tienda y debe dejarse como 0 al compilar el paquete (aunque el almacén puede cambiar el valor de esta sección). Las demás secciones se deben establecer en un entero comprendido entre 0 y 65535 (excepto para la primera sección, que no puede ser 0).

Al elegir un paquete de UWP del envío publicado, el Microsoft Store usará siempre el paquete de versión más reciente que sea aplicable al dispositivo Windows 10 del cliente. Esto te da más flexibilidad y te permite controlar qué paquetes se proporcionan a los clientes en tipos específicos de dispositivo. Es importante señalar que puedes enviar los paquetes en cualquier orden; no es necesario que los paquetes de un envío posterior tengan un número de versión más alto.

Puede proporcionar varios paquetes de UWP con el mismo número de versión. Sin embargo, los paquetes con el mismo número de versión no pueden tener también la misma arquitectura, ya que la identidad completa que la Tienda usa para cada paquete debe ser exclusiva. Para obtener más información, consulta [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Cuando se proporcionan varios paquetes UWP que usan el mismo número de versión, se usará la arquitectura (en el orden x64, x86, ARM, neutral) para decidir cuál es de rango superior (cuando el almacén determina qué paquete se debe proporcionar al dispositivo de un cliente). Al clasificar lotes de aplicaciones que usan el mismo número de versión, se detiene en cuenta la arquitectura de mayor rango. Un lote de aplicaciones con un paquete x64 tiene un rango superior a otro que solo contenga un paquete x86.

Esto ofrece mucha flexibilidad al evolucionar la aplicación a lo largo del tiempo. Puede cargar y enviar paquetes nuevos que usen números de versión inferiores para agregar compatibilidad con dispositivos de Windows 10 que no admitía anteriormente, puede agregar paquetes de versiones superiores que tengan dependencias más estrictas para aprovechar las características de hardware o del sistema operativo, o bien puede agregar paquetes de versiones superiores que actúen como actualizaciones para parte de la base de clientes de o de todas ellas.

El siguiente ejemplo muestra cómo se puede usar la numeración de versión a lo largo de varios envíos para ofrecer los paquetes previstos a los clientes.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Ejemplo: pasar a un único paquete a lo largo de varios envíos

Windows 10 te permite escribir un único código base que se ejecuta en cualquier lugar. Esto hace mucho más fácil iniciar un nuevo proyecto en varias plataformas. Sin embargo, por diversas razones, es posible que no quieras combinar código base ya existente para crear un único proyecto inmediatamente.

Puedes usar las reglas de control de versiones de paquete para llevar gradualmente a los clientes hasta un único paquete de la familia de dispositivos universal, a la vez que envías una serie de actualizaciones provisionales a familias de dispositivos concretas (incluidas aquellas que aprovechan las API de Windows 10). En el ejemplo siguiente se muestra cómo se aplican las mismas reglas de forma coherente a través de una serie de envíos para la misma aplicación.

| Envío | Contenido                                                  | Experiencia del cliente                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -   Versión del paquete: 1.1.10.0 <br> -   Familia de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versión del paquete: 1.1.0.0 <br> -   Familia de dispositivos: Windows.Mobile, minVersion 10.0.10240.0     | -   Los dispositivos de Windows 10 Desktop con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.10.0 <br> -   Los dispositivos de Windows 10 Mobile con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.0.0 <br> -   Las demás familias de dispositivos no podrán comprar e instalar la aplicación |
| 2          | -   Versión del paquete: 1.1.10.0 <br> -   Familia de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versión del paquete: 1.1.0.0 <br> -   Familia de dispositivos: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -   Versión del paquete: 1.0.0.0 <br> -   Familia de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Los dispositivos de Windows 10 Desktop con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.10.0 <br> -   Los dispositivos de Windows 10 Mobile con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.0.0 <br> -   Cuando otras familias de dispositivos (distintas de los tipos de escritorio y móvil) estén disponibles, obtendrán la versión 1.0.0.0 <br> -   Los dispositivos móviles y de escritorio que ya tengan instalada la aplicación no tendrán ninguna actualización (porque ya tienen la mejor versión disponible: 1.1.10.0 y 1.1.0.0 son mayores que 1.0.0.0) |
| 3          | -   Versión del paquete: 1.1.10.0 <br> -   Familia de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versión del paquete: 1.1.5.0 <br> -   Familia de dispositivos: Windows.Universal, minVersion 10.0.10250.0 <br> <br> -   Versión del paquete: 1.0.0.0 <br> -   Familia de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Los dispositivos de Windows 10 Desktop con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.10.0 <br> -   Los dispositivos de Windows 10 Mobile con la compilación 10.0.10250.0 o superior obtienen la versión 1.1.5.0 <br> -   Los dispositivos de Windows 10 Mobile con número de compilación >=10.0.10240.0 y <10.010250.0 obtienen la versión 1.1.0.0 
| 4          | -   Versión del paquete: 2.0.0.0 <br> -   Familia de dispositivos: Windows.Universal, minVersion 10.0.10240.0   | -   Todos los clientes de todas las familias de dispositivos de Windows 10 con la compilación v10.0.10240.0 o superior obtienen el paquete 2.0.0.0 | 

> [!NOTE]
>  En todos los casos, los dispositivos del cliente recibirán el paquete que tiene el número de versión más alto posible para el que se cumplan los requisitos. Por ejemplo, en el tercer envío antes descrito, todos los dispositivos de escritorio obtendrán la versión v1.1.10.0, aunque tengan la versión 10.0.10250.0 o superior del sistema operativo y, por lo tanto, también pudieran aceptar la versión v1.1.5.0. Dado que 1.1.10.0 es el número de versión más alto disponible para ellos, ese es el paquete que obtienen.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Usar el número de versión para revertir a un paquete distribuido anteriormente para las nuevas adquisiciones

Si conserva copias de los paquetes, tendrá la opción de revertir el paquete de la aplicación en la tienda a un paquete de Windows 10 anterior si detecta problemas con una versión. Se trata de una manera temporal de limitar la interrupción a los clientes mientras se tarda en solucionar el problema.

Para ello, cree un nuevo [envío](app-submissions.md). Quita el paquete problemático y carga el paquete anterior que quieras proporcionar en la Tienda. Los clientes que ya hayan recibido el paquete al que estás revirtiendo seguirán teniendo el paquete problemático (ya que el antiguo tiene un número de versión anterior). Sin embargo, así se evita que nadie pueda comprar el paquete problemático y mantiene la aplicación disponible en la Tienda.

Para solucionar el problema de los clientes que ya han recibido el paquete problemático, puede enviar un nuevo paquete de Windows 10 que tenga un número de versión superior al del paquete incorrecto en cuanto pueda. Cuando el envío supere el proceso de certificación, todos los clientes se actualizarán al nuevo paquete, ya que su número de versión es mayor.


## <a name="version-numbering-for-windows81-and-earlier-and-windows-phone-81-packages"></a>Números de versión para Windows 8.1 (y versiones anteriores) y paquetes de Windows Phone 8.1

> [!IMPORTANT]
> Ya no se pueden cargar nuevos paquetes XAP compilados con los SDK de Windows Phone 8. x. Las aplicaciones que ya se encuentran en el almacén con paquetes XAP seguirán funcionando en dispositivos Windows 10 Mobile. Para obtener más información, consulte esta [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Para los paquetes .appx destinados a Windows Phone 8.1, el número de versión del paquete en un nuevo envío siempre debe ser mayor que el del paquete incluido en el último envío (o cualquier envío anterior).

En el caso de los paquetes. appx que tienen como destino Windows 8 y Windows 8.1, se aplica la misma regla por arquitectura: el número de versión del paquete en un nuevo envío siempre debe ser mayor que el del paquete publicado por última vez en el almacén para la misma arquitectura.

Asimismo, el número de versión de los paquetes de Windows 8.1 siempre debe ser mayor que los números de versión de los paquetes de Windows 8 de la misma aplicación. Dicho de otro modo, el número de versión de cualquier paquete de Windows 8 que envíes deber ser inferior al número de versión de cualquier paquete de Windows 8.1 que hayas enviado para la misma aplicación.

> [!NOTE]
> Si la aplicación también tiene paquetes de Windows 10, el número de versión de los paquetes de Windows 10 debe ser mayor que los de cualquiera de los paquetes de Windows 8, Windows 8.1 o Windows Phone 8,1. Para obtener más información, consulte [agregar paquetes para Windows 10 a una aplicación publicada previamente](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Estos son algunos ejemplos de lo que sucede en los distintos escenarios de actualización de número de versión para los paquetes destinados a Windows 8 y Windows 8.1.

| Con esta versión de tu aplicación en la Tienda  | Y cargas esta versión | Una vez que la nueva versión se encuentra en el almacén, se instalará en una nueva adquisición. | Una vez que la nueva versión esté en el almacén, se actualizará si un cliente ya tiene la aplicación. |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Nothing                                     | x86, v1.0.0.0               | x86, v1.0.0.0 en ambos equipos, x86 y x64                                                | Nada. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 para la arquitectura del cliente                                                   | Nada. Los números de versión son los mismos. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 para clientes con x86 <br> v1.0.0.1 para clientes con x64                 | Nada para clientes que ejecutan la aplicación en un equipo x86. <br> v1.0.0.0 se actualizará a v1.0.0.1 para clientes que ejecuten la aplicación en un equipo x64. <br> **Nota:**    Si la versión x86 de la aplicación se ejecuta en un equipo x64, la aplicación no se actualizará a la versión x64 a menos que el cliente desinstale y vuelva a instalar. |
| Nothing                                     | independiente, v1.0.0.1           | independiente, v1.0.0.1 en todos los equipos                                                         | Nada. |
| independiente, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 para la arquitectura del equipo del cliente          | Nada. Aquellos que tengan la versión independiente v1.0.0.1 de la aplicación, podrán seguir usándola. |
| independiente, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 para la arquitectura del equipo del cliente | Nada para clientes que ejecutan la versión independiente v1.0.0.1 de la aplicación. <br> v1.0.0.0 se actualizará a v1.0.0.1 para clientes que ejecutan la versión v1.0.0.0 de la aplicación desarrollada para las arquitecturas específicas de sus equipos. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 para la arquitectura del equipo del cliente  | v1.0.0.1 se actualizará a v1.0.0.2 para los clientes que ejecuten la versión v1.0.0.1 de la aplicación compilada para la arquitectura específica de su equipo. |

> [!NOTE]
> A diferencia de los paquetes. appx, los números de versión de los paquetes. xap no se tienen en cuenta a la hora de determinar qué paquete debe proporcionar un cliente determinado. Para proporcionar a un cliente un paquete .xap a uno más reciente, asegúrate de quitar el antiguo paquete .xap del nuevo envío.
