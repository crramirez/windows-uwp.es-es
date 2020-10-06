---
Description: Si quieres que tu aplicación admita diferentes idiomas de pantalla y tenga literales de cadena en el código, o bien marcado XAML o manifiesto de paquete de la aplicación, mueve esas cadenas a un archivo de recursos (.resw). A continuación, puedes hacer una copia traducida de ese archivo de recursos para cada idioma que admita la aplicación.
title: Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.date: 11/01/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 411ecb63189084ba83f9971ded2bbe02d899aabd
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762866"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación

Para más información sobre la propuesta de valor de localizar la aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md).

Si quieres que tu aplicación admita diferentes idiomas de pantalla y tenga literales de cadena en el código, o bien marcado XAML o manifiesto de paquete de la aplicación, mueve esas cadenas a un archivo de recursos (.resw). A continuación, puedes hacer una copia traducida de ese archivo de recursos para cada idioma que admita la aplicación.

Los literales de cadena codificados de forma rígida pueden aparecer en código imperativo o en marcado XAML, por ejemplo, como la propiedad **Text** de un **TextBlock**. También pueden aparecer en el archivo de origen del manifiesto del paquete de la aplicación (el `Package.appxmanifest` archivo), por ejemplo, como el valor de nombre para mostrar en la pestaña aplicación del diseñador de manifiestos de Visual Studio. Mueva estas cadenas a un archivo de recursos (. resw) y reemplace los literales de cadena codificados de la aplicación y en el manifiesto por referencias a los identificadores de recursos.

A diferencia de los recursos de imagen, donde solo un recurso de imagen está contenido en un archivo de recursos de imagen, *varios* recursos de cadena se encuentran en un archivo de recursos de cadena. Un archivo de recursos de cadena es un archivo de recursos (. resw) y normalmente se crea este tipo de archivo de recursos en una carpeta \Strings del proyecto. Para obtener información sobre cómo usar calificadores en los nombres de los archivos de recursos (. resw), consulte [adaptar los recursos para el idioma, la escala y otros calificadores](tailor-resources-lang-scale-contrast.md).

## <a name="store-strings-in-a-resources-file"></a>Almacenar cadenas en un archivo de recursos

1. Establezca el idioma predeterminado de la aplicación.
    1. Con la solución abierta en Visual Studio, Abra `Package.appxmanifest` .
    2. En la pestaña aplicación, confirme que el idioma predeterminado está establecido correctamente (por ejemplo, "en" o "en-US"). En los pasos restantes se supone que ha establecido el idioma predeterminado en "en-US".
    <br>**Nota:**   Como mínimo, debe proporcionar recursos de cadena localizados para este idioma predeterminado. Estos son los recursos que se cargarán si no se encuentra ninguna coincidencia mejor para la configuración del idioma preferido del usuario o del idioma de la pantalla.
2. Cree un archivo de recursos (. resw) para el idioma predeterminado.
    1. En el nodo del proyecto, cree una nueva carpeta y asígnele el nombre "strings".
    2. En `Strings` , cree una nueva subcarpeta y asígnele el nombre "en-US".
    3. En `en-US` , cree un nuevo archivo de recursos (. resw) y confirme que se llama "Resources. resw".
    <br>**Nota:**   Si tiene archivos de recursos de .NET (. resx) que desea migrar, vea [portar XAML y UI](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization).
