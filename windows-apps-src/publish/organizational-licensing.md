---
author: jnHs
Description: "Puedes especificar si la aplicación puede comprarse por volumen, y en qué condiciones, a través de la Tienda Windows para empresas en la sección Licencias organizativas de la página Precios y disponibilidad de un envío de aplicación."
title: Opciones de licencias organizativas
ms.assetid: 1EB139B0-67E7-4F66-AAEF-491B1E52E96F
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0fb906dc84085d3006be7f5f25d895e1150f9265
ms.lasthandoff: 02/07/2017

---

# <a name="organizational-licensing-options"></a>Opciones de licencias organizativas


Puedes especificar si la aplicación puede comprarse por volumen, y en qué condiciones, a través de la Tienda Windows para empresas en la sección **Licencias organizativas** de la página [Precios y disponibilidad](set-app-pricing-and-availability.md#organizational-licensing) de un envío de aplicación.

Estas opciones de configuración te permiten ofrecer tu aplicación a empresas que compran e implementan licencias múltiples para sus usuarios, lo que supone una oportunidad para aumentar tu relevancia en empresas que usan Windows 10 en varios tipos de dispositivo, incluidos equipos, tabletas y teléfonos. Además, deberás permitir las licencias organizativas de las [aplicaciones de línea de negocio (LOB)](distribute-lob-apps-to-enterprises.md) que publiques directamente para las empresas.

> **Nota**  La selección de opciones se configura de manera independiente en cada aplicación. Puedes cambiar tus preferencias para una aplicación en cualquier momento creando un nuevo envío. Los cambios surtirán efecto cuando el envío complete el [proceso de certificación](the-app-certification-process.md).

## <a name="allowing-your-app-to-be-offered-to-organizations"></a>Permitir que la aplicación se ofrezca a las organizaciones

De manera predeterminada, la casilla denominada **Hacer que mi aplicación esté disponible para organizaciones mediante la concesión de licencias administradas por la Tienda (en línea) y distribución** está seleccionada. Esto significa que quieres que la aplicación se incluya en los catálogos de aplicaciones que se ofrecerán a las organizaciones que realicen adquisiciones por volumen, con las licencias de aplicaciones administradas a través del sistema de licencias en línea de la Tienda.

> **Nota**  Esto no garantiza que la aplicación esté disponible para todas las organizaciones.

Si prefieres no permitir que ofrezcamos tu aplicación a las organizaciones para la adquisición por volumen, desmarca esta casilla. Ten en cuenta que este cambio no tendrá lugar hasta que la aplicación complete el proceso de certificación. Si alguna organización ha adquirido licencias previamente, dichas licencias todavía son válidas y las personas que ya tienen la aplicación podrán seguir usándola.

> **Sugerencia**  Para publicar aplicaciones de línea de negocio (LOB) exclusivamente para una determinada organización, puedes configurar una asociación de empresas y permitir que esta organización agregue las aplicaciones directamente a su tienda privada. Para obtener más información, consulta [Distribuir aplicaciones de LOB a empresas](distribute-lob-apps-to-enterprises.md).

## <a name="allowing-disconnected-offline-licensing"></a>Permitir las licencias desconectadas (sin conexión)


Muchas organizaciones necesitan aplicaciones que permitan la licencia sin conexión. Por ejemplo, si tienen que implementar aplicaciones en dispositivos que rara vez o nunca se conectan a Internet. Si quieres permitir que tu aplicación esté a disposición de estos clientes, activa la casilla **Allow organization-managed (offline) licensing and distribution for organizations**.

> **Nota**  De forma predeterminada, esta casilla está desactivada; debes activarla para permitirnos ofrecer tu aplicación a organizaciones comprobadas que la instalarán mediante licencias administradas por la organización (sin conexión). Las organizaciones deben someterse a una validación adicional para instalar aplicaciones de pago en sus usuarios finales de este modo.

Las licencias sin conexión permiten a las organizaciones adquirir la aplicación por volumen e instalarla sin necesidad de que cada uno de los dispositivos se conecte al sistema de licencias de la Tienda. La organización podrá descargar el paquete de la aplicación y una licencia que permite la instalación en dispositivos (a través de sus propias herramientas de administración o de la precarga de aplicaciones en imágenes del sistema operativo) sin notificar a la Tienda el uso de una licencia en particular. Si habilitas este escenario, podrás aumentar en gran medida la flexibilidad de implementación y podrás mejorar considerablemente el atractivo de la aplicación para estos clientes.

> **Importante** Las licencias sin conexión no se admiten para paquetes .xap.  

 
## <a name="paid-app-support"></a>Compatibilidad con aplicaciones de pago

Actualmente, las cuentas de desarrollador en determinados mercados pueden ofrecer aplicaciones de pago para adquirir licencias por volumen a través de la Tienda Windows para empresas. 

> **Nota** En algunos mercados, el precio que se muestra para una aplicación en la Tienda Windows para empresas puede ser diferente del precio que se muestra a los clientes minoristas en la Tienda Windows para la misma franja de precios. El pago de ganancias de las compras de una empresa funciona exactamente igual que para las compras de un consumidor de la aplicación. Para obtener más información, consulta [Recibir pagos](getting-paid-apps.md) y [Acuerdo para Desarrolladores de Aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh694058).

Si tu país o región no aparecen a continuación, las aplicaciones de pago no se ofrecen actualmente en la Tienda Windows para empresas. Si es así, es posible que las selecciones de licencias organizativas que hagas para las aplicaciones de pago se apliquen más adelante, dado que la Tienda Windows para empresas cada vez admite más mercados adicionales de cuentas de desarrolladores para envíos de aplicaciones de pago.

En este momento, los desarrolladores en los siguientes países y regiones pueden distribuir aplicaciones de pago a clientes empresariales a través de la Tienda Windows para empresas:

- Austria
- Bélgica
- Bulgaria
- Canadá
- Croacia
- Chipre
- República Checa
- Dinamarca
- Estonia
- Finlandia
- Francia
- Alemania
- Grecia
- Hungría
- Irlanda
- Isla de Man
- Italia
- Letonia
- Liechtenstein
- Lituania
- Luxemburgo
- Malta
- Mónaco
- Países Bajos
- Noruega
- Polonia
- Portugal
- Rumania
- Eslovaquia
- Eslovenia
- España
- Suecia
- Suiza
- Reino Unido
- Estados Unidos

