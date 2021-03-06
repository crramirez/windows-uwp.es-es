---
title: Controlar la suspensión de aplicaciones
description: Aprende a guardar datos importantes de la aplicación cuando el sistema la suspende.
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 1c75200a768efd258aa84b20493b9d296578a05c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171829"
---
# <a name="handle-app-suspend"></a>Controlar la suspensión de aplicaciones

**API importantes**

- [**Suspendiendo**](/uwp/api/windows.ui.xaml.application.suspending)

Aprende a guardar datos importantes de la aplicación cuando el sistema la suspende. El ejemplo registra un controlador de eventos para el evento [**Suspensión**](/uwp/api/windows.ui.xaml.application.suspending) y guarda una cadena en un archivo.

## <a name="register-the-suspending-event-handler"></a>Registrar el controlador de eventos de suspensión

Haz el registro para controlar el evento [**Suspensión**](/uwp/api/windows.ui.xaml.application.suspending), que indica que la aplicación debe guardar sus datos de aplicación antes de que el sistema la suspenda.

```csharp
using System;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
   }
}
```

```vb
Public NotInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Suspending, AddressOf App_Suspending
   End Sub
   
End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Suspending({ this, &MainPage::App_Suspending });
}
```

```cpp
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;

MainPage::MainPage()
{
   InitializeComponent();
   Application::Current->Suspending +=
       ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
}
```

## <a name="save-application-data-before-suspension"></a>Guardar los datos de la aplicación antes de la suspensión

Cuando la aplicación controla el evento [**Suspensión**](/uwp/api/windows.ui.xaml.application.suspending), tiene la oportunidad de guardar sus datos de aplicación importantes en la función de controlador. La aplicación debe usar la API de almacenamiento [**LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) para guardar los datos de aplicación simples de manera sincrónica.

```csharp
partial class MainPage
{
    async void App_Suspending(
        Object sender,
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object,
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated.
    End Sub

End Class
```

```cppwinrt
void MainPage::App_Suspending(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::ApplicationModel::SuspendingEventArgs const& /* e */)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

## <a name="release-resources"></a>Liberar recursos

Conviene que liberes los recursos exclusivos y los identificadores de archivos para que otras aplicaciones puedan tener acceso a ellos cuando la aplicación esté suspendida. Algunos ejemplos de recursos exclusivos son las cámaras, los dispositivos de E/S, los dispositivos externos y los recursos de red. Liberar de forma explícita recursos exclusivos e identificadores de archivos sirve para que otras aplicaciones puedan acceder a ellos cuando la aplicación esté suspendida. Cuando la aplicación se reanude, deberá volver a adquirir sus recursos exclusivos y sus identificadores de archivos.

## <a name="remarks"></a>Observaciones

El sistema suspende la aplicación cuando el usuario cambia a otra aplicación, al escritorio o a la pantalla Inicio. El sistema reanuda la aplicación cuando el usuario vuelve a cambiar a ella. Cuando el sistema reanuda la aplicación, el contenido de las variables y las estructuras de datos es el mismo que antes de que el sistema la suspendiera. El sistema restaura la aplicación en el punto exacto en el que estaba, para que parezca al usuario que se ejecutaba en segundo plano

El sistema intenta mantener la aplicación y sus datos en la memoria mientras está suspendida. No obstante, si el sistema no tiene los recursos necesarios para mantener la aplicación en memoria, finalizará su ejecución. Cuando el usuario vuelve a una aplicación suspendida que ha finalizado, el sistema envía un evento [**Activado**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) y debe restaurar sus datos de aplicación en su método [**OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched).

El sistema no notifica a una aplicación cuando se cierra, con lo cual la aplicación deberá guardar sus datos de aplicación y liberar los recursos exclusivos y los identificadores de archivos cuando se suspenda y restaurarlos cuando vuelva a activarse.

Si realizas una llamada asincrónica en el controlador, el control vuelve inmediatamente de esa llamada asincrónica. Eso significa que, a continuación, la ejecución puede volver del controlador de eventos y la aplicación se moverá al siguiente estado aunque aún no haya completado la llamada asincrónica. Usa el método [**GetDeferral**](/uwp/api/Windows.ApplicationModel) en el objeto [**EnteredBackgroundEventArgs**](/uwp/api/Windows.ApplicationModel) que se pasa al controlador de eventos para retrasar la suspensión hasta después de llamar al método [**Complete**](/uwp/api/windows.foundation.deferral.complete) en el objeto [**Windows.Foundation.Deferral**](/uwp/api/windows.foundation.deferral) devuelto.

Un aplazamiento no aumenta la cantidad que tienes que ejecutar el código antes de que finalice la aplicación. Solo retrasa su terminación hasta que se llama al método *Complete* del aplazamiento o hasta que pasa la fecha límite, *lo que ocurra primero*. Para ampliar la hora en el estado de suspensión, use [ **ExtendedExecutionSession**](run-minimized-with-extended-execution.md)

> [!NOTE]
> Para mejorar la capacidad de respuesta del sistema en Windows 8.1, a las aplicaciones se les concede un acceso de prioridad baja a los recursos una vez suspendidos. Para admitir esta nueva prioridad, se ha ampliado el tiempo de espera de la operación de suspensión para que la aplicación tenga el equivalente al tiempo de espera de 5 segundos para la prioridad normal en Windows o entre 1 y 10 segundos en Windows Phone. No puedes ampliar ni modificar este período de tiempo de espera.

**Una nota sobre la depuración con Visual Studio:** Visual Studio impide que Windows suspenda una aplicación que está asociada al depurador. Esto permite que el usuario vea la interfaz de usuario de depuración de Visual Studio mientras se ejecuta la aplicación. Mientras depuras una aplicación, puedes enviarle un evento de suspensión mediante Visual Studio. Asegúrate de que se muestra la barra de herramientas **Ubicación de depuración** y luego haz clic en el botón **Suspender**.

## <a name="related-topics"></a>Temas relacionados

* [Ciclo de vida de la aplicación](app-lifecycle.md)
* [Controlar la activación de aplicaciones](activate-an-app.md)
* [Controlar la reanudación de aplicaciones](resume-an-app.md)
* [Directrices sobre la experiencia del usuario para inicio, suspensión y reanudación](./index.md)
* [Ejecución ampliada](run-minimized-with-extended-execution.md)

 

 