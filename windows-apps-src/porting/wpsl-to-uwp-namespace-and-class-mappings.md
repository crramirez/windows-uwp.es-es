---
description: En este se tema se ofrece una asignación completa de las API de Windows Phone Silverlight a sus equivalentes de la Plataforma universal de Windows (UWP).
title: WPSL a las asignaciones de espacio de nombres y clases de UWP
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ab6bdace41041f03bd4316b1f65de1a5aeb60835
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167499"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight a las asignaciones de API de UWP


En este se tema se ofrece una asignación completa de las API de Windows Phone Silverlight a sus equivalentes de la Plataforma universal de Windows (UWP). Por lo general, no existe una asignación exacta de funcionalidades, aunque: cualquier plataforma puede tener más o menos funcionalidades que su equivalente en un espacio de nombres o una clase.

La tabla de asignaciones te ayudará cuando trabajes en un proyecto de UWP y vuelvas a usar el código fuente de un proyecto de Windows Phone Silverlight. Existen diferencias en los nombres de los espacios de nombres y las clases (incluidos los controles de la interfaz de usuario) entre ambas plataformas. En muchos casos, basta con cambiar el nombre de un espacio de nombres y, después, el código se compilará. A veces, una clase o el nombre la API han cambiado, así como el nombre del espacio de nombres. En otros casos, la asignación requiere un poco más de trabajo y, en raras ocasiones, requiere un cambio de enfoque.

**Cómo usar la tabla:  ** En primer lugar, busque el nombre de la clase que está usando. Las clases se muestran siempre que la asignación es más complicada que simplemente cambiar el nombre del espacio de nombres. Si la clase no aparece en la lista, significa que la asignación es simplemente un cambio de espacio de nombres. Por lo tanto, busca el nombre del espacio de nombres de la clase y encontrarás el nombre del espacio de nombres de UWP equivalente. La clase estará en ese espacio de nombres. Si el espacio de nombres no figura en la lista, su nombre no ha cambiado.

**Nota:**    Windows 10 es compatible con muchas más de las .NET Framework que una aplicación de Windows Phone Store. Por ejemplo, Windows 10 tiene varios System. ServiceModel. \* los espacios de nombres, así como System.Net, System .net. NetworkInformation y System .net. Sockets.
Además, en una aplicación de Windows 10, te beneficiarás de .NET Native, que es una tecnología de compilación anticipada que convierte MSIL en código máquina que se puede ejecutar nativamente. Las aplicaciones de .NET Native se inician más rápido, usan menos memoria y usan menos batería que sus equivalentes MSIL.

