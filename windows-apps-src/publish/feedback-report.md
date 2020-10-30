---
description: El informe de comentarios del centro de Partners le permite ver los problemas, las sugerencias y los votos que los clientes de Windows 10 han enviado a través del centro de comentarios.
title: Informe de comentarios
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2ad8cb47a2fe8df1ebbf1d1b67659a85b682d2b4
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034798"
---
# <a name="feedback-report"></a>Informe de comentarios

> [!WARNING]
> El informe de comentarios está en desuso el 15 de abril de 2020 este informe dejará de ser compatible después del 2020 15 de abril. Los datos de este informe no se actualizarán después de esta fecha y el informe se quitará en el futuro sin previo aviso. Puede seguir viendo los comentarios recibidos de los clientes directamente en el centro de opiniones.

El **Informe de comentarios** del centro de Partners le permite ver los problemas, las sugerencias y los votos que los clientes de Windows 10 han enviado a través del centro de comentarios. Puede ver estos datos en el centro de Partners o exportar los datos para verlos sin conexión.

> [!NOTE]
> También puede [responder a los comentarios](respond-to-customer-feedback.md) directamente desde este informe para que los clientes sepan que está escuchando.

Animar a los clientes a que envíen comentarios sobre la aplicación es una excelente manera de obtener información sobre los problemas y las características que más les importan. Cuando los clientes saben que pueden enviar comentarios directamente, es posible que sea menos probable que dejen esos comentarios como una revisión negativa en la tienda.

Puedes usar la API de comentarios de [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) para permitir que los clientes [inicien directamente el Centro de opiniones desde la aplicación](../monetize/launch-feedback-hub-from-your-app.md). Ten en cuenta que cualquier cliente que haya descargado tu aplicación en un dispositivo con Windows 10 que admita el Centro de opiniones tiene la posibilidad de dejar comentarios sobre ella mediante la aplicación Centro de opiniones. Por este motivo, es posible que vea los comentarios de los clientes en este informe, incluso si no ha solicitado específicamente sus comentarios dentro de la aplicación.

Los comentarios también pueden resultar útiles a la hora de usar [paquetes piloto](package-flights.md), ya que el informe de **comentarios** muestra el paquete específico que cada cliente tenía instalado en su dispositivo cuando dejó los comentarios.

> [!TIP]
> Para ver una visión rápida de las revisiones, las clasificaciones y los comentarios de los usuarios en todas las aplicaciones en los últimos 30 días, expanda **participar** en el menú de navegación izquierdo y seleccione **revisiones y comentarios.** 


## <a name="apply-filters"></a>Aplicación de filtros

Cerca de la parte superior de la página, puede seleccionar el período de tiempo para el que desea mostrar los datos. La selección predeterminada es **vigencia** , pero puede elegir Mostrar datos durante 30 días, 3 meses, 6 meses o 12 meses.

También puede expandir **filtros** para filtrar todos los datos de esta página por las siguientes opciones.

- **Tipo de comentarios** : la opción predeterminada es **Todos** . Puedes seleccionar **Problema** o **Sugerencia** para mostrar solo este tipo de comentarios.
- **Tipo de dispositivo** : la opción predeterminada es **Todos los dispositivos** . Puede elegir un tipo de dispositivo específico si desea que esta página solo muestre los comentarios que hayan dejado los clientes que usen ese tipo de dispositivo.
- **Versión del paquete** : el valor predeterminado es **Todos los paquetes** . Puedes seleccionar uno de los paquetes para que solo se muestren los comentarios enviados por clientes que usaban este paquete concreto en el momento de enviar los comentarios.
- **Mercado** : el valor predeterminado es **Todos los mercados** . Puedes elegir uno determinado para mostrar solo los comentarios de los clientes de ese mercado.
- **Grupo** : el valor predeterminado es **Todos** . Puedes optar por ver solo los comentarios enviados por [usuarios de Windows Insider](https://insider.windows.com).

> [!TIP]
> Si no ve ningún comentario en la página, asegúrese de que los filtros no hayan excluido todos sus comentarios. Por ejemplo, si filtras por un **Tipo de dispositivo** que no es compatible con la aplicación, no verás ningún comentario.


## <a name="viewing-feedback-details"></a>Ver los detalles de informes

En este informe, verá los comentarios individuales que han dejado los clientes. A la izquierda del texto de comentarios de cada elemento, verá el número de veces que otros clientes de la central de comentarios han votado los comentarios. Puedes ordenar los comentarios de tres formas:

- **Votos a favor** (predeterminado): muestra los comentarios que otros clientes han votado a favor, empezando por los comentarios que han recibido más votos a favor.
- **Tendencias** : muestra los comentarios a favor de los cuales han votado otros clientes en los últimos siete días, empezando por los comentarios con actividad más reciente.
- **Más recientes** : muestra todos los comentarios, empezando por los comentarios que se han enviado más recientemente.

Junto a cada comentario, verás la fecha en que se envió el comentario y el tipo de comentario. También verá el mercado del cliente, el paquete específico que se instaló en el dispositivo que usaban al dejar los comentarios, el tipo de ese dispositivo y **Windows Insider** si el cliente que envía los comentarios es miembro del programa Windows Insider.

También verá una opción aquí para [responder a los comentarios](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Comentarios sobre traducciones

De forma predeterminada, los comentarios que no se escribieron en su idioma preferido se traducen automáticamente. Si lo prefiere, la traducción de comentarios se puede deshabilitar desactivando la casilla **traducir comentarios** en la parte superior derecha, cerca de los filtros de página.

Ten en cuenta que los comentarios los traduce un sistema de traducción automática y que la traducción resultante no siempre será precisa. Se te proporciona el texto original por si quieres compararlo con la traducción o traducirlo por otros medios.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Iniciar Centro de opiniones directamente desde la aplicación

Como se mencionó anteriormente, te recomendamos que incorpores un vínculo al Centro de opiniones directamente en tu aplicación para animar a los clientes a que envíen comentarios. Para obtener más información, consulta [Launch Feedback Hub from your app (Iniciar el Centro de opiniones desde la aplicación)](../monetize/launch-feedback-hub-from-your-app.md).
