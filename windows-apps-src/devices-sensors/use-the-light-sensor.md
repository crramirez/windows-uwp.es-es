---
author: muhsinking
ms.assetid: 15BAB25C-DA8C-4F13-9B8F-EA9E4270BCE9
title: Usar el sensor de luz
description: Aprende a usar el sensor de luz ambiental para detectar cambios de iluminación.
ms.author: mukin
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e6f7f838e5640f873593ac2e08c6a9b30f5258e5
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5987069"
---
# <a name="use-the-light-sensor"></a>Usar el sensor de luz


**API importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**LightSensor**](https://msdn.microsoft.com/library/windows/apps/BR225790)

**Muestra**

-   Para ver una implementación más completa, consulta la [muestra de sensor de luz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor).

Aprende a usar el sensor de luz ambiental para detectar cambios de iluminación.

El sensor de luz ambiental es uno de los distintos tipos de sensores ambientales que permiten que las aplicaciones respondan a los cambios en el entorno del usuario.

## <a name="prerequisites"></a>Requisitos previos

Debes estar familiarizado con el lenguaje de marcado de aplicaciones Extensible (XAML), Microsoft VisualC # y eventos.

El dispositivo o emulador que estés usando debe ser compatible con un sensor de luz ambiental.

## <a name="create-a-simple-light-sensor-app"></a>Crear una aplicación de sensor de luz simple

Esta sección se divide en dos subsecciones: En la primera subsección, conocerás los pasos necesarios para crear una aplicación de sensor de luz simple desde cero. En la siguiente subsección se aplica la aplicación que acabas de crear.

###  <a name="instructions"></a>Instrucciones

-   Crea un nuevo proyecto. Para ello, elige una **Aplicación vacía (Windows universal)** en las plantillas de proyecto **Visual C#**.

-   Abre el archivo BlankPage.xaml.cs del proyecto y reemplaza el código existente con lo siguiente.

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
    using Windows.Devices.Sensors; // Required to access the sensor platform and the ALS

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class BlankPage : Page
        {
            private LightSensor _lightsensor; // Our app' s lightsensor object

            // This event handler writes the current light-sensor reading to
            // the textbox named "txtLUX" on the app' s main page.

            private void ReadingChanged(object sender, LightSensorReadingChangedEventArgs e)
            {
                Dispatcher.RunAsync(CoreDispatcherPriority.Normal, (s, a) =>
                {
                    LightSensorReading reading = (a.Context as LightSensorReadingChangedEventArgs).Reading;
                    txtLuxValue.Text = String.Format("{0,5:0.00}", reading.IlluminanceInLux);
                });
            }

            public BlankPage()
            {
                InitializeComponent();
                _lightsensor = LightSensor.GetDefault(); // Get the default light sensor object

                // Assign an event handler for the ALS reading-changed event
                if (_lightsensor != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _lightsensor.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _lightsensor.ReportInterval = reportInterval;

                    // Establish the even thandler
                    _lightsensor.ReadingChanged += new TypedEventHandler<LightSensor, LightSensorReadingChangedEventArgs>(ReadingChanged);
                }

            }

        }
    }
```

Tendrás que cambiar el nombre del espacio de nombres del fragmento de código anterior por el nombre de tu proyecto. Por ejemplo, si creaste un proyecto denominado **LightingCS**, reemplazarías `namespace App1` por `namespace LightingCS`.

-   Abre el archivo MainPage.xaml y reemplaza el contenido original por el siguiente código XML.

```xml
    <Page
        x:Class="App1.BlankPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
            <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>


        </Grid>

    </Page>
```

Deberás reemplazar la primera parte del nombre de la clase en el fragmento anterior por el espacio de nombres de tu aplicación. Por ejemplo, si creaste un proyecto denominado **LightingCS**, reemplazarías `x:Class="App1.MainPage"` por `x:Class="LightingCS.MainPage"`. También deberás reemplazar `xmlns:local="using:App1"` por `xmlns:local="using:LightingCS"`.

-   Presiona F5 (o selecciona **Depurar** > **Iniciar depuración**) para crear, implementar y ejecutar la aplicación.

Con la aplicación en ejecución, puedes cambiar los valores del sensor de luz al cambiar la luz disponibles para el sensor o usando herramientas del emulador.

-   Detén la aplicación. Para ello, vuelve a Visual Studio y presiona Mayús+F5 o selecciona **Depurar** > **Detener depuración** para detener la aplicación.

###  <a name="explanation"></a>Explicación

En el ejemplo anterior, se muestra el poco código debes escribir para poder integrar los datos de entrada del sensor de luz en la aplicación.

La aplicación establece una conexión con el sensor de orientación predeterminado en el método **BlankPage**.

```csharp
_lightsensor = LightSensor.GetDefault(); // Get the default light sensor object
```

La aplicación establece el intervalo de informes en el método **BlankPage**. Este código recupera el intervalo mínimo admitido por el dispositivo y lo compara con un intervalo solicitado de 16 milisegundos (el cual se aproxima a una frecuencia de actualización de 60 Hz). Si el intervalo mínimo admitido es mayor que el intervalo solicitado, el código establece el valor en el mínimo. De lo contrario, establece el valor en el intervalo solicitado.

```csharp
uint minReportInterval = _lightsensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_lightsensor.ReportInterval = reportInterval;
```
Los nuevos datos del sensor de luz se capturan en el método **ReadingChanged**. Cada vez que el controlador del sensor recibe nuevos datos del sensor, este pasa los valores a la aplicación mediante este controlador de eventos. La aplicación registra este controlador de eventos en la siguiente línea.

```csharp
_lightsensor.ReadingChanged += new TypedEventHandler<LightSensor,
LightSensorReadingChangedEventArgs>(ReadingChanged);
```

Estos nuevos valores se escriben en un TextBlock que se encuentra en el código XAML del proyecto.

```xml
<TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
 <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>
```