---
author: mcleanbyron
Description: Después de definir el experimento en el panel del Centro de desarrollo y codificar el experimento en la aplicación, estás listo para activar el experimento y usar el panel del Centro de desarrollo para revisar los resultados del experimento.
title: Administra tu experimento en el panel del Centro de desarrollo
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
---

# Administra tu experimento en el panel del Centro de desarrollo

Después de [definir el experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md) y [codificar la aplicación para el experimento](code-your-experiment-in-your-app.md), estás listo para activar el experimento y usar el panel del Centro de desarrollo para revisar los resultados del experimento. Después de haber obtenido todos los datos que necesitas, puedes terminar el experimento y elegir si quieres seguir usando la configuración del control de variación en todas las aplicaciones o cambiar a la configuración de una de sus variaciones.

> **Nota** Cuando activas un experimento, el Centro de desarrollo inicia inmediatamente la recopilación de datos de las aplicaciones que están pensadas para registrar los datos del experimento. Sin embargo, pueden pasar varias horas antes de que los datos del experimento aparezcan en el panel.

Para ver un tutorial que muestra de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Activar el experimento

Cuando estés satisfecho con los parámetros del experimento en el panel y hayas actualizado el código de aplicación, estarás listo para activar el experimento para que puedas iniciar la recopilación de datos del experimento a partir de la aplicación. Cuando el experimento esté activo, tu aplicación puede recuperar la configuración de la variación y notificar los eventos de vista y conversión al Centro de desarrollo.

1. Inicia sesión en el [panel del Centro de desarrollo](https://dev.windows.com/overview).
2. En **Tus aplicaciones**, selecciona la aplicación con el experimento que quieras activar.
3. En el panel de navegación, selecciona **Servicios** y, a continuación, selecciona **Experimentación**.
4. La sección **Experimentos** enumera los experimentos de borrador, activos y finalizados para la aplicación actual. Haz clic en el filtro **Borrador** y, a continuación, haz clic en **Activar** para el experimento que quieras activar.

> **Importante**  Después de activar un experimento, ya no se pueden modificar sus parámetros a menos que sea un experimento de prueba (hiciste clic en la casilla **Experimento de prueba** cuando lo creaste). Te recomendamos que escribas el código del experimento en tu aplicación antes de activar el experimento.


## Revisar los resultados del experimento

1. En el Centro de desarrollo, vuelve a la página de la aplicación **Experimentación**.
2. En la sección **Experimentos**, haz clic en el filtro **Active** y, a continuación, haz clic en el nombre de tu experimento activo para ir a la página del experimento.
3. Para un experimento activo o completado, las primeras dos secciones en esta página proporcionan los resultados de la prueba:
  * La sección **Resumen de resultados** enumera los objetivos del experimento y el porcentaje de velocidad de conversión para cada variación.
  * La sección **Detalles de los resultados** proporciona más detalles para cada objetivo del experimento, incluidas vistas, conversiones, tasa de conversión,% diferencial, confianza y significado. La *confianza* es una medida estadística la confiabilidad de una estimación, que calcula el margen de error. La *importancia* es una medida estadística, basada en el tamaño de muestra para determinar la probabilidad de que un resultado no se deba al azar, sino que se atribuya a una causa específica.

  >**Nota** El Centro de desarrollo solo notifica el primer evento de conversión para cada usuario en un período de 24 horas. Si un usuario desencadena varios eventos de conversión en tu aplicación en un período de 24 horas, solo se informa el primer evento de conversión. El objetivo es evitar que un usuario con muchos eventos de conversión desvíe los resultados del experimento de un grupo de usuarios de muestra.


## Completar el experimento

1. En el panel, vuelve a la página del experimento. Consulta la sección anterior para leer instrucciones.
2. En la sección **Resumen de resultados**, realiza una de las siguientes acciones:
  * Si quieres finalizar el experimento y continuar con la configuración de la variación de control en la aplicación, haz clic en **Mantener**.
  * Si quieres finalizar el experimento pero cambiar a la configuración en una variación diferente en la aplicación, haz clic en **Cambiar** en la variación a la que quieres cambiar.
3. Haz clic en **Aceptar** para confirmar que deseas finalizar el experimento.


## Temas relacionados

  * [Define tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md)
  * [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)


<!--HONumber=May16_HO2-->


