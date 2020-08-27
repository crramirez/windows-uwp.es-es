---
description: Obtenga información sobre las directrices para optimizar la capacidad de visualización de las unidades de anuncios y cómo medir las métricas de visualización con el informe de rendimiento de publicidad.
title: Optimizar la visualización de las unidades de anuncios
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, directrices, visualización
ms.localizationpriority: medium
ms.openlocfilehash: 1653651c41128d359fcacddf0c0d7d408e798d07
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942795"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>Optimizar la visualización de las unidades de anuncios

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

El [Informe de rendimiento de publicidad](../publish/advertising-performance-report.md) incluye métricas de visualización para las unidades de anuncios. La visualización es una métrica importante porque el sector de publicidad está pasando a valorar las impresiones visibles en lugar de solo las impresiones. Los anunciantes tienden a apostar por las impresiones visibles porque tienen una mayor probabilidad de que los usuarios vean sus anuncios.  

En consonancia con las directrices de visualización de IAB, una impresión de banners se cuenta como visible si cumple los siguientes criterios:

* Requisito de píxel: mayor o igual que el 50% de los píxeles del anuncio estaban en el espacio visible de la aplicación.
* Requisito de tiempo: la hora a la que se cumple el requisito de píxeles es mayor o igual que una segunda, publicación de anuncio.

Una impresión de anuncio de vídeo se cuenta como visible si cumple los siguientes criterios:

* Requisito de píxel: mayor o igual que el 50% de los píxeles del anuncio estaban en la parte visible de la aplicación.
* Requisito de tiempo: el vídeo ha alcanzado el requisito de píxeles y se ha reproducido durante dos segundos de publicación de anuncios.

La visualización se calcula con la fórmula siguiente:

**Visualización = [impresiones visualizadas] * 100/[total de impresiones de AD]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>Directrices para mejorar la vista de la unidad de ad

Para optimizar las impresiones visibles para las unidades de anuncios, siga estas instrucciones.

1. **Medir el rendimiento** &nbsp; &nbsp; Use el [Informe de rendimiento de publicidad](../publish/advertising-performance-report.md) para revisar las métricas de visualización de cada una de las unidades de anuncios.
2.  **Coloque el control de ad correctamente** &nbsp; &nbsp; En general, la posición más visible para un anuncio es justo encima del pliegue (es decir, en la parte inferior de la parte visible de la página que los usuarios pueden ver sin tener que desplazarse). Los anuncios que se muestran en la parte superior de la página no son visibles. Considere la posibilidad de inspeccionar cada elemento de la página para ver cómo puede optimizar el espacio visible en la aplicación. Asegúrese de que los controles de ad no se colocan en el fondo de la aplicación.
3.  **Administrar la longitud** &nbsp; &nbsp; de la página El contenido de página más corto tiene una mayor vista. Implemente el desplazamiento infinito si las páginas de la aplicación deben ser más largas.
4.  **Corregir la posición** &nbsp; &nbsp; del anuncio Asegúrese de que sus anuncios permanecen en una posición fija incluso cuando el usuario se desplaza. Esto le ayudará a obtener un tiempo de vista más alto y aumentar la velocidad de visualización.
5.  **Establecer opacidad** &nbsp; &nbsp; Asegúrese de que la opacidad del contenedor que contiene el control de ad sea del 100%.
6.  **Implementar la carga** &nbsp; &nbsp; diferida No cargue los anuncios hasta que los usuarios se desplacen a la vista que contiene el control de anuncios. Esto garantizará que el anuncio se muestre más tiempo para que el usuario pueda verlo.
7.  **Experimentar con varios tamaños** &nbsp; &nbsp; de anuncio Seleccione algunos tamaños de AD y la vista de la medida para cada uno de ellos mediante el [Informe de rendimiento de publicidad](../publish/advertising-performance-report.md). Elija el que mejor se adapte a su caso.
8.  **Revisar el diseño** &nbsp; &nbsp; de la aplicación Realice mejoras en el diseño de la página de la aplicación para reducir el tiempo de carga de la página. Los usuarios tienden a quedarse más tiempo en las páginas que se cargan más rápido.
