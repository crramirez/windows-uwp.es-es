---
Description: Administre las opciones de envío, como las opciones de retención de la publicación, las notas para la certificación y mucho más.
title: Administrar las opciones de envío
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, retención de publicación, fecha de publicación, envío de envío para publicar, aprobación de funcionalidad restringida
ms.localizationpriority: medium
ms.openlocfilehash: c2548ddd35fc50f62727d986d2d934d2346eb7b5
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253797"
---
# <a name="manage-submission-options"></a>Administrar las opciones de envío

La página **Opciones de envío** del proceso de envío de la aplicación es donde puede proporcionar más información para ayudarnos a probar el producto correctamente. Este es un paso opcional, pero se recomienda para muchos envíos. También puede establecer opciones de suspensión de publicación si desea retrasar el proceso de publicación.


## <a name="publishing-hold-options"></a>Opciones de retención de publicación

De forma predeterminada, publicaremos el envío en cuanto pase la certificación (o por cualquier fecha que haya especificado en la sección  [programación](configure-precise-release-scheduling.md) de la página **precios y disponibilidad** ). Opcionalmente, puede optar por suspender la publicación de su envío hasta una fecha determinada o hasta que indique manualmente que se debe publicar. A continuación se describen las opciones de esta sección. 


### <a name="publish-your-submission-as-soon-as-it-passes-certification-or-per-dates-you-specify"></a>Publique su envío en cuanto pase la certificación (o por las fechas que especifique)

**Publique este envío tan pronto como pase la certificación (o por las fechas seleccionadas en la sección programación)** es la selección predeterminada y significa que el envío comenzará el proceso de publicación en cuanto pase la certificación, a menos que haya configurado fechas en la sección [programación](configure-precise-release-scheduling.md) de la página **precios y disponibilidad** .   

Para la mayoría de los envíos, se recomienda dejar la sección **Opciones de suspensión de publicación** establecida en esta opción. Si desea especificar determinadas fechas para que se publique el envío, use **publicar este envío en cuanto pase la certificación (o por las fechas seleccionadas en la sección programación)**. Si deja esta sección establecida en la opción predeterminada, no se publicará el envío anterior a la fecha o las fechas que estableció en la sección **programación** . Las fechas seleccionadas en la sección **programación** se usarán para determinar el momento en que el producto pasa a estar disponible para los clientes de la tienda.


### <a name="publish-your-submission-manually"></a>Publicar el envío manualmente

Si aún no desea establecer una fecha de lanzamiento y prefiere que el envío permanezca sin publicar hasta que decida iniciar el proceso de publicación de forma manual, puede elegir **no publicar este envío hasta que seleccione publicar ahora**. La elección de esta opción significa que el envío no se publicará hasta que indique que debe ser. Después de que el envío pase la certificación, puede publicarlo seleccionando **publicar ahora** en la página estado de certificación o seleccionando una fecha específica de la misma manera que se describe a continuación.


### <a name="start-publishing-your-submission-on-a-certain-date"></a>Empezar a publicar el envío en una fecha determinada

Elija **empezar a publicar este envío en** para asegurarse de que el envío no se publique hasta una fecha determinada. Con esta opción, el envío se lanzará lo antes posible en la fecha que especifiques o después de ella. La fecha debe ser al menos 24 horas después del momento actual. Junto con la fecha, también puedes especificar la hora en que el envío debe comenzar a publicarse. 

Puede cambiar esta fecha de lanzamiento después de enviar el producto, siempre y cuando no haya entrado todavía en el paso de publicación. 
 
Como se indicó anteriormente, si desea especificar determinadas fechas para que se publique el envío, use **publicar este envío en cuanto pase la certificación (o por las fechas seleccionadas en la sección programación)** y deje las **Opciones de suspensión de publicación** establecidas en la selección predeterminada. El uso de la opción **empezar a publicar este envío en** significa que el envío no iniciará el proceso de publicación hasta esa fecha, pero los retrasos durante la certificación o la publicación podrían provocar que la fecha de lanzamiento real sea posterior a la fecha seleccionada. 


## <a name="notes-for-certification"></a>Notas para certificación

A medida que envía la aplicación, tiene la opción de usar la sección **notas para la certificación** para proporcionar información adicional a los evaluadores de certificación. Esta información puede ayudarte a garantizar que tu aplicación se prueba correctamente. 

Para obtener más información, consulte [notas para la certificación](notes-for-certification.md).


## <a name="restricted-capabilities"></a>Funcionalidades restringidas

Si detectamos que los paquetes declaran cualquier [funcionalidad restringida](../packaging/app-capability-declarations.md#restricted-capabilities), deberá proporcionar información en esta sección para recibir la aprobación. Para cada capacidad, díganos por qué la aplicación debe declarar la funcionalidad y cómo se usa. Asegúrate de incluir tantos detalles como sea posible para ayudarnos a comprender por qué el producto tiene que declarar la funcionalidad. 

Durante el proceso de certificación, nuestros evaluadores revisarán la información que proporciones para determinar si se aprueba el envío para usar la funcionalidad. Ten en cuenta que esto demora el envío para completar el proceso de certificación. Si aprobamos el uso de la funcionalidad, tu aplicación continuará el resto del proceso de certificación. Por lo general, no tendrás que repetir el proceso de aprobación de funcionalidad al enviar actualizaciones realizadas en la aplicación (a menos que declares funcionalidades adicionales). 

Si no se aprueba el uso de la funcionalidad, el envío no se realizará correctamente y se proporcionarán comentarios en el informe de certificación. Después, tiene la opción de crear un nuevo envío y cargar los paquetes que no declaran la funcionalidad, o, si es aplicable, solucionar los problemas relacionados con el uso de la capacidad y solicitar la aprobación en un nuevo envío.

Tenga en cuenta que hay algunas funcionalidades restringidas que rara vez se aprobarán. Para obtener más información sobre cada capacidad restringida, consulte [declaraciones de funcionalidades](../packaging/app-capability-declarations.md#restricted-capabilities)de la aplicación.

