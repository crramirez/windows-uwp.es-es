---
author: mcleanbyron
Description: You can use the Windows Dev Center dashboard to run experiments for your Universal Windows Platform (UWP) apps with A/B testing.
title: Ejecutar experimentos de aplicaciones con pruebas A/B
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, Microsoft Store Services SDK, pruebas A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: a4d7ef0fe9297f8e3affc908a9ba74de8bb29f3a
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "4024059"
---
# <a name="run-app-experiments-with-ab-testing"></a>Ejecuta experimentos para aplicaciones con pruebas A/B

Puedes usar el panel del Centro de desarrollo de Windows para definir las variables remotas que se pueden recuperar en el tiempo de ejecución desde las aplicaciones para la Plataforma Universal de Windows (UWP) y puedes probar las variaciones de estos valores con los usuarios para identificar los valores más eficaces para manejar el comportamiento de usuario deseado. Tu aplicación puede usar variables remotas para configurar las experiencias de aplicación, como las compras desde la aplicación, el flujo de registros, subtítulos y colocación de anuncios.

El objetivo de la prueba A/B debe ser identificar una variación de los valores de variables remotos que probablemente obtenga mejores tasas de conversión (por ejemplo, más compras desde la aplicación) al mismo tiempo que se te proporciona una experiencia de aplicación más atractiva. Después de haber identificado una variación correcta, puedes terminar el experimento de inmediato y habilitar esa variación para todos los usuarios desde el panel del Centro de desarrollo, sin tener que volver a publicar la aplicación.

## <a name="create-and-run-an-ab-test"></a>Crear y ejecutar una prueba A/B

Para crear y ejecutar una prueba A/B, sigue estos pasos:

1. [Creación de un proyecto y definición de variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). Este proyecto contiene las variables y los valores de variables predeterminados para los experimentos.  
2. [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md). Usa una API en Microsoft Store Services SDK para obtener los valores de variables remotos del proyecto que creaste en el panel, usa estos datos para modificar el comportamiento de la característica que estás probando y envía los eventos de vista y conversión al Centro de desarrollo.
3. [Define tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md). Crea un experimento en el proyecto que defina las variaciones y los objetivos únicos de tu prueba A/B.
4. [Ejecuta y administra tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md). Activa tu experimento y usa el panel para revisar los resultados y completar el experimento.

Para ver un tutorial que muestra el proceso de principio a fin, consulta [Create and run your first experiment with A/B testing](create-and-run-your-first-experiment-with-a-b-testing.md) (Crear y ejecutar el primer experimento con pruebas A/B).

## <a name="requirements"></a>Requisitos

Las pruebas A/B en el Centro de desarrollo de Windows solo se admiten para aplicaciones para UWP.

Para poder ejecutar experimentos con pruebas A/B, debes configurar el equipo de desarrollo:

* Sigue [estas](../get-started/get-set-up.md) instrucciones para configurar el equipo para el desarrollo para UWP.
* [Instala Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk). Además de la API para los experimentos, este SDK también proporciona API para otras características, como mostrar anuncios y dirigir a los clientes al Centro de opiniones para recopilar los comentarios sobre la aplicación.

## <a name="best-practices"></a>Procedimientos recomendados

Para obtener los resultados más útiles, te recomendamos que sigas estas recomendaciones al ejecutar experimentos con pruebas A/B:

* Considera ejecutar experimentos con tan solo dos variaciones con una distribución aleatoria 50/50 para las asignaciones de variaciones.
* Ejecuta experimentos durante un mínimo 2 a 4 semanas para recopilar datos suficientes que sean estadísticamente significativos y usables.

<span id="terms" />

## <a name="related-terms"></a>Términos relacionados

|  Término  |  Definición  |
|--------|--------------|
| Proyecto    |   Un conjunto de variables remotas con valores predeterminados al que tu aplicación puede acceder con Microsoft Store Services SDK. Opcionalmente, un proyecto también puede contener uno o más experimentos que compartan las mismas variables remotas.  |
| Experimento    |   Conjunto de parámetros que definen una prueba A/B que tus usuarios recibirán. Los experimentos se definen en el ámbito de un proyecto y cada uno consta de: <p></p><ul><li>Un *evento de visualización* que indica cuándo el usuario empieza a ver una variación que forma parte del experimento.</li><li>Uno o más objetivos con *eventos de conversión* que indican cuándo se ha alcanzado un objetivo.</li><li>Una o más *variaciones* que definen los datos variables usados por el experimento. La variación de *control* usa los valores de variables predeterminados que se definen en el proyecto para el experimento. Además de la variación de control, los experimentos suelen tener, como mínimo, una variación adicional con los valores de variables únicos para el experimento. </li></ul>          |
| Id. de proyecto    |   Identificador único que asocia tu aplicación con un proyecto en tu cuenta del Centro de desarrollo. Debes usar este identificador para conectarte con el servicio de pruebas A/B en el código de tu aplicación para recibir los datos de variación y notificar los eventos de conversión y vista al Centro de desarrollo. Para obtener más información, consulta [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md).<p></p><p>Cada proyecto y todos los experimentos del proyecto se asocian con exactamente un identificador. Puedes usar identificadores de proyecto para ayudar a diferenciar entre distintos conjuntos de experimentos. Por ejemplo, podría tener una serie de experimentos que lanza para los evaluadores de su organización y otra serie de experimentos que lanza únicamente para los usuarios externos de su aplicación.  Una aplicación puede hacer referencia a varios identificadores de proyecto si se implementan varios experimentos.</p>         |
| Variación    |   Colección de una o más variables que estás probando en el experimento. Cada experimento debe tener al menos una variable y dos variaciones (incluido el control). Un experimento puede tener hasta cinco variaciones.           |
| Variable    |  Valor que la aplicación usa para inicializar una propiedad o algún otro valor de la aplicación. Durante un experimento, el valor de la variable cambia de variación en variación. Después de finalizar un experimento, se asigna a la variable el valor de la variación que elijas lanzar a todos los usuarios de tu aplicación. Las variables pueden tener los siguientes tipos: cadena, booleano, doble y entero.
| Evento de vista    |  Cadena arbitraria que representa una actividad cuando el usuario comienza a visualizar una variación que forma parte de tu experimento. Por lo general, este es el nombre de un evento de tu código. El código de tu aplicación enviará esta cadena de evento de vista al Centro de desarrollo cuando el usuario comience a visualizar una variación. Para obtener más información, consulta [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md).
| Evento de conversión    |  Cadena arbitraria que representa una meta de un objetivo de un experimento. Por lo general, este es el nombre de un evento de tu código. El código de tu aplicación enviará esta cadena de evento de conversión al Centro de desarrollo cuando el usuario alcanza un objetivo. Para obtener más información, consulta [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md).  

## <a name="related-topics"></a>Temas relacionados

* [Creación de un proyecto y definición de variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Programar tu aplicación para los experimentos.](code-your-experiment-in-your-app.md)
* [Definición de tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administración de tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
* [Crea y ejecuta tu primer experimento con pruebas A/B.](create-and-run-your-first-experiment-with-a-b-testing.md)
