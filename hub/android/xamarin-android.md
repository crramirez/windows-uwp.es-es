---
title: Creación de una aplicación Android sencilla con Xamarin. Android
description: Introducción a la escritura de aplicaciones Android con Xamarin. Android
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: Android, Windows, Xamarin. Android, tutorial, XAML
ms.date: 04/28/2020
ms.openlocfilehash: c731b5f96243333e4a4ad150de499ac9459113bc
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255212"
---
# <a name="get-started-developing-for-android-using-xamarinandroid"></a>Introducción al desarrollo para Android con Xamarin. Android

Esta guía le ayudará a empezar a usar Xamarin. Android en Windows para crear una aplicación multiplataforma que funcione en dispositivos Android.

En este artículo, creará una sencilla aplicación de Android con Xamarin. Android y Visual Studio 2019.

## <a name="requirements"></a>Requisitos

Para usar este tutorial, necesitará lo siguiente:

- Windows 10
- [Visual Studio 2019: Community, Professional o Enterprise](https://visualstudio.microsoft.com/downloads/) (vea la nota)
- La carga de trabajo "desarrollo para dispositivos móviles con .NET" para Visual Studio 2019

> [!NOTE]
> Esta guía funcionará con Visual Studio 2017 o 2019. Si usa Visual Studio 2017, algunas instrucciones pueden ser incorrectas debido a diferencias en la interfaz de usuario entre las dos versiones de Visual Studio.

También tendrá un teléfono Android o un emulador configurado en el que ejecutar la aplicación. Consulte [configuración de un emulador de Android](emulator.md).

## <a name="create-a-new-xamarinandroid-project"></a>Creación de un nuevo proyecto de Xamarin.Android

Inicie Visual Studio. Seleccione Archivo > nuevo proyecto de > para crear un nuevo proyecto.

En el cuadro de diálogo nuevo proyecto, seleccione la plantilla **aplicación Android (Xamarin)** y haga clic en **siguiente**.

Asigne al proyecto el nombre **TimeChangerAndroid** y haga clic en **crear**.

En el cuadro de diálogo Nueva aplicación multiplataforma, seleccione **aplicación en blanco**. En la **versión mínima de Android**, seleccione **Android 5,0 (Lollipop)**. Haga clic en **OK**.

Xamarin creará una nueva solución con un solo proyecto denominado **TimeChangerAndroid**.

## <a name="create-a-ui-with-xaml"></a>Crear una interfaz de usuario con XAML

En el directorio **Resources\layout** del proyecto, Abra **activity_main. XML**. El XML de este archivo define la primera pantalla que verá un usuario al abrir TimeChanger.

La interfaz de usuario de TimeChanger es sencilla. Muestra la hora actual y tiene botones para ajustar la hora en incrementos de una hora. Utiliza un vertical `LinearLayout` para alinear el tiempo por encima de los botones y una horizontal `LinearLayout` para organizar los botones en paralelo. El contenido se centra en la pantalla al establecer el atributo **Android: Grav** en **Center** en el vertical `LinearLayout` .

Reemplace el contenido de **activity_main. XML** por el código siguiente.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="At runtime, I will display current time"
        android:id="@+id/timeDisplay"
    />
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Up"
            android:id="@+id/upButton"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Down"
            android:id="@+id/downButton"/>
    </LinearLayout>
</LinearLayout>
```

En este momento, puede ejecutar **TimeChangerAndroid** y ver la interfaz de usuario que ha creado. En la siguiente sección, agregará funcionalidad a la interfaz de usuario que muestra la hora actual y habilitando los botones para realizar una acción.

## <a name="add-logic-code-with-c"></a>Agregar código lógico con C #

Abra **MainActivity.CS**. Este archivo contiene la lógica de código subyacente que agregará funcionalidad a la interfaz de usuario.

### <a name="set-the-current-time"></a>Establecer la hora actual

En primer lugar, obtenga una referencia a `TextView` que mostrará la hora. Use **FindViewById** para buscar en todos los elementos de la interfaz de usuario el que tenga el valor **Android: ID** (que se estableció `"@+id/timeDisplay"` en en el XML del paso anterior). Este es el `TextView` que mostrará la hora actual.

```csharp
var timeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
```

Los controles de interfaz de usuario deben actualizarse en el subproceso de la interfaz de usuario. Es posible que los cambios realizados desde otro subproceso no actualicen correctamente el control tal como se muestra en la pantalla. Dado que no hay ninguna garantía de que este código siempre se ejecute en el subproceso de la interfaz de usuario, use el método **RunOnUiThread** para asegurarse de que las actualizaciones se muestran correctamente. Este es el `UpdateTimeLabel` método completo.

```csharp
private void UpdateTimeLabel(object state = null)
{
    RunOnUiThread(() =>
    {
        TimeDisplay.Text = DateTime.Now.ToLongTimeString();
    });
}
```

### <a name="update-the-current-time-once-every-second"></a>Actualizar la hora actual una vez por segundo

En este momento, la hora actual será precisa, como máximo, un segundo después de que se inicie TimeChangerAndroid. La etiqueta se debe actualizar periódicamente para mantener la precisión del tiempo. Un objeto de **temporizador** llamará periódicamente a un método de devolución de llamada que actualiza la etiqueta con la hora actual.

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
TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="create-the-button-click-event-handlers"></a>Crear los controladores de eventos de clic de botón

Todos los botones arriba y abajo que deben hacer es aumentar o disminuir la propiedad HourOffset y llamar a UpdateTimeLabel.

```csharp
public void UpButton_Click(object sender, System.EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

### <a name="wire-up-the-up-and-down-buttons-to-their-corresponding-event-handlers"></a>Conectar los botones arriba y abajo a los controladores de eventos correspondientes

Para asociar los botones a sus controladores de eventos correspondientes, use primero FindViewById para buscar los botones por sus identificadores. Una vez que tenga una referencia al objeto de botón, puede Agregar un controlador de eventos a su `Click` evento.

```csharp
Button upButton = FindViewById<Button>(Resource.Id.upButton);
upButton.Click += UpButton_Click;
```

## <a name="completed-mainactivitycs-file"></a>Archivo MainActivity.cs completado

Cuando haya terminado, MainActivity.cs debe tener el siguiente aspecto:

```csharp
using Android.App;
using Android.OS;
using Android.Support.V7.App;
using Android.Runtime;
using Android.Widget;
using System;
using System.Threading;

namespace TimeChangerAndroid
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        public TextView TimeDisplay { get; private set; }
        public int HourOffset { get; private set; }

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set the view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);

            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);

            Button upButton = FindViewById<Button>(Resource.Id.upButton);
            upButton.Click += OnUpButton_Click;

            Button downButton = FindViewById<Button>(Resource.Id.downButton);
            downButton.Click += OnDownButton_Click;

            TimeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
        }

        private void UpdateTimeLabel(object state = null)
        {
            // Timer callbacks run on a background thread, but UI updates must run on the UI thread.
            RunOnUiThread(() =>
            {
                TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
            });
        }

        public void OnUpButton_Click(object sender, System.EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        public void OnDownButton_Click(object sender, System.EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-your-app"></a>Ejecutar la aplicación

Para ejecutar la aplicación, presione **F5** o haga clic en depurar > iniciar depuración. En función de cómo [esté configurado el depurador](emulator.md), la aplicación se iniciará en un dispositivo o en un emulador.

## <a name="related-links"></a>Vínculos relacionados

- [Pruebe en un dispositivo o emulador Android](emulator.md).
- [Creación de una aplicación de ejemplo de Android con Xamarin. Forms](xamarin-forms.md)
