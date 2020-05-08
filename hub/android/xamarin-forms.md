---
title: Creación de una aplicación Android sencilla con Xamarin. Forms
description: Introducción a la escritura de aplicaciones Android con Xamarin. Forms
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: Android, Windows, Xamarin. Forms, XAML, tutorial
ms.date: 04/28/2020
ms.openlocfilehash: a1426bfef9863227c1ac110bc295536786695df7
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255199"
---
# <a name="get-started-developing-for-android-using-xamarinforms"></a>Introducción al desarrollo para Android con Xamarin. Forms

Esta guía le ayudará a empezar a usar Xamarin. Forms en Windows para crear una aplicación multiplataforma que funcione en dispositivos Android.

En este artículo, creará una sencilla aplicación de Android con Xamarin. Forms y Visual Studio 2019.

## <a name="requirements"></a>Requisitos

Para usar este tutorial, necesitará lo siguiente:

- Windows 10
- [Visual Studio 2019: Community, Professional o Enterprise](https://visualstudio.microsoft.com/downloads/) (vea la nota)
- La carga de trabajo "desarrollo para dispositivos móviles con .NET" para Visual Studio 2019

> [!NOTE]
> Esta guía funcionará con Visual Studio 2017 o 2019. Si usa Visual Studio 2017, algunas instrucciones pueden ser incorrectas debido a diferencias en la interfaz de usuario entre las dos versiones de Visual Studio.

También tendrá un teléfono Android o un emulador configurado en el que ejecutar la aplicación. Consulte [prueba en un emulador o dispositivo Android](emulator.md).

## <a name="create-a-new-xamarinforms-project"></a>Crear un nuevo proyecto de Xamarin. Forms

Inicie Visual Studio. Haga clic en archivo > nuevo > proyecto para crear un nuevo proyecto.

En el cuadro de diálogo nuevo proyecto, seleccione la plantilla **aplicación móvil (Xamarin. Forms)** y haga clic en **siguiente**.

Asigne al proyecto el nombre **TimeChangerForms** y haga clic en **crear**.

En el cuadro de diálogo Nueva aplicación multiplataforma, seleccione **en blanco**. En la sección plataforma, Active **Android** y desactive todas las demás casillas. Haga clic en **Aceptar**.

Xamarin creará una nueva solución con dos proyectos: **TimeChangerForms** y **TimeChangerForms. Android.**

## <a name="create-a-ui-with-xaml"></a>Crear una interfaz de usuario con XAML

Expanda el proyecto **TimeChangerForms** y Abra **mainpage. Xaml**. El XAML de este archivo define la primera pantalla que verá un usuario al abrir TimeChanger.

La interfaz de usuario de TimeChanger es sencilla. Muestra la hora actual y tiene botones para ajustar la hora en incrementos de una hora. Usa un StackLayout vertical para alinear el tiempo por encima de los botones y un StackLayout horizontal para organizar los botones en paralelo. El contenido se centra en la pantalla mediante el establecimiento de **HorizontalOptions** de StackLayout vertical y **VerticalOptions** en **"CenterAndExpand"**.

Reemplace el contenido de MainPage. xaml por el código siguiente.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="TimeChangerForms.MainPage">

    <StackLayout HorizontalOptions="CenterAndExpand"
                 VerticalOptions="CenterAndExpand">
        <Label x:Name="time"
               HorizontalOptions="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               Text="At runtime, this Label will display the current time.">
        </Label>
        <StackLayout Orientation="Horizontal">
            <Button HorizontalOptions="End"
                    VerticalOptions="End"
                    Text="Up"
                    Clicked="OnUpButton_Clicked"/>
            <Button HorizontalOptions="Start"
                    VerticalOptions="End"
                    Text="Down"
                    Clicked="OnDownButton_Clicked"/>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Llegados a este punto, la interfaz de usuario está completa. Sin embargo, TimeChangerForms no se compilará porque se hace referencia a los métodos **UpButton_Clicked** y **DownButton_Clicked** en el código XAML, pero no se definen en ningún lugar. Incluso si la aplicación se ejecutó, no se mostraría la hora actual. En la siguiente sección, corregirá estos errores y agregará funcionalidad a la interfaz de usuario.

## <a name="add-logic-code-with-c"></a>Agregar código lógico con C #

En el Explorador de soluciones, haga clic con el botón derecho en MainPage. XAML y haga clic en **Ver código**. Este archivo contiene el código subyacente que agregará funcionalidad a la interfaz de usuario.

### <a name="set-the-current-time"></a>Establecer la hora actual

El código de este archivo puede hacer referencia a los controles declarados en el código XAML con el valor del atributo **x:Name** del control. En este caso, se llama a `time`la etiqueta que muestra la hora actual.

Los controles de interfaz de usuario deben actualizarse en el subproceso principal. Es posible que los cambios realizados desde otro subproceso no actualicen correctamente el control tal como se muestra en la pantalla. Dado que no hay ninguna garantía de que este código siempre se ejecute en el subproceso principal, use el método **BeginInvokeOnMainThread** para asegurarse de que las actualizaciones se muestren correctamente. Este es el método UpdateTimeLabel completo.

```csharp
private void UpdateTimeLabel(object state = null)
{
    Device.BeginInvokeOnMainThread(() =>
        {
            time.Text = DateTime.Now.ToLongTimeString();
        }
    );
}
```

### <a name="update-the-current-time-once-every-second"></a>Actualizar la hora actual una vez por segundo

En este momento, la hora actual será precisa, como máximo, un segundo después de que se inicie TimeChangerForms. La etiqueta se debe actualizar periódicamente para mantener la precisión del tiempo. Un objeto de **temporizador** llamará periódicamente a un método de devolución de llamada que actualiza la etiqueta con la hora actual.

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>Agregar HourOffset

Los botones arriba y abajo ajustan la hora en incrementos de una hora. Agregue una propiedad **HourOffset** para realizar el seguimiento del ajuste actual.

```csharp
public int HourOffset { get; private set; }
```

Ahora, actualice el método UpdateTimeLabel para tener en cuenta la propiedad HourOffset.

```csharp
currentTime.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="add-button-click-event-handlers"></a>Agregar controladores de eventos de clic de botón

Todos los botones arriba y abajo que deben hacer es aumentar o disminuir la propiedad HourOffset y llamar a UpdateTimeLabel.

```csharp
private void UpButton_Clicked(object sender, EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

Cuando haya terminado, MainPage.xaml.cs debe tener el siguiente aspecto:

```csharp
using System;
using System.ComponentModel;
using System.Threading;
using Xamarin.Forms;

namespace TimeChangerForms
{
    // Learn more about making custom code visible in the Xamarin.Forms previewer
    // by visiting https://aka.ms/xamarinforms-previewer
    [DesignTimeVisible(false)]
    public partial class MainPage : ContentPage
    {
        public int HourOffset { get; private set; }

        public MainPage()
        {
            InitializeComponent();
        }

        protected override void OnAppearing()
        {
            base.OnAppearing();
            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
        }

        private void UpdateTimeLabel(object state = null)
        {
            Device.BeginInvokeOnMainThread(() =>
                {
                    time.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
                }
            );
        }

        private void OnUpButton_Clicked(object sender, EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        private void OnDownButton_Clicked(object sender, EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-the-app"></a>Ejecutar la aplicación

Para ejecutar la aplicación, presione **F5** o haga clic en depurar > iniciar depuración. En función de cómo [esté configurado el depurador](emulator.md), la aplicación se iniciará en un dispositivo o en un emulador.

## <a name="related-links"></a>Vínculos relacionados

- [Pruebe en un dispositivo o emulador Android](emulator.md).

- [Creación de una aplicación de ejemplo de Android con Xamarin. Android](xamarin-android.md)
