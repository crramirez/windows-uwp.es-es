---
title: Actualizar un icono dinámico desde una tarea en segundo plano
description: Usa una tarea en segundo plano para actualizar el icono dinámico de tu aplicación con contenido actualizado.
Search.SourceType: Video
ms.assetid: 9237A5BD-F9DE-4B8C-B689-601201BA8B9A
ms.date: 01/11/2018
ms.topic: article
keywords: Windows 10, uwp, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 3d5d9e1b93c9f769a748a6a44d0ed77f5bd3a9da
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8691179"
---
# <a name="update-a-live-tile-from-a-background-task"></a>Actualizar un icono dinámico desde una tarea en segundo plano

**API importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Usa una tarea en segundo plano para actualizar el icono dinámico de tu aplicación con contenido actualizado.

Este vídeo te muestra cómo agregar iconos dinámicos a tus aplicaciones.

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Updating-a-live-tile-from-a-background-task/player" width="720" height="405" allowFullScreen="true" frameBorder="0"></iframe>

## <a name="create-the-background-task-project"></a>Crear el proyecto de tareas en segundo plano  

Para habilitar un icono dinámico para tu aplicación, agrega un nuevo proyecto de componente de Windows en tiempo de ejecución a tu solución. Este es un ensamblado independiente que el sistema operativo carga y ejecuta en segundo plano cuando un usuario instala tu aplicación.

1.  En el Explorador de soluciones, haz clic con el botón secundario en la solución, haz clic en **Agregar** y luego haz clic en **Nuevo proyecto**.
2.  En el diálogo **Agregar nuevo proyecto**, selecciona la plantilla **Componente de Windows en tiempo de ejecución** en la sección **Instalado &gt; Otros lenguajes &gt; Visual C# &gt; Windows Universal**
3.  Asigna el nombre BackgroundTasks al proyecto y haz clic o pulsa **Aceptar**. Microsoft Visual Studio agrega el nuevo proyecto a la solución.
4.  En el proyecto principal, agrega una referencia al proyecto BackgroundTasks.

## <a name="implement-the-background-task"></a>Implementar la tarea en segundo plano


Implementa la interfaz [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) para crear una clase que actualice el icono dinámico de tu aplicación. El trabajo en segundo plano se incluye en el método Run. En este caso, la tarea obtiene una fuente de distribución para los blogs de MSDN. Para impedir que la tarea se cierre de forma prematura mientras aún se ejecuta el código asincrónico, obtén un aplazamiento.

1.  En el Explorador de soluciones, cambia el nombre del archivo generado automáticamente, Class1.cs, a BlogFeedBackgroundTask.cs.
2.  En BlogFeedBackgroundTask.cs, reemplaza el código generado automáticamente con el código auxiliar de la clase **BlogFeedBackgroundTask**.
3.  En la implementación del método Run, agrega código para los métodos **GetMSDNBlogFeed** y **UpdateTile**.

```cs
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

// Added during quickstart
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.UI.Notifications;
using Windows.Web.Syndication;

namespace BackgroundTasks
{
    public sealed class BlogFeedBackgroundTask  : IBackgroundTask
    {
        public async void Run( IBackgroundTaskInstance taskInstance )
        {
            // Get a deferral, to prevent the task from closing prematurely
            // while asynchronous code is still running.
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();

            // Download the feed.
            var feed = await GetMSDNBlogFeed();

            // Update the live tile with the feed items.
            UpdateTile( feed );

            // Inform the system that the task is finished.
            deferral.Complete();
        }

        private static async Task<SyndicationFeed> GetMSDNBlogFeed()
        {
            SyndicationFeed feed = null;

            try
            {
                // Create a syndication client that downloads the feed.  
                SyndicationClient client = new SyndicationClient();
                client.BypassCacheOnRetrieve = true;
                client.SetRequestHeader( customHeaderName, customHeaderValue );

                // Download the feed.
                feed = await client.RetrieveFeedAsync( new Uri( feedUrl ) );
            }
            catch( Exception ex )
            {
                Debug.WriteLine( ex.ToString() );
            }

            return feed;
        }

        private static void UpdateTile( SyndicationFeed feed )
        {
            // Create a tile update manager for the specified syndication feed.
            var updater = TileUpdateManager.CreateTileUpdaterForApplication();
            updater.EnableNotificationQueue( true );
            updater.Clear();

            // Keep track of the number feed items that get tile notifications.
            int itemCount = 0;

            // Create a tile notification for each feed item.
            foreach( var item in feed.Items )
            {
                XmlDocument tileXml = TileUpdateManager.GetTemplateContent( TileTemplateType.TileWideText03 );

                var title = item.Title;
                string titleText = title.Text == null ? String.Empty : title.Text;
                tileXml.GetElementsByTagName( textElementName )[0].InnerText = titleText;

                // Create a new tile notification.
                updater.Update( new TileNotification( tileXml ) );

                // Don't create more than 5 notifications.
                if( itemCount++ > 5 ) break;
            }
        }

        // Although most HTTP servers do not require User-Agent header, others will reject the request or return
        // a different response if this header is missing. Use SetRequestHeader() to add custom headers.
        static string customHeaderName = "User-Agent";
        static string customHeaderValue = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";

        static string textElementName = "text";
        static string feedUrl = @"http://blogs.msdn.com/b/MainFeed.aspx?Type=BlogsOnly";
    }
}
```

