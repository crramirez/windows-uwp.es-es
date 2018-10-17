---
author: jnHs
Description: The Microsoft Store enforces certain rules related to version numbers, which work somewhat differently in different OS versions.
title: Numeración de la versión del paquete
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7cf93cf06b273605b91c31da5b6a6b8cef8dae39
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "4746878"
---
# <a name="package-version-numbering"></a>Numeración de la versión del paquete

Cada paquete que proporciones debe tener un número de versión (proporcionado como un valor en el atributo **Version** del elemento **Package/Identity** en el manifiesto de la aplicación). Microsoft Store aplica ciertas reglas relacionadas con los números de versión que dependen de las diferentes versiones del sistema operativo.

> [!NOTE]
> Este tema hace referencia a "paquetes" pero, a menos que se indique lo contrario, las mismas reglas se aplican a los números de versión para los archivos.msix/.appx y.msixbundle/.appxbundle.


## <a name="version-numbering-for-windows-10-packages"></a>Números de versión para paquetes de Windows 10

> [!IMPORTANT]
> Para los paquetes de Windows 10 (UWP), la última sección (cuarta) del número de versión se reserva para uso de la tienda y se debe mantener como 0 al compilar el paquete (aunque la tienda puede cambiar el valor de esta sección).

Al elegir un paquete para UWP desde tu envío publicado, Microsoft Store siempre usará el paquete de la última versión que sea compatible con el dispositivo del cliente de Windows 10. Esto te da más flexibilidad y te permite controlar qué paquetes se proporcionan a los clientes en tipos específicos de dispositivo. Es importante señalar que puedes enviar los paquetes en cualquier orden; no es necesario que los paquetes de un envío posterior tengan un número de versión más alto.

