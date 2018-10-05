---
author: stevewhims
Description: The Multilingual App Toolkit (MAT) 4.0 integrates with Microsoft Visual Studio 2017 to provide UWP apps with translation support, translation file management, and editor tools.
title: Usar el Kit de herramientas para aplicaciones multilingües
template: detail.hbs
ms.author: stwhi
ms.date: 01/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 39e002247cabb6389ddf23860499ebf1f166b03a
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "4384921"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>Usar el Kit de herramientas para aplicaciones multilingües 4.0

El Kit de herramientas para aplicaciones multilingües (MAT) 4.0 se integra con Microsoft Visual Studio 2017 para proporcionar a las aplicaciones UWP compatibilidad con traducción, administración de archivos de traducción y herramientas de editor. Estas son algunas de las propuestas de valor del kit de herramientas.

- Te ayuda a administrar el estado de la traducción y cambios en los recursos durante el desarrollo.
- Ofrece una interfaz de usuario para elegir idiomas en función de proveedores de traducción configurados.
- Admite el formato de archivo XLIFF estándar de la industria de localización.
- Proporciona un motor de pseudoidioma que ayuda a identificar problemas de traducción durante el desarrollo.
- Se conecta con el Portal lingüístico de Microsoft para acceder fácilmente a cadenas y terminología traducidas.
- Conecta con Microsoft Translator para obtener rápidas sugerencias de traducción.

## <a name="how-to-use-the-toolkit"></a>Cómo usar el kit de herramientas

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>Paso 1. Diseña tu aplicación para globalización y localización

Para poder usar la MAT eficazmente, la aplicación debe localizarse previamente. En particular, el proyecto debe contener uno o más archivos de recursos (.resw) con las cadenas de la aplicación en el idioma predeterminado. Para más información, consulta [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md). Una vez que lo hayas hecho, resulta muy sencillo y rápido añadir idiomas adicionales con el kit de herramientas.

Para la propuesta de valor de globalización y localización&mdash;, así como las definiciones de los términos de **globalización**, **localizabilidad** y **localización**&mdash;consulta [Globalización y localización ](globalizing-portal.md).

Consulta también [Directrices para globalización](guidelines-and-checklist-for-globalizing-your-app.md) y [Haz que la aplicación sea localizable](prepare-your-app-for-localization.md).

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>Paso 2. Descarga e instala el Kit de herramientas para aplicaciones multilingües 4.0

El Kit de herramientas para aplicaciones multilingües 4.0 (MAT 4.0) tiene dos partes, cada una con su propio instalador.

- [Kit de herramientas para aplicaciones multilingües 4.0, extensión para Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308). Contiene la extensión de MAT 4.0 para Visual Studio 2017 en un instalador .vsix.
- [Editor del Kit de herramientas para aplicaciones multilingües 4.0](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit). Contiene el editor multilingüe independiente MAT 4.0 en un instalador .msi. También incluye la extensión MAT 4.0 para Visual Studio 2015 y Visual Studio 2013.

Si usas Visual Studio de 2017, descarga y ejecuta ambos instaladores, uno tras otro. Si usas Visual Studio 2015 o Visual Studio 2013, descarga y ejecuta el instalador .msi.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>Paso 3. Habilita el Kit de herramientas de aplicaciones multilingües para el proyecto

El MAT debe estar habilitado en el proyecto para poder empezar a localizar la aplicación. Aquí se explica cómo habilitar el kit de herramientas.

- Abre la solución de proyecto en Visual Studio.
- Selecciona el proyecto que quieras en el Explorador de soluciones.
- En el menú **Herramientas**, selecciona **Kit de herramientas para aplicaciones multilingües** > **Habilitar selección**. 

En la ventana de resultados (que muestra el resultado del Kit de herramientas para aplicaciones multilingües), aparecerá el mensaje `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>`. Si aparece este mensaje, el MAT está listo para usarse.

