---
title: Novedades para el SDK de Xbox Live - marzo de 2017
description: Novedades para el SDK de Xbox Live - marzo de 2017
ms.assetid: 03180585-6f87-4929-acfc-750bd78988a0
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: be8127e01d8eaae96a1d71f71967a653c00b0280
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595350"
---
# <a name="whats-new-for-the-xbox-live-sdk---march-2017"></a>Novedades para el SDK de Xbox Live - marzo de 2017

Consulte la [What's New - diciembre de 2016](1612-whats-new.md) artículo para lo que se agregó en la versión de diciembre de 2016.

## <a name="xbox-services-api"></a>API de servicios de Xbox

### <a name="data-platform-2017"></a>Plataforma de datos de 2017

Hemos introducido una API simplificada de estadísticas.  Tradicionalmente, debía enviar eventos correspondientes a stat reglas definidas en XDP o centro de partners y estos actualizan los valores stat en la nube.  Nos referimos a este modelo como estadísticas de 2013.

Con estadísticas de 2017, el título es ahora en el control de sus valores stat.  Basta con llamar a una API con el valor de estado más reciente y que se envía al servicio directamente sin necesidad de eventos.  Esto utiliza el nuevo `StatsManager` API y se puede leer más información en [estadísticas del Reproductor](../leaderboards-and-stats-2017/player-stats.md)

### <a name="github"></a>GitHub

Xbox Live API (XSAPI) ahora está disponible en GitHub en [ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api).  Con los archivos binarios que vienen con el XDK o como NuGet paquetes se sigue recomendando, sin embargo es Bienvenidos a usar el origen de y agradecemos las contribuciones de código de origen.  

## <a name="xbox-live-creators-program"></a>Programa de creadores de Xbox Live

El programa de creadores de Xbox Live es un programa para desarrolladores que ofrece un subconjunto de funcionalidad de Xbox Live a una audiencia más amplia de desarrollador.  Si ya está en el ID@Xbox programa, esto no tendrá ningún impacto para usted.

Puede leer más acerca del programa en [visión general del programa para desarrolladores](../developer-program-overview.md).

## <a name="documentation"></a>Documentación

Hay los siguientes artículos nuevos

| Artículo | Descripción |
|---------|-------------|
|[Configuración del servicio en directo de Xbox](../xbox-live-service-configuration.md) | Información actualizada acerca de cómo realizar la configuración del servicio para el título de Xbox Live
| [Configurar Xbox Live en Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | Información nueva sobre la instalación de Unity para los desarrolladores de programas de creadores de Xbox Live |
| [Espacios aislados de Xbox Live](../xbox-live-sandboxes.md) | Una guía simplificada para espacios aislados de Xbox Live y aislamiento de contenido |
| [Cuentas de prueba en vivo de Xbox](../xbox-live-test-accounts.md) | Información acerca de cómo probar el trabajo de las cuentas y cómo crearlos en el centro de partners |
