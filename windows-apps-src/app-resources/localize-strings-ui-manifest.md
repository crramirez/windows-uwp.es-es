---
Description: Si quieres que tu aplicación admita diferentes idiomas de pantalla y tienes literales de cadena en el código o en el marcado XAML, o bien en el manifiesto de paquete de la aplicación, en ese caso mueve esas cadenas a un archivo de recursos (.resw). A continuación, puedes hacer una copia traducida de ese archivo de recursos para cada idioma que admita la aplicación.
title: Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de aplicación
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.date: 11/01/2017
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: 23cd899a196fbe3d28b7156890d65e90ac88cdad
ms.sourcegitcommit: 9f097438937539f94b6a14a09ee65d30f71da9c6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68223963"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de aplicación

Para obtener más información sobre la propuesta de valor de localizar tu aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md).

Si quieres que tu aplicación admita diferentes idiomas de pantalla y tienes literales de cadena en el código o en el marcado XAML, o bien en el manifiesto de paquete de la aplicación, en ese caso mueve esas cadenas a un archivo de recursos (.resw). A continuación, puedes hacer una copia traducida de ese archivo de recursos para cada idioma que admita la aplicación.

Los literales de cadena incrustados en el código fuente pueden aparecer en código imperativo o en el marcado XAML, por ejemplo, como la propiedad **Texto** de un **TextBlock**. También pueden aparecer en el archivo de origen del manifiesto del paquete de la aplicación (el archivo `Package.appxmanifest`), por ejemplo, como el valor del nombre para mostrar de la pestaña de la aplicación del diseñador de manifiestos de Visual Studio. Mover estas cadenas a un archivo de recursos de (.resw) y sustituir los literales de cadena escritos directamente en el código fuente de la aplicación y en el manifiesto de referencias a identificadores de recursos.

A diferencia de los recursos de imagen, donde se encuentra solo un recurso de imagen en un archivo de recursos de imagen, un archivo de recursos de cadena contiene *varios* recursos de cadena. Un archivo de recursos de cadena es un archivo de recursos (.resw), que es una clase de archivo de recursos que crearás normalmente en una carpeta \Strings en el proyecto. Para información general sobre cómo usar calificadores en los nombres de los archivos de recursos (.resw), consulta [Adaptar los recursos de idioma, escala y otros calificadores](tailor-resources-lang-scale-contrast.md).

## <a name="store-strings-in-a-resources-file"></a>Store cadenas en un archivo de recursos

1. Establece el idioma predeterminado de la aplicación.
    1. Con la solución abierta en Visual Studio, abre `Package.appxmanifest`.
    2. En la pestaña Aplicaciones, confirma que el idioma predeterminado esté establecido correctamente (por ejemplo, "en" o "en-US"). Los pasos restantes supondrán que has configurado el idioma predeterminado en "en-US".
    <br>**Tenga en cuenta** como mínimo, deberá proporcionar recursos de cadena localizados para este idioma predeterminado. Estos son los recursos que se cargarán si no puede encontrarse ninguna coincidencia mejor para el idioma preferido del usuario o la configuración de idioma de pantalla.
2. Crear un archivo de recursos (.resw) para el idioma predeterminado.
    1. En el nodo del proyecto, crea una nueva carpeta y asígnale el nombre "Strings".
    2. En `Strings`, crea una nueva subcarpeta y asígnale el nombre "en-US".
    3. En `en-US`, crea un nuevo archivo de recursos (.resw) y confirma que se llame "Resources.resw".
    <br>**Tenga en cuenta** si tiene archivos de recursos .NET (.resx) que desea que el puerto, consulte [trasladar XAML y la interfaz de usuario](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization).
