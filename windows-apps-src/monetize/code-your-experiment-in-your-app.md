---
Description: Para realizar un experimento en tu aplicación de la Plataforma universal de Windows (UWP) con pruebas A/B, debes escribir el código del experimento en tu aplicación.
title: Programar tu aplicación para los experimentos.
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP, Microsoft Store Services SDK, pruebas A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: 2e00c3f8d7f1f41d6b44743ebb663b09575fb21a
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334953"
---
# <a name="code-your-app-for-experimentation"></a>Programar tu aplicación para los experimentos.

Después de [crear un proyecto y definir variables remotas en el centro de partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), está listo para actualizar el código en la aplicación de plataforma Universal de Windows (UWP) para:
* Recibir los valores de variable remotos desde el centro de partners.
* Usar variables remotas para configurar las experiencias de aplicación para tus usuarios.
* Registro de eventos en el centro de partners que indican cuando los usuarios han visto el experimento y realiza una acción deseada (también denominado una *conversión*).

Para agregar este comportamiento a tu aplicación, tendrás que usar las API proporcionadas por Microsoft Store Services SDK.

Las secciones siguientes describen el proceso general de obtención de las variaciones del experimento y registro de eventos al centro de partners. Después del código de la aplicación para la experimentación, puede [definir un experimento en el centro de partners](define-your-experiment-in-the-dev-center-dashboard.md). Para ver un tutorial que muestre de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crear y ejecutar el primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

> [!NOTE]
> Algunas de la API en el de Microsoft Store Services SDK experimentación utilizan el [patrón asincrónico](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) para recuperar datos de centro de partners. Esto significa que parte de la ejecución de estos métodos puede tener lugar después de la invocación de los métodos, por lo que la interfaz de usuario de la aplicación puede seguir respondiendo mientras se completan las operaciones. El patrón asincrónico requiere que tu aplicación use la palabra clave **async** y el operador **await** al llamar a las API, tal como se muestra en los ejemplos de código de este artículo. Por convención, los métodos asincrónicos terminan con **Async**.

## <a name="configure-your-project"></a>Configurar tu proyecto

Para empezar, instala Microsoft Store Services SDK en tu equipo de desarrollo y agrega las referencias necesarias a tu proyecto.

1. [Instala Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, expande el nodo del proyecto, haz clic con el botón derecho en **Referencias** y en **Agregar referencia**.
3. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
4. En la lista de los SDK, selecciona la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

> [!NOTE]
> Los ejemplos de código de este artículo suponen que tu archivo de código contiene instrucciones **using** para los espacios de nombres **System.Threading.Tasks** y **Microsoft.Services.Store.Engagement**.

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>Obtener datos de variación y registrar el evento de vista para tu experimento

En el proyecto, busca el código de la característica que quieres modificar en el experimento. Agregue código que recupera datos de una variación, use estos datos para modificar el comportamiento de la característica que se está probando y, a continuación, iniciar el evento de vista para el experimento en el servicio de pruebas A/b en Centro de partners.

El código específico que necesitas dependerá de la aplicación, pero en el ejemplo siguiente se muestra el proceso básico. Para ver un ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

[!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

Los siguientes pasos describen las partes importantes de este proceso de forma detallada.

1. Declarar un [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) objeto que representa la asignación de variación actual y un [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) objeto que se va a usar para la vista del registro y conversión eventos al centro de partners.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. Declara una variable de cadena que se asigne al [Id. de proyecto](run-app-experiments-with-a-b-testing.md#terms) para el experimento que quieras recuperar.
    > [!NOTE]
    > Obtener un proyecto de Id. de cuándo se [crear un proyecto en el centro de partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). El Id. de proyecto que se muestra a continuación es solo un ejemplo.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. Obtén la asignación actual de variación en caché para el experimento llamando al método estático [GetCachedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) y pasa el Id. de proyecto para tu experimento en el método. Este método devuelve un objeto [StoreServicesExperimentVariationResult](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) que proporciona acceso a la asignación de variación a través de la propiedad [ExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation).

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. Comprueba la propiedad [IsStale](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) para determinar si la asignación de variación en caché debe actualizarse con una asignación de variación remota del servidor. Si necesita actualizarse, llama al método estático [GetRefreshVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) para buscar una actualización de una asignación de variación del servidor y actualiza la variación local en caché.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. Utiliza los métodos [GetBoolean](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean), [GetDouble](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble), [GetInt32](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32), o [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) del objeto [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) para obtener los valores para la asignación de variación. En cada método, el primer parámetro es el nombre de la variación del que se va a recuperar (Esto es el mismo nombre de una variación que escriba en el centro de partners). El segundo parámetro es el valor predeterminado que el método debe devolver si no es capaz de recuperar el valor especificado del centro de partners (por ejemplo, si no hay ninguna conectividad de red) y no está disponible una versión en caché de la variación.

    El siguiente ejemplo usa el método [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) para obtener una variable llamada *buttonText* y especifica un valor variable predeterminado de **Botón gris**.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. En tu código, usa los valores de variables para modificar el comportamiento de la característica que estás probando. Por ejemplo, el siguiente código asigna el valor *buttonText* al contenido de un botón en tu aplicación. Este ejemplo asume que ya has definido este botón en otro lugar en el proyecto.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. Por último, inicie sesión el [de eventos de vista](run-app-experiments-with-a-b-testing.md#terms) para el experimento en el servicio de pruebas A/b en Centro de partners. Inicializa el campo ```logger``` en un objeto [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) y llama al método [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation). Pase el [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) objeto que representa la asignación actual de variación (este objeto proporciona contexto sobre el evento al centro de partners) y el nombre del evento de vista para el experimento. Debe coincidir con el nombre de evento de vista que especifique para el experimento en el centro de partners. Tu código debe registrar el evento de visualización que indica cuándo el usuario empieza a ver una variación que forma parte del experimento.

    El siguiente ejemplo muestra cómo registrar un evento de vista denominado **userViewedButton**. En este ejemplo, el objetivo del experimento es hacer que el usuario haga clic en un botón en la aplicación, por lo que el evento de vista se registra después de que la aplicación haya recuperado los datos de variación (en este caso, el texto del botón) y los haya asignado al contenido del botón.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-partner-center"></a>Registro de eventos de conversión en el centro de partners

A continuación, agregue código que registra [los eventos de conversión](run-app-experiments-with-a-b-testing.md#terms) para el servicio de pruebas A/b en Centro de partners. El código debe registrar un evento de conversión cuando el usuario alcanza un objetivo del experimento. El código específico que necesitas dependerá de la aplicación, pero estos son los pasos generales. Para ver un ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. En el código que se ejecuta cuando el usuario alcanza un objetivo de uno de los objetivos del experimento, llama de nuevo al método [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) y pasa el objeto [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) y el nombre de un evento de conversión para tu experimento. Debe coincidir con uno de los nombres de eventos de conversión que especifique para el experimento en el centro de partners.

    El siguiente ejemplo registra un evento de conversión denominado **userClickedButton** desde el controlador de eventos **Click** para un botón. En este ejemplo, el objetivo del experimento es hacer que el usuario haga clic en el botón.

    [!code-csharp[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>Pasos siguientes

Después de programar el experimento en la aplicación, estás listo para los siguientes pasos:
1. [Definir el experimento en el centro de partners](define-your-experiment-in-the-dev-center-dashboard.md). Crear un experimento que defina los eventos de vista, eventos de conversión y variaciones únicas para tu prueba A/B.
2. [Ejecutar y administrar el experimento en el centro de partners](manage-your-experiment.md).


## <a name="related-topics"></a>Temas relacionados

* [Cree un proyecto y definir variables remotas en el centro de partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definir el experimento en el centro de partners](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administrar el experimento en el centro de partners](manage-your-experiment.md)
* [Cree y ejecute su primer experimento con un pruebas A/b](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecutar los experimentos de la aplicación con un pruebas A/b](run-app-experiments-with-a-b-testing.md)
