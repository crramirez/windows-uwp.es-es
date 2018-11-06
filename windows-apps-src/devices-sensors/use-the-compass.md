---
author: muhsinking
ms.assetid: 5B30E32F-27E0-4656-A834-391A559AC8BC
title: Usar la brújula
description: Aprende a usar la brújula para determinar el rumbo actual.
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4af6b0fb339ba1fde3ea94f456eac98be8a1db9b
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "6051579"
---
# <a name="use-the-compass"></a>Usar la brújula


**API importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Brújula**](https://msdn.microsoft.com/library/windows/apps/BR225705)

**Muestra**

-   Para ver una implementación más completa, consulta la [muestra de brújula](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass).

Aprende a usar la brújula para determinar el rumbo actual.

Una aplicación puede devolver la orientación actual con respecto al norte magnético o verdadero. Las aplicaciones de navegación usan la brújula para determinar la dirección de un dispositivo y orientar el mapa según corresponda.

## <a name="prerequisites"></a>Requisitos previos

Debes estar familiarizado con el lenguaje de marcado de aplicaciones Extensible (XAML), Microsoft VisualC # y eventos.

El dispositivo o emulador que estés usando debe tener una brújula.

## <a name="create-a-simple-compass-app"></a>Crear una aplicación de brújula simple

Esta sección se divide en dos subsecciones: En la primera subsección, conocerás los pasos necesarios para crear una aplicación de brújula simple desde cero. En la siguiente subsección se aplica la aplicación que acabas de crear.

### <a name="instructions"></a>Instrucciones

-   Crea un nuevo proyecto. Para ello, elige una **Aplicación vacía (Windows universal)** en las plantillas de proyecto **Visual C#**.

-   Abre el archivo MainPage.xaml.cs del proyecto y reemplaza el código existente con lo siguiente.

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the compass


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Compass _compass; // Our app' s compass object

            // This event handler writes the current compass reading to
            // the textblocks on the app' s main page.

            private async void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
            {
               await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    CompassReading reading = e.Reading;
                    txtMagnetic.Text = String.Format("{0,5:0.00}", reading.HeadingMagneticNorth);
                    if (reading.HeadingTrueNorth.HasValue)
                        txtNorth.Text = String.Format("{0,5:0.00}", reading.HeadingTrueNorth);
                    else
                        txtNorth.Text = "No reading.";
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
               _compass = Compass.GetDefault(); // Get the default compass object

                // Assign an event handler for the compass reading-changed event
                if (_compass != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _compass.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _compass.ReportInterval = reportInterval;
                    _compass.ReadingChanged += new TypedEventHandler<Compass, CompassReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
    ```

You'll need to rename the namespace in the previous snippet with the name you gave your project. For example, if you created a project named **CompassCS**, you'd replace `namespace App1` with `namespace CompassCS`.

-   Open the file MainPage.xaml and replace the original contents with the following XML.

```xml
        <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
            <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
            <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>

         </Grid>
    </Page>
```

Deberás reemplazar la primera parte del nombre de la clase en el fragmento anterior por el espacio de nombres de tu aplicación. Por ejemplo, si creaste un proyecto denominado **CompassCS**, reemplazarías `x:Class="App1.MainPage"` por `x:Class="CompassCS.MainPage"`. También deberás reemplazar `xmlns:local="using:App1"` por `xmlns:local="using:CompassCS"`.

-   Presiona F5 (o selecciona **Depurar** > **Iniciar depuración**) para crear, implementar y ejecutar la aplicación.

Con la aplicación en ejecución, puedes cambiar los valores de brújula moviendo el dispositivo o usando herramientas del emulador.

-   Detén la aplicación. Para ello, vuelve a Visual Studio y presiona Mayús+F5 o selecciona **Depurar** > **Detener depuración** para detener la aplicación.

### <a name="explanation"></a>Explicación

En el ejemplo anterior, se muestra el poco código que debes escribir para poder integrar los datos de entrada de la brújula en la aplicación.

La aplicación establece una conexión con la brújula predeterminada en el método **MainPage**.

```csharp
_compass = Compass.GetDefault(); // Get the default compass object
```

La aplicación establece el intervalo de informes en el método **MainPage**. Este código recupera el intervalo mínimo admitido por el dispositivo y lo compara con un intervalo solicitado de 16 milisegundos (el cual se aproxima a una frecuencia de actualización de 60 Hz). Si el intervalo mínimo admitido es mayor que el intervalo solicitado, el código establece el valor en el mínimo. De lo contrario, establece el valor en el intervalo solicitado.

```csharp
uint minReportInterval = _compass.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_compass.ReportInterval = reportInterval;
```

Los nuevos datos de la brújula se capturan en el método **ReadingChanged**. Cada vez que el controlador del sensor recibe nuevos datos del sensor, pasa los valores a la aplicación mediante este controlador de eventos. La aplicación registra este controlador de eventos en la siguiente línea.

```csharp
_compass.ReadingChanged += new TypedEventHandler<Compass,
CompassReadingChangedEventArgs>(ReadingChanged);
```

Estos nuevos valores se escriben en los TextBlocks que se encuentran en el código XAML del proyecto.

```xml
 <TextBlock HorizontalAlignment="Left" Height="22" Margin="8,18,0,0" TextWrapping="Wrap" Text="Magnetic Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFFBF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="18" Margin="8,58,0,0" TextWrapping="Wrap" Text="True North Heading:" VerticalAlignment="Top" Width="104" Foreground="#FFF3F3F3"/>
 <TextBlock x:Name="txtMagnetic" HorizontalAlignment="Left" Height="22" Margin="130,18,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFFBF6F6"/>
 <TextBlock x:Name="txtNorth" HorizontalAlignment="Left" Height="18" Margin="130,58,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="116" Foreground="#FFF5F1F1"/>
```
 

 