### <a name="step-4-add-languages-to-your-project"></a>Paso 4. Agrega idiomas al proyecto

Sigue estos pasos para agregar idiomas al proyecto.

1. En el Explorador de soluciones, haz clic con el botón derecho en el nodo del proyecto.
2. Haz clic en **Kit de herramientas para aplicaciones multilingües** > **Agregar idiomas de traducción...**.
3. En el cuadro de diálogo Idiomas de traducción, selecciona los idiomas que deseas admitir y haz clic en Aceptar.

El Kit de herramientas realiza estas tareas en respuesta.

- Para cada idioma que agregaste, se crea una carpeta nueva con nombre para la [etiqueta de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) del idioma. Dentro de esa carpeta, se crean nuevos archivos de recursos (.resw) para que coincida con las que contienen las cadenas de idioma predeterminado.
- Si es la primera vez que has agregado un idioma, se añadirá una carpeta nueva denominada `MultilingualResources` al proyecto. Dentro de esa carpeta, se agrega un archivo .xlf para cada idioma. Los archivos .xlf contienen una unidad de traducción para cada cadena en cada archivo de recursos (.resw) del proyecto.
- La ventana de salida confirma la incorporación de los idiomas que agregaste.

Siempre que agregues o quites un archivo de recursos de idioma predeterminado (.resw), o que agregues o quites una cadena dentro de un archivo de recursos de idioma predeterminado (.resw), recompila el proyecto para volver a sincronizar los archivos .xlf. Así se garantiza que los archivos .xlf contengan la unión de las cadenas en el idioma predeterminado.

Se pueden usar los proveedores de traducción instalados&mdash; como el [Portal lingüístico de Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=330295) y [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)&mdash;, para traducir los recursos de la aplicación. Cuando un proveedor admite un idioma específico, se muestra su icono junto al nombre del idioma en el cuadro de diálogo Idiomas de traducción.

En el cuadro de diálogo Idiomas de traducción, la casilla de todos los idiomas basados en .xlf descubiertos por el kit de herramientas aparecerá activada para indicar que ese idioma ya está incluido en el proyecto.

Una vez que se agregan al proyecto, los idiomas no pueden quitarse desactivando el cuadro de diálogo Idiomas de traducción. Para quitar un idioma, haz clic con el botón secundario en el archivo .xlf específico del idioma y selecciona **Eliminar**. Si confirma, también se eliminará el archivo de recursos (.resw) correspondiente.

### <a name="step-5-test-your-app-using-pseudo-language"></a>Paso 5. Prueba la aplicación con Pseudoidioma.

Pseudoidioma es una modificación artificial del producto de software diseñada para simular la localización real de idiomas sin dejar de ser legible para los hablantes nativos. La pseudotraducción reemplaza caracteres y expande la longitud de la cadena del recurso para detectar posibles problemas o errores de localización en una fase temprana del ciclo del proyecto, antes de que comience la localización real de manera determinada.

Sigue estos pasos para pseudolocalizar y probar el proyecto.

1. Usa el cuadro de diálogo Idiomas de traducción para agregar un pseudoidioma (Pseudo) [qps-ploc] al proyecto.
2. Haz clic con el botón derecho en el archivo `<project-name>.qps-ploc.xlf`del Explorador de soluciones y haz clic en **Kit de herramientas para aplicaciones multilingües** > **Generar traducciones automáticas**.
3. En **configuración** > **Hora e idioma** > **Región e idioma** > **Idiomas**, haz clic en **Agregar un idioma**.
5. En el cuadro de búsqueda, escribe `qps-ploc`.
6. Haz clic en `English (qps-ploc)` para añadirlo.
7. En la lista de idiomas, selecciona `English (qps-ploc)` y haz clic en **Establecer como predeterminado**.
8. Prueba la aplicación pseudolocalizada. Por ejemplo, busca problemas de diseño de la interfaz de usuario en los que no se muestre una cadena completa (la cadena se trunca) o en los que haya cadenas que no están traducidas (sino que están codificadas de forma rígida).

