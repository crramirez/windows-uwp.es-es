---
author: mcleanbyron
Description: To run an experiment in your Universal Windows Platform (UWP) app with A/B testing, you must code the experiment in your app.
title: Programar tu aplicación para los experimentos
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK, pruebas A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: b0931d712ca99b429e2aaa7dec4b855f41ce55ef
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "3662016"
---
# <a name="code-your-app-for-experimentation"></a>Programar tu aplicación para los experimentos

Después de [crear un proyecto y definir variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), estarás listo para actualizar el código de tu aplicación en la Plataforma universal de Windows (UWP) para:
* Recibir los valores de variables remotas del Centro de desarrollo de Windows.
* Usar variables remotas para configurar las experiencias de aplicación para tus usuarios.
* Registrar eventos en el Centro de desarrollo que indican cuándo han visto los usuarios el experimento y han realizado una acción deseada (también denominada *conversión*).

Para agregar este comportamiento a tu aplicación, tendrás que usar las API proporcionadas por Microsoft Store Services SDK.

Las siguientes secciones describen el proceso general de obtención de variaciones del experimento y del registro de eventos en el Centro de desarrollo. Después de programar tu aplicación para los experimentos, puedes [definir un experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md). Para ver un tutorial que muestra de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

> [!NOTE]
> Algunas de la API de experimentación de Microsoft Store Services SDK usan el [patrón asincrónico](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) para recuperar los datos del centro de desarrollo. Esto significa que parte de la ejecución de estos métodos puede tener lugar después de la invocación de los métodos, por lo que la interfaz de usuario de la aplicación puede seguir respondiendo mientras se completan las operaciones. El patrón asincrónico requiere que tu aplicación use la palabra clave **async** y el operador **await** al llamar a las API, tal como se muestra en los ejemplos de código de este artículo. Por convención, los métodos asincrónicos terminan con **Async**.

## <a name="configure-your-project"></a>Configuración de tu proyecto

Para empezar, instala Microsoft Store Services SDK en tu equipo de desarrollo y agrega las referencias necesarias a tu proyecto.

1. [Instala Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, expande el nodo del proyecto, haz clic con el botón derecho en **Referencias** y en **Agregar referencia**.
3. En **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
4. En la lista de los SDK, selecciona la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

> [!NOTE]
> Los ejemplos de código en este artículo se supone que el archivo de código tiene instrucciones **using** para los espacios de nombres **System.Threading.Tasks** y **Microsoft.Services.Store.Engagement** .

## <a name="get-variation-data-and-log-the-view-event-for-your-experiment"></a>Obtener datos de variación y registrar el evento de vista para tu experimento

En el proyecto, busca el código de la característica que quieres modificar en el experimento. Agrega código que recupera datos de una variación, usa estos datos para modificar el comportamiento de la característica que estás probando y, a continuación, registra el evento de vista para tu experimento en el servicio de pruebas A/B en el Centro de desarrollo.

El código específico que necesitas dependerá de la aplicación, pero en el ejemplo siguiente se muestra el proceso básico. Para ver un ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

[!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#ExperimentCodeSample)]

Los siguientes pasos describen las partes importantes de este proceso de forma detallada.

1. Declara un objeto [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) que represente la asignación de variación actual y un objeto [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) que usarás para registrar eventos de vista y conversión en el Centro de desarrollo.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet1)]

2. Declara una variable de cadena que se asigne al [Id. de proyecto](run-app-experiments-with-a-b-testing.md#terms) para el experimento que quieras recuperar.
    > [!NOTE]
    > Obtener un proyecto de Id. de cuándo se [crea un proyecto en el panel del centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). El Id. de proyecto que se muestra a continuación es solo un ejemplo.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet2)]

3. Obtén la asignación actual de variación en caché para el experimento llamando al método estático [GetCachedVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync) y pasa el Id. de proyecto para tu experimento en el método. Este método devuelve un objeto [StoreServicesExperimentVariationResult](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult) que proporciona acceso a la asignación de variación a través de la propiedad [ExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation).

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet3)]

4. Comprueba la propiedad [IsStale](htthttps://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale) para determinar si la asignación de variación en caché debe actualizarse con una asignación de variación remota del servidor. Si necesita actualizarse, llama al método estático [GetRefreshVariationAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync) para buscar una actualización de una asignación de variación del servidor y actualiza la variación local en caché.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet4)]

5. Utiliza los métodos [GetBoolean](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean), [GetDouble](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble), [GetInt32](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32), o [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) del objeto [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) para obtener los valores para la asignación de variación. En cada método, el primer parámetro es el nombre de la variación que quieres recuperar (es decir, el mismo nombre de una variación que colocas en el panel del Centro de desarrollo). El segundo parámetro es el valor predeterminado que el método debería devolver si no es capaz de recuperar el valor especificado del Centro de desarrollo (por ejemplo, si no hay ninguna conectividad de red) y si no se dispone de una versión de la variación en caché.

    El siguiente ejemplo usa el método [GetString](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring) para obtener una variable llamada *buttonText* y especifica un valor variable predeterminado de **Botón gris**.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet5)]

6. En tu código, usa los valores de variables para modificar el comportamiento de la característica que estás probando. Por ejemplo, el siguiente código asigna el valor *buttonText* al contenido de un botón en tu aplicación. Este ejemplo asume que ya has definido este botón en otro lugar en el proyecto.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet6)]

7. Por último, registra el [evento de vista](run-app-experiments-with-a-b-testing.md#terms) para tu experimento en el servicio de pruebas A/B en el Centro de desarrollo. Inicializa el campo ```logger``` en un objeto [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger) y llama al método [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation). Pasa el objeto [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) que representa la asignación de variación actual (este objeto proporciona el contexto del evento al Centro de desarrollo) y el nombre del evento de vista para tu experimento. Esto debe coincidir con el nombre de evento de vista que especifiques para tu experimento en el panel del Centro de desarrollo. Tu código debe registrar el evento de visualización que indica cuándo el usuario empieza a ver una variación que forma parte del experimento.

    El siguiente ejemplo muestra cómo registrar un evento de vista denominado **userViewedButton**. En este ejemplo, el objetivo del experimento es hacer que el usuario haga clic en un botón en la aplicación, por lo que el evento de vista se registra después de que la aplicación haya recuperado los datos de variación (en este caso, el texto del botón) y los haya asignado al contenido del botón.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet7)]

## <a name="log-conversion-events-to-dev-center"></a>Registrar eventos de conversión en el Centro de desarrollo

A continuación, agrega el código que registra los [eventos de conversión](run-app-experiments-with-a-b-testing.md#terms) al servicio de pruebas A/B del Centro de desarrollo. El código debe registrar un evento de conversión cuando el usuario alcanza un objetivo del experimento. El código específico que necesitas dependerá de la aplicación, pero estos son los pasos generales. Para ver un ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. En el código que se ejecuta cuando el usuario alcanza un objetivo de uno de los objetivos del experimento, llama de nuevo al método [LogForVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation) y pasa el objeto [StoreServicesExperimentVariation](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesexperimentvariation) y el nombre de un evento de conversión para tu experimento. Esto debe coincidir con uno de los nombres de eventos de conversión que especificaste para tu experimento en el panel del Centro de desarrollo.

    El siguiente ejemplo registra un evento de conversión denominado **userClickedButton** desde el controlador de eventos **Click** para un botón. En este ejemplo, el objetivo del experimento es hacer que el usuario haga clic en el botón.

    [!code-cs[ExperimentExamples](./code/StoreSDKSamples/cs/ExperimentExamples.cs#Snippet8)]

## <a name="next-steps"></a>Pasos siguientes

Después de programar el experimento en la aplicación, estás listo para los siguientes pasos:
1. [Definir tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md). Crear un experimento que defina los eventos de vista, eventos de conversión y variaciones únicas para tu prueba A/B.
2. [Ejecutar y administrar tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md).


## <a name="related-topics"></a>Temas relacionados

* [Creación de un proyecto y definición de variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definición de tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administración de tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
* [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)
