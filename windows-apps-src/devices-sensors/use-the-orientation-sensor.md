---
ms.assetid: 1889AC3A-A472-4294-89B8-A642668A8A6E
title: Usar el sensor de orientación
description: Aprende a usar los sensores de orientación para determinar la orientación del dispositivo.
ms.date: 06/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4426cbc2e2d3c6e7d980b0733b6deb5178025abb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624100"
---
# <a name="use-the-orientation-sensor"></a>Usar el sensor de orientación


**API importantes**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371)
-   [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399)

**Ejemplos**

-   [Ejemplo del sensor de orientación](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor)
-   [Ejemplo del sensor de orientación simple](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor)

Aprende a usar los sensores de orientación para determinar la orientación del dispositivo.

Hay dos tipos diferentes de las API que se incluyen en el sensor de orientación del [ **Windows.Devices.Sensors** ](https://msdn.microsoft.com/library/windows/apps/BR206408) espacio de nombres: [**OrientationSensor** ](https://msdn.microsoft.com/library/windows/apps/BR206371) y [ **SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399). Si bien ambos sensores son sensores de orientación, ese término está sobrecargado y se usan para fines muy diferentes. Sin embargo, dado que ambos son sensores de orientación, se tratan en este artículo.

La API de [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) se usa para aplicaciones 3D para obtener un cuaternión y una matriz de rotación. Un cuaternión puede entender más fácilmente como un giro de un punto de \[x, y, z\] alrededor de un eje arbitrario (contrastar con una matriz de rotación, que representa las rotaciones en torno a tres ejes). Las operaciones matemáticas en torno a los cuaterniones son bastante complejas, ya que implican las propiedades geométricas de números complejos y las propiedades matemáticas de números imaginarios. Sin embargo, es fácil trabajar con ellas y son compatibles con marcos como DirectX. Una aplicación compleja en 3D puede usar el sensor de orientación para ajustar la perspectiva del usuario. Este sensor combina los datos de entrada del acelerómetro, el girómetro y la brújula.

La API de [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/BR206399) se usa para determinar la orientación actual del dispositivo en cuanto a las definiciones, como vertical hacia arriba, vertical hacia abajo, horizontal hacia la izquierda y horizontal hacia la derecha. También puede detectar si un dispositivo está boca arriba o boca abajo. En lugar de devolver propiedades como "vertical arriba" u "horizontal izquierda", este sensor devuelve un valor de rotación: "No gira", "Rotated90DegreesCounterclockwise" y así sucesivamente. En la siguiente tabla, se asignan las propiedades de orientación comunes a la lectura del sensor correspondiente.

| Orientación     | Lectura del sensor correspondiente      |
|-----------------|-----------------------------------|
| Vertical hacia arriba     | NotRotated                        |
| Horizontal hacia la izquierda  | Rotated90DegreesCounterclockwise  |
| Vertical hacia abajo   | Rotated180DegreesCounterclockwise |
| Horizontal hacia la derecha | Rotated270DegreesCounterclockwise |

## <a name="prerequisites"></a>Requisitos previos

Debe estar familiarizado con Extensible Application Markup Language (XAML), Microsoft Visual C#y eventos.

El dispositivo o emulador que estés usando debe tener un sensor de orientación.

## <a name="create-an-orientationsensor-app"></a>Crear una aplicación de sensor de orientación

Esta sección se divide en dos subsecciones: En la primera subsección, conocerás los pasos necesarios para crear una aplicación de orientación desde cero. En la siguiente subsección se aplica la aplicación que acabas de crear.

###  <a name="instructions"></a>Instrucciones

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

    // The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private OrientationSensor _sensor;

            private async void ReadingChanged(object sender, OrientationSensorReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    OrientationSensorReading reading = e.Reading;

                    // Quaternion values
                    txtQuaternionX.Text = String.Format("{0,8:0.00000}", reading.Quaternion.X);
                    txtQuaternionY.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Y);
                    txtQuaternionZ.Text = String.Format("{0,8:0.00000}", reading.Quaternion.Z);
                    txtQuaternionW.Text = String.Format("{0,8:0.00000}", reading.Quaternion.W);

                    // Rotation Matrix values
                    txtM11.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M11);
                    txtM12.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M12);
                    txtM13.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M13);
                    txtM21.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M21);
                    txtM22.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M22);
                    txtM23.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M23);
                    txtM31.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M31);
                    txtM32.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M32);
                    txtM33.Text = String.Format("{0,8:0.00000}", reading.RotationMatrix.M33);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _sensor = OrientationSensor.GetDefault();

                // Establish the report interval for all scenarios
                uint minReportInterval = _sensor.MinimumReportInterval;
                uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                _sensor.ReportInterval = reportInterval;

                // Establish event handler
                _sensor.ReadingChanged += new TypedEventHandler<OrientationSensor, OrientationSensorReadingChangedEventArgs>(ReadingChanged);
            }
        }
    }
