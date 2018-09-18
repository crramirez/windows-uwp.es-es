---
author: mcleanbyron
Description: After you define your experiment in the Dev Center dashboard and code your experiment in your app, you are ready to active your experiment and use the Dev Center dashboard to review the results of your experiment.
title: Administrar tu experimento en el panel
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP, Microsoft Store Services SDK, Microsoft Store Services SDK, A/B tests, pruebas A/B, experiments, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: 073d5ec67bb012cbfe21c279ee97ec3c50b8458f
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "4017129"
---
# <a name="manage-your-experiment-in-the-dashboard"></a>Administrar tu experimento en el panel

Después de [definir el experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md) y [codificar la aplicación para el experimento](code-your-experiment-in-your-app.md), estás listo para activar el experimento y usar el panel del Centro de desarrollo para revisar los resultados del experimento. Después de haber obtenido todos los datos que necesitas, puedes terminar el experimento y elegir si quieres seguir usando los valores de variables del control de variación para todas tus aplicaciones o cambiar a los valores de variables de otra de tus variaciones.

> [!NOTE]
> Cuando activas un experimento, el Centro de desarrollo empieza inmediatamente a recopilar datos de las aplicaciones que están pensadas para registrar los datos del experimento. Sin embargo, pueden pasar varias horas antes de que los datos del experimento aparezcan en el panel.

Para ver un tutorial que muestra de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Activar el experimento

Cuando estés satisfecho con los parámetros del experimento en el panel y hayas actualizado el código de aplicación, estarás listo para activar el experimento para que puedas iniciar la recopilación de datos del experimento a partir de la aplicación. Cuando el experimento esté activo, tu aplicación puede recuperar los valores de variación y notificar los eventos de vista y conversión al Centro de desarrollo.

1. Inicia sesión en el [panel del Centro de desarrollo](https://dev.windows.com/overview).
2. En **Tus aplicaciones**, selecciona la aplicación con el experimento que quieras activar.
3. En el panel de navegación, selecciona **Servicios** y, a continuación, selecciona **Experimentación**.
4. En la tabla de proyectos de la sección **Proyectos**, expande el proyecto que contiene tu experimento y, después, realiza una de las siguientes acciones:
  * Haz clic en el vínculo **Activar** del experimento. El experimento se agrega a la sección **Experimentos activos** en la parte superior de la página.
  * Haz clic en el nombre del experimento, desplázate a la parte inferior de la página del experimento y haz clic en **Activar**.

> [!IMPORTANT]
> Después de activar un experimento, ya no se pueden modificar sus parámetros a menos que hayas hecho clic en la casilla **Experimento editable** al crear el experimento. Te recomendamos que escribas el código del experimento en tu aplicación antes de activar el experimento.

## <a name="review-the-results-of-your-experiment"></a>Revisar los resultados del experimento

1. En el Centro de desarrollo, vuelve a la página **Experimentación** de la aplicación.
2. En la sección **Experimentos activos**, haz clic en el nombre de tu experimento activo para ir a la página del experimento.
3. Para un experimento activo o completado, las primeras dos secciones en esta página proporcionan los resultados de la prueba:
  * La sección **Resumen de resultados** enumera los objetivos del experimento y el porcentaje de velocidad de conversión para cada variación.
  * La sección **Detalles de los resultados** proporciona más detalles para cada variación de todos los objetivos del experimento, incluidas vistas, conversiones, usuarios únicos, tasa de conversión, % diferencial, confianza y significación. La *confianza* es una medida estadística la confiabilidad de una estimación, que calcula el margen de error. La *importancia* es una medida estadística, basada en el tamaño de muestra para determinar la probabilidad de que un resultado no se deba al azar, sino que se atribuya a una causa específica.

> [!NOTE]
> El Centro de desarrollo solo notifica el primer evento de conversión para cada usuario en un período de 24 horas. Si un usuario desencadena varios eventos de conversión en tu aplicación en un período de 24 horas, solo se informa el primer evento de conversión. El objetivo es evitar que un usuario con muchos eventos de conversión desvíe los resultados del experimento de un grupo de usuarios de muestra.


## <a name="complete-your-experiment"></a>Completar el experimento

1. En el panel, vuelve a la página del experimento. Consulta la sección anterior para leer instrucciones.
2. En la sección **Resumen de resultados**, realiza una de las siguientes acciones:
  * Si quieres finalizar el experimento y continuar usando los valores de variables de la variación de control en la aplicación, haz clic en **Mantener**.
  * Si quieres finalizar el experimento pero cambiar a los valores de variables de una variación diferente en la aplicación, haz clic en **Cambiar** en la variación a la que quieres cambiar.
3. Haz clic en **Aceptar** para confirmar que quieres finalizar el experimento.


## <a name="related-topics"></a>Temas relacionados

* [Creación de un proyecto y definición de variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Programar tu aplicación para los experimentos.](code-your-experiment-in-your-app.md)
* [Definir el experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
* [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)