## <a name="set-up-the-package-manifest"></a>Configurar el manifiesto del paquete


Para configurar el manifiesto del paquete, ábrelo y agrégale una nueva declaración de tarea en segundo plano. Establece el punto de entrada de la tarea en el nombre de la clase, incluido su espacio de nombres.

1.  Abre "Package.appxmanifest" en el Explorador de soluciones.
2.  Haz clic o pulsa en la pestaña **Declaraciones** .
3.  En **Declaraciones disponibles**, selecciona **BackgroundTasks** y haz clic en **Agregar**. Visual Studio agrega **BackgroundTasks** en **Declaraciones admitidas**.
4.  En **Tipos de tareas admitidos**, asegúrate de que esté activado **Temporizador**.
5.  En **Configuración de la aplicación**, establece el punto de entrada en **BackgroundTasks.BlogFeedBackgroundTask**.
6.  Haz clic o pulsa en la pestaña **IU de la aplicación**.
7.  Establece **Notificaciones de pantallas de bloqueo** en **Distintivo y texto de imagen**.
8.  Establece una ruta de acceso en un icono de 24 x 24 píxeles en el campo **Logotipo del distintivo** .
    **Importante**este icono debe usar únicamente píxeles transparentes y monocromáticos.
9.  En el campo **Logotipo pequeño**, establece una ruta de acceso en un icono de 30 x 30 píxeles.
10. En el campo **Logotipo ancho** , establece una ruta de acceso a un icono de 310 x 150 píxeles.

## <a name="register-the-background-task"></a>Registrar la tarea en segundo plano


Crea un [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) para registrar tu tarea.

> **Nota**a partir de Windows8.1, los parámetros de registro de tareas en segundo plano se validan en el momento del registro. Se devuelve un error si cualquiera de los parámetros de registro no es válido. La aplicación debe poder enfrentarse a los escenarios en que se produce un error en el registro de tareas en segundo plano. Por ejemplo, usa una instrucción condicional para comprobar si hay errores de registro y después vuelve a probar el registro con errores con valores de parámetros diferentes.
 

En la página principal de tu aplicación, agrega el método **RegisterBackgroundTask** y llámalo en el controlador de eventos **OnNavigatedTo**.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Syndication;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234238

namespace ContosoApp
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.  The Parameter
        /// property is typically used to configure the page.</param>
        protected override void OnNavigatedTo( NavigationEventArgs e )
        {
            this.RegisterBackgroundTask();
        }


        private async void RegisterBackgroundTask()
        {
            var backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();
            if( backgroundAccessStatus == BackgroundAccessStatus.AllowedMayUseActiveRealTimeConnectivity ||
                backgroundAccessStatus == BackgroundAccessStatus.AllowedWithAlwaysOnRealTimeConnectivity )
            {
                foreach( var task in BackgroundTaskRegistration.AllTasks )
                {
                    if( task.Value.Name == taskName )
                    {
                        task.Value.Unregister( true );
                    }
                }

                BackgroundTaskBuilder taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = taskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger( new TimeTrigger( 15, false ) );
                var registration = taskBuilder.Register();
            }
        }

        private const string taskName = "BlogFeedBackgroundTask";
        private const string taskEntryPoint = "BackgroundTasks.BlogFeedBackgroundTask";
    }
}
```

## <a name="debug-the-background-task"></a>Depurar la tarea en segundo plano


Para depurar la tarea en segundo plano, establece un punto de interrupción en el método Run de la tarea. En la barra de herramientas **Ubicación de depuración**, selecciona tu tarea en segundo plano. Esto hace que el sistema llame al método Run inmediatamente.

1.  Establece un punto de interrupción en el método Run de la tarea.
2.  Presiona F5 o pulsa **Depurar &gt; Iniciar depuración** para implementar y ejecutar la aplicación.
3.  Una vez iniciada la aplicación, vuelve a Visual Studio.
4.  Asegúrate de que la barra de herramientas **Ubicación de depuración** aparezca visible. Se encuentra en el menú **Ver &gt; Barras de herramientas**.
5.  En la barra de herramientas **Ubicación de depuración** , haz clic en la lista desplegable **Suspender** y selecciona **BlogFeedBackgroundTask**.
6.  Visual Studio suspende la ejecución en el punto de interrupción.
7.  Presiona F5 o pulsa **Depurar &gt; Continuar** para continuar con la ejecución de la aplicación.
8.  Presiona Mayús+F5 o pulsa **Depurar &gt; Detener depuración** para detener la depuración.
9.  Regresa al icono de la aplicación en la pantalla Inicio. Tras unos pocos segundos, aparecerán notificaciones de icono en el icono de tu aplicación.

## <a name="related-topics"></a>Temas relacionados


* [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
* [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622)
* [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616)
* [Dar soporte a una aplicación con tareas en segundo plano](support-your-app-with-background-tasks.md)
* [Directrices y lista de comprobación de iconos y distintivos](https://msdn.microsoft.com/library/windows/apps/hh465403)

 

 
