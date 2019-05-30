---
description: En este tema se proporciona una asignación completa de Windows Phone Silverlight APIs a sus equivalentes de la plataforma Universal de Windows (UWP).
title: Windows Phone Silverlight para las asignaciones de espacio de nombres y clase UWP
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb71a95de3f54cb62fa3d2cbc96e5c7935e5d945
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372453"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight para asignaciones de API de UWP


En este tema se proporciona una asignación completa de Windows Phone Silverlight APIs a sus equivalentes de la plataforma Universal de Windows (UWP). Por lo general, no existe una asignación exacta de funcionalidades, aunque: cualquier plataforma puede tener más o menos funcionalidades que su equivalente en un espacio de nombres o una clase.

La tabla de asignación le ayudará cuando está trabajando en un proyecto de UWP y está reutilizando código fuente a partir de un proyecto de Windows Phone Silverlight. Existen diferencias en los nombres de los espacios de nombres y las clases (incluidos los controles de la interfaz de usuario) entre ambas plataformas. En muchos casos, basta con cambiar el nombre de un espacio de nombres y, después, el código se compilará. A veces, una clase o el nombre la API han cambiado, así como el nombre del espacio de nombres. En otros casos, la asignación requiere un poco más de trabajo y, en raras ocasiones, requiere un cambio de enfoque.

**Cómo usar la tabla:  ** En primer lugar, busque el nombre de la clase que está usando. Las clases se muestran siempre que la asignación es más complicada que simplemente cambiar el nombre del espacio de nombres. Si la clase no aparece en la lista, significa que la asignación es simplemente un cambio de espacio de nombres. Por lo tanto, busca el nombre del espacio de nombres de la clase y encontrarás el nombre del espacio de nombres de UWP equivalente. La clase estará en ese espacio de nombres. Si el espacio de nombres no figura en la lista, su nombre no ha cambiado.

**Tenga en cuenta**  Windows 10 es compatible con mucho más de .NET Framework, que hace una aplicación de Windows Phone Store. Por ejemplo, Windows 10 tiene varios System.ServiceModel. \* espacios de nombres, así como de System.Net, System.Net.NetworkInformation y System.Net.Sockets.
Además, en una aplicación de Windows 10, se beneficiará de .NET Native, que una tecnología de compilación ahead of time convierte MSIL en código máquina ejecutables de forma nativa. Las aplicaciones de .NET Native se inician más rápido, usan menos memoria y usan menos batería que sus equivalentes MSIL.

