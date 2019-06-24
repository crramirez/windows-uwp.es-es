---
description: En este artículo se describen los códigos de error comunes para las operaciones de Microsoft Store para aplicaciones y complementos, incluidas compras desde la aplicación, licencias y actualizaciones de aplicaciones de la instalación automática.
title: Códigos de error para las operaciones de Microsoft Store
ms.date: 08/24/2017
ms.topic: article
keywords: windows 10, uwp, compras desde la aplicación, IAP, complementos, códigos de error
ms.localizationpriority: medium
ms.openlocfilehash: e887e5fec2a2e04658332a25a3a6c8e23fe2550c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321717"
---
# <a name="error-codes-for-store-operations"></a>Códigos de error para las operaciones de Microsoft Store

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

En este artículo se describen los códigos de error comunes que pueden surgir mientras desarrollas o pruebas operaciones relacionadas con Microsoft Store en tu aplicación.

## <a name="in-app-purchase-error-codes"></a>Códigos de error de compra desde la aplicación

Los siguientes códigos de error están relacionados con las operaciones de compra desde la aplicación.

|  Código de error  |  Descripción  |
|--------------|---------------|
| 0x803F6100   | No se pudo completar la compra desde la aplicación porque el Rincón infantil está activo. Para completar la compra, inicia sesión en el dispositivo con tu cuenta de Microsoft y vuelve a ejecutar la aplicación.               |
| 0x803F6101   | No se pudo encontrar la aplicación especificada. Es posible que la aplicación ya no esté disponible en Microsoft Store, o es posible que hayas proporcionado el Id. de Microsoft Store incorrecto para la aplicación.     |
| 0x803F6102   | No se pudo encontrar el complemento especificado. Es posible que el complemento ya no esté disponible en Microsoft Store, o es posible que hayas proporcionado el Id. de Microsoft Store incorrecto para el complemento.                                               |
| 0x803F6103   | No se pudo encontrar el producto especificado. Es posible que el producto ya no esté disponible en Microsoft Store, o es posible que hayas proporcionado el Id. de Microsoft Store incorrecto para el producto.                                          |
| 0x803F6104   | No se pudo completar la compra desde la aplicación porque se está ejecutando una versión de prueba de la aplicación. Para completar las compras desde la aplicación, instala la versión completa de la aplicación.               |
| 0x803F6105   | No se pudo completar la compra desde la aplicación porque no has iniciado sesión con tu cuenta de Microsoft.                                              |
| 0x803F6107   | Ha ocurrido algo inesperado al procesar la operación actual.                                             |
| 0x803F6108   | No se pudo completar la compra desde la aplicación porque falta información sobre la licencia de la aplicación. Este error puede producirse cuando transfieres tu aplicación. Para resolver este problema, desinstala la aplicación y vuelve a instalarla desde Microsoft Store para actualizar la licencia de la aplicación.                                          |
| 0x803F6109   | No se pudo completar el suministro de complementos consumibles porque la cantidad especificada es mayor que el saldo restante.        |
| 0x803F610A   | No se admite el tipo de proveedor especificado para la cuenta de usuario de Microsoft Store.                                            |
| 0x803F610B   | No se admite la operación especificada de Microsoft Store.                                             |
| 0x803F610C   | La aplicación no admite el contrato especificado de la tarea de fondo.                                             |
| 0x80040001   | La lista proporcionada de id. de productos de complementos no es válida.                        |
| 0x80040002   | La lista proporcionada de palabras clave no es válida.                   |
| 0x80040003   | El destino de suministro no es válido.                       |

## <a name="licensing-error-codes"></a>Códigos de error de licencias

Los siguientes códigos de error están relacionados con las operaciones de licencias para aplicaciones o complementos.

|  Código de error  |  Descripción  |
|--------------|---------------|
| 0x803F700C   | El dispositivo está actualmente sin conexión. Para usar esta aplicación mientras el dispositivo está desconectado, abre la configuración de Microsoft Store y cambia la configuración **Permisos sin conexión**.            |
| 0x803F8001   | No tienes derecho de usar el producto. Es posible que estés usando una cuenta de Microsoft diferente de la que usaste para comprar el producto.           |
| 0x803F8002   | Tu derecho para usar el producto ha expirado.           |
| 0x803F8003   | Tu derecho para usar el producto está en un estado no válido que impide la creación de una licencia.   |
| 0x803F8009<br/>0x803F800A   | El período de prueba de la aplicación ha expirado.   |
| 0x803F8190   |  La licencia no permite que el producto se use en el país o región de tu dispositivo.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Has alcanzado el número máximo de dispositivos que pueden usarse con juegos y aplicaciones de Microsoft Store. Para usar este juego o aplicación en el dispositivo actual, primero debes eliminar otro dispositivo de la cuenta.  |
| 0x803F9000<br/>0x803F9001    |  La licencia está dañada o ha expirado. Para ayudar a resolver este error, intente ejecutar el [Solucionador de problemas para aplicaciones de Windows](https://support.microsoft.com/help/4027498/microsoft-store-fix-problems-with-apps) para restablecer la caché de Store.     |
| 0x803F9006    |  No se pudo completar la operación porque el usuario que tiene derecho a usar este producto no ha iniciado sesión en el dispositivo con su cuenta de Microsoft.            |
| 0x803F9008<br/>0x803F9009    |  El dispositivo está desconectado. El dispositivo debe estar conectado para usar este producto.            |
| 0x803F900A    |  La suscripción ha caducado.            |


## <a name="self-install-update-error-codes"></a>Códigos de error de actualizaciones de instalación automática

Los siguientes códigos de error están relacionados con las [actualizaciones de paquetes de instalación automática](../packaging/self-install-package-updates.md).

|  Código de error  |  Descripción  |
|--------------|---------------|
| 0x803F6200   | El consentimiento del usuario es necesario para descargar la actualización del paquete.               |
| 0x803F6201   | El consentimiento del usuario es necesario para descargar e instalar la actualización del paquete.                                                  |
| 0x803F6203   | El consentimiento del usuario es necesario para instalar la actualización del paquete.                                         |
| 0x803F6204   | El consentimiento del usuario es necesario para descargar la actualización del paquete porque la descarga tendrá lugar en una conexión de red de uso medido.                                             |
| 0x803F6206   | El consentimiento del usuario es necesario para descargar e instalar la actualización del paquete porque la descarga tendrá lugar en una conexión de red de uso medido.     |


## <a name="related-topics"></a>Temas relacionados

* [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md)
* [Obtener la información de producto para las aplicaciones y complementos](get-product-info-for-apps-and-add-ons.md)
* [Obtener la información de licencia para las aplicaciones y complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar la adquisición de la aplicación de las aplicaciones y complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar complemento consumibles compras](enable-consumable-add-on-purchases.md)
* [Habilitar los complementos de suscripción para la aplicación](enable-subscription-add-ons-for-your-app.md)
* [Implementar una versión de prueba de la aplicación](implement-a-trial-version-of-your-app.md)
