---
author: jnHs
Description: Product declarations help make sure your app is displayed appropriately in the Microsoft Store and offered to the right set of customers.
title: Declaraciones de producto
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: de519f37c5eacfa64f23d0f438701d4ae9dbc934
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7432103"
---
# <a name="product-declarations"></a>Declaraciones de producto

La sección **declaraciones de producto** de la página de [Propiedades](enter-app-properties.md) del [proceso de envío](app-submissions.md) de ayuda a asegurarse de que la aplicación se muestra correctamente y que ofrece al conjunto adecuado de los clientes y ayuda a comprender cómo pueden usar la aplicación.

Las siguientes secciones describen algunas de las declaraciones y lo que debes tener en cuenta al determinar si cada una de ellas se aplica a la aplicación. Ten en cuenta que dos de estas declaraciones se comprueban de forma predeterminada (tal y como se describe a continuación). Dependiendo de la categoría de producto, también es posible que veas declaraciones adicionales. Asegúrate de revisar todas las declaraciones y asegurarse de que reflejan con precisión el envío.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Esta aplicación permite a los usuarios realizar compras, pero no usa el sistema de comercio de Microsoft Store.

Para casi cualquier envío, debes dejar esta casilla sin marcar, dado que las aplicaciones que ofrecen oportunidades para comprar artículos que son o que se pueden consumir o utiliza dentro de la aplicación deben usar la compra de la aplicación de Microsoft Store API para crear y enviar los complementos. Por el [Acuerdo para desarrolladores](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), las aplicaciones que se crearan y enviaran antes del 29 de junio de 2015, podrían seguir ofreciendo la aplicación sin usar el motor de comercio de Microsoft, siempre que la funcionalidad de compra cumpla con los [ Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions). Si esto se aplica a tu aplicación, debes marcar esta casilla. Si no, déjala desactivada.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Esta aplicación se ha probado y cumple las directrices de accesibilidad.

Si activas esta casilla, la aplicación la podrán detectar los clientes que buscan específicamente aplicaciones accesibles en la Tienda.

Solo debes activar la casilla si has realizado las acciones siguientes:

-   Has configurado toda la información de accesibilidad para los elementos de la interfaz de usuario, como los nombres accesibles.
-   Has implementado operaciones y navegación por teclado, teniendo en cuenta el orden de tabulación, la activación del teclado, la navegación con las teclas de flecha y los métodos abreviados.
-   Te has asegurado de ofrecer una experiencia visual accesible al incluir cosas, como por ejemplo, una proporción de contraste de texto de 4.5:1, y no te bases solamente en el color para transmitirle información al usuario.
-   Has usado herramientas de prueba de accesibilidad, como Inspect o AccChecker, para comprobar tu aplicación y resolver todos los errores de prioridad alta detectados por esas herramientas.
-   Has comprobado rigurosamente los escenarios clave de la aplicación, como las facilidades y las herramientas como Narrador, Lupa, Teclado en pantalla, Contraste alto y Valores altos de PPP.

Cuando declaras que tu aplicación es accesible, aceptas que será accesible para todos los clientes, incluidas las personas con discapacidades. Esto significa, por ejemplo, que probaste la aplicación con el modo de contraste alto y con el lector de pantalla, y que comprobaste correctamente las funciones de la interfaz de usuario con un teclado, la lupa y otras herramientas de accesibilidad.

Para obtener más información, consulta la [accesibilidad](../design/accessibility/accessibility.md), [pruebas de accesibilidad](../design/accessibility/accessibility-testing.md)y la [accesibilidad en la tienda](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> No lista tu aplicación como accesible a menos que se ha diseñado y probado para ese propósito específicamente. Si la aplicación se declara como accesible, pero en realidad no admite la accesibilidad, probablemente recibas comentarios negativos de la comunidad.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble.

Esta casilla está activada de manera predeterminada, para permitir que los clientes instalen la aplicación en almacenamiento extraíble o externa multimedia como una tarjeta SD, o a un volumen del sistema que no sea la unidad como una unidad externa.

Si quieres impedir que la aplicación se instale en unidades alternativas o almacenamiento extraíble y solo permiten la instalación en la unidad de disco duro interna en su dispositivo, desactiva esta casilla. (Ten en cuenta que no hay ninguna opción para restringir la instalación para que una aplicación *solo* pueda instalarse en medios de almacenamiento extraíbles).


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows puede incluir los datos de la aplicación en copias de seguridad automáticas en OneDrive.

Esta casilla está activada de forma predeterminada para permitir que los datos de la aplicación se incluyan cuando un cliente elige que Windows realice copias de seguridad automáticas en OneDrive.

Si quieres impedir que los datos de la aplicación se incluyan en copias de seguridad automáticas, desactiva la casilla.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Esta aplicación envía datos de Kinect a los servicios externos. 

Si la aplicación usa datos de Kinect y envía a cualquier servicio externo, debes marcar esta casilla.



 

 

 




