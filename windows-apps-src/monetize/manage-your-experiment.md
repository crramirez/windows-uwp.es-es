---
description: Después de definir el experimento en el centro de Partners y codificar el experimento en la aplicación, está listo para activar el experimento y usar el centro de partners para revisar los resultados del experimento.
title: Administrar el experimento en el Centro de partners
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: SDK de Windows 10, UWP, Microsoft Store Services, pruebas A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: dfcd9819940d21dcc81c5ac698b76381adf05af6
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030558"
---
# <a name="manage-your-experiment-in-partner-center"></a>Administrar el experimento en el Centro de partners

Después [de definir el experimento en el centro de Partners](define-your-experiment-in-the-dev-center-dashboard.md) y [codificar la aplicación para experimentación](code-your-experiment-in-your-app.md), está listo para activar el experimento y usar el centro de partners para revisar los resultados del experimento. Después de haber obtenido todos los datos que necesitas, puedes terminar el experimento y elegir si quieres seguir usando los valores de variables del control de variación para todas tus aplicaciones o cambiar a los valores de variables de otra de tus variaciones.

> [!NOTE]
> Al activar un experimento, el centro de Partners comienza inmediatamente a recopilar datos de cualquier aplicación que se instrumenta para registrar los datos del experimento. Sin embargo, los datos del experimento pueden tardar varias horas en aparecer en el centro de Partners.

Para ver un tutorial que muestre de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crear y ejecutar el primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Activar el experimento

Cuando esté satisfecho con los parámetros de su experimento en el centro de Partners y haya actualizado el código de la aplicación, estará listo para activar el experimento para que pueda empezar a recopilar datos del experimento desde la aplicación. Cuando el experimento está activo, la aplicación puede recuperar los valores de variación y la vista de informe y los eventos de conversión al centro de Partners.

1. Inicie sesión en el [Centro de partners](https://partner.microsoft.com/dashboard).
2. En **Tus aplicaciones** , selecciona la aplicación con el experimento que quieras activar.
3. En el panel de navegación, selecciona **Servicios** y, a continuación, selecciona **Experimentación** .
4. En la tabla de proyectos de la sección **Proyectos** , expande el proyecto que contiene tu experimento y, después, realiza una de las siguientes acciones:
  * Haz clic en el vínculo **Activar** del experimento. El experimento se agrega a la sección **Experimentos activos** en la parte superior de la página.
  * Haz clic en el nombre del experimento, desplázate a la parte inferior de la página del experimento y haz clic en **Activar** .

> [!IMPORTANT]
> Después de activar un experimento, ya no podrá modificar los parámetros del experimento a menos que haya activado la casilla **experimento editable** al crear el experimento. Te recomendamos que escribas el código del experimento en tu aplicación antes de activar el experimento.

## <a name="review-the-results-of-your-experiment"></a>Revisar los resultados del experimento

1. En el centro de Partners, vuelva a la página **experimentación** de la aplicación.
2. En la sección **Experimentos activos** , haz clic en el nombre de tu experimento activo para ir a la página del experimento.
3. Para un experimento activo o completado, las primeras dos secciones en esta página proporcionan los resultados de la prueba:
  * La sección **Resumen de resultados** enumera los objetivos del experimento y el porcentaje de velocidad de conversión para cada variación.
  * La sección **Detalles de los resultados** proporciona más detalles para cada variación de todos los objetivos del experimento, incluidas vistas, conversiones, usuarios únicos, tasa de conversión, % diferencial, confianza y significación. La *confianza* es una medida estadística la confiabilidad de una estimación, que calcula el margen de error. La *importancia* es una medida estadística, basada en el tamaño de muestra para determinar la probabilidad de que un resultado no se deba al azar, sino que se atribuya a una causa específica.

> [!NOTE]
> El centro de Partners solo informa del primer evento de conversión de cada usuario en un período de 24 horas. Si un usuario desencadena varios eventos de conversión en tu aplicación en un período de 24 horas, solo se informa el primer evento de conversión. El objetivo es evitar que un usuario con muchos eventos de conversión desvíe los resultados del experimento de un grupo de usuarios de muestra.


## <a name="complete-your-experiment"></a>Completar el experimento

1. En el centro de Partners, vuelva a la página del experimento. Consulta la sección anterior para leer instrucciones.
2. En la sección **Resumen de resultados** , realiza una de las siguientes acciones:
  * Si quieres finalizar el experimento y continuar usando los valores de variables de la variación de control en la aplicación, haz clic en **Mantener** .
  * Si quieres finalizar el experimento pero cambiar a los valores de variables de una variación diferente en la aplicación, haz clic en **Cambiar** en la variación a la que quieres cambiar.
3. Haz clic en **Aceptar** para confirmar que quieres finalizar el experimento.


## <a name="related-topics"></a>Temas relacionados

* [Crear un proyecto y definir variables remotas en el centro de Partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Programar tu aplicación para los experimentos.](code-your-experiment-in-your-app.md)
* [Definir el experimento en el Centro de partners](define-your-experiment-in-the-dev-center-dashboard.md)
* [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)
