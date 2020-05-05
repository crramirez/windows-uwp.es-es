---
Description: Puedes especificar si la aplicación puede comprarse por volumen, y en qué condiciones, a través de Microsoft Store para Empresas y Microsoft Store para Educación en la sección Licencias organizativas de la página de envío de aplicación.
title: Opciones de licencia organizativas
ms.assetid: 1EB139B0-67E7-4F66-AAEF-491B1E52E96F
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, store para empresas, store para educación, organizativa, licencias por volumen, empresa, educación store, empresa store, compra por volumen, en masa
localizationpriority: high
ms.openlocfilehash: 880e472b9a6ed19bae85c00b4014b431e3185f61
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81545025"
---
# <a name="organizational-licensing-options"></a>Opciones de licencia organizativas


Puedes especificar si la aplicación puede comprarse por volumen, y en qué condiciones, a través de la Microsoft Store para Empresas y Microsoft Store para Educación en la sección **Licencias organizativas** de la página [Precios y disponibilidad](set-app-pricing-and-availability.md#organizational-licensing) de un envío de aplicación.

Estos valores de configuración te permiten ofrecer tu aplicación a organizaciones (empresariales y educativas) que compran e implementan licencias múltiples para sus usuarios, lo que supone una oportunidad para aumentar tu relevancia en organizaciones que usan Windows 10 en varios tipos de dispositivo, incluidos equipos, tabletas y teléfonos.

Además, deberás permitir las licencias organizativas de las [aplicaciones de línea de negocio (LOB)](distribute-lob-apps-to-enterprises.md) que publiques directamente para las empresas.

> [!NOTE]
> Las opciones son independientes para cada aplicación. Puedes cambiar tus preferencias para una aplicación en cualquier momento creando un nuevo envío. Los cambios surtirán efecto cuando el envío complete el [proceso de certificación](the-app-certification-process.md).

> [!IMPORTANT]
> Los envíos que usen la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) no estarán disponibles para Microsoft Store para Empresas ni Microsoft Store para Educación. Para que la aplicación esté disponible para compras por volumen por parte de organizaciones, debes crear y enviar tus envíos en el Centro de partners.


## <a name="allowing-your-app-to-be-offered-to-organizations"></a>Permitir que la aplicación se ofrezca a las organizaciones

De manera predeterminada, la casilla denominada **Hacer que mi aplicación esté disponible para organizaciones mediante la concesión de licencias administradas por la Tienda (en línea) y distribución** está seleccionada. Esto significa que quieres que la aplicación se incluya en los catálogos de aplicaciones que se ofrecerán a las organizaciones que realicen adquisiciones por volumen, con las licencias de aplicaciones administradas a través del sistema de licencias en línea de la Tienda.

> [!NOTE]
> Esto no garantiza que la aplicación esté disponible para todas las organizaciones.

Si prefieres no permitir que ofrezcamos tu aplicación a las organizaciones para la adquisición por volumen, desmarca esta casilla. Ten en cuenta que este cambio no tendrá lugar hasta que la aplicación complete el proceso de certificación. Si alguna organización ha adquirido licencias previamente, dichas licencias todavía son válidas y las personas que ya tienen la aplicación podrán seguir usándola.

> [!TIP]
> Si quieres publicar aplicaciones de línea de negocio (LOB) exclusivamente para una organización, puedes configurar una asociación de empresas y permitir que esta empresa agregue las aplicaciones directamente a su tienda privada. Para obtener más información, consulta [Distribuir aplicaciones de LOB a empresas](distribute-lob-apps-to-enterprises.md).


## <a name="allowing-disconnected-offline-licensing"></a>Permitir las licencias desconectadas (sin conexión)

Muchas organizaciones necesitan aplicaciones que permitan la licencia sin conexión. Por ejemplo, si tienen que implementar aplicaciones en dispositivos que rara vez o nunca se conectan a Internet. Si quieres permitir que tu aplicación esté a disposición de estos clientes, activa la casilla **Allow organization-managed (offline) licensing and distribution for organizations**.

Ten en cuenta que esta casilla está **desactivada** de manera predeterminada. Debes activarla para permitirnos ofrecer tu aplicación a organizaciones comprobadas que la instalarán mediante licencias administradas por la organización (sin conexión). Las organizaciones deben someterse a una validación adicional para instalar aplicaciones de pago en sus usuarios finales de este modo.

Las licencias sin conexión permiten a las organizaciones adquirir la aplicación por volumen e instalarla sin necesidad de que cada uno de los dispositivos se conecte al sistema de licencias de la Tienda. La organización podrá descargar el paquete de la aplicación y una licencia que permite la instalación en dispositivos (a través de sus propias herramientas de administración o de la precarga de aplicaciones en imágenes del sistema operativo) sin notificar a la Tienda el uso de una licencia en particular. Si habilitas este escenario, podrás aumentar en gran medida la flexibilidad de implementación y podrás mejorar considerablemente el atractivo de la aplicación para estos clientes.

> [!IMPORTANT]
> Las licencias sin conexión no se admiten para paquetes .xap.

 
## <a name="paid-app-support"></a>Compatibilidad con aplicaciones de pago

Actualmente, las cuentas de desarrollador ubicadas en determinados mercados pueden ofrecer aplicaciones de pago para compras por volumen a través de Microsoft Store para Empresas. 

> [!NOTE]
> En algunos mercados, el precio que se muestra para una aplicación en Microsoft Store para Empresas o Microsoft Store para Educación puede ser diferente del precio que se muestra a los clientes minoristas en Microsoft Store para la misma franja de precios. El pago de ganancias de las compras de una empresa funciona exactamente igual que para las compras de un consumidor de la aplicación. Para obtener más información, consulta [Recibir pagos](getting-paid-apps.md) y [Acuerdo para Desarrolladores de Aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Para obtener una lista de los mercados en los que Microsoft Store para Empresas y Microsoft Store para Educación están disponibles, consulta [Microsoft Store for Business and Microsoft Store for Education overview](https://docs.microsoft.com/windows/manage/windows-store-for-business-overview#supported-markets) (Introducción a Microsoft Store para Empresas y Microsoft Store para Educación).

Si tu país o región no aparece a continuación, tus aplicaciones de pago no se ofrecen actualmente en Microsoft Store para Empresas y Microsoft Store para Educación. Si este es el caso, es posible que las selecciones de licencias organizativas que hagas para tus aplicaciones de pago se apliquen más adelante, ya que podríamos admitir más mercados adicionales de cuentas de desarrollador para envíos de aplicaciones de pago en el futuro.

En este momento, los desarrolladores ubicados en los siguientes países y regiones pueden distribuir aplicaciones de pago a clientes empresariales a través de Microsoft Store para Empresas y Microsoft Store para Educación:

- Austria
- Bélgica
- Bulgaria
- Canadá
- Croacia
- Chipre
- Chequia
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
- Rumanía
- Eslovaquia
- Eslovenia
- España
- Suecia
- Suiza
- Reino Unido
- Estados Unidos
