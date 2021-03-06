---
description: Diseña páginas de ayuda externa para ofrecer instrucciones detalladas y asesoramiento sobre la aplicación.
title: Directrices para diseñar páginas de ayuda externa
label: External help
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 56afd553-c520-4a28-b63d-2e1b3c1d3606
ms.localizationpriority: medium
ms.openlocfilehash: 315b384192cb8232560bfa09cdd572fa83a3a549
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030728"
---
# <a name="external-help-pages"></a>Páginas de ayuda externa



Si la aplicación requiere ayuda detallada para el contenido complejo, considera la posibilidad de hospedar estas instrucciones en una página web.

## <a name="when-to-use-external-help-pages"></a>Cuándo deben usarse las páginas de ayuda externa

Las páginas de ayuda externa son menos cómodas para el uso general o para obtener una referencia rápida. Son adecuadas para el contenido de ayuda que es demasiado amplio como para incorporarse en la propia aplicación, así como para tutoriales e instrucciones para las funciones avanzadas de una aplicación que no las usará el público general.

Si el contenido de la ayuda es lo suficientemente específico o breve como para mostrarse en la aplicación, deberías mostrarlo allí. No dirijas a los usuarios fuera de la aplicación para obtener ayuda a menos que sea necesario.

## <a name="navigating-external-help-pages"></a>Navegar por las páginas de la ayuda externa

Cuando un usuario se dirige a una página de ayuda externa, sigue uno de los dos escenarios siguientes:
-   Se le vincula directamente a la página que corresponde al problema conocido. Esto es ayuda contextual y debe usarse siempre que sea posible.
-   Se le vincula a una página de ayuda general, con una pantalla ordenada con categorías y subcategorías entre las que puede elegir.

Proporcionar a los usuarios una manera de buscar la ayuda puede ser útil, pero no hagas que esta búsqueda sea la única manera de navegar por la ayuda. A veces a los usuarios puede resultarles difícil describir sus problemas, lo que podría dificultar la búsqueda. Los usuarios deben poder encontrar rápidamente páginas adecuadas para sus problemas sin necesidad de buscar.

## <a name="tutorials-and-detailed-walkthroughs"></a>Tutoriales y guías detalladas

Las páginas de ayuda externa son el lugar ideal para proporcionar a los usuarios tutoriales y guías, tanto de vídeo o como de texto.
-   Los tutoriales deberían centrarse en las ideas más complicadas y las funciones avanzadas. Los usuarios no deberían necesitar un tutorial para usar la aplicación.
-   Asegúrate de que estos tutoriales se muestren de forma diferente de las instrucciones de ayuda estándar. Los usuarios que buscan instrucciones avanzadas tienen más ganas de buscar que los usuarios que buscan soluciones sencillas para sus problemas.
-   Considera la posibilidad de vincular tutoriales desde un directorio y desde páginas de ayuda individuales que correspondan a cada tutorial.

## <a name="related-articles"></a>Artículos relacionados

* [Directrices para la ayuda de la aplicación](guidelines-for-app-help.md)
