---
Description: Your app can load image resource files containing images tailored for display scale factor, theme, high contrast, and other runtime contexts.
title: Cargar imágenes y recursos adaptados a escala, tema, contraste alto y otros
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: 6f4749b8560624ed58f43b33fe3373d909919347
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7968793"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>Cargar imágenes y activos adaptados a escala, tema, contraste alto y otros
La aplicación puede cargar archivos de recursos de imagen (u otros archivos de activos) adaptados para [factor de escala de visualización](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), tema, contraste alto y otros contextos de tiempo de ejecución. Estas imágenes pueden referenciarse desde código imperativo o desde marcado XAML, por ejemplo como la propiedad **Origen** de una **Imagen**. También pueden aparecer en el archivo de origen del manifiesto del paquete de la aplicación (el archivo `Package.appxmanifest`), por ejemplo, como el valor de icono de la aplicación en la pestaña de activos visuales del Diseñador de manifiestos de Visual Studio en los iconos y notificaciones del sistema. Con calificadores en los nombres de archivo de las imágenes y, opcionalmente, cargándolas dinámicamente con ayuda de un [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live), puedes hacer que se cargue el archivo de imagen más adecuado que coincida mejor con la configuración de tiempo de ejecución del usuario en lo relativo a escala de la pantalla, tema, contraste alto, idioma y otros contextos.

Un recurso de imagen se encuentra en un archivo de recursos de imagen. También puedes pensar en la imagen como un activo, y el archivo que lo contiene como un archivo de activo; y puedes encontrar estos tipos de archivos de recursos en la carpeta \Assets del proyecto. Para información general sobre cómo usar calificadores en los nombres de los archivos de recursos de imagen, consulta [Adaptar los recursos de idioma, escala y otros calificadores](tailor-resources-lang-scale-contrast.md).

