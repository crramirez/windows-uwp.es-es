---
author: mcleanbyron
Description: "Para realizar un experimento en tu aplicación de la Plataforma universal de Windows (UWP) con pruebas A/B, debes escribir el código del experimento en tu aplicación."
title: "Programación de tu aplicación para los experimentos"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: 29a94fd14d11256ade28463c04abfec81287cf39
ms.openlocfilehash: e5de32dcc7b0694e72d9686b3b9a64de17a02277

---

# Programación de tu aplicación para los experimentos

Después de [crear un proyecto y definir variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md), estarás listo para actualizar el código de tu aplicación en la Plataforma universal de Windows (UWP) para:
* Recibir los valores de variables remotas del Centro de desarrollo de Windows.
* Usar variables remotas para configurar las experiencias de aplicación para tus usuarios.
* Registrar eventos en el Centro de desarrollo que indican cuándo han visto los usuarios el experimento y han realizado una acción deseada (también denominada *conversión*).

Las siguientes secciones describen el proceso general de obtención de las variaciones del experimento y del registro de eventos en el Centro de desarrollo. Después de programar tu aplicación para los experimentos, puedes [definir un experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md). Para ver un tutorial que muestre de principio a fin el proceso de crear y ejecutar un experimento, consulta [Creación y ejecución de tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Configuración de tu proyecto

Para empezar, instala Microsoft Store Services SDK en tu equipo de desarrollo y agrega las referencias necesarias a tu proyecto.

1. Instalación de [Microsoft Store Services SDK](http://aka.ms/store-em-sdk).
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, expande el nodo del proyecto, haz clic con el botón derecho en **Referencias** y en **Agregar referencia**.
3. En **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
4. En la lista de los SDK, selecciona la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

## Adición de código para obtener datos de variación

En el proyecto, busca el código de la característica que quieres modificar en el experimento. Agrega código que recupere los datos de una variación y usa estos datos para modificar el comportamiento de la característica que estás probando. El código específico que necesita dependerá de la aplicación, pero estos son los pasos generales. Para ver el ejemplo de código completo, consulta [Creación y ejecución de tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Declara un objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) que represente la asignación de variación actual y un objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) que usarás para registrar eventos de vista y conversión en el Centro de desarrollo.
```CSharp
private StoreServicesExperimentVariation variation;
private StoreServicesCustomEventLogger logger;
```

2. Obtén la asignación actual de variación en caché para el experimento llamando al método estático [GetCachedVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getcachedvariationasync.aspx) y pasa el [Id. de proyecto](run-app-experiments-with-a-b-testing.md#terms) para tu experimento en el método. Este método devuelve un objeto [StoreServicesExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.aspx) que proporciona acceso a la asignación de variación a través de la propiedad [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariationresult.experimentvariation.aspx).
  >**Nota**&nbsp;&nbsp;Obtienes un Id. de proyecto al [crear un proyecto en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md). El Id. de proyecto que se muestra a continuación es solo un ejemplo.

  ```CSharp
var result = await StoreServicesExperimentVariation.GetCachedVariationAsync(
      "F48AC670-4472-4387-AB7D-D65B095153FB");
variation = result.ExperimentVariation;
```

4. Comprueba la propiedad [IsStale](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.isstale.aspx) para determinar si la asignación de variación en caché debe actualizarse con una asignación de variación remota del servidor. Si necesita actualizarse, llama al método estático [GetRefreshVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.getrefreshedvariationasync.aspx) para buscar una actualización de una asignación de variación del servidor y actualiza la variación local en caché.
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != StoreServicesEngagementErrorCode.None || result.ExperimentVariation.IsStale)
{
      result = await StoreServicesExperimentVariation.GetRefreshedVariationAsync(projectId);

      // If the call succeeds, use the new result. Otherwise, use the cached value.
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
4. En tu código, usa los valores de variables para modificar el comportamiento de la característica que estás probando. Por ejemplo, el siguiente código asigna el valor *buttonText* al contenido de un botón.
```CSharp
button.Content = buttonText;
```

## Adición de código para registrar eventos de vista y de conversión en el Centro de desarrollo

A continuación, agrega el código que registra los [eventos de vista](run-app-experiments-with-a-b-testing.md#terms) y [de conversión](run-app-experiments-with-a-b-testing.md#terms) en el servicio de pruebas A/B en el Centro de desarrollo. El código debe registrar el evento de vista cuando el usuario comienza a visualizar una variación que forma parte del experimento y debe registrar el evento de conversión cuando el usuario alcanza un objetivo del experimento.

El código específico que necesita dependerá de la aplicación, pero estos son los pasos generales. Para ver el ejemplo de código completo, consulta [Creación y ejecución de tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. En el código que se ejecuta cuando el usuario comienza a visualizar una variación, inicializa el campo ```logger``` en un objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.aspx) y llama al método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx). Pasa el objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) que representa la asignación de variación actual (este objeto proporciona el contexto del evento al Centro de desarrollo) y el nombre del evento de vista (es el mismo nombre de evento de vista que introduces en tu panel del Centro de desarrollo). El siguiente ejemplo registra un evento de vista denominado **userViewedButton**.

  ```CSharp
  if (logger == null)
  {
      logger = StoreServicesCustomEventLogger.GetDefault();
  }

  logger.LogForVariation(variation, "userViewedButton");
  ```

2. En el código que se ejecuta cuando el usuario alcanza una meta de uno de los objetivos del experimento, llama al método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) de nuevo y pasa el objeto [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) y el nombre de un evento de conversión (es decir, el mismo nombre de un evento de conversión que introduces en el panel del Centro de desarrollo). El siguiente ejemplo registra un evento de conversión denominado **userClickedButton**.
```CSharp
logger.LogForVariation(variation, "userClickedButton");
```

## Pasos siguientes

Después de programar el experimento en la aplicación, estás listo para los siguientes pasos:
1. [Definir tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md). Crear un experimento que defina los eventos de vista, eventos de conversión y variaciones únicas para tu prueba A/B.
2. [Ejecutar y administrar tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md).


## Temas relacionados

* [Creación de un proyecto y definición de variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Definición de tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administración de tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
* [Creación y ejecución de tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecución de experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Sep16_HO1-->