Además de la sustitución y la expansión de caracteres, el pseudomotor proporciona un identificador de seguimiento único para cada recurso. Esta herramienta de seguimiento se antepone al comienzo de cada cadena y se encierra entre corchetes `[xxxxx]`. Puedes usar estos rastreadores durante la prueba de inspección de la interfaz de usuario visual. Pueden ayudar a realizar un seguimiento de recursos específicos del producto, especialmente si múltiples recursos tienen texto similar o duplicado.

En el siguiente ejemplo "Hello, World!", la pseudotraducción se expande para que ocupe un 30por ciento más de espacio en la pantalla y después aplica la herramienta de seguimiento de recursos.

```
"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"
```

### <a name="step-6-translate-your-app-into-selected-languages"></a>Paso 6. Traduce la aplicación a los idiomas seleccionados

El Kit de herramientas para aplicaciones multilingües forma parte del proceso de compilación. En una compilación, las cadenas actualizadas se agregan automáticamente a cada archivo .xlf de idioma.
Después de haber probado la aplicación con pseudoidioma, tienes tres opciones para traducir tu aplicación a otros idiomas.

#### <a name="option-1-translate-the-strings-yourself"></a>Opción 1. Traduce las cadenas tú mismo

Puedes usar el Editor multilingüe para traducir las cadenas de manera individual. Como ya hemos explicado, se incluye en [el instalador .msi ](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit).

- Haz clic con el botón secundario en el archivo .xlf que quieras traducir.
- Haz clic en **Abrir con... ** y selecciona el Editor multilingüe. Opcionalmente, puedes hacer clic en **Establecer como predeterminado **.
- Para cada cadena **Origen** se muestra la cadena original en el idioma predeterminado. En **Traducción**, escribe la cadena traducida al idioma correspondiente del archivo .xlf que estás editando.
- Cuando hayas terminado, guarda y cierra el archivo.

Recompila el proyecto para hacer que las cadenas traducidas se copien en el archivo de recursos (.resw) que se corresponda con el archivo .xlf que estabas editando.

También puedes abrir el Editor multilingüe de esta forma. Ve a Inicio, muestra todas las aplicaciones, abre la carpeta del kit de herramientas para aplicaciones multilingües y haz clic en el Editor multilingüe para iniciarlo.

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>Opción 2. Envía los archivos .xlf a un tercero para que los traduzca

Para subcontratar los trabajos de traducción y edición a los localizadores, selecciona los archivos .xlf deseados en el Explorador de soluciones, haz clic en ellos con el botón derecho del ratón y haz clic en **Kit de herramientas para aplicaciones multilingües** > **Exportar traducciones...**.

Selecciona **Salida: destinatario de correo** en el cuadro de diálogo Exportar recursos de cadena, haz clic en Aceptar y los archivos se comprimirán y adjuntarán a un correo electrónico nuevo. Selecciona **Salida: Ubicación de carpeta de archivo**, busca una carpeta y haz clic en Aceptar, elige de manera opcional los archivos que se van a comprimir, haz clic en Aceptar de nuevo y tus archivos se (comprimirán y) guardarán en la ubicación que hayas elegido, dentro de una nueva carpeta con nombre para el proyecto.

Una vez a los localizadores finalicen el trabajo de traducción y envíen los archivos .xlf traducidos, puedes importarlos a tu proyecto. Selecciona los archivos .xlf deseados en el Explorador de soluciones, haz clic en ellos con el botón derecho del ratón y haz clic en **Kit de herramientas para aplicaciones multilingües** > **Importar/reciclar traducciones...**. Haz clic en **Agregar**, navega a los archivos .xlf o .zip y haz clic en **Importar**.

**Nota** El proceso de importación realiza una validación básica antes de la importación. Esto garantiza que la información de referencia cultural de destino en los archivos que se están importando coincida con la de los archivos .xlf existentes.