3. Abra `Resources.resw` y agregue estos recursos de cadena.

    `Strings/en-US/Resources.resw`

    ![Captura de pantalla de la tabla Agregar recurso de las cadenas > E N U S > archivo resources. resw.](images/addresource-en-us.png)

    En este ejemplo, "GREETING" es un identificador de recursos de cadena al que se puede hacer referencia desde el marcado, como se muestra. En el caso del identificador "GREETING", se proporciona una cadena para una propiedad Text y se proporciona una cadena para una propiedad width. "GREETING. Text" es un ejemplo de un identificador de propiedad porque corresponde a una propiedad de un elemento de la interfaz de usuario. También puede, por ejemplo, agregar "GREETING. Foreground" en la columna Name y establecer su valor en "red". El identificador "despedida" es un identificador de recurso de cadena simple; no tiene subpropiedades y se puede cargar desde código imperativo, como se muestra. La columna comment es un buen lugar para proporcionar instrucciones especiales a los traductores.

    En este ejemplo, dado que tenemos una entrada de identificador de recurso de cadena simple denominada "despedida", *no se pueden tener* identificadores de propiedad basados en el mismo identificador. Por lo tanto, si se agrega "despedida. Text", se produciría un error de entrada duplicada al compilar `Resources.resw` .

    Los identificadores de recursos no distinguen mayúsculas de minúsculas y deben ser únicos en cada archivo de recursos. Asegúrese de usar identificadores de recursos significativos para proporcionar contexto adicional a los traductores. Y no cambian los identificadores de recursos después de enviar los recursos de cadena para su traducción. Los equipos de localización usan el identificador de recursos para realizar un seguimiento de las adiciones, eliminaciones y actualizaciones en los recursos. Los cambios en los identificadores de recursos &mdash; que también se conoce como "cambio de identificadores de recursos" &mdash; requieren que se retraduzcan las cadenas, ya que aparecerán como si se eliminaran las cadenas y otras agregadas.

## <a name="refer-to-a-string-resource-identifier-from-xaml"></a>Referencia a un identificador de recursos de cadena desde XAML

Una [Directiva x:UID](../xaml-platform/x-uid-directive.md) se usa para asociar un control u otro elemento en el marcado con un identificador de recurso de cadena.

```xaml
<TextBlock x:Uid="Greeting"/>
```

En tiempo de ejecución, `\Strings\en-US\Resources.resw` se carga (ya que ahora es el único archivo de recursos del proyecto). La directiva **x:UID** en el **TextBlock** hace que se produzca una búsqueda, para buscar los identificadores de propiedad dentro `Resources.resw` de que contienen el identificador de recurso de cadena "GREETING". Los identificadores de propiedad "GREETING. Text" y "GREETING. width" se encuentran y sus valores se aplican al **TextBlock**, invalidando los valores establecidos localmente en el marcado. También se aplicaría el valor "GREETING. Foreground", si lo agregó. Pero solo se usan identificadores de propiedad para establecer propiedades en elementos de marcado XAML, por lo que establecer **x:UID** en "despedida" en este TextBlock no tendría ningún efecto. `Resources.resw`*contiene el* identificador de recursos de cadena "despedida", pero no contiene ningún identificador de propiedad.

Al asignar un identificador de recurso de cadena a un elemento XAML, asegúrese de que *todos* los identificadores de propiedad de ese identificador sean adecuados para el elemento XAML. Por ejemplo, si establece `x:Uid="Greeting"` en un **TextBlock** , "GREETING. Text" se resolverá porque el tipo **TextBlock** tiene una propiedad Text. Pero si se establece `x:Uid="Greeting"` en un **botón** , "GREETING. Text" producirá un error en tiempo de ejecución porque el tipo de **botón** no tiene una propiedad Text. Una solución para ese caso es crear un identificador de propiedad denominado "ButtonGreeting. Content" y establecerlo `x:Uid="ButtonGreeting"` en el **botón**.

En lugar de establecer el **ancho** de un archivo de recursos, es probable que desee permitir que los controles se ajusten dinámicamente al contenido.

**Nota:**   En el caso de [las propiedades adjuntas](../xaml-platform/attached-properties-overview.md), necesita una sintaxis especial en la columna Nombre de un archivo. resw. Por ejemplo, para establecer un valor para la propiedad adjunta [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) para el identificador de "saludo", esto es lo que escribiría en la columna nombre.

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>Referencia a un identificador de recursos de cadena desde el código

Puede cargar explícitamente un recurso de cadena basándose en un identificador de recurso de cadena simple.

> [!NOTE]
> Si tiene una llamada a cualquier método **GetForCurrentView** que se *pueda* ejecutar en un subproceso de trabajo o de fondo, proteja esa llamada con una `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` prueba. La llamada a **GetForCurrentView** desde un subproceso de trabajo o en segundo plano produce la excepción "* &lt; &gt; no se puede crear TypeName en subprocesos que no tienen un CoreWindow".*

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

