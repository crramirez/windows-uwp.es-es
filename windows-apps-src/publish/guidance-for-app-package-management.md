---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: Instrucciones para la administración de paquetes de la aplicación
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b0b6315b1177138c3ede7834e2dbc792ee106dd
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "4059233"
---
# <a name="guidance-for-app-package-management"></a>Orientación para administrar paquetes de la aplicación

Descubre cómo se ponen los paquetes de la aplicación a disposición de los clientes y cómo se administran escenarios de paquetes específicos.

-   [Versiones del sistema operativo y distribución de paquetes](#os-versions-and-package-distribution)
-   [Agregar paquetes para Windows 10 a una aplicación publicada anteriormente](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Mantener la compatibilidad de paquete para Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-81)
-   [Quitar una aplicación de la Tienda](#removing-an-app-from-the-store)
-   [Quitar paquetes de una familia de dispositivos anteriormente compatibles](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versiones del sistema operativo y distribución de paquetes

Distintos sistemas operativos pueden ejecutar distintos tipos de paquetes. Si en el dispositivo de un cliente puede ejecutarse más de uno de los paquetes, Microsoft Store proporcionará el más apropiado.

En general, las versiones más posteriores del sistema operativo pueden ejecutar paquetes destinados a versiones anteriores del sistema operativo de la misma familia de dispositivos. Sin embargo, los clientes solo obtendrán esos paquetes si la aplicación no incluye un paquete destinado a su versión del sistema operativo actual.

Por ejemplo, los dispositivos de Windows 10 pueden ejecutar todas las versiones de la aplicación compatibles con sistemas operativos anteriores (para la misma familia de dispositivos). Los dispositivos de escritorio de Windows 10 pueden ejecutar aplicaciones creadas para Windows 8.1 o Windows 8; los dispositivos móviles de Windows 10 pueden ejecutar aplicaciones creadas para Windows Phone 8.1, Windows Phone 8 e incluso Windows Phone 7.x. 

En los siguientes ejemplos se muestran varios escenarios para una aplicación que incluye paquetes destinados a diferentes versiones del sistema operativo (a menos que las limitaciones específicas de los paquetes no permitan su ejecución en cada tipo de versión o dispositivo de sistema operativo que aquí se indican; por ejemplo, la arquitectura del paquete debe ser adecuada para el dispositivo). 

### <a name="example-app-1"></a>Ejemplo de aplicación 1

| Sistema operativo de destino del paquete | Sistemas operativos que obtendrán este paquete |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Dispositivos de escritorio con Windows 10, Windows 8.1      |
| Windows Phone 8.1                   | Dispositivos móviles con Windows 10, Windows Phone 8.1 |
| Windows Phone8                     | Windows Phone8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

En el ejemplo de aplicación 1, la aplicación todavía carece de paquetes para la plataforma universal de Windows (UWP) creados específicamente para dispositivos con Windows 10, pero los clientes de Windows 10 podrán obtener igualmente la aplicación. Obtendrán los mejores paquetes disponibles para su tipo de dispositivo.

### <a name="example-app-2"></a>Ejemplo de aplicación 2

| Sistema operativo de destino del paquete  | Sistemas operativos que obtendrán este paquete |
|--------------------------------------|----------------------------------------------|
| Windows 10 (familia de dispositivos universal) | Windows 10 (todas las familias de dispositivos)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x, Windows Phone 8           |

En el ejemplo de aplicación 2, no hay ningún paquete que se pueda ejecutar en Windows 8. Los clientes que ejecutan cualquier otra versión de sistema operativo pueden obtener la aplicación. Todos los clientes con Windows 10 recibirán el mismo paquete.

### <a name="example-app-3"></a>Ejemplo de aplicación 3

| Sistema operativo de destino del paquete | Sistemas operativos que obtendrán este paquete                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 (familia de dispositivos de escritorio)  | Dispositivos de escritorio con Windows 10                                    |
| Windows Phone8                     | Dispositivos móviles con Windows 10, Windows Phone 8, Windows Phone 8.1 |

En el ejemplo de aplicación 3, como no hay ningún paquete para UWP destinado a la familia de dispositivos móviles, los clientes con dispositivos móviles Windows 10 recibirán el paquete de Windows Phone 8. Si esta aplicación agrega más adelante un paquete destinado a la familia de dispositivos móviles (o a la familia de dispositivos universal), dicho paquete estará entonces disponible para los clientes con dispositivos móviles Windows 10 en lugar del paquete de Windows Phone 8.

Ten en cuenta también que esta aplicación de ejemplo no incluye ningún paquete que se puede ejecutar en Windows Phone 7.x.

### <a name="example-app-4"></a>Ejemplo de aplicación 4

| Sistema operativo de destino del paquete  | Sistemas operativos que obtendrán este paquete |
|--------------------------------------|----------------------------------------------|
| Windows 10 (familia de dispositivos universal) | Windows 10 (todas las familias de dispositivos)             |

En el ejemplo de aplicación 4, cualquier dispositivo que ejecute Windows 10 puede obtener la aplicación, pero esta no estará disponible para los clientes con versiones anteriores del sistema operativo. Dado que el paquete para UWP está destinado a la familia de dispositivos universal, estará disponible para cualquier dispositivo Windows 10 (por las [selecciones de disponibilidad de familias de dispositivos](device-family-availability.md)).


## <a name="removing-an-app-from-the-store"></a>Quitar una aplicación de Store

En ocasiones, es posible que quieras dejar de ofrecer una aplicación a los clientes, es decir, "dejar de publicarla". Para ello, haz clic en **Make app unavailable** en la página de **Información general de la aplicación**. Después de confirmar que quieres que la aplicación deje de estar disponible, en el plazo de unas horas dejará de estar visible en Microsoft Store y ningún cliente nuevo podrá acceder a ella (a no ser que tengan un [código promocional](generate-promotional-codes.md) y usen un dispositivo Windows 10).

> [!IMPORTANT]
> Esta opción reemplazará cualquier configuración de [visibilidad](choose-visibility-options.md#discoverability) que tengas seleccionada en los envíos. 

Esta opción tiene el mismo efecto que si hubieras creado un envío y hubieras elegido **Hacer este producto disponible, pero no detectable, en Store** con la opción **Detener adquisición**. Sin embargo, no requiere crear un nuevo envío.

Ten en cuenta que los clientes que ya tengan la aplicación podrán seguir usándola y podrás volver a descargarla (y podrían incluso recibir actualizaciones si envías nuevos paquetes más adelante).

Después de hacer que la aplicación deje de estar disponible, la seguirás viendo en el panel. Si decides volver a ofrecer la aplicación a los clientes, puedes hacer clic en **Make app available** en la página de información general de la aplicación. Después de confirmar la acción, la aplicación estará disponible para los nuevos clientes (a menos que la configuración de tu último envío lo impida) en cuestión de horas.

> [!NOTE]
> Si quieres que tu aplicación siga estando disponible, pero no quieres seguir ofreciéndola a los nuevos clientes con una versión de sistema operativo determinada, puedes crear un nuevo envío y quitar todos los paquetes de la versión de sistema operativo para la que quieres impedir nuevas compras. Por ejemplo, si anteriormente tenías paquetes para Windows Phone8.1 y Windows10, y no quieres seguir ofreciendo la aplicación a los clientes nuevos de Windows Phone 8.1, quita todos los paquetes de Windows Phone 8.1 del envío. Después de que se publique la actualización, los clientes nuevos de WindowsPhone8.1 no podrán comprar la aplicación (aunque los clientes que ya la tengan podrán seguir usándola). Sin embargo, la aplicación seguirá disponible para los clientes nuevos de Windows10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Quitar paquetes de una familia de dispositivos anteriormente compatibles

Si quitas todos los paquetes de un determinado [familia de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) que anteriormente compatible con la aplicación, se te pedirá para confirmar que esta es tu intención antes de poder guardar los cambios en la página de **paquetes** .

Al publicar un envío que quite todos los paquetes que podrían ejecutarse en una familia de dispositivos que era compatible con la aplicación, los clientes nuevos no podrán comprar la aplicación en esa familia. Siempre puedes publicar otra actualización más adelante para proporcionar paquetes para esa familia de dispositivos de nuevo.

Ten en cuenta que aunque quites todos los paquetes que admitan una determinada familia de dispositivos, los clientes existentes que ya hayan instalado la aplicación en ese tipo de dispositivo pueden seguir usándola, y obtendrán las actualizaciones que proporciones más adelante.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>Agregar paquetes para Windows 10 a una aplicación publicada anteriormente

Si tienes una aplicación en la Tienda dirigida a Windows 8.x o Windows Phone 8.x y quieres actualizarla para Windows 10, crea un nuevo envío y agrega los paquetes .appxupload para UWP en el paso [Paquetes](upload-app-packages.md). Después de que la aplicación pase por el proceso de certificación, los clientes que ya tenían tu aplicación y están ahora en Windows 10 obtendrán el paquete para UWP como una actualización de la tienda. El paquete para UWP también estará disponible en las adquisiciones nuevas para los clientes de Windows 10.

> [!NOTE]
> Una vez que un cliente de Windows 10 obtiene el paquete para UWP, no puedes revertirlo para que use un paquete para una versión anterior del sistema operativo. 

Ten en cuenta que el número de versión de los paquetes de Windows10 siempre debe ser mayor que los números de versión de los paquetes de Windows8, Windows8.1 o Windows Phone8.1 que incluyas (o paquetes de la versión del sistema operativo que ya hayas publicado). Para obtener más información, consulta [Numeración de la versión del paquete](package-version-numbering.md).

Para obtener más información sobre cómo empaquetar aplicaciones para UWP para Microsoft Store, consulta [Empaquetado de aplicaciones](../packaging/index.md).

> [!IMPORTANT]
> Ten en cuenta que si proporcionas paquetes destinados a la familia de dispositivos universal, todos los clientes que ya tuvieran tu aplicación en un sistema operativo anterior (Windows Phone 8, Windows 8.1, etc.) y que actualicen a Windows 10, se actualizarán para obtener el paquete de Windows 10.
> 
> Esto ocurre incluso si se ha excluido una familia de dispositivos específica en el paso de la [disponibilidad de familia de dispositivos](device-family-availability.md) de tu envío, desde el que la sección solo se aplica a las nuevas adquisiciones. Si no quieres que todos los clientes anteriores obtengan el nuevo paquete de Windows 10 universal, asegúrate de actualizar el elemento [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) del manifiesto appx para incluir solo la familia de dispositivos concreta que quieras admitir.
> 
> Por ejemplo, supongamos que desea que los clientes de Windows 8 y Windows 8.1 que se hayan actualizado a un dispositivo de escritorio de Windows 10 para obtener la nueva aplicación para UWP, pero quieres que los clientes de Windows Phone que ahora se en dispositivos de Windows 10 Mobile para mantener los paquetes que anteriormente realizan availabl e (destinados a Windows Phone 8 o Windows Phone 8.1). Para ello, tendrás que actualizar el [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) en el manifiesto appx para incluir solo **Windows.Desktop** (para la familia de dispositivos de escritorio), en lugar de dejarlo como el valor **Windows.Universal** (correspondiente a la familia de dispositivos universal) Microsoft Visual Studio incluye en el manifiesto de manera predeterminada. No envíes ningún paquete para UWP destinado a las familias de dispositivos Universal o Mobile (**Windows.Universal** o **Windows.Universal**). De esta forma, los clientes de Windows 10 Mobile no obtendrán ninguno de los paquetes para UWP.


## <a name="maintaining-package-compatibility-for-windows-phone-81"></a>Mantener la compatibilidad de paquete para Windows Phone 8.1

Se aplican algunos requisitos a los tipos de paquetes al actualizar aplicaciones publicadas anteriormente para Windows Phone 8.1:

-   Una vez que una aplicación tiene un paquete publicado de Windows Phone 8.1, todas las actualizaciones posteriores deben contener un paquete de Windows Phone 8.1.
-   Una vez que una aplicación tiene un XAP publicado de Windows Phone 8.1, todas las actualizaciones posteriores deben contener un XAP de Windows Phone 8.1, un .appx de Windows Phone 8.1 o un .appxbundle de Windows Phone 8.1.
-   Una vez que una aplicación tiene un appx publicado de Windows Phone 8.1, todas las actualizaciones posteriores deben contener un .appx de Windows Phone 8.1 o un .appxbundle de Windows Phone 8.1. En otras palabras, no se permite un XAP de Windows Phone 8.1. Esto también se aplica a un .appxupload que contenga un .appx de Windows Phone 8.1.
-   Una vez que una aplicación tiene un .appxbundle publicado de Windows Phone 8.1, todas las actualizaciones posteriores deben contener un .appxbundle de Windows Phone 8.1. En otras palabras, no se permite un XAP de Windows Phone 8.1 o un .appx de Windows Phone 8.1. Esto también se aplica a un .appxupload que contenga un .appxbundle de Windows Phone 8.1.

Si no se siguen estas reglas, es posible que se produzcan errores al cargar el paquete, lo que impedirá que el envío se complete.