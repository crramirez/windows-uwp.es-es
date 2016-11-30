---
author: mcleanbyron
Description: "Para realizar un experimento en tu aplicación de la Plataforma universal de Windows (UWP) con pruebas A/B, debes escribir el código del experimento en tu aplicación."
title: "Programación de tu aplicación para los experimentos"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 126fee708d82f64fd2a49b844306c53bb3d4cc86
ms.openlocfilehash: ae0ddedf09913d42a036f48d2f60d7a62769b4bb

---

# Programación de tu aplicación para los experimentos

Después de [crear un proyecto y definir variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), estarás listo para actualizar el código de tu aplicación en la Plataforma universal de Windows (UWP) para:
* Recibir los valores de variables remotas del Centro de desarrollo de Windows.
* Usar variables remotas para configurar las experiencias de aplicación para tus usuarios.
* Registrar eventos en el Centro de desarrollo que indican cuándo han visto los usuarios el experimento y han realizado una acción deseada (también denominada *conversión*).

Para agregar este comportamiento a tu aplicación, tendrás que usar las API proporcionadas por Microsoft Store Services SDK.

Las siguientes secciones describen el proceso general de obtención de variaciones del experimento y del registro de eventos en el Centro de desarrollo. Después de programar tu aplicación para los experimentos, puedes [definir un experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md). Para ver un tutorial que muestra de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

>**Nota** Algunas de las API de experimentación de Windows Store Services SDK utilizan el [patrón asincrónico](../threading-async/asynchronous-programming-universal-windows-platform-apps.md) para recuperar datos desde el Centro de desarrollo. Esto significa que parte de la ejecución de estos métodos puede tener lugar después de la invocación de los métodos, por lo que la interfaz de usuario de la aplicación puede seguir respondiendo mientras se completan las operaciones. El patrón asincrónico requiere que tu aplicación use la palabra clave **async** y el operador **await** al llamar a las API, tal como se muestra en los ejemplos de código de este artículo. Por convención, los métodos asincrónicos terminan con **Async**.

## Configuración de tu proyecto

Para empezar, instala Microsoft Store Services SDK en tu equipo de desarrollo y agrega las referencias necesarias a tu proyecto.