```

Tendrás que cambiar el nombre del espacio de nombres del fragmento de código anterior por el nombre de tu proyecto. Por ejemplo, si creaste un proyecto denominado **OrientationSensorCS**, reemplazarías `namespace App1` por `namespace OrientationSensorCS`.

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

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,4,0,0" TextWrapping="Wrap" Text="M11:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,36,0,0" TextWrapping="Wrap" Text="M12:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,72,0,0" TextWrapping="Wrap" Text="M13:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="31" Margin="4,118,0,0" TextWrapping="Wrap" Text="M21:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="4,160,0,0" TextWrapping="Wrap" Text="M22:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,201,0,0" TextWrapping="Wrap" Text="M23:" VerticalAlignment="Top" Width="35"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="4,234,0,0" TextWrapping="Wrap" Text="M31:" VerticalAlignment="Top" Width="39"/>
            <TextBlock HorizontalAlignment="Left" Height="28" Margin="4,274,0,0" TextWrapping="Wrap" Text="M32:" VerticalAlignment="Top" Width="46"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="4,322,0,0" TextWrapping="Wrap" Text="M33:" VerticalAlignment="Top" Width="39"/>
            <TextBlock x:Name="txtM11" HorizontalAlignment="Left" Height="19" Margin="43,4,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM12" HorizontalAlignment="Left" Height="23" Margin="43,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM13" HorizontalAlignment="Left" Height="15" Margin="43,72,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM21" HorizontalAlignment="Left" Height="20" Margin="43,114,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM22" HorizontalAlignment="Left" Height="19" Margin="43,156,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM23" HorizontalAlignment="Left" Height="16" Margin="43,197,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM31" HorizontalAlignment="Left" Height="17" Margin="43,230,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM32" HorizontalAlignment="Left" Height="19" Margin="43,270,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock x:Name="txtM33" HorizontalAlignment="Left" Height="21" Margin="43,322,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,8,0,0" TextWrapping="Wrap" Text="Quaternion X:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="194,36,0,0" TextWrapping="Wrap" Text="Quaternion Y:" VerticalAlignment="Top" Width="81"/>
            <TextBlock HorizontalAlignment="Left" Height="15" Margin="194,72,0,0" TextWrapping="Wrap" Text="Quaternion Z:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionX" HorizontalAlignment="Left" Height="15" Margin="279,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="104"/>
            <TextBlock x:Name="txtQuaternionY" HorizontalAlignment="Left" Height="12" Margin="275,36,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="108"/>
            <TextBlock x:Name="txtQuaternionZ" HorizontalAlignment="Left" Height="19" Margin="275,68,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="89"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="194,96,0,0" TextWrapping="Wrap" Text="Quaternion W:" VerticalAlignment="Top" Width="81"/>
            <TextBlock x:Name="txtQuaternionW" HorizontalAlignment="Left" Height="12" Margin="279,96,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="72"/>

        </Grid>
    </Page>
```

Deberás reemplazar la primera parte del nombre de la clase en el fragmento anterior por el espacio de nombres de tu aplicación. Por ejemplo, si creaste un proyecto denominado **OrientationSensorCS**, reemplazarías `x:Class="App1.MainPage"` por `x:Class="OrientationSensorCS.MainPage"`. También deberás reemplazar `xmlns:local="using:App1"` por `xmlns:local="using:OrientationSensorCS"`.

-   Presiona F5 (o selecciona **Depurar** > **Iniciar depuración**) para crear, implementar y ejecutar la aplicación.

Con la aplicación en ejecución, puedes cambiar la orientación moviendo el dispositivo o usando las herramientas del emulador.

-   Detén la aplicación. Para ello, vuelve a Visual Studio y presiona Mayús + F5 o selecciona **Depurar** > **Detener depuración** para detener la aplicación.

###  <a name="explanation"></a>Explicación

En el ejemplo anterior, se muestra el poco código que debes escribir para poder integrar los datos de entrada del sensor de orientación en la aplicación.

La aplicación establece una conexión con el sensor de orientación predeterminado en el método **MainPage**.

```csharp
_sensor = OrientationSensor.GetDefault();
```

La aplicación establece el intervalo de informes en el método **MainPage**. Este código recupera el intervalo mínimo admitido por el dispositivo y lo compara con un intervalo solicitado de 16 milisegundos (el cual se aproxima a una frecuencia de actualización de 60 Hz). Si el intervalo mínimo admitido es mayor que el intervalo solicitado, el código establece el valor en el mínimo. De lo contrario, establece el valor en el intervalo solicitado.

