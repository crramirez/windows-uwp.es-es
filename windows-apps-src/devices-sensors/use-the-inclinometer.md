---
author: DBirtolo
ms.assetid: 16AD53CA-1252-456C-8567-2263D3EC95F3
title: "Usar el inclinómetro"
description: "Aprende a usar el inclinómetro para determinar la rotación alrededor del eje X (pitch), la rotación alrededor del eje y la rotación alrededor del eje Y (yaw)."
translationtype: Human Translation
ms.sourcegitcommit: d403e78b775af0f842ba2172295a09e35015dcc8
ms.openlocfilehash: f8b56e0ef13e471d99519256b5a6d124b569102d

---
# Usar el inclinómetro

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** API importantes **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Inclinómetro**](https://msdn.microsoft.com/library/windows/apps/BR225766)

Aprende a usar el inclinómetro para determinar la rotación alrededor del eje X (pitch), la rotación alrededor del eje y la rotación alrededor del eje Y (yaw).

Algunos juegos en 3D necesitan un inclinómetro como dispositivo de entrada. Un ejemplo común es el simulador de vuelos, que asigna los tres ejes del inclinómetro (X, Y y Z) a los datos de entrada del timón de profundidad, el alerón y el timón de dirección del avión.

 ## Requisitos previos

Debes estar familiarizado con el lenguaje XAML, Microsoft Visual C# y eventos.

El dispositivo o emulador que estés usando debe tener un inclinómetro.

 ## Crear una aplicación simple de inclinómetro

Esta sección se divide en dos subsecciones: En la primera subsección, conocerás los pasos necesarios para crear una aplicación de inclinómetro simple desde cero. En la siguiente subsección se aplica la aplicación que acabas de crear.

###  Instrucciones

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Inclinometer _inclinometer;

            // This event handler writes the current inclinometer reading to 
            // the three text blocks on the app' s main page.

            private async void ReadingChanged(object sender, InclinometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    InclinometerReading reading = e.Reading;
                    txtPitch.Text = String.Format("{0,5:0.00}", reading.PitchDegrees);
                    txtRoll.Text = String.Format("{0,5:0.00}", reading.RollDegrees);
                    txtYaw.Text = String.Format("{0,5:0.00}", reading.YawDegrees);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _inclinometer = Inclinometer.GetDefault();
     

                if (_inclinometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _inclinometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _inclinometer.ReportInterval = reportInterval;

                    // Establish the event handler
                    _inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer, InclinometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

Tendrás que cambiar el nombre del espacio de nombres del fragmento de código anterior por el nombre de tu proyecto. Por ejemplo, si creaste un proyecto denominado **InclinometerCS**, reemplazarías `namespace App1` por `namespace InclinometerCS`.

-   Abre el archivo MainPage.xaml y reemplaza el contenido original por el siguiente código XML.

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
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
            <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
            <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
            <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>

        </Grid>
    </Page>
```

Deberás reemplazar la primera parte del nombre de la clase en el fragmento anterior por el espacio de nombres de tu aplicación. Por ejemplo, si creaste un proyecto denominado **InclinometerCS**, reemplazarías `x:Class="App1.MainPage"` por `x:Class="InclinometerCS.MainPage"`. También deberás reemplazar `xmlns:local="using:App1"` por `xmlns:local="using:InclinometerCS"`.

-   Presiona F5 (o selecciona **Depurar** > **Iniciar depuración**) para crear, implementar y ejecutar la aplicación.

Con la aplicación en ejecución, puedes cambiar los valores de inclinómetro moviendo el dispositivo o usando herramientas del emulador.

-   Detén la aplicación. Para ello, vuelve a Visual Studio y presiona Mayús + F5 o selecciona **Depurar** > **Detener depuración** para detener la aplicación.

###  Explicación

En el ejemplo anterior, se muestra el poco código que debes escribir para poder integrar los datos de entrada del inclinómetro en la aplicación.

La aplicación establece una conexión con el inclinómetro predeterminado en el método **MainPage**.

```csharp
_inclinometer = Inclinometer.GetDefault();
```

La aplicación establece el intervalo de informes en el método **MainPage**. Este código recupera el intervalo mínimo admitido por el dispositivo y lo compara con un intervalo solicitado de 16 milisegundos (el cual se aproxima a una frecuencia de actualización de 60 Hz). Si el intervalo mínimo admitido es mayor que el intervalo solicitado, el código establece el valor en el mínimo. De lo contrario, establece el valor en el intervalo solicitado.

```csharp
uint minReportInterval = _inclinometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_inclinometer.ReportInterval = reportInterval;
```

Los nuevos datos del inclinómetro se capturan en el método **ReadingChanged**. Cada vez que el controlador del sensor recibe nuevos datos del sensor, pasa los valores a la aplicación mediante este controlador de eventos. La aplicación registra este controlador de eventos en la siguiente línea.

```csharp
_inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer, 
InclinometerReadingChangedEventArgs>(ReadingChanged);
```

Estos nuevos valores se escriben en los bloques de texto que se encuentran en el código XAML del proyecto.

```xml
<TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
 <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
 <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
 <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>
```

 ## Temas relacionados

* [Muestra de inclinómetro](http://go.microsoft.com/fwlink/p/?linkid=241380)




<!--HONumber=Jun16_HO4-->


