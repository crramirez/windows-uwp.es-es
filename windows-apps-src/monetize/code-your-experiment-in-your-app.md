---
author: mcleanbyron
Description: "Después de definir el experimento en el panel del Centro de desarrollo, ya puedes escribir el código el experimento de la aplicación."
title: "Escribe el código de tu aplicación para los experimentos"
ms.assetid: 6A5063E1-28CD-4087-A4FA-FBB511E9CED5
translationtype: Human Translation
ms.sourcegitcommit: d403e78b775af0f842ba2172295a09e35015dcc8
ms.openlocfilehash: 4e6706624e71c6d448a3d457c27d11c9f6ecc156

---

# Escribe el código de tu aplicación para los experimentos

Después de [definir el experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md), ya puedes actualizar el código de la aplicación de la Plataforma universal de Windows (UWP) para obtener los datos de variación del experimento. Usa estos datos para modificar el comportamiento de la característica que estás probando y registra el evento de vista y los eventos de conversión al Centro de desarrollo.

Las siguientes secciones describen el proceso general de obtención de las variaciones del experimento y del registro de eventos al Centro de desarrollo. Para ver un tutorial que muestra de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Configurar tu proyecto

Para empezar, instala el SDK de Microsoft Store Engagement and Monetization de Microsoft Store en el equipo de desarrollo y agrega las referencias necesarias a tu proyecto.

1. Instala el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk).
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, expande el nodo del proyecto, haz clic con el botón secundario en el nodo **Referencias** y haz clic en **Agregar referencia**.
3. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
4. En la lista de los SDK, selecciona la casilla junto a **SDK de Microsoft Store Engagement** y haz clic en **Aceptar**.

## Agregar código para obtener la configuración de variación

En el proyecto, busca el código de la característica que quieres modificar en el experimento. Agrega código que recupera la configuración de una variación y usa estos datos para modificar el comportamiento de la característica que estás probando. El código específico que necesita dependerá de la aplicación, pero estos son los pasos generales. Para ver el ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con una prueba A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. Declara un objeto [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) que usarás para recuperar las variaciones del experimento y un objeto [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) que representa la asignación actual de la variación.
```CSharp
private readonly ExperimentClient experiment;
private ExperimentVariation variation;
```

2. Inicializa el objeto [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.aspx) y pasa la clave de API que recuperaste de la página **Experimentos** del panel al constructor. Para obtener más información acerca de la clave de API, consulta el tema [Definir el experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md#generate-an-api-key). La clave de API que se muestra a continuación es solo un ejemplo.
```CSharp
experiment = new ExperimentClient("F48AC670-4472-4387-AB7D-D65B095153FB");
```

3. Consigue la asignación actual de variación en caché para el experimento llamando al método [GetVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.getvariationasync.aspx). Este método devuelve un objeto [ExperimentVariationResult](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.aspx) que proporciona acceso a la asignación de variación a través de la propiedad [Variation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariationresult.variation.aspx).
```CSharp
ExperimentVariationResult result = await experiment.GetVariationAsync();
variation = result.Variation;
```

4. Comprueba la propiedad [NeedsRefresh](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.needsrefresh.aspx) para determinar si es necesario actualizar la asignación de variación en caché. Si necesita actualizarse, llama al método [RefreshVariationAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentclient.refreshvariationasync.aspx) para comprobar una actualización de una asignación de variación del servidor y actualizar la variación local en caché.
```CSharp
// Check whether the cached variation assignment needs to be refreshed.
// If so, then refresh it.
if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
{
      result = await experiment.RefreshVariationAsync();

      // If the call succeeds, use the new result. Otherwise, use the
      // cached value we retrieved earlier.
      if (result.ErrorCode == EngagementErrorCode.Success)
      {
          variation = result.Variation;
      }
}
```

5. Usa los métodos [GetBoolean](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getboolean.aspx), [GetDouble](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getdouble.aspx), [GetInteger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getinteger.aspx), o [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) del objeto [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) para obtener la configuración de la asignación de variación. En cada método, el primer parámetro es el nombre de la configuración que quieras recuperar (tal como la escribiste en el panel del Centro de desarrollo). El segundo parámetro es el valor predeterminado que el método debería devolver si no es capaz de recuperar el valor especificado del Centro de desarrollo (por ejemplo, si no hay ninguna conectividad de red) y si no hay una versión de la variación en caché.

  El siguiente ejemplo usa el método [GetString](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.getstring.aspx) para obtener una configuración llamada *buttonText* y especifica un valor predeterminado de **Botón gris**.
```CSharp
var buttonText = currentVariation.GetString("buttonText", "Grey Button");
```
4. En el código, usa los valores de configuración para modificar el comportamiento de la característica que estás probando. Por ejemplo, el siguiente código asigna el valor *buttonText* al contenido de un botón.
```CSharp
button.Content = buttonText;
```

## Agregar código para registrar eventos de vista y de conversión en el Centro de desarrollo

A continuación, agrega el código que registra los eventos de vista y de conversión al servicio de pruebas A/B del Centro de desarrollo. El código debe registrar el evento de vista cuando el usuario comienza a visualizar una variación que forma parte del experimento y debe registrar el evento de conversión cuando el usuario alcanza un objetivo del experimento.

El código específico que necesita dependerá de la aplicación, pero estos son los pasos generales. Para ver el ejemplo de código completo, consulta [Crea y ejecuta tu primer experimento con una prueba A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

1. En el código que se ejecuta cuando el usuario comienza a visualizar la variación, llama al método estático [registro](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) del objeto [StoreServicesCustomEvents](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.aspx). Pasa el nombre del evento de vista que definiste en el experimento al panel del Centro de desarrollo y el objeto [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx) que representa la asignación de variación actual (este objeto proporciona el contexto del evento al Centro de desarrollo). El siguiente ejemplo registra un evento de vista denominado **userViewedButton**.
```CSharp
StoreServicesCustomEvents.Log("userViewedButton", variation);
```
2. En el código que se ejecuta cuando el usuario alcanza un objetivo de uno de los objetivos del experimento, llama de nuevo al método [registro](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) y pasa el nombre de un evento de conversión que definiste en el experimento y el objeto [ExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.experimentvariation.aspx). El siguiente ejemplo registra un evento de conversión denominado **userClickedButton**.
```CSharp
StoreServicesCustomEvents.Log("userClickedButton", variation);
```

## Pasos siguientes

Después de definir el experimento en el panel del Centro de desarrollo y el poner el código del experimento en tu aplicación, ya puedes [ejecutar y administrar la prueba en el panel del centro de desarrollo](manage-your-experiment.md).

## Temas relacionados

  * [Define tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Administra tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
  * [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