```csharp
uint minReportInterval = _sensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_sensor.ReportInterval = reportInterval;
```

Los nuevos datos del sensor se capturan en el método **ReadingChanged**. Cada vez que el controlador del sensor recibe nuevos datos del sensor, pasa los valores a la aplicación mediante este controlador de eventos. La aplicación registra este controlador de eventos en la siguiente línea.

```csharp
_sensor.ReadingChanged += new TypedEventHandler<OrientationSensor,
OrientationSensorReadingChangedEventArgs>(ReadingChanged);
```

Estos nuevos valores se escriben en los bloques de texto que se encuentran en el código XAML del proyecto.

## <a name="create-a-simpleorientation-app"></a>Crear una aplicación de orientación simple

Esta sección se divide en dos subsecciones: En la primera subsección, conocerás los pasos necesarios para crear una aplicación de orientación simple desde cero. En la siguiente subsección se aplica la aplicación que acabas de crear.

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;
    // The Blank Page item template is documented at https://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private SimpleOrientationSensor _simpleorientation;

            // This event handler writes the current sensor reading to
            // a text block on the app' s main page.

            private async void OrientationChanged(object sender, SimpleOrientationSensorOrientationChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    SimpleOrientation orientation = e.Orientation;
                    switch (orientation)
                    {
                        case SimpleOrientation.NotRotated:
                            txtOrientation.Text = "Not Rotated";
                            break;
                        case SimpleOrientation.Rotated90DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 90 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated180DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 180 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Rotated270DegreesCounterclockwise:
                            txtOrientation.Text = "Rotated 270 Degrees Counterclockwise";
                            break;
                        case SimpleOrientation.Faceup:
                            txtOrientation.Text = "Faceup";
                            break;
                        case SimpleOrientation.Facedown:
                            txtOrientation.Text = "Facedown";
                            break;
                        default:
                            txtOrientation.Text = "Unknown orientation";
                            break;
                    }
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _simpleorientation = SimpleOrientationSensor.GetDefault();

                // Assign an event handler for the sensor orientation-changed event
                if (_simpleorientation != null)
                {
                    _simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor, SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
                }
            }
        }
    }
```

Tendrás que cambiar el nombre del espacio de nombres del fragmento de código anterior por el nombre de tu proyecto. Por ejemplo, si creaste un proyecto denominado **SimpleOrientationCS**, reemplazarías `namespace App1` por `namespace SimpleOrientationCS`.

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
            <TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
            <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>

        </Grid>
    </Page>
```

Deberás reemplazar la primera parte del nombre de la clase en el fragmento anterior por el espacio de nombres de tu aplicación. Por ejemplo, si creaste un proyecto denominado **SimpleOrientationCS**, reemplazarías `x:Class="App1.MainPage"` por `x:Class="SimpleOrientationCS.MainPage"`. También deberás reemplazar `xmlns:local="using:App1"` por `xmlns:local="using:SimpleOrientationCS"`.

-   Presiona F5 (o selecciona **Depurar** > **Iniciar depuración**) para crear, implementar y ejecutar la aplicación.

Con la aplicación en ejecución, puedes cambiar la orientación moviendo el dispositivo o usando las herramientas del emulador.

-   Detén la aplicación. Para ello, vuelve a Visual Studio y presiona Mayús + F5 o selecciona **Depurar** > **Detener depuración** para detener la aplicación.

### <a name="explanation"></a>Explicación

En el ejemplo anterior se muestra el poco código que debes escribir para poder integrar los datos de entrada del sensor de orientación simple en la aplicación.

La aplicación establece una conexión con el sensor predeterminado en el método **MainPage**.

```csharp
_simpleorientation = SimpleOrientationSensor.GetDefault();
```

Los nuevos datos del sensor se capturan en el método **OrientationChanged**. Cada vez que el controlador del sensor recibe nuevos datos del sensor, pasa los valores a la aplicación mediante este controlador de eventos. La aplicación registra este controlador de eventos en la siguiente línea.

```csharp
_simpleorientation.OrientationChanged += new TypedEventHandler<SimpleOrientationSensor,
SimpleOrientationSensorOrientationChangedEventArgs>(OrientationChanged);
```

Estos nuevos valores se escriben en un bloque de texto que se encuentra en el código XAML del proyecto.

```csharp
<TextBlock HorizontalAlignment="Left" Height="24" Margin="8,8,0,0" TextWrapping="Wrap" Text="Current Orientation:" VerticalAlignment="Top" Width="101" Foreground="#FFF8F7F7"/>
 <TextBlock x:Name="txtOrientation" HorizontalAlignment="Left" Height="24" Margin="118,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="175" Foreground="#FFFEFAFA"/>
```