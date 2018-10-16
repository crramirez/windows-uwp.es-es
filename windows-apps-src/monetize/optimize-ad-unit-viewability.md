---
author: Xansky
description: Obtén información sobre formas de mejorar la visualización de tus unidades de anuncios.
title: Optimizar la visualización de tus unidades de anuncios
ms.author: mhopkins
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, directrices, visualización
ms.localizationpriority: medium
ms.openlocfilehash: f757dc3a2b2965f42df32318629fb3ec1e940f47
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4623840"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>Optimizar la visualización de tus unidades de anuncios

El [Informe de rendimiento de publicidad](../publish/advertising-performance-report.md) incluye las métricas de visualización para tus unidades de anuncios. La visualización es una métrica importante porque el sector de la publicidad avanza hacia la valoración de las impresiones visibles en lugar de simplemente las impresiones entregadas. Los anunciantes tienden a pujar por las impresiones visibles porque tienen mayor probabilidad de que los usuarios los vean.  

De acuerdo con las directrices de visualización de IAB, una impresión de anuncio de banner se cuenta como visible si cumple los siguientes criterios:

* Requisito de píxeles: un porcentaje mayor o igual al 50% de los píxeles del anuncio se encontraban en el espacio visible de la aplicación.
* Requisito de tiempo: el tiempo que se cumple el requisito de píxeles fue superior o igual a un segundo continuo tras la representación del anuncio.

Una impresión de anuncio de vídeo se cuenta como visible si cumple los criterios siguientes:

* Requisito de píxeles: un porcentaje mayor o igual al 50% de los píxeles del anuncio se encontraban en la parte visible de la aplicación.
* Requisito de tiempo: el vídeo cumplió el requisito de píxeles y se reprodujo durante dos segundos continuos tras la representación del anuncio.

La visualización se calcula mediante la fórmula siguiente:

**Visualización = [Impresiones visualizadas] * 100 / [Total de impresiones de anuncios]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>Directrices para mejorar la visualización de la unidad de anuncios

Para optimizar las impresiones visibles de tus unidades de anuncios, sigue estas directrices.

1. **Mide el rendimiento**&nbsp;&nbsp;Usa el [Informe de rendimiento de la publicidad](../publish/advertising-performance-report.md) para revisar las métricas de visualización de cada una de las unidades de anuncios.
2.  **Coloca el control del anuncio de manera correcta**&nbsp;&nbsp;En general, la posición más visible de un anuncio es justo encima del contenido visible (es decir, en la parte inferior de la parte visible de la página que los usuarios pueden ver sin tener que desplazarse). Los anuncios que se muestran en la parte superior de la página no son visibles. Piensa en la posibilidad de inspección de cada elemento de la página para ver cómo puedes optimizar el espacio visible de la aplicación. Asegúrate de que los controles de anuncios no están colocados en el segundo plano de la aplicación.
3.  **Administra la duración de la página**&nbsp;&nbsp;Un contenido más corto de la página hace que la visibilidad sea mayor. Implementa un desplazamiento infinito si las páginas de la aplicación deben ser más largas.
4.  **Corrige la posición del anuncio**&nbsp;&nbsp;Asegúrate de que tus anuncios permanecen en una posición fija, incluso cuando el usuario se desplaza. Esto te ayudará a lograr un tiempo mayor de visualización aumentará tu capacidad de visualización.
5.  **Establece la opacidad**&nbsp;&nbsp;Asegúrate de que la opacidad del contenedor que contiene el control del anuncio es del 100%.
6.  **Implementa la carga diferida**&nbsp;&nbsp;No cargues tus anuncios hasta que los usuarios se desplacen en la vista que contiene el control del anuncio. Esto garantizará que el anuncio se muestra más tiempo para que lo vea el usuario.
7.  **Experimenta con diversos tamaños de anuncio**&nbsp;&nbsp;Selecciona algunos tamaños de anuncios y mide la visualización para cada uno de ellos con el [Informe de rendimiento de la publicidad](../publish/advertising-performance-report.md). Elige el que mejor se adapte a ti.
8.  **Vuelve a visitar el diseño de la aplicación**&nbsp;&nbsp;Realizar mejoras en el diseño de la página de la aplicación para reducir el tiempo de carga de la página. Los usuarios tienden a quedarse más tiempo en las páginas que se cargan más rápido.
