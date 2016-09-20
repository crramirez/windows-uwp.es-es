---
author: mcleanbyron
Description: Puedes usar el panel del Centro de desarrollo de Windows para ejecutar los experimentos para las aplicaciones para la Plataforma universal de Windows (UWP) con pruebas A/B.
title: Ejecuta experimentos para aplicaciones con pruebas A/B
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 88fd0516e3c10b657884b93377480b62c1758992

---

# Ejecuta experimentos para aplicaciones con pruebas A/B

Puedes usar el panel del Centro de desarrollo de Windows para ejecutar los experimentos para las aplicaciones para la Plataforma universal de Windows (UWP) con pruebas A/B.

En una prueba A/B, experimentas con las asignaciones de variables del programa en tus aplicaciones a través del Centro de desarrollo. Al construir la lógica de una aplicación en función de variables del programa que puedan someterse a pruebas A/B, puedes habilitar variaciones de la aplicación para segmentos aleatorios de usuarios. El objetivo de la prueba A/B es detectar una variación que probablemente obtenga mejores tasas de conversión (por ejemplo, más compras desde la aplicación).

Después de haber identificado una variación que se adapte mejor a tus objetivos empresariales, puedes terminar el experimento de inmediato y habilitar esa variación para todos los usuarios desde el panel del Centro de desarrollo, sin tener que volver a publicar la aplicación.

## Crear y ejecutar una prueba A/B

Para crear y ejecutar una prueba A/B, sigue estos pasos:

1. 
            [Define tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md). Cada experimento consta de:
  * Un *evento de visualización* que indica cuándo el usuario empieza a ver una variación que forma parte del experimento.
  * Uno o más objetivos con *eventos de conversión* que indican cuándo se ha alcanzado un objetivo.
  * Una o más *variaciones* que definen la configuración usada por el experimento.
2. 
            [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md). Usa una API en el SDK de Microsoft Store Engagement and Monetization para obtener la configuración de la variación para el experimento, usa estos datos para modificar el comportamiento de la característica que estás probando y envía los eventos de vista y conversión al Centro de desarrollo.
3. 
            [Ejecuta y administra tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md). Usa el panel para revisar los resultados y completar el experimento.

Para ver un tutorial que muestra el proceso de principio a fin, consulta [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md) (Crear y ejecutar el primer experimento con pruebas A/B).

## Requisitos

Las pruebas A/B en el Centro de desarrollo de Windows solo se admiten para aplicaciones para UWP.

Para poder ejecutar experimentos con pruebas A/B, debes configurar el equipo de desarrollo:

* Sigue [estas](../get-started/get-set-up.md) instrucciones para configurar el equipo para el desarrollo para UWP.
* Instala el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Además de la API para los experimentos, este SDK también proporciona API para otras características, como mostrar anuncios y dirigir a los clientes al Centro de opiniones para recopilar los comentarios sobre la aplicación. Para obtener más información sobre este SDK, consulta [Monetize your app with the Store Engagement and Monetization SDK](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) (Rentabilizar tu aplicación con el SDK de Store Engagement and Monetization).

## Procedimientos recomendados

Para obtener los resultados más útiles, te recomendamos que sigas estas recomendaciones al ejecutar experimentos con pruebas A/B:

* Considera ejecutar experimentos con tan solo dos variaciones con una distribución aleatoria 50/50 para las asignaciones de variaciones.
* Ejecuta experimentos durante un mínimo 2 a 4 semanas para recopilar datos suficientes que sean estadísticamente significativos y usables.

## Temas relacionados

* [Define tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
* [Escribe el código de tu aplicación para los experimentos.](code-your-experiment-in-your-app.md)
* [Administra tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
* [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