3. Abre `Resources.resw` y agregar estos recursos de cadena.

    `Strings/en-US/Resources.resw`

    ![agregar recurso, inglés](images/addresource-en-us.png)

    En este ejemplo, "Greeting" es un identificador de recursos de cadena al que se puede hacer referencia desde el marcado, como veremos. Para el identificador "Greeting", se proporciona una cadena de una propiedad Texto y se proporciona una cadena para una propiedad Ancho. "Greeting.Text" es un ejemplo de un identificador de propiedad, ya que corresponde a una propiedad de un elemento de interfaz de usuario. También podrías, por ejemplo, agregar "Greeting.Foreground" en la columna Nombre y establecer su valor en "Rojo". El identificador "Farewell" es un identificador de recursos de cadena simple; no tiene ninguna subpropiedad y se puede cargar desde el código imperativo, como veremos. La columna Comentario es un buen lugar para proporcionar instrucciones especiales a los traductores.

    En este ejemplo, dado que tenemos una entrada de identificador de recursos de cadena simple denominada "Farewell", no podemos tener *también* identificadores de propiedad basados en ese mismo identificador. Por lo tanto, agregar "Farewell.Text" provocaría un error de entrada duplicada al crear `Resources.resw`.

    Los identificadores de recursos distinguen entre mayúscula y minúscula y deben ser exclusivos para cada archivo de recursos. Asegúrate de usar identificadores de recursos significativos para proporcionar contexto adicional para los traductores. Y no cambies los identificadores de recursos después de que los recursos de cadena se envíen para su traducción. Los equipos de localización usan el identificador de recursos para realizar un seguimiento de las adiciones, eliminaciones y actualizaciones en los recursos. Los cambios realizados en los identificadores de recursos (también conocidos como "variaciones en los identificadores de recursos") requieren que las cadenas vuelvan a traducirse, ya que se indicará que se eliminaron y que se agregaron otras diferentes.

## <a name="refer-to-a-string-resource-identifier-from-xaml"></a>Hacer referencia a un identificador de recurso de cadena desde XAML

Puedes usar una un [directiva x: Uid](../xaml-platform/x-uid-directive.md) para asociar un control u otro elemento del marcado con un identificador de recursos de cadena.

```xaml
<TextBlock x:Uid="Greeting"/>
```

En el momento de ejecución, se carga `\Strings\en-US\Resources.resw` (dado que desde ese momento es el único archivo de recursos del proyecto). La directiva **x: Uid** de **TextBlock** hace que tenga lugar una búsqueda, para encontrar los identificadores de las propiedades de dentro de `Resources.resw`, que contiene el identificador de recursos de cadena "Greeting". Se encuentran los identificadores de propiedades "Greeting.Text" y "Greeting.Width" y sus valores se aplican al **TextBlock**, invalidando los valores que se hubieran establecido localmente en el marcado. El valor de "Greeting.Foreground" se aplicaría, también, si se ha agregado. Pero solo los identificadores de las propiedades se usan para establecer las propiedades de los elementos de marcado XAML, por lo que establecer **x: Uid** en "Farewell" en este TextBlock no tendría ningún efecto. `Resources.resw` *Does* contienen el identificador de recurso de cadena "Despedida", pero no contiene ningún identificador de propiedad para él.

Al asignar un identificador de recursos de cadena a un elemento XAML, asegúrate de que *todos los* los identificadores de propiedades de dicho identificador sean apropiados para el elemento XAML. Por ejemplo, si estableces `x:Uid="Greeting"` en un **TextBlock**, a continuación, "Greeting.Text" resolverá porque el tipo **TextBlock** tiene una propiedad de texto. Sin embargo, si estableces `x:Uid="Greeting"` en un **Botón**, en ese caso "Greeting.Text" producirá un error de tiempo de ejecución porque el tipo **Botón** no tiene una propiedad de texto. Una solución para este caso es crear un identificador de propiedad denominado "ButtonGreeting.Content" y establecer `x:Uid="ButtonGreeting"` en el **Botón**.

En lugar de establecer **Ancho** desde un archivo de recursos, probablemente preferirás dejar que los controles ajusten dinámicamente el tamaño al contenido.

**Tenga en cuenta** para [propiedades adjuntas](../xaml-platform/attached-properties-overview.md), necesita una sintaxis especial en la columna de nombre de un archivo .resw. Por ejemplo, para establecer un valor para la propiedad adjunta [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) del identificador "Greeting", esto es lo que debe especificar en la columna Nombre.

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>Referirse a un identificador de recurso de cadena desde código

Puedes cargar explícitamente un recurso de cadena en función de un único identificador de recursos de cadena.