Recompila el proyecto para hacer que las cadenas traducidas se copien en el archivo de recursos (.resw) que se corresponda con el archivo .xlf que estabas importando.

Estos proveedores de otros fabricantes ofrecen servicios de localización y pueden ayudarte.

- [Elanex](https://www.elanex.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.moravia.com/)
- [SDL](https://www.sdl.com/languagecloud/managed-translation/ilp/instantquote)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> La lista anterior se ofrece solo para fines informativos y no supone ningún compromiso. Microsoft no emite ninguna declaración ni garantía con respecto a estos proveedores o sus servicios y en ninguna circunstancia Microsoft asumirá ninguna responsabilidad por el uso de dichos proveedores o servicios. Las preguntas, quejas o reclamaciones relacionadas con dichos proveedores o servicios se deberán dirigir al proveedor correspondiente.

#### <a name="option-3-use-the-integrated-translation-services"></a>Opción 3. Usar los servicios de traducción integrados

Los servicios de traducción están integrados en el IDE de Visual Studio y en el editor multilingüe. Esto permite acceder a dichos servicios fácilmente tanto al desarrollar el producto como al traducir los recursos. Para este servicio, necesitarás una suscripción de cuenta de Azure, como se describe en [Microsoft Translator pasa al Azure Portal](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal).

Para acceder a los servicios de traducción en Visual Studio, selecciona y haz clic con el botón derecho en uno o varios archivos .xlf del Explorador de soluciones y haz clic en **Generar traducciones automáticas**.

El editor multilingüe te proporciona la misma compatibilidad con traducción, así como las sugerencias de traducción interactivas, que te permiten seleccionar la traducción que mejor se adapte a las cadenas de tus recursos. Después de obtener la sugerencia de traducción, puedes ajustar la cadena a tu estilo de traducción.

En el Kit de herramientas para aplicaciones multilingües se incluyen dos proveedores.

- El proveedor del [Portal lingüístico de Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=330295) admite el reciclaje de traducciones y la correspondencia terminológica en función de las traducciones del texto de la interfaz de usuario de los productos y servicios de Microsoft.
- El proveedor [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220) habilita los servicios de traducción automática a petición.

Tú y tus traductores podéis administrar el estado de las traducciones en el editor multilingüe para revisar más tarde las traducciones que no son seguras. Puedes establecer el estado de cada cadena de la pestaña **Propiedades**. Los valores de estado son: **Nueva**, **Necesita revisión**, **Traducida**, **Final** y **Aprobada**. El indicador a la izquierda de la fila muestra el estado. Cuando todas las filas estén de color verde en el editor multilingüe, el trabajo de traducción habrá finalizado.

Recompila el proyecto para hacer que las cadenas traducidas se copien en el archivo de recursos (.resw) que se corresponda con el archivo .xlf que estabas editando.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>Paso 7. Carga la aplicación en Microsoft Store

Antes de iniciar el proceso de certificación de Microsoft Store, debes excluir el archivo `<project-name>.qps-ploc.xlf` del proyecto. Pseudoidioma se usa para detectar posibles problemas o errores de localización, pero no es un idioma válido para Microsoft Store. Si no se quita, la aplicación producirá un error durante el proceso de certificación de Microsoft Store.

## <a name="related-topics"></a>Temas relacionados

* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md)
* [Globalización y localización](globalizing-portal.md)
* [Directrices sobre globalización](guidelines-and-checklist-for-globalizing-your-app.md)
* [Haz que tu aplicación sea localizable](prepare-your-app-for-localization.md)
* [Etiqueta de idioma de BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)

## <a name="downloads"></a>Descargas

* [Instalador .vsix del Kit de herramientas para aplicaciones multilingües 4.0](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [Instalador .msi del Kit de herramientas para aplicaciones multilingües 4.0](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>Servicios de traducción

* [Portal lingüístico de Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=330295)
* [Microsoft Translator ](http://go.microsoft.com/fwlink/p/?LinkId=258220)
