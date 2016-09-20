---
author: TylerMSFT
title: "Administrar la suspensión de la aplicación"
description: "Aprende a guardar datos importantes de la aplicación cuando el sistema la suspende."
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.sourcegitcommit: fb83213a4ce58285dae94da97fa20d397468bdc9
ms.openlocfilehash: 3ad58dc20a660d89622d215c46d263adf27a0542

---

# Administrar la suspensión de la aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Suspensión**](https://msdn.microsoft.com/library/windows/apps/br242341)

Aprende a guardar datos importantes de la aplicación cuando el sistema la suspende. El ejemplo registra un controlador de eventos para el evento [**Suspensión**](https://msdn.microsoft.com/library/windows/apps/br242341) y guarda una cadena en un archivo.

## Registrar el controlador de eventos de suspensión


Haz el registro para controlar el evento [**Suspensión**](https://msdn.microsoft.com/library/windows/apps/br242341), que indica que la aplicación debe guardar sus datos de aplicación antes de que el sistema la suspenda.

> [!div class="tabbedCodeSnippets"]
> ```cs
> using System;
> using Windows.ApplicationModel;
> using Windows.ApplicationModel.Activation;
> using Windows.UI.Xaml;
>
> partial class MainPage
> {
>    public MainPage()
>    {
>       InitializeComponent();
>       Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
>    }
> }
> ```
> ```vb
> Public NotInheritable Class MainPage
>
>    Public Sub New()
>       InitializeComponent()
>       AddHandler Application.Current.Suspending, AddressOf App_Suspending
>    End Sub
>    
> End Class
> ```
> ```cpp
> using namespace Windows::ApplicationModel;
> using namespace Windows::ApplicationModel::Activation;
> using namespace Windows::Foundation;
> using namespace Windows::UI::Xaml;
> using namespace AppName;
>
> MainPage::MainPage()
> {
>    InitializeComponent();
>    Application::Current->Suspending +=
>        ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
> }
> ```

## [!div class="tabbedCodeSnippets"]


Guardar los datos de la aplicación antes de la suspensión Cuando la aplicación controla el evento [**Suspensión**](https://msdn.microsoft.com/library/windows/apps/br242341), tiene la oportunidad de guardar sus datos de aplicación importantes en la función de controlador.

> [!div class="tabbedCodeSnippets"]
> ```cs
> partial class MainPage
> {
>     async void App_Suspending(
>         Object sender,
>         Windows.ApplicationModel.SuspendingEventArgs e)
>     {
>         // TODO: This is the time to save app data in case the process is terminated
>     }
> }
> ```
> ```vb
> Public NonInheritable Class MainPage
>
>     Private Sub App_Suspending(
>         sender As Object,
>         e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending
>
>         ' TODO: This is the time to save app data in case the process is terminated
>     End Sub
>
> End Class
> ```
> ```cpp
> void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
> {
>     // TODO: This is the time to save app data in case the process is terminated
> }
> ```

## La aplicación debe usar la API de almacenamiento [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) para guardar datos de aplicación simples de manera sincrónica.


[!div class="tabbedCodeSnippets"] Observaciones El sistema suspende la aplicación cuando el usuario cambia a otra aplicación, al escritorio o a la pantalla Inicio. El sistema reanuda la aplicación cuando el usuario vuelve a cambiar a ella.

Cuando el sistema reanuda la aplicación, el contenido de las variables y las estructuras de datos es el mismo que antes de que el sistema la suspendiera. El sistema restaura la aplicación en el punto exacto en el que estaba, para que parezca al usuario que se ejecutaba en segundo plano El sistema intenta mantener la aplicación y sus datos en memoria mientras está suspendida.

No obstante, si el sistema no tiene los recursos necesarios para mantener la aplicación en memoria, finalizará su ejecución.

> Cuando el usuario vuelve a una aplicación suspendida que ha finalizado, el sistema envía un evento [**Activado**](https://msdn.microsoft.com/library/windows/apps/br225018) y debe restaurar sus datos de aplicación en su método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). El sistema no notifica a una aplicación cuando se cierra, con lo cual la aplicación deberá guardar sus datos de aplicación y liberar los recursos exclusivos y los identificadores de archivos cuando se suspenda, y restaurarlos cuando vuelva a activarse.

> 
            **Nota** Si necesitas realizar algún trabajo asincrónico durante la suspensión de la aplicación, deberás aplazar la suspensión hasta que haya finalizado ese trabajo. Puedes usar el método [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) en el objeto [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) (disponible a través de los argumentos de evento) para retrasar la suspensión hasta después de haber llamado al método [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) en el objeto [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) devuelto. 
            **Nota** Para mejorar la capacidad de respuesta del sistema en Windows 8.1, las aplicaciones tienen acceso de prioridad baja a los recursos después de que entren en suspensión.

> Para admitir esta nueva prioridad, se ha ampliado el tiempo de espera de la operación de suspensión para que la aplicación tenga el equivalente al tiempo de espera de 5 segundos para la prioridad normal en Windows o entre 1 y 10 segundos en Windows Phone. No puedes ampliar ni modificar este período de tiempo de espera. 
            **Una nota sobre la depuración con Visual Studio:** Visual Studio impide que Windows suspenda una aplicación que está conectada al depurador. Esto permite que el usuario vea la interfaz de usuario de depuración de Visual Studio mientras se ejecuta la aplicación.

## Mientras depuras una aplicación, puedes enviarle un evento de suspensión mediante Visual Studio.


* [Asegúrate de que se muestra la barra de herramientas **Ubicación de depuración** y luego haz clic en el botón **Suspender**.](activate-an-app.md)
* [Temas relacionados](resume-an-app.md)
* [Controlar la activación de aplicaciones](https://msdn.microsoft.com/library/windows/apps/dn611862)
* [Controlar la reanudación de la aplicación](app-lifecycle.md)

 

 



<!--HONumber=Jun16_HO5-->


