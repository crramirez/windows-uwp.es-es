---
description: Las declaraciones de producto ayudan a asegurarse de que la aplicación se muestra correctamente en el Microsoft Store y que se ofrece al conjunto correcto de clientes.
title: Declaraciones de producto
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3469992a73a4e90e25ce34883319343ad6986d75
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034618"
---
# <a name="product-declarations"></a>Declaraciones de producto

La sección **declaraciones de producto** de la página de [propiedades](enter-app-properties.md) del [proceso de envío](app-submissions.md) ayuda a asegurarse de que la aplicación se muestra correctamente y se ofrece al conjunto correcto de clientes, y ayuda a comprender cómo puede usar la aplicación.

En las secciones siguientes se describen algunas de las declaraciones y lo que debe tener en cuenta a la hora de determinar si cada declaración se aplica a la aplicación. Tenga en cuenta que dos de estas declaraciones están activadas de forma predeterminada (como se describe a continuación). En función de la categoría del producto, también puede ver declaraciones adicionales. Asegúrese de revisar todas las declaraciones y asegúrese de que reflejen con precisión el envío.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Esta aplicación permite a los usuarios realizar compras, pero no usa el sistema de comercio de Microsoft Store.

Para casi todos los envíos, debe dejar esta casilla desactivada, ya que las aplicaciones que ofrecen oportunidades para comprar elementos que se pueden consumir o usar dentro de la aplicación deben usar el Microsoft Store API de compras desde la aplicación para crear y enviar los complementos. Según el [acuerdo de desarrollador de aplicaciones](/legal/windows/agreements/app-developer-agreement), las aplicaciones que se crearon y enviaron antes del 29 de junio de 2015, podían seguir ofreciendo funcionalidad de compra en la aplicación sin usar el motor de comercio de Microsoft, siempre y cuando la funcionalidad de compra cumpla con las [directivas de Microsoft Store](store-policies.md#108-financial-transactions). Si esto se aplica a tu aplicación, debes marcar esta casilla. Si no, déjala desactivada.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Esta aplicación se ha probado y cumple las directrices de accesibilidad.

Si activas esta casilla, la aplicación la podrán detectar los clientes que buscan específicamente aplicaciones accesibles en la Tienda.

Solo debes activar la casilla si has realizado las acciones siguientes:

-   Has configurado toda la información de accesibilidad para los elementos de la interfaz de usuario, como los nombres accesibles.
-   Has implementado operaciones y navegación por teclado, teniendo en cuenta el orden de tabulación, la activación del teclado, la navegación con las teclas de flecha y los métodos abreviados.
-   Te has asegurado de ofrecer una experiencia visual accesible al incluir cosas, como por ejemplo, una proporción de contraste de texto de 4.5:1, y no te bases solamente en el color para transmitirle información al usuario.
-   Has usado herramientas de prueba de accesibilidad, como Inspect o AccChecker, para comprobar tu aplicación y resolver todos los errores de prioridad alta detectados por esas herramientas.
-   Has comprobado rigurosamente los escenarios clave de la aplicación, como las facilidades y las herramientas como Narrador, Lupa, Teclado en pantalla, Contraste alto y Valores altos de PPP.

Cuando declaras que tu aplicación es accesible, aceptas que será accesible para todos los clientes, incluidas las personas con discapacidades. Esto significa, por ejemplo, que probaste la aplicación con el modo de contraste alto y con el lector de pantalla, y que comprobaste correctamente las funciones de la interfaz de usuario con un teclado, la lupa y otras herramientas de accesibilidad.

Para obtener más información, vea [accesibilidad](../design/accessibility/accessibility.md), [pruebas de accesibilidad](../design/accessibility/accessibility-testing.md)y [accesibilidad en la tienda](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> No enumere la aplicación como accesible a menos que la haya diseñado y probado específicamente para ello. Si la aplicación se declara como accesible, pero en realidad no admite la accesibilidad, probablemente recibas comentarios negativos de la comunidad.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble.

Esta casilla está activada de forma predeterminada para permitir que los clientes instalen la aplicación en un medio de almacenamiento externo o extraíble, como una tarjeta SD, o en una unidad de volumen que no sea del sistema, como una unidad externa.

Si quiere evitar que la aplicación se instale en unidades alternativas o almacenamiento extraíble, y solo permite la instalación en el disco duro interno de su dispositivo, desactive esta casilla. (Tenga en cuenta que no hay ninguna opción para restringir la instalación de forma que una aplicación *solo* pueda instalarse en medios de almacenamiento extraíbles).


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows puede incluir los datos de la aplicación en copias de seguridad automáticas en OneDrive.

Esta casilla está activada de forma predeterminada para permitir que los datos de la aplicación se incluyan cuando un cliente elige que Windows realice copias de seguridad automáticas en OneDrive.

Si quieres impedir que los datos de la aplicación se incluyan en copias de seguridad automáticas, desactiva la casilla.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Esta aplicación envía datos de Kinect a servicios externos. 

Si la aplicación usa datos de Kinect y lo envía a cualquier servicio externo, debe activar esta casilla.



 

 

 