> [!NOTE]
> Si tienes una llamada a cualquier método **GetForCurrentView** que *podría* ejecutarse en un subproceso de trabajo o en segundo plano, protege esa llamada con una prueba `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)`. La llamada a **GetForCurrentView** a partir de los resultados de subproceso de trabajo o fondo genera la excepción de que es posible que no se cree la excepción " *&lt;nombreDeTipo&gt; en subprocesos que no tienen una clase CoreWindow*".

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView() };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"Farewell"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView();
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("Farewell");
```

Puedes usar este mismo código desde dentro de una biblioteca de clases (Windows Universal) o un proyecto [Biblioteca de Windows Runtime (Windows Universal)](../winrt-components/index.md). En el momento de ejecución, se cargan los recursos de la aplicación que aloja la biblioteca. Te recomendamos que las bibliotecas carguen recursos de la aplicación que alojan, dado que es probable que la aplicación tenga un mayor grado de localización. Cuando una biblioteca tiene que proporcionar recursos, debería proporcionar a su aplicación alojada la opción de sustituir esos recursos como si fueran una entrada.

Si un nombre de recurso está segmentado (contiene "." caracteres), a continuación, reemplazar los puntos con barra diagonal ("/") caracteres en el nombre del recurso. Los identificadores de propiedad, por ejemplo, contener puntos; por lo que deberá hacer esta sustitución para cargar uno de ellos desde el código.

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

Si tiene alguna duda, puede usar [MakePri.exe](makepri-exe-command-options.md) para volcar el archivo PRI de la aplicación. Cada recurso `uri` se muestra en el archivo de volcados.

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>Hacer referencia a un identificador de recursos de cadena desde el manifiesto del paquete de la aplicación

1. Abra el archivo de manifiesto de origen del paquete de aplicación (el `Package.appxmanifest` archivo), en lo que de manera predeterminada, la aplicación `Display name` se expresa como un literal de cadena.

   ![agregar recurso, inglés](images/display-name-before.png)

2. Para realizar una versión de esta cadena localizable, abre `Resources.resw` y agrega un nuevo recurso de cadena con el nombre "AppDisplayName" y el valor "Adventure Works Cycles".

3. Reemplaza el literal de cadena de nombre para mostrar con una referencia al identificador de recurso de cadena que acabas de crear ("AppDisplayName"). Debes usar el esquema `ms-resource` de URI (Identificador uniforme de recursos) para hacer esto.

   ![agregar recurso, inglés](images/display-name-after.png)

4. Repite este proceso para cada cadena del manifiesto que desees localizar. Por ejemplo, el nombre abreviado de la aplicación (que puedes configurar para que aparezca en la ventana de la aplicación en Inicio). Para obtener una lista de todos los elementos del manifiesto del paquete de la aplicación que se pueden localizar, consulta [Elementos de manifiesto localizables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="localize-the-string-resources"></a>Localizar los recursos de cadena

1. Realiza una copia de tu archivo de recursos (.resw) para otro idioma.
    1. En "Strings", crea una nueva subcarpeta y asígnale el nombre "de-DE", de alemán (Alemania).
   <br>**Tenga en cuenta** para el nombre de carpeta, puede usar cualquiera [etiqueta de idioma BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302). Consulta [Adaptar los recursos al idioma, la escala y otros calificadores](tailor-resources-lang-scale-contrast.md) para obtener detalles sobre el calificador de idioma y ver una lista de etiquetas de idioma comunes.
   2. Haz una copia de `Strings/en-US/Resources.resw` en la carpeta `Strings/de-DE`.
2. Traduce las cadenas.
    1. Abre `Strings/de-DE/Resources.resw` y traduce los valores de la columna Valor. No es necesario que traduzcas los comentarios.

    `Strings/de-DE/Resources.resw`

    ![agrega recurso, alemán](images/addresource-de-de.png)

Si lo deseas, puedes repetir los pasos 1 y 2 para un idioma adicional.

`Strings/fr-FR/Resources.resw`

![add resource, francés](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>Probar la aplicación

Prueba la aplicación con el idioma para mostrar predeterminado. Entones, puedes cambiar el idioma de pantalla en **Configuración** > **Hora e idioma** > **Región e idioma** > **Idiomas** y vuelve a probar la aplicación. Mira las cadenas de la interfaz de usuario y también en el shell (por ejemplo, la barra de título, que es el nombre para mostrar, y el nombre abreviado de la ventanas).

**Nota** Si se encuentra un nombre de carpeta que coincida con la configuración de idioma de pantalla, en ese caso se carga el archivo de recursos dentro de esa carpeta. De lo contrario, se utiliza la reserva, terminando con los recursos del idioma predeterminado de la aplicación.

## <a name="factoring-strings-into-multiple-resources-files"></a>Factorización de cadenas en varios archivos de recursos

Puedes mantener todas las cadenas en un único archivo de recursos (resw) o puedes factorizarlas en varios archivos de recursos. Por ejemplo, es posible que quieras mantener los mensajes de error en un archivo de recursos, las cadenas de manifiesto del paquete de aplicación en otra y las cadenas de interfaz de usuario en un tercero. Este es más o menos el aspecto que tendría tu estructura de carpetas en ese caso.

![agregar recurso, inglés](images/manifest-resources.png)

Para definir el ámbito de una referencia de identificador de recursos de cadena a un archivo en particular, solo tienes que agregar `/<resources-file-name>/` antes el identificador. El siguiente ejemplo de marcado presupone que `ErrorMessages.resw` contiene un recurso cuyo nombre es "PasswordTooWeak.Text" y cuyo valor describe el error.

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

Deberá agregar `/<resources-file-name>/` delante del identificador de recurso de cadena para los archivos de recursos *distinto* `Resources.resw`. Eso es porque "Resources.resw" es el nombre de archivo predeterminado, por lo que es lo que se asume que si se omite un nombre de archivo (como hicimos en los ejemplos anteriores de este tema).

El siguiente ejemplo de código presupone que `ErrorMessages.resw` contiene un recurso cuyo nombre es "MismatchedPasswords" y cuyo valor describe el error.

> [!NOTE]
> Si tienes una llamada a cualquier método **GetForCurrentView** que *podría* ejecutarse en un subproceso de trabajo o en segundo plano, protege esa llamada con una prueba `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)`. La llamada a **GetForCurrentView** a partir de los resultados de subproceso de trabajo o fondo genera la excepción de que es posible que no se cree la excepción " *&lt;nombreDeTipo&gt; en subprocesos que no tienen una clase CoreWindow*".

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ErrorMessages");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("MismatchedPasswords");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView(L"ErrorMessages") };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"MismatchedPasswords"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView("ErrorMessages");
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("MismatchedPasswords");
```

