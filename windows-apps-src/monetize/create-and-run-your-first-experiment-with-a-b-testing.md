---
author: mcleanbyron
Description: En este tutorial, puedes crear y ejecutar el primer experimento con pruebas A/B.
title: Crea y ejecuta tu primer experimento con pruebas A/B
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
---

# Crea y ejecuta tu primer experimento con pruebas A/B

En este tutorial, podrás:
* Crear una prueba en el panel del Centro de desarrollo de Windows que comprueba si al cambiar el color de fondo de un botón de una aplicación aumenta el número de clics correctamente.
* Crear una aplicación que recupere la configuración de variación del Centro de desarrollo, use estos datos para cambiar el color de fondo de un botón y registre los datos de eventos de conversión y de visualización en el Centro de desarrollo.
* Ejecutar la aplicación para recopilar datos de prueba.
* Revisar los resultados del experimento en el panel del Centro de desarrollo, elegir una variación para habilitar para todos los usuarios de la aplicación y completar el experimento.

Para obtener una visión general de la prueba A/B del Centro de desarrollo, consulta [Ejecutar experimentos de la aplicación con una prueba A/B](run-app-experiments-with-a-b-testing.md).

## Requisitos previos.

Para seguir con este tutorial, debes tener una cuenta del Centro de desarrollo de Windows y debes configurar el equipo de desarrollo como se describe en [Ejecutar experimentos de la aplicación con una prueba A/B](run-app-experiments-with-a-b-testing.md).

## Crear el experimento en el Centro de desarrollo de Windows