Puedes proporcionar varios paquetes para UWP con el mismo número de versión. Sin embargo, los paquetes con el mismo número de versión no pueden tener también la misma arquitectura, ya que la identidad completa que la Tienda usa para cada paquete debe ser exclusiva. Para obtener más información, consulta [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Al proporcionar varios paquetes UWP que usen el mismo número de versión, la arquitectura (en el orden x64, x 86, ARM, independiente) se usará para decidir cuál es el intervalo superior (cuando el almacén determina qué paquete proporcionar a los dispositivos de un cliente). Al clasificar lotes de aplicaciones que usan el mismo número de versión, se detiene en cuenta la arquitectura de mayor rango. Un lote de aplicaciones con un paquete x64 tiene un rango superior a otro que solo contenga un paquete x86.

Esto ofrece mucha flexibilidad al evolucionar la aplicación a lo largo del tiempo. Puedes cargar y enviar nuevos paquetes que usan los números de versión inferiores para agregar compatibilidad con dispositivos Windows 10 que no admitían anteriormente, puedes agregar paquetes que tienen dependencias más estrictas para aprovechar las ventajas del hardware o características del sistema operativo o bien agregar paquetes que sirven como actualizaciones para algunos o todos tus clientes base.

El siguiente ejemplo muestra cómo se puede usar la numeración de versión a lo largo de varios envíos para ofrecer los paquetes previstos a los clientes.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Ejemplo: pasar a un único paquete a lo largo de varios envíos

Windows 10 te permite escribir un único código base que se ejecuta en cualquier lugar. Esto hace mucho más fácil iniciar un nuevo proyecto en varias plataformas. Sin embargo, por diversas razones, es posible que no quieras combinar código base ya existente para crear un único proyecto inmediatamente.

Puedes usar las reglas de control de versiones de paquete para llevar gradualmente a los clientes hasta un único paquete de la familia de dispositivos universal, a la vez que envías una serie de actualizaciones provisionales a familias de dispositivos concretas (incluidas aquellas que aprovechan las API de Windows 10). El siguiente ejemplo muestra cómo se aplican las mismas reglas de forma coherente a través de una serie de envíos para la misma aplicación.

| Envío | Contenido                                                  | Experiencia del cliente                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -   Versión del paquete: 1.1.10.0 <br> -   Familia de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versión del paquete: 1.1.0.0 <br> -   Familia de dispositivos: Windows.Mobile, minVersion 10.0.10240.0     | -   Los dispositivos de Windows 10 Desktop con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.10.0 <br> -   Los dispositivos de Windows 10 Mobile con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.0.0 <br> -   Las demás familias de dispositivos no podrán comprar e instalar la aplicación |
| 2          | -   Versión del paquete: 1.1.10.0 <br> -   Familia de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versión del paquete: 1.1.0.0 <br> -   Familia de dispositivos: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -   Versión del paquete: 1.0.0.0 <br> -   Familia de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Los dispositivos de Windows 10 Desktop con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.10.0 <br> -   Los dispositivos de Windows 10 Mobile con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.0.0 <br> -   Cuando otras familias de dispositivos (distintas de los tipos de escritorio y móvil) estén disponibles, obtendrán la versión 1.0.0.0 <br> -   Los dispositivos móviles y de escritorio que ya tengan instalada la aplicación no tendrán ninguna actualización (porque ya tienen la mejor versión disponible: 1.1.10.0 y 1.1.0.0 son mayores que 1.0.0.0) |
| 3          | -   Versión del paquete: 1.1.10.0 <br> -   Familia de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versión del paquete: 1.1.5.0 <br> -   Familia de dispositivos: Windows.Universal, minVersion 10.0.10250.0 <br> <br> -   Versión del paquete: 1.0.0.0 <br> -   Familia de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Los dispositivos de Windows 10 Desktop con la compilación 10.0.10240.0 o superior obtienen la versión 1.1.10.0 <br> -   Los dispositivos de Windows 10 Mobile con la compilación 10.0.10250.0 o superior obtienen la versión 1.1.5.0 <br> -   Los dispositivos de Windows 10 Mobile con número de compilación >=10.0.10240.0 y <10.010250.0 obtienen la versión 1.1.0.0 
| 4          | -   Versión del paquete: 2.0.0.0 <br> -   Familia de dispositivos: Windows.Universal, minVersion 10.0.10240.0   | -   Todos los clientes de todas las familias de dispositivos de Windows 10 con la compilación v10.0.10240.0 o superior obtienen el paquete 2.0.0.0 | 

> [!NOTE]
>  En todos los casos, los dispositivos de los clientes recibirán el paquete con el mayor número de versión que admitan. Por ejemplo, en el tercer envío antes descrito, todos los dispositivos de escritorio obtendrán la versión v1.1.10.0, aunque tengan la versión 10.0.10250.0 o superior del sistema operativo y, por lo tanto, también pudieran aceptar la versión v1.1.5.0. Dado que 1.1.10.0 es el número de versión más alto disponible para ellos, ese es el paquete que obtienen.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Usar el número de versión para revertir a un paquete distribuido anteriormente para las nuevas adquisiciones

Si guardas copias de los paquetes, tendrás la opción de revertir el paquete de la aplicación en el almacén de un paquete de Windows 10 anterior si debe detectar problemas con alguna versión. Se trata de una manera temporal de limitar la molestia para tus clientes mientras te tomas tiempo para solucionar el problema.

Para ello, crea un nuevo [envío](app-submissions.md). Quita el paquete problemático y carga el paquete anterior que quieras proporcionar en la Tienda. Los clientes que ya hayan recibido el paquete al que estás revirtiendo seguirán teniendo el paquete problemático (ya que el antiguo tiene un número de versión anterior). Sin embargo, así se evita que nadie pueda comprar el paquete problemático y mantiene la aplicación disponible en la Tienda.

Para solucionar el problema de los clientes que ya hayan recibido el paquete problemático, puedes enviar un nuevo paquete de Windows 10 que tiene un número de versión superior del paquete dañado tan pronto como sea posible. Cuando el envío supere el proceso de certificación, todos los clientes se actualizarán al nuevo paquete, ya que su número de versión es mayor.


## <a name="version-numbering-for-windows-81-and-earlier-and-windows-phone-81-packages"></a>Números de versión para Windows 8.1 (y versiones anteriores) y paquetes de Windows Phone 8.1

Para los paquetes .appx destinados a Windows Phone 8.1, el número de versión del paquete en un nuevo envío siempre debe ser mayor que el del paquete incluido en el último envío (o cualquier envío anterior).

Para los paquetes .appx destinados a Windows 8 y Windows 8.1, se aplica la misma regla según la arquitectura: el número de versión del paquete de un nuevo envío siempre debe ser mayor que el del último paquete publicado en Store para la misma arquitectura.

Asimismo, el número de versión de los paquetes de Windows 8.1 siempre debe ser mayor que los números de versión de los paquetes de Windows 8 de la misma aplicación. Dicho de otro modo, el número de versión de cualquier paquete de Windows 8 que envíes deber ser inferior al número de versión de cualquier paquete de Windows 8.1 que hayas enviado para la misma aplicación.

> [!NOTE]
> Si la aplicación también tiene paquetes de Windows 10, el número de versión de los paquetes de Windows 10 debe ser mayor que los de cualquiera de los paquetes de Windows 8, Windows 8.1 o Windows Phone 8.1. Para obtener más información, consulta [Agregar paquetes para Windows 10 a una aplicación publicada anteriormente](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Estos son algunos ejemplos de lo que sucede en los escenarios de actualización de número de versión diferente para paquetes destinados a Windows 8 y Windows 8.1.

| Con esta versión de tu aplicación en la Tienda  | Después de cargar esta versión | Una vez que la nueva versión esté en Store, se instalará como nueva adquisición | Una vez que la nueva versión esté en Store, se actualizará si algún cliente ya tiene la aplicación |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Nada                                     | x86, v1.0.0.0               | x86, v1.0.0.0 en ambos equipos, x86 y x64                                                | Nada |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 para la arquitectura del cliente                                                   | Nada Los números de versión son los mismos. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 para clientes con x86 <br> v1.0.0.1 para clientes con x64                 | Nada para clientes que ejecutan la aplicación en un equipo x86. <br> v1.0.0.0 se actualizará a v1.0.0.1 para clientes que ejecuten la aplicación en un equipo x64. <br> **Nota**  Si la versión x86 de la aplicación se ejecuta en un equipo x64, la aplicación no se actualizará a la versión x64 a menos que el cliente la desinstale y la vuelva a instalar. |
| Nada                                     | independiente, v1.0.0.1           | independiente, v1.0.0.1 en todos los equipos                                                         | Nada |
| independiente, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 para la arquitectura del equipo del cliente          | Nada Aquellos que tengan la versión independiente v1.0.0.1 de la aplicación, podrán seguir usándola. |
| independiente, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 para la arquitectura del equipo del cliente | Nada para clientes que ejecutan la versión independiente v1.0.0.1 de la aplicación. <br> v1.0.0.0 se actualizará a v1.0.0.1 para clientes que ejecutan la versión v1.0.0.0 de la aplicación desarrollada para las arquitecturas específicas de sus equipos. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 para la arquitectura del equipo del cliente  | v1.0.0.1 se actualizará a v1.0.0.2 para los clientes que ejecuten la versión v1.0.0.1 de la aplicación compilada para la arquitectura específica de su equipo. |

> [!NOTE]
> A diferencia de los paquetes .appx, no se tienen en cuenta los números de versión de ningún paquete .xap al especificar qué paquete proporcionar a un cliente determinado. Para proporcionar a un cliente un paquete .xap a uno más reciente, asegúrate de quitar el antiguo paquete .xap del nuevo envío.