Si fueras a mover el recurso "AppDisplayName" fuera de `Resources.resw` y adentro de `ManifestResources.resw`, en ese caso, en el manifiesto del paquete de aplicación cambiarías `ms-resource:AppDisplayName` a `ms-resource:/ManifestResources/AppDisplayName`.

Si un nombre de archivo de recursos está segmentado (contiene "." caracteres), a continuación, dejar los puntos en el nombre cuando se haga referencia a él. **No** reemplazar puntos con barra diagonal ("/") de caracteres, como lo haría con un nombre de recurso.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

Si tiene alguna duda, puede usar [MakePri.exe](makepri-exe-command-options.md) para volcar el archivo PRI de la aplicación. Cada recurso `uri` se muestra en el archivo de volcados.

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>Cargar una cadena para un idioma u otro contexto específicos

El valor predeterminado [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) (obtenido de [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contiene un valor de calificador para cada nombre de calificador, que representa el contexto en tiempo de ejecución predeterminado (en otras palabras, la configuración de la máquina y usuario actuales). Los archivos de recursos (.resw) se comparan, en función de los calificadores de sus nombres, con los valores de calificador de ese contexto de tiempo de ejecución.

Pero es posible que haya veces en las que quieras que tu aplicación invalide la configuración del sistema y ser explícito sobre el idioma, escala u otro valor de calificador que utilices cuando busques un archivo de recursos para cargar. Por ejemplo, puedes querer que los usuarios puedan seleccionar un idioma alternativo para sugerencias sobre herramientas o mensajes de error.

Puedes hacerlo creando un nuevo **ResourceContext** (en lugar de usar el predeterminado), reemplazando sus valores y, a continuación, usando ese objeto de contexto en tus búsquedas de cadena.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

El uso de **QualifierValues** como en el ejemplo de código anterior funciona para cualquier calificador. En el caso especial del idioma, puedes hacer esto en su lugar.

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

Para lograr el mismo efecto a un nivel global, *puedes* reemplazar los valores de calificador en el **ResourceContext** predeterminado. Pero, en su lugar, se recomienda llamar a [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Únicamente estableces valores una vez con una llamada a **SetGlobalQualifierValue** y, a continuación, esos valores están en vigor en el **ResourceContext** predeterminado cada vez que los usas para búsquedas.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

Algunos de los calificadores tienen un proveedor de datos del sistema. Así, en lugar de llamar a **SetGlobalQualifierValue**, en su lugar se puede ajustar el proveedor a través de su propia API. Por ejemplo, este código muestra cómo establecer [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride).

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>Actualización de cadenas en respuesta a eventos de cambio de valor de calificador

La aplicación en ejecución puede responder a cambios en la configuración del sistema que afecten a los valores de calificador en el **ResourceContext** predeterminado. Cualquiera de estas opciones de configuración del sistema invoca al evento [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) en [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

En respuesta a este evento, puedes volver a cargar las cadenas desde el **ResourceContext** predeterminado .

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
    var dispatcher = this.myXAMLTextBlockElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIText();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIText());
    }
}

private void RefreshUIText()
{
    var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
    this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
}
```

## <a name="load-strings-from-a-class-library-or-a-windows-runtime-library"></a>Cargar cadenas desde una biblioteca de clases o una biblioteca en tiempo de ejecución de Windows

Los recursos de cadena de una biblioteca de clases (Windows Universal) referenciada o [Biblioteca de Windows Runtime (Windows Universal)](../winrt-components/index.md) se suelen agregar en una subcarpeta del paquete en las que están incluidos durante el proceso de compilación. El identificador de recursos de una cadena de este tipo normalmente toma la forma *LibraryName/ResourcesFileName/ResourceIdentifier*.

Una biblioteca puede obtener un ResourceLoader para sus propios recursos. Por ejemplo, el código siguiente muestra cómo una biblioteca o una aplicación que hace referencia a él puede obtener ResourceLoader para recursos de cadena de la biblioteca.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

Para una biblioteca en tiempo de ejecución de Windows (Universal Windows), si se segmenta el espacio de nombres predeterminado (contiene "." caracteres), a continuación, utilice puntos en el nombre de la asignación de recursos.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

No es necesario hacerlo para una biblioteca de clases (Windows Universal). Si tiene alguna duda, puede especificar [opciones de línea de comandos MakePri.exe](makepri-exe-command-options.md) para volcar el componente o el archivo PRI de la biblioteca. Cada recurso `uri` se muestra en el archivo de volcados.

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>Cargar cadenas desde otros paquetes

Los recursos de un paquete de aplicación se administra y se tiene acceso a través del paquete propietario de nivel superior [**ResourceMap** ](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) que es accesible desde la actual [**ResourceManager** ](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live). Dentro de cada paquete, varios componentes pueden tener sus propios subárboles ResourceMap, que puede tener acceso a través de [ **ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live).

Un paquete de marcos puede tener acceso a sus propios recursos con un URI de identificador de recursos absoluto. Consulta también [esquemas URI](uri-schemes.md).

## <a name="loading-strings-in-non-packaged-applications"></a>Cargar las cadenas en aplicaciones que no sean empaquetados

A partir de Windows versión 1903 (es posible que la actualización de 2019), no se empaquetan las aplicaciones también pueden aprovechar el sistema de administración de recursos.

Basta con crear las controles o bibliotecas de usuario UWP y [almacenar todas las cadenas en un archivo de recursos](#store-strings-in-a-resources-file). A continuación, puede [hacer referencia a un identificador de recurso de cadena desde XAML](#refer-to-a-string-resource-identifier-from-xaml), [hacer referencia a un identificador de recurso de cadena de código](#refer-to-a-string-resource-identifier-from-code), o [cargar cadenas desde una biblioteca de clases o una biblioteca en tiempo de ejecución de Windows](#load-strings-from-a-class-library-or-a-windows-runtime-library).

Para usar recursos en aplicaciones no embalados, debe hacer algunas cosas:

1. Use [GetForViewIndependentUse](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.resourceloader.getforviewindependentuse) en lugar de [GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) al resolver los recursos desde el código porque no hay ningún *vista actual* en escenarios no empaquetados. La siguiente excepción se produce si se llama a [GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) en escenarios no empaquetado: *Contextos de recurso no se pueden crear en subprocesos que no tienen una CoreWindow.*
1. Use [MakePri.exe](https://docs.microsoft.com/windows/uwp/app-resources/compile-resources-manually-with-makepri) para generar manualmente el archivo resources.pri de la aplicación.
    - Ejecute `makepri new /pr <PROJECTROOT> /cf <PRICONFIG> /dq <DEFAULTLANGUAGEQUALIFIER> /of resources.pri`:
    - El &lt;PRICONFIG&gt; debe omitir el "&lt;empaquetado&gt;" sección para que todos los recursos están agrupados en un archivo resources.pri único. Si usa el valor predeterminado [archivo de configuración de MakePri.exe](https://docs.microsoft.com/windows/uwp/app-resources/makepri-exe-configuration) creado por [createconfig](https://docs.microsoft.com/windows/uwp/app-resources/makepri-exe-command-options#createconfig-command), deberá eliminar el "&lt;empaquetado&gt;" sección manualmente después de crearlo.
    - El &lt;PRICONFIG&gt; debe contener todos los indizadores relevantes necesarios para combinar todos los recursos en el proyecto en un archivo resources.pri único. El valor predeterminado [archivo de configuración de MakePri.exe](https://docs.microsoft.com/windows/uwp/app-resources/makepri-exe-configuration) creado por [createconfig](https://docs.microsoft.com/windows/uwp/app-resources/makepri-exe-command-options#createconfig-command) incluye todos los indizadores.
    - Si no usa la configuración predeterminada, asegúrese de que está habilitado el indizador PRI (Revise la configuración predeterminada para saber cómo hacerlo) para combinar adoptados de referencias de proyecto para UWP, referencias de NuGet etc., que se encuentran en la raíz del proyecto.
        > [!NOTE]
        > Si se omite `/IndexName`, y el proyecto no tiene un manifiesto de aplicación, se establece automáticamente el espacio de nombres IndexName/raíz del archivo PRI en *aplicación*, que comprende el tiempo de ejecución para las aplicaciones empaquetadas no (Esto quita el anterior dependencia fuerte en Id. de paquete). Al especificar identificadores URI, ms-resource: / / / referencias que omiten el espacio de nombres raíz inferir *aplicación* como el espacio de nombres raíz para las aplicaciones empaquetadas no (o puede especificar *aplicación* explícitamente como en ms-resource://Application/).
1. Copie el archivo PRI en el directorio de salida de compilación del .exe
1. Ejecute el archivo .exe 
    > [!NOTE]
    > El sistema de administración de recursos utiliza el idioma del sistema en lugar de la lista de idioma preferido del usuario al resolver los recursos en función de lenguaje en las aplicaciones empaquetadas no. Solo se usa la lista de idioma preferido del usuario para aplicaciones UWP.

> [!Important]
> Debe recompilar manualmente archivos PRI cada vez que se modifican los recursos. Se recomienda usar un script posterior a la compilación que controla la [MakePri.exe](https://docs.microsoft.com/windows/uwp/app-resources/compile-resources-manually-with-makepri) comando y copia la salida resources.pri en el directorio .exe.

## <a name="important-apis"></a>API importantes
* [ApplicationModel.Resources.ResourceLoader](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.ResourceLoader)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Temas relacionados
* [Interfaz de usuario y la portabilidad de XAML](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [x: Uid (directiva)](../xaml-platform/x-uid-directive.md)
* [Propiedades adjuntas](../xaml-platform/attached-properties-overview.md)
* [Elementos localizables de manifiesto](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Etiqueta de idioma BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302)
* [Adaptar los recursos de idioma, la escala y otros calificadores](tailor-resources-lang-scale-contrast.md)
* [Cómo cargar recursos de cadena](https://docs.microsoft.com/previous-versions/windows/apps/hh965323(v=win.10))
