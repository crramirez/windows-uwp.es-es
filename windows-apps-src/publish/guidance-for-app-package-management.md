---
author: jnHs
Description: "Descubre cómo se ponen los paquetes de la aplicación a disposición de los clientes y cómo se administran escenarios de paquetes específicos."
title: "Orientación para administrar paquetes de la aplicación"
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ac8d150f426c7d32e8a3b61b7f08cc0b84feedb8

---

# Orientación para administrar paquetes de la aplicación


Descubre cómo se ponen los paquetes de la aplicación a disposición de los clientes y cómo se administran escenarios de paquetes específicos.

-   [Versiones del sistema operativo y distribución de paquetes](#os-versions-and-package-distribution)
-   [Agregar paquetes para Windows 10 a una aplicación publicada anteriormente](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Mantener la compatibilidad de paquete para Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-8-1)
-   [Quitar una aplicación de la Tienda](#removing-an-app-from-the-store)
-   [Quitar paquetes de una familia de dispositivos anteriormente compatibles](#removing-packages-for-a-previously-supported-device-family)

## Versiones del sistema operativo y distribución de paquetes


Distintos sistemas operativos pueden ejecutar distintos tipos de paquetes. Si en el dispositivo de un cliente puede ejecutarse más de uno de los paquetes, la Tienda Windows proporcionará el más apropiado.

En general, un sistema operativo puede ejecutar los paquetes destinados a versiones anteriores del sistema operativo dentro de la misma familia de dispositivos. Sin embargo, los clientes solo obtendrán esos paquetes si la aplicación no incluye un paquete destinado a su versión actual del sistema operativo.

Por ejemplo, los dispositivos de Windows 10 pueden ejecutar todas las versiones de la aplicación compatibles con sistemas operativos anteriores (para la misma familia de dispositivos). Los dispositivos de escritorio de Windows 10 pueden ejecutar aplicaciones creadas para Windows 8.1 o Windows 8; los dispositivos móviles de Windows 10 pueden ejecutar aplicaciones creadas para Windows Phone 8.1, Windows Phone 8 e incluso Windows Phone 7.x.

Los siguientes ejemplos muestran varios escenarios para una aplicación que incluye paquetes destinados a diferentes sistemas operativos. En algunos casos, las limitaciones específicas de los paquetes pueden impedir que se ejecuten en todas las versiones del sistema operativo y en todos los tipos de dispositivo que se enumeran aquí (por ejemplo, la arquitectura debe ser la apropiada), pero estos ejemplos deberían ayudarte a entender qué versiones del sistema operativo pueden ejecutar los paquetes específicos.

### Ejemplo de aplicación 1

| Sistema operativo de destino del paquete | Sistemas operativos que obtendrán este paquete |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Dispositivos de escritorio con Windows 10, Windows 8.1      |
| Windows Phone 8.1                   | Dispositivos móviles con Windows 10, Windows Phone 8.1 |
| Windows Phone8                     | Windows Phone8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

En el ejemplo de aplicación 1, la aplicación todavía carece de paquetes para la plataforma universal de Windows (UWP) creados específicamente para dispositivos con Windows 10, pero los clientes de Windows 10 podrán obtener igualmente la aplicación. Obtendrán el mejor paquete disponible en función de su tipo de dispositivo.

### Ejemplo de aplicación 2

| Sistema operativo de destino del paquete  | Sistemas operativos que obtendrán este paquete |
|--------------------------------------|----------------------------------------------|
| Windows 10 (familia de dispositivos universal) | Windows 10 (todas las familias de dispositivos)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x, Windows Phone 8           |

En el ejemplo de aplicación 2, no hay ningún paquete que se pueda ejecutar en Windows 8. Los clientes que ejecutan otras versiones de sistema operativo pueden obtener la aplicación.

### Ejemplo de aplicación 3

| Sistema operativo de destino del paquete | Sistemas operativos que obtendrán este paquete                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 (familia de dispositivos de escritorio)  | Dispositivos de escritorio con Windows 10                                    |
| Windows Phone8                     | Dispositivos móviles con Windows 10, Windows Phone 8, Windows Phone 8.1 |

En el ejemplo de aplicación 3, como no hay ningún paquete para UWP destinado a la familia de dispositivos móviles, los clientes con dispositivos móviles Windows 10 recibirán el paquete de Windows Phone 8. Si esta aplicación agrega más adelante un paquete destinado a la familia de dispositivos móviles (o a la familia de dispositivos universal), dichos paquetes estarán entonces disponibles para los clientes con dispositivos móviles de Windows 10 en lugar del paquete de Windows Phone 8.

Ten en cuenta también que esta aplicación de ejemplo no incluye ningún paquete que se puede ejecutar en Windows 7.x.

### Ejemplo de aplicación 4

| Sistema operativo de destino del paquete  | Sistemas operativos que obtendrán este paquete |
|--------------------------------------|----------------------------------------------|
| Windows 10 (familia de dispositivos universal) | Windows 10 (todas las familias de dispositivos)             |

En el ejemplo de aplicación 4, cualquier dispositivo que ejecute Windows 10 puede obtener la aplicación, pero esta no estará disponible para los clientes con versiones anteriores del sistema operativo. Como el paquete para UWP está destinado a la familia de dispositivos universal, estará disponible para los dispositivos móviles y de escritorio de Windows 10.

## Agregar paquetes para Windows 10 a una aplicación publicada anteriormente


Si tienes una aplicación en la Tienda y quieres actualizarla para Windows 10, crea un nuevo envío y agrega los paquetes .appxupload para UWP en el paso [Paquetes](upload-app-packages.md). Después de que la aplicación pase por el proceso de certificación, los clientes que ya tenían tu aplicación antes de actualizar a Windows 10 podrán obtener el paquete para UWP como una actualización de la Tienda. El paquete para UWP también estará disponible en las adquisiciones nuevas para los clientes de Windows 10.

> 
            **Importante**  Cuando un cliente de Windows 10 obtenga el paquete para UWP, no podrás revertirlo para que use un paquete para una versión de un sistema operativo anterior. Asegúrate de que has probado exhaustivamente los paquetes para UWP en Windows 10 antes de agregarlos para su envío.

Puedes actualizar cualquiera de los demás paquetes al mismo tiempo o realizar otros cambios en el envío (por ejemplo, tal vez quieras [crear descripciones específicas para plataformas](create-platform-specific-descriptions.md) que se mostrarán a los clientes con versiones anteriores del sistema operativo). También puedes dejar todo lo demás exactamente igual, si lo prefieres.

> 
            **Nota**  El número de versión de los paquetes de Windows10 siempre debe ser mayor que los números de versión de los paquetes de Windows8, Windows8.1 o Windows Phone8.1 que publiques (o paquetes para versiones de esos sistemas operativos que hayas publicado anteriormente) para la misma aplicación. Para más información sobre los números de versión en Windows 10, consulta [Numeración de la versión del paquete](package-version-numbering.md).

Cuando el nuevo envío complete el proceso de certificación, los paquetes para UWP estarán disponibles, junto con cualquier otro paquete que hayas puesto a disposición de los clientes que aún no cuentan con Windows 10.

Para más información sobre cómo empaquetar aplicaciones para UWP para la Tienda, consulta [Empaquetar aplicaciones universales de Windows para Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

> 
            **Importante**  Ten en cuenta que si proporcionas paquetes destinados a la familia de dispositivos universal, todos los clientes que ya tuvieran tu aplicación en un sistema operativo anterior (Windows Phone 8, Windows 8.1, etc.) y que luego se actualicen a Windows 10 se actualizarán al paquete universal de Windows 10.
> 
> Esto sucederá aunque hayas excluido una familia de dispositivos específica en el paso [Precios y disponibilidad](set-app-pricing-and-availability.md#windows-10-device-families) del envío, dado que la selección **Familias de dispositivos** solo se aplica a las nuevas adquisiciones. Si no quieres que todos los clientes anteriores obtengan el nuevo paquete de Windows 10, asegúrate de actualizar el elemento [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) del manifiesto appx para incluir solo la familia de dispositivos concreta que quieras admitir.
> 
> Por ejemplo, si solo quieres que los clientes de Windows 8 y Windows 8.1 que se hayan actualizado a Windows 10 obtengan tu aplicación para UWP y quieres que los clientes con Windows Phone 8.1 y versiones anteriores mantengan los paquetes que ya habías puesto a su disposición (destinados a Windows Phone 8 o Windows Phone 8.1). Para ello, tendrás que asegurarte de que actualizas el [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) en el manifiesto appx para que incluya solo **Windows.Desktop** (para la familia de dispositivos de escritorio), en lugar de dejarlo con el valor **Windows.Universal** (para la familia de dispositivos universal) que Microsoft Visual Studio incluye en el manifiesto appx de manera predeterminada. No envíes ningún paquete para UWP destinado a las familias de dispositivos universal o móvil (**Windows.Universal** o **Windows.Universal**). De esta forma, los clientes móviles de Windows 10 no obtendrán ninguno de los paquetes para UWP.
> 
> Para obtener más información acerca de las familias de dispositivos, consulta [Guía de aplicaciones de la Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631).

## Mantener la compatibilidad de paquete para Windows Phone 8.1


Se aplican algunos requisitos a los tipos de paquetes al actualizar aplicaciones publicadas anteriormente para Windows Phone 8.1:

-   Una vez que una aplicación tiene un paquete publicado de Windows Phone 8.1, todas las actualizaciones posteriores deben contener un paquete de Windows Phone 8.1.
-   Una vez que una aplicación tiene un XAP publicado de Windows Phone 8.1, todas las actualizaciones posteriores deben contener un XAP de Windows Phone 8.1, un .appx de Windows Phone 8.1 o un .appxbundle de Windows Phone 8.1.
-   Una vez que una aplicación tiene un appx publicado de Windows Phone 8.1, todas las actualizaciones posteriores deben contener un .appx de Windows Phone 8.1 o un .appxbundle de Windows Phone 8.1. En otras palabras, no se permite un XAP de Windows Phone 8.1. Esto también se aplica a un .appxupload que contenga un .appx de Windows Phone 8.1.
-   Una vez que una aplicación tiene un .appxbundle publicado de Windows Phone 8.1, todas las actualizaciones posteriores deben contener un .appxbundle de Windows Phone 8.1. En otras palabras, no se permite un XAP de Windows Phone 8.1 o un .appx de Windows Phone 8.1. Esto también se aplica a un .appxupload que contenga un .appxbundle de Windows Phone 8.1.

Si no se siguen estas reglas se producirán errores al cargar el paquete, lo que impedirá que el envío se complete.

## Quitar una aplicación de la Tienda


En ocasiones, es posible que quieras dejar de ofrecer definitivamente una aplicación a los clientes, es decir, "cancelar su publicación". Para ello, haz clic en **Make app unavailable** en la página de información general de la aplicación. Después de confirmar que quieres que la aplicación deje de estar disponible, en el plazo de unas horas dejará de estar visible en la Tienda y ningún cliente nuevo podrá acceder a ella de ninguna manera, ni siquiera con códigos promocionales.

> 
            **Importante**  Esta acción reemplazará cualquier configuración de [distribución y visibilidad](set-app-pricing-and-availability.md#distribution-and-visibility) que tengas seleccionada en los envíos.

Ten en cuenta que los clientes que ya tengan la aplicación podrán seguir usándola (y podrían incluso recibir actualizaciones si envías nuevos paquetes más adelante).

Después de hacer que la aplicación deje de estar disponible, la seguirás viendo en el panel. Si decides volver a ofrecer la aplicación a los clientes, puedes hacer clic en **Make app available** en la página de información general de la aplicación. Después de confirmar la acción, la aplicación estará disponible para los nuevos clientes (a menos que la configuración de tu último envío lo impida) en cuestión de horas.

> 
            **Nota**  Si quieres que tu aplicación siga estando disponible, pero no quieres seguir ofreciéndola a los clientes con una versión de sistema operativo determinada, puedes crear un nuevo envío y quitar todos los paquetes de la versión de sistema operativo para la que quieres impedir nuevas compras. Por ejemplo, si anteriormente tenías paquetes para Windows Phone8, Windows Phone8.1 y Windows10, y no quieres seguir ofreciendo la aplicación a los clientes nuevos de WindowsPhone8, quita los paquetes de WindowsPhone8 del envío. Después de que se publique la actualización, los clientes nuevos de WindowsPhone8 no podrán comprar la aplicación (aunque los clientes que ya la tengan podrán seguir usándola). La aplicación seguirá disponible para los clientes nuevos de WindowsPhone8.1 y Windows10.

## Quitar paquetes de una familia de dispositivos anteriormente compatibles


Si quitas todos los paquetes de una determinada familia de dispositivos que era compatible anteriormente con la aplicación, se te pedirá que confirmes que esta es tu intención antes de poder guardar los cambios en la página **Paquetes**.

Al publicar un envío que quite los paquetes de una familia de dispositivos que era compatible con la aplicación anteriormente, los nuevos clientes no podrán comprar la aplicación en esa familia de dispositivos. Siempre puedes publicar otra actualización más adelante para proporcionar paquetes para esa familia de dispositivos de nuevo.

Ten en cuenta que aunque quites todos los paquetes que admitan una determinada familia de dispositivos, los clientes existentes que ya hayan instalado la aplicación en ese tipo de dispositivo pueden seguir usándola, y obtendrán las actualizaciones que proporciones más adelante.

 

 







<!--HONumber=Jun16_HO4-->


