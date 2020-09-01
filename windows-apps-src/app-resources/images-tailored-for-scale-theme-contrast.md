---
Description: La aplicación puede cargar archivos de recursos de imagen que contengan imágenes adaptadas al factor de escala de visualización, tema, contraste alto y otros contextos en tiempo de ejecución.
title: Cargar imágenes y recursos adaptados a escala, tema, contraste alto y otros
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 0a1d6639a901385c3a33fb0ed670b9b7e4cf683e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157609"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>Cargar imágenes y recursos adaptados a escala, tema, contraste alto y otros
La aplicación puede cargar archivos de recursos de imagen (u otros archivos de recursos) adaptados para [Mostrar el factor de escala](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), el tema, el contraste alto y otros contextos en tiempo de ejecución. Se puede hacer referencia a estas imágenes desde el código imperativo o desde el marcado XAML, por ejemplo, como la propiedad de **origen** de una **imagen**. También pueden aparecer en el archivo de origen del manifiesto del paquete de la aplicación (el `Package.appxmanifest` archivo) &mdash; , por ejemplo, como el icono de la aplicación en la pestaña activos visuales del diseñador de manifiestos de Visual Studio &mdash; o en los iconos y notificaciones del sistema. Mediante el uso de calificadores en los nombres de archivo de las imágenes y, de forma opcional, cargarlos dinámicamente con la ayuda de un [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live), puede hacer que se cargue el archivo de imagen más adecuado que mejor coincida con la configuración de tiempo de ejecución del usuario para mostrar la escala, el tema, el contraste alto, el lenguaje y otros contextos.

Un recurso de imagen se encuentra en un archivo de recursos de imagen. También puede considerar la imagen como un recurso y el archivo que lo contiene como un archivo de recursos. y puede encontrar estos tipos de archivos de recursos en la carpeta \Assets del proyecto. Para obtener información sobre cómo usar calificadores en los nombres de los archivos de recursos de imagen, consulte [adaptar los recursos para el idioma, la escala y otros calificadores](tailor-resources-lang-scale-contrast.md).