| Windows Phone Silverlight | Windows en tiempo de ejecución |
| ------------------------- | --------------- |
| Publicidad | |
| Clase **Microsoft.Advertising.Mobile.UI.AdControl** | Clase [AdControl](../monetize/display-ads-in-your-app.md) |
| Alarmas, avisos y agentes en segundo plano | |
| Clase **Microsoft.Phone.BackgroundAgent** | Clase [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) |
| Espacio de nombres **Microsoft.Phone.Scheduler** | Espacio de nombres [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background) |
| Clase **Microsoft.Phone.Scheduler.Alarm** | Clases [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) y [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) |
| Clases **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask**, **ScheduledTaskAgent** | Clase [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) |
| Clase **Microsoft.Phone.Scheduler.Reminder** | Clases [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) y [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) |
| Clase **Microsoft.Phone.PictureDecoder** | Clase [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) |
| Espacio de nombres **Microsoft.Phone.BackgroundAudio** | Espacio de nombres [**Windows.Media.Playback**](/uwp/api/Windows.Media.Playback) |
| Espacio de nombres **Microsoft.Phone.BackgroundTransfer** | Espacio de nombres [**Windows.Networking.BackgroundTransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) |
| Entorno y modelo de aplicaciones | |
| Clase **System.AppDomain** | No hay equivalente directo. Consulta las clases [**Application**](/uwp/api/Windows.UI.Xaml.Application) y [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication). |
| Clase **System.Environment** | No hay equivalente directo. |
| Clase **System.ComponentModel.Annotations**  | No hay equivalente directo. |
| Clase **System.ComponentModel.BackgroundWorker** | Clase [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) |
| Clase **System.ComponentModel.DesignerProperties** | Clase [**DesignMode**](/uwp/api/Windows.ApplicationModel.DesignMode) |
| Clases **System.Threading.Thread**, **System.Threading.ThreadPool** | Clase [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) |
| (ST = **System. Threading**) <br/> Método **ST.Thread.MemoryBarrier** | (ST = **System. Threading**) <br/> Método **ST.Interlocked.MemoryBarrier** |
| (ST = **System. Threading**) <br/> Propiedad **ST.Thread.ManagedThreadId** | (S = **sistema**) <br/> Propiedad **S.Environment.ManagedThreadId** |
| Clase **System.Threading.Timer** | Clase [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) |
| (SWT = **System.Windows.Threading**) <br/> Clase **SWT.Dispatcher** | Clase [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) |
| (SWT = **System.Windows.Threading**) <br/> Clase **SWT.DispatcherTimer** | Clase [**DispatcherTimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer) |
| Blend para Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> Clase **MEDC.GeometryHelper** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Expression.Interactivity** | Espacio de nombres[Microsoft.Xaml.Interactivity](/previous-versions/dn458193(v=vs.120)) |
| Espacio de nombres **Microsoft.Expression.Interactivity.Core** | Espacio de nombres [Microsoft.Xaml.Interactions.Core](/previous-versions/dn458182(v=vs.120)) |
| (MEIC = **Microsoft. Expression. Interactivity. Core**) <br/> Clase **MEIC.ExtendedVisualStateManager** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Expression.Interactivity.Input** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Expression.Interactivity.Media** | Espacio de nombres [Microsoft.Xaml.Interactions.Media](/previous-versions/dn458186(v=vs.120)) |
| Espacio de nombres **Microsoft.Expression.Shapes** | No hay equivalente directo. |
| (MI = **Microsoft. Internal**) <br/> Interfaz **MI.IManagedFrameworkInternalHelper** | No hay equivalente directo. |
| Datos de contactos y calendarios | |
| Espacio de nombres **Microsoft.Phone.UserData** | Espacios de nombres [**Windows.ApplicationModel.Contacts**](/uwp/api/Windows.ApplicationModel.Contacts), [**Windows.ApplicationModel.Appointments**](/uwp/api/Windows.ApplicationModel.Appointments) |
| (MPU = **Microsoft. Phone. UserData**) <br/> Clases **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** | Clase [**Contact**](/uwp/api/Windows.ApplicationModel.Contacts.Contact) |
| (MPU = **Microsoft. Phone. UserData**) <br/> Clase **MPU.Appointments** | Clase [**AppointmentCalendar**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) |
| (MPU = **Microsoft. Phone. UserData**) <br/> Clase **MPU.Contacts** | Clase [**ContactStore**](/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) |
| Controles e infraestructura de la interfaz de usuario | |
| Clase **ControlTiltEffect.TiltEffect** | Las animaciones de la biblioteca de animaciones de Windows Runtime están integradas en los estilos predeterminados de los controles comunes. Vea [animación](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espacio de nombres **Microsoft.Phone.Controls** | Espacio de nombres [**Windows. UI. Xaml. Controls**](/uwp/api/Windows.UI.Xaml.Controls) |
| (MPC = **Microsoft. Phone. Controls**) <br/> Clase **MPC.ContextMenu** | Clase [**PopupMenu**](/uwp/api/Windows.UI.Popups.PopupMenu) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.DatePickerPage** | Clase [**DatePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.GestureListener** | Clase [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.LongListSelector** | [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) (clase) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.ObscuredEventArgs** | Clases [**SystemProtection**](/uwp/api/Windows.Phone.System.SystemProtection), [**WindowActivatedEventArgs**](/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.Panorama** | Clase [**Hub**](/uwp/api/Windows.UI.Xaml.Controls.Hub) |
| (MPC = **Microsoft. Phone. Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>Clases **SWN.NavigationService** | Clase [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.PhoneApplicationPage** | Clase [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.TiltEffect** | Clase [**PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.TimePickerPage** | Clase [**TimePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.WebBrowser** | [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) (clase) |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.WebBrowserExtensions** | No hay equivalente directo. |
| (MPC = **Microsoft. Phone. Controls**) <br/>Clase **MPC.WrapPanel** | No existe un equivalente directo para fines de diseño general. Las clases [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) y [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) pueden usarse en la plantilla del panel de elementos de un control de elementos. |
| (MPD = **Microsoft. Phone. Data**) <br/>Espacio de nombres **MPD.Linq** | No hay equivalente directo. |
| (MPD = **Microsoft. Phone. Data**) <br/>Espacio de nombres **MPD.Linq.Mapping** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Phone.Globalization** | No hay equivalente directo. |
| (MPI = **Microsoft.Phone.Info**) <br/>Clases **MPI.DeviceExtendedProperties**, **DeviceStatus** | Clases [**EasClientDeviceInformation**](/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation), [**MemoryManager**](/uwp/api/Windows.System.MemoryManager). Para obtener más información, consulte [Estado del dispositivo](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Clase **MPI.MediaCapabilities** | No hay equivalente directo. |
| (MPI = **Microsoft.Phone.Info**) <br/>Clase **MPI.UserExtendedProperties** | Clase [**AdvertisingManager**](/uwp/api/Windows.System.UserProfile.AdvertisingManager) |
| Espacio de nombres **System.Windows** | Espacio de nombres [**Windows.UI.Xaml**](/uwp/api/Windows.UI.Xaml) |
| Espacio de nombres **System.Windows.Automation** | Espacio de nombres [**Windows.UI.Xaml.Automation**](/uwp/api/Windows.UI.Xaml.Automation) |
| Espacios de nombres **System.Windows.Controls**, **System.Windows.Input** | Espacios de nombres [**Windows.UI.Core**](/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Controls**](/uwp/api/Windows.UI.Xaml.Controls) |
| Clases **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** | Clase [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) |
| Clase **System.Windows.Controls.RichTextBox** | Clase [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) |
| Clase **System.Windows.Controls.WrapPanel** | No existe un equivalente directo para fines de diseño general. Las clases [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) y [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) pueden usarse en la plantilla del panel de elementos de un control de elementos. |
| Espacio de nombres **System.Windows.Controls.Primitives** | Espacio de nombres [**Windows.UI.Xaml.Controls.Primitives**](/uwp/api/Windows.UI.Xaml.Controls.Primitives) |
| Espacio de nombres **System.Windows.Controls.Shapes** | Espacio de nombres [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Espacio de nombres **System.Windows.Data** | Espacio de nombres [**Windows.UI.Xaml.Data**](/uwp/api/Windows.UI.Xaml.Data) |
| Espacio de nombres **System.Windows.Documents** | Espacio de nombres [**Windows.UI.Xaml.Documents**](/uwp/api/Windows.UI.Xaml.Documents) |
| Espacio de nombres **System.Windows.Ink** | No hay equivalente directo. |
| Espacio de nombres **System.Windows.Markup** | Espacio de nombres [**Windows.UI.Xaml.Markup**](/uwp/api/Windows.UI.Xaml.Markup) | 
| Espacio de nombres **System.Windows.Navigation** | Espacio de nombres [**Windows.UI.Xaml.Navigation**](/uwp/api/Windows.UI.Xaml.Navigation) |
| Evento **System.Windows.UIElement.Tap**; delegado **EventHandler&lt;GestureEventArgs&gt;** | Evento [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped); delegado [**TappedEventHandler**](/uwp/api/windows.ui.xaml.input.tappedeventhandler) |
| Datos y servicios |  |
| Clase **System.Data.Linq.DataContext** | No hay equivalente directo. |
| Clase **System.Data.Linq.Mapping.ColumnAttribute** | No hay equivalente directo. |
| Clase **System.Data.Linq.SqlClient.SqlHelpers** | No hay equivalente directo. |
| Dispositivos | |
| Espacios de nombres **Microsoft.Devices**, **Microsoft.Devices.Sensors** | Espacios de nombres [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration), [**Windows.Devices.Enumeration.Pnp**](/uwp/api/Windows.Devices.Enumeration.Pnp), [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input), [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors) |
| Clases **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** | Clase [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) . También la clase [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) (solo Windows). |
| Clase **Microsoft.Devices.CameraButtons** | Clase [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) |
| Clase **Microsoft.Devices.CameraVideoBrushExtensions** | Clase [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) |
| Clase **Microsoft.Devices.Environment** | No hay equivalente directo. Como solución alternativa, usa la compilación condicional y define un símbolo personalizado. O bien, puedes diseñar una solución alternativa mediante la propiedad [IsAttached](/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached) . |
| Clase **Microsoft.Devices.MediaHistory** | No hay equivalente directo. |
| Clase **Microsoft.Devices.VibrateController** | Clase [**VibrationDevice**](/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) |
| Clase **Microsoft.Devices.Radio.FMRadio** | No hay equivalente directo. |
| Clases **Microsoft.Devices.Sensors.Accelerometer**, **Compass** | En el espacio de nombres [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors) |
| Clase **Microsoft.Devices.Sensors.Gyroscope** | Clase [**Gyrometer**](/uwp/api/Windows.Devices.Sensors.Gyrometer) |
| Clase **Microsoft.Devices.Sensors.Motion** | Clase [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer) |
| Globalización | |
| **System. Globalization** (espacio de nombres) | [**Windows. Globalization**](/uwp/api/Windows.Globalization) (espacio de nombres) |
| (ST = **System. Threading**) <br/> Propiedad **ST.Thread.CurrentCulture** | (SG = **System. Globalization**) <br/> Propiedad **S.CultureInfo.CurrentCulture** |
| (ST = **System. Threading**) <br/> Propiedad **ST.Thread.CurrentUICulture** | (SG = **System. Globalization**) <br/> Propiedad **S.CultureInfo.CurrentUICulture** |
| Gráficos y animación | |
| **Microsoft. XNA. Framework. \* ** namespaces, [biblioteca de clases de XNA framework](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)), [biblioteca de clases de canalización de contenido](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | No hay equivalente directo. En general, se usa [Microsoft DirectX](/windows/desktop/directx) con C++. Consulta [Desarrollo de juegos](/previous-versions/windows/apps/hh452744(v=win.10)) e [Interoperabilidad de DirectX y XAML](/previous-versions/windows/apps/hh825871(v=win.10)). |
| Clase **Microsoft.Xna.Framework.Audio.Microphone** | Clase [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) |
| Clase **Microsoft.Xna.Framework.Audio.SoundEffect** | [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) (clase) |
| Espacio de nombres **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.SysTEM**) <br/> Espacio de nombres [**WPS.UserProfile.GameServices.Core**](/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) |
| Clase **Microsoft.Xna.Framework.GamerServices.Guide** | No hay equivalente directo. |
| Clase **Microsoft.Xna.Framework.Input.GamePad** | Clase [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) |
| Clase **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | Clase [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Clases **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** | Clase [**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) |
| Clase **Microsoft.Xna.Framework.Media.MediaQueue** | Clase [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) |
| Clase **Microsoft.Xna.Framework.Media.Playlist** | Clase [**BackgroundMediaPlayer**](/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) |
| Espacio de nombres **System.Windows.Media** | Espacio de nombres [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) |
| Clase **System.Windows.Media.RadialGradientBrush** | No hay equivalente directo. Vea [multimedia y gráficos](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espacio de nombres **System.Windows.Media.Animation** | Espacio de nombres [**Windows.UI.Xaml.Media.Animation**](/uwp/api/Windows.UI.Xaml.Media.Animation) |
| Espacio de nombres **System.Windows.Media.Effects** | No hay equivalente directo. |
| Espacio de nombres **System.Windows.Media.Imaging** | Espacio de nombres [**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging) |
| Espacio de nombres **System.Windows.Media.Media3D** | Espacio de nombres [**Windows.UI.Xaml.Media.Media3D**](/uwp/api/Windows.UI.Xaml.Media.Media3D) |
| Espacio de nombres **System.Windows.Shapes** | Espacio de nombres [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) |
| Iniciadores y selectores | |
| Clases **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** | Clase [**ContactPicker**](/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) |
| Clases **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** | Espacio de nombres [**Windows.ApplicationModel.Wallet**](/uwp/api/Windows.ApplicationModel.Wallet) |
| Clases **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** | No hay equivalente directo. |
| Clase **Microsoft.Phone.Tasks.CameraCaptureTask** | Clase [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) . También la clase [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) (solo Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | Clase [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) (método [**RequestAppPurchaseAsync**](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync)) |
| Clases **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** | Clase [**Launcher**](/uwp/api/Windows.System.Launcher) |
| Clase **Microsoft.Phone.Tasks.EmailComposeTask** | Clase [**EmailMessage**](/uwp/api/Windows.ApplicationModel.Email.EmailMessage) |
| Clase **Microsoft.Phone.Tasks.GameInviteTask** | No hay equivalente directo. |
| Clases **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** | No hay equivalente directo. |
| Clase **Microsoft.Phone.Tasks.PhoneCallTask** | Clase [**PhoneCallManager**](/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) |
| Clase **Microsoft.Phone.Tasks.PhotoChooserTask** | Clase [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) |
| Clase **Microsoft.Phone.Tasks.SaveAppointmentTask** | Clase [**AppointmentManager**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) |
| Clases **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** | Clase [**StoredContact**](/uwp/api/Windows.Phone.PersonalInformation.StoredContact) (solo Windows Phone) | 
| Clase **Microsoft.Phone.Tasks.SaveRingtoneTask** | No hay equivalente directo. |
| Clases **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** | Clase [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) |
| Location | |
| Espacio de nombres **System.Device.Location** | Espacio de nombres [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) |
| Clase **System.Device.GeoCoordinateWatcher** | Clase [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) |
| Mapas | |
| Espacio de nombres **Microsoft.Phone.Maps** | Espacio de nombres [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) |
| Espacio de nombres **Microsoft.Phone.Maps.Controls** | Espacio de nombres [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps) |
| Clase **Microsoft.Phone.Maps.Controls.Map** | Clase [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) |
| Espacio de nombres **Microsoft.Phone.Maps.Services** | Espacio de nombres [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) |
| Clases **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** | Clases [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) |
| Clases **System.Device.Location.GeoCoordinate** | Clase [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) |
| Clase **Microsoft.Phone.Maps.Services.Route** | Clase [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) |
| Clase **Microsoft.Phone.Maps.Services.RouteQuery** | Clase [**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) |
| Monetización | |
| Espacio de nombres **Microsoft.Phone.Marketplace** | Espacio de nombres [**Windows.ApplicationModel.Store**](/uwp/api/Windows.ApplicationModel.Store) |
| Multimedia | |
| Espacio de nombres **Microsoft.Phone.Media** | [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) (clase) |
| Redes | |
| (MPNN = **Microsoft. Phone .net. NetworkInformation**) <br/> Clase **MPNN.DeviceNetworkInformation** | Clases [**Hostname**](/uwp/api/Windows.Networking.HostName), [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) |
| (MPNN = **Microsoft. Phone .net. NetworkInformation**) <br/> Clase **MPNN.NetworkInterface** | Clase [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) |
| (MPNN = **Microsoft. Phone .net. NetworkInformation**) <br/> Clase **MPNN.NetworkInterfaceInfo** | Clase [**ConnectionProfile**](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) |
| (MPNN = **Microsoft. Phone .net. NetworkInformation**) <br/> Clase **MPNN.NetworkInterfaceList** | Clase [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) |
| (MPNN = **Microsoft. Phone .net. NetworkInformation**) <br/> Clase **MPNN.SocketExtensions** | No hay equivalente directo. |
| (MPNN = **Microsoft. Phone .net. NetworkInformation**) <br/> Clase **MPNN.WebRequestExtensions** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Phone.Networking.Voip** | No hay equivalente directo. |
| Clase **System.Net.CookieCollection** | Todavía se admite, pero faltan algunas propiedades (por ejemplo, IsReadOnly) |
| Clase **System.Net.DownloadProgressChangedEventArgs** y clases similares relacionadas con **System.Net.WebClient** | Clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System .net. http. HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Deriva de [System.Net.Http.StreamContent](/previous-versions/visualstudio/hh138119(v=vs.118)) para medir el progreso. |
| Clases **System.Net.DnsEndPoint**, **IPAddress** | Aún se admiten estas clases, pero faltan algunas propiedades. Alternativamente, migra a la clase [**HostName**](/uwp/api/Windows.Networking.HostName). |
| Clase **System.Net.HttpUtility** | Clase [**HtmlFormatHelper**](/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) |
| Clase **System.Net.HttpWebRequest** | Solo se admite parcialmente, pero la alternativa recomendada y vanguardista es la clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Estas API usan [System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) para representar una solicitud HTTP. |
| Clase **System.Net.HttpWebResponse** | Todavía se admite, pero usa Dispose() en lugar de Close(). No obstante, la alternativa recomendada y vanguardista es la clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Estas API usan [System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) para representar una respuesta HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Clase **SNN.NetworkChange** | Todavía se admite, excepto para el constructor. |
| Clase **System.Net.OpenReadCompletedEventArgs** y otras clases similares relacionadas con **System.Net.WebClient** | Clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Clase **System.Net.Sockets.Socket** | Todavía se admite, pero usa Dispose() en lugar de Close(). Alternativamente, migra a la clase [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket). |
| Clase **System.Net.Sockets.SocketException** | Se sigue admitiendo, pero usa la propiedad SocketErrorCode en lugar de ErrorCode. |
| Clases **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** | Clase [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket) |
| Clase **System.Net.UploadProgressChangedEventArgs** y clases similares relacionadas con **System.Net.WebClient** | Clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Clase **System.Net.WebClient** | Clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Clase **System.Net.WebRequest** | Se admite parcialmente (un conjunto de propiedades diferente), pero la alternativa recomendada y vanguardista es la clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Estas API usan [System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) para representar una solicitud HTTP. |
| Clase **System.Net.WebResponse** | Todavía se admite, pero usa Dispose() en lugar de Close(). No obstante, la alternativa recomendada y vanguardista es la clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))). Estas API usan [System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) para representar una respuesta HTTP. |
| (SN = **System.Net**) <br/> Clase **SN.WriteStreamClosedEventArgs** | Clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> Clase **SN.WriteStreamClosedEventHandler** | Clase [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| Clase **System.UriFormatException** | Clase **System.FormatException** |
| Notificaciones | |
| MPN = espacio de nombres **Microsoft.Phone.Notification** | Espacios de nombres [**Windows.UI.Notifications**](/uwp/api/Windows.UI.Notifications), [**Windows.Networking.PushNotifications**](/uwp/api/Windows.Networking.PushNotifications) |
| MPN = **Microsoft.Phone.Notification** <br/> Clase **MPN.HttpNotification** | Clase [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification) |
| MPN = **Microsoft.Phone.Notification** <br/> Clase **MPN.HttpNotificationChannel** | Clase [**PushNotificationChannel**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) |
| Programar | |
| Espacio de nombres **System** | Espacio de nombres [**Windows. Foundation**](/uwp/api/Windows.Foundation) |
| Clases **System.Diagnostics.StackFrame**, **StackTrace** | No hay equivalente directo. |
| Espacio de nombres **System.Diagnostics** | Espacio de nombres [**Windows.Foundation.Diagnostics**](/uwp/api/Windows.Foundation.Diagnostics) |
| Interfaz **System.ICloneable** | Un método personalizado que devuelve el tipo adecuado. |
| Clase **System.Reflection.Emit.ILGenerator** | No hay equivalente directo. |
| Extensiones reactivas | |
| Espacio de nombres **Microsoft.Phone.Reactive** | No hay equivalente directo. |
| Reflexión | |
| Clase **System.Type** class | Clase **System.Reflection.TypeInfo**. Consulte [reflexión en el .NET Framework para aplicaciones para UWP](/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps). |
| Recursos | |
| Clase **System.Resources.ResourceManager** | (WA = **Windows. ApplicationModel**)<br/>Espacios de nombres [**WA.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) y [**WA.Resources**](/uwp/api/Windows.ApplicationModel.Resources), clase [**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager). Vea [crear y recuperar recursos en aplicaciones de Windows Runtime](/previous-versions/windows/apps/hh694557(v=vs.140)). |
| Elemento seguro | |
| (MPS = **Microsoft. Phone. SecureElement**) <br/> Clases **MPS.SecureElementChannel**, **MPS.SecureElementSession** | Clase [**SmartCardConnection**](/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) |
| (MPS = **Microsoft. Phone. SecureElement**) <br/> Clase **MPS.SecureElementReader** | Clase [**SmartCardReader**](/uwp/api/Windows.Devices.SmartCards.SmartCardReader) |
| Seguridad | |
| (CDC = **System. Security. Cryptography**) <br/> Clases **SSC.Aes**, **SSC.RSA** | Clase [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) |
| (CDC = **System. Security. Cryptography**) <br/> Clases **SSC.HMACSHA256**, **SSC.SHA256** | Clase [**HashAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) |
| (CDC = **System. Security. Cryptography**) <br/> Clase **SSC.ProtectedData** | Clase [**DataProtectionProvider**](/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) |
| (CDC = **System. Security. Cryptography**) <br/> Clase **SSC.RandomNumberGenerator** | Clase [**CryptographicBuffer**](/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) |
| (CDC = **System. Security. Cryptography**) <br/> Clase **SSC.X509Certificates.X509Certificate** | Clase [**CertificateEnrollmentManager**](/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) |
| Shell | |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clase **MPSh.ApplicationBar** | [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) (clase) |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clase **MPSh.ApplicationBarIconButton** | Clase [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) (cuando se usa dentro de la propiedad [**PrimaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands)) |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clase **MPSh.ApplicationBarMenuItem** | Clase [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) (cuando se usa dentro de la propiedad [**SecondaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands)) |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clases **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** | Clase [**TileTemplateType**](/uwp/api/Windows.UI.Notifications.TileTemplateType) |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clase **MPSh.PhoneApplicationService** | Clases [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication), [**DisplayRequest**](/uwp/api/Windows.System.Display.DisplayRequest) |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clase **MPSh.ProgressIndicator** | Clase [**StatusBarProgressIndicator**](/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clase **MPSh.ShellTile** | Clase [**SecondaryTile**](/uwp/api/Windows.UI.StartScreen.SecondaryTile) |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clase **MPSh.ShellTileSchedule** | Clase [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater) |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clase **MPSh.ShellToast** | Clase [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) |
| (MPSh = **Microsoft. Phone. Shell**) <br/> Clase **MPSh.SystemTray** | Clase [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar) |
| Almacenamiento y E/S | |
| Clases **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** | Clase [**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) |
| Espacio de nombres **System.IO** | Espacios de nombres [**Windows.Storage**](/uwp/api/Windows.Storage), [**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams) |
| Clase **System.IO.Directory** | [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) (clase) |
| Clase **System.IO.File** | Clases [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) y [**PathIO**](/uwp/api/Windows.Storage.PathIO)
| (SII = **System. IO. IsolatedStorage**) <br/> Clase **SII.IsolatedStorageFile** |Propiedad [**ApplicationData.LocalFolder**](/uwp/api/windows.storage.applicationdata.localfolder) |
| (SII = **System. IO. IsolatedStorage**) <br/> Clase **SII.IsolatedStorageSettings** | Propiedad [**ApplicationData.LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) |
| Clase **System.IO.Stream** | Se sigue admitiendo, pero usa ReadAsync() y WriteAsync() en lugar de BeginRead()/EndRead() y BeginWrite()/EndWrite(). |
| Cartera | |
| Espacio de nombres **Microsoft.Phone.Wallet** | Espacio de nombres [**Windows.ApplicationModel.Wallet**](/uwp/api/Windows.ApplicationModel.Wallet) |
| Xml | |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTimeOffset** |


El siguiente tema es el [traslado del proyecto](wpsl-to-uwp-porting-to-a-uwp-project.md).