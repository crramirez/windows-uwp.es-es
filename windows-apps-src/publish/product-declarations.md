---
Description: Las declaraciones del producto ayudan a asegurarse de que la aplicación se muestren correctamente en la Microsoft Store y se ofrece para el conjunto correcto de los clientes.
title: Declaraciones de producto
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1e17fbd81c84ca4ce72d36dbabf9991fe8c6d75d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611790"
---
# <a name="product-declarations"></a>Declaraciones de producto

El **declaraciones de producto** sección de la [propiedades](enter-app-properties.md) página de la [proceso de envío](app-submissions.md) le ayuda a asegurarse de que la aplicación se muestren correctamente y se ofrece con el conjunto adecuado de los clientes y ayuda a ellos comprender cómo pueden usar la aplicación.

Las siguientes secciones describen algunas de las declaraciones y lo que necesita tener en cuenta al determinar si cada declaración se aplica a la aplicación. Tenga en cuenta que dos de estas declaraciones se comprueban de forma predeterminada (como se describe a continuación.) Según la categoría de su producto, también puede ver las declaraciones adicionales. Asegúrese de revisar todas las declaraciones y asegúrese de que reflejan con precisión su envío.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Esta aplicación permite a los usuarios realizar compras, pero no utiliza el sistema de comercio de Microsoft Store.

Para casi cualquier envío, debe dejar desactivada esta casilla, desde las aplicaciones que ofrecen la oportunidad de comprar los elementos que están o se pueden consumir o utilizarse dentro de la aplicación deben usar la compra de Microsoft Store en la aplicación de API para crear y enviar los complementos. Por el [acuerdo del desarrollador de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), las aplicaciones que se crearon y enviadas antes del 29 de junio de 2015, podrían continuar ofrecer funcionalidad de compra en la aplicación sin usar el motor de comercio de Microsoft, siempre que la funcionalidad de compra cumple con la [las directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions). Si esto se aplica a tu aplicación, debes marcar esta casilla. Si no, déjala desactivada.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Esta aplicación se ha probado y cumple las directrices de accesibilidad.

Si activas esta casilla, la aplicación la podrán detectar los clientes que buscan específicamente aplicaciones accesibles en la Tienda.

Solo debes activar la casilla si has realizado las acciones siguientes:

-   Has configurado toda la información de accesibilidad para los elementos de la interfaz de usuario, como los nombres accesibles.
-   Has implementado operaciones y navegación por teclado, teniendo en cuenta el orden de tabulación, la activación del teclado, la navegación con las teclas de flecha y los métodos abreviados.
-   Te has asegurado de ofrecer una experiencia visual accesible al incluir cosas, como por ejemplo, una proporción de contraste de texto de 4.5:1, y no te bases solamente en el color para transmitirle información al usuario.
-   Has usado herramientas de prueba de accesibilidad, como Inspect o AccChecker, para comprobar tu aplicación y resolver todos los errores de prioridad alta detectados por esas herramientas.
-   Has comprobado rigurosamente los escenarios clave de la aplicación, como las facilidades y las herramientas como Narrador, Lupa, Teclado en pantalla, Contraste alto y Valores altos de PPP.

Cuando declaras que tu aplicación es accesible, aceptas que será accesible para todos los clientes, incluidas las personas con discapacidades. Esto significa, por ejemplo, que probaste la aplicación con el modo de contraste alto y con el lector de pantalla, y que comprobaste correctamente las funciones de la interfaz de usuario con un teclado, la lupa y otras herramientas de accesibilidad.

Para obtener más información, consulta [Accesibilidad](../design/accessibility/accessibility.md), [Pruebas de accesibilidad](../design/accessibility/accessibility-testing.md) y [Accesibilidad en la Tienda](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> No lista en la aplicación como accesibles a menos que tiene específicamente diseñado y probado para ese propósito. Si la aplicación se declara como accesible, pero en realidad no admite la accesibilidad, probablemente recibas comentarios negativos de la comunidad.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble.

Esta casilla está activada de forma predeterminada, para permitir que los clientes que instalen la aplicación para almacenamiento extraíble o externa, unidad de medios, como una tarjeta SD o en un volumen que no son de sistema, como una unidad externa.

Si desea impedir que la aplicación se instale en unidades alternativas o almacenamiento extraíble y solo permitir la instalación en el disco duro interno en su dispositivo, desactive esta casilla. (Tenga en cuenta que no hay ninguna opción para restringir la instalación para que una aplicación pueda *sólo* se puede instalar en los medios de almacenamiento extraíbles.)


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows puede incluir los datos de la aplicación en copias de seguridad automáticas en OneDrive.

Esta casilla está activada de forma predeterminada para permitir que los datos de la aplicación se incluyan cuando un cliente elige que Windows realice copias de seguridad automáticas en OneDrive.

Si quieres impedir que los datos de la aplicación se incluyan en copias de seguridad automáticas, desactiva la casilla.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Esta aplicación envía datos de Kinect a los servicios externos. 

Si tu aplicación usa los datos de Kinect y los envía a cualquier servicio externo, debes marcar esta casilla.



 

 

 




