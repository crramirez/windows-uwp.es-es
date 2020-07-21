---
Description: En este tema se definen los términos lista de idioma del perfil de usuario, lista de idiomas del manifiesto de la aplicación y lista de idiomas del tiempo de ejecución de la aplicación. Usaremos estos términos en este tema y otros temas en esta área de características, por lo que es importante saber lo que significan.
title: Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.date: 11/08/2017
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 9998436b106acce6a9223140e66d2633c2210a54
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493360"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>Comprender los idiomas del perfil del usuario y los idiomas de manifiesto de la aplicación
Un usuario de Windows puede usar la **configuración**  >  **hora &** la  >  **región de idioma & idioma** para configurar una lista ordenada de los idiomas para mostrar preferidos, o simplemente un solo idioma de visualización preferido. Un lenguaje puede tener una variante regional. Por ejemplo, puede seleccionar español como se habla en España, español, como se habla en México, español, como se habla en el Estados Unidos, entre otros.

Además, en **configuración**  >  **& región de idioma**  >  **& idioma**, pero separado del idioma, el usuario puede especificar su ubicación (conocida como región) en el mundo. Tenga en cuenta que el valor de idioma para mostrar (y variante regional) no es un determinante de la configuración regional y viceversa. Por ejemplo, un usuario puede estar viviendo actualmente en Francia, pero elegir un idioma de visualización de Windows preferido (México).

