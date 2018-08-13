---
author: mcleanbyron
description: Este artículo describen los códigos de error comunes para las operaciones de almacenamiento para aplicaciones y complementos, incluidos en la aplicación de compras, licencias y las actualizaciones de aplicaciones de instalación automática.
title: Códigos de error para las operaciones de Microsoft Store
ms.author: mcleans
ms.date: 08/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, las compras desde la aplicación, IAPs, complementos, códigos de error
ms.localizationpriority: medium
ms.openlocfilehash: 0931397e24eaba44cdf04092f5f367c43a91dd82
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "959389"
---
# <a name="error-codes-for-store-operations"></a>Códigos de error para las operaciones de Microsoft Store

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

En este artículo se describe los códigos de error comunes que pueden surgir mientras se está desarrollando o probando las operaciones relacionadas con el almacenamiento en la aplicación.

## <a name="in-app-purchase-error-codes"></a>Códigos de error en la aplicación de compra

Los siguientes códigos de error están relacionados con las operaciones de compra de la aplicación.

|  Código de error  |  Descripción  |
|--------------|---------------|
| 0x803F6100   | No se pudo completar la adquisición de aplicación debido a que la esquina de clase Kid está activo. Para completar la compra, inicie sesión en el dispositivo con su cuenta de Microsoft y ejecute de nuevo la aplicación.               |
| 0x803F6101   | No se pudo encontrar la aplicación especificada. La aplicación ya no estará disponible en el almacén, o es posible que haya proporcionado el identificador de almacenamiento incorrecto de la aplicación.     |
| 0x803F6102   | No se pudo encontrar el complemento especificado. El complemento ya no estará disponible en el almacén o su es posible que haya proporcionado el identificador de almacenamiento incorrecto para el complemento.                                               |
| 0x803F6103   | No se pudo encontrar el producto especificado. El producto ya no estará disponible en el almacén, o es posible que haya proporcionado el identificador de almacenamiento incorrecta para el producto.                                          |
| 0x803F6104   | No se pudo completar la adquisición de aplicación porque se está ejecutando una versión de prueba de la aplicación. Para completar las compras desde la aplicación, instale la versión completa de la aplicación.               |
| 0x803F6105   | No se pudo completar la adquisición de aplicación debido a que no están firmados con su cuenta de Microsoft.                                              |
| 0x803F6107   | Algo inesperado ha ocurrido durante el procesamiento de la operación actual.                                             |
| 0x803F6108   | No se pudo completar la adquisición de aplicación debido a que falta información de la licencia de aplicaciones. Este error puede producirse cuando se lado carga de la aplicación. Para resolver este problema, desinstale la aplicación y, a continuación, vuelva a instalarlo desde el almacén para actualizar la licencia de aplicaciones.                                          |
| 0x803F6109   | No se pudo completar el cumplimiento del complemento consumibles debido a que la cantidad especificada es mayor que el saldo pendiente.        |
| 0x803F610A   | No se admite el tipo de proveedor especificado para la cuenta de usuario de almacenamiento.                                            |
| 0x803F610B   | No se admite la operación de almacenamiento especificada.                                             |
| 0x803F610C   | La aplicación no es compatible con el contrato de tarea de fondo especificado.                                             |
| 0x80040001   | La lista proporcionada de producto complementario identificadores no es válido.                        |
| 0x80040002   | La lista proporcionada de palabras clave no es válida.                   |
| 0x80040003   | El destino de entrega no es válido.                       |

## <a name="licensing-error-codes"></a>Códigos de error de licencia

Los siguientes códigos de error están relacionados con las licencias de las operaciones para las aplicaciones o los complementos.

|  Código de error  |  Descripción  |
|--------------|---------------|
| 0x803F700C   | El dispositivo está actualmente sin conexión. Para usar esta aplicación mientras el dispositivo está sin conexión, abra la configuración de almacenamiento y permite activar o desactivar la opción **Permisos de sin conexión** .            |
| 0x803F8001   | No es necesario un derecho para el producto. Puede que esté utilizando una cuenta de Microsoft diferente a la que se usó para comprar el producto.           |
| 0x803F8002   | Ha caducado su derecho para el producto.           |
| 0x803F8003   | Su derecho del producto se encuentra en un estado no válido que impide que se creen una licencia.   |
| 0x803F8009<br/>0x803F800A   | Ha caducado el período de prueba de la aplicación.   |
| 0x803F8190   |  La licencia no permite el producto que se usará en el actual país o la región de su dispositivo.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Ha alcanzado el número máximo de dispositivos que se pueden usar con juegos y aplicaciones de la tienda. Para usar este juego o la aplicación en el dispositivo actual, en primer lugar quitar otro dispositivo de su cuenta.  |
| 0x803F9000<br/>0x803F9001    |  La licencia ha caducado o está dañado. Para ayudar a resolver este error, intente ejecutar el [Solucionador de problemas para aplicaciones de Windows](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) para restablecer la memoria caché del almacén.     |
| 0x803F9006    |  No se pudo completar la operación porque el usuario que tiene derecho a este producto no ha iniciado sesión el dispositivo con su cuenta de Microsoft.            |
| 0x803F9008<br/>0x803F9009    |  El dispositivo está sin conexión. El dispositivo debe estar en línea para usar este producto.            |
| 0x803F900A    |  La suscripción ha caducado.            |


## <a name="self-install-update-error-codes"></a>Instalar automáticamente los códigos de error de actualización

Los siguientes códigos de error están relacionados con la [instalación automática de actualizaciones de paquetes](../packaging/self-install-package-updates.md).

|  Código de error  |  Descripción  |
|--------------|---------------|
| 0x803F6200   | Consentimiento de usuario es necesario para descargar el paquete de actualización.               |
| 0x803F6201   | Consentimiento de usuario es necesario para descargar e instalar el paquete de actualización.                                                  |
| 0x803F6203   | Consentimiento de usuario es necesario para instalar el paquete de actualización.                                         |
| 0x803F6204   | Consentimiento de usuario es necesario para descargar la actualización de paquete debido a que la descarga se producirá en una conexión de red intencionadas.                                             |
| 0x803F6206   | Consentimiento de usuario es necesario para descargar e instalar la actualización de paquete debido a que la descarga se producirá en una conexión de red intencionadas.     |


## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de una suscripción para tu aplicación](enable-subscription-add-ons-for-your-app.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