| Windows Phone Silverlight | Windows en tiempo de ejecución |
| ------------------------- | --------------- |
| Publicidad | |
| Clase **Microsoft.Advertising.Mobile.UI.AdControl** | Clase [AdControl](https://docs.microsoft.com/windows/uwp/monetize/display-ads-using-the-microsoft-advertising-libraries) |
| Alarmas, avisos y agentes en segundo plano | |
| Clase **Microsoft.Phone.BackgroundAgent** | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) clase |
| Espacio de nombres **Microsoft.Phone.Scheduler** | [**Windows.ApplicationModel.Background** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background) espacio de nombres |
| Clase **Microsoft.Phone.Scheduler.Alarm** | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) y [ **ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) clases |
| Clases **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask**, **ScheduledTaskAgent** | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) clase |
| Clase **Microsoft.Phone.Scheduler.Reminder** | [**BackgroundTaskBuilder** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) y [ **ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) clases |
| Clase **Microsoft.Phone.PictureDecoder** | [**BitmapDecoder** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) clase |
| Espacio de nombres **Microsoft.Phone.BackgroundAudio** | [**Windows.Media.Playback** ](https://docs.microsoft.com/uwp/api/Windows.Media.Playback) espacio de nombres |
| Espacio de nombres **Microsoft.Phone.BackgroundTransfer** | [**Windows.Networking.BackgroundTransfer** ](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer) espacio de nombres |
| Entorno y modelo de aplicaciones | |
| Clase **System.AppDomain** | No hay equivalente directo. Consulta las clases [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) y [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication). |
| Clase **System.Environment** | No hay equivalente directo. |
| Clase **System.ComponentModel.Annotations**  | No hay equivalente directo. |
| Clase **System.ComponentModel.BackgroundWorker** | [**Grupo de subprocesos** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) clase |
| Clase **System.ComponentModel.DesignerProperties** | [**DesignMode** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DesignMode) clase |
| Clases **System.Threading.Thread**, **System.Threading.ThreadPool** | [**Grupo de subprocesos** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPool) clase |
| (ST = **System.Threading**) <br/> Método **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Método **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propiedad **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propiedad **S.Environment.ManagedThreadId** |
| Clase **System.Threading.Timer** | [**ThreadPoolTimer** ](https://docs.microsoft.com/uwp/api/Windows.System.Threading.ThreadPoolTimer) clase |
| (SWT = **System.Windows.Threading**) <br/> Clase **SWT.Dispatcher** | [**CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) clase |
| (SWT = **System.Windows.Threading**) <br/> Clase **SWT.DispatcherTimer** | [**DispatcherTimer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DispatcherTimer) clase |
| Blend para Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> Clase **MEDC.GeometryHelper** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Expression.Interactivity** | Espacio de nombres[Microsoft.Xaml.Interactivity](https://go.microsoft.com/fwlink/p/?LinkId=328776) |
| Espacio de nombres **Microsoft.Expression.Interactivity.Core** | Espacio de nombres [Microsoft.Xaml.Interactions.Core](https://go.microsoft.com/fwlink/p/?LinkId=328773) |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> Clase **MEIC.ExtendedVisualStateManager** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Expression.Interactivity.Input** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Expression.Interactivity.Media** | Espacio de nombres [Microsoft.Xaml.Interactions.Media](https://go.microsoft.com/fwlink/p/?LinkId=328775) |
| Espacio de nombres **Microsoft.Expression.Shapes** | No hay equivalente directo. |
| (MI = **Microsoft.Internal**) <br/> Interfaz **MI.IManagedFrameworkInternalHelper** | No hay equivalente directo. |
| Datos de contactos y calendarios | |
| Espacio de nombres **Microsoft.Phone.UserData** | [**Windows.ApplicationModel.Contacts**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts), [**Windows.ApplicationModel.Appointments**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments) namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> Clases **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** | [**Póngase en contacto con** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) clase |
| (MPU = **Microsoft.Phone.UserData**) <br/> Clase **MPU.Appointments** | [**AppointmentCalendar** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) clase |
| (MPU = **Microsoft.Phone.UserData**) <br/> Clase **MPU.Contacts** | [**ContactStore** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) clase |
| Controles e infraestructura de la interfaz de usuario | |
| Clase **ControlTiltEffect.TiltEffect** | Las animaciones de la biblioteca de animaciones de Windows Runtime están integradas en los estilos predeterminados de los controles comunes. Consulta [Animación](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espacio de nombres **Microsoft.Phone.Controls** | [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> Clase **MPC.ContextMenu** | [**Menú emergente** ](https://docs.microsoft.com/uwp/api/Windows.UI.Popups.PopupMenu) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.DatePickerPage** | [**DatePickerFlyout** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.GestureListener** | [**GestureRecognizer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.LongListSelector** | [**SemanticZoom** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.ObscuredEventArgs** | [**Protección del sistema**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.SystemProtection), [ **WindowActivatedEventArgs** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) clases |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.Panorama** | [**Concentrador** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>Clases **SWN.NavigationService** | [**Marco** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.PhoneApplicationPage** | [**Página** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.TiltEffect** | [**PointerDownThemeAnimation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.TimePickerPage** | [**TimePickerFlyout** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.WebBrowser** | [**WebView** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.WebBrowserExtensions** | No hay equivalente directo. |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.WrapPanel** | No existe un equivalente directo para fines de diseño general. [**ItemsWrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) y [ **WrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) se puede usar en la plantilla del panel de elementos de un control de elementos. |
| (MPD = **Microsoft.Phone.Data**) <br/>Espacio de nombres **MPD.Linq** | No hay equivalente directo. |
| (MPD = **Microsoft.Phone.Data**) <br/>Espacio de nombres **MPD.Linq.Mapping** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Phone.Globalization** | No hay equivalente directo. |
| (MPI = **Microsoft.Phone.Info**) <br/>Clases **MPI.DeviceExtendedProperties**, **DeviceStatus** | [**EasClientDeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation), [ **MemoryManager** ](https://docs.microsoft.com/uwp/api/Windows.System.MemoryManager) clases. Para obtener más información, consulta [Estado del dispositivo](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Clase **MPI.MediaCapabilities** | No hay equivalente directo. |
| (MPI = **Microsoft.Phone.Info**) <br/>Clase **MPI.UserExtendedProperties** | [**AdvertisingManager** ](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile.AdvertisingManager) clase |
| Espacio de nombres **System.Windows** | [**Windows.UI.Xaml** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml) espacio de nombres |
| Espacio de nombres **System.Windows.Automation** | [**Windows.UI.Xaml.Automation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation) espacio de nombres |
| Espacios de nombres **System.Windows.Controls**, **System.Windows.Input** | [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Controls**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls) namespaces |
| Clases **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** | [**SwapChainPanel** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) clase |
| Clase **System.Windows.Controls.RichTextBox** | [**RichEditBox** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) clase |
| Clase **System.Windows.Controls.WrapPanel** | No existe un equivalente directo para fines de diseño general. [**ItemsWrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) y [ **WrapGrid** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) se puede usar en la plantilla del panel de elementos de un control de elementos. |
| Espacio de nombres **System.Windows.Controls.Primitives** | [**Windows.UI.Xaml.Controls.Primitives** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives) espacio de nombres |
| Espacio de nombres **System.Windows.Controls.Shapes** | [**Windows.UI.Xaml.Controls.Shapes** ](/uwp/api/Windows.UI.Xaml.Shapes) espacio de nombres |
| Espacio de nombres **System.Windows.Data** | [**Windows.UI.Xaml.Data** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data) espacio de nombres |
| Espacio de nombres **System.Windows.Documents** | [**Windows.UI.Xaml.Documents** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents) espacio de nombres |
| Espacio de nombres **System.Windows.Ink** | No hay equivalente directo. |
| Espacio de nombres **System.Windows.Markup** | [**Windows.UI.Xaml.Markup** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup) espacio de nombres | 
| Espacio de nombres **System.Windows.Navigation** | [**Windows.UI.Xaml.Navigation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation) espacio de nombres |
| Evento **System.Windows.UIElement.Tap**; delegado **EventHandler&lt;GestureEventArgs&gt;** | [**Pulsa** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped) eventos, [ **TappedEventHandler** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.tappedeventhandler) delegar |
| Datos y servicios |  |
| Clase **System.Data.Linq.DataContext** | No hay equivalente directo. |
| Clase **System.Data.Linq.Mapping.ColumnAttribute** | No hay equivalente directo. |
| Clase **System.Data.Linq.SqlClient.SqlHelpers** | No hay equivalente directo. |
| Dispositivos | |
| Espacios de nombres **Microsoft.Devices**, **Microsoft.Devices.Sensors** | [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration), [ **Windows.Devices.Enumeration.Pnp**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.Pnp), [ **Windows.Devices.Input** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [ **Windows.Devices.Sensors** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) espacios de nombres |
| Clases **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) clase. También la clase [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) (solo Windows). |
| Clase **Microsoft.Devices.CameraButtons** | [**HardwareButtons** ](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) clase |
| Clase **Microsoft.Devices.CameraVideoBrushExtensions** | [**CaptureElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) clase |
| Clase **Microsoft.Devices.Environment** | No hay equivalente directo. Como solución alternativa, usa la compilación condicional y define un símbolo personalizado. O bien, puedes diseñar una solución alternativa mediante la propiedad [IsAttached](https://docs.microsoft.com/dotnet/api/system.diagnostics.debugger.isattached?redirectedfrom=MSDN#System_Diagnostics_Debugger_IsAttached) . |
| Clase **Microsoft.Devices.MediaHistory** | No hay equivalente directo. |
| Clase **Microsoft.Devices.VibrateController** | [**VibrationDevice** ](https://docs.microsoft.com/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) clase |
| Clase **Microsoft.Devices.Radio.FMRadio** | No hay equivalente directo. |
| Clases **Microsoft.Devices.Sensors.Accelerometer**, **Compass** | En el espacio de nombres [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors) |
| Clase **Microsoft.Devices.Sensors.Gyroscope** | [**Girómetro** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer) clase |
| Clase **Microsoft.Devices.Sensors.Motion** | [**Inclinómetro** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) clase |
| Globalización | |
| Espacio de nombres **System.Globalization** | [**Windows.Globalization** ](https://docs.microsoft.com/uwp/api/Windows.Globalization) espacio de nombres |
| (ST = **System.Threading**) <br/> Propiedad **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propiedad **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propiedad **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propiedad **S.CultureInfo.CurrentUICulture** |
| Elementos gráficos y animación | |
| **Microsoft.Xna.Framework. \***  espacios de nombres, [biblioteca de clases de XNA Framework](https://go.microsoft.com/fwlink/p/?LinkId=263769), [biblioteca de clases de canalización de contenido](https://go.microsoft.com/fwlink/p/?LinkId=263770) | No hay equivalente directo. En general, se usa [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) con C++. Consulta [Desarrollo de juegos](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) e [Interoperabilidad de DirectX y XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). |
| Clase **Microsoft.Xna.Framework.Audio.Microphone** | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) clase |
| Clase **Microsoft.Xna.Framework.Audio.SoundEffect** | [**MediaElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) clase |
| Espacio de nombres **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://docs.microsoft.com/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) namespace |
| Clase **Microsoft.Xna.Framework.GamerServices.Guide** | No hay equivalente directo. |
| Clase **Microsoft.Xna.Framework.Input.GamePad** | [**HardwareButtons** ](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons) clase |
| Clase **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | [**GestureRecognizer** ](https://docs.microsoft.com/uwp/api/Windows.UI.Input.GestureRecognizer) clase |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Clases **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** | [**KnownFolders** ](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) clase |
| Clase **Microsoft.Xna.Framework.Media.MediaQueue** | [**SystemMediaTransportControls** ](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) clase |
| Clase **Microsoft.Xna.Framework.Media.Playlist** | [**BackgroundMediaPlayer** ](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) clase |
| Espacio de nombres **System.Windows.Media** | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) namespace |
| Clase **System.Windows.Media.RadialGradientBrush** | No hay equivalente directo. Consulta [Multimedia y elementos gráficos](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espacio de nombres **System.Windows.Media.Animation** | [**Windows.UI.Xaml.Media.Animation** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) espacio de nombres |
| Espacio de nombres **System.Windows.Media.Effects** | No hay equivalente directo. |
| Espacio de nombres **System.Windows.Media.Imaging** | [**Windows.UI.Xaml.Media.Imaging**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging) namespace |
| Espacio de nombres **System.Windows.Media.Media3D** | [**Windows.UI.Xaml.Media.Media3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D) namespace |
| Espacio de nombres **System.Windows.Shapes** | [**Windows.UI.Xaml.Shapes** ](/uwp/api/Windows.UI.Xaml.Shapes) espacio de nombres |
| Iniciadores y selectores | |
| Clases **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** | [**Contactpicker pueda** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) clase |
| Clases **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| Clases **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** | No hay equivalente directo. |
| Clase **Microsoft.Phone.Tasks.CameraCaptureTask** | [**MediaCapture** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) clase. También la clase [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI) (solo Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) clase ([**RequestAppPurchaseAsync** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) método) |
| Clases **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** | [**Selector de** ](https://docs.microsoft.com/uwp/api/Windows.System.Launcher) clase |
| Clase **Microsoft.Phone.Tasks.EmailComposeTask** | [**EmailMessage** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Email.EmailMessage) clase |
| Clase **Microsoft.Phone.Tasks.GameInviteTask** | No hay equivalente directo. |
| Clases **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** | No hay equivalente directo. |
| Clase **Microsoft.Phone.Tasks.PhoneCallTask** | [**PhoneCallManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) clase |
| Clase **Microsoft.Phone.Tasks.PhotoChooserTask** | [**FileOpenPicker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) clase |
| Clase **Microsoft.Phone.Tasks.SaveAppointmentTask** | [**AppointmentManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) clase |
| Clases **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** | [**StoredContact** ](https://docs.microsoft.com/uwp/api/Windows.Phone.PersonalInformation.StoredContact) clase (solo Windows Phone) | 
| Clase **Microsoft.Phone.Tasks.SaveRingtoneTask** | No hay equivalente directo. |
| Clases **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** | [**DataPackage** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) clase |
| Location | |
| Espacio de nombres **System.Device.Location** | [**Windows.Devices.Geolocation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation) espacio de nombres |
| Clase **System.Device.GeoCoordinateWatcher** | [**Geolocator** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator) clase |
| Maps | |
| Espacio de nombres **Microsoft.Phone.Maps** | [**Windows.Services.Maps** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) espacio de nombres |
| Espacio de nombres **Microsoft.Phone.Maps.Controls** | [**Windows.UI.Xaml.Controls.Maps** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps) espacio de nombres |
| Clase **Microsoft.Phone.Maps.Controls.Map** | [**MapControl** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) clase |
| Espacio de nombres **Microsoft.Phone.Maps.Services** | [**Windows.Services.Maps** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) espacio de nombres |
| Clases **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** | [**MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) clase |
| Clases **System.Device.Location.GeoCoordinate** | [**Geopoint** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint) clase |
| Clase **Microsoft.Phone.Maps.Services.Route** | [**MapRoute** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRoute) clase |
| Clase **Microsoft.Phone.Maps.Services.RouteQuery** | [**MapRouteFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapRouteFinder) clase |
| Monetización | |
| Espacio de nombres **Microsoft.Phone.Marketplace** | [**Windows.ApplicationModel.Store** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) espacio de nombres |
| Multimedia | |
| Espacio de nombres **Microsoft.Phone.Media** | [**MediaElement** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) clase |
| Funciones de red | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.DeviceNetworkInformation** | [**Nombre de host**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName), [ **NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) clases |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.NetworkInterface** | [**NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) clase |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.NetworkInterfaceInfo** | [**ConnectionProfile** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) clase |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.NetworkInterfaceList** | [**NetworkInformation** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation) clase |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.SocketExtensions** | No hay equivalente directo. |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.WebRequestExtensions** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Phone.Networking.Voip** | No hay equivalente directo. |
| Clase **System.Net.CookieCollection** | Todavía se admite, pero faltan algunas propiedades (por ejemplo, IsReadOnly) |
| Clase **System.Net.DownloadProgressChangedEventArgs** y clases similares relacionadas con **System.Net.WebClient** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) clase (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Deriva de [System.Net.Http.StreamContent](https://docs.microsoft.com/previous-versions/visualstudio/hh138119(v=vs.118)) para medir el progreso. |
| Clases **System.Net.DnsEndPoint**, **IPAddress** | Aún se admiten estas clases, pero faltan algunas propiedades. Alternativamente, migra a la clase [**HostName**](https://docs.microsoft.com/uwp/api/Windows.Networking.HostName). |
| Clase **System.Net.HttpUtility** | [**HtmlFormatHelper** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) clase |
| Clase **System.Net.HttpWebRequest** | Solo se admite parcialmente, pero la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar una solicitud HTTP. |
| Clase **System.Net.HttpWebResponse** | Todavía se admite, pero usa Dispose() en lugar de Close(). No obstante, la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage(v=vs.110).aspx) para representar una respuesta HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Clase **SNN.NetworkChange** | Todavía se admite, excepto para el constructor. |
| Clase **System.Net.OpenReadCompletedEventArgs** y otras clases similares relacionadas con **System.Net.WebClient** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) clase (o [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Clase **System.Net.Sockets.Socket** | Todavía se admite, pero usa Dispose() en lugar de Close(). Alternativamente, migra a la clase [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket). |
| Clase **System.Net.Sockets.SocketException** | Se sigue admitiendo, pero usa la propiedad SocketErrorCode en lugar de ErrorCode. |
| Clases **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** | [**DatagramSocket** ](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) clase |
| Clase **System.Net.UploadProgressChangedEventArgs** y clases similares relacionadas con **System.Net.WebClient** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) clase (o [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Clase **System.Net.WebClient** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) clase (o [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Clase **System.Net.WebRequest** | Se admite parcialmente (un conjunto de propiedades diferente), pero la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar una solicitud HTTP. |
| Clase **System.Net.WebResponse** | Todavía se admite, pero usa Dispose() en lugar de Close(). No obstante, la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage(v=vs.110).aspx) para representar una respuesta HTTP. |
| (SN = **System.Net**) <br/> Clase **SN.WriteStreamClosedEventArgs** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) clase (o [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> Clase **SN.WriteStreamClosedEventHandler** | [**HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) clase (o [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Clase **System.UriFormatException** | Clase **System.FormatException** |
| Notificaciones | |
| MPN = espacio de nombres **Microsoft.Phone.Notification** | [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications), [**Windows.Networking.PushNotifications**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications) namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> Clase **MPN.HttpNotification** | [**TileNotification** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification) clase |
| MPN = **Microsoft.Phone.Notification** <br/> Clase **MPN.HttpNotificationChannel** | [**PushNotificationChannel** ](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) clase |
| Programación | |
| Espacio de nombres **System** | [**Windows.Foundation** ](https://docs.microsoft.com/uwp/api/Windows.Foundation) espacio de nombres |
| Clases **System.Diagnostics.StackFrame**, **StackTrace** | No hay equivalente directo. |
| Espacio de nombres **System.Diagnostics** | [**Windows.Foundation.Diagnostics** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Diagnostics) espacio de nombres |
| Interfaz **System.ICloneable** | Un método personalizado que devuelve el tipo adecuado. |
| Clase **System.Reflection.Emit.ILGenerator** | No hay equivalente directo. |
| Extensiones reactivas | |
| Espacio de nombres **Microsoft.Phone.Reactive** | No hay equivalente directo. |
| Reflexión | |
| Clase **System.Type** class | Clase **System.Reflection.TypeInfo**. Consulta [Reflexión en .NET Framework para aplicaciones para UWP](https://docs.microsoft.com/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps). |
| Recursos | |
| Clase **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>[**WA. Resources.Core** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core) y [ **WA. Recursos** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources) espacios de nombres, [ **ResourceManager** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) clase. Consulta [Crear y recuperar recursos en aplicaciones de Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/hh694557(v=vs.140)). |
| Elemento seguro | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Clases **MPS.SecureElementChannel**, **MPS.SecureElementSession** | [**SmartCardConnection** ](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) clase |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Clase **MPS.SecureElementReader** | [**SmartCardReader**](https://docs.microsoft.com/uwp/api/Windows.Devices.SmartCards.SmartCardReader) class |
| Seguridad | |
| (SSC = **System.Security.Cryptography**) <br/> Clases **SSC.Aes**, **SSC.RSA** | [**CryptographicEngine** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) clase |
| (SSC = **System.Security.Cryptography**) <br/> Clases **SSC.HMACSHA256**, **SSC.SHA256** | [**HashAlgorithmProvider** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) clase |
| (SSC = **System.Security.Cryptography**) <br/> Clase **SSC.ProtectedData** | [**DataProtectionProvider** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) clase |
| (SSC = **System.Security.Cryptography**) <br/> Clase **SSC.RandomNumberGenerator** | [**CryptographicBuffer** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) clase |
| (SSC = **System.Security.Cryptography**) <br/> Clase **SSC.X509Certificates.X509Certificate** | [**CertificateEnrollmentManager** ](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) clase |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ApplicationBar** | [**Barra de comandos** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CommandBar) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ApplicationBarIconButton** | [**AppBarButton** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) clase (cuando se usa dentro del [ **PrimaryCommands** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) propiedad) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ApplicationBarMenuItem** | [**AppBarButton** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) clase (cuando se usa dentro del [ **SecondaryCommands** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) propiedad) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clases **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** | [**TileTemplateType** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileTemplateType) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.PhoneApplicationService** | [**CoreApplication**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplication), [ **DisplayRequest** ](https://docs.microsoft.com/uwp/api/Windows.System.Display.DisplayRequest) clases |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ProgressIndicator** | [**StatusBarProgressIndicator**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ShellTile** | [**SecondaryTile** ](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ShellTileSchedule** | [**Tileupdater a** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ShellToast** | [**ToastNotificationManager** ](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationManager) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.SystemTray** | [**StatusBar** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) clase |
| Almacenamiento y E/S | |
| Clases **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** | [**KnownFolders** ](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) clase |
| Espacio de nombres **System.IO** | [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage), [**Windows.Storage.Streams**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) namespaces |
| Clase **System.IO.Directory** | [**StorageFolder** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) clase |
| Clase **System.IO.File** | [**StorageFile** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) y [ **PathIO** ](https://docs.microsoft.com/uwp/api/Windows.Storage.PathIO) clases
| (SII = **System.IO.IsolatedStorage**) <br/> Clase **SII.IsolatedStorageFile** |[**ApplicationData.LocalFolder** ](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) propiedad |
| (SII = **System.IO.IsolatedStorage**) <br/> Clase **SII.IsolatedStorageSettings** | [**ApplicationData.LocalSettings** ](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) propiedad |
| Clase **System.IO.Stream** | Se sigue admitiendo, pero usa ReadAsync() y WriteAsync() en lugar de BeginRead()/EndRead() y BeginWrite()/EndWrite(). |
| Cartera | |
| Espacio de nombres **Microsoft.Phone.Wallet** | [**Windows.ApplicationModel.Wallet**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Wallet) namespace |
| xml | |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTimeOffset** |


El siguiente tema es [Migración del proyecto](wpsl-to-uwp-porting-to-a-uwp-project.md).

