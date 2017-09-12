---
author: jnHs
Description: "El informe de comentarios del panel del Centro de desarrollo de Windows te permite ver los problemas, sugerencias y votos a favor que los clientes de Windows 10 han enviado a través del Centro de opiniones."
title: Informe de comentarios
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: e78ab0ac31bd41c3fb7b05039b14f422e4c9071b
ms.sourcegitcommit: 5ece992c31870df4c089360ef47501bd4ce14fa9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2017
---
# <a name="feedback-report"></a>Informe de comentarios

El informe **Comentarios** del panel del Centro de desarrollo de Windows te permite ver los problemas, sugerencias y votos a favor que los clientes de Windows 10 han enviado a través del Centro de opiniones. Puedes visualizar estos datos en tu panel o exportarlos para consultarlos sin conexión.

> [!NOTE]
> También puedes [responder a los comentarios](respond-to-customer-feedback.md) directamente desde este informe para que los clientes sepan que lees sus comentarios.

Animar a los clientes a que envíen comentarios sobre la aplicación es una excelente manera de obtener información sobre los problemas y las características que más les importan. Cuando los clientes saben que pueden enviar comentarios directamente, es menos probable que dejen comentarios con una revisión negativa en la Tienda.

Puedes usar la API de comentarios de [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) para permitir que los clientes [inicien directamente el Centro de opiniones desde la aplicación](../monetize/launch-feedback-hub-from-your-app.md). Ten en cuenta que cualquier cliente que haya descargado tu aplicación en un dispositivo con Windows 10 que admita el Centro de opiniones tiene la posibilidad de dejar comentarios sobre ella mediante la aplicación Centro de opiniones. Por este motivo, es posible que veas comentarios de los clientes en este informe, aunque no hayas solicitado específicamente que te envíen comentarios desde dentro de tu aplicación.

Los comentarios también pueden ser especialmente útiles si usas la [distribución de paquetes piloto](package-flights.md), ya que el informe Comentarios te muestra el paquete específico que cada cliente tenía instalado en su dispositivo en el momento de enviar los comentarios.

> [!TIP]
> Para obtener una vista rápida de las valoraciones, clasificaciones y comentarios de los usuarios de todas las aplicaciones en los últimos 30 días, expande **Interactuar** en el menú de navegación izquierdo y selecciona **Valoraciones y comentarios**. 


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **Duración**, pero puedes mostrar datos durante 30días, 3meses, 6meses o 12meses.

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página mediante las siguientes opciones.

- **Tipo de comentarios**: la opción predeterminada es **Todos**. Puedes seleccionar **Problema** o **Sugerencia** para mostrar solo este tipo de comentarios.
- **Tipo de dispositivo**: la opción predeterminada es **Todos los dispositivos**. Puedes elegir un tipo de dispositivo específico si quieres que esta página solo muestre comentarios aportados por clientes que usen ese tipo de dispositivo.
- **Versión del paquete**: el valor predeterminado es **Todos los paquetes**. Puedes seleccionar uno de los paquetes para que solo se muestren los comentarios enviados por clientes que usaban este paquete concreto en el momento de enviar los comentarios.
- **Mercado**: el valor predeterminado es **Todos los mercados**. Puedes elegir uno determinado para mostrar solo los comentarios de los clientes de ese mercado.
- **Grupo**: el valor predeterminado es **Todos**. Puedes optar por ver solo los comentarios enviados por [usuarios de WindowsInsider](http://insider.windows.com).

> [!TIP]
> Si no ves los comentarios en la página, comprueba que los filtros no hayan excluido todos los comentarios. Por ejemplo, si filtras por un **Tipo de dispositivo** que no es compatible con la aplicación, no verás ningún comentario.


## <a name="viewing-feedback-details"></a>Ver los detalles de comentarios

En este informe, encontrarás los comentarios individuales que dejaron los clientes. En la parte izquierda del texto de comentarios de cada elemento, verás el número de veces que otros clientes han votado a favor de los comentarios en el Centro de opiniones. Puedes ordenar los comentarios de tres formas:

- **Votos a favor** (predeterminado): muestra los comentarios a favor de los cuales han votado otros clientes, empezando por los comentarios que han recibido más votos a favor.
- **Tendencias**: muestra los comentarios a favor de los cuales han votado otros clientes en los últimos siete días, empezando por los comentarios con actividad más reciente.
- **Más recientes**: muestra todos los comentarios, empezando por los comentarios que se han enviado más recientemente.

Junto a cada comentario, verás la fecha en que se envió el comentario y el tipo de comentario. También verás ver el mercado del cliente, el paquete específico de la aplicación instalado en el dispositivo en el momento de enviar los comentarios, el tipo de dispositivo y **Windows Insider**, si el cliente que envía los comentarios es miembro del Programa Windows Insider.

También verás una opción aquí para [responder a los comentarios](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Comentarios sobre traducciones

De manera predeterminada, se traducen los comentarios que no se escribieron en tu idioma preferido. Si lo prefieres, para deshabilitar la traducción de los comentarios, desactiva la casilla **Traducir comentarios** situada en la esquina superior derecha, cerca de los filtros de página.

Ten en cuenta que los comentarios los traduce un sistema de traducción automática y que la traducción resultante no siempre será precisa. Se te proporciona el texto original por si quieres compararlo con la traducción o traducirlo por otros medios.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Iniciar Centro de opiniones directamente desde la aplicación

Como se mencionó anteriormente, te recomendamos que incorpores un vínculo al Centro de opiniones directamente en tu aplicación para animar a los clientes a que envíen comentarios. Para obtener más información, consulta [Launch Feedback Hub from your app (Iniciar el Centro de opiniones desde la aplicación)](../monetize/launch-feedback-hub-from-your-app.md).
