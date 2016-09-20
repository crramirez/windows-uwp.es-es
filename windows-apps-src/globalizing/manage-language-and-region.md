---
author: DelfCo
Description: "Controla el modo en que Windows selecciona los recursos de la interfaz de usuario y da formato a los elementos de la interfaz de usuario de la aplicación mediante las diversas opciones de idioma y región que ofrece Windows."
title: "Administrar los idiomas y la región"
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
label: Manage language and region
template: detail.hbs
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 294f087fffeefda67ddacd09636915144bf18ff4

---

# Administrar los idiomas y la región





**API importantes**

-   [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**Espacio de nombres WinJS.Resources**](https://msdn.microsoft.com/library/windows/apps/br229779)

Controla el modo en que Windows selecciona los recursos de la interfaz de usuario y da formato a los elementos de la interfaz de usuario de la aplicación mediante las diversas opciones de idioma y región que ofrece Windows.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Introducción


Para obtener una aplicación de muestra que indica cómo administrar la configuración de idioma y región, consulta [Muestra de recursos de la aplicación y localización](http://go.microsoft.com/fwlink/p/?linkid=231501).

Un usuario de Windows no necesita elegir solamente un idioma de un conjunto limitado de idiomas. Por el contrario, puede decir a Windows que habla cualquier idioma del mundo, incluso si Windows en sí no está traducido a ese idioma. El usuario puede hasta especificar que puede hablar múltiples idiomas.

Puede especificar su ubicación, que puede ser en cualquier parte del mundo. También puede especificar que habla cualquier idioma de cualquier ubicación. La ubicación y el idioma no se limitan entre sí. Tan solo porque el usuario hable francés no significa que se encuentra en Francia, ni porque se encuentre en Francia significa que prefiere hablar francés.

Los usuarios de Windows pueden ejecutar aplicaciones en un idioma completamente distinto al de Windows. Por ejemplo, pueden ejecutar una aplicación en español mientras Windows esté ejecutándose en inglés.

En las aplicaciones de la Tienda Windows, los idiomas se representan mediante una [etiqueta de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). La mayoría de las API en Windows Runtime, HTML y XAML puede devolver o aceptar representaciones de cadena de estas etiquetas de idioma BCP-47. Consulta también la [lista de idiomas de IANA](http://go.microsoft.com/fwlink/p/?linkid=227303).

Para ver la lista de etiquetas de idioma que se admiten específicamente en la Tienda Windows, consulta [Idiomas admitidos](https://msdn.microsoft.com/library/windows/apps/jj657969).

## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Tareas


### <span id="Users_can_set_their_language_preferences."></span><span id="users_can_set_their_language_preferences."></span><span id="USERS_CAN_SET_THEIR_LANGUAGE_PREFERENCES."></span>Los usuarios pueden establecer sus preferencias de idioma.

La lista de preferencias de idioma del usuario es una lista ordenada que describe los idiomas del usuario en el orden en el que este los prefiere.

El usuario establece la lista en **Configuración**&gt;**Hora e idioma**&gt;**Región e idioma**. Como alternativa, puede usar **Panel de control**&gt;**Reloj, idioma y región**.

La lista de preferencias de idioma del usuario puede contener varios idiomas y variantes regionales u otras variantes específicas. Por ejemplo, el usuario puede preferir fr-CA, pero también puede comprender en-GB.

### <span id="Specify_the_supported_languages_in_the_app_s_manifest."></span><span id="specify_the_supported_languages_in_the_app_s_manifest."></span><span id="SPECIFY_THE_SUPPORTED_LANGUAGES_IN_THE_APP_S_MANIFEST."></span>Especifica los idiomas admitidos en el manifiesto de la aplicación.

Especifica la lista de idiomas admitidos en el [**elemento Resources**](https://msdn.microsoft.com/library/windows/apps/dn934770) del archivo de manifiesto de la aplicación (generalmente Package.appxmanifest) o Visual Studio genera automáticamente la lista de idiomas en el archivo de manifiesto en función de los idiomas encontrados en el proyecto. El manifiesto debe describir con precisión los idiomas admitidos en el nivel apropiado de granularidad. Los idiomas enumerados en el manifiesto son los idiomas que se muestran a los usuarios en la Tienda Windows.

### <span id="Specify_the_default_language."></span><span id="specify_the_default_language."></span><span id="SPECIFY_THE_DEFAULT_LANGUAGE."></span>Especifica el idioma predeterminado.

Abre package.appxmanifest en Visual Studio, ve a la pestaña **Aplicación** y establece el idioma que usaste para crear la aplicación como idioma predeterminado.

Una aplicación usa el idioma predeterminado cuando no admite ninguno de los idiomas que el usuario ha elegido. Visual Studio usa el idioma predeterminado para agregar metadatos a los activos marcados en ese idioma, lo que permite que se elijan los activos apropiados en el tiempo de ejecución.

La propiedad del idioma predeterminado también debe configurarse como el primer idioma del manifiesto para establecer adecuadamente el idioma de la aplicación (descrito en el siguiente paso, Crea la lista de idiomas de la aplicación). Los recursos del idioma predeterminado deben seguir calificándose con su idioma (por ejemplo, en-US/logo.png). El idioma predeterminado no especifica el idioma implícito de los activos sin calificar. Para obtener más información, consulta [Cómo asignar nombre a los recursos mediante calificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).

### <span id="Qualify_resources_with_their_language."></span><span id="qualify_resources_with_their_language."></span><span id="QUALIFY_RESOURCES_WITH_THEIR_LANGUAGE."></span>Califica los recursos con su idioma.

Ten muy en cuenta el público y el idioma y la ubicación de los usuarios a los que quieres dirigirte. Muchas personas que viven en una región no prefieren el idioma principal de dicha región. Por ejemplo, en muchos hogares de los Estados Unidos, el idioma principal es el español.

Cuando califiques los recursos con el idioma:

-   Incluye script cuando no se haya definido ningún valor de script de supresión para el idioma. Consulta el [Registro de subetiquetas de IANA](http://go.microsoft.com/fwlink/p/?linkid=227303) para obtener más información sobre las etiquetas de idioma. Por ejemplo, usa zh-Hant, zh-Hant-TW o zh-Hans, y no zh-CN o zh-TW.
-   Marca todo el contenido lingüístico con un idioma. La propiedad de proyecto de idioma predeterminado no es el idioma de recursos sin marcar (es decir, independiente del idioma); especifica qué recurso de idioma marcado debería elegirse si no hay ningún otro recurso de idioma marcado que coincida con el usuario.

Marca los activos con una representación precisa del contenido.

-   Windows lleva a cabo tareas complejas de coincidencia incluso entre variantes regionales (por ejemplo, en-US o en-GB), de modo que las aplicaciones pueden marcar libremente los activos con una representación precisa del contenido y permitir que Windows obtenga la coincidencia adecuada para cada usuario.
-   La Tienda Windows muestra el contenido del manifiesto a los usuarios que ven la aplicación.
-   Ten en cuenta que algunas herramientas y otros componentes como los traductores automáticos pueden buscar etiquetas de idioma específicas, como información de dialectos, que son útiles para comprender los datos.
-   Asegúrate de marcar los activos con detalles completos, especialmente cuando hay varias variantes disponibles. Por ejemplo, marca en-GB y en-US si ambas son específicas de esa región.
-   En el caso de los idiomas con un solo dialecto estándar, no es necesario agregar la región. En algunas situaciones resulta razonable usar una etiqueta general, por ejemplo, marcar los activos con ja en lugar de ja-JP.

En ocasiones no es necesario localizar todos los recursos.

-   En el caso de los recursos que se ofrecen en todos los idiomas, como las cadenas de la interfaz de usuario, puedes marcarlos con su idioma correspondiente y asegurarte de que todos estos recursos están en el idioma predeterminado. No es necesario especificar un recurso independiente de idioma (no marcado con un idioma).
-   En los recursos incluidos en un subconjunto del conjunto completo de idiomas de la aplicación (localización parcial), especifica el conjunto de los idiomas de los activos y asegúrate de que todos estos recursos están en el idioma predeterminado. Windows selecciona el mejor idioma posible para el usuario según el orden de preferencia de los idiomas que habla el usuario. Por ejemplo, puede ser que no toda la interfaz de usuario de una aplicación se localice en catalán si la aplicación tiene un conjunto de recursos completo en español. Para los usuarios que hayan establecido como idioma de preferencia el catalán y después el español, los recursos no disponibles en catalán aparecerán en español.
-   En el caso de los recursos que tengan excepciones específicas en algunos idiomas y que en el resto de idiomas se asignen a un recurso común, el recurso que debería usarse para todos los idiomas debería marcarse con la etiqueta de idioma predeterminado 'und'. Windows interpreta la etiqueta de idioma 'und' de manera similar a '\*', en el sentido de que establece la correspondencia con el idioma de la aplicación principal después de cualquier otra coincidencia específica. Por ejemplo, si algunos recursos (como el ancho de un elemento) son distintos para finés pero los demás recursos son iguales para todos los idiomas, el recurso de finés debería marcarse con la etiqueta de idioma finés y el resto debería marcarse con 'und'.
-   En el caso de los recursos que se basan en el script de un idioma y no en el idioma, como la fuente o el alto del texto, usa la etiqueta de idioma indeterminado con un script especificado: 'und-&lt;script&gt;'. Por ejemplo, para las fuentes latinas usa und-Latn\\fonts.css y para las fuentes cirílicas usa und-Cryl\\fonts.css.

### <span id="Create_the_application_language_list."></span><span id="create_the_application_language_list."></span><span id="CREATE_THE_APPLICATION_LANGUAGE_LIST."></span>Crea la lista de idiomas de la aplicación.

En tiempo de ejecución, el sistema determina las preferencias de idioma del usuario admitidas por la aplicación, según el manifiesto, y crea una *lista de idiomas de la aplicación*. Usa esta lista para determinar los idiomas en los que debería estar la aplicación. La lista determina los idiomas que se usan para los números, horas, fechas, recursos y demás componentes del sistema y la aplicación. Por ejemplo, el Sistema de administración de recursos ([**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022), [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) y el [**WinJS.Resources Namespace**](https://msdn.microsoft.com/library/windows/apps/br229779)) cargan recursos de interfaz de usuario de acuerdo con el idioma de la aplicación. 
            [
              **Windows.Globalization**
            ](https://msdn.microsoft.com/library/windows/apps/br206813) también elige formatos en función de la lista de idiomas de la aplicación. La lista de idiomas de la aplicación está disponible mediante el uso de [**Windows.Globalization.ApplicationLanguages.Languages**](https://msdn.microsoft.com/library/windows/apps/hh972396).

Es difícil establecer la coincidencia de idiomas en los recursos. Te recomendamos que dejes que Windows establezca la coincidencia porque hay muchos componentes opcionales en una etiqueta de idioma que pueden influir en la prioridad de la coincidencia y que se pueden encontrar en la práctica.

Algunos de los componentes opcionales de una etiqueta de idioma son:

-   Script para idiomas de script de supresión. Por ejemplo, en-Latn-US coincide con en-US.
-   Región. Por ejemplo, en-US coincide con en.
-   Variantes. Por ejemplo, de-DE-1996 coincide con de-DE.
-   -x y otras extensiones. Por ejemplo, en-US-x-Pirate coincide con en-US.

También hay muchos componentes en una etiqueta de idioma que no tienen el formato xx o xx-yy, y no todos coinciden.

-   zh-Hant no coincide con zh-Hans.

Windows da prioridad a la coincidencia de idiomas entendida de manera estándar. Por ejemplo, en-US coincide, por orden de prioridad, con en-US, en, en-GB, etc.

-   Windows también establece la coincidencia regional. Por ejemplo, en-US coincide con en-US, luego con en y luego con en-\*.
-   Windows tiene datos adicionales para establecer la coincidencia de afinidad en una región, como la región principal de un idioma. Por ejemplo, fr-FR es una coincidencia mejor para fr-BE que fr-CA.
-   Si dependes de las API de Windows, podrás obtener de manera gratuita cualquier mejora en la coincidencia de idioma de Windows.

La coincidencia con el primer idioma de la lista se produce antes de la coincidencia con el segundo, incluso para otras variantes regionales. Por ejemplo, se prefiere un recurso para en-GB sobre fr-CA si el idioma de la aplicación es en-US. Solo se elige un recurso para fr-CA si no hay recursos para una forma de en.

La lista de idiomas de la aplicación está configurada como la variante regional del usuario, incluso si es diferente de la variante regional que la aplicación proporcionó. Por ejemplo, si el usuario habla en-GB, pero la aplicación admite en-US, la lista de idiomas de la aplicación incluye en-GB. Esto garantiza que las fechas, las horas y los números tengan un formato más cercano a las expectativas del usuario (en-GB), pero los recursos de la interfaz de usuario siguen cargándose (debido a la coincidencia de idiomas) en el idioma admitido por la aplicación (en-US).

La lista de idiomas de la aplicación está compuesta de los siguientes elementos:

1.  
            **(Opcional) Sobrescritura del idioma principal** [**PrimaryLanguageOverride**](https://msdn.microsoft.com/library/windows/apps/hh972398) es una sencilla configuración de sobrescritura para las aplicaciones que ofrecen a los usuarios su propia elección de idioma independiente, o bien para las aplicaciones con un motivo fundado para sobrescribir las opciones de idioma predeterminado. Para más información, echa un vistazo a la [muestra de recursos de la aplicación y localización](http://go.microsoft.com/fwlink/p/?linkid=231501).
2.  **Los idiomas del usuario admitidos por la aplicación.** Se trata de una lista de preferencias de idioma del usuario, en orden de preferencia de idioma. Está filtrada según la lista de idiomas compatibles en el manifiesto de la aplicación. Si los idiomas del usuario se filtran en función de aquellos que la aplicación admite, se mantendrá la coherencia en los kits de desarrollo de software (SDK), las bibliotecas de clases, los paquetes de marcos dependientes y la aplicación.
3.  **Si 1 y 2 están vacíos, el idioma predeterminado o el primer idioma admitido por la aplicación.** Si el usuario no habla ninguno de los idiomas que admite la aplicación, el idioma de la aplicación seleccionado es el primer idioma admitido por la aplicación.

Consulta la sección Comentarios debajo de los ejemplos.

### <span id="Set_the_HTTP_Accept_Language_header."></span><span id="set_the_http_accept_language_header."></span><span id="SET_THE_HTTP_ACCEPT_LANGUAGE_HEADER."></span>Establece el encabezado Accept Language HTTP.

Las solicitudes HTTP realizadas desde aplicaciones de la Tienda Windows y aplicaciones de escritorio en solicitudes web típicas y XMLHttpRequest (XHR) usan el encabezado estándar Accept-Language HTTP. De manera predeterminada, el encabezado HTTP se configura como las preferencias de idioma del usuario (en el orden de preferencia del usuario) como se especifica en **Configuración**&gt;**Hora e idioma**&gt;**Región e idioma**. Cada idioma de la lista se expande aún más para incluir valores independientes del idioma y una ponderación (q). Por ejemplo, la lista de idiomas de un usuario de fr-FR y en-US da como resultado un encabezado Accept-Language HTTP de fr-FR, fr, en-US, en ("fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3").

### <span id="Use_the_APIs_in_the_Windows.Globalization_namespace."></span><span id="use_the_apis_in_the_windows.globalization_namespace."></span><span id="USE_THE_APIS_IN_THE_WINDOWS.GLOBALIZATION_NAMESPACE."></span>Usa las API en el espacio de nombres Windows.Globalization.

Generalmente, los elementos de API del espacio de nombres [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) usan la lista de idiomas de la aplicación para determinar el idioma. Si ninguno tiene un formato que coincida, se usa la configuración regional del usuario, que es la misma que se usa para el reloj del sistema. La configuración regional del usuario está disponible en **Configuración**&gt;**Hora e idioma**&gt;**Región e idioma**&gt;**Opciones adicionales de fecha, hora y configuración regional**&gt;**Región: Cambiar formatos de fecha, hora o número**. Las API de **Windows.Globalization** también admiten una invalidación para especificar una lista de idiomas que se usará en lugar de la lista de idiomas de la aplicación.


            [
              **Windows.Globalization**
            ](https://msdn.microsoft.com/library/windows/apps/br206813) también tiene un objeto [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) proporcionado como objeto auxiliar. Permite que las aplicaciones inspeccionen detalles acerca del idioma como el script del idioma, el nombre para mostrar y el nombre nativo.

### <span id="Use_geographic_region_when_appropriate."></span><span id="use_geographic_region_when_appropriate."></span><span id="USE_GEOGRAPHIC_REGION_WHEN_APPROPRIATE."></span>Usa la región geográfica cuando sea conveniente.

En lugar del idioma, puedes usar la configuración de la región geográfica de la casa del usuario para elegir qué contenido se le debe mostrar. Por ejemplo, el valor predeterminado de una aplicación de noticias puede ser mostrar el contenido de la ubicación principal del usuario, que se establece cuando se instala Windows y está disponible en la interfaz de usuario de Windows en **Región: Cambiar formatos de fecha, hora o número**, tal como se describe en la tarea anterior. Para recuperar la configuración de la región principal del usuario actual, usa [**Windows.System.UserProfile.GlobalizationPreferences.HomeGeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br241829).


            [
              **Windows.Globalization**
            ](https://msdn.microsoft.com/library/windows/apps/br206813) también tiene un objeto [**GeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br206795) proporcionado como objeto auxiliar. Permite que las aplicaciones inspeccionen detalles acerca de una región particular, como su nombre para mostrar, nombre nativo y monedas en uso.

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Comentarios


En la siguiente tabla se incluyen ejemplos de lo que el usuario vería en la interfaz de usuario de la aplicación bajo diversos parámetros de configuración de idioma y región.

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
<th align="left">Idiomas admitidos por la aplicación (definidos en el manifiesto)</th>
<th align="left">Preferencias de idioma del usuario (establecidas en el Panel de control)</th>
<th align="left">Invalidación de idioma principal de la aplicación (opcional)</th>
<th align="left">Idiomas de la aplicación</th>
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

 

## <span id="related_topics"></span>Temas relacionados


* [Etiqueta de idioma de BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Lista de idiomas IANA](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [Ejemplo de recursos de aplicación y localización](http://go.microsoft.com/fwlink/p/?linkid=231501)
* [Idiomas admitidos](https://msdn.microsoft.com/library/windows/apps/jj657969)
 

 






<!--HONumber=Jun16_HO4-->


