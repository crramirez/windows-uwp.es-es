---
author: mcleanbyron
Description: En este tutorial, puedes crear, ejecutar y administrar el primer experimento con pruebas A/B.
title: Crear y ejecutar tu primer experimento con pruebas A/B
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store Services SDK, pruebas A/B, experimentos
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: fb9e93747fa77031fe906d9ab7463fc41b73cdeb
ms.lasthandoff: 02/07/2017

---

# <a name="create-and-run-your-first-experiment-with-ab-testing"></a>Crear y ejecutar tu primer experimento con pruebas A/B

En este tutorial, podrás:
* Crear un [proyecto](run-app-experiments-with-a-b-testing.md#terms) de experimentación en el panel del Centro de desarrollo que defina varias variables remotas que representen el texto y el color de un botón de la aplicación.
* Crear una aplicación con código que recupere los valores de las variables remotas, use estos datos para cambiar el color de fondo de un botón y registre los datos de eventos de conversión y de visualización en el Centro de desarrollo.
* Crear un experimento en el proyecto para comprobar si al cambiar el color de fondo de un botón de una aplicación aumenta satisfactoriamente el número de clics.
* Ejecutar la aplicación para recopilar datos del experimento.
* Revisa rlos resultados del experimento en el panel del Centro de desarrollo, elige una variación para habilitar para todos los usuarios de la aplicación y completa la prueba.

Para obtener una visión general de la prueba A/B del Centro de desarrollo, consulta [Ejecutar experimentos de la aplicación con una prueba A/B](run-app-experiments-with-a-b-testing.md).

## <a name="prerequisites"></a>Requisitos previos.

Para seguir con este tutorial, debes tener una cuenta del Centro de desarrollo de Windows y debes configurar el equipo de desarrollo como se describe en [Ejecución de experimentos de la aplicación con pruebas A/B](run-app-experiments-with-a-b-testing.md).

## <a name="create-a-project-with-remote-variables-in-windows-dev-center"></a>Creación de un proyecto con variables remotas en el Centro de desarrollo de Windows

1. Inicia sesión en el [panel del Centro de desarrollo](https://dev.windows.com/overview).
2. Si ya tienes una aplicación en el Centro de desarrollo que quieres usar para crear un experimento, selecciona la aplicación en tu panel. Si aún no tienes una aplicación en el panel, [crea una aplicación nueva reservando un nombre](../publish/create-your-app-by-reserving-a-name.md) y, a continuación, selecciona esa aplicación en el panel.
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

1. En Visual Studio 2015, crea un nuevo proyecto de la Plataforma universal de Windows con Visual C#. Nombra este proyecto **SampleExperiment**.
2. En el Explorador de soluciones, expande el nodo del proyecto, haz clic con el botón derecho en **Referencias** y en **Agregar referencia**.
3. En **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
4. En la lista de los SDK, selecciona la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.
5. En **Explorador de soluciones**, haz doble clic en MainPage.xaml para abrir el diseñador de la página principal de la aplicación.
6. Arrastra un **botón** de **Herramientas** a la página.
7. Haz doble clic en el botón del diseñador para abrir el archivo de código y agregar un controlador de eventos para el evento **Click**.  
8. Reemplaza todo el contenido del archivo de código por el siguiente código. Asigna la variable ```projectId``` al valor [Id. de proyecto](run-app-experiments-with-a-b-testing.md#terms) que obtuviste en el panel del Centro de desarrollo en la sección anterior.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

10. Guarda el archivo de código y crea el proyecto.

## <a name="create-the-experiment-in-windows-dev-center"></a>Creación del experimento en el Centro de desarrollo de Windows

1. Vuelve a la página de proyecto de **Experimentos de clics de botón** en el panel del Centro de desarrollo de Windows.
2. En la sección **Experimentos**, haz clic en el botón **Nuevo experimento**.
5. En la sección **Datos del experimento**, escribe el nombre **Optimizar clics de botón** en el campo **Nombre del experimento**.
6. En la sección **Evento de visualización** , escribe **userViewedButton** en el campo de **Nombre del evento de visualización**. Ten en cuenta que este nombre coincida con la cadena de evento de visualización en la que has iniciado sesión en el código que agregaste en la sección anterior.
7. En la sección **Objetivos y eventos de conversión**, escribe los siguientes valores:
  * En el campo **Nombre de objetivo**, escribe **Aumentar clics de botón**.
  * En el campo **Nombre de evento de conversión**, escribe el nombre **userClickedButton**. Ten en cuenta que este nombre coincida con la cadena de evento de conversión en la que has iniciado sesión en el código que agregaste en la sección anterior.
  * En el campo **Objetivo**, elige **Maximizar**.
8. En la sección **Variables remotas y variaciones** , confirma que la casilla **Distribuir de forma equitativa** está activada para que las variaciones se distribuyan por igual en la aplicación.
9. Agrega variables para tu experimento:
  9. Haz clic en el control de la lista desplegable, elige **buttonText**y haz clic en **Agregar variable**. La cadena **Botón gris** debe aparecer automáticamente en la columna **Variación A** (este valor se deriva de la configuración del proyecto). En la columna **Variación B**, escribe **Botón azul**.
  9. Haz clic de nuevo en el control de la lista desplegable, elige **r**y haz clic en **Agregar variable**. La cadena **128** debe aparecer automáticamente en la columna **Variación A**. En la columna **Variación B**, escribe **1**.
  9. Haz clic de nuevo en el control de la lista desplegable, elige **g**y haz clic en **Agregar variable**. La cadena **128** debe aparecer automáticamente en la columna **Variación A**. En la columna **Variación B**, escribe **1**.  
  9. Haz clic de nuevo en el control de la lista desplegable, elige **b**y haz clic en **Agregar variable**. La cadena **128** debe aparecer automáticamente en la columna **Variación A**. En la columna **Variación B**, escribe **255**.  
10. Haz clic en **Guardar** y luego en **Activar**.

> **Importante**&nbsp;&nbsp;Después de activar un experimento, ya no podrás modificar sus parámetros a menos que hayas hecho clic en la casilla **Experimento editable** al crear el experimento. Normalmente, recomendamos que escribas el código del experimento en tu aplicación antes de activar el experimento.

## <a name="run-the-app-to-gather-experiment-data"></a>Ejecución de la aplicación para recopilar datos del experimento

1. Ejecuta la aplicación **SampleExperiment** que creaste anteriormente.
2. Confirma que ves un botón azul o gris. Haz clic en el botón y, a continuación, cierra la aplicación.
3. Repite los pasos anteriores varias veces en el mismo equipo para confirmar que la aplicación muestra el mismo color de botón.

## <a name="review-the-results-and-complete-the-experiment"></a>Revisa los resultados y completa el experimento

Espera al menos varias horas después de completar la sección anterior y, a continuación, sigue estos pasos para revisar los resultados del experimento y completarlo.

> **Nota**&nbsp;&nbsp;En cuanto activas un experimento, el Centro de desarrollo inicia inmediatamente la recopilación de datos de las aplicaciones que están diseñadas para registrar los datos del experimento. Sin embargo, pueden pasar varias horas antes de que los datos del experimento aparezcan en el panel.

1. En el Centro de desarrollo, vuelve a la página de la aplicación **Experimentación**.
2. En la sección **Experimentos activos**, haz clic en **Optimizar clics de botón** y ve a la página de este experimento.
3. Confirma que los resultados que se muestran en las secciones **Resumen de resultados** y **Detalles de resultados** coinciden con lo que se espera ver. Para obtener más información acerca de estas secciones, consulta [Administra tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md#review-the-results-of-your-experiment).

  >**Nota**&nbsp;&nbsp;El Centro de desarrollo solo notifica el primer evento de conversión para cada usuario en un período de 24 horas. Si un usuario desencadena varios eventos de conversión en tu aplicación en un período de 24 horas, solo se informa el primer evento de conversión. El objetivo es evitar que un usuario con muchos eventos de conversión desvíe los resultados del experimento de un grupo de usuarios de muestra.
4. Ahora estás listo para finalizar el experimento. En la sección **Resumen de resultados**, en la columna **Variación B**, haz clic en **Cambiar**. Esto cambia todos los usuarios de la aplicación al botón azul.
5. Haz clic en **Aceptar** para confirmar que deseas finalizar el experimento.
6. Ejecuta la aplicación **SampleExperiment** que creaste en la sección anterior.
7. Confirma que ves un botón azul. Ten en cuenta que la aplicación puede tardar hasta dos minutos en recibir una asignación de variación actualizada.

## <a name="related-topics"></a>Temas relacionados

* [Creación de un proyecto y definición de variables remotas en el panel del Centro de desarrollo](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Programar tu aplicación para los experimentos.](code-your-experiment-in-your-app.md)
* [Definición de tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
* [Administración de tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
* [Ejecución de experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)

