---
author: mcleblanc
description: "En este se tema se ofrece una asignación completa de las API de Windows Phone Silverlight a sus equivalentes de la Plataforma universal de Windows (UWP)."
title: Asignaciones de espacios de nombres y clases de Windows Phone Silverlight a UWP
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 751c0c0355e8f6c02248c6f6d61f8c62c709cd95
ms.lasthandoff: 02/07/2017

---

# <a name="windows-phone-silverlight-to-uwp-namespace-and-class-mappings"></a>Asignaciones de espacios de nombres y clases de Windows Phone Silverlight a UWP

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este se tema se ofrece una asignación completa de las API de Windows Phone Silverlight a sus equivalentes de la Plataforma universal de Windows (UWP). Por lo general, no existe una asignación exacta de funcionalidades, aunque: cualquier plataforma puede tener más o menos funcionalidades que su equivalente en un espacio de nombres o una clase.

La tabla de asignaciones te ayudará cuando trabajes en un proyecto de UWP y vuelvas a usar el código fuente de un proyecto de Windows Phone Silverlight. Existen diferencias en los nombres de los espacios de nombres y las clases (incluidos los controles de la interfaz de usuario) entre ambas plataformas. En muchos casos, basta con cambiar el nombre de un espacio de nombres y, después, el código se compilará. A veces, una clase o el nombre la API han cambiado, así como el nombre del espacio de nombres. En otros casos, la asignación requiere un poco más de trabajo y, en raras ocasiones, requiere un cambio de enfoque.

**Cómo usar la tabla:  ** En primer lugar, busca el nombre de la clase que estás usando. Las clases se muestran siempre que la asignación es más complicada que simplemente cambiar el nombre del espacio de nombres. Si la clase no aparece en la lista, significa que la asignación es simplemente un cambio de espacio de nombres. Por lo tanto, busca el nombre del espacio de nombres de la clase y encontrarás el nombre del espacio de nombres de UWP equivalente. La clase estará en ese espacio de nombres. Si el espacio de nombres no figura en la lista, su nombre no ha cambiado.

**Nota**  Windows 10 admite mucho más de .NET Framework que una aplicación de la Tienda de Windows Phone. Por ejemplo, Windows 10 tiene varios espacios de nombres System.ServiceModel.\* como System.Net System.Net.NetworkInformation y System.Net.Sockets.
Además, en una aplicación de Windows 10, te beneficiarás de .NET Native, que es una tecnología de compilación anticipada que convierte MSIL en código máquina que se puede ejecutar nativamente. Las aplicaciones de .NET Native se inician más rápido, usan menos memoria y usan menos batería que sus equivalentes MSIL.

| Windows Phone Silverlight | Windows Runtime |
|---------------------------|-----------------|
| Publicidad | |
| Clase **Microsoft.Advertising.Mobile.UI.AdControl** | Clase [AdControl](http://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx) |
| Alarmas, avisos y agentes en segundo plano | |
| Clase **Microsoft.Phone.BackgroundAgent** | Clase [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) |
| Espacio de nombres **Microsoft.Phone.Scheduler** | Espacio de nombre [ 
              **Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847) namespace |
| Clase **Microsoft.Phone.Scheduler.Alarm** | Clases [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) y [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) |
| Clases **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask**, **ScheduledTaskAgent** | Clase [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) |
| Clase **Microsoft.Phone.Scheduler.Reminder** | Clases [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) y [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) |
| Clase **Microsoft.Phone.PictureDecoder** | Clase [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) |
| Espacio de nombres **Microsoft.Phone.BackgroundAudio** | Espacio de nombres [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) |
| Espacio de nombres **Microsoft.Phone.BackgroundTransfer** | Espacio de nombres [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) |
| Entorno y modelo de aplicaciones | |
| Clase **System.AppDomain** | No hay equivalente directo. Consulta las clases [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) y [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016). |
| Clase **System.Environment** | No hay equivalente directo. |
| Clase **System.ComponentModel.Annotations**  | No hay equivalente directo. |
| Clase **System.ComponentModel.BackgroundWorker** | Clase [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) |
| Clase **System.ComponentModel.DesignerProperties** | Clase [**DesignMode**](https://msdn.microsoft.com/library/windows/apps/br224664) |
| Clases **System.Threading.Thread**, **System.Threading.ThreadPool** | Clase [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) |
| (ST = **System.Threading**) <br/> Método **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Método **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propiedad **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propiedad **S.Environment.ManagedThreadId** |
| Clase **System.Threading.Timer** | Clase [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) |
| (SWT = **System.Windows.Threading**) <br/> Clase **SWT.Dispatcher** | Clase [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) |
| (SWT = **System.Windows.Threading**) <br/> Clase **SWT.DispatcherTimer** | Clase[**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) |
| Blend para Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> Clase **MEDC.GeometryHelper** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Expression.Interactivity** | Espacio de nombres [Microsoft.Xaml.Interactivity](http://go.microsoft.com/fwlink/p/?LinkId=328776) |
| Espacio de nombres **Microsoft.Expression.Interactivity.Core** | Espacio de nombres [Microsoft.Xaml.Interactions.Core](http://go.microsoft.com/fwlink/p/?LinkId=328773) |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> Clase **MEIC.ExtendedVisualStateManager** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Expression.Interactivity.Input** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Expression.Interactivity.Media** | Espacio de nombres [Microsoft.Xaml.Interactions.Media](http://go.microsoft.com/fwlink/p/?LinkId=328775) |
| Espacio de nombres **Microsoft.Expression.Shapes** | No hay equivalente directo. |
| (MI = **Microsoft.Internal**) <br/> Interfaz **MI.IManagedFrameworkInternalHelper** | No hay equivalente directo. |
| Datos de contactos y calendarios | |
| Espacio de nombres **Microsoft.Phone.UserData** | Espacios de nombres [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/br225002), [**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/dn263359) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Clases **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** | Clase [**Contact**](https://msdn.microsoft.com/library/windows/apps/br224849) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Clase **MPU.Appointments** | Clase [**AppointmentCalendar**](https://msdn.microsoft.com/library/windows/apps/dn596134) |
| (MPU = **Microsoft.Phone.UserData**) <br/> Clase **MPU.Contacts** | Clase [**ContactStore**](https://msdn.microsoft.com/library/windows/apps/dn624859) |
| Controles e infraestructura de la interfaz de usuario | |
| Clase **ControlTiltEffect.TiltEffect** | Las animaciones de la biblioteca de animaciones de Windows Runtime están integradas en los estilos predeterminados de los controles comunes. Consulta [Animación](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espacio de nombres **Microsoft.Phone.Controls** | Espacio de nombres [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) |
| (MPC = **Microsoft.Phone.Controls**) <br/> Clase **MPC.ContextMenu** | Clase [**PopupMenu**](https://msdn.microsoft.com/library/windows/apps/br208693) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.DatePickerPage** | Clase [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.GestureListener** | Clase [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.LongListSelector** | Clase [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.ObscuredEventArgs** | Clases [**SystemProtection**](https://msdn.microsoft.com/library/windows/apps/jj585394), [**WindowActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208377) | 
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.Panorama** | Clase [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) | 
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>Clases **SWN.NavigationService** | Clase [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.PhoneApplicationPage** | Clase [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)|
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.TiltEffect** | Clase [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164) | 
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.TimePickerPage** | Clase [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313) |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.WebBrowser** | Clase [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) | 
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.WebBrowserExtensions** | No hay equivalente directo. | 
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.WrapPanel** | No existe un equivalente directo para fines de diseño general. Las clases [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) y [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) pueden usarse en la plantilla del panel de elementos de un control de elementos. | 
| (MPD = **Microsoft.Phone.Data**) <br/>Espacio de nombres **MPD.Linq** | No hay equivalente directo. | 
| (MPD = **Microsoft.Phone.Data**) <br/>Espacio de nombres **MPD.Linq.Mapping** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Phone.Globalization** | No hay equivalente directo. | 
| (MPI = **Microsoft.Phone.Info**) <br/>Clases **MPI.DeviceExtendedProperties**, **DeviceStatus** | Clases [**EasClientDeviceInformation**](https://msdn.microsoft.com/library/windows/apps/hh701390), [**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831). Para obtener más información, consulta [Estado del dispositivo](wpsl-to-uwp-input-and-sensors.md). | 
| (MPI = **Microsoft.Phone.Info**) <br/>Clase **MPI.MediaCapabilities** | No hay equivalente directo. | 
| (MPI = **Microsoft.Phone.Info**) <br/>Clase **MPI.UserExtendedProperties** | Clase [**AdvertisingManager**](https://msdn.microsoft.com/library/windows/apps/dn363391) | 
| Espacio de nombres **System.Windows** | Espacio de nombres [**Windows.UI.Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) | 
| Espacio de nombres **System.Windows.Automation** | Espacio de nombres [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/br209179) | 
| Espacios de nombres **System.Windows.Controls**, **System.Windows.Input** | Espacios de nombres [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) | 
| Clases **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** | Clase [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) | 
| Clase **System.Windows.Controls.RichTextBox** | Clase [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) | 
| Clase **System.Windows.Controls.WrapPanel** | No existe un equivalente directo para fines de diseño general. Las clases [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) y [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) pueden usarse en la plantilla del panel de elementos de un control de elementos. | 
| Espacio de nombres **System.Windows.Controls.Primitives** | Espacio de nombres [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818) |
| Espacio de nombres **System.Windows.Controls.Shapes** | Espacio de nombres [**Windows.UI.Xaml.Controls.Shapes**](https://msdn.microsoft.com/library/windows/apps/br243401) | 
| Espacio de nombres **System.Windows.Data** | Espacio de nombres [**Windows.UI.Xaml.Data**](https://msdn.microsoft.com/library/windows/apps/br209917) | 
| Espacio de nombres **System.Windows.Documents** | Espacio de nombres [**Windows.UI.Xaml.Documents**](https://msdn.microsoft.com/library/windows/apps/br209984) | 
| Espacio de nombres **System.Windows.Ink** | No hay equivalente directo. |
| Espacio de nombres **System.Windows.Markup** | Espacio de nombres [**Windows.UI.Xaml.Markup**](https://msdn.microsoft.com/library/windows/apps/br228046) | 
| Espacio de nombres **System.Windows.Navigation** | Espacio de nombres [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) |
| Evento **System.Windows.UIElement.Tap**; delegado **EventHandler&lt;GestureEventArgs&gt;** | Evento [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985); delegado [**TappedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227993) | 
| Datos y servicios |  | 
| Clase **System.Data.Linq.DataContext** | No hay equivalente directo. | 
| Clase **System.Data.Linq.Mapping.ColumnAttribute** | No hay equivalente directo. | 
| Clase **System.Data.Linq.SqlClient.SqlHelpers** | No hay equivalente directo. | 
| Dispositivos | |apli
| Espacios de nombres **Microsoft.Devices**, **Microsoft.Devices.Sensors** | Espacios de nombres [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459), [**Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517), [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648), [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) |
| Clases **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** | Clase [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124). También la clase [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) (solo Windows). |
| Clase **Microsoft.Devices.CameraButtons** | Clase [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) |
| Clase **Microsoft.Devices.CameraVideoBrushExtensions** | Clase [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) |
| Clase **Microsoft.Devices.Environment** | No hay equivalente directo. Como solución alternativa, usa la compilación condicional y define un símbolo personalizado. O bien, puedes diseñar una solución alternativa mediante la propiedad [IsAttached](http://msdn.microsoft.com/library/e299w87h.aspx) . |
| Clase **Microsoft.Devices.MediaHistory** | No hay equivalente directo. | 
| Clase **Microsoft.Devices.VibrateController** | Clase [**VibrationDevice**](https://msdn.microsoft.com/library/windows/apps/jj207230) |
| Clase **Microsoft.Devices.Radio.FMRadio** | No hay equivalente directo. | 
| Clases **Microsoft.Devices.Sensors.Accelerometer**, **Compass** | En el espacio de nombres [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) |
| Clase **Microsoft.Devices.Sensors.Gyroscope** | Clase [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/br225718) |
| Clase **Microsoft.Devices.Sensors.Motion** | Clase [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/br225766)  |
| Globalization | |
| Espacio de nombres **System.Globalization** | Espacio de nombres [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) |
| (ST = **System.Threading**) <br/> Propiedad **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propiedad **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propiedad **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propiedad **S.CultureInfo.CurrentUICulture** |
| Elementos gráficos y animación | |
| Espacios de nombres **Microsoft.Xna.Framework.\***, [Biblioteca de clases de XNA Framework](http://go.microsoft.com/fwlink/p/?LinkId=263769), [Biblioteca de clases de canalización de contenido](http://go.microsoft.com/fwlink/p/?LinkId=263770) | No hay equivalente directo. En general, se usa [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) con C++. Consulta [Desarrollo de juegos](https://msdn.microsoft.com/library/windows/apps/hh452744) e [Interoperabilidad de DirectX y XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). |
| Clase **Microsoft.Xna.Framework.Audio.Microphone** | Clase [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) |
| Clase **Microsoft.Xna.Framework.Audio.SoundEffect** | Clase [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) |
| Espacio de nombres **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> Espacio de nombres [**WPS.UserProfile.GameServices.Core**](https://msdn.microsoft.com/library/windows/apps/jj207609) |
| Clase **Microsoft.Xna.Framework.GamerServices.Guide** | No hay equivalente directo. | 
| Clase **Microsoft.Xna.Framework.Input.GamePad** | Clase [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) |
| Clase **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | Clase [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Clases **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** | Clase [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) |
| Clase **Microsoft.Xna.Framework.Media.MediaQueue** | Clase [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) |
| Clase **Microsoft.Xna.Framework.Media.Playlist** | Clase [**BackgroundMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652527) |
| Espacio de nombres **System.Windows.Media** | Espacio de nombres [**Windows.UI.Xaml.Media**](https://msdn.microsoft.com/library/windows/apps/br243045) |
| Clase **System.Windows.Media.RadialGradientBrush** | No hay equivalente directo. Consulta [Multimedia y elementos gráficos](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espacio de nombres **System.Windows.Media.Animation** | Espacio de nombres [**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/br243232) |
| Espacio de nombres **System.Windows.Media.Effects** | No hay equivalente directo. | 
| Espacio de nombres **System.Windows.Media.Imaging** | Espacio de nombres [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) |
| Espacio de nombres **System.Windows.Media.Media3D** | Espacio de nombres [**Windows.UI.Xaml.Media.Media3D**](https://msdn.microsoft.com/library/windows/apps/br243274) |
| Espacio de nombres **System.Windows.Shapes** | Espacio de nombres [**Windows.UI.Xaml.Shapes**](https://msdn.microsoft.com/library/windows/apps/br243401) |
| Iniciadores y selectores | |
| Clases **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** | Clase [**ContactPicker**](https://msdn.microsoft.com/library/windows/apps/br224913) |
| Clases **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** | Espacio de nombres [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) |
| Clases **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** | No hay equivalente directo. | 
| Clase **Microsoft.Phone.Tasks.CameraCaptureTask** | Clase [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124). También la clase [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) (solo Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | Clase [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) (método [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813)) |
| Clases **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** | Clase [**Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) |
| Clase **Microsoft.Phone.Tasks.EmailComposeTask** | Clase [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/dn631270) |
| Clase **Microsoft.Phone.Tasks.GameInviteTask** | No hay equivalente directo. | 
| Clases **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** | No hay equivalente directo. | 
| Clase **Microsoft.Phone.Tasks.PhoneCallTask** | Clase [**PhoneCallManager**](https://msdn.microsoft.com/library/windows/apps/dn624832) |
| Clase **Microsoft.Phone.Tasks.PhotoChooserTask** | Clase [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) |
| Clase **Microsoft.Phone.Tasks.SaveAppointmentTask** | Clase [**AppointmentManager**](https://msdn.microsoft.com/library/windows/apps/dn297254) |
| Clases **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** | Clase [**StoredContact**](https://msdn.microsoft.com/library/windows/apps/jj207727) (solo Windows Phone) | 
| Clase **Microsoft.Phone.Tasks.SaveRingtoneTask** | No hay equivalente directo. | 
| Clases **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** | Clase [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) |
| Ubicación | |
| Espacio de nombres **System.Device.Location** | Espacio de nombres [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) |
| Clase **System.Device.GeoCoordinateWatcher** | Clase [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) |
| Mapas | |
| Espacio de nombres **Microsoft.Phone.Maps** | Espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) |
| Espacio de nombres **Microsoft.Phone.Maps.Controls** | Espacio de nombres [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) |
| Clase **Microsoft.Phone.Maps.Controls.Map** | Clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) |
| Espacio de nombres **Microsoft.Phone.Maps.Services** | Espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) |
| Clases **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** | Clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) |
| Clase **System.Device.Location.GeoCoordinate** | Clase [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) |
| Clase **Microsoft.Phone.Maps.Services.Route** | Clase [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) |
| Clase **Microsoft.Phone.Maps.Services.RouteQuery** | Clase [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) |
| Monetización | |
| Espacio de nombres **Microsoft.Phone.Marketplace** | Espacio de nombres [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197) |
| Multimedia | |
| Espacio de nombres **Microsoft.Phone.Media** | Clase [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) |
| Redes | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.DeviceNetworkInformation** | Clases [**Hostname**](https://msdn.microsoft.com/library/windows/apps/br207113), [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293)
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.NetworkInterface** | Clase [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.NetworkInterfaceInfo** | Clase [**ConnectionProfile**](https://msdn.microsoft.com/library/windows/apps/br207249) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.NetworkInterfaceList** | Clase [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.SocketExtensions** | No hay equivalente directo. | 
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.WebRequestExtensions** | No hay equivalente directo. | 
| Espacio de nombres **Microsoft.Phone.Networking.Voip** | No hay equivalente directo. | 
| Clase **System.Net.CookieCollection** | Todavía se admite, pero faltan algunas propiedades (por ejemplo, IsReadOnly) |
| Clase **System.Net.DownloadProgressChangedEventArgs** y otras clases similares relacionadas con **System.Net.WebClient** | Clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Deriva de [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx) para medir el progreso. |
| Clases **System.Net.DnsEndPoint**, **IPAddress** | Aún se admiten estas clases, pero faltan algunas propiedades. Alternativamente, migra a la clase [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113). |
| Clase **System.Net.HttpUtility** | Clase [**HtmlFormatHelper**](https://msdn.microsoft.com/library/windows/apps/hh738437) |
| Clase **System.Net.HttpWebRequest** | Solo se admite parcialmente, pero la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar una solicitud HTTP. |
| Clase **System.Net.HttpWebResponse** | Todavía se admite, pero usa Dispose() en lugar de Close(). No obstante, la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) para representar una respuesta HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Clase **SNN.NetworkChange** | Todavía se admite, excepto para el constructor. |
| Clase **System.Net.OpenReadCompletedEventArgs** y otras clases similares relacionadas con **System.Net.WebClient** | Clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Clase **System.Net.Sockets.Socket** | Todavía se admite, pero usa Dispose() en lugar de Close(). Alternativamente, migra a la clase [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). |
| Clase **System.Net.Sockets.SocketException** | Se sigue admitiendo, pero usa la propiedad SocketErrorCode en lugar de ErrorCode.
| Clases **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** | Clase [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) | 
| Clase **System.Net.UploadProgressChangedEventArgs** y otras clases similares relacionadas con **System.Net.WebClient** | Clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx))
| Clase **System.Net.WebClient** | Clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx))
| Clase **System.Net.WebRequest** | Se admite parcialmente (un conjunto de propiedades diferente), pero la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar una solicitud HTTP.
| Clase **System.Net.WebResponse** | Todavía se admite, pero usa Dispose() en lugar de Close(). No obstante, la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) para representar una respuesta HTTP.
| (SN = **System.Net**) <br/> Clase **SN.WriteStreamClosedEventArgs** | Clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx))
| (SN = **System.Net**) <br/> Clase **SN.WriteStreamClosedEventHandler** | Clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx))
| Clase **System.UriFormatException** | Clase **System.FormatException** |
| Notificaciones | |
| MPN = espacio de nombres **Microsoft.Phone.Notification** | Espacios de nombres [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661), [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) |
| MPN = **Microsoft.Phone.Notification** <br/> Clase **MPN.HttpNotification** | Clase [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) |
| MPN = **Microsoft.Phone.Notification** <br/> Clase **MPN.HttpNotificationChannel** | Clase [**PushNotificationChannel**](https://msdn.microsoft.com/library/windows/apps/br241283) |
| Programación | |
| Espacio de nombres **System** | Espacios de nombres [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021) |
| Clases **System.Diagnostics.StackFrame**, **StackTrace** | No hay equivalente directo. | 
| Espacio de nombres **System.Diagnostics** | Espacio de nombres [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/br206677) |
| Interfaz **System.ICloneable** | Un método personalizado que devuelve el tipo adecuado. |
| Clase **System.Reflection.Emit.ILGenerator** | No hay equivalente directo. | 
| Extensiones reactivas | |
| Espacio de nombres **Microsoft.Phone.Reactive** | No hay equivalente directo. | 
| Reflexión | |
| Clase **System.Type** class | Clase **System.Reflection.TypeInfo**. Consulta [Reflexión en .NET Framework para aplicaciones de la Tienda Windows](https://msdn.microsoft.com/library/hh535795.aspx). |
| Recursos | |
| Clase **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>Espacios de nombres [**WA.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) y [**WA.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022), clase [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078). Consulta [Crear y recuperar recursos en aplicaciones de Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx). |
| Elemento seguro | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Clases **MPS.SecureElementChannel**, **MPS.SecureElementSession** | Clase [**SmartCardConnection**](https://msdn.microsoft.com/library/windows/apps/dn608002) |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Clase **MPS.SecureElementReader** | Clase [**SmartCardReader**](https://msdn.microsoft.com/library/windows/apps/dn263857) |
| Seguridad | |
| (SSC = **System.Security.Cryptography**) <br/> Clases **SSC.Aes**, **SSC.RSA** | Clase [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) |
| (SSC = **System.Security.Cryptography**) <br/> Clases **SSC.HMACSHA256**, **SSC.SHA256** | Clase [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) |
| (SSC = **System.Security.Cryptography**) <br/> Clase **SSC.ProtectedData** | Clase [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559) |
| (SSC = **System.Security.Cryptography**) <br/> Clase **SSC.RandomNumberGenerator** | Clase [**CryptographicBuffer**](https://msdn.microsoft.com/library/windows/apps/br227092) |
| (SSC = **System.Security.Cryptography**) <br/> Clase **SSC.X509Certificates.X509Certificate** | Clase [**CertificateEnrollmentManager**](https://msdn.microsoft.com/library/windows/apps/br212075) |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ApplicationBar** | Clase [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/dn279427) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ApplicationBarIconButton** | Clase [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) (cuando se usa dentro de la propiedad [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279430))
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ApplicationBarMenuItem** | Clase [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) (cuando se usa dentro de la propiedad [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279434) )
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clases **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** | Clase [**TileTemplateType**](https://msdn.microsoft.com/library/windows/apps/br208621) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.PhoneApplicationService** | Clases [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016), [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816)
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ProgressIndicator** | Clase [**StatusBarProgressIndicator**](https://msdn.microsoft.com/library/windows/apps/dn633865) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ShellTile** | Clase [**SecondaryTile**](https://msdn.microsoft.com/library/windows/apps/br242183) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ShellTileSchedule** | Clase [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ShellToast** | Clase [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.SystemTray** | Clase [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) |
| Almacenamiento y E/S | |
| Clases **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** | Clase [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) |
| Espacio de nombres **System.IO** | Espacios de nombres [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346), [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791)
| Clase **System.IO.Directory** | Clase [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) |
| Clase **System.IO.File** | Clases [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) y [**PathIO**](https://msdn.microsoft.com/library/windows/apps/hh701663)
| (SII = **System.IO.IsolatedStorage**) <br/> Clase **SII.IsolatedStorageFile** | Propiedad [**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) |
| (SII = **System.IO.IsolatedStorage**) <br/> Clase **SII.IsolatedStorageSettings** | Propiedad [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx) |
| Clase **System.IO.Stream** | Se sigue admitiendo, pero usa ReadAsync() y WriteAsync() en lugar de BeginRead()/EndRead() y BeginWrite()/EndWrite(). |
| Cartera | |
| Espacio de nombres **Microsoft.Phone.Wallet** | Espacio de nombres [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) |
| Xml | |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTimeOffset** |
 

El siguiente tema es [Migración del proyecto](wpsl-to-uwp-porting-to-a-uwp-project.md).


