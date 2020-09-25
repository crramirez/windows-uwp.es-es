---
title: Iniciar la aplicación predeterminada para un URI
description: Aprende a iniciar la aplicación predeterminada de un identificador de recursos uniforme (URI). Los URI te permiten iniciar otra aplicación para realizar una tarea específica. En este tema también se proporciona una descripción general de los muchos esquemas de URI integrados en Windows.
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.date: 06/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad25d4ba5d8dfe638d3de3e210f69ea204c48a14
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220018"
---
# <a name="launch-the-default-app-for-a-uri"></a>Iniciar la aplicación predeterminada para un URI


**API importantes**

- [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync)
- [**PreferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
- [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview)

Aprende a iniciar la aplicación predeterminada de un identificador de recursos uniforme (URI). Los URI te permiten iniciar otra aplicación para realizar una tarea específica. En este tema también se proporciona una descripción general de los muchos esquemas de URI integrados en Windows. También puedes iniciar el URI personalizado. Para obtener más información sobre cómo registrar un esquema de URI personalizado y controlar la activación de URI, consulta [Administración de la activación de URI](handle-uri-activation.md).

Los esquemas de URI te permiten abrir aplicaciones haciendo clic en hipervínculos. Al igual que puedes iniciar un nuevo correo electrónico con **mailto:**, puedes abrir el explorador web predeterminado con **http:**.

En este tema se describen los siguientes esquemas de URI integrados en Windows:

| Esquema de URI | Inicia |
| ----------:|----------|
|[bingmaps:, MS-Drive-to: y MS-Walk-to: ](#maps-app-uri-schemes) | Aplicación Mapas |
|[http](#http-uri-scheme) | Explorador web predeterminado |
|[mailto](#email-uri-scheme) | Aplicación de correo electrónico predeterminada |
|[ms-call:](#call-app-uri-scheme) |  Aplicación de llamada |
|[ms-chat:](#messaging-app-uri-scheme) | Aplicación Mensajes |
|[ms-people:](#people-app-uri-scheme) | Aplicación Contactos |
|[MS-fotos:](#photos-app-uri-scheme) | Aplicación Fotos |
|[MS-Settings:](#settings-app-uri-scheme) | Aplicación Configuración |
|[ms-store:](#store-app-uri-scheme)  | Aplicación de la tienda |
|[ms-tonepicker:](#tone-picker-uri-scheme) | Selector de tono |
|[ms-yellowpage:](#nearby-numbers-app-uri-scheme) | Aplicación de números cercanos |
|[msnweather:](#weather-app-uri-scheme) | Aplicación meteorológica |

<br>
Por ejemplo, el siguiente URI abre el explorador predeterminado y muestra el sitio web de Bing.

`https://bing.com`

También puedes iniciar esquemas de URI personalizados. Si no hay ninguna aplicación instalada para controlar ese URI, puedes recomendar una aplicación para que la instale el usuario. Para obtener más información, consulte [recomendar una aplicación si no hay ninguna disponible para controlar el URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

En general, la aplicación no puede seleccionar qué aplicación se inicia, sino que es el usuario quien la determina. Se puede registrar más de una aplicación para controlar el mismo esquema de URI. La excepción a esto son los esquemas de URI reservados. Los registros de esquemas de URI reservados se ignoran. Para obtener una lista completa de los esquemas de URI reservados, consulta [Administración de la activación de URI](handle-uri-activation.md). En casos en los que más de una aplicación puede haber registrado el mismo esquema de URI, la aplicación puede recomendar iniciar una aplicación específica. Para obtener más información, consulte [recomendar una aplicación si no hay ninguna disponible para controlar el URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

### <a name="call-launchuriasync-to-launch-a-uri"></a>Llamar a LaunchUriAsync para iniciar un URI

Usa el método [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) para iniciar un URI. Cuando llames a este método, tu aplicación debe ser la aplicación en primer plano; es decir, debe estar visible para el usuario. Este requisito permite asegurar que el usuario permanezca en control. Para cumplir este requisito, asegúrate de enlazar todos los inicios de URI directamente a la interfaz de usuario de la aplicación. El usuario siempre debe tener que realizar alguna acción para iniciar el URI. Si intentas iniciar un URI y tu aplicación no está en primer plano, se producirá un error en el inicio y se invocará a la devolución de llamada de error.

Primero, crea un objeto [**System.Uri**](/dotnet/api/system.uri) para que represente el URI, y luego pásalo al método [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync). Usa el resultado devuelto para ver si la llamada se realizó correctamente, como se muestra en el siguiente ejemplo.

```cs
private async void launchURI_Click(object sender, RoutedEventArgs e)
{
   // The URI to launch
   var uriBing = new Uri(@"http://www.bing.com");

   // Launch the URI
   var success = await Windows.System.Launcher.LaunchUriAsync(uriBing);

   if (success)
   {
      // URI launched
   }
   else
   {
      // URI launch failed
   }
}
```

En algunos casos, el sistema operativo le pedirá al usuario que compruebe si realmente desea cambiar de aplicación.

![Un cuadro de diálogo de advertencia superpuesto en un fondo atenuado de la aplicación. El cuadro de diálogo pregunta al usuario si quiere cambiar de aplicación y tiene los botones "Sí" y "No" en la parte inferior derecha. El botón "No" está resaltado.](images/warningdialog.png)

Si siempre desea que se produzca este mensaje, use el [**Windows.SysTEM. Propiedad LauncherOptions. TreatAsUntrusted**](/uwp/api/windows.system.launcheroptions.treatasuntrusted) para indicar al sistema operativo que debe mostrar una advertencia.

```cs
// The URI to launch
var uriBing = new Uri(@"http://www.bing.com");

// Set the option to show a warning
var promptOptions = new Windows.System.LauncherOptions();
promptOptions.TreatAsUntrusted = true;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriBing, promptOptions);
```

### <a name="recommend-an-app-if-one-is-not-available-to-handle-the-uri"></a>Recomendar una aplicación si no hay ninguna disponible para administrar el URI

En algunos casos, es posible que el usuario no tenga instalada una aplicación para administrar el URI que estás iniciando. Si esto sucede, de manera predeterminada, el sistema operativo ofrece un vínculo al usuario para que busque una aplicación apropiada en la Tienda. Si quieres recomendar al usuario qué aplicación comprar en este escenario, debes pasar la recomendación junto con el URI que estás iniciando.

Las recomendaciones también son útiles cuando más de una aplicación se ha registrado para controlar un esquema de URI. Al recomendar una aplicación específica, Windows abrirá esa aplicación si ya está instalada.

Para hacer una recomendación, llama al método [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)**](/uwp/api/windows.system.launcher.launchuriasync#Windows_System_Launcher_LaunchUriAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_) con [**LauncherOptions.preferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname) establecido con el nombre de familia de paquete correspondiente a la aplicación de la Tienda que quieras recomendar. El sistema operativo usará esta información para reemplazar la opción general de buscar una aplicación por una opción específica para comprar la aplicación recomendada en la Tienda.

```cs
// Set the recommended app
var options = new Windows.System.LauncherOptions();
options.PreferredApplicationPackageFamilyName = "Contoso.URIApp_8wknc82po1e";
options.PreferredApplicationDisplayName = "Contoso URI Ap";

// Launch the URI and pass in the recommended app
// in case the user has no apps installed to handle the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

### <a name="set-remaining-view-preference"></a>Establecer las preferencias de visualización restantes

Las aplicaciones de origen que llaman a [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) pueden solicitar permanecer en pantalla después de iniciarse un URI. Windows intenta compartir de manera predeterminada todo el espacio disponible entre la aplicación de origen y la aplicación de destino que controla el URI. Las aplicaciones de origen pueden usar la propiedad [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) para indicar al sistema operativo que prefieren que la ventana de la aplicación ocupe más o menos espacio del que hay disponible. También se puede usar **DesiredRemainingView** para indicar que la aplicación de origen no necesita permanecer en pantalla después del inicio del URI y puede sustituirse por completo por la aplicación de destino. Esta propiedad especifica únicamente el tamaño de ventana preferido de la aplicación que llama; no especifica el comportamiento de ninguna otra aplicación que también esté en pantalla al mismo tiempo.

**Nota:**    Windows tiene en cuenta varios factores diferentes cuando determina el tamaño de ventana final de la aplicación de origen, por ejemplo, la preferencia de la aplicación de origen, el número de aplicaciones en pantalla, la orientación de la pantalla, etc. Aunque establezcas la propiedad [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview), no se garantiza un comportamiento de ventanas específico para la aplicación de origen.

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## <a name="uri-schemes"></a>Esquemas de URI ##

A continuación se describen los diversos esquemas de URI.

### <a name="call-app-uri-scheme"></a>Llamar al esquema de URI de la aplicación

Use el esquema de URI **MS-Call:** para iniciar la aplicación de llamada.

| Esquema de URI       | Resultado                   |
|------------------|--------------------------|
| ms-call:settings | Llama a la página de configuración de la aplicación. |

### <a name="email-uri-scheme"></a>Esquema de URI de correo electrónico

Use el esquema **mailto:** URI para iniciar la aplicación de correo electrónico predeterminada.

| Esquema de URI |Results                          |
|------------|---------------------------------|
| mailto:    | Inicia la aplicación de correo electrónico predeterminada. |
| mailto: \[ dirección de correo electrónico\] | Inicia la aplicación de correo electrónico y crea un mensaje nuevo con la dirección de correo electrónico especificada en la línea "Para". Ten en cuenta que el correo electrónico no se envía hasta que el usuario presiona "Enviar". |

### <a name="http-uri-scheme"></a>Esquema de URI de HTTP

Use el esquema **http:** URI para iniciar el explorador Web predeterminado.

| Esquema de URI | Results                           |
|------------|-----------------------------------|
| http:      | Abre el explorador web predeterminado. |

### <a name="maps-app-uri-schemes"></a>Esquemas de URI de la aplicación Mapas

Use los esquemas **bingmaps:**, **MS-Drive-to:** y **MS-Walk-to:** URI para [iniciar la aplicación de Windows Maps](launch-maps-app.md) en mapas, direcciones y resultados de búsqueda específicos. Por ejemplo, el siguiente URI abre la aplicación Mapas de Windows y muestra un mapa centrado sobre la ciudad de Nueva York.

`bingmaps:?cp=40.726966~-74.006076`

![Ejemplo de la aplicación Mapas de Windows.](images/mapnyc.png)

Para obtener más información, consulta [Iniciar la aplicación Mapas de Windows](launch-maps-app.md). Para usar el control de mapa en tu propia aplicación, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](../maps-and-location/display-maps.md).

### <a name="messaging-app-uri-scheme"></a>Esquema de URI de la aplicación Mensajes

Use el esquema **MS-chat:** URI para iniciar la aplicación de mensajería de Windows.

| Esquema de URI |Results |
|------------|--------|
| ms-chat:   | Inicia la aplicación de mensajería. |
| ms-chat:?ContactID={contacted}  |  Permite que la aplicación de mensajería se inicie con información de un contacto determinado.   |
| ms-chat:?Body={body} | Permite que la aplicación de mensajería se inicie con una cadena que se usará como el contenido del mensaje.|
| ms-chat:?Addresses={address}&Body={body} | Permite que la aplicación de mensajería se inicie con información de unas direcciones concretas, y con una cadena que se usará como el contenido del mensaje. Nota: Las direcciones se pueden concatenar. |
| ms-chat:?TransportId={transportId}  | Permite que la aplicación de mensajería se inicie con un identificador de transporte concreto. |

### <a name="tone-picker-uri-scheme"></a>Esquema de URI de selector de tono

Use el esquema **MS-tonepicker:** URI para elegir tonos, alarmas y tonos del sistema. También puedes guardar nuevos tonos y obtener el nombre para mostrar de un tono.

| Esquema de URI | Results |
|------------|---------|
| ms-tonepicker: | Elige tonos, alarmas y tonos del sistema. |

Los parámetros se pasan a través de una clase [ValueSet](/uwp/api/windows.foundation.collections.valueset) a la API LaunchURI. Consulta [Elegir y guardar los tonos con el esquema URI ms-tonepicker](launch-ringtone-picker.md) para obtener más información.

### <a name="nearby-numbers-app-uri-scheme"></a>Esquema de URI de la aplicación de números cercanos

Use el esquema **MS-yellowpage:** URI para iniciar la aplicación de números cercanos.

| Esquema de URI | Results |
|------------|---------|
| MS-yellowpage:? Input = \[ keyword \]&Method = \[ String o T9\] | Inicia la aplicación de números cercanos.<br>`input` hace referencia a la palabra clave que desea buscar.<br>`method` hace referencia al tipo de búsqueda (cadena o búsqueda T9).<br>Si `method`es `T9` (un tipo de teclado), `keyword` debe ser una cadena numérica que se asigna a las letras del teclado T9 que se van a buscar.<br>Si `method`es `String` , `keyword` es la palabra clave que se va a buscar. |

### <a name="people-app-uri-scheme"></a>Esquema de URI de la aplicación Contactos

Use el esquema **MS-People:** URI para iniciar la aplicación People.
Para obtener más información, consulta [Iniciar la aplicación Contactos](launch-people-apps.md).

### <a name="photos-app-uri-scheme"></a>Esquema de URI de aplicación de fotos

Use el esquema **MS-Photos:** URI para iniciar la aplicación fotos para ver una imagen o editar un vídeo. Por ejemplo:  
Para ver una imagen: `ms-photos:viewer?fileName=c:\users\userName\Pictures\image.jpg`  
O bien, para editar un vídeo: `ms-photos:videoedit?InputToken=123abc&Action=Trim&StartTime=01:02:03`  

> [!NOTE]
> Los URI para editar un vídeo o mostrar una imagen solo están disponibles en el escritorio.

| Esquema de URI |Results |
|------------|--------|
| MS-Photos: Viewer? fileName = {FILENAME} | Inicia la aplicación fotos para ver la imagen especificada donde {FILENAME} es un nombre de ruta de acceso completo. Por ejemplo: `c:\users\userName\Pictures\ImageToView.jpg` |
| MS-Photos: videoedit? InputToken = {token de entrada} | Inicia la aplicación fotos en el modo de edición de vídeo para el archivo representado por el token de archivo. **InputToken** es obligatorio. Use  [SharedStorageAccessManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager) para obtener un token para un archivo. |
| MS-Photos: videoedit? Acción = {Action} | Un parámetro que indica el modo de edición de vídeo en el que se va a abrir la aplicación de fotos, donde {Action} es uno de los siguientes: **slowmotion**, **FrameExtraction**, **Trim**, **View**y **Ink**. La **acción** es obligatoria. |
| MS-Photos: videoedit? StartTime = {TimeSpan} | Parámetro opcional que especifica dónde empezar a reproducir el vídeo. `{timespan}` debe tener el formato `"hh:mm:ss.ffff"` . Si no se especifica, el valor predeterminado es. `00:00:00.0000` |

### <a name="settings-app-uri-scheme"></a>Esquema de URI de la aplicación Configuración

Use el esquema **MS-Settings:** URI para [iniciar la aplicación de configuración de Windows](launch-settings-app.md). El inicio de la aplicación Configuración es una parte importante de la programación de una aplicación compatible con la privacidad. Si la aplicación no puede obtener acceso a un recurso con información confidencial, se recomienda proporcionar al usuario un vínculo a la configuración de privacidad de ese recurso. Por ejemplo, el siguiente URI abre la aplicación Configuración y muestra la configuración de privacidad de la cámara.

`ms-settings:privacy-webcam`

![Configuración de privacidad de la cámara.](images/privacyawarenesssettingsapp.png)

Para obtener más información, consulta [Iniciar la aplicación Configuración de Windows](launch-settings-app.md) y [Directrices para aplicaciones compatibles con la privacidad](../security/index.md).

### <a name="store-app-uri-scheme"></a>Esquema de URI de la aplicación de la Tienda

Use el esquema de URI **MS-Windows-Store:** para [iniciar la aplicación para UWP](launch-store-app.md). Abra las páginas de detalles del producto, las páginas de revisión del producto y las páginas de búsqueda, etc. Por ejemplo, el siguiente URI abre la aplicación de UWP e inicia la Página principal de la tienda.

`ms-windows-store://home/`

Para obtener más información, consulte [Inicio de la aplicación para UWP](launch-store-app.md).

### <a name="weather-app-uri-scheme"></a>Esquema de URI de la aplicación meteorológico

Use el esquema de URI **msnweather:** para iniciar la aplicación meteorológica.

| Esquema de URI | Results |
|------------|---------|
| msnweather://Forecast? la = \[ latitud \]&lo = \[ longitud\] | Inicia la aplicación Weather en la página de previsión en función de las coordenadas geográficas de ubicación.<br>`latitude` hace referencia a la latitud de la ubicación.<br> `longitude` hace referencia a la longitud de la ubicación.<br> |