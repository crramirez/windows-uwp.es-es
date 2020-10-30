---
description: El kit de herramientas de aplicaciones multilingües (PASPARTÚ) 4,0 se integra con Microsoft Visual Studio 2019 para proporcionar a las aplicaciones de Windows compatibilidad con traducción, administración de archivos de traducción y herramientas del editor.
title: Usar el Kit de herramientas para aplicaciones multilingües
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 912a92e0eaabbefb4bf3e6f1ac3596d6a2919234
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034338"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>Usar el Kit de herramientas para aplicaciones multilingües 4.0

El kit de herramientas de aplicaciones multilingües (PASPARTÚ) 4,0 se integra con Microsoft Visual Studio 2019 para proporcionar a las aplicaciones de Windows compatibilidad con traducción, administración de archivos de traducción y herramientas del editor. Estas son algunas de las propuestas de valores del kit de herramientas.

- Ayuda a administrar los cambios de recursos y el estado de la traducción durante el desarrollo.
- Proporciona una interfaz de usuario para elegir idiomas basados en proveedores de traducción configurados.
- Admite el formato de archivo XLIFF estándar del sector de localización.
- Proporciona un motor pseudo-lenguaje que ayuda a identificar los problemas de traducción durante el desarrollo.
- Se conecta con el portal de lenguaje de Microsoft para acceder fácilmente a la terminología y las cadenas traducidas.
- Se conecta con el traductor de Microsoft para obtener sugerencias de traducción rápida.

## <a name="how-to-use-the-toolkit"></a>Cómo usar el kit de herramientas

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>Paso 1. Diseño de la aplicación para globalización y localización

Antes de poder usar el PASPARTÚ de forma eficaz, la aplicación debe ser localizable. En concreto, el proyecto debe contener uno o más archivos de recursos (. resw) que contengan las cadenas de la aplicación en el idioma predeterminado. Para obtener más información, consulte [localizar cadenas en el manifiesto del paquete de la interfaz de usuario y de la aplicación](../../app-resources/localize-strings-ui-manifest.md). Una vez hecho esto, el kit de herramientas permite agregar más idiomas de forma rápida y sencilla.

Para la propuesta de valor de globalización y localización, así &mdash; como definiciones de los términos **globalización** , **localizabilidad** y **localización** , &mdash; consulte [globalización y localización](globalizing-portal.md).

Vea también [instrucciones para la globalización](guidelines-and-checklist-for-globalizing-your-app.md) y [haga que su aplicación sea localizable](prepare-your-app-for-localization.md).

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>Paso 2. Descargar e instalar el kit de herramientas de aplicaciones multilingüe 4,0

Hay dos partes en el kit de herramientas de aplicaciones multilingüe 4,0 (PASPARTÚ 4,0), cada una con su propio instalador.

- [Extensión del kit de herramientas de aplicaciones multilingüe 4,0 para Visual Studio 2017 y versiones posteriores](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308). Contiene la extensión PASPARTÚ 4,0 para Visual Studio 2019, en forma de instalador. vsix.
- [Editor multilingüe del kit de herramientas de aplicaciones 4,0](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit). Contiene la herramienta de editor multilingüe independiente del PASPARTÚ 4,0, en forma de instalador. msi. También incluye la extensión PASPARTÚ 4,0 para Visual Studio 2015 y Visual Studio 2013.

Si usa Visual Studio 2017 o Visual Studio 2019, descargue y ejecute ambos instaladores, uno tras otro. Si usa Visual Studio 2015 o Visual Studio 2013, descargue y ejecute el instalador. msi.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>Paso 3. Habilitación del kit de herramientas de aplicaciones multilingüe para su proyecto

El PASPARTÚ debe estar habilitado para el proyecto antes de poder localizar la aplicación. Aquí se muestra cómo habilitar el kit de herramientas.

- Abra la solución de proyecto en Visual Studio.
- Seleccione el proyecto que desee en Explorador de soluciones.
- En el menú **herramientas** , seleccione **Multilingual App Toolkit**  >  **Habilitar selección** . 

En la ventana de salida (que muestra la salida del kit de herramientas de aplicaciones multilingüe), vea el mensaje `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>` . Si aparece este mensaje, el PASPARTÚ está listo para usarse.

### <a name="step-4-add-languages-to-your-project"></a>Paso 4. Agregar idiomas al proyecto

Siga estos pasos para agregar idiomas a su proyecto.

1. En el Explorador de soluciones, haga clic con el botón secundario en el nodo del proyecto.
2. Haga clic en **Multilingual App Toolkit**  >  **Agregar idiomas de traducción..** ..
3. En el cuadro de diálogo idiomas de traducción, seleccione los idiomas que desea admitir y haga clic en Aceptar.

El kit de herramientas realiza estas acciones en respuesta.

- Para cada idioma agregado, se crea una nueva carpeta con el nombre de la [etiqueta de idioma BCP-47](https://tools.ietf.org/html/bcp47) del idioma. Dentro de esa carpeta, se crean nuevos archivos de recursos (. resw) para que coincidan con los que contienen las cadenas de idioma predeterminadas.
- Si es la primera vez que agrega un idioma, `MultilingualResources` se agrega al proyecto una nueva carpeta denominada. Dentro de esa carpeta, se agrega un archivo. xlf para cada idioma. Los archivos. xlf contienen una unidad de traducción para cada cadena de cada archivo de recursos (. resw) del proyecto.
- La ventana de salida confirma la adición de los lenguajes que ha agregado.

Siempre que agregue o quite un archivo de recursos de idioma predeterminado (. resw) o agregue o quite una cadena en un archivo de recursos de idioma predeterminado (. resw), recompile el proyecto para volver a sincronizar los archivos. xlf. Esto garantiza que los archivos. xlf contienen la Unión de las cadenas en el idioma predeterminado.

Los proveedores de traducción instalados, como Microsoft &mdash; [Language portal](https://www.microsoft.com/Language/) y [Microsoft Translator](https://www.microsofttranslator.com/), &mdash; se pueden usar para traducir los recursos de la aplicación. Cuando un proveedor admite un idioma específico, se muestra el icono del proveedor junto al nombre del idioma en el cuadro de diálogo idiomas de traducción.

En el cuadro de diálogo idiomas de traducción, los idiomas basados en. xlf detectados por el kit de herramientas tienen la casilla de selección activada previamente para indicar que el idioma ya está incluido en el proyecto.

Una vez que se agrega un idioma al proyecto, no se puede quitar al desactivar la casilla en el cuadro de diálogo idiomas de traducción. Para quitar un idioma, haga clic con el botón derecho en el archivo. xlf específico del idioma y seleccione **eliminar** . Si lo confirma, también se eliminará el archivo de recursos (. resw) correspondiente.

### <a name="step-5-test-your-app-using-pseudo-language"></a>Paso 5. Prueba de la aplicación con un pseudo idioma

Pseudo idioma es una modificación artificial del producto de software diseñado para simular la localización de idioma real, pero que queda legible para los hablantes nativos. La pseudo traducción reemplaza los caracteres y expande la longitud de la cadena de recursos para detectar posibles problemas de localizabilidad o errores al principio del ciclo del proyecto y antes de que se inicie la localización en Earnest.

Siga estos pasos para deslocalizar y probar el proyecto.

1. Use el cuadro de diálogo idiomas de traducción para agregar pseudo Language (pseudo) [QPS-PLOC] a su proyecto.
2. Haga clic con el botón derecho en el `<project-name>.qps-ploc.xlf` archivo en explorador de soluciones y haga clic en **Multilingual App Toolkit**  >  **generar traducciones de máquinas** .
3. En **configuración**  >  **hora & región de idioma**  >  **&**  >  **idiomas** , haga clic en **Agregar un idioma** .
5. En el cuadro de búsqueda, escriba `qps-ploc`.
6. Haga clic `English (qps-ploc)` para agregarlo.
7. En la lista idioma, seleccione `English (qps-ploc)` y haga clic en **establecer como predeterminado** .
8. Pruebe la aplicación pseudo localizada. Por ejemplo, busque los problemas de diseño de la interfaz de usuario en los que no se muestre toda una cadena (la cadena está truncada) o las cadenas que no se han traducido (pero en su lugar se ha codificado de forma rígida).

Además del reemplazo y la expansión de caracteres, el pseudo motor proporciona un identificador de seguimiento único para cada recurso. Este seguimiento se antepone al inicio de cada cadena y se escribe entre corchetes `[xxxxx]` . Puede usar estos rastreadores durante las pruebas de inspección de la interfaz de usuario visual. Pueden ayudar a realizar un seguimiento de los recursos específicos del producto, especialmente si varios recursos tienen texto similar o duplicado.

En este "Hello, World!" ejemplo de texto, la pseudo traducción se expande para ocupar aproximadamente un 30 por ciento más de espacio de pantalla y, a continuación, aplica el seguimiento de recursos.

`"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"`

### <a name="step-6-translate-your-app-into-selected-languages"></a>Paso 6. Trasladar la aplicación a los idiomas seleccionados

El kit de herramientas de aplicaciones multilingües se integra en el proceso de compilación. Durante una compilación, las cadenas actualizadas se agregan automáticamente a cada archivo. xlf de idioma.
Después de probar la aplicación mediante un pseudo idioma, hay tres opciones para traducir la aplicación en otros idiomas para su lanzamiento.

#### <a name="option-1-translate-the-strings-yourself"></a>Opción 1. Traduzca las cadenas por su cuenta

Puede usar el editor multilingüe para traducir cadenas individualmente. Como ya se mencionó, se incluye en [el instalador. msi](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit).

- Haga clic con el botón secundario en el archivo. xlf que desee traducir.
- Haga clic en **abrir con...** y seleccione editor multilingüe. Opcionalmente, puede hacer clic en **establecer como predeterminado** .
- Para cada cadena, **source** muestra la cadena original en el idioma predeterminado. En **traducción** , escriba la cadena traducida en el idioma correspondiente al archivo. xlf que está editando.
- Cuando haya terminado, guarde y cierre el archivo.

Recompile el proyecto para que las cadenas traducidas se copien en el archivo de recursos (. resw) que corresponde al archivo. xlf que estaba editando.

También puede iniciar el editor multilingüe de este modo. Vaya a Inicio, Mostrar todas las aplicaciones, abra la carpeta Multilingual App Toolkit y haga clic en editor multilingüe para iniciarla.

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>Opción 2. Enviar los archivos. xlf a un tercero para su traducción

Para externalizar el trabajo de traducción y edición a los localizadores, seleccione los archivos. xlf que desee en explorador de soluciones, haga clic con el botón secundario en ellos y haga clic en el **Kit de herramientas de aplicaciones multilingües**  >  **exportar traducciones.** ...

Seleccione **salida: destinatario de correo** en el cuadro de diálogo exportar recursos de cadena y haga clic en Aceptar. los archivos se comprimirán y adjuntarán a un correo electrónico nuevo. Seleccionar **salida: ubicación** de la carpeta de archivos, explorador de una carpeta y haga clic en Aceptar. Si lo desea, elija los archivos que se van a comprimir, vuelva a hacer clic en aceptar, y los archivos se guardarán (en zip y) en la ubicación que elija, dentro de una nueva carpeta denominada para el proyecto.

Una vez que los localizadores hayan completado el trabajo de traducción y le envíen los archivos. xlf traducidos, puede importarlos en el proyecto. Seleccione los archivos. xlf que desee en explorador de soluciones, haga clic en ellos con el botón secundario y haga clic en el **Kit de herramientas de aplicaciones multilingües**  >  **importar/reciclar traducciones.** ... Haga clic en **Agregar** , desplácese hasta los archivos. xlf o. zip y haga clic en **importar** .

**Nota:** El proceso de importación realiza la validación básica antes de la importación. Esto garantiza que la información de la referencia cultural de destino de los archivos que se van a importar coincide con la de los archivos. xlf existentes.

Recompile el proyecto para que las cadenas traducidas se copien en los archivos de recursos (. resw) que correspondan a los archivos. xlf que acaba de importar.

Estos proveedores de terceros ofrecen servicios de localización y pueden ayudarle.

- [Elanex](https://www.strakertranslations.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.rws.com/what-we-do/rws-moravia/)
- [SDL](https://www.sdl.com/translate/get-started/instant-quote.html)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> La lista anterior se proporciona solo con fines informativos y no es una aprobación. Microsoft no emite ninguna declaración ni garantía con respecto a estos proveedores o sus servicios y en ninguna circunstancia Microsoft asumirá ninguna responsabilidad por el uso de dichos proveedores o servicios. Cualquier pregunta, queja o reclamación relacionada con estos proveedores o sus servicios debe dirigirse al proveedor correspondiente.

#### <a name="option-3-use-the-integrated-translation-services"></a>Opción 3. Usar los servicios de traducción integrados

Los servicios de traducción se integran en el IDE de Visual Studio, así como en el editor multilingüe. Esto proporciona acceso sencillo a los servicios de traducción mientras desarrolla el producto y localiza los recursos. Para este servicio, necesitará una suscripción a la cuenta de Azure, tal y como se describe en [Microsoft Translator se desplaza a la Azure portal](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal).

Para acceder a los servicios de traducción dentro de Visual Studio, seleccione y haga clic con el botón secundario en uno o varios archivos. xlf en Explorador de soluciones y haga clic en **generar traducciones de máquinas** .

El editor multilingüe proporciona la misma compatibilidad de traducción, así como agregar sugerencias de traducción interactiva, que le permiten seleccionar la traducción que mejor se adapte a sus cadenas de recursos. Una vez proporcionada la sugerencia de traducción, puede ajustar la cadena para el estilo de traducción.

Se incluyen dos proveedores con el kit de herramientas de aplicaciones multilingüe.

- El proveedor del [portal de lenguaje de Microsoft](https://www.microsoft.com/Language/) permite el reciclaje de la traducción y la compatibilidad con la búsqueda de coincidencias con la terminología basada en traducciones del texto de la interfaz de usuario para productos y servicios de Microsoft.
- El proveedor de [traductores de Microsoft](https://www.microsofttranslator.com/) habilita los servicios de traducción automática a petición.

Usted y sus traductores pueden administrar el estado de las traducciones en el editor multilingüe para revisar inciertas traducciones más adelante. Puede establecer el estado de cada cadena en la pestaña **propiedades** . Los valores de estado son: **nuevo** , **necesita revisar** , **traducir** , **Finalizar** y **Cerrar sesión** . El indicador situado a la izquierda de la fila muestra el estado. Cuando todas las filas muestran el color verde en el editor multilingüe, se realiza el trabajo de traducción.

Recompile el proyecto para que las cadenas traducidas se copien en los archivos de recursos (. resw) que correspondan a los archivos. xlf que acaba de editar.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>Paso 7. Carga de la aplicación en el Microsoft Store

Antes de iniciar el proceso de certificación de Microsoft Store, debe excluir el `<project-name>.qps-ploc.xlf` archivo del proyecto. Pseudo Language se usa para detectar posibles problemas de localizabilidad o errores, pero no es un lenguaje Microsoft Store válido. Si no se quita, se producirá un error en la aplicación durante el proceso de certificación de Microsoft Store.

## <a name="related-topics"></a>Temas relacionados

* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](../../app-resources/localize-strings-ui-manifest.md)
* [Globalización y localización](globalizing-portal.md)
* [Directrices sobre globalización](guidelines-and-checklist-for-globalizing-your-app.md)
* [Hacer que la aplicación sea localizable](prepare-your-app-for-localization.md)
* [Etiqueta de idioma de BCP-47](https://tools.ietf.org/html/bcp47)

## <a name="downloads"></a>Descargas

* [Kit de herramientas de aplicaciones multilingües 4,0. instalador de VSIX](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [Instalador de Multilingual App Toolkit 4,0. msi](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>Servicios de traducción

* [Portal de lenguaje de Microsoft](https://www.microsoft.com/Language/)
* [Microsoft Translator](https://www.microsofttranslator.com/)
