---
Description: Para realizar un experimento en tu aplicación de la Plataforma universal de Windows (UWP) con pruebas A/B, debes escribir el código del experimento en tu aplicación.
title: Programar tu aplicación para los experimentos.
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.date: 02/08/2017
ms.topic: article
keywords: SDK de Windows 10, UWP, Microsoft Store Services, pruebas A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: dbdd95ab0d4ecde5fbe5cfb8d84d2d328b4c5a24
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363668"
---
# <a name="code-your-app-for-experimentation"></a>Programar tu aplicación para los experimentos.

Después [de crear un proyecto y definir las variables remotas en el centro de Partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), está listo para actualizar el código en la aplicación plataforma universal de Windows (UWP) para:
* Recibir valores de variables remotas del centro de Partners.
* Usar variables remotas para configurar las experiencias de aplicación para tus usuarios.
* Registre eventos en el centro de partners que indiquen Cuándo los usuarios han visto el experimento y han realizado una acción deseada (también denominada *conversión*).

Para agregar este comportamiento a tu aplicación, tendrás que usar las API proporcionadas por Microsoft Store Services SDK.

En las secciones siguientes se describe el proceso general de obtención de las variaciones del experimento y registro de eventos en el centro de Partners. Después de codificar la aplicación para la experimentación, puede [definir un experimento en el centro de Partners](define-your-experiment-in-the-dev-center-dashboard.md). Para ver un tutorial que muestre de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crear y ejecutar el primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

> [!NOTE]
> Algunas de las API de experimentación del SDK de Microsoft Store Services usan el [patrón asincrónico](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) para recuperar datos del centro de Partners. Esto significa que parte de la ejecución de estos métodos puede tener lugar después de la invocación de los métodos, por lo que la interfaz de usuario de la aplicación puede seguir respondiendo mientras se completan las operaciones. El patrón asincrónico requiere que tu aplicación use la palabra clave **async** y el operador **await** al llamar a las API, tal como se muestra en los ejemplos de código de este artículo. Por convención, los métodos asincrónicos terminan con **Async**.

## <a name="configure-your-project"></a>Configurar el proyecto

Para empezar, instala Microsoft Store Services SDK en tu equipo de desarrollo y agrega las referencias necesarias a tu proyecto.

1. [Instale el SDK de Microsoft Store Services](microsoft-store-services-sdk.md#install-the-sdk).
2. Abra el proyecto en Visual Studio.
3. En el Explorador de soluciones, expande el nodo del proyecto, haz clic con el botón derecho en **Referencias** y en **Agregar referencia**.
3. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
4. En la lista de los SDK, selecciona la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

> [!NOTE]
> En los ejemplos de código de este artículo se supone que el archivo de código tiene instrucciones **using** para los espacios de nombres **System. Threading. Tasks** y **Microsoft. Services. Store. Engagement** .

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>Obtener datos de variación y registrar el evento de vista para tu experimento

En el proyecto, busca el código de la característica que quieres modificar en el experimento. Agregue código que recupere datos para una variación, use estos datos para modificar el comportamiento de la característica que está probando y, a continuación, registre el evento de vista del experimento en el servicio de pruebas a/B del centro de Partners.

El código específico que necesitas dependerá de la aplicación, pero en el ejemplo siguiente se muestra el proceso básico. Para ver un ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="ExperimentCodeSample":::

Los siguientes pasos describen las partes importantes de este proceso de forma detallada.

1. Declare un objeto [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) que represente la asignación de variación actual y un objeto [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) que usará para registrar los eventos de vista y conversión en el centro de Partners.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet1":::

2. Declara una variable de cadena que se asigne al [Id. de proyecto](run-app-experiments-with-a-b-testing.md#terms) para el experimento que quieras recuperar.
    > [!NOTE]
    > Puede obtener un identificador de proyecto al [crear un proyecto en el centro de Partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). El Id. de proyecto que se muestra a continuación es solo un ejemplo.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet2":::

3. Obtén la asignación actual de variación en caché para el experimento llamando al método estático [GetCachedVariationAsync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) y pasa el Id. de proyecto para tu experimento en el método. Este método devuelve un objeto [StoreServicesExperimentVariationResult](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) que proporciona acceso a la asignación de variación a través de la propiedad [ExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation).

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet3":::

4. Comprueba la propiedad [IsStale](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) para determinar si la asignación de variación en caché debe actualizarse con una asignación de variación remota del servidor. Si necesita actualizarse, llama al método estático [GetRefreshVariationAsync](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) para buscar una actualización de una asignación de variación del servidor y actualiza la variación local en caché.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet4":::

5. Utiliza los métodos [GetBoolean](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean), [GetDouble](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble), [GetInt32](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32), o [GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) del objeto [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) para obtener los valores para la asignación de variación. En cada método, el primer parámetro es el nombre de la variación que desea recuperar (es el mismo nombre de una variación que se especifica en el centro de Partners). El segundo parámetro es el valor predeterminado que el método debe devolver si no es capaz de recuperar el valor especificado del centro de Partners (por ejemplo, si no hay conectividad de red) y no hay disponible una versión en caché de la variación.

    El siguiente ejemplo usa el método [GetString](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) para obtener una variable llamada *buttonText* y especifica un valor variable predeterminado de **Botón gris**.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet5":::

6. En tu código, usa los valores de variables para modificar el comportamiento de la característica que estás probando. Por ejemplo, el siguiente código asigna el valor *buttonText* al contenido de un botón en tu aplicación. Este ejemplo asume que ya has definido este botón en otro lugar en el proyecto.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet6":::

7. Por último, registre el [evento de vista](run-app-experiments-with-a-b-testing.md#terms) del experimento en el servicio de prueba a/B del centro de Partners. Inicializa el campo ```logger``` en un objeto [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) y llama al método [LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation). Pase el objeto [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) que representa la asignación de variación actual (este objeto proporciona contexto sobre el evento al centro de Partners) y el nombre del evento de vista para el experimento. Debe coincidir con el nombre del evento de vista que especifique para el experimento en el centro de Partners. Tu código debe registrar el evento de visualización que indica cuándo el usuario empieza a ver una variación que forma parte del experimento.

    El siguiente ejemplo muestra cómo registrar un evento de vista denominado **userViewedButton**. En este ejemplo, el objetivo del experimento es hacer que el usuario haga clic en un botón en la aplicación, por lo que el evento de vista se registra después de que la aplicación haya recuperado los datos de variación (en este caso, el texto del botón) y los haya asignado al contenido del botón.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet7":::

## <a name="log-conversion-events-to-partner-center"></a>Registrar eventos de conversión en el centro de Partners

Después, agregue el código que registra [los eventos de conversión](run-app-experiments-with-a-b-testing.md#terms) al servicio de pruebas a/B en el centro de Partners. El código debe registrar un evento de conversión cuando el usuario alcanza un objetivo del experimento. El código específico que necesitas dependerá de la aplicación, pero estos son los pasos generales. Para ver un ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. En el código que se ejecuta cuando el usuario alcanza un objetivo de uno de los objetivos del experimento, llama de nuevo al método [LogForVariation](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) y pasa el objeto [StoreServicesExperimentVariation](/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) y el nombre de un evento de conversión para tu experimento. Debe coincidir con uno de los nombres de evento de conversión que especifique para el experimento en el centro de Partners.

    El siguiente ejemplo registra un evento de conversión denominado **userClickedButton** desde el controlador de eventos **Click** para un botón. En este ejemplo, el objetivo del experimento es hacer que el usuario haga clic en el botón.

    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentExamples.cs" id="Snippet8":::

## <a name="next-steps"></a>Pasos siguientes

Después de programar el experimento en la aplicación, estás listo para los siguientes pasos:
1. [Defina el experimento en el centro de Partners](define-your-experiment-in-the-dev-center-dashboard.md). Crear un experimento que defina los eventos de vista, eventos de conversión y variaciones únicas para tu prueba A/B.
2. [Ejecute y administre el experimento en el centro de Partners](manage-your-experiment.md).


## <a name="related-topics"></a>Temas relacionados

* [Crear un proyecto y definir variables remotas en el centro de Partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definir el experimento en el Centro de partners](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administrar el experimento en el Centro de partners](manage-your-experiment.md)
* [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)