Algunos calificadores comunes para las imágenes son [escala](tailor-resources-lang-scale-contrast.md#scale), [tema](tailor-resources-lang-scale-contrast.md#theme), [contraste](tailor-resources-lang-scale-contrast.md#contrast)y [destino](tailor-resources-lang-scale-contrast.md#targetsize).

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>Calificar un recurso de imagen para la escala, el tema y el contraste
El valor predeterminado para el `scale` calificador es `scale-100` . Por lo tanto, estas dos variantes son equivalentes (ambas proporcionan una imagen a escala 100 o factor de escala 1).

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
</pre>
</blockquote>


Puede usar calificadores en nombres de carpeta en lugar de nombres de archivo. Esta sería una estrategia mejor si tiene varios archivos de recursos por calificador. A efectos de Ilustración, estas dos variantes son equivalentes a las dos anteriores.

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
</pre>
</blockquote>

A continuación se muestra un ejemplo de cómo puede proporcionar variantes de un recurso de imagen &mdash; con el nombre `/Assets/Images/logo.png` &mdash; para diferentes valores de escala, tema y contraste alto. En este ejemplo se usa la nomenclatura de carpetas.

<blockquote>
<pre>
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
</pre>
</blockquote>

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>Referencia a una imagen u otro recurso desde el marcado y el código XAML
El nombre &mdash; o el identificador &mdash; de un recurso de imagen es la ruta de acceso y el nombre de archivo con todos los calificadores quitados. Si denomina carpetas y/o archivos como en cualquiera de los ejemplos de la sección anterior, tiene un recurso de imagen único y su nombre (como una ruta de acceso absoluta) es `/Assets/Images/logo.png` . Aquí se muestra cómo usar ese nombre en el marcado XAML.

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

Tenga en cuenta que usa el `ms-appx` esquema de URI porque está haciendo referencia a un archivo que procede del paquete de la aplicación. Consulte [esquemas de URI](uri-schemes.md). Y aquí se muestra cómo hacer referencia al mismo recurso de imagen en el código imperativo.

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

Puede usar `ms-appx` para cargar cualquier archivo arbitrario desde el paquete de la aplicación.

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

El `ms-appx-web` esquema tiene acceso a los mismos archivos que `ms-appx` , pero en el compartimiento Web.

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

Para cualquiera de los escenarios mostrados en estos ejemplos, use la sobrecarga del [constructor de URI](/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_) que deduce el [UriKind](/dotnet/api/system.urikind). Especifique un URI absoluto válido, incluido el esquema y la autoridad, o simplemente deje que la autoridad tenga como valor predeterminado el paquete de la aplicación como en el ejemplo anterior.

Observe cómo en estos URI de ejemplo el esquema (" `ms-appx` " o " `ms-appx-web` ") va seguido de " `://` ", que va seguido de una ruta de acceso absoluta. En una ruta de acceso absoluta, el " `/` " inicial hace que la ruta de acceso se interprete desde la raíz del paquete.

> [!NOTE]
> Los `ms-resource` esquemas de URI (para [los recursos de cadena](localize-strings-ui-manifest.md)) y `ms-appx(-web)` (para imágenes y otros activos) realizan la coincidencia automática del calificador para buscar el recurso más adecuado para el contexto actual. El `ms-appdata` esquema de URI (que se usa para cargar los datos de la aplicación) no realiza ninguna coincidencia automática, pero puede responder al contenido de [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) y cargar explícitamente los recursos adecuados de los datos de la aplicación con su nombre de archivo físico completo en el URI. Para obtener información sobre los datos de la aplicación, vea [almacenar y recuperar la configuración y otros datos](../design/app-settings/store-and-retrieve-app-data.md)de la aplicación. Los esquemas de URI web (por ejemplo, `http` , `https` y `ftp` ) no realizan una búsqueda de coincidencias automática. Para obtener información sobre lo que debe hacer en ese caso, consulte [hospedaje y carga de imágenes en la nube](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud).

Las rutas de acceso absolutas son una buena opción si los archivos de imagen permanecen donde se encuentran en la estructura del proyecto. Si desea poder trasladar un archivo de imagen, pero está atento a que permanece en la misma ubicación en relación con su archivo de marcado XAML de referencia, en lugar de una ruta de acceso absoluta, puede que desee usar una ruta de acceso relativa al archivo de marcado contenedor. Si lo hace, no necesita usar un esquema de URI. Todavía se beneficiará de la coincidencia automática de calificadores en este caso, pero solo porque se usa la ruta de acceso relativa en el marcado XAML.

```xaml
<Image Source="Assets/Images/logo.png"/>
```

Consulte también [compatibilidad con iconos y notificaciones para el idioma, la escala y el contraste alto](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

## <a name="qualify-an-image-resource-for-targetsize"></a>Calificar un recurso de imagen para el destino
Puede usar los `scale` `targetsize` calificadores y en diferentes variantes del mismo recurso de imagen, pero no puede usarlos ambos en una sola variante de un recurso. Además, debe definir al menos una variante sin un `TargetSize` calificador. Esa variante debe definir un valor para `scale` , o bien dejar que tenga como valor predeterminado `scale-100` . Por lo tanto, estas dos variantes del `/Assets/Square44x44Logo.png` recurso son válidas.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

Y estas dos variantes son válidas. 

<blockquote>
<pre>
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

Pero esta variante no es válida.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200_targetsize-24.png
</pre>
</blockquote>

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>Referencia a un archivo de imagen del manifiesto del paquete de la aplicación
Si cambia el nombre de las carpetas o archivos como en cualquiera de los dos ejemplos válidos de la sección anterior, tiene un único recurso de imagen de icono de aplicación y su nombre (como una ruta de acceso relativa) es `Assets\Square44x44Logo.png` . En el manifiesto del paquete de la aplicación, simplemente haga referencia al recurso por su nombre. No es necesario usar ningún esquema de URI.

![agregar recurso, inglés](images/app-icon.png)

Eso es todo lo que necesita hacer y el sistema operativo realizará la coincidencia automática del calificador para buscar el recurso más adecuado para el contexto actual. Para obtener una lista de todos los elementos del manifiesto de paquete de aplicación que puede localizar o calificar de este modo de esta manera, consulte [elementos de manifiesto localizable](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="qualify-an-image-resource-for-layoutdirection"></a>Calificar un recurso de imagen para LayoutDirection
Vea [creación de reflejo de imágenes](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images).

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>Cargar una imagen para un idioma específico u otro contexto
Para más información sobre la propuesta de valor de localizar la aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md).

La [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) predeterminada (obtenida de [**ResourceContext. GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contiene un valor de calificador para cada nombre de calificador, que representa el contexto de tiempo de ejecución predeterminado (es decir, la configuración de la máquina y el usuario actual). Los archivos de imagen se &mdash; comparan en función de los calificadores de sus nombres &mdash; con los valores de calificador de ese contexto en tiempo de ejecución.

Pero puede haber ocasiones en las que desee que la aplicación invalide la configuración del sistema y sea explícita sobre el idioma, la escala u otro valor de calificador que se usará al buscar una imagen coincidente para cargar. Por ejemplo, puede que desee controlar exactamente cuándo y qué imágenes de contraste alto se cargan.

Para ello, puede crear un nuevo **ResourceContext** (en lugar de usar el valor predeterminado), reemplazar sus valores y, a continuación, usar ese objeto de contexto en las búsquedas de imagen.

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

Para el mismo efecto en un nivel global, *puede* invalidar los valores de calificador en el valor predeterminado de **ResourceContext**. Pero en su lugar, se recomienda llamar a [**ResourceContext. SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Los valores se establecen una vez con una llamada a **SetGlobalQualifierValue** y, a continuación, esos valores están en vigor en el **ResourceContext** predeterminado cada vez que se usa para las búsquedas. De forma predeterminada, la clase [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) usa el valor predeterminado **ResourceContext**.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>Actualizar imágenes en respuesta a eventos de cambio de valor de calificador
La aplicación en ejecución puede responder a los cambios en la configuración del sistema que afectan a los valores de calificador en el contexto de recursos predeterminado. Cualquiera de estas configuraciones del sistema invoca el evento [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) en [**ResourceContext. QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

En respuesta a este evento, puede volver a cargar las imágenes con la ayuda del **ResourceContext**predeterminado, que [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) utiliza de forma predeterminada.

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
* [ResourceContext. SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Temas relacionados
* [Adapte los recursos para el idioma, la escala y otros calificadores](tailor-resources-lang-scale-contrast.md)
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md)
* [Almacenar y recuperar la configuración y otros datos de aplicación](../design/app-settings/store-and-retrieve-app-data.md)
* [Compatibilidad de iconos y notificaciones del sistema para el idioma, la escala y el contraste alto](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)
* [Elementos de manifiesto localizables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Imágenes de creación de reflejo](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [Globalización y localización](../design/globalizing/globalizing-portal.md)