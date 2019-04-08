---
Description: En este tutorial, puedes crear, ejecutar y administrar el primer experimento con pruebas A/B.
title: Crear y ejecutar tu primer experimento
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP, Microsoft Store Services SDK, pruebas A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: 8dba9095326c01029e14742c98c1c368b896dfb8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660220"
---
# <a name="create-and-run-your-first-experiment"></a>Crear y ejecutar tu primer experimento

En este tutorial, podrás:
* Crear una experimentación [proyecto](run-app-experiments-with-a-b-testing.md#terms) en el centro de partners que define varias variables remotas que representan el texto y el color de un botón de la aplicación.
* Crear una aplicación con código que recupera los valores de variable remotos, usa estos datos para cambiar el color de fondo de un botón y vista de los registros y datos de eventos de conversión al centro de partners.
* Crear un experimento en el proyecto para comprobar si al cambiar el color de fondo de un botón de una aplicación aumenta satisfactoriamente el número de clics.
* Ejecutar la aplicación para recopilar datos del experimento.
* Revise los resultados del experimento en el centro de partners, elija una variación para habilitar para todos los usuarios de la aplicación y completar el experimento.

Para obtener una introducción A / B probar con el centro de partners, consulte [ejecutar experimentos de la aplicación con un pruebas A/b](run-app-experiments-with-a-b-testing.md).

## <a name="prerequisites"></a>Requisitos previos

Para seguir este tutorial, debe tener una cuenta de centro de partners y debe configurar el equipo de desarrollo como se describe en [ejecutar experimentos de la aplicación con un pruebas A/b](run-app-experiments-with-a-b-testing.md).

## <a name="create-a-project-with-remote-variables-in-partner-center"></a>Crear un proyecto con variables remotas en el centro de partners

1. Inicia sesión en el [Centro de partners](https://partner.microsoft.com/dashboard).
2. Si ya tiene una aplicación en el centro de partners que desea usar para crear un experimento, seleccione esa aplicación en el centro de partners. Si aún no tiene una aplicación en el centro de partners, [crear una nueva aplicación mediante la reserva de un nombre](../publish/create-your-app-by-reserving-a-name.md) y, a continuación, seleccione esa aplicación en el centro de partners.
3. En el panel de navegación, haz clic en **Servicios** y, a continuación, haz clic en **Experimentación**.
4. En la sección de **Proyectos** de la página siguiente, haz clic en el botón **Nuevo proyecto**.
5. En la página **Nuevo proyecto**, escribe el nombre **Experimentos de clics de botón** para el nuevo proyecto.
6. Expande la sección **Variables remotas** y haz clic en **Agregar variable** cuatro veces. Ahora deberías tener cuatro filas de variables vacías.
  * En la primera fila, escribe **buttonText** para el nombre y el tipo de variable **Botón gris** en la columna **Valor predeterminado**.
  * En la segunda fila, escribe **r** para el nombre y el tipo de variable **128** en la columna **Valor predeterminado**.
  * En la tercera fila, escribe **g** para el nombre y el tipo de variable **128** en la columna **Valor predeterminado**.
  * En la cuarta fila, escribe **b** para el nombre y el tipo de variable **128** en la columna **Valor predeterminado**.
7. Haz clic en **Guardar** y toma nota del valor de la [Id. de proyecto](run-app-experiments-with-a-b-testing.md#terms) que aparece en la sección **Integración de SDK**. En la siguiente sección, actualizarás el código de la aplicación y referenciarás este valor en el código.

## <a name="code-the-experiment-in-your-app"></a>Codificación del experimento en la aplicación

1. En Visual Studio, crea un nuevo proyecto de la Plataforma universal de Windows con Visual C#. Nombra este proyecto **SampleExperiment**.
2. En el Explorador de soluciones, expande el nodo del proyecto, haz clic con el botón derecho en **Referencias** y en **Agregar referencia**.
3. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
4. En la lista de los SDK, selecciona la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.
5. En **Explorador de soluciones**, haz doble clic en MainPage.xaml para abrir el diseñador de la página principal de la aplicación.
6. Arrastra un **botón** de **Herramientas** a la página.
7. Haz doble clic en el botón del diseñador para abrir el archivo de código y agregar un controlador de eventos para el evento **Click**.  
8. Reemplaza todo el contenido del archivo de código por el siguiente código. Asignar el ```projectId``` variable a la [identificador del proyecto](run-app-experiments-with-a-b-testing.md#terms) valor que ha obtenido en el centro de partners en la sección anterior.
    [!code-cs[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

9. Guarda el archivo de código y crea el proyecto.

## <a name="create-the-experiment-in-partner-center"></a>Creación del experimento en el centro de partners

1. Vuelva a la **experimentos de haga clic en botón** página del proyecto en el centro de partners.
2. En la sección **Experimentos**, haz clic en el botón **Nuevo experimento**.
3. En la sección **Datos del experimento**, escribe el nombre **Optimizar clics de botón** en el campo **Nombre del experimento**.
4. En la sección **Evento de visualización** , escribe **userViewedButton** en el campo de **Nombre del evento de visualización**. Ten en cuenta que este nombre coincida con la cadena de evento de visualización en la que has iniciado sesión en el código que agregaste en la sección anterior.
5. En la sección **Objetivos y eventos de conversión**, escribe los siguientes valores:
  * En el campo **Nombre de objetivo**, escribe **Aumentar clics de botón**.
  * En el campo **Nombre de evento de conversión**, escribe el nombre **userClickedButton**. Ten en cuenta que este nombre coincida con la cadena de evento de conversión en la que has iniciado sesión en el código que agregaste en la sección anterior.
  * En el campo **Objetivo**, elige **Maximizar**.
6. En la sección **Variables remotas y variaciones** , confirma que la casilla **Distribuir de forma equitativa** está activada para que las variaciones se distribuyan por igual en la aplicación.
7. Agrega variables para tu experimento:
    1. Haz clic en el control de la lista desplegable, elige **buttonText**y haz clic en **Agregar variable**. La cadena **Botón gris** debe aparecer automáticamente en la columna **Variación A** (este valor se deriva de la configuración del proyecto). En la columna **Variación B**, escribe **Botón azul**.
    2. Haz clic de nuevo en el control de la lista desplegable, elige **r**y haz clic en **Agregar variable**. La cadena **128** debe aparecer automáticamente en la columna **Variación A**. En la columna **Variación B**, escribe **1**.
    3. Haz clic de nuevo en el control de la lista desplegable, elige **g**y haz clic en **Agregar variable**. La cadena **128** debe aparecer automáticamente en la columna **Variación A**. En la columna **Variación B**, escribe **1**.  
    4. Haz clic de nuevo en el control de la lista desplegable, elige **b**y haz clic en **Agregar variable**. La cadena **128** debe aparecer automáticamente en la columna **Variación A**. En la columna **Variación B**, escribe **255**.  
8. Haz clic en **Guardar** y luego en **Activar**.

> [!IMPORTANT]
> Después de activar un experimento, ya no se pueden modificar sus parámetros a menos que hayas hecho clic en la casilla **Experimento editable** al crear el experimento. Normalmente, recomendamos que escribas el código del experimento en tu aplicación antes de activar el experimento.

## <a name="run-the-app-to-gather-experiment-data"></a>Ejecución de la aplicación para recopilar datos del experimento

1. Ejecuta la aplicación **SampleExperiment** que creaste anteriormente.
2. Confirma que ves un botón azul o gris. Haz clic en el botón y, a continuación, cierra la aplicación.
3. Repite los pasos anteriores varias veces en el mismo equipo para confirmar que la aplicación muestra el mismo color de botón.

## <a name="review-the-results-and-complete-the-experiment"></a>Revisa los resultados y completa el experimento

Espera al menos varias horas después de completar la sección anterior y, a continuación, sigue estos pasos para revisar los resultados del experimento y completarlo.

> [!NOTE]
> Tan pronto como activar un experimento, centro de partners empieza inmediatamente a recopilar datos desde las aplicaciones que se ha instrumentado para registrar los datos del experimento. Sin embargo, puede tardar varias horas de experimento datos aparezcan en el centro de partners.

1. En el centro de partners, vuelva a la **experimentación** página de la aplicación.
2. En la sección **Experimentos activos**, haz clic en **Optimizar clics de botón** y ve a la página de este experimento.
3. Confirma que los resultados que se muestran en las secciones **Resumen de resultados** y **Detalles de resultados** coinciden con lo que se espera ver. Para obtener más información acerca de estas secciones, consulte [administrar su experimento en el centro de partners](manage-your-experiment.md#review-the-results-of-your-experiment).
    > [!NOTE]
    > Centro de partners notifica solo el primer evento de conversión para cada usuario en un período de 24 horas. Si un usuario desencadena varios eventos de conversión en tu aplicación en un período de 24 horas, solo se informa el primer evento de conversión. El objetivo es evitar que un usuario con muchos eventos de conversión desvíe los resultados del experimento de un grupo de usuarios de muestra.

4. Ahora estás listo para finalizar el experimento. En la sección **Resumen de resultados**, en la columna **Variación B**, haz clic en **Cambiar**. Esto cambia todos los usuarios de la aplicación al botón azul.
5. Haz clic en **Aceptar** para confirmar que quieres finalizar el experimento.
6. Ejecuta la aplicación **SampleExperiment** que creaste en la sección anterior.
7. Confirma que ves un botón azul. Ten en cuenta que la aplicación puede tardar hasta dos minutos en recibir una asignación de variación actualizada.

## <a name="related-topics"></a>Temas relacionados

* [Cree un proyecto y definir variables remotas en el centro de partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md)
* [Definir el experimento en el centro de partners](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administrar el experimento en el centro de partners](manage-your-experiment.md)
* [Ejecutar los experimentos de la aplicación con un pruebas A/b](run-app-experiments-with-a-b-testing.md)
