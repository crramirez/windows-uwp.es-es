---
author: stevewhims
Description: This topic defines the terms user profile language list, app manifest language list, and app runtime language list. We'll be using these terms in this topic and other topics in this feature area, so it's important to know what they mean.
title: Comprender los idiomas del perfil de usuario y los idiomas de manifiesto de la aplicación
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.author: stwhi
ms.date: 11/08/2017
ms.topic: article
keywords: windows 10, uwp, globalización, localización
ms.localizationpriority: medium
ms.openlocfilehash: 2215231b21700fc17b08c2149316f9a59f8d1f04
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "5546416"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>Comprender los idiomas del perfil de usuario y los idiomas de manifiesto de la aplicación
Un usuario de Windows puede utilizar **Configuración** > **Hora e idioma** > **Región e idioma** para configurar una lista ordenada de idiomas de visualización preferidos o simplemente un único idioma de visualización preferido. Un idioma puede tener una variante regional. Por ejemplo, puedes seleccionar el español de España, el español de México o el español que se habla en Estados Unidos, entre otros.

También, en **Configuración** > **Hora e idioma** > **Región e idioma**, aparte del idioma, el usuario puede especificar su ubicación (conocida como región) en el mundo. Ten en cuenta que el idioma de visualización (y la variante regional) no es un elemento que determina la configuración de la región y viceversa. Por ejemplo, un usuario que actualmente puede estar viviendo en Francia elige como idioma preferido de visualización de Windows el español (México).

En las aplicaciones UWP, los idiomas se representan mediante una [etiqueta de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). Por ejemplo, la etiqueta de idioma BCP-47 "en-US" corresponde a inglés (Estados Unidos) en el menú **Configuración **. Las API de UWP correspondientes aceptan y devuelven representaciones de cadena de las etiquetas de idioma BCP-47.