Algunos calificadores usuales de imágenes son [scale](tailor-resources-lang-scale-contrast.md#scale), [theme](tailor-resources-lang-scale-contrast.md#theme), [contrast](tailor-resources-lang-scale-contrast.md#contrast) y [targetsize](tailor-resources-lang-scale-contrast.md#targetsize).

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>Calificar un recurso de imagen para la escala, el tema y el contraste
El valor predeterminado del calificador `scale` es `scale-100`. Por lo tanto, son equivalentes estas dos variantes (ambas proporcionan una imagen a escala 100, o factor de escala 1).

```
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
```

Puedes usar los calificadores de los nombres de carpetas en lugar de nombre de archivos. Si tienes varios archivos de activos por calificador, esa sería una estrategia más acertada. Con fines de ilustración, estas dos variantes son equivalentes a las dos anteriores.

```
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
```

A continuación viene un ejemplo de cómo puedes proporcionar variantes de un recurso de imagen, denominado `/Assets/Images/logo.png` para configuraciones diferentes de escala de la pantalla, tema y contraste alto. Este ejemplo usa nombres de carpeta.

```
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
```

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>Hacer referencia a una imagen u otros activos de código y marcado XAML
El nombre o identificador de un recurso de imagen es el nombre de ruta de acceso y el del archivo, con todos los calificadores quitados. Si das nombres a carpetas o archivos como en cualquiera de los ejemplos de la sección anterior, en ese caso tienes un único recurso de imagen y su nombre (como ruta absoluta) es `/Assets/Images/logo.png`. Esta es la manera de usar este nombre en marcado XAML.

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

Ten en cuenta que usas el esquema URI `ms-appx` porque estás haciendo referencia a un archivo que procede de paquete de la aplicación. Consulta [esquemas URI](uri-schemes.md). Y aquí te mostramos cómo hacer referencia al mismo recurso de imagen en código imperativo.

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

Puedes usar `ms-appx` para cargar cualquier archivo arbitrario de tu paquete de la aplicación.

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

El esquema `ms-appx-web` accede a los mismos archivos que `ms-appx`, pero en el compartimiento web.

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

Para cualquiera de los escenarios que se muestran en estos ejemplos, usa la sobrecarga [constructor Uri](https://docs.microsoft.com/en-us/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_) que infiere el [UriKind](https://docs.microsoft.com/en-us/dotnet/api/system.urikind). Especifica un URI absoluto válido, incluido el esquema y autoridad, o simplemente deja que la autoridad sea predeterminada para el paquete de la aplicación, como en el ejemplo anterior.

Observa cómo en estos URI de ejemplo, el esquema ("`ms-appx`" o "`ms-appx-web`") va seguido de "`://`", seguido a su vez por una ruta de acceso absoluta. En una ruta de acceso absoluta, el símbolo "`/`" al comienzo hace que la ruta de acceso se interprete desde la raíz del paquete.

**Nota** Los esquemas URI `ms-resource` (para [recursos de cadena](localize-strings-ui-manifest.md)) y `ms-appx(-web)` (para imágenes y otros activos) llevan a cabo la confrontación de calificadores automática, para encontrar el recurso más apropiado para el contexto actual. El esquema de URI `ms-appdata` (que se usa para cargar datos de la aplicación) no realiza ninguna confrontación automática de este tipo, pero puede responderse al contenido de [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) y cargar explícitamente los activos adecuados desde los datos de la aplicación mediante su nombre completo del archivo físico en el URI. Para más información sobre los datos de aplicaciones, consulta [Almacenar y recuperar la configuración y otros datos de aplicaciones](../design/app-settings/store-and-retrieve-app-data.md). Los esquemas de URI web (por ejemplo, `http`, `https` y `ftp`) no realizan tampoco la comparación automática. Para obtener información sobre qué debes hacer en ese caso, consulta [Alojar y cargar imágenes en la nube](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud).

Las rutas de acceso absolutas son una buena opción si los archivos de imagen semantienen donde están en la estructura del proyecto. Si quieres poder mover un archivo de imagen, pero te importa que permanezca en la misma ubicación en relación con su archivo de marcado XAML de referencia, a continuación, en lugar de una ruta absoluta, podría convenirte usar una ruta relativa al archivo de marcado que lo contiene. Si lo haces, no necesitas usar un esquema de URI. En este caso, seguirás beneficiándote de la coincidencia de calificadores automática, pero solo porque estás usando la ruta de acceso relativa en el marcado XAML.

```xaml
<Image Source="Assets/Images/logo.png"/>
```

Consulta también [Compatibilidad de ventanas y notificaciones del sistema para idioma, escala y contraste alto](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

## <a name="qualify-an-image-resource-for-targetsize"></a>Calificar un recurso de imagen para targetsize
Puedes usar los calificadores `scale`y `targetsize` en variantes distintas del mismo recurso de imagen; pero no puedes usar ambos en una única variante de un recurso. También debes definir al menos una variante sin un calificador `TargetSize`. Dicha variante debe definir un valor para `scale`, o dejar el valor predeterminado `scale-100`. Por lo tanto, estas dos variantes del recurso `/Assets/Square44x44Logo.png` son válidas.

```
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
```

Y estas dos variantes son válidas. 

```
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
```

Pero esta variante no es válida.

```
\Assets\Square44x44Logo.scale-200_targetsize-24.png
```

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>Hacer referencia a un archivo de imagen desde el manifiesto de paquete de la aplicación
Si das nombres a carpetas o archivos como en cualquiera de los dos ejemplos válidos de la sección anterior, tendrás un único recurso de imagen de icono de aplicación y su nombre (como ruta relativa) será `Assets\Square44x44Logo.png`. En el manifiesto de paquete de la aplicación, simplemente haz referencia al recurso por su nombre. No es necesario usar ningún esquema de URI.

![agregar recurso, inglés](images/app-icon.png)

Eso es todo lo que debes hacer; y el sistema operativo llevará a cabo la comparación automática de calificadores para buscar el recurso que sea más adecuado para el contexto actual. Para obtener una lista de todos los elementos del manifiesto del paquete de la aplicación que se pueden localizar o, por el contrario, calificar de esta forma, consulta [Elementos de manifiesto localizables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="qualify-an-image-resource-for-layoutdirection"></a>Calificar un recurso de imagen para layoutdirection
Consulta [Creación de espejos de imágenes](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images).

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>Cargar una imagen para un idioma u otro contexto específicos
Para obtener más información sobre la propuesta de valor de localizar tu aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md).

El valor predeterminado [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) (obtenido de [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contiene un valor de calificador para cada nombre de calificador, que representa el contexto en tiempo de ejecución predeterminado (en otras palabras, la configuración de la máquina y usuario actuales). Los archivos de imagen se comparan, en función de los calificadores de sus nombres, con los valores de calificador de ese contexto de tiempo de ejecución.

Pero es posible que haya veces en las que quieras que tu aplicación invalide la configuración del sistema y ser explícito sobre el idioma, escala u otro valor de calificador que utilices cuando busques una imagen coincidente para cargar. Por ejemplo, es posible que quieras controlar exactamente cuándo y qué imágenes de alto contraste se cargan.

Puedes hacerlo creando un nuevo **ResourceContext** (en lugar de usar el predeterminado), reemplazando sus valores y, a continuación, usando ese objeto de contexto en tus búsquedas de imagen.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView 
resourceContext.QualifierValues["Contrast"] = "high";
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
var resourceCandidate = namedResource.Resolve(resourceContext);
var imageFileStream = resourceCandidate.GetValueAsStreamAsync().GetResults();
var bitmapImage = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
bitmapImage.SetSourceAsync(imageFileStream);
this.myXAMLImageElement.Source = bitmapImage;
```

Para lograr el mismo efecto a un nivel global, *puedes* reemplazar los valores de calificador en el **ResourceContext** predeterminado. Pero, en su lugar, se recomienda llamar a [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Únicamente estableces valores una vez con una llamada a **SetGlobalQualifierValue** y, a continuación, esos valores están en vigor en el **ResourceContext** predeterminado cada vez que los usas para búsquedas. De manera predeterminada, la clase [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) usa el **ResourceContext** predeterminado.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>Actualización de imágenes en respuesta a eventos de cambio de valor de calificador
La aplicación en ejecución puede responder a cambios en la configuración del sistema que afectan a los valores de calificador en el contexto de recursos predeterminado. Cualquiera de estas opciones de configuración del sistema invocan al evento [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) en [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

En respuesta a este evento, puedes volver a cargar tus imágenes con ayuda del **ResourceContext** predeterminado, que usa de forma predeterminada [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live).

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myImageXAMLElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIImages();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIImages());
    }
}

private void RefreshUIImages()
{
    var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
    this.myImageXAMLElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
}
```

## <a name="important-apis"></a>API importantes
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Temas relacionados
* [Adaptar los recursos al idioma, escala y otros calificadores](tailor-resources-lang-scale-contrast.md)
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md)
* [Almacenar y recuperar la configuración y otros datos de aplicación](../design/app-settings/store-and-retrieve-app-data.md)
* [Compatibilidad de ventanas y notificaciones del sistema para idioma, escala y contraste alto.](tile-toast-language-scale-contrast.md)
* [Elementos de manifiesto localizables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Creación de imágenes reflejadas](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [Globalización y localización](../design/globalizing/globalizing-portal.md)
