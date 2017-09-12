---
author: jnHs
Description: "Para usar servicios de mapa en aplicaciones para Windows Phone 8.1 y versiones anteriores, necesitas un identificador de aplicación del servicio de mapas y un token para incluirlo en el código de la aplicación. Puedes obtener este token en el panel de información del Centro de desarrollo."
title: Usar servicios de mapa
ms.assetid: E5EE6B56-B86F-4D62-B16A-F023FE98EFAB
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 875d3efd6e9b27d7fced140f3d53a473fad1ae3c
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2017
---
# <a name="use-map-services"></a>Usar servicios de mapa


Para usar servicios de mapa en aplicaciones para Windows Phone 8.1 y versiones anteriores, necesitas un identificador de aplicación del servicio de mapas y un token para incluirlo en el código de la aplicación. Puedes obtener este token en el panel de información del Centro de desarrollo desplazándote a la página **Mapas** de la sección **Servicios** de la aplicación.

> **Nota** Para usar los servicios de mapa en aplicaciones destinadas a otros sistemas operativos, visita el [Centro de desarrollo de Mapas de Bing](http://go.microsoft.com/fwlink/p/?LinkId=614880). Consulta [Solicitar una clave de autenticación de mapas](https://msdn.microsoft.com/library/windows/apps/mt219694) para obtener más información.

Una vez que hayas [reservado el nombre de la aplicación](create-your-app-by-reserving-a-name.md), busca la sección **Servicios** en el menú de navegación izquierdo y expándela para mostrar la página **Mapas**. Al hacer clic en **Obtener token**, se generarán **ApplicationID** y **AuthenticationToken** y aparecerán en esta página.

> **Nota** No tienes que finalizar el envío de la aplicación en este momento. Después de solicitar un token y el id., la información se guardará en esta página. Puedes volver a la página en cualquier momento para ver esta información.

También tendrás que asegurarte de agregar **ApplicationID** y **AuthenticationToken** al código antes de empaquetar y enviar la aplicación. Para obtener más información, consulta [Cómo agregar un control de mapa a una página (Windows Phone 8.1)](http://go.microsoft.com/fwlink/p/?LinkId=614882).

 

 




