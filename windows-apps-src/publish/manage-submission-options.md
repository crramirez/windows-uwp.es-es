---
author: jnHs
Description: Manage submission options such as publishing hold options, notes for certification, and more.
title: Administrar las opciones de envío
ms.author: wdg-dev-content
ms.date: 05/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, suspensión de publicación, fecha de publicación, realizar un envío para publicar, aprobación de funcionalidad restringida
ms.localizationpriority: medium
ms.openlocfilehash: 147f34c40cc5d2b612dcdd92edc0c76340cf58f7
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "4500074"
---
# <a name="manage-submission-options"></a>Administrar las opciones de envío

La página **Opciones de envío** del proceso de envío de aplicación es donde puedes proporcionar más información que nos ayude a probar tu producto correctamente. Este es un paso opcional, pero se recomienda para muchos de los envíos. Opcionalmente, también puedes establecer opciones de suspensión de publicación, si quieres retrasar el proceso de publicación.


## <a name="publishing-hold-options"></a>Opciones de suspensión de publicación

De manera predeterminada, publicaremos tu envío en cuanto supere la certificación (o conforme a las fechas especificadas en la sección  [Programación](configure-precise-release-scheduling.md) de la página **Precios y disponibilidad**). Opcionalmente, puedes elegir establecer una suspensión en la publicación de tu envío hasta una fecha determinada o hasta que indiques específicamente que debe publicarse. A continuación se describen las opciones de esta sección. 


### <a name="publish-your-submission-as-soon-as-it-passes-certification-or-per-dates-you-specify"></a>Publicar tu envío en cuanto supere la certificación (o conforme a las fechas que especifiques)

**Publicar este envío tan pronto como supere la certificación (o conforme a las fechas que hayas seleccionado en la sección Programación)** es la selección predeterminada y significa que el envío comenzará el proceso de publicación en cuanto supere la certificación, a menos que tengas fechas configuradas en la sección [Programación](configure-precise-release-scheduling.md) de la página **Precios y disponibilidad**.   

Para la mayoría de los envíos, se recomienda dejar la sección **Opciones de suspensión de publicación** manteniendo esta opción. Si quieres especificar determinadas fechas para la publicación de tu envío, usa **Publicar este envío en cuanto supere la certificación (o conforme a las fechas que hayas seleccionado en la sección Programación**. Dejar esta sección en la opción predeterminada no provocará que el envío se publique antes de las fechas que hayas establecido en la sección **Programación**. Las fechas que seleccionaste en la sección **programación** se usará para determinar cuando tu producto esté disponible para los clientes en la tienda.


### <a name="publish-your-submission-manually"></a>Publicar el envío manualmente

Si no deseas establecer todavía una fecha de lanzamiento y prefieres que el envío permanezca sin publicar hasta que decidas iniciar manualmente el proceso de publicación, puedes elegir **No publicar este envío hasta que seleccione Publicar ahora**. Elegir esta opción significa que el envío no se publicará hasta que tú indiques que debe ser así. Después de la certificación de pasadas envío, puedes publicarla seleccionando **Publicar ahora** en la página de estado de certificación, o seleccionando una fecha específica de la misma manera, como se describe a continuación.


### <a name="start-publishing-your-submission-on-a-certain-date"></a>Empezar a publicar el envío en una fecha determinada

Elige **Comenzar la publicación de este envío en una fecha** para garantizar que el envío no se publique hasta una fecha determinada. Con esta opción, el envío se lanzará lo antes posible en la fecha que especifiques o después de ella. La fecha debe ser al menos 24 horas después del momento actual. Junto con la fecha, también puedes especificar la hora en la que el envío debe comenzar a publicarse. 

Puedes cambiar esta fecha de lanzamiento después de enviar tu producto, siempre no haya entrado en el paso publicar. 
 
Como se indicó anteriormente, si quieres especificar determinadas fechas para que el envío se publique, usa **Publicar este envío tan pronto como supere la certificación (o conforme a las fechas que hayas seleccionado en la sección Programación)** y dejar las **opciones de suspensión de publicación** establecidas en la selección predeterminada. El uso de la opción **Empezar a publicar este envío en una fecha** significa que el envío no comenzará el proceso de publicación hasta esa fecha, pero los retrasos durante la certificación o la publicación podrían hacer la fecha de lanzamiento real sea posterior a la fecha seleccionada. 


## <a name="notes-for-certification"></a>Notas para la certificación

Cuando envías tu aplicación, tienes la opción de usar la sección **Notas para la certificación** para proporcionar información adicional a los evaluadores de certificación. Esta información puede ayudarte a asegurarte de que tu aplicación se pruebe correctamente. 

Para obtener más información, consulta [Notas para la certificación](notes-for-certification.md).


## <a name="restricted-capabilities"></a>Funcionalidades restringidas

Si detectamos que los paquetes declaran [funcionalidades restringidas](../packaging/app-capability-declarations.md#restricted-capabilities), tendrás que proporcionar información en esta sección para recibir la aprobación. Para cada funcionalidad, indícanos por qué tu aplicación tiene que declarar la funcionalidad y cómo se utiliza. Asegúrate de incluir tantos detalles como sea posible para ayudarnos a comprender por qué el producto tiene que declarar la funcionalidad. 

Durante el proceso de certificación, nuestros evaluadores revisarán la información que proporciones para determinar si se aprueba el envío para usar la funcionalidad. Ten en cuenta que esto demora el envío para completar el proceso de certificación. Si aprobamos el uso de la funcionalidad, tu aplicación continuará el resto del proceso de certificación. Por lo general, no tendrás que repetir el proceso de aprobación de funcionalidad al enviar actualizaciones realizadas en la aplicación (a menos que declares funcionalidades adicionales). 

Si no aprobamos el uso de la funcionalidad, tu envío no pasará la certificación e incluiremos comentarios en el informe de certificación. A continuación, tienes la opción de crear un nuevo envío y cargar los paquetes que no declaren la funcionalidad o, si procede, solucionar los problemas relacionados con el uso de la funcionalidad y solicitar la autorización en un nuevo envío.

Ten en cuenta que hay algunas funcionalidades restringidas que muy rara vez se aprueban. Para obtener más información sobre cada funcionalidad restringida , consulta [Declaraciones de funcionalidades de las aplicaciones](../packaging/app-capability-declarations.md#restricted-capabilities).

