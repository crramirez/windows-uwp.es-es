---
Description: El informe de comentarios en el centro de partners le permite ver los problemas, sugerencias y votos a favor que han enviado los clientes de Windows 10 a través del centro de opiniones.
title: Informe de comentarios
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a827b316d40f9a9cc9c0a5bee0252d47d6af6036
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63791072"
---
# <a name="feedback-report"></a>Informe de comentarios

El **informes de comentarios** en el centro de partners permite ver los problemas, sugerencias y votos a favor que han enviado los clientes de Windows 10 a través del centro de opiniones. Puede ver estos datos en el centro de partners, o exportar los datos que se va a ver sin conexión.

> [!NOTE]
> También puedes [responder a los comentarios](respond-to-customer-feedback.md) directamente desde este informe para que los clientes sepan que lees sus comentarios.

Animar a los clientes a que envíen comentarios sobre la aplicación es una excelente manera de obtener información sobre los problemas y las características que más les importan. Cuando los clientes saben que pueden enviar comentarios directamente, es menos probable que dejen comentarios con una revisión negativa en la Tienda.

Puedes usar la API de comentarios de [Microsoft Store Services SDK](https://aka.ms/store-em-sdk) para permitir que los clientes [inicien directamente el Centro de opiniones desde la aplicación](../monetize/launch-feedback-hub-from-your-app.md). Ten en cuenta que cualquier cliente que haya descargado tu aplicación en un dispositivo con Windows 10 que admita el Centro de opiniones tiene la posibilidad de dejar comentarios sobre ella mediante la aplicación Centro de opiniones. Por este motivo, es posible que vea comentarios de clientes en este informe aunque no ha preguntado específicamente para enviar comentarios desde dentro de la aplicación.

Comentarios también pueden ser útil cuando se usa [distribución de paquete paquetes piloto](package-flights.md), puesto que la **comentarios** informe muestra el paquete que cada cliente tenía instalado en su dispositivo al que un voto.

> [!TIP]
> Un vistazo rápido a las revisiones, clasificaciones y comentarios de los usuarios a través de todas las aplicaciones en los últimos 30 días, expanda **interactuar** en el menú de navegación izquierdo y seleccione **revisiones y comentarios.** 


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **Duración**, pero puedes mostrar datos durante 30 días, 3 meses, 6 meses o 12 meses.

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página mediante las siguientes opciones.

- **Tipo de comentario**: El valor predeterminado es **todas**. Puedes seleccionar **Problema** o **Sugerencia** para mostrar solo este tipo de comentarios.
- **Tipo de dispositivo**: El valor predeterminado es **todos los dispositivos**. Puedes elegir un tipo de dispositivo específico si quieres que esta página solo muestre comentarios aportados por clientes que usen ese tipo de dispositivo.
- **Versión del paquete**: El valor predeterminado es **todos los paquetes**. Puedes seleccionar uno de los paquetes para que solo se muestren los comentarios enviados por clientes que usaban este paquete concreto en el momento de enviar los comentarios.
- **Mercado**: El valor predeterminado es **todos los mercados**. Puedes elegir uno determinado para mostrar solo los comentarios de los clientes de ese mercado.
- **Grupo**: El valor predeterminado es **todas**. Puedes optar por ver solo los comentarios enviados por [usuarios de Windows Insider](https://insider.windows.com).

> [!TIP]
> Si no ves los comentarios en la página, comprueba que los filtros no hayan excluido todos los comentarios. Por ejemplo, si filtras por un **Tipo de dispositivo** que no es compatible con la aplicación, no verás ningún comentario.


## <a name="viewing-feedback-details"></a>Ver los detalles de informes

En este informe, encontrarás los comentarios individuales que dejaron los clientes. En la parte izquierda del texto de comentarios de cada elemento, verás el número de veces que otros clientes han votado a favor de los comentarios en el Centro de opiniones. Puedes ordenar los comentarios de tres formas:

- **Upvoted** (valor predeterminado): Muestra los comentarios que ha sido upvoted por otros clientes, a partir de los comentarios que reciben los votos a favor de la mayoría.
- **Tendencias**: Muestra los comentarios que ha estado upvoted por otros clientes en los últimos siete días a partir de los comentarios que ha ido la actividad más reciente.
- **Más reciente**: Muestra todos los comentarios, empezando por el voto más recientemente.

Junto a cada comentario, verás la fecha en que se envió el comentario y el tipo de comentario. También verá mercado del cliente, el paquete que se instaló en el dispositivo que están usando cuando deja los comentarios, el tipo de ese dispositivo, y **Windows Insider** si el cliente envía los comentarios es un miembro de el programa Windows Insider.

También verás una opción aquí para [responder a los comentarios](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Comentarios sobre traducciones

De forma predeterminada, los comentarios que no se ha escrito en su idioma preferido se traducen automáticamente. Si lo prefieres, para deshabilitar la traducción de los comentarios, desactiva la casilla **Traducir comentarios** situada en la esquina superior derecha, cerca de los filtros de página.

Ten en cuenta que los comentarios los traduce un sistema de traducción automática y que la traducción resultante no siempre será precisa. Se te proporciona el texto original por si quieres compararlo con la traducción o traducirlo por otros medios.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Iniciar Centro de opiniones directamente desde la aplicación

Como se mencionó anteriormente, te recomendamos que incorpores un vínculo al Centro de opiniones directamente en tu aplicación para animar a los clientes a que envíen comentarios. Para obtener más información, consulta [Launch Feedback Hub from your app (Iniciar el Centro de opiniones desde la aplicación)](../monetize/launch-feedback-hub-from-your-app.md).
