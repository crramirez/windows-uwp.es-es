---
author: jnHs
Description: "Puedes proporcionar información adicional sobre la aplicación en la sección Declaraciones de la aplicación de la página Propiedades de la aplicación durante el proceso de envío."
title: "Declaraciones de la aplicación"
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ee2e54494a868fbec8c4a8bd7bbb9bdcd6cbf9ae

---

# Declaraciones de la aplicación

Puedes proporcionar información adicional sobre la aplicación en la sección **Declaraciones de la aplicación** de la página **Propiedades de la aplicación** durante el [proceso de envío](app-submissions.md). Con estas declaraciones, puedes asegurarte de que la aplicación se muestra correctamente y que se ofrece al conjunto de clientes adecuado, o puedes indicar cuáles son los clientes que pueden usar la aplicación.

En las siguientes secciones se describe cada declaración y lo que debes tener en cuenta al determinar si cada una de ellas se aplica a tu aplicación.

## Esta aplicación permite a los usuarios realizar compras, pero no usa el sistema de comercio de la Tienda Windows.

En la mayoría de las aplicaciones, esta casilla debe dejarse desmarcada, ya que las aplicaciones que ofrecen la oportunidad de realizar compras desde la aplicación suelen usar la API de compra desde la aplicación de Microsoft para crear y [enviar los IAP](iap-submissions.md). Conforme al [Acuerdo para desarrolladores de aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh694058), las aplicaciones que se crearon y enviaron antes del 29 de junio de 2015 pueden seguir ofreciendo funcionalidad de compra desde la aplicación sin usar el motor de comercio de Microsoft, siempre que dicha funcionalidad cumpla las [Directivas de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_8). Si esto se aplica a tu aplicación, debes marcar esta casilla. Si no, déjala desactivada.

## Esta aplicación se ha probado y cumple las directrices de accesibilidad.

Si activas esta casilla, la aplicación la podrán detectar los clientes que buscan específicamente aplicaciones accesibles en la Tienda.

Solo debes activar la casilla si has realizado las acciones siguientes:

-   Has configurado toda la información de accesibilidad para los elementos de la interfaz de usuario, como los nombres accesibles.
-   Has implementado operaciones y navegación por teclado, teniendo en cuenta el orden de tabulación, la activación del teclado, la navegación con las teclas de flecha y los métodos abreviados.
-   Te has asegurado de ofrecer una experiencia visual accesible al incluir cosas, como por ejemplo, una proporción de contraste de texto de 4.5:1, y no te bases solamente en el color para transmitirle información al usuario.
-   Has usado herramientas de prueba de accesibilidad, como Inspect o AccChecker, para comprobar tu aplicación y resolver todos los errores de prioridad alta detectados por esas herramientas.
-   Has comprobado rigurosamente los escenarios clave de la aplicación, como las facilidades y las herramientas como Narrador, Lupa, Teclado en pantalla, Contraste alto y Valores altos de PPP.

Cuando declaras que tu aplicación es accesible, aceptas que será accesible para todos los clientes, incluidas las personas con discapacidades. Esto significa, por ejemplo, que probaste la aplicación con el modo de contraste alto y con el lector de pantalla, y que comprobaste correctamente las funciones de la interfaz de usuario con un teclado, la lupa y otras herramientas de accesibilidad.

Para obtener más información, consulta [Accesibilidad en aplicaciones de Windows Runtime](https://msdn.microsoft.com/library/windows/apps/dn263101), [Pruebas de accesibilidad](https://msdn.microsoft.com/library/windows/apps/mt297664) y [Accesibilidad en la Tienda](https://msdn.microsoft.com/library/windows/apps/mt297663).

> 
            **Importante**  No describas tu aplicación como accesible, a menos que hayas realizado en ella ingeniería específica y la hayas probado para dicho fin. Si la aplicación se declara como accesible, pero en realidad no admite la accesibilidad, probablemente recibas comentarios negativos de la comunidad.

## Los clientes pueden instalar esta aplicación en unidades alternativas o almacenamiento extraíble.

Esta casilla está activada de forma predeterminada para permitir que los clientes instalen tu aplicación en medios de almacenamiento extraíbles, como una tarjeta SD, o en una unidad de un volumen que no sea del sistema, como una unidad externa.

Si quieres impedir que la aplicación se instale en unidades alternativas o almacenamiento extraíble, desactiva esta casilla.

Ten en cuenta que no hay ninguna opción para restringir la instalación de modo que una aplicación solo pueda instalarse en un medio de almacenamiento extraíble.

> 
            **Nota**  En Windows Phone 8.1, esta condición se indicó previamente a través de StoreManifest.xml.

## Windows puede incluir los datos de la aplicación en copias de seguridad automáticas en OneDrive.

Esta casilla está activada de forma predeterminada para permitir que los datos de la aplicación se incluyan cuando un cliente elige que Windows realice copias de seguridad automáticas en OneDrive.

Si quieres impedir que los datos de la aplicación se incluyan en copias de seguridad automáticas, desactiva la casilla.

> 
            **Nota**  En Windows Phone 8.1, esta condición se indicó previamente a través de StoreManifest.xml.

 

 

 







<!--HONumber=Jun16_HO4-->