Consulta también el [Registro de subetiquetas de IANA](http://go.microsoft.com/fwlink/p/?linkid=227303).

Las siguientes tres secciones definen los términos "Lista de idiomas del perfil de usuario", "Lista de idiomas de manifiesto de la aplicación" y "Lista de idiomas de la aplicación en tiempo de ejecución". Utilizaremos estos términos en este y otros temas en esta área de funciones, por lo que es importante que sepas lo que significan.

## <a name="user-profile-language-list"></a>Lista de idiomas del perfil de usuario
La lista de idiomas del perfil de usuario es el nombre de la lista que está configurada por el usuario en **Configuración** > **Hora e idioma** > **Región e idioma** > **Idiomas **. Puedes utilizar la propiedad [**GlobalizationPreferences.Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) en código para acceder a la lista de idiomas del perfil de usuario como una lista de cadenas de solo lectura, donde cada cadena es una [etiqueta de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) como "en-US" o "ja-JP".

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>Lista de idiomas de manifiesto de la aplicación
La lista de idiomas de manifiesto de la aplicación es la lista de idiomas para la que la aplicación declara (o declarará) la compatibilidad. Esta lista crece a medida que avanzas en la aplicación durante el ciclo de vida del desarrollo hasta la localización.

La lista se determina en el tiempo de compilación, pero tienes dos opciones para controlar exactamente cómo se produce. Una opción es dejar que Visual Studio determine la lista desde los archivos del proyecto. Para ello, primero establece el **Idioma predeterminado** de la aplicación en la pestaña **Aplicación** del archivo de origen del manifiesto del paquete de aplicación (`Package.appxmanifest`). A continuación, confirma que el mismo archivo contiene esta configuración (que lo hace de forma predeterminada).

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

Cada vez que Visual Studio genera el archivo de manifiesto del paquete de aplicación integrado (`AppxManifest.xml`), se expande ese único elemento `Resource` en el archivo de origen en una unión de todos los calificadores de idioma que se encuentra en el proyecto (consulta [Adaptar los recursos para los calificadores de idioma, de escala, de contraste alto y otros](../../app-resources/tailor-resources-lang-scale-contrast.md)). Por ejemplo, si has comenzado a localizar y dispones de una cadena, imagen o archivo de recursos cuyo nombre de archivo o carpeta incluye "en-US", "ja-JP" y "fr-FR", a continuación, tu archivo `AppxManifest.xml` integrado contendrá lo siguiente (la primera entrada de la lista es el idioma predeterminado que estableces).

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

La otra opción es reemplazar únicamente ese elemento "x-generar" `<Resource>` en el archivo de origen de manifiesto del paquete de aplicación (`Package.appxmanifest`) con la lista ampliada de elementos `<Resource>` (ten cuidado para mostrar el idioma predeterminado en primer lugar). Esta opción implica más tareas de mantenimiento para ti, pero podría ser una opción adecuada si usas un sistema de compilación personalizado.

Para empezar, la lista de idiomas de manifiesto de la aplicación solo contendrá un idioma. Quizás es en-US. Pero, finalmente, una vez hayas configurado manualmente el manifiesto o al agregar recursos traducidos a tu proyecto, aumentará esa lista.

Cuando la aplicación está en Microsoft Store, los idiomas en la lista de idiomas de manifiesto de la aplicación son los que se muestran a los clientes. Para obtener una lista de etiquetas de idioma BCP-47 que se admitan específicamente en Microsoft Store, consulta [Idiomas admitidos](../../publish/supported-languages.md).

Puedes utilizar la propiedad [**ApplicationLanguages.ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) en código para acceder a la lista de idiomas de manifiesto de la aplicación como una lista de cadenas de solo lectura, donde cada cadena es una etiqueta de idioma BCP-47.

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>Lista de idiomas de la aplicación en tiempo de ejecución
La tercera lista de idiomas de interés es la intersección entre las dos listas que acabamos de describir. En tiempo de ejecución, la lista de idiomas para los que la aplicación declaró compatibilidad (la lista de idiomas de manifiesto de la aplicación) se compara con la lista de idiomas para los que el usuario declara una preferencia (la lista de idiomas del perfil de usuario). La lista de idiomas del tiempo de ejecución de la aplicación está configurada para esta intersección (si no está vacía) o simplemente para el idioma predeterminado de la aplicación (si la intersección está vacía).

Más concretamente, la lista de idiomas de la aplicación en tiempo de ejecución se compone de estos elementos.

1.  **(Opcional) Invalidación del idioma principal**. La [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) es una sencilla configuración de invalidación para las aplicaciones que ofrecen a los usuarios su propia elección de idioma independiente, o bien para las aplicaciones con un motivo fundado para invalidar las opciones de idioma predeterminado. Para más información, echa un vistazo a la [Muestra de recursos de la aplicación y localización](http://go.microsoft.com/fwlink/p/?linkid=231501).
2.  **Los idiomas del usuario son compatibles con la aplicación**. Esta es la lista de idiomas del perfil de usuario filtrada por la lista de idiomas del manifiesto de la aplicación. Si los idiomas del usuario se filtran en función de aquellos que la aplicación admite, se mantendrá la coherencia en los kits de desarrollo de software (SDK), las bibliotecas de clases, los paquetes de marcos dependientes y la aplicación.
3.  **Si 1 y 2 están vacíos, entonces el idioma predeterminado o el primer idioma admitido por la aplicación**. Si la lista de idiomas del perfil de usuario no contiene ninguno de los idiomas que admite la aplicación, entonces el idioma de la aplicación en tiempo de ejecución es el primer idioma admitido por la aplicación.

Puedes utilizar la propiedad [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) para acceder a la lista de idiomas de la aplicación en tiempo de ejecución como una cadena que contiene una lista de etiquetas de idioma BCP-47 separadas por punto y coma.

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

También puedes acceder como una lista de cadenas de solo lectura, cada una con una etiqueta de idioma BCP-47 única. Puedes utilizar la propiedad [**ResourceContext.Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) o la propiedad [**ApplicationLanguages.Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages) para realizarlo.

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

La lista de idiomas de la aplicación en tiempo de ejecución determina los recursos que Windows carga para la aplicación y también el idioma o idiomas utilizados para dar formato a las fechas, horas, números y otros componentes. Consulta [Globalizar los formatos de fecha y hora o número](use-global-ready-formats.md).

**Nota** Si el idioma del perfil de usuario y el idioma del manifiesto de la aplicación son variantes regionales de cada uno, se usa la variante regional del usuario como el idioma de la aplicación en tiempo de ejecución. Por ejemplo, si el usuario prefiere en-GB, pero la aplicación admite en-US, el idioma de la aplicación en tiempo de ejecución es en-GB. Esto garantiza que las fechas, las horas y los números tengan un formato más cercano a las expectativas del usuario (en-GB), pero los recursos localizados siguen cargándose (debido a la coincidencia de idiomas) en el idioma admitido por la aplicación (en-US).

## <a name="qualify-resource-files-with-their-language"></a>Calificar los archivos de recursos con sus idiomas
Nombra los archivos de recursos o sus carpetas con calificadores de recursos de idiomas. Para saber más sobre los calificadores de recursos, consulta [Adaptar los recursos para idioma, escala, contraste alto y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)). Un archivo de recursos puede ser una sola imagen u otro archivo de activos, o puede ser un archivo de recursos de contenedor, como un Archivo de recursos (.resw), que contiene recursos de cadena.

**Nota** Incluso los recursos en el idioma predeterminado de la aplicación deben calificarse con su idioma. Por ejemplo, el idioma predeterminado de tu aplicación es el inglés (Estados Unidos), por lo que califica incluso tus recursos en-US similares a `\Assets\Images\en-US\logo.png`. 

- Windows lleva a cabo la coincidencia compleja, como por ejemplo entre variantes regionales como en-US y en-GB. Por lo tanto, incluye u omite la subetiqueta de región según corresponda. Consulta [Cómo el sistema de gestión de recursos hace coincidir las etiquetas de idioma](../../app-resources/how-rms-matches-lang-tags.md).
- Incluye un script cuando no se haya definido ningún valor de script de supresión para el idioma. Consulta el [Registro de subetiquetas de idioma IANA](http://go.microsoft.com/fwlink/p/?linkid=227303) para obtener más información sobre las etiquetas de idioma. Por ejemplo, usa zh-Hant, zh-Hant-TW o zh-Hans, y no zh-CN o zh-TW.
- En el caso de los idiomas con un solo dialecto estándar, no es necesario agregar la región. En algunas situaciones resulta razonable usar una etiqueta general, por ejemplo, marcar los activos con ja en lugar de ja-JP.
- Algunas herramientas y otros componentes como los traductores automáticos pueden buscar etiquetas de idioma específicas, como información de dialectos, que son útiles para comprender los datos.

A veces no es necesario localizar todos los recursos.

- Para recursos como las cadenas de interfaz de usuario que se ofrecen en todos los idiomas, márcalos con su idioma. Asegúrate de que todas estas cadenas existen en el idioma predeterminado.
- En los recursos incluidos en un subconjunto del conjunto completo de idiomas de la aplicación (localización parcial), especifica el conjunto de los idiomas de los activos y asegúrate de que todos estos recursos están en el idioma predeterminado. Por ejemplo, puede ser que no toda la interfaz de usuario de tu aplicación se localice en catalán si tu aplicación tiene un conjunto de recursos completo en español. Para los usuarios que hayan establecido como idioma de preferencia el catalán y después el español, los recursos que no estén disponibles en catalán aparecerán en español.
- En el caso de los recursos que tengan excepciones específicas en algunos idiomas pero que en el resto de idiomas se asignen a un recurso común, marca el recurso destinado para todos los idiomas con la etiqueta de idioma predeterminado 'und'. Windows interpreta la etiqueta de idioma 'und' como carácter comodín (de manera similar a '\*'), en el sentido de que establece la correspondencia con el idioma de la aplicación principal después de cualquier otra coincidencia específica. Por ejemplo, si algunos recursos son distintos para el finés pero los demás recursos son iguales para todos los idiomas, el recurso de finés debería marcarse con la etiqueta de idioma finés y el resto debería marcarse con 'und'.
- En el caso de los recursos que se basan en el script de un idioma y no en el idioma, como la fuente o el alto del texto, usa la etiqueta de idioma indeterminado con un script especificado: 'und-&lt;script&gt;'. Por ejemplo, para las fuentes latinas `und-Latn\\fonts.css` y cirílicas usa `und-Cryl\\fonts.css`.

## <a name="set-the-http-accept-language-request-header"></a>Establecer el encabezado de solicitud Accept Language HTTP
Considera si los servicios web a los que llamas tienen el mismo grado de localización que tu aplicación. Las solicitudes HTTP realizadas desde aplicaciones UWP y aplicaciones de escritorio en solicitudes web típicas y XMLHttpRequest (XHR) usan el encabezado estándar de solicitud Accept-Language HTTP. De manera predeterminada, el encabezado HTTP se establece en la lista de idiomas del perfil de usuario. Cada idioma de la lista se expande aún más para incluir valores independientes del idioma y una ponderación (q). Por ejemplo, la lista de idiomas de un usuario de fr-FR y en-US da como resultado un encabezado de solicitud Accept-Language HTTP de fr-FR, fr, en-US, en ("fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3"). Pero si tu aplicación del tiempo (por ejemplo) muestra una interfaz de usuario en francés (Francia), pero el idioma que está en primer lugar de la lista de preferencias del usuario es el alemán, a continuación, deberás solicitar explícitamente francés (Francia) desde el servicio para que siga siendo coherente dentro de la aplicación.

## <a name="apis-in-the-windowsglobalization-namespace"></a>API en el espacio de nombres Windows.Globalization.
Generalmente, las API del espacio de nombres [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) utilizan la lista de idiomas de la aplicación en tiempo de ejecución para determinar el idioma. Si ninguno tiene un formato que coincida, se utiliza la configuración regional del usuario, Es la misma que se utiliza para el reloj del sistema. La configuración regional del usuario está disponible en **Configuración** > **Hora e idioma** > **Región e idioma** > **Opciones adicionales de fecha, hora y configuración regional** > **Región: Cambiar formatos de fecha, hora o número**. Las API de **Windows.Globalization** también tienen invalidación para especificar una lista de idiomas que se usará en lugar de la lista de idiomas de la aplicación en tiempo de ejecución.

Al utilizar la clase [**Idioma**](/uwp/api/windows.globalization.language?branch=live), puedes inspeccionar detalles acerca de un idioma en particular como el script del idioma, el nombre para mostrar y el nombre nativo.

## <a name="use-geographic-region-when-appropriate"></a>Usa la región geográfica cuando sea conveniente
En **Configuración** > **Hora e idioma** > **Región e idioma** > **País o región**, el usuario puede especificar su ubicación en el mundo. En lugar del idioma, puedes usar esta configuración para elegir qué contenido se le debe mostrar al usuario. Por ejemplo, una aplicación de noticias puede mostrar el contenido de esta región de manera predeterminada.

Puedes acceder a esta configuración en código utilizando la propiedad [**GlobalizationPreferences.HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion).

Utilizando la clase [**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live) puedes inspeccionar detalles acerca de una región particular, como su nombre para mostrar, nombre nativo y monedas en uso.

## <a name="examples"></a>Ejemplos
En la siguiente tabla se incluyen ejemplos de lo que el usuario vería en la interfaz de usuario de tu aplicación bajo diversos parámetros de configuración de idioma y región.

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
<th align="left">Lista de idiomas de manifiesto de la aplicación</th>
<th align="left">Lista de idiomas del perfil de usuario</th>
<th align="left">Invalidación de idioma principal de la aplicación (opcional)</th>
<th align="left">Lista de idiomas de la aplicación en tiempo de ejecución</th>
<th align="left">Lo que el usuario ve en la aplicación</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Inglés (Reino Unido) (predeterminado); Alemán (Alemania)</td>
<td align="left">Inglés (Reino Unido)</td>
<td align="left">ninguno</td>
<td align="left">Inglés (Reino Unido)</td>
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
<td align="left">Inglés (Estados Unidos)</td>
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

## <a name="important-apis"></a>API importantes
* [GlobalizationPreferences.Languages](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages.ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext.Languages](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages.Languages](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [Idioma](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>Artículos relacionados
* [Etiqueta de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Registro de subetiquetas de idiomas IANA](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [Adaptar los recursos al idioma, escala, contraste alto y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Idiomas admitidos](../../publish/supported-languages.md)
* [Globalizar los formatos de fecha, hora o número](use-global-ready-formats.md)
* [Cómo el sistema de administración de recursos hace coincidir las etiquetas de idioma](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>Ejemplos
* [Ejemplo de recursos de aplicación y localización](http://go.microsoft.com/fwlink/p/?linkid=231501)
