---
Description: Después de definir el experimento en el centro de partners y el experimento de código en la aplicación, estará listo en activo el experimento y usar el centro de partners para revisar los resultados del experimento.
title: Administrar tu experimento en el Centro de partners
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP, Microsoft Store Services SDK, pruebas A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: 6e5c0d0ca1b1d771df2b224cc41ec5a37e267bc9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594930"
---
# <a name="manage-your-experiment-in-partner-center"></a>Administrar tu experimento en el Centro de partners

Después de [definir el experimento en el centro de partners](define-your-experiment-in-the-dev-center-dashboard.md) y [codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md), está listo para activar el experimento y usar el centro de partners para revisar los resultados del experimento. Después de haber obtenido todos los datos que necesitas, puedes terminar el experimento y elegir si quieres seguir usando los valores de variables del control de variación para todas tus aplicaciones o cambiar a los valores de variables de otra de tus variaciones.

> [!NOTE]
> Al activar un experimento, centro de partners empieza inmediatamente a recopilar datos desde las aplicaciones que se ha instrumentado para registrar los datos del experimento. Sin embargo, puede tardar varias horas de experimento datos aparezcan en el centro de partners.

Para ver un tutorial que muestre de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crear y ejecutar el primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## <a name="activate-your-experiment"></a>Activar el experimento

Cuando esté satisfecho con los parámetros del experimento en el centro de partners y ha actualizado el código de aplicación, está listo para activar el experimento para que pueda empezar a recopilar datos de experimento de la aplicación. Cuando está activo el experimento, la aplicación puede recuperar los valores de variación y notificar los eventos de vista y la conversión a centro de partners.

1. Inicia sesión en el [Centro de partners](https://partner.microsoft.com/dashboard).
2. En **Tus aplicaciones**, selecciona la aplicación con el experimento que quieras activar.
3. En el panel de navegación, selecciona **Servicios** y, a continuación, selecciona **Experimentación**.
4. En la tabla de proyectos de la sección **Proyectos**, expande el proyecto que contiene tu experimento y, después, realiza una de las siguientes acciones:
  * Haz clic en el vínculo **Activar** del experimento. El experimento se agrega a la sección **Experimentos activos** en la parte superior de la página.
  * Haz clic en el nombre del experimento, desplázate a la parte inferior de la página del experimento y haz clic en **Activar**.

> [!IMPORTANT]
> Después de activar un experimento, ya no se pueden modificar sus parámetros a menos que hayas hecho clic en la casilla **Experimento editable** al crear el experimento. Te recomendamos que escribas el código del experimento en tu aplicación antes de activar el experimento.

## <a name="review-the-results-of-your-experiment"></a>Revisar los resultados del experimento

1. En el centro de partners, vuelva a la **experimentación** página de la aplicación.
2. En la sección **Experimentos activos**, haz clic en el nombre de tu experimento activo para ir a la página del experimento.
3. Para un experimento activo o completado, las primeras dos secciones en esta página proporcionan los resultados de la prueba:
  * La sección **Resumen de resultados** enumera los objetivos del experimento y el porcentaje de velocidad de conversión para cada variación.
  * La sección **Detalles de los resultados** proporciona más detalles para cada variación de todos los objetivos del experimento, incluidas vistas, conversiones, usuarios únicos, tasa de conversión, % diferencial, confianza y significación. La *confianza* es una medida estadística la confiabilidad de una estimación, que calcula el margen de error. La *importancia* es una medida estadística, basada en el tamaño de muestra para determinar la probabilidad de que un resultado no se deba al azar, sino que se atribuya a una causa específica.

> [!NOTE]
> Centro de partners notifica solo el primer evento de conversión para cada usuario en un período de 24 horas. Si un usuario desencadena varios eventos de conversión en tu aplicación en un período de 24 horas, solo se informa el primer evento de conversión. El objetivo es evitar que un usuario con muchos eventos de conversión desvíe los resultados del experimento de un grupo de usuarios de muestra.


## <a name="complete-your-experiment"></a>Completar el experimento

1. En el centro de partners, volver a la página del experimento. Consulta la sección anterior para leer instrucciones.
2. En la sección **Resumen de resultados**, realiza una de las siguientes acciones:
  * Si quieres finalizar el experimento y continuar usando los valores de variables de la variación de control en la aplicación, haz clic en **Mantener**.
  * Si quieres finalizar el experimento pero cambiar a los valores de variables de una variación diferente en la aplicación, haz clic en **Cambiar** en la variación a la que quieres cambiar.
3. Haz clic en **Aceptar** para confirmar que quieres finalizar el experimento.


## <a name="related-topics"></a>Temas relacionados

* [Cree un proyecto y definir variables remotas en el centro de partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md)
* [Definir el experimento en el centro de partners](define-your-experiment-in-the-dev-center-dashboard.md)
* [Cree y ejecute su primer experimento con un pruebas A/b](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecutar los experimentos de la aplicación con un pruebas A/b](run-app-experiments-with-a-b-testing.md)