En el caso de las aplicaciones de Windows, un idioma se representa como una [etiqueta de idioma BCP-47](https://tools.ietf.org/html/bcp47). Por ejemplo, la etiqueta de idioma BCP-47 "en-US" corresponde a Inglés (Estados Unidos) en **configuración**. Las API de Windows Runtime adecuadas aceptan y devuelven representaciones de cadena de etiquetas de lenguaje BCP-47.

Vea también el [registro de subetiqueta del lenguaje IANA](https://www.iana.org/assignments/language-subtag-registry).

En las tres secciones siguientes se definen los términos "lista de lenguajes de Perfil de usuario", "lista de idiomas del manifiesto de la aplicación" y "lista de idioma de tiempo de ejecución de la aplicación". Usaremos estos términos en este tema y otros temas en esta área de características, por lo que es importante saber lo que significan.

## <a name="user-profile-language-list"></a>Lista de idioma del perfil de usuario
La lista de idiomas de Perfil de usuario es el nombre de la lista que configura el usuario en **configuración**  >  **tiempo & región de idioma**  >  **&**  >  **idiomas**. En el código, puede usar la propiedad [**GlobalizationPreferences. Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) para tener acceso a la lista de idiomas de Perfil de usuario como una lista de cadenas de solo lectura, donde cada cadena es una sola [etiqueta de lenguaje BCP-47](https://tools.ietf.org/html/bcp47) como "en-US" o "ja-JP".

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>Lista de idiomas del manifiesto de aplicación
La lista de idiomas del manifiesto de la aplicación es la lista de idiomas para los que la aplicación declara (o declarará) la compatibilidad. Esta lista crece a medida que avanza la aplicación a través del ciclo de vida de desarrollo hasta la localización.

La lista se determina en tiempo de compilación, pero tiene dos opciones para controlar exactamente cómo sucede eso. Una opción consiste en permitir que Visual Studio determine la lista de los archivos del proyecto. Para ello, primero establezca el **idioma predeterminado** de la aplicación en la pestaña **aplicación** del archivo de origen del manifiesto del paquete de la aplicación ( `Package.appxmanifest` ). A continuación, confirme que el mismo archivo contiene esta configuración (lo que hace de forma predeterminada).

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

Cada vez que Visual Studio genera el archivo de manifiesto del paquete de aplicación compilado ( `AppxManifest.xml` ), expande ese único `Resource` elemento en el archivo de código fuente en una Unión de todos los calificadores de lenguaje que encuentra en el proyecto (vea [adaptar los recursos para el idioma, la escala, el contraste alto y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)). Por ejemplo, si ha empezado a localizar y tiene una cadena, una imagen o recursos de archivo cuyos nombres de carpeta o archivo incluyen "en-US", "ja-JP" y "fr-FR", el `AppxManifest.xml` archivo compilado contendrá lo siguiente (la primera entrada de la lista es el idioma predeterminado que establezca).

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

La otra opción es reemplazar ese único elemento "x-Generate" `<Resource>` en el archivo de origen del manifiesto del paquete de la aplicación ( `Package.appxmanifest` ) con la lista de elementos expandida `<Resource>` (Asegúrese de mostrar primero el idioma predeterminado). Esta opción implica más trabajo de mantenimiento, pero puede ser una opción adecuada si usa un sistema de compilación personalizado.

Para empezar, la lista de idiomas del manifiesto de aplicación solo contendrá un idioma. Quizás eso sea en-US. Sin embargo &mdash; , a medida que se configura manualmente el manifiesto, o cuando se agregan recursos traducidos al proyecto que la lista aumentará &mdash; .

Cuando la aplicación está en el Microsoft Store, los idiomas de la lista idioma del manifiesto de aplicación son los que se muestran a los clientes. Para obtener una lista de etiquetas de lenguaje BCP-47 que se admiten específicamente en el Microsoft Store, consulte [idiomas admitidos](../../publish/supported-languages.md).

En el código, puede usar la propiedad [**ApplicationLanguages. ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) para tener acceso a la lista de idiomas del manifiesto de aplicación como una lista de cadenas de solo lectura, donde cada cadena es una sola etiqueta de lenguaje BCP-47.

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>Lista de idiomas del tiempo de ejecución de aplicaciones
La tercera lista de idiomas de interés es la intersección entre las dos listas que acabamos de describir. En tiempo de ejecución, la lista de idiomas para los que la aplicación ha declarado compatibilidad (la lista de idiomas del manifiesto de la aplicación) se compara con la lista de idiomas para los que el usuario ha declarado una preferencia (la lista de idioma del perfil de usuario). La lista del lenguaje en tiempo de ejecución de la aplicación se establece en esta intersección (si la intersección no está vacía) o solo en el idioma predeterminado de la aplicación (si la intersección está vacía).

Más concretamente, la lista de idiomas de tiempo de ejecución de la aplicación se compone de estos elementos.

1.  **(Opcional) invalidación del idioma principal**. [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) es un valor de invalidación simple para las aplicaciones que proporcionan a los usuarios su propia elección de lenguaje independiente, o aplicaciones que tienen algún motivo seguro para invalidar las opciones de idioma predeterminado. Para más información, echa un vistazo a la [muestra de recursos de la aplicación y localización](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Application%20resources%20and%20localization%20sample%20(Windows%208)).
2.  **Los idiomas del usuario que son compatibles con la aplicación**. Esta es la lista de idiomas del perfil de usuario filtrada por la lista de idiomas del manifiesto de la aplicación. Si los idiomas del usuario se filtran en función de aquellos que la aplicación admite, se mantendrá la coherencia en los kits de desarrollo de software (SDK), las bibliotecas de clases, los paquetes de marcos dependientes y la aplicación.
3.  **Si 1 y 2 están vacíos, el idioma predeterminado o el primero admitido por la aplicación**. Si la lista de idioma del perfil de usuario no contiene ningún lenguaje compatible con la aplicación, el lenguaje de tiempo de ejecución de la aplicación es el primer idioma admitido por la aplicación.

En el código, puede usar la propiedad [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) para tener acceso a la lista de idiomas de tiempo de ejecución de la aplicación en forma de una cadena que contiene una lista delimitada por signos de punto y coma de etiquetas de lenguaje BCP-47.

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

También puede tener acceso a ella como una lista de solo lectura de cadenas, cada una de las cuales contiene una sola etiqueta de lenguaje BCP-47. Para ello, puede usar la propiedad [**ResourceContext. Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) o la propiedad [**ApplicationLanguages. Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages) .

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

La lista de idiomas del tiempo de ejecución de la aplicación determina los recursos que Windows carga para la aplicación y los lenguajes que se usan para dar formato a las fechas, horas, números y otros componentes. Consulte [globalizar los formatos de fecha, hora y número](use-global-ready-formats.md).

**Nota:** Si el idioma del perfil de usuario y el idioma del manifiesto de la aplicación son variantes regionales entre sí, la variante regional del usuario se usa como lenguaje de tiempo de ejecución de la aplicación. Por ejemplo, si el usuario prefiere en-GB y la aplicación admite en-US, el idioma de tiempo de ejecución de la aplicación es en GB. Esto garantiza que las fechas, las horas y los números tengan un formato más estrecho a las expectativas del usuario (en-GB), pero los recursos localizados se siguen cargando (debido a la coincidencia de idioma) en el idioma compatible de la aplicación (en-US).

## <a name="qualify-resource-files-with-their-language"></a>Calificar archivos de recursos con su idioma
Asigne un nombre a los archivos de recursos, o sus carpetas, con calificadores de recursos de idioma. Para obtener más información acerca de los calificadores de recursos, consulte [adaptar los recursos para el idioma, la escala, el contraste alto y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md). Un archivo de recursos puede ser una imagen (u otro recurso), o puede ser un archivo contenedor de recursos, como un archivo *. resw* que contiene cadenas de texto.

**Nota:** Incluso los recursos del idioma predeterminado de la aplicación deben especificar el calificador de idioma. Por ejemplo, si el idioma predeterminado de la aplicación es el inglés (Estados Unidos), califique los recursos como `\Assets\Images\en-US\logo.png` .

- Windows realiza una coincidencia compleja, incluidas las variantes regionales como en-US y en-GB. Por tanto, incluya la subetiqueta region según corresponda. Vea [cómo el sistema de administración de recursos coincide con las etiquetas de idioma](../../app-resources/how-rms-matches-lang-tags.md).
- Especifique una subetiqueta de script de idioma en el calificador cuando no haya definido ningún valor de suprimir script para el idioma. Por ejemplo, en lugar de zh-CN o zh-TW, use ZH-hant, ZH-hant-TW o ZH-Hans (para obtener más información, consulte el [registro de subetiquetas del lenguaje IANA](https://www.iana.org/assignments/language-subtag-registry)).
- En el caso de los idiomas que tienen un único dialecto estándar, no es necesario incluir el calificador region. Por ejemplo, use ja en lugar de ja-JP.
- Algunas herramientas y otros componentes, como los traductores de máquina, pueden encontrar etiquetas de idioma específicas, como la información de dialecto regional, útil para entender los datos.

### <a name="not-all-resources-need-to-be-localized"></a>No es necesario localizar todos los recursos

La localización podría no ser necesaria para todos los recursos.

- Como mínimo, asegúrese de que todos los recursos existen en el idioma predeterminado.
- Un subconjunto de algunos recursos puede bastar para un lenguaje estrechamente relacionado (localización parcial). Por ejemplo, podría no localizar toda la interfaz de usuario de la aplicación en catalán si la aplicación tiene un conjunto completo de recursos en español. En el caso de los usuarios que hablan catalán y en español, los recursos que no están disponibles en catalán aparecen en español.
- Algunos recursos pueden requerir excepciones para lenguajes específicos, mientras que la mayoría de los demás recursos se asignan a un recurso común. En este caso, marque el recurso que se va a usar para todos los idiomas con la etiqueta de idioma sin determinar ' und '. Windows interpreta la etiqueta de idioma ' und ' como un carácter comodín (similar a ' \* ') en que coincide con el idioma de la aplicación superior después de cualquier otra coincidencia específica. Por ejemplo, si algunos recursos son diferentes para finés, pero el resto de los recursos es el mismo para todos los lenguajes, el recurso finlandés debe marcarse con la etiqueta de idioma Finlandés y el resto se debe marcar con ' und '.
- En el caso de los recursos que se basan en un script de idioma, como una fuente o un alto de texto, use la etiqueta de idioma indeterminada con un script especificado: ' und- &lt; script &gt; '. Por ejemplo, en el caso de las fuentes latinas, use `und-Latn\\fonts.css` y para las fuentes cirílicos `und-Cryl\\fonts.css` .

## <a name="set-the-http-accept-language-request-header"></a>Establecer el encabezado de solicitud HTTP Accept-Language
Tenga en cuenta si los servicios web a los que llama tienen la misma extensión que la aplicación. Las solicitudes HTTP realizadas desde aplicaciones Windows en solicitudes web típicas y XMLHttpRequest (XHR) usan el encabezado de solicitud HTTP Accept-Language estándar. De forma predeterminada, el encabezado HTTP se establece en la lista de idioma del perfil de usuario. Cada idioma de la lista se expande aún más para incluir valores independientes del idioma y una ponderación (q). Por ejemplo, la lista de idioma de un usuario de fr-FR y en-US da como resultado un encabezado de solicitud HTTP Accept-Language de fr-FR, fr, en-US, en ("fr-FR, fr; q = 0,8, en-US; q = 0.5, en; q = 0.3"). Pero si la aplicación meteorológica (por ejemplo) muestra una interfaz de usuario en francés (Francia), pero el idioma superior del usuario en su lista de preferencias es el alemán, deberá solicitar explícitamente francés (Francia) desde el servicio para mantener la coherencia en la aplicación.

## <a name="apis-in-the-windowsglobalization-namespace"></a>API en el espacio de nombres Windows. Globalization
Normalmente, las API del espacio de nombres [**Windows. Globalization**](/uwp/api/windows.globalization?branch=live) usan la lista de idioma de tiempo de ejecución de la aplicación para determinar el idioma. Si ninguno de los idiomas tiene un formato coincidente, se usa la configuración regional del usuario. que es la misma que se usa para el reloj del sistema. La configuración regional del usuario está disponible en **configuración**  >  **hora & región de idioma**  >  **& idioma**  >  **fecha adicional, hora & región de configuración regional**  >  **: cambiar los formatos de fecha, hora o número**. Las API de **Windows. Globalization** también tienen invalidaciones para especificar una lista de los idiomas que se van a usar, en lugar de la lista de idioma de tiempo de ejecución de la aplicación.

Con la clase [**Language**](/uwp/api/windows.globalization.language?branch=live) , puede inspeccionar los detalles de un idioma determinado, como el script del idioma, el nombre para mostrar y el nombre nativo.

## <a name="use-geographic-region-when-appropriate"></a>Usar región geográfica cuando sea necesario
En **configuración**  >  **hora & región de idioma**  >  **&**  >  **país o región**del idioma, el usuario puede especificar su ubicación en el mundo. Puede usar esta configuración, en lugar de idioma, para elegir el contenido que se va a mostrar al usuario. Por ejemplo, una aplicación de noticias podría mostrar el contenido de esta región de forma predeterminada.

En el código, puede tener acceso a esta configuración mediante la propiedad [**GlobalizationPreferences. HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion) .

Con la clase [**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live) , puede inspeccionar los detalles de una región determinada, como su nombre para mostrar, el nombre nativo y las monedas en uso.

## <a name="examples"></a>Ejemplos
En la tabla siguiente se incluyen ejemplos de lo que el usuario verá en la interfaz de usuario de la aplicación en varias configuraciones de idioma y región.

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Lista de idiomas del manifiesto de aplicación</th>
<th align="left">Lista de idioma del perfil de usuario</th>
<th align="left">Invalidación de idioma principal de la aplicación (opcional)</th>
<th align="left">Lista de idiomas del tiempo de ejecución de aplicaciones</th>
<th align="left">Lo que el usuario ve en la aplicación</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Inglés (Reino Unido) (predeterminado); Alemán (Alemania)</td>
<td align="left">Inglés (GB)</td>
<td align="left">ninguno</td>
<td align="left">Inglés (GB)</td>
<td align="left">UI: Inglés (Reino Unido)<br>Fechas/Horas/Números: Inglés (Reino Unido)</td>
</tr>
<tr>
<td align="left">Alemán (Alemania) (predeterminado); Francés (Francia); Italiano (Italia)</td>
<td align="left">Francés (Austria)</td>
<td align="left">ninguno</td>
<td align="left">Francés (Austria)</td>
<td align="left">UI: Francés (Francia) (reserva desde francés (Austria))<br>Fechas/Horas/Números: Francés (Austria)</td>
</tr>
<tr>
<td align="left">Inglés (Estados Unidos) (predeterminado); Francés (Francia); Inglés (Reino Unido)</td>
<td align="left">Inglés (Canadá); Francés (Canadá)</td>
<td align="left">ninguno</td>
<td align="left">Inglés (Canadá); Francés (Canadá)</td>
<td align="left">UI: Inglés (Estados Unidos) (reserva desde inglés (Canadá))<br>Fechas/Horas/Números: Inglés (Canadá)</td>
</tr>
<tr>
<td align="left">Español (España) (predeterminado); Español (México); Español (Latinoamérica); Portugués (Brasil)</td>
<td align="left">Inglés (EE. UU.)</td>
<td align="left">ninguno</td>
<td align="left">Español (España)</td>
<td align="left">UI: Español (España) (usa el valor predeterminado dado que no existe reserva disponible para el inglés)<br>Fechas/Horas/Números Español (España)</td>
</tr>
<tr>
<td align="left">Catalán (predeterminado); Español (España); Francés (Francia)</td>
<td align="left">Catalán; Francés (Francia)</td>
<td align="left">ninguno</td>
<td align="left">Catalán; Francés (Francia)</td>
<td align="left">UI: principalmente catalán y algo de francés (Francia) porque no todas las cadenas están en catalán<br>Fechas/Horas/Números: catalán</td>
</tr>
<tr>
<td align="left">Inglés (Reino Unido) (predeterminado); Francés (Francia); Alemán (Alemania)</td>
<td align="left">Alemán (Alemania); Inglés (Reino Unido)</td>
<td align="left">Inglés (Reino Unido) (elegido por el usuario en la interfaz de usuario de la aplicación)</td>
<td align="left">Inglés (Reino Unido); Alemán (Alemania)</td>
<td align="left">UI: Inglés (Reino Unido) (invalidación de idioma)<br>Fechas/Horas/Números Inglés (Reino Unido)</td>
</tr>
</tbody>
</table>

>[!NOTE]
> Para obtener una lista de los códigos de país o región estándar usados por Microsoft, consulte la [lista de países o regiones oficiales](/windows/uwp/publish/supported-languages).

## <a name="important-apis"></a>API importantes
* [GlobalizationPreferences. Languages](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages.ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext. QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext. Languages](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages. Languages](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [Idioma](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>Temas relacionados
* [Etiqueta de idioma de BCP-47](https://tools.ietf.org/html/bcp47)
* [Registro de subetiqueta del lenguaje IANA](https://www.iana.org/assignments/language-subtag-registry)
* [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Idiomas admitidos](../../publish/supported-languages.md)
* [Globalizar los formatos de fecha, hora y número](use-global-ready-formats.md)
* [Cómo el sistema de administración de recursos compara etiquetas de idioma](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>Ejemplos
* [Ejemplo de recursos de aplicación y localización](https://code.msdn.microsoft.com/windowsapps/Application-resources-and-cd0c6eaa)
