---
author: Xansky
description: En este artículo se describe los códigos de error comunes para las operaciones de la tienda de aplicaciones y complementos, incluidos en la aplicación de compras, licencias y actualizaciones de la aplicación de instalación automática.
title: Códigos de error para las operaciones de Microsoft Store
ms.author: mhopkins
ms.date: 08/24/2017
ms.topic: article
keywords: Windows 10, uwp, compras desde la aplicación, IAP, complementos, códigos de error
ms.localizationpriority: medium
ms.openlocfilehash: 1a4eff890da48bd60405cadee2d7ecb92bb1b2fa
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5934030"
---
# <a name="error-codes-for-store-operations"></a>Códigos de error para las operaciones de Microsoft Store

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

En este artículo se describe los códigos de error comunes que podrían surgir mientras desarrollar o probar las operaciones relacionadas con la tienda en la aplicación.

## <a name="in-app-purchase-error-codes"></a>Códigos de error de compra desde la aplicación

Los siguientes códigos de error están relacionados con las operaciones de compra desde la aplicación.

|  Código de error  |  Descripción  |
|--------------|---------------|
| 0x803F6100   | No se pudo completar la compra en la aplicación porque rincón infantil está activo. Para completar la compra, inicia sesión en el dispositivo con tu cuenta de Microsoft y ejecutar la aplicación de nuevo.               |
| 0x803F6101   | No se pudo encontrar la aplicación especificada. La aplicación ya no estará disponible en la tienda o es posible que haya proporcionado el Id. de Store incorrecto para la aplicación.     |
| 0x803F6102   | No se pudo encontrar el complemento especificado. El complemento ya no estará disponible en la tienda o tu es posible que haya proporcionado el Id. de Store incorrecto para el complemento.                                               |
| 0x803F6103   | No se pudo encontrar el producto especificado. El producto ya no estará disponible en la tienda o es posible que has proporcionado el Id. de Store incorrecto para el producto.                                          |
| 0x803F6104   | No se pudo completar la compra en la aplicación porque se está ejecutando una versión de prueba de la aplicación. Para completar las compras desde la aplicación, instala la versión completa de la aplicación.               |
| 0x803F6105   | No se pudo completar la compra en la aplicación porque no iniciaste sesión con tu cuenta de Microsoft.                                              |
| 0x803F6107   | Sucedido algo inesperado durante el procesamiento de la operación actual.                                             |
| 0x803F6108   | No se pudo completar la compra en la aplicación porque falta información de la licencia de la aplicación. Este error puede producirse cuando se lado carga la aplicación. Para resolver este problema, desinstala la aplicación y, a continuación, volver a instalar desde la tienda para actualizar la licencia de la aplicación.                                          |
| 0x803F6109   | No se pudo completar el suministro de complementos consumibles porque la cantidad especificada es mayor que el saldo restante.        |
| 0x803F610A   | No se admite el tipo de proveedor especificado para la cuenta de usuario de la tienda.                                            |
| 0x803F610B   | No se admite la operación de Store especificada.                                             |
| 0x803F610C   | La aplicación no admite el contrato de tarea de fondo especificado.                                             |
| 0x80040001   | La lista de producto del complemento proporcionada identificadores no es válido.                        |
| 0x80040002   | La lista de palabras clave proporcionada no es válida.                   |
| 0x80040003   | El destino de suministro no es válido.                       |

## <a name="licensing-error-codes"></a>Códigos de error de licencias

Los siguientes códigos de error están relacionados con las licencias de las operaciones de las aplicaciones o complementos.

|  Código de error  |  Descripción  |
|--------------|---------------|
| 0x803F700C   | El dispositivo está actualmente desconectado. Para usar esta aplicación mientras el dispositivo está sin conexión, abre la configuración de la tienda y alterna la configuración de **Permisos sin conexión** .            |
| 0x803F8001   | No tienes un derecho para el producto. Es posible que se puede usar una cuenta de Microsoft diferente a la que se usó para comprar el producto.           |
| 0x803F8002   | Ha expirado el derecho para el producto.           |
| 0x803F8003   | El derecho para el producto está en un estado no válido que impide que una licencia que se creen.   |
| 0x803F8009<br/>0x803F800A   | Ha expirado el período de prueba de la aplicación.   |
| 0x803F8190   |  La licencia no permite el producto que se usará en el actual país o región de tu dispositivo.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Ha alcanzado el número máximo de dispositivos que pueden usarse con juegos y aplicaciones de la tienda. Para usar este juego o la aplicación en el dispositivo actual, quita primero otro dispositivo de tu cuenta.  |
| 0x803F9000<br/>0x803F9001    |  La licencia ha caducado o está dañado. Para ayudar a resolver este error, prueba a ejecutar el [Solucionador de problemas para las aplicaciones de Windows](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) para restablecer la caché de la tienda.     |
| 0x803F9006    |  No se pudo completar la operación porque el usuario que tiene derecho a este producto no está firmado el dispositivo con su cuenta de Microsoft.            |
| 0x803F9008<br/>0x803F9009    |  El dispositivo está sin conexión. El dispositivo debe estar conectado a Internet para usar este producto.            |
| 0x803F900A    |  La suscripción ha expirado.            |


## <a name="self-install-update-error-codes"></a>Instalar automáticamente los códigos de error de actualización

Los siguientes códigos de error están relacionados con la [instalación automática de actualizaciones de paquete](../packaging/self-install-package-updates.md).

|  Código de error  |  Descripción  |
|--------------|---------------|
| 0x803F6200   | Consentimiento del usuario es necesaria para descargar la actualización de paquete.               |
| 0x803F6201   | Consentimiento del usuario es necesaria para descargar e instalar la actualización de paquete.                                                  |
| 0x803F6203   | Consentimiento del usuario es necesario para instalar la actualización de paquete.                                         |
| 0x803F6204   | Consentimiento del usuario es necesaria para descargar la actualización de paquete porque la descarga se producirá en una conexión de red de uso medido.                                             |
| 0x803F6206   | Consentimiento del usuario es necesaria para descargar e instalar la actualización de paquete porque la descarga se producirá en una conexión de red de uso medido.     |


## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener información de producto para aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener información de licencia para aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras desde la aplicación para aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumibles](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de una suscripción para tu aplicación](enable-subscription-add-ons-for-your-app.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