1. [Instala Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, expande el nodo del proyecto, haz clic con el botón derecho en **Referencias** y en **Agregar referencia**.
3. En **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
4. En la lista de los SDK, selecciona la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

>**Nota** Los ejemplos de código en este artículo asumen que tu código contiene instrucciones **using** para los espacios de nombres **System.Threading.Tasks** y **Microsoft.Services.Store.Engagement**.

## Obtener datos de variación y registrar el evento de vista para tu experimento

En el proyecto, busca el código de la característica que quieres modificar en el experimento. Agrega código que recupera datos de una variación, usa estos datos para modificar el comportamiento de la característica que estás probando y, a continuación, registra el evento de vista para tu experimento en el servicio de pruebas A/B en el Centro de desarrollo.

El código específico que necesitas dependerá de la aplicación, pero en el ejemplo siguiente se muestra el proceso básico. Para ver un ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;

// Assign this variable to the project ID for your experiment from Dev Center.
// The project ID shown below is for example purposes only.
private string projectId = "F48AC670-4472-4387-AB7D-D65B095153FB";

private async Task InitializeExperiment()
{
    // Get the current cached variation assignment for the experiment.
    var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(projectId);
    variation = result.ExperimentVariation;

    // Refresh the cached variation assignment if necessary.
    if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
    {
        result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

        if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
        {
            variation = result.ExperimentVariation;
        }
    }

    // Get the remote variable named "buttonText" and assign the value
    // to the button.
    var buttonText = variation.GetString("buttonText", "Grey Button");
    await button.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            button.Content = buttonText;
        });

    // Log the view event named "userViewedButton" to Dev Center.
    if (logger == null)
    {
        logger = StoreServicesCustomEventLogger.GetDefault();
    }

    logger.LogForVariation(variation, "userViewedButton");
}
```

Los siguientes pasos describen las partes importantes de este proceso de forma detallada.

1. Declara un objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) que represente la asignación de variación actual y un objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) que usarás para registrar eventos de vista y conversión en el Centro de desarrollo.
  ```CSharp
  private StoreServicesExperimentVariation variation;
  private StoreServicesCustomEventLogger logger;
  ```

1. Declara una variable de cadena que se asigna al [Id. de proyecto](run-app-experiments-with-a-b-testing.md#terms) para el experimento que quieras recuperar.
  >**Nota** Obtienes un Id. de proyecto al [crear un proyecto en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). El Id. de proyecto que se muestra a continuación es solo un ejemplo.

  ```CSharp
  private string projectId = "F48AC670-4472-4387-AB7D-D65B095153FB";
  ```

2. Obtén la asignación actual de variación en caché para el experimento llamando al método estático [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) y pasa el Id. de proyecto para tu experimento en el método. Este método devuelve un objeto [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) que proporciona acceso a la asignación de variación a través de la propiedad [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx).

   ```CSharp
   var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(projectId);
   variation = result.ExperimentVariation;
   ```

4. Comprueba la propiedad [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) para determinar si la asignación de variación en caché debe actualizarse con una asignación de variación remota del servidor. Si necesita actualizarse, llama al método estático [GetRefreshVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) para buscar una actualización de una asignación de variación del servidor y actualiza la variación local en caché.

  ```CSharp
  if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
  {
        result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

        if (result.ErrorCode == StoreServicesEngagementErrorCode.None)
        {
            variation = result.ExperimentVariation;
        }
  }
  ```

5. Utiliza los métodos [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getdouble.aspx), [GetInt32](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getint32.aspx), o [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) del objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) para obtener los valores para la asignación de variación. En cada método, el primer parámetro es el nombre de la variación que quieres recuperar (es decir, el mismo nombre de una variación que colocas en el panel del Centro de desarrollo). El segundo parámetro es el valor predeterminado que el método debería devolver si no es capaz de recuperar el valor especificado del Centro de desarrollo (por ejemplo, si no hay ninguna conectividad de red) y si no se dispone de una versión de la variación en caché.

  El siguiente ejemplo usa el método [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getstring.aspx) para obtener una variable llamada *buttonText* y especifica un valor variable predeterminado de **Botón gris**.

  ```CSharp
  var buttonText = variation.GetString("buttonText", "Grey Button");
  ```

4. En tu código, usa los valores de variables para modificar el comportamiento de la característica que estás probando. Por ejemplo, el siguiente código asigna el valor *buttonText* al contenido de un botón en tu aplicación. Este ejemplo asume que ya has definido este botón en otro lugar en el proyecto.

  ```CSharp
  await button.Dispatcher.RunAsync(
      CoreDispatcherPriority.Normal,
      () =>
      {
          button.Content = buttonText;
      });
  ```

5. Por último, registra el [evento de vista](run-app-experiments-with-a-b-testing.md#terms) para tu experimento en el servicio de pruebas A/B en el Centro de desarrollo. Inicializa el campo ```logger``` en un objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) y llama al método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx). Pasa el objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) que representa la asignación de variación actual (este objeto proporciona el contexto del evento al Centro de desarrollo) y el nombre del evento de vista para tu experimento. Esto debe coincidir con el nombre de evento de vista que especifiques para tu experimento en el panel del Centro de desarrollo. Tu código debe registrar el evento de visualización que indica cuándo el usuario empieza a ver una variación que forma parte del experimento.

  El siguiente ejemplo muestra cómo registrar un evento de vista denominado **userViewedButton**. En este ejemplo, el objetivo del experimento es hacer que el usuario haga clic en un botón en la aplicación, por lo que el evento de vista se registra después de que la aplicación ha recuperado los datos de variación (en este caso, el texto del botón) y los ha asignado al contenido del botón.

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

## Registrar eventos de conversión en el Centro de desarrollo

A continuación, agrega el código que registra los [eventos de conversión](run-app-experiments-with-a-b-testing.md#terms) al servicio de pruebas A/B del Centro de desarrollo. El código debe registrar un evento de conversión cuando el usuario alcanza un objetivo del experimento. El código específico que necesitas dependerá de la aplicación, pero estos son los pasos generales. Para ver un ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. En el código que se ejecuta cuando el usuario alcanza un objetivo de uno de los objetivos del experimento, llama de nuevo al método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) y pasa el objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) y el nombre de un evento de conversión para tu experimento. Esto debe coincidir con uno de los nombres de eventos de conversión que especificaste para tu experimento en el panel del Centro de desarrollo.

  El siguiente ejemplo registra un evento de conversión denominado **userClickedButton** desde el controlador de eventos **Click** para un botón. En este ejemplo, el objetivo del experimento es hacer que el usuario haga clic en el botón.

  ```CSharp
  private void button_Click(object sender, RoutedEventArgs e)
  {
      if (logger == null)
      {
          logger = StoreServicesCustomEventLogger.GetDefault();
      }

      logger.LogForVariation(variation, "userClickedButton");
  }
  ```

## Pasos siguientes

Después de programar el experimento en la aplicación, estás listo para los siguientes pasos:
1. [Definir tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md). Crear un experimento que defina los eventos de vista, eventos de conversión y variaciones únicas para tu prueba A/B.
2. [Ejecutar y administrar tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md).


## Temas relacionados

* [Creación de un proyecto y definición de variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definición de tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administración de tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
* [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Nov16_HO1-->