1. Inicia sesión en el [panel del Centro de desarrollo](https://dev.windows.com/overview).
2. Si ya tienes una aplicación en el Centro de desarrollo que deseas usar para crear un experimento, debajo de **Tus aplicaciones**, selecciona la aplicación. Si aún no tienes una aplicación en el panel, [crea una aplicación nueva reservando un nombre](../publish/create-your-app-by-reserving-a-name.md) y, a continuación, selecciona esa aplicación en el panel.
3. En el panel de navegación, haz clic en **Servicios** y, a continuación, haz clic en **Experimentación**.
4. En la sección **Claves API**, selecciona **Nueva clave API** para generar una nueva clave de API y escribe el nombre **Mi primer experimento** para la clave de API. Deberás usar esta clave de API en la siguiente sección de este tutorial.
5. En la sección **Experimentos**, haz clic en **Nuevo experimento**. En el campo **Nombre del experimento**, escribe **Optimizar clics de botón**.
6. En el campo **Nombre de evento de visualización**, escribe **userViewedButton**. Más adelante en este tutorial, agregarás el código que registra este evento de visualización cuando se inicializa la página principal de la aplicación y el botón es visible para el usuario.
7. En la sección **Objetivos y los eventos de conversión**, escribe los siguientes valores:
  * En el campo **Nombre de objetivo**, escribe **Aumentar clics de botón**.
  * En el campo **Nombre de evento de conversión**, escribe el nombre **userClickedButton**. Más adelante en este tutorial, agregarás código que registra este evento de conversión en el controlador de eventos Click del botón.
  * En el campo **Objetivo**, elige **Maximizar**.
8. En la sección **Variaciones y configuración**, haz clic en **Agregar configuración** tres veces. Ahora deberías tener cuatro filas de valores vacíos.
  * En la primera fila, escribe **buttonText** para el nombre de la configuración, escribe **Botón gris** en la columna **Variación A** y, a continuación, escribe **Botón azul** en la columna **Variación B**.
  * En la segunda fila, escribe **r** para el nombre de la configuración, escribe **128** en la **Variación A** y escribe **1** en la columna **Variación B**.
  * En la tercera fila, escribe **g** para el nombre de la configuración, escribe **128** en la **Variación A** y escribe **1** en la columna **Variación B**.
  * En la cuarta fila, escribe **b** para el nombre de la configuración, escribe **128** en la **Variación A** y escribe **255** en la columna **Variación B**.  
9. Confirma que la casilla de **Distribuir de forma equitativa** está activada para que las variaciones se distribuyan por igual en la aplicación.
10. Haz clic en **Guardar** y luego en **Activar**.

> **Importante**  Después de activar un experimento, ya no se pueden modificar sus parámetros a menos que sea un experimento de prueba (si hiciste clic en la casilla **Experimento de prueba** cuando lo creaste). Normalmente, recomendamos que escribas el código del experimento en tu aplicación antes de activar el experimento. Para simplificar, en este tutorial se puede activar el experimento ahora.

## Codifica el experimento en la aplicación.

1. En Visual Studio 2015, crea un nuevo proyecto de la Plataforma universal de Windows con Visual C#. Nombra este proyecto **SampleExperiment**.
2. En el Explorador de soluciones, expande el nodo del proyecto, haz clic con el botón secundario en el nodo **Referencias** y haz clic en **Agregar referencia**.
3. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
4. En la lista de los SDK, selecciona la casilla junto a **SDK de Microsoft Store Engagement** y haz clic en **Aceptar**.
5. En el **Explorador de soluciones**, haz doble clic en MainPage.xaml para abrir el diseñador de la página principal de la aplicación.
6. Arrastra un **botón** de **Herramientas** a la página.
7. Haz doble clic en el botón del diseñador para abrir el archivo de código y agregar un controlador de eventos para el evento **Click**.  
8. Reemplaza todo el contenido del archivo de código por el siguiente código.

  ```CSharp
  using System;
  using Windows.UI.Xaml;
  using Windows.UI.Xaml.Controls;
  using Windows.UI.Xaml.Media;
  using System.Threading.Tasks;
  using Windows.UI;
  using Windows.UI.Core;

  // Namespace for A/B testing.
  using Microsoft.Services.Store.Engagement;

  namespace SampleExperiment
  {  
    public sealed partial class MainPage : Page
    {
        private readonly ExperimentClient experiment;
        private ExperimentVariation variation;

        // Assign this variable to your API key from Dev Center. The API key
        // shown below is for example purposes only.
        private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";    

        public MainPage()
        {
            this.InitializeComponent();

            // Initialize the ExperimentClient for A/B testing.
            experiment = new ExperimentClient(apiKey);

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
            #pragma warning disable CS4014
            Initialize();
            #pragma warning restore CS4014
        }

        private async Task Initialize()
        {
            // Get the current cached variation assignment for the experiment.
            ExperimentVariationResult result = await experiment.GetVariationAsync();
            variation = result.Variation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
            {
                result = await experiment.RefreshVariationAsync();

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == EngagementErrorCode.Success)
                {
                    variation = result.Variation;
                }
            }

            // Get settings named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the settings default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInteger("r", 128);
            var g = (byte)variation.GetInteger("g", 128);
            var b = (byte)variation.GetInteger("b", 128);

            // Assign button text and color.
            await button.Dispatcher.RunAsync(
                CoreDispatcherPriority.Normal,
                () =>
                {
                    button.Background = new SolidColorBrush(Color.FromArgb(255, r, g, b));
                    button.Content = buttonText;
                    button.Visibility = Visibility.Visible;
                });

            // Log the view event named "userViewedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userViewedButton", variation);
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userClickedButton", variation);
        }
     }
  }
  ```
9. En la siguiente línea de código, asigna la variable *apiKey* a la clave de API que obtuviste en el panel del Centro de desarrollo en la sección anterior. La clave de API que se muestra a continuación es solo un ejemplo.
```CSharp
private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";
```
10. Guarda el archivo de código y crea el proyecto.

## Ejecuta la aplicación para recopilar datos de prueba

1. Ejecuta la aplicación **SampleExperiment** que creaste en la sección anterior.
2. Confirma que ves un botón azul o gris. Haz clic en el botón y, a continuación, cierra la aplicación.
3. Repite los pasos anteriores varias veces en el mismo equipo para confirmar que la aplicación muestra el mismo color de botón.

## Revisa los resultados y completa el experimento

Espera al menos varias horas después de completar la sección anterior y, a continuación, sigue estos pasos para revisar los resultados del experimento y completarlo.

> **Nota** En cuanto activas un experimento, el Centro de desarrollo inicia inmediatamente la recopilación de datos de las aplicaciones que están diseñadas para registrar los datos del experimento. Sin embargo, pueden pasar varias horas antes de que los datos del experimento aparezcan en el panel.

1. En el Centro de desarrollo, vuelve a la página de la aplicación **Experimentación**.
2. En la sección **Experimentos**, haz clic en el filtro **Activo** y, a continuación, haz clic en **Optimizar clics de botón** para ir a la página de este experimento.
3. Confirma que los resultados que se muestran en las secciones **Resumen de resultados** y **Detalles de resultados** coinciden con lo que se espera ver. Para obtener más información acerca de estas secciones, consulta [Administra tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md#review-the-results-of-your-experiment).

  >**Nota** El Centro de desarrollo solo notifica el primer evento de conversión para cada usuario en un período de 24 horas. Si un usuario desencadena varios eventos de conversión en tu aplicación en un período de 24 horas, solo se informa el primer evento de conversión. El objetivo es evitar que un usuario con muchos eventos de conversión desvíe los resultados del experimento de un grupo de usuarios de muestra.

4. Ahora estás listo para finalizar el experimento. En la sección **Resumen de resultados**, en la columna **Variación B**, haz clic en **Cambiar**. Esto cambia todos los usuarios de la aplicación al botón azul.
5. Haz clic en **Aceptar** para confirmar que deseas finalizar el experimento.
6. Ejecuta la aplicación **SampleExperiment** que creaste en la sección anterior.
7. Confirma que ves un botón azul. Ten en cuenta que la aplicación puede tardar hasta dos minutos en recibir una asignación de variación actualizada.

## Temas relacionados

  * [Define tu experimento en el panel del Centro de desarrollo](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md)
  * [Administra tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
  * [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)


<!--HONumber=May16_HO2-->