Puede usar este mismo código desde una biblioteca de clases (Windows universal) o un proyecto de [biblioteca de Windows Runtime (Windows universal)](../winrt-components/index.md) . En tiempo de ejecución, se cargan los recursos de la aplicación que hospeda la biblioteca. Se recomienda que una biblioteca cargue los recursos de la aplicación que lo hospeda, ya que es probable que la aplicación tenga un mayor grado de localización. Si una biblioteca necesita proporcionar recursos, debe proporcionar a su aplicación de hospedaje la opción de reemplazar esos recursos como una entrada.

Si se segmenta un nombre de recurso (contiene caracteres "."), reemplace los puntos por caracteres de barra diagonal ("/") en el nombre del recurso. Los identificadores de propiedad, por ejemplo, contienen puntos; por lo tanto, debe hacer esto Substition para cargar uno de ellos desde el código.

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

En caso de duda, puede usar [MakePri.exe](makepri-exe-command-options.md) para volcar el archivo PRI de la aplicación. `uri`En el archivo volcado se muestra cada uno de los recursos.

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>Referencia a un identificador de recurso de cadena desde el manifiesto del paquete de la aplicación

1. Abra el archivo de origen del manifiesto del paquete de la aplicación (el `Package.appxmanifest` archivo), en el que, de forma predeterminada, la aplicación `Display name` se expresa como un literal de cadena.

   ![Captura de pantalla del archivo package. appxmanifest que muestra la pestaña aplicación con el nombre para mostrar establecido en Adventure Works Cycles.](images/display-name-before.png)

2. Para crear una versión localizable de esta cadena, Abra `Resources.resw` y agregue un nuevo recurso de cadena con el nombre "AppDisplayName" y el valor "Adventure Works Cycles".

3. Reemplace el literal de cadena del nombre para mostrar por una referencia al identificador de recurso de cadena que acaba de crear ("AppDisplayName"). Para ello, se usa el `ms-resource` esquema URI (identificador uniforme de recursos).

   ![Captura de pantalla del archivo package. appxmanifest que muestra la pestaña aplicación con el nombre para mostrar establecido en el nombre para mostrar de la aplicación de recursos M S.](images/display-name-after.png)

4. Repita este proceso para cada cadena del manifiesto que desee localizar. Por ejemplo, el nombre corto de la aplicación (que puede configurar para que aparezca en el icono de la aplicación en el inicio). Para obtener una lista de todos los elementos del manifiesto de paquete de aplicación que puede localizar, consulte [elementos de manifiesto localizables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="localize-the-string-resources"></a>Localizar los recursos de cadena

1. Haga una copia del archivo de recursos (. resw) para otro idioma.
    1. En "cadenas", cree una nueva subcarpeta y asígnele el nombre "de-DE" para Deutsch (Deutschland).
   <br>**Nota:**   En el nombre de la carpeta, puede usar cualquier [etiqueta de idioma BCP-47](https://tools.ietf.org/html/bcp47). Consulte [adaptar los recursos para el idioma, la escala y otros calificadores](tailor-resources-lang-scale-contrast.md) para obtener más información sobre el calificador de idioma y una lista de etiquetas de lenguaje común.
   2. Haga una copia de `Strings/en-US/Resources.resw` en la `Strings/de-DE` carpeta.
2. Traduzca las cadenas.
    1. Abra `Strings/de-DE/Resources.resw` y traduzca los valores de la columna valor. No es necesario traducir los comentarios.

    `Strings/de-DE/Resources.resw`

    ![agrega recurso, alemán](images/addresource-de-de.png)

Si lo desea, puede repetir los pasos 1 y 2 para un idioma adicional.

`Strings/fr-FR/Resources.resw`

![agrega recurso, francés](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>Prueba de la aplicación

Prueba la aplicación con el idioma para mostrar predeterminado. Después, puede cambiar el idioma para mostrar en **configuración**  >  **hora &** región de idioma & lenguajes de  >  **idioma**  >  **Languages** y volver a probar la aplicación. Busque cadenas en la interfaz de usuario y también en el Shell (por ejemplo, la barra de título, &mdash; que es el nombre &mdash; para mostrar y el nombre corto de los mosaicos).

**Nota:** Si se encuentra un nombre de carpeta que coincida con la configuración de idioma para mostrar, se carga el archivo de recursos dentro de esa carpeta. De lo contrario, se realiza la reserva, finalizando con los recursos del idioma predeterminado de la aplicación.

## <a name="factoring-strings-into-multiple-resources-files"></a>Factorizar cadenas en varios archivos de recursos

Puede mantener todas las cadenas en un archivo de recursos único (resw) o puede factorizarlas en varios archivos de recursos. Por ejemplo, puede que desee conservar los mensajes de error en un archivo de recursos, las cadenas del manifiesto del paquete de la aplicación en otro y las cadenas de la interfaz de usuario en un tercer lugar. Este es el aspecto de la estructura de carpetas en ese caso.

![Captura de pantalla del panel de soluciones que muestra la carpeta Adventure Works Cycles > Strings con los archivos y las carpetas de la configuración regional en alemán, en inglés y en francés.](images/manifest-resources.png)

Para limitar el ámbito de una referencia de identificador de recurso de cadena a un archivo determinado, solo tiene que agregar `/<resources-file-name>/` antes del identificador. En el ejemplo de marcado siguiente se da por supuesto que `ErrorMessages.resw` contiene un recurso cuyo nombre es "PasswordTooWeak. Text" y cuyo valor describe el error.

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

Solo tiene que agregar `/<resources-file-name>/` antes del identificador de recursos de cadena para los archivos de recursos *que no sean* `Resources.resw` . Esto se debe a que "Resources. resw" es el nombre de archivo predeterminado, por lo que se supone que se da por hecho que se omite un nombre de archivo (como hicimos en los ejemplos anteriores de este tema).

En el ejemplo de código siguiente se da por supuesto que `ErrorMessages.resw` contiene un recurso cuyo nombre es "MismatchedPasswords" y cuyo valor describe el error.

> [!NOTE]
> Si tiene una llamada a cualquier método **GetForCurrentView** que se *pueda* ejecutar en un subproceso de trabajo o de fondo, proteja esa llamada con una `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` prueba. La llamada a **GetForCurrentView** desde un subproceso de trabajo o en segundo plano produce la excepción "* &lt; &gt; no se puede crear TypeName en subprocesos que no tienen un CoreWindow".*

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

Si tuviera que trasladar el recurso "AppDisplayName" de `Resources.resw` y a `ManifestResources.resw` , en el manifiesto del paquete de la aplicación cambiaría `ms-resource:AppDisplayName` a `ms-resource:/ManifestResources/AppDisplayName` .

Si se segmenta un nombre de archivo de recursos (contiene caracteres "."), deje los puntos en el nombre al hacer referencia a él. **No** Reemplace los puntos por caracteres de barra diagonal ("/"), como haría con un nombre de recurso.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

En caso de duda, puede usar [MakePri.exe](makepri-exe-command-options.md) para volcar el archivo PRI de la aplicación. `uri`En el archivo volcado se muestra cada uno de los recursos.

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>Carga de una cadena para un idioma específico u otro contexto

La [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) predeterminada (obtenida de [**ResourceContext. GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) contiene un valor de calificador para cada nombre de calificador, que representa el contexto de tiempo de ejecución predeterminado (es decir, la configuración de la máquina y el usuario actual). Los archivos de recursos (. resw) se &mdash; comparan en función de los calificadores de sus nombres &mdash; con los valores de calificador de ese contexto en tiempo de ejecución.

Pero puede haber ocasiones en las que desee que la aplicación invalide la configuración del sistema y sea explícita sobre el idioma, la escala u otro valor de calificador que se usará al buscar un archivo de recursos coincidentes para cargarlo. Por ejemplo, puede que desee que los usuarios puedan seleccionar un idioma alternativo para la información sobre herramientas o los mensajes de error.

Para ello, puede crear un nuevo **ResourceContext** (en lugar de usar el valor predeterminado), invalidar sus valores y, a continuación, usar ese objeto de contexto en las búsquedas de cadenas.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

El uso de **QualifierValues** como en el ejemplo de código anterior funciona para cualquier calificador. En el caso del idioma especial, puede hacerlo como alternativa.

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

Para el mismo efecto en un nivel global, *puede* invalidar los valores de calificador en el valor predeterminado de **ResourceContext**. Pero en su lugar, se recomienda llamar a [**ResourceContext. SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_). Los valores se establecen una vez con una llamada a **SetGlobalQualifierValue** y, a continuación, esos valores están en vigor en el **ResourceContext** predeterminado cada vez que se usa para las búsquedas.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

Algunos calificadores tienen un proveedor de datos del sistema. Por lo tanto, en lugar de llamar a **SetGlobalQualifierValue** , puede ajustar el proveedor a través de su propia API. Por ejemplo, este código muestra cómo establecer [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride).

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>Actualizar cadenas en respuesta a eventos de cambio de valor de calificador

La aplicación en ejecución puede responder a los cambios en la configuración del sistema que afectan a los valores de calificador en el valor predeterminado de **ResourceContext**. Cualquiera de estas configuraciones del sistema invoca el evento [**MapChanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) en [**ResourceContext. QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

En respuesta a este evento, puede volver a cargar las cadenas desde el **ResourceContext**predeterminado.

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

## <a name="load-strings-from-a-class-library-or-a-windows-runtime-library"></a>Carga de cadenas de una biblioteca de clases o de una biblioteca de Windows Runtime

Los recursos de cadena de una biblioteca de clases a la que se hace referencia (Windows universal) o de la [biblioteca de Windows Runtime (Windows universal)](../winrt-components/index.md) se suelen agregar en una subcarpeta del paquete en el que se incluyen durante el proceso de compilación. El identificador de recurso de una cadena de este tipo suele tomar la forma *nombrebiblioteca/Resources/ResourceIdentifier*.

Una biblioteca puede obtener un ResourceLoader para sus propios recursos. Por ejemplo, en el código siguiente se muestra cómo una biblioteca o una aplicación que hace referencia a ella puede obtener un ResourceLoader para los recursos de cadena de la biblioteca.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

En el caso de una biblioteca de Windows Runtime (Windows universal), si el espacio de nombres predeterminado está segmentado (contiene caracteres "."), use puntos en el nombre del mapa de recursos.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

No es necesario hacerlo para una biblioteca de clases (Windows universal). En caso de duda, puede especificar [MakePri.exe opciones](makepri-exe-command-options.md) de la línea de comandos para volcar el archivo PRI de la biblioteca o componente. `uri`En el archivo volcado se muestra cada uno de los recursos.

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>Cargar cadenas desde otros paquetes

Los recursos de un paquete de la aplicación se administran y se obtiene acceso a ellos a través del [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) de nivel superior del paquete al que se puede acceder desde el [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)actual. Dentro de cada paquete, varios componentes pueden tener sus propios subárboles de ResourceMap, a los que se puede tener acceso a través de [**ResourceMap. GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live).

Un paquete de Framework puede tener acceso a sus propios recursos con un URI de identificador de recursos absoluto. Vea también [esquemas de URI](uri-schemes.md).

## <a name="loading-strings-in-non-packaged-applications"></a>Cargar cadenas en aplicaciones no empaquetadas

A partir de la versión 1903 de Windows (actualización 2019 de mayo), las aplicaciones no empaquetadas también pueden aprovechar el sistema de administración de recursos.

Solo tiene que crear sus controles y bibliotecas de usuario de UWP y [almacenar cualquier cadena en un archivo de recursos](#store-strings-in-a-resources-file). Después, puede [hacer referencia a un identificador de recurso de cadena desde XAML](#refer-to-a-string-resource-identifier-from-xaml), [hacer referencia a un identificador de recurso de cadena desde el código](#refer-to-a-string-resource-identifier-from-code)o [cargar cadenas de una biblioteca de clases o de una biblioteca de Windows Runtime](#load-strings-from-a-class-library-or-a-windows-runtime-library).

Para usar recursos en aplicaciones no empaquetadas, debe realizar algunas acciones:

1. Use [GetForViewIndependentUse](/uwp/api/windows.applicationmodel.resources.resourceloader.getforviewindependentuse) en lugar de [GetForCurrentView](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) al resolver recursos desde el código, ya que no hay ninguna *vista actual* en escenarios no empaquetados. La siguiente excepción se produce si se llama a [GetForCurrentView](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) en escenarios no empaquetados: *no se pueden crear contextos de recursos en subprocesos que no tienen un CoreWindow.*
1. Use [MakePri.exe](./compile-resources-manually-with-makepri.md) para generar manualmente el archivo resources. PRI de la aplicación.
    - Ejecute `makepri new /pr <PROJECTROOT> /cf <PRICONFIG> /of resources.pri`:
    - &lt;Archivo priconfig &gt; debe omitir la &lt; sección "empaquetado &gt; " para que todos los recursos se incluyan en un único archivo resources. PRI. Si usa el [ archivo de configuración deMakePri.exe](./makepri-exe-configuration.md) predeterminado creado por [createconfig](./makepri-exe-command-options.md#createconfig-command), debe eliminar manualmente la &lt; sección "empaquetado &gt; " una vez creada.
    - &lt;Archivo priconfig &gt; debe contener todos los indizadores relevantes necesarios para combinar todos los recursos del proyecto en un único archivo resources. PRI. El [ archivo de configuración deMakePri.exe](./makepri-exe-configuration.md) predeterminado creado por [createconfig](./makepri-exe-command-options.md#createconfig-command) incluye todos los indizadores.
    - Si no usa la configuración predeterminada, asegúrese de que el indexador PRI está habilitado (Revise la configuración predeterminada para realizar esta tarea) para combinar la consulta de la carpeta de proyecto UWP, las referencias de NuGet, etc., que se encuentran en la raíz del proyecto.
        > [!NOTE]
        > Al omitir `/IndexName` y el proyecto no tiene un manifiesto de aplicación, el espacio de nombres IndexName/root del archivo PRI se establece automáticamente en la *aplicación*, que el Runtime entiende para las aplicaciones no empaquetadas (esto quita la dependencia fuerte anterior en el identificador del paquete). Al especificar los URI de recurso, las referencias de MS-Resource:///que omiten la *aplicación* espacio de nombres raíz Infer como espacio de nombres raíz para aplicaciones no empaquetadas (o puede especificar la *aplicación* explícitamente como en MS-Resource://Application/).
1. Copie el archivo PRI en el directorio de salida de la compilación del archivo. exe.
1. Ejecutar el archivo. exe 
    > [!NOTE]
    > El sistema de administración de recursos usa el idioma para mostrar del sistema en lugar de la lista de idiomas preferidos del usuario al resolver recursos en función del idioma de las aplicaciones no empaquetadas. La lista de idiomas preferidos del usuario solo se usa para aplicaciones UWP.

> [!Important]
> Debe volver a generar manualmente los archivos PRI siempre que se modifiquen los recursos. Se recomienda usar un script posterior a la compilación que controle el comando [MakePri.exe](./compile-resources-manually-with-makepri.md) y copie la salida de Resources. PRI en el directorio. exe.

## <a name="important-apis"></a>API importantes
* [ApplicationModel.Resources.ResourceLoader](/uwp/api/Windows.ApplicationModel.Resources.ResourceLoader)
* [ResourceContext. SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Temas relacionados
* [Migración de XAML y la interfaz de usuario](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [Directiva x:Uid](../xaml-platform/x-uid-directive.md)
* [propiedades adjuntas](../xaml-platform/attached-properties-overview.md)
* [Elementos de manifiesto localizables](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Etiqueta de idioma de BCP-47](https://tools.ietf.org/html/bcp47)
* [Adapte los recursos para el idioma, la escala y otros calificadores](tailor-resources-lang-scale-contrast.md)
* [Cómo cargar recursos de cadenas](/previous-versions/windows/apps/hh965323(v=win.10))