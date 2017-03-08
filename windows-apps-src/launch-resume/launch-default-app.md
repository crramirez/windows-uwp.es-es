---
author: TylerMSFT
title: "Iniciar la aplicación predeterminada de un URI"
description: "Aprende a iniciar la aplicación predeterminada de un identificador de recursos uniforme (URI). Los URI te permiten iniciar otra aplicación para realizar una tarea específica. En este tema también se proporciona una descripción general de los muchos esquemas de URI integrados en Windows."
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: fcc1d056fc3a4cb8d57ae5082e62cf0b802bb4e7
ms.lasthandoff: 02/07/2017

---

# <a name="launch-the-default-app-for-a-uri"></a>Iniciar la aplicación predeterminada para un URI

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**API importantes**

- [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-  [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
- [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Aprende a iniciar la aplicación predeterminada de un identificador de recursos uniforme (URI). Los URI te permiten iniciar otra aplicación para realizar una tarea específica. En este tema también se proporciona una descripción general de los muchos esquemas de URI integrados en Windows. También puedes iniciar el URI personalizado. Para obtener más información sobre cómo registrar un esquema de URI personalizado y controlar la activación de URI, consulta [Administración de la activación de URI](handle-uri-activation.md).


Los esquemas de URI te permiten abrir aplicaciones haciendo clic en hipervínculos. Al igual que puedes iniciar un nuevo correo electrónico con **mailto:**, puedes abrir el explorador web predeterminado con **http:**.

En este tema se describen los siguientes esquemas de URI integrados en Windows:

| Esquema de URI | Inicia |
| -------|--------------|
|[bingmaps:, ms-drive-to: y ms-walk-to: ](#maps-app-uri-schemes) | Aplicación Mapas |
|[http:](#http-uri-scheme) | Explorador web predeterminado |
|[mailto:](#email-uri-scheme) | Aplicación de correo electrónico predeterminada |
|[ms-call:](#call-app-uri-scheme) |  Aplicación de llamada |
|[ms-chat:](#messaging-app-uri-scheme) | Aplicación Mensajes |
|[ms-people:](#people-app-uri-scheme) | Aplicación Contactos |
|[ms-settings:](#settings-app-uri-scheme) | Aplicación Configuración |
|[ms-store:](#store-app-uri-scheme)  | Aplicación de la Tienda |
|[ms-tonepicker:](#tone-picker-uri-scheme) | Selector de tono |
|[ms-yellowpage:](#nearby-numbers-app-uri-scheme) | Aplicación de números cercanos |

<br> Por ejemplo, el siguiente URI abre el explorador predeterminado y muestra el sitio web de Bing.

`http://bing.com`

También puedes iniciar esquemas de URI personalizados. Si no hay ninguna aplicación instalada para controlar ese URI, puedes recomendar una aplicación para que la instale el usuario. Para obtener más información, consulta [Recomendar una aplicación si no hay ninguna disponible para administrar el URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

En general, la aplicación no puede seleccionar qué aplicación se inicia. sino que es el usuario el que determina la aplicación que se inicia. Se puede registrar más de una aplicación para controlar el mismo esquema de URI. La excepción a esto son los esquemas de URI reservados. Los registros de esquemas de URI reservados se ignoran. Para obtener una lista completa de los esquemas de URI reservados, consulta [Administración de la activación de URI](handle-uri-activation.md). En casos en los que más de una aplicación puede haber registrado el mismo esquema de URI, la aplicación puede recomendar iniciar una aplicación específica. Para obtener más información, consulta [Recomendar una aplicación si no hay ninguna disponible para administrar el URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

### <a name="call-launchuriasync-to-launch-a-uri"></a>Llamar a LaunchUriAsync para iniciar un URI

Usa el método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar un URI. Cuando llames a este método, tu aplicación debe ser la aplicación en primer plano; es decir, debe estar visible para el usuario. Este requisito permite asegurar que el usuario permanezca en control. Para cumplir este requisito, asegúrate de enlazar todos los inicios de URI directamente a la interfaz de usuario de la aplicación. El usuario siempre debe tener que realizar alguna acción para iniciar el URI. Si intentas iniciar un URI y tu aplicación no está en primer plano, se producirá un error en el inicio y se invocará a la devolución de llamada de error.

Primero, crea un objeto [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/system.uri.aspx) para que represente el URI, y luego pásalo al método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476). Usa el resultado devuelto para ver si la llamada se realizó correctamente, como se muestra en el siguiente ejemplo.

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

En algunos casos, el sistema operativo pedirá al usuario que vea si realmente quiere cambiar las aplicaciones.

![Un cuadro de diálogo de advertencia superpuesto en un fondo atenuado de la aplicación. El cuadro de diálogo pregunta al usuario si quiere cambiar de aplicación y tiene los botones "Sí" y "No" en la parte inferior derecha. El botón "No" está resaltado.](images/warningdialog.png)

Si quieres que esta pregunta aparezca siempre, usa la propiedad [**Windows.System.LauncherOptions.TreatAsUntrusted**](https://msdn.microsoft.com/library/windows/apps/hh701442) para indicar al sistema operativo que muestre una advertencia.

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

Para hacer una recomendación, llama al método [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701484) con [**LauncherOptions.preferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) establecido con el nombre de familia de paquete correspondiente a la aplicación de la Tienda que quieras recomendar. El sistema operativo usará esta información para reemplazar la opción general de buscar una aplicación por una opción específica para comprar la aplicación recomendada en la Tienda.

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

Las aplicaciones de origen que llaman a [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) pueden solicitar permanecer en pantalla después de iniciarse un URI. Windows intenta compartir de manera predeterminada todo el espacio disponible entre la aplicación de origen y la aplicación de destino que controla el URI. Las aplicaciones de origen pueden usar la propiedad [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) para indicar al sistema operativo que prefieren que la ventana de la aplicación ocupe más o menos espacio del que hay disponible. **DesiredRemainingView** también se puede usar para indicar que la aplicación de origen no necesita permanecer en pantalla después del inicio del URI y puede sustituirse por completo por la aplicación de destino. Esta propiedad especifica únicamente el tamaño de ventana preferido de la aplicación que llama; no especifica el comportamiento de ninguna otra aplicación que también esté en pantalla al mismo tiempo.

**Nota** Windows tiene en cuenta diferentes factores a la hora de determinar el tamaño final de la ventana de la aplicación de origen como, por ejemplo, la preferencia de la aplicación de origen, el número de aplicaciones en pantalla, la orientación de la pantalla, etc. Aunque establezcas la propiedad [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314), no se garantiza un comportamiento de ventanas específico para la aplicación de origen.

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## <a name="uri-schemes"></a>Esquemas de URI ##

A continuación se describen los diversos esquemas de URI.
<br>

### <a name="call-app-uri-scheme"></a>Llamar al esquema de URI de la aplicación

La aplicación puede usar el esquema de URI **ms-call:** para iniciar la aplicación de llamada.

| Esquema de URI       | Resultado                   |
|------------------|--------------------------|
| ms-call:settings | Llama a la página de configuración de la aplicación. | 
<br>
### <a name="email-uri-scheme"></a>Esquema de URI de correo electrónico

La aplicación puede usar los esquemas de URI **mailto:** para iniciar la aplicación de correo predeterminada.

| Esquema de URI               | Resultados                                                                                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mailto:                  | Inicia la aplicación de correo electrónico predeterminada.                                                                                                                             |
| mailto:\[email address\] | Inicia la aplicación de correo electrónico y crea un mensaje nuevo con la dirección de correo electrónico especificada en la línea "Para". Ten en cuenta que el correo electrónico no se envía hasta que el usuario presiona "Enviar". |
<br>
### <a name="http-uri-scheme"></a>Esquema de URI de HTTP

La aplicación puede usar los esquemas de URI **http:** para iniciar el explorador web predeterminado.

| Esquema de URI | Resultados                           |
|------------|-----------------------------------|
| http:      | Abre el explorador web predeterminado. |
<br>
### <a name="maps-app-uri-schemes"></a>Esquemas de URI de la aplicación Mapas

La aplicación puede usar los esquemas de URI **bingmaps:**, **ms-drive-to:** y **ms-walk-to:** para [iniciar la aplicación Mapas de Windows](launch-maps-app.md) para ver mapas, indicaciones y resultados de búsqueda específicos. Por ejemplo, el siguiente URI abre la aplicación Mapas de Windows y muestra un mapa centrado sobre la ciudad de Nueva York.

`bingmaps:?cp=40.726966~-74.006076`

![Ejemplo de la aplicación Mapas de Windows.](images/mapnyc.png)

Para obtener más información, consulta [Iniciar la aplicación Mapas de Windows](launch-maps-app.md). Para usar el control de mapa en tu propia aplicación, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](https://msdn.microsoft.com/library/windows/apps/mt219695).
<br>
### <a name="messaging-app-uri-scheme"></a>Esquema de URI de la aplicación Mensajes

La aplicación puede usar el esquema de URI **ms-chat:** para iniciar la aplicación Mensajes de Windows.

| Esquema de URI |Resultado | |-- ---------|--------| | ms-chat:   | Inicia la aplicación Mensajes. | | ms-chat:?ContactID={contacted}  |  Permite que la aplicación Mensajes se inicie con información de un contacto determinado.   | | ms-chat:?Body={body} | Permite que la aplicación Mensajes se inicie con una cadena que se usará como el contenido del mensaje.| | ms-chat:?Addresses={address}&Body={body} | Permite que la aplicación Mensajes se inicie con información de direcciones concretas y con una cadena que se usará como el contenido del mensaje. Nota: Las direcciones se pueden concatenar. | | ms-chat:?TransportId={transportId}  | Permite que la aplicación Mensajes se inicie con un identificador de transporte concreto. |
<br>
### <a name="tone-picker-uri-scheme"></a>Esquema de URI de selector de tono

La aplicación puede usar esquema de URI **ms-tonepicker:** para elegir tonos, alarmas y tonos del sistema. También puedes guardar nuevos tonos y obtener el nombre para mostrar de un tono.

| Esquema de URI | Resultados |
|------------|---------|
| ms-tonepicker: | Elige tonos, alarmas y tonos del sistema. |

Los parámetros se pasan a través de una clase [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) a la API LaunchURI. Consulta [Elegir y guardar los tonos con el esquema URI ms-tonepicker](launch-ringtone-picker.md) para obtener más información.

### <a name="nearby-numbers-app-uri-scheme"></a>Esquema de URI de la aplicación de números cercanos
<br>
La aplicación puede usar el esquema de URI **ms-yellowpage:** para iniciar la aplicación de números cercanos.

| Esquema de URI | Resultados |
|------------|---------|
| ms-yellowpage:?input=\[keyword\]&method=\[String o T9\] | Inicia la aplicación de números cercanos. `input` hace referencia a la palabra clave que quieres buscar. `method` hace referencia al tipo de búsqueda (cadena o T9). <br> Si `method`es `T9` (un tipo de teclado), `keyword` debe ser una cadena numérica que se asigna a las letras del teclado T9 que se van a buscar.<br>Si `method`es `String` , `keyword` es la palabra clave que se va a buscar. |
 
<br>
### <a name="people-app-uri-scheme"></a>Esquema de URI de la aplicación Contactos

La aplicación puede usar el esquema de URI **ms-people:** para iniciar la aplicación Contactos.
Para obtener más información, consulta [Iniciar la aplicación Contactos](launch-people-apps.md).

<br>
### <a name="settings-app-uri-scheme"></a>Esquema de URI de la aplicación Configuración

La aplicación puede usar el esquema de URI **ms-settings:** para [iniciar la aplicación Configuración de Windows](launch-settings-app.md). El inicio de la aplicación Configuración es una parte importante de la programación de una aplicación compatible con la privacidad. Si la aplicación no puede obtener acceso a un recurso con información confidencial, se recomienda proporcionar al usuario un vínculo a la configuración de privacidad de ese recurso. Por ejemplo, el siguiente URI abre la aplicación Configuración y muestra la configuración de privacidad de la cámara.

`ms-settings:privacy-webcam`

![Configuración de privacidad de la cámara.](images/privacyawarenesssettingsapp.png)

Para obtener más información, consulta [Iniciar la aplicación Configuración de Windows](launch-settings-app.md) y [Directrices para aplicaciones compatibles con la privacidad](https://msdn.microsoft.com/library/windows/apps/hh768223).

<br>
### <a name="store-app-uri-scheme"></a>Esquema de URI de la aplicación de la Tienda

La aplicación puede usar el esquema de URI **ms-windows-store:** para [iniciar la aplicación de la Tienda Windows](launch-store-app.md). Permite abrir páginas de detalles del producto, páginas de revisión del producto, páginas de búsqueda, etc. Por ejemplo, el siguiente URI abre la aplicación de la Tienda Windows e inicia la página principal de la Tienda.

`ms-windows-store://home/`

Para obtener más información, consulta [Iniciar la aplicación de la Tienda Windows](launch-store-app.md).

