---
description: En este tema se proporciona una asignación completa de Windows Phone Silverlight APIs a sus equivalentes de la plataforma Universal de Windows (UWP).
title: Windows Phone Silverlight para las asignaciones de espacio de nombres y clase UWP
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9acd42f57117fb01eef4ba8f87d35664be21cf32
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634980"
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
| Clase **Microsoft.Advertising.Mobile.UI.AdControl** | Clase [AdControl](https://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx) |
| Alarmas, avisos y agentes en segundo plano | |
| Clase **Microsoft.Phone.BackgroundAgent** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) clase |
| Espacio de nombres **Microsoft.Phone.Scheduler** | [**Windows.ApplicationModel.Background** ](https://msdn.microsoft.com/library/windows/apps/br224847) espacio de nombres |
| Clase **Microsoft.Phone.Scheduler.Alarm** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) y [ **ToastNotificationManager** ](https://msdn.microsoft.com/library/windows/apps/br208642) clases |
| Clases **Microsoft.Phone.Scheduler.PeriodicTask**, **ScheduledAction**, **ScheduledActionService**, **ScheduledTask**, **ScheduledTaskAgent** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) clase |
| Clase **Microsoft.Phone.Scheduler.Reminder** | [**BackgroundTaskBuilder** ](https://msdn.microsoft.com/library/windows/apps/br224768) y [ **ToastNotificationManager** ](https://msdn.microsoft.com/library/windows/apps/br208642) clases |
| Clase **Microsoft.Phone.PictureDecoder** | [**BitmapDecoder** ](https://msdn.microsoft.com/library/windows/apps/br226176) clase |
| Espacio de nombres **Microsoft.Phone.BackgroundAudio** | [**Windows.Media.Playback** ](https://msdn.microsoft.com/library/windows/apps/dn640562) espacio de nombres |
| Espacio de nombres **Microsoft.Phone.BackgroundTransfer** | [**Windows.Networking.BackgroundTransfer** ](https://msdn.microsoft.com/library/windows/apps/br207242) espacio de nombres |
| Entorno y modelo de aplicaciones | |
| Clase **System.AppDomain** | No hay equivalente directo. Consulta las clases [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) y [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016). |
| Clase **System.Environment** | No hay equivalente directo. |
| Clase **System.ComponentModel.Annotations**  | No hay equivalente directo. |
| Clase **System.ComponentModel.BackgroundWorker** | [**Grupo de subprocesos** ](https://msdn.microsoft.com/library/windows/apps/br229621) clase |
| Clase **System.ComponentModel.DesignerProperties** | [**DesignMode** ](https://msdn.microsoft.com/library/windows/apps/br224664) clase |
| Clases **System.Threading.Thread**, **System.Threading.ThreadPool** | [**Grupo de subprocesos** ](https://msdn.microsoft.com/library/windows/apps/br229621) clase |
| (ST = **System.Threading**) <br/> Método **ST.Thread.MemoryBarrier** | (ST = **System.Threading**) <br/> Método **ST.Interlocked.MemoryBarrier** |
| (ST = **System.Threading**) <br/> Propiedad **ST.Thread.ManagedThreadId** | (S = **System**) <br/> Propiedad **S.Environment.ManagedThreadId** |
| Clase **System.Threading.Timer** | [**ThreadPoolTimer** ](https://msdn.microsoft.com/library/windows/apps/br230587) clase |
| (SWT = **System.Windows.Threading**) <br/> Clase **SWT.Dispatcher** | [**CoreDispatcher** ](https://msdn.microsoft.com/library/windows/apps/br208211) clase |
| (SWT = **System.Windows.Threading**) <br/> Clase **SWT.DispatcherTimer** | [**DispatcherTimer** ](https://msdn.microsoft.com/library/windows/apps/br244250) clase |
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
| Espacio de nombres **Microsoft.Phone.UserData** | [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/br225002), [**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/dn263359) namespaces |
| (MPU = **Microsoft.Phone.UserData**) <br/> Clases **MPU.Account**, **ContactAddress**, **ContactCompanyInformation**, **ContactEmailAddress**, **ContactPhoneNumber** | [**Póngase en contacto con** ](https://msdn.microsoft.com/library/windows/apps/br224849) clase |
| (MPU = **Microsoft.Phone.UserData**) <br/> Clase **MPU.Appointments** | [**AppointmentCalendar** ](https://msdn.microsoft.com/library/windows/apps/dn596134) clase |
| (MPU = **Microsoft.Phone.UserData**) <br/> Clase **MPU.Contacts** | [**ContactStore** ](https://msdn.microsoft.com/library/windows/apps/dn624859) clase |
| Controles e infraestructura de la interfaz de usuario | |
| Clase **ControlTiltEffect.TiltEffect** | Las animaciones de la biblioteca de animaciones de Windows Runtime están integradas en los estilos predeterminados de los controles comunes. Consulta [Animación](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espacio de nombres **Microsoft.Phone.Controls** | [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) namespace |
| (MPC = **Microsoft.Phone.Controls**) <br/> Clase **MPC.ContextMenu** | [**Menú emergente** ](https://msdn.microsoft.com/library/windows/apps/br208693) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.DatePickerPage** | [**DatePickerFlyout** ](https://msdn.microsoft.com/library/windows/apps/dn625013) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.GestureListener** | [**GestureRecognizer** ](https://msdn.microsoft.com/library/windows/apps/br241937) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.LongListSelector** | [**SemanticZoom** ](https://msdn.microsoft.com/library/windows/apps/hh702601) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.ObscuredEventArgs** | [**Protección del sistema**](https://msdn.microsoft.com/library/windows/apps/jj585394), [ **WindowActivatedEventArgs** ](https://msdn.microsoft.com/library/windows/apps/br208377) clases |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.Panorama** | [**Concentrador** ](https://msdn.microsoft.com/library/windows/apps/dn251843) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**,<br/>(SWN = **System.Windows.Navigation**) <br/>Clases **SWN.NavigationService** | [**Marco** ](https://msdn.microsoft.com/library/windows/apps/br242682) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.PhoneApplicationPage** | [**Página** ](https://msdn.microsoft.com/library/windows/apps/br227503) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.TiltEffect** | [**PointerDownThemeAnimation** ](https://msdn.microsoft.com/library/windows/apps/hh969164) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.TimePickerPage** | [**TimePickerFlyout** ](https://msdn.microsoft.com/library/windows/apps/dn608313) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.WebBrowser** | [**WebView** ](https://msdn.microsoft.com/library/windows/apps/br227702) clase |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.WebBrowserExtensions** | No hay equivalente directo. |
| (MPC = **Microsoft.Phone.Controls**) <br/>Clase **MPC.WrapPanel** | No existe un equivalente directo para fines de diseño general. [**ItemsWrapGrid** ](https://msdn.microsoft.com/library/windows/apps/dn298849) y [ **WrapGrid** ](https://msdn.microsoft.com/library/windows/apps/br227717) se puede usar en la plantilla del panel de elementos de un control de elementos. |
| (MPD = **Microsoft.Phone.Data**) <br/>Espacio de nombres **MPD.Linq** | No hay equivalente directo. |
| (MPD = **Microsoft.Phone.Data**) <br/>Espacio de nombres **MPD.Linq.Mapping** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Phone.Globalization** | No hay equivalente directo. |
| (MPI = **Microsoft.Phone.Info**) <br/>Clases **MPI.DeviceExtendedProperties**, **DeviceStatus** | [**EasClientDeviceInformation**](https://msdn.microsoft.com/library/windows/apps/hh701390), [ **MemoryManager** ](https://msdn.microsoft.com/library/windows/apps/dn633831) clases. Para obtener más información, consulta [Estado del dispositivo](wpsl-to-uwp-input-and-sensors.md). |
| (MPI = **Microsoft.Phone.Info**) <br/>Clase **MPI.MediaCapabilities** | No hay equivalente directo. |
| (MPI = **Microsoft.Phone.Info**) <br/>Clase **MPI.UserExtendedProperties** | [**AdvertisingManager** ](https://msdn.microsoft.com/library/windows/apps/dn363391) clase |
| Espacio de nombres **System.Windows** | [**Windows.UI.Xaml** ](https://msdn.microsoft.com/library/windows/apps/br209045) espacio de nombres |
| Espacio de nombres **System.Windows.Automation** | [**Windows.UI.Xaml.Automation** ](https://msdn.microsoft.com/library/windows/apps/br209179) espacio de nombres |
| Espacios de nombres **System.Windows.Controls**, **System.Windows.Input** | [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) namespaces |
| Clases **System.Windows.Controls.DrawingSurface**, **DrawingSurfaceBackgroundGrid** | [**SwapChainPanel** ](https://msdn.microsoft.com/library/windows/apps/dn252834) clase |
| Clase **System.Windows.Controls.RichTextBox** | [**RichEditBox** ](https://msdn.microsoft.com/library/windows/apps/br227548) clase |
| Clase **System.Windows.Controls.WrapPanel** | No existe un equivalente directo para fines de diseño general. [**ItemsWrapGrid** ](https://msdn.microsoft.com/library/windows/apps/dn298849) y [ **WrapGrid** ](https://msdn.microsoft.com/library/windows/apps/br227717) se puede usar en la plantilla del panel de elementos de un control de elementos. |
| Espacio de nombres **System.Windows.Controls.Primitives** | [**Windows.UI.Xaml.Controls.Primitives** ](https://msdn.microsoft.com/library/windows/apps/br209818) espacio de nombres |
| Espacio de nombres **System.Windows.Controls.Shapes** | [**Windows.UI.Xaml.Controls.Shapes** ](/uwp/api/Windows.UI.Xaml.Shapes) espacio de nombres |
| Espacio de nombres **System.Windows.Data** | [**Windows.UI.Xaml.Data** ](https://msdn.microsoft.com/library/windows/apps/br209917) espacio de nombres |
| Espacio de nombres **System.Windows.Documents** | [**Windows.UI.Xaml.Documents** ](https://msdn.microsoft.com/library/windows/apps/br209984) espacio de nombres |
| Espacio de nombres **System.Windows.Ink** | No hay equivalente directo. |
| Espacio de nombres **System.Windows.Markup** | [**Windows.UI.Xaml.Markup** ](https://msdn.microsoft.com/library/windows/apps/br228046) espacio de nombres | 
| Espacio de nombres **System.Windows.Navigation** | [**Windows.UI.Xaml.Navigation** ](https://msdn.microsoft.com/library/windows/apps/br243300) espacio de nombres |
| Evento **System.Windows.UIElement.Tap**; delegado **EventHandler&lt;GestureEventArgs&gt;** | [**Pulsa** ](https://msdn.microsoft.com/library/windows/apps/br208985) eventos, [ **TappedEventHandler** ](https://msdn.microsoft.com/library/windows/apps/br227993) delegar |
| Datos y servicios |  |
| Clase **System.Data.Linq.DataContext** | No hay equivalente directo. |
| Clase **System.Data.Linq.Mapping.ColumnAttribute** | No hay equivalente directo. |
| Clase **System.Data.Linq.SqlClient.SqlHelpers** | No hay equivalente directo. |
| Dispositivos | |
| Espacios de nombres **Microsoft.Devices**, **Microsoft.Devices.Sensors** | [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459), [ **Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517), [ **Windows.Devices.Input** ](https://msdn.microsoft.com/library/windows/apps/br225648), [ **Windows.Devices.Sensors** ](https://msdn.microsoft.com/library/windows/apps/br206408) espacios de nombres |
| Clases **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** | [**MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/br241124) clase. También la clase [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) (solo Windows). |
| Clase **Microsoft.Devices.CameraButtons** | [**HardwareButtons** ](https://msdn.microsoft.com/library/windows/apps/jj207557) clase |
| Clase **Microsoft.Devices.CameraVideoBrushExtensions** | [**CaptureElement** ](https://msdn.microsoft.com/library/windows/apps/br209278) clase |
| Clase **Microsoft.Devices.Environment** | No hay equivalente directo. Como solución alternativa, usa la compilación condicional y define un símbolo personalizado. O bien, puedes diseñar una solución alternativa mediante la propiedad [IsAttached](https://msdn.microsoft.com/library/e299w87h.aspx) . |
| Clase **Microsoft.Devices.MediaHistory** | No hay equivalente directo. |
| Clase **Microsoft.Devices.VibrateController** | [**VibrationDevice** ](https://msdn.microsoft.com/library/windows/apps/jj207230) clase |
| Clase **Microsoft.Devices.Radio.FMRadio** | No hay equivalente directo. |
| Clases **Microsoft.Devices.Sensors.Accelerometer**, **Compass** | En el espacio de nombres [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) |
| Clase **Microsoft.Devices.Sensors.Gyroscope** | [**Girómetro** ](https://msdn.microsoft.com/library/windows/apps/br225718) clase |
| Clase **Microsoft.Devices.Sensors.Motion** | [**Inclinómetro** ](https://msdn.microsoft.com/library/windows/apps/br225766) clase |
| Globalización | |
| Espacio de nombres **System.Globalization** | [**Windows.Globalization** ](https://msdn.microsoft.com/library/windows/apps/br206813) espacio de nombres |
| (ST = **System.Threading**) <br/> Propiedad **ST.Thread.CurrentCulture** | (SG = **System.Globalization**) <br/> Propiedad **S.CultureInfo.CurrentCulture** |
| (ST = **System.Threading**) <br/> Propiedad **ST.Thread.CurrentUICulture** | (SG = **System.Globalization**) <br/> Propiedad **S.CultureInfo.CurrentUICulture** |
| Elementos gráficos y animación | |
| **Microsoft.Xna.Framework. \***  espacios de nombres, [biblioteca de clases de XNA Framework](https://go.microsoft.com/fwlink/p/?LinkId=263769), [biblioteca de clases de canalización de contenido](https://go.microsoft.com/fwlink/p/?LinkId=263770) | No hay equivalente directo. En general, se usa [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) con C++. Consulta [Desarrollo de juegos](https://msdn.microsoft.com/library/windows/apps/hh452744) e [Interoperabilidad de DirectX y XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). |
| Clase **Microsoft.Xna.Framework.Audio.Microphone** | [**MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/br241124) clase |
| Clase **Microsoft.Xna.Framework.Audio.SoundEffect** | [**MediaElement** ](https://msdn.microsoft.com/library/windows/apps/br242926) clase |
| Espacio de nombres **Microsoft.Xna.Framework.GamerServices** | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://msdn.microsoft.com/library/windows/apps/jj207609) namespace |
| Clase **Microsoft.Xna.Framework.GamerServices.Guide** | No hay equivalente directo. |
| Clase **Microsoft.Xna.Framework.Input.GamePad** | [**HardwareButtons** ](https://msdn.microsoft.com/library/windows/apps/jj207557) clase |
| Clase **Microsoft.Xna.Framework.Input.Touch.TouchPanel** | [**GestureRecognizer** ](https://msdn.microsoft.com/library/windows/apps/br241937) clase |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> Clases **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** | [**KnownFolders** ](https://msdn.microsoft.com/library/windows/apps/br227151) clase |
| Clase **Microsoft.Xna.Framework.Media.MediaQueue** | [**SystemMediaTransportControls** ](https://msdn.microsoft.com/library/windows/apps/dn278677) clase |
| Clase **Microsoft.Xna.Framework.Media.Playlist** | [**BackgroundMediaPlayer** ](https://msdn.microsoft.com/library/windows/apps/dn652527) clase |
| Espacio de nombres **System.Windows.Media** | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) namespace |
| Clase **System.Windows.Media.RadialGradientBrush** | No hay equivalente directo. Consulta [Multimedia y elementos gráficos](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Espacio de nombres **System.Windows.Media.Animation** | [**Windows.UI.Xaml.Media.Animation** ](https://msdn.microsoft.com/library/windows/apps/br243232) espacio de nombres |
| Espacio de nombres **System.Windows.Media.Effects** | No hay equivalente directo. |
| Espacio de nombres **System.Windows.Media.Imaging** | [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) namespace |
| Espacio de nombres **System.Windows.Media.Media3D** | [**Windows.UI.Xaml.Media.Media3D**](https://msdn.microsoft.com/library/windows/apps/br243274) namespace |
| Espacio de nombres **System.Windows.Shapes** | [**Windows.UI.Xaml.Shapes** ](/uwp/api/Windows.UI.Xaml.Shapes) espacio de nombres |
| Iniciadores y selectores | |
| Clases **Microsoft.Phone.Tasks.AddressChooserTask**, **EmailAddressChooserTask**, **PhoneNumberChooserTask** | [**Contactpicker pueda** ](https://msdn.microsoft.com/library/windows/apps/br224913) clase |
| Clases **Microsoft.Phone.Tasks.AddWalletItemTask**, **AddWalletItemResult** | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) namespace |
| Clases **Microsoft.Phone.Tasks.BingMapsDirectionsTask**, **BingMapsTask** | No hay equivalente directo. |
| Clase **Microsoft.Phone.Tasks.CameraCaptureTask** | [**MediaCapture** ](https://msdn.microsoft.com/library/windows/apps/br241124) clase. También la clase [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) (solo Windows). |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp** ](https://msdn.microsoft.com/library/windows/apps/hh779765) clase ([**RequestAppPurchaseAsync** ](https://msdn.microsoft.com/library/windows/apps/hh967813) método) |
| Clases **Microsoft.Phone.Tasks.ConnectionSettingsTask**, **MarketplaceHubTask**, **MarketplaceReviewTask**, **MarketplaceSearchTask**, **MediaPlayerLauncher**, **SearchTask**, **SmsComposeTask**, **WebBrowserTask** | [**Selector de** ](https://msdn.microsoft.com/library/windows/apps/br241801) clase |
| Clase **Microsoft.Phone.Tasks.EmailComposeTask** | [**EmailMessage** ](https://msdn.microsoft.com/library/windows/apps/dn631270) clase |
| Clase **Microsoft.Phone.Tasks.GameInviteTask** | No hay equivalente directo. |
| Clases **Microsoft.Phone.Tasks.MapDownloaderTask**, **MapsDirectionsTask**, **MapsTask**, **MapUpdaterTask** | No hay equivalente directo. |
| Clase **Microsoft.Phone.Tasks.PhoneCallTask** | [**PhoneCallManager** ](https://msdn.microsoft.com/library/windows/apps/dn624832) clase |
| Clase **Microsoft.Phone.Tasks.PhotoChooserTask** | [**FileOpenPicker** ](https://msdn.microsoft.com/library/windows/apps/br207847) clase |
| Clase **Microsoft.Phone.Tasks.SaveAppointmentTask** | [**AppointmentManager** ](https://msdn.microsoft.com/library/windows/apps/dn297254) clase |
| Clases **Microsoft.Phone.Tasks.SaveContactTask**, **SaveEmailAddressTask**, **SavePhoneNumberTask** | [**StoredContact** ](https://msdn.microsoft.com/library/windows/apps/jj207727) clase (solo Windows Phone) | 
| Clase **Microsoft.Phone.Tasks.SaveRingtoneTask** | No hay equivalente directo. |
| Clases **Microsoft.Phone.Tasks.ShareLinkTask**, **ShareMediaTask**, **ShareStatusTask** | [**DataPackage** ](https://msdn.microsoft.com/library/windows/apps/br205873) clase |
| Ubicación | |
| Espacio de nombres **System.Device.Location** | [**Windows.Devices.Geolocation** ](https://msdn.microsoft.com/library/windows/apps/br225603) espacio de nombres |
| Clase **System.Device.GeoCoordinateWatcher** | [**Geolocator** ](https://msdn.microsoft.com/library/windows/apps/br225534) clase |
| Maps | |
| Espacio de nombres **Microsoft.Phone.Maps** | [**Windows.Services.Maps** ](https://msdn.microsoft.com/library/windows/apps/dn636979) espacio de nombres |
| Espacio de nombres **Microsoft.Phone.Maps.Controls** | [**Windows.UI.Xaml.Controls.Maps** ](https://msdn.microsoft.com/library/windows/apps/dn610751) espacio de nombres |
| Clase **Microsoft.Phone.Maps.Controls.Map** | [**MapControl** ](https://msdn.microsoft.com/library/windows/apps/dn637004) clase |
| Espacio de nombres **Microsoft.Phone.Maps.Services** | [**Windows.Services.Maps** ](https://msdn.microsoft.com/library/windows/apps/dn636979) espacio de nombres |
| Clases **Microsoft.Phone.Maps.Services.GeocodeQuery**, **ReverseGeocodeQuery** | [**MapLocationFinder** ](https://msdn.microsoft.com/library/windows/apps/dn627550) clase |
| Clases **System.Device.Location.GeoCoordinate** | [**Geopoint** ](https://msdn.microsoft.com/library/windows/apps/dn263675) clase |
| Clase **Microsoft.Phone.Maps.Services.Route** | [**MapRoute** ](https://msdn.microsoft.com/library/windows/apps/dn636937) clase |
| Clase **Microsoft.Phone.Maps.Services.RouteQuery** | [**MapRouteFinder** ](https://msdn.microsoft.com/library/windows/apps/dn636938) clase |
| Monetización | |
| Espacio de nombres **Microsoft.Phone.Marketplace** | [**Windows.ApplicationModel.Store** ](https://msdn.microsoft.com/library/windows/apps/br225197) espacio de nombres |
| Multimedia | |
| Espacio de nombres **Microsoft.Phone.Media** | [**MediaElement** ](https://msdn.microsoft.com/library/windows/apps/br242926) clase |
| Funciones de red | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.DeviceNetworkInformation** | [**Nombre de host**](https://msdn.microsoft.com/library/windows/apps/br207113), [ **NetworkInformation** ](https://msdn.microsoft.com/library/windows/apps/br207293) clases |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.NetworkInterface** | [**NetworkInformation** ](https://msdn.microsoft.com/library/windows/apps/br207293) clase |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.NetworkInterfaceInfo** | [**ConnectionProfile** ](https://msdn.microsoft.com/library/windows/apps/br207249) clase |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.NetworkInterfaceList** | [**NetworkInformation** ](https://msdn.microsoft.com/library/windows/apps/br207293) clase |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.SocketExtensions** | No hay equivalente directo. |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> Clase **MPNN.WebRequestExtensions** | No hay equivalente directo. |
| Espacio de nombres **Microsoft.Phone.Networking.Voip** | No hay equivalente directo. |
| Clase **System.Net.CookieCollection** | Todavía se admite, pero faltan algunas propiedades (por ejemplo, IsReadOnly) |
| Clase **System.Net.DownloadProgressChangedEventArgs** y clases similares relacionadas con **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) clase (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Deriva de [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx) para medir el progreso. |
| Clases **System.Net.DnsEndPoint**, **IPAddress** | Aún se admiten estas clases, pero faltan algunas propiedades. Alternativamente, migra a la clase [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113). |
| Clase **System.Net.HttpUtility** | [**HtmlFormatHelper** ](https://msdn.microsoft.com/library/windows/apps/hh738437) clase |
| Clase **System.Net.HttpWebRequest** | Solo se admite parcialmente, pero la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar una solicitud HTTP. |
| Clase **System.Net.HttpWebResponse** | Todavía se admite, pero usa Dispose() en lugar de Close(). No obstante, la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) para representar una respuesta HTTP. |
| (SNN = **System.Net.NetworkInformation**) <br/> Clase **SNN.NetworkChange** | Todavía se admite, excepto para el constructor. |
| Clase **System.Net.OpenReadCompletedEventArgs** y otras clases similares relacionadas con **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) clase (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Clase **System.Net.Sockets.Socket** | Todavía se admite, pero usa Dispose() en lugar de Close(). Alternativamente, migra a la clase [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882). |
| Clase **System.Net.Sockets.SocketException** | Se sigue admitiendo, pero usa la propiedad SocketErrorCode en lugar de ErrorCode. |
| Clases **System.Net.Sockets.UdpAnySourceMulticastClient**, **UdpSingleSourceMulticastClient** | [**DatagramSocket** ](https://msdn.microsoft.com/library/windows/apps/br241319) clase |
| Clase **System.Net.UploadProgressChangedEventArgs** y clases similares relacionadas con **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) clase (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Clase **System.Net.WebClient** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) clase (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Clase **System.Net.WebRequest** | Se admite parcialmente (un conjunto de propiedades diferente), pero la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar una solicitud HTTP. |
| Clase **System.Net.WebResponse** | Todavía se admite, pero usa Dispose() en lugar de Close(). No obstante, la alternativa recomendada y vanguardista es la clase [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx)). Estas API usan [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) para representar una respuesta HTTP. |
| (SN = **System.Net**) <br/> Clase **SN.WriteStreamClosedEventArgs** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) clase (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| (SN = **System.Net**) <br/> Clase **SN.WriteStreamClosedEventHandler** | [**HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) clase (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| Clase **System.UriFormatException** | Clase **System.FormatException** |
| Notificaciones | |
| MPN = espacio de nombres **Microsoft.Phone.Notification** | [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661), [**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) namespaces |
| MPN = **Microsoft.Phone.Notification** <br/> Clase **MPN.HttpNotification** | [**TileNotification** ](https://msdn.microsoft.com/library/windows/apps/br208616) clase |
| MPN = **Microsoft.Phone.Notification** <br/> Clase **MPN.HttpNotificationChannel** | [**PushNotificationChannel** ](https://msdn.microsoft.com/library/windows/apps/br241283) clase |
| Programación | |
| Espacio de nombres **System** | [**Windows.Foundation** ](https://msdn.microsoft.com/library/windows/apps/br226021) espacio de nombres |
| Clases **System.Diagnostics.StackFrame**, **StackTrace** | No hay equivalente directo. |
| Espacio de nombres **System.Diagnostics** | [**Windows.Foundation.Diagnostics** ](https://msdn.microsoft.com/library/windows/apps/br206677) espacio de nombres |
| Interfaz **System.ICloneable** | Un método personalizado que devuelve el tipo adecuado. |
| Clase **System.Reflection.Emit.ILGenerator** | No hay equivalente directo. |
| Extensiones reactivas | |
| Espacio de nombres **Microsoft.Phone.Reactive** | No hay equivalente directo. |
| Reflexión | |
| Clase **System.Type** class | Clase **System.Reflection.TypeInfo**. Consulta [Reflexión en .NET Framework para aplicaciones para UWP](https://msdn.microsoft.com/library/hh535795.aspx). |
| Recursos | |
| Clase **System.Resources.ResourceManager** | (WA = **Windows.ApplicationModel**)<br/>[**WA. Resources.Core** ](https://msdn.microsoft.com/library/windows/apps/br225039) y [ **WA. Recursos** ](https://msdn.microsoft.com/library/windows/apps/br206022) espacios de nombres, [ **ResourceManager** ](https://msdn.microsoft.com/library/windows/apps/br206078) clase. Consulta [Crear y recuperar recursos en aplicaciones de Windows Runtime](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx). |
| Elemento seguro | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Clases **MPS.SecureElementChannel**, **MPS.SecureElementSession** | [**SmartCardConnection** ](https://msdn.microsoft.com/library/windows/apps/dn608002) clase |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> Clase **MPS.SecureElementReader** | [**SmartCardReader**](https://msdn.microsoft.com/library/windows/apps/dn263857) class |
| Seguridad | |
| (SSC = **System.Security.Cryptography**) <br/> Clases **SSC.Aes**, **SSC.RSA** | [**CryptographicEngine** ](https://msdn.microsoft.com/library/windows/apps/br241490) clase |
| (SSC = **System.Security.Cryptography**) <br/> Clases **SSC.HMACSHA256**, **SSC.SHA256** | [**HashAlgorithmProvider** ](https://msdn.microsoft.com/library/windows/apps/br241511) clase |
| (SSC = **System.Security.Cryptography**) <br/> Clase **SSC.ProtectedData** | [**DataProtectionProvider** ](https://msdn.microsoft.com/library/windows/apps/br241559) clase |
| (SSC = **System.Security.Cryptography**) <br/> Clase **SSC.RandomNumberGenerator** | [**CryptographicBuffer** ](https://msdn.microsoft.com/library/windows/apps/br227092) clase |
| (SSC = **System.Security.Cryptography**) <br/> Clase **SSC.X509Certificates.X509Certificate** | [**CertificateEnrollmentManager** ](https://msdn.microsoft.com/library/windows/apps/br212075) clase |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ApplicationBar** | [**Barra de comandos** ](https://msdn.microsoft.com/library/windows/apps/dn279427) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ApplicationBarIconButton** | [**AppBarButton** ](https://msdn.microsoft.com/library/windows/apps/dn279244) clase (cuando se usa dentro del [ **PrimaryCommands** ](https://msdn.microsoft.com/library/windows/apps/dn279430) propiedad) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ApplicationBarMenuItem** | [**AppBarButton** ](https://msdn.microsoft.com/library/windows/apps/dn279244) clase (cuando se usa dentro del [ **SecondaryCommands** ](https://msdn.microsoft.com/library/windows/apps/dn279434) propiedad) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clases **MPSh.CycleTileData**, **MPSh.FlipTileData**, **MPSh.IconicTileData**, **MPSh.ShellTileData**, **MPSh.StandardTileData** | [**TileTemplateType** ](https://msdn.microsoft.com/library/windows/apps/br208621) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.PhoneApplicationService** | [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016), [ **DisplayRequest** ](https://msdn.microsoft.com/library/windows/apps/br241816) clases |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ProgressIndicator** | [**StatusBarProgressIndicator**](https://msdn.microsoft.com/library/windows/apps/dn633865) class |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ShellTile** | [**SecondaryTile** ](https://msdn.microsoft.com/library/windows/apps/br242183) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ShellTileSchedule** | [**Tileupdater a** ](https://msdn.microsoft.com/library/windows/apps/br208628) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.ShellToast** | [**ToastNotificationManager** ](https://msdn.microsoft.com/library/windows/apps/br208642) clase |
| (MPSh = **Microsoft.Phone.Shell**) <br/> Clase **MPSh.SystemTray** | [**StatusBar** ](https://msdn.microsoft.com/library/windows/apps/dn633864) clase |
| Almacenamiento y E/S | |
| Clases **Microsoft.Phone.Storage.ExternalStorage**, **ExternalStorageDevice**, **ExternalStorageFile**, **ExternalStorageFolder** | [**KnownFolders** ](https://msdn.microsoft.com/library/windows/apps/br227151) clase |
| Espacio de nombres **System.IO** | [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346), [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) namespaces |
| Clase **System.IO.Directory** | [**StorageFolder** ](https://msdn.microsoft.com/library/windows/apps/br227230) clase |
| Clase **System.IO.File** | [**StorageFile** ](https://msdn.microsoft.com/library/windows/apps/br227171) y [ **PathIO** ](https://msdn.microsoft.com/library/windows/apps/hh701663) clases
| (SII = **System.IO.IsolatedStorage**) <br/> Clase **SII.IsolatedStorageFile** |[**ApplicationData.LocalFolder** ](https://msdn.microsoft.com/library/windows/apps/br241621) propiedad |
| (SII = **System.IO.IsolatedStorage**) <br/> Clase **SII.IsolatedStorageSettings** | [**ApplicationData.LocalSettings** ](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx) propiedad |
| Clase **System.IO.Stream** | Se sigue admitiendo, pero usa ReadAsync() y WriteAsync() en lugar de BeginRead()/EndRead() y BeginWrite()/EndWrite(). |
| Cartera | |
| Espacio de nombres **Microsoft.Phone.Wallet** | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) namespace |
| Xml | |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTime** |
| (SX = **System.Xml**) | Método **SX.XmlConvert.ToDateTimeOffset** |


El siguiente tema es [Migración del proyecto](wpsl-to-uwp-porting-to-a-uwp-project.md).

