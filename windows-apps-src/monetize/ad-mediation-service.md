---
description: El servicio de mediación de anuncios de Microsoft te permite maximizar las funcionalidades de ingresos por anuncios y de promoción de la aplicación mostrando anuncios de múltiples redes de anuncio.
title: Servicio de mediación de anuncios de Microsoft
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anuncios, publicidad, mediación de anuncios
ms.localizationpriority: medium
ms.openlocfilehash: 13cdba15e73028e0bf0560dc373e6f06bd0e86d5
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507199"
---
# <a name="microsoft-ad-mediation-service"></a>Servicio de mediación de anuncios de Microsoft

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cuando usas el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) para [mostrar anuncios en tus aplicaciones](display-ads-in-your-app.md), también puede usar opcionalmente el servicio de mediación de anuncios de Microsoft para maximizar los ingresos por anuncios. Este artículo proporciona información general sobre el servicio de mediación de anuncios y sus objetivos.

El servicio de mediación de anuncios es una parte de la [plataforma de monetización de anuncios de Microsoft](https://developer.microsoft.com/windows/ad-monetization-platform). La plataforma se compone de las siguientes partes.

![addreferences](images/ad-mediation-service.png)

El objetivo del servicio de mediación de anuncios es maximizar los ingresos de anuncios para tus aplicaciones creando estas funcionalidades.

## <a name="diversity-of-demand-by-market-and-format"></a>Diversidad de demanda según el mercado y el formato

El servicio de mediación de anuncios se integra con una variedad de redes de anuncios a través de formatos de anuncio diferentes (banner, banner intersticial, vídeo intersticial y nativo). El servicio de mediación de anuncios funciona con cada red de anuncios para asegurarte de que pueden servir anuncios con mayor potencial para interactuar con el usuario final. Seguiremos añadiendo socios de anuncio diferentes para expandir la variedad y la calidad de la demanda.

## <a name="manage-complexity-of-ad-network-relationships"></a>Administrar la complejidad de las relaciones de red de anuncios  

El servicio de mediación de anuncios se integra con una amplia variedad de redes de anuncios para que no tengas que hacer este trabajo. Después de usar el SDK de Microsoft Advertising para mostrar anuncios en la aplicación, puede modificar la configuración de la mediación [de anuncios en el centro de Partners](../publish/in-app-ads.md#mediation-settings) para mostrar anuncios de varias redes de ad. Te beneficias de anuncios de nuevas redes de anuncios sin tener que realizar cambios en el código.

Administramos en tu nombre la relación de principio a fin con las redes de anuncios. Se ocupa por nosotros de todo, desde la integración de redes de anuncios hasta el servicio de anuncios, informes y pagos, sin ningún esfuerzo adicional del usuario.

## <a name="machine-learning-based-yield-optimization-algorithms"></a>Algoritmos de optimización del rendimiento basado en aprendizaje de máquina

El servicio de mediación de anuncios funciona para generar el mayor rendimiento para los desarrolladores. Para hacerlo, emplea algoritmos de optimización del rendimiento basado en aprendizaje de máquina. Para cada llamada de anuncio, los algoritmos determinan el mejor orden de *cascada* en que las redes de anuncios deben llamarse para maximizar los ingresos para ti. Esto se hace teniendo en cuenta varios factores, incluidos:

* El usuario que realiza la solicitud.
* La aplicación para la que se realiza la solicitud.
* El rendimiento pasado de la red de anuncios.
* El formato del anuncio y el mercado del que procede la solicitud.
* La hora de la llamada del anuncio.
* La categoría del contenido de la aplicación.
* El segmento de usuarios.
* La ubicación del usuario.

Se incluyen automáticamente nuevas redes de anuncios y se evalúan por su rendimiento mediante un presupuesto de aprendizaje. En un período corto de tiempo, encuentran su sitio en la cascada. Esto hace que las redes de anuncios sean más competitivas y ayudan al desarrollador a monetizarlo al máximo a través de aplicaciones.

Es muy recomendable usar nuestra [configuración de mediación recomendada](../publish/in-app-ads.md#mediation-settings) para maximizar los ingresos de anuncios en tus aplicaciones. Esto permite que nuestros algoritmos posibiliten el mejor rendimiento de la aplicación. Sin embargo, también tiene la libertad de elegir su propia configuración de mediación en el centro de partners para tener un mayor control sobre las redes de anuncios que sirven a los anuncios y el orden en que lo hacen.

## <a name="rich-data-and-signals"></a>Señales y datos enriquecidos

El servicio de mediación de anuncios funciona con varias redes de anuncios para mejorar la selección de destino del usuario para mostrar anuncios más relevantes a cada usuario. Esto se hace enviando señales más enriquecidas sobre el usuario y la aplicación a la red de anuncios, teniendo en mente los requisitos de privacidad.

## <a name="related-topics"></a>Temas relacionados

* [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Configuración de mediación](../publish/in-app-ads.md#mediation-settings)
* [Informe de rendimiento de publicidad](../publish/advertising-performance-report.md)
