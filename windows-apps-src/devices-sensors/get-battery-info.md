---
ms.assetid: 90BB59FC-90FE-453E-A8DE-9315E29EB98C
title: Obtener información sobre la batería
description: Aprende a obtener información detallada sobre la batería mediante las API del espacio de nombres Windows.Devices.Power.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 81f4232d038b89f2c49cf584346d632911fb70e2
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8695254"
---
# <a name="get-battery-information"></a>Obtener información sobre la batería


** API importantes **

-   [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017)
-   [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432)

Aprende a obtener información detallada sobre la batería mediante las API del espacio de nombres [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017). Un *informe de batería* ([**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)) describe la carga, la capacidad y el estado de la batería o agregado de baterías. Este tema muestra cómo tu aplicación puede obtener informes de batería y recibir notificaciones de cambios. Los ejemplos de código provienen de la aplicación de batería básica que aparece al final de este tema.

## <a name="get-aggregate-battery-report"></a>Obtener un informe de un conjunto de baterías


Algunos dispositivos tienen más de una batería y no siempre resulta obvio saber cómo contribuye cada una de ellas a la capacidad de energía global del dispositivo. Aquí es donde entra en acción la clase [**AggregateBattery**](https://msdn.microsoft.com/library/windows/apps/Dn895011). El *agregado de baterías* representa todos los controladores de la batería conectados al dispositivo y puede proporcionar un único objeto [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) general.

**Nota**una clase de [**batería**](https://msdn.microsoft.com/library/windows/apps/Dn895004) realmente corresponde a un controlador de batería. Según el dispositivo, unas veces se adjunta el controlador a la batería física y otras a la carcasa del dispositivo. Por tanto, es posible crear un objeto de batería, incluso cuando no hay baterías presentes. Otras veces, el objeto de batería puede ser **null**.

Una vez tengas un objeto del agregado de batería, llama a [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) para obtener la clase [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) correspondiente.

```csharp
private void RequestAggregateBatteryReport()
{
    // Create aggregate battery object
    var aggBattery = Battery.AggregateBattery;

    // Get report
    var report = aggBattery.GetReport();

    // Update UI
    AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
}
```

## <a name="get-individual-battery-reports"></a>Obtener informes de batería individuales

También puedes crear un objeto [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) para baterías individuales. Usa [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getdeviceselector.aspx) con el método [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432) para obtener una colección de objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) que representen los controladores de la batería que están conectados al dispositivo. A continuación, mediante la propiedad **Id** del objeto **DeviceInformation** que quieras usar, crea la clase [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) correspondiente con el método [**FromIdAsyn**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.fromidasync.aspx). Por último, llama a [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) para obtener el informe de batería.

Este ejemplo muestra cómo crear un informe de batería para todas las baterías conectadas al dispositivo.

```csharp
async private void RequestIndividualBatteryReports()
{
    // Find batteries 
    var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
    foreach(DeviceInformation device in deviceInfo)
    {
        try
        {
        // Create battery object
        var battery = await Battery.FromIdAsync(device.Id);

        // Get report
        var report = battery.GetReport();

        // Update UI
        AddReportUI(BatteryReportPanel, report, battery.DeviceId);
        }
        catch { /* Add error handling, as applicable */ }
    }
}
```

## <a name="access-report-details"></a>Detalles del informe de acceso

El objeto [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) proporciona mucha información sobre la batería. Para obtener más información, consulta la referencia de la API acerca de sus propiedades: **Status** (una enumeración [**BatteryStatus**](https://msdn.microsoft.com/library/windows/apps/Dn818458)), [**ChargeRateInMilliwatts**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.chargerateinmilliwatts.aspx), [**DesignCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.designcapacityinmilliwatthours.aspx), [**FullChargeCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.fullchargecapacityinmilliwatthours.aspx) y [**RemainingCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.remainingcapacityinmilliwatthours). Este ejemplo muestra algunas de las propiedades del informe de batería que usa la aplicación de batería básica, la cual se proporciona más adelante en este tema.

```csharp
...
TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };
...
...
```

## <a name="request-report-updates"></a>Solicitar actualizaciones de informe

El objeto [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) desencadena el evento [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) cuando cambia el estado de la batería, la capacidad o la carga. Generalmente esto sucede de forma inmediata para los cambios de estado y periódicamente para los demás cambios. Este ejemplo muestra cómo registrarse para recibir las actualizaciones del informe de batería.

```csharp
...
Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
...
```

## <a name="handle-report-updates"></a>Controlar las actualizaciones del informe

Cuando se produce una actualización de la batería, el evento [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) pasa el correspondiente objeto [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) al método del controlador de eventos. Sin embargo, no se llama al controlador de eventos desde el subproceso de la interfaz de usuario. Tendrás que usar el objeto [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) para invocar cualquier cambio en la interfaz de usuario, tal y como se muestra en este ejemplo.

```csharp
async private void AggregateBattery_ReportUpdated(Battery sender, object args)
{
    if (reportRequested)
    {

        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }
        });
    }
}
```

## <a name="example-basic-battery-app"></a>Ejemplo: aplicación de batería básica

Prueba estas API mediante la creación de la siguiente aplicación de batería básica en Microsoft Visual Studio. En la página de inicio de Visual Studio, haz clic en **Nuevo proyecto** y, a continuación, en las plantillas **Visual C# &gt; Windows &gt; Universal**; crea una nueva aplicación usando la plantilla **Aplicación vacía**.

Después abre el archivo **MainPage.xaml** y copia el siguiente XML en el archivo (y reemplaza su contenido original).

```xml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" >
        <StackPanel VerticalAlignment="Center" Margin="15,30,0,0" >
            <RadioButton x:Name="AggregateButton" Content="Aggregate results" GroupName="Type" IsChecked="True" />
            <RadioButton x:Name="IndividualButton" Content="Individual results" GroupName="Type" IsChecked="False" />
        </StackPanel>
        <StackPanel Orientation="Horizontal">
        <Button x:Name="GetBatteryReportButton" 
                Content="Get battery report" 
                Margin="15,15,0,0" 
                Click="GetBatteryReport"/>
        </StackPanel>
        <StackPanel x:Name="BatteryReportPanel" Margin="15,15,0,0"/>
    </StackPanel>
</Page>
```

Si el nombre de la aplicación no es **App1**, deberás reemplazar la primera parte del nombre de la clase en el fragmento anterior por el espacio de nombres de tu aplicación. Por ejemplo, si creaste un proyecto denominado **BasicBatteryApp**, reemplazarías `x:Class="App1.MainPage"` por `x:Class="BasicBatteryApp.MainPage"`. También deberás reemplazar `xmlns:local="using:App1"` por `xmlns:local="using:BasicBatteryApp"`.

A continuación, abre el archivo **MainPage.xaml.cs** del proyecto y reemplaza el código existente por el siguiente.

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Devices.Enumeration;
using Windows.Devices.Power;
using Windows.UI.Core;

namespace App1
{
    public sealed partial class MainPage : Page
    {
        bool reportRequested = false;
        public MainPage()
        {
            this.InitializeComponent();
            Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
        }


        private void GetBatteryReport(object sender, RoutedEventArgs e)
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }

            // Note request
            reportRequested = true;
        }

        private void RequestAggregateBatteryReport()
        {
            // Create aggregate battery object
            var aggBattery = Battery.AggregateBattery;

            // Get report
            var report = aggBattery.GetReport();

            // Update UI
            AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
        }

        async private void RequestIndividualBatteryReports()
        {
            // Find batteries 
            var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
            foreach(DeviceInformation device in deviceInfo)
            {
                try
                {
                // Create battery object
                var battery = await Battery.FromIdAsync(device.Id);

                // Get report
                var report = battery.GetReport();

                // Update UI
                AddReportUI(BatteryReportPanel, report, battery.DeviceId);
                }
                catch { /* Add error handling, as applicable */ }
            }
        }


        private void AddReportUI(StackPanel sp, BatteryReport report, string DeviceID)
        {
            // Create battery report UI
            TextBlock txt1 = new TextBlock { Text = "Device ID: " + DeviceID };
            txt1.FontSize = 15;
            txt1.Margin = new Thickness(0, 15, 0, 0);
            txt1.TextWrapping = TextWrapping.WrapWholeWords;

            TextBlock txt2 = new TextBlock { Text = "Battery status: " + report.Status.ToString() };
            txt2.FontStyle = Windows.UI.Text.FontStyle.Italic;
            txt2.Margin = new Thickness(0, 0, 0, 15);

            TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
            TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
            TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
            TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };

            // Create energy capacity progress bar & labels
            TextBlock pbLabel = new TextBlock { Text = "Percent remaining energy capacity" };
            pbLabel.Margin = new Thickness(0,10, 0, 5);
            pbLabel.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            ProgressBar pb = new ProgressBar();
            pb.Margin = new Thickness(0, 5, 0, 0);
            pb.Width = 200;
            pb.Height = 10;
            pb.IsIndeterminate = false;
            pb.HorizontalAlignment = HorizontalAlignment.Left;

            TextBlock pbPercent = new TextBlock();
            pbPercent.Margin = new Thickness(0, 5, 0, 10);
            pbPercent.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            // Disable progress bar if values are null
            if ((report.FullChargeCapacityInMilliwattHours == null)||
                (report.RemainingCapacityInMilliwattHours == null))
            {
                pb.IsEnabled = false;
                pbPercent.Text = "N/A";
            }
            else
            {
                pb.IsEnabled = true;
                pb.Maximum = Convert.ToDouble(report.FullChargeCapacityInMilliwattHours);
                pb.Value = Convert.ToDouble(report.RemainingCapacityInMilliwattHours);
                pbPercent.Text = ((pb.Value / pb.Maximum) * 100).ToString("F2") + "%";
            }

            // Add controls to stackpanel
            sp.Children.Add(txt1);
            sp.Children.Add(txt2);
            sp.Children.Add(txt3);
            sp.Children.Add(txt4);
            sp.Children.Add(txt5);
            sp.Children.Add(txt6);
            sp.Children.Add(pbLabel);
            sp.Children.Add(pb);
            sp.Children.Add(pbPercent);
        }

        async private void AggregateBattery_ReportUpdated(Battery sender, object args)
        {
            if (reportRequested)
            {

                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    // Clear UI
                    BatteryReportPanel.Children.Clear();


                    if (AggregateButton.IsChecked == true)
                    {
                        // Request aggregate battery report
                        RequestAggregateBatteryReport();
                    }
                    else
                    {
                        // Request individual battery report
                        RequestIndividualBatteryReports();
                    }
                });
            }
        }
    }
}
```

Si la aplicación no se llama **App1**, tendrás que cambiar el nombre del espacio de nombres en el ejemplo anterior por el nombre de tu proyecto. Por ejemplo, si creaste un proyecto denominado **BasicBatteryApp**, reemplazarías el espacio de nombres `App1` por `BasicBatteryApp`.

Por último, para ejecutar esta aplicación de batería básica: en el menú **Depurar**, haz clic en **Iniciar depuración** para probar la solución.

**Sugerencia**para recibir los valores numéricos del objeto [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) , depurar la aplicación en el **Equipo Local** o un **dispositivo** externo (por ejemplo, un Windows Phone). Al depurar en un emulador de dispositivo, el objeto **BatteryReport** devuelve **null** a las propiedades de capacidad y velocidad.

 

