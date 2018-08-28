---
author: jnHs
Description: Product declarations help make sure your app is displayed appropriately in the Microsoft Store and offered to the right set of customers.
title: Declaraciones de producto
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.author: wdg-dev-content
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 959e056d5edf5e1fe7a1c51a2f855c9e11512cb0
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "2888810"
---
# <a name="product-declarations"></a>Declaraciones de producto

La sección de **declaraciones del producto** de la página de [Propiedades](enter-app-properties.md) del [proceso de envío](app-submissions.md) de ayuda a asegurarse de que la aplicación se muestra correctamente y que ofrece para el conjunto adecuado de los clientes y ayuda a ellos comprender cómo pueden usar su aplicación.

En las secciones siguientes se describen algunas de las declaraciones y qué debe tener en cuenta al determinar si cada declaración se aplica a la aplicación. Tenga en cuenta que dos de estas declaraciones están activadas de forma predeterminada (tal y como se describe a continuación.) Función de la categoría de su producto, también es posible que vea declaraciones adicionales. Asegúrese de revisar todas las declaraciones y asegurarse de que reflejan con precisión su envío.

## <a name="this-app-allows-users-to-make-purchases-but-does-not-use-the-microsoft-store-commerce-system"></a>Esta aplicación permite a los usuarios realizar compras, pero no utiliza el sistema de comercio de Microsoft Store.

Para casi todas las envío, debe dejar esta casilla desactivada, desde aplicaciones que ofrecen la oportunidad de adquirir elementos o pueden ser que consumen o que usa dentro de la aplicación deben usar la adquisición de aplicación de Microsoft Store API para crear y enviar los complementos. Por el [Contrato de aplicación para desarrolladores](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), aplicaciones que se han creado y enviado antes del 29 de junio de 2015, podrían continuar ofrecer funcionalidad de compra en la aplicación sin usar el motor de comercio de Microsoft, siempre y cuando la funcionalidad de compra cumple con el [ Las directivas de almacén de Microsoft](https://docs.microsoft.com/legal/windows/agreements/store-policies#108-financial-transactions). Si esto se aplica a tu aplicación, debes marcar esta casilla. Si no, déjala desactivada.

## <a name="this-app-has-been-tested-to-meet-accessibility-guidelines"></a>Esta aplicación se ha probado y cumple las directrices de accesibilidad.

Si activas esta casilla, la aplicación la podrán detectar los clientes que buscan específicamente aplicaciones accesibles en la Tienda.

Solo debes activar la casilla si has realizado las acciones siguientes:

-   Has configurado toda la información de accesibilidad para los elementos de la interfaz de usuario, como los nombres accesibles.
-   Has implementado operaciones y navegación por teclado, teniendo en cuenta el orden de tabulación, la activación del teclado, la navegación con las teclas de flecha y los métodos abreviados.
-   Te has asegurado de ofrecer una experiencia visual accesible al incluir cosas, como por ejemplo, una proporción de contraste de texto de 4.5:1, y no te bases solamente en el color para transmitirle información al usuario.
-   Has usado herramientas de prueba de accesibilidad, como Inspect o AccChecker, para comprobar tu aplicación y resolver todos los errores de prioridad alta detectados por esas herramientas.
-   Has comprobado rigurosamente los escenarios clave de la aplicación, como las facilidades y las herramientas como Narrador, Lupa, Teclado en pantalla, Contraste alto y Valores altos de PPP.

Cuando declaras que tu aplicación es accesible, aceptas que será accesible para todos los clientes, incluidas las personas con discapacidades. Esto significa, por ejemplo, que probaste la aplicación con el modo de contraste alto y con el lector de pantalla, y que comprobaste correctamente las funciones de la interfaz de usuario con un teclado, la lupa y otras herramientas de accesibilidad.

Para obtener más información, vea [accesibilidad](../design/accessibility/accessibility.md), [pruebas de accesibilidad](../design/accessibility/accessibility-testing.md)y [accesibilidad en el almacén](../design/accessibility/accessibility-in-the-store.md).

> [!IMPORTANT]
> No lista su aplicación como accesibles a menos que específicamente se ha diseñado y probado para ese propósito. Si la aplicación se declara como accesible, pero en realidad no admite la accesibilidad, probablemente recibas comentarios negativos de la comunidad.

## <a name="customers-can-install-this-app-to-alternate-drives-or-removable-storage"></a>Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble.

Esta casilla está activada de forma predeterminada, para permitir que los clientes instalar la aplicación de almacenamiento externo o extraíble unidad de medios, como una tarjeta SD o a un volumen que no sea de sistema, como una unidad externa. (Para Windows Phone 8.1, esto se ha anteriormente indicado a través de StoreManifest.xml.)

Si desea impedir que la aplicación se instale a las unidades alternativas o almacenamiento extraíble y sólo permiten la instalación a la unidad de disco duro interno en su dispositivo, desactive esta casilla.

Tenga en cuenta que no hay ninguna opción para restringir la instalación para que una aplicación *sólo* puede instalarse en los medios de almacenamiento extraíble.


## <a name="windows-can-include-this-apps-data-in-automatic-backups-to-onedrive"></a>Windows puede incluir los datos de la aplicación en copias de seguridad automáticas en OneDrive.

Esta casilla está activada de forma predeterminada para permitir que los datos de la aplicación se incluyan cuando un cliente elige que Windows realice copias de seguridad automáticas en OneDrive. (Para Windows Phone 8.1, esto se ha anteriormente indicado a través de StoreManifest.xml.)

Si quieres impedir que los datos de la aplicación se incluyan en copias de seguridad automáticas, desactiva la casilla.


## <a name="this-app-sends-kinect-data-to-external-services"></a>Esta aplicación envía datos Kinect a los servicios externos. 

Si la aplicación usa Kinect datos y lo envía a cualquier servicio externo, debe Active esta casilla.



 

 

 




