---
description: En este tema se proporcionan respuestas a preguntas y problemas frecuentes relacionados con el kit de herramientas de aplicaciones multilingües (PASPARTÚ) 4,0.
title: Preguntas más frecuentes sobre el kit de herramientas de aplicaciones multilingües & solución
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización
ms.localizationpriority: medium
ms.openlocfilehash: 86c7805f92adf3551729783e2359c85103a0c13e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030758"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>Preguntas frecuentes y solución de problemas del Kit de herramientas para aplicaciones multilingües 4.0

En este tema se proporcionan respuestas a preguntas y problemas frecuentes relacionados con el kit de herramientas de aplicaciones multilingües (PASPARTÚ) 4,0.

Consulte también [uso del kit de herramientas de aplicaciones multilingüe 4,0](use-mat.md).

**Nota:** El kit de herramientas admite archivos. resw (XAML) y. resjson (JavaScript). Pero en este tema solo haremos referencia a los archivos. resw. Un archivo. resw se conoce como archivo de recursos. Contiene cadenas, ya sea en el idioma predeterminado o traducidas en otro lenguaje. La carpeta que contiene un archivo. resw normalmente se denomina para el valor de una etiqueta de idioma.

## <a name="do-i-need-resw-files-in-multiple-languages"></a>¿Necesito archivos. resw en varios idiomas?

No. Una de las principales ventajas del kit de herramientas es que los archivos. resw en varios idiomas no son necesarios. El kit de herramientas administra y sincroniza los recursos de la aplicación mediante el uso de archivos. xlf. Esto elimina los desafíos asociados al mantenimiento del contenido sincronizado entre varios archivos. resw.

Los proyectos que contienen archivos. resw y. xlf coincidentes provocan que se omitan las traducciones del archivo. xlf. Cuando esto sucede, se muestra una advertencia durante la compilación para indicar que las traducciones de. xlf no se incluyen en la aplicación final. Un archivo. resw y un archivo. xlf coinciden cuando tienen un idioma de destino con el mismo código de idioma. Un ejemplo de par coincidente sería `Strings\de-DE\Resources.resw` y un `<project-name>.de-DE.xlf` archivo (que contiene `target-language="de-DE"` ).

## <a name="can-i-have-resw-files-in-multiple-languages"></a>¿Puedo tener archivos. resw en varios idiomas?

Pero no se recomienda. Si quiere incluir archivos. resw en varios idiomas en el proyecto y usar el kit de herramientas, asegúrese de que no tiene archivos. resw y. xlf coincidentes.

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>No veo una opción en el menú herramientas para habilitar el kit de herramientas de aplicaciones multilingües

Siga estos pasos.

- Asegúrese de seleccionar el nodo del proyecto, no el nodo de la solución, antes de abrir el menú **herramientas** .
- Confirme que la extensión del kit de herramientas se instala con el administrador de extensiones de Visual Studio.
- Confirme que el proyecto es un proyecto de UWP.

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>Al compilar mi proyecto, no veo un mensaje que indica que se ha iniciado una compilación multilingüe del kit de herramientas de aplicaciones

Confirme que ha habilitado el PASPARTÚ para su proyecto. En el menú **herramientas** , seleccione **Multilingual App Toolkit**  >  **Habilitar selección** . Si el proyecto se ha habilitado con una versión anterior, deshabilite y vuelva a habilitar el PASPARTÚ mediante el menú **herramientas** . Esto actualiza el proyecto para que funcione con la nueva versión del kit de herramientas.

Asegúrese de que está instalado el componente "tarea de compilación para todas las ediciones de Visual Studio". Este componente de compilación se instala con la extensión, pero se puede anular manualmente la selección durante la instalación. Este componente es necesario para actualizar los archivos. xlf y agregar la traducción en el archivo PRI. Cuando este componente está instalado y funciona correctamente, verá estos mensajes de compilación.

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>El kit de herramientas informa de que no encontró ningún archivo de idioma XLIFF durante la compilación

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

Este mensaje se muestra cuando el kit de herramientas no encuentra ningún archivo en el proyecto con la extensión. xlf. El kit de herramientas genera y mantiene estos archivos en la `MultilingualResources` carpeta de forma predeterminada. Se pueden migrar; pero es mejor dejarlos en esa carpeta, ya que permite que el editor multilingüe busque los archivos de metadatos relacionados.

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>El archivo my. xlf no se incluye en la lista de archivos procesados por el kit de herramientas durante la compilación

Si excluye manualmente un archivo. xlf del proyecto y, a continuación, lo vuelve a incluir, es posible que el elemento de tipo de archivo no se establezca correctamente. En Visual Studio, seleccione el archivo y compruebe el ventana Propiedades. La acción de compilación del archivo se debe establecer en XliffResource y copiar en el directorio de salida debe establecerse en no copiar. Este es el aspecto que debe tener la referencia en el archivo del proyecto.

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>He agregado lenguajes basados en XLF. ¿Dónde están mis cadenas?

El archivo de recursos de idioma predeterminado (. resw) es el "esquema" canónico de las cadenas que usa la aplicación. Los archivos de recursos traducidos pueden contener todas estas cadenas, o un subconjunto de ellas.

Al compilar el proyecto, los archivos de recursos y los archivos. xlf se sincronizan.

- Los archivos. xlf se actualizan para reflejar cualquier cadena agregada o eliminada, o archivos de recursos agregados o quitados.
- Los archivos de recursos se actualizan para reflejar cualquier cadena traducida en los archivos. xlf.

Esto se explica con más detalle en [usar el kit de herramientas de aplicaciones multilingüe 4,0](use-mat.md).

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>Al compilar el proyecto, los archivos. xlf permanecen vacíos

Antes de poder usar el PASPARTÚ de forma eficaz, la aplicación debe ser localizable. Esto se explica con más detalle en [usar el kit de herramientas de aplicaciones multilingüe 4,0](use-mat.md).

## <a name="what-is-microsoft-translator"></a>¿Qué es Microsoft Translator?

Microsoft Translator es un servicio basado en la nube que proporciona traducción basada en equipo. La traducción automática es ideal para obtener acceso a la traducción cuando la traducción humana no es razonable obtener. Puede obtener más información en [Microsoft Translator](https://www.microsofttranslator.com/).

El kit de herramientas usa el servicio Microsoft Translator para proporcionar sugerencias de traducción. Puede ver qué idiomas son compatibles con Microsoft Translator cuando el icono de Microsoft Translator está presente en el cuadro de diálogo idiomas de traducción.

Puede traducir rápidamente la aplicación mediante Microsoft Translator desde el editor multilingüe seleccionando una cadena y haciendo clic en **traducir** .

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>¿Qué es un pseudo idioma y qué son los pseudo recursos?

Pseudo idioma es una modificación artificial del producto de software diseñado para simular la localización de idioma real. Puede encontrar más detalles sobre el pseudo idioma y los superusuarios de los superrecursos en [usar el kit de herramientas de aplicaciones multilingüe 4,0](use-mat.md).

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>Cómo establecer mi preferencia de idioma en un pseudo idioma para poder probar las cadenas de mis pseudo-Loc?

Esto se explica en [uso del kit de herramientas de aplicaciones multilingüe 4,0](use-mat.md).

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>¿Qué tipo de problemas de localizabilidad puedo encontrar con el pseudo idioma?

Esto se explica en [uso del kit de herramientas de aplicaciones multilingüe 4,0](use-mat.md).

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>No veo ninguna traducción al iniciar la aplicación o la aplicación solo se ha traducido parcialmente

Abra el archivo. xlf en el editor multilingüe para ver si las traducciones están presentes. Cuando se cambian explícitamente las cadenas en el archivo default language. resw, se quitan las traducciones correspondientes de los archivos. xlf. Esto se hace para asegurarse de que una traducción coincide con su cadena de origen. Traduzca las cadenas en los archivos. xlf y vuelva a compilar para actualizar los archivos. resw de idioma no predeterminados.

Si las cadenas se traducen en los archivos. xlf, pero no aparecen en la aplicación, vuelva a compilar el proyecto para actualizar los archivos. resw de idioma no predeterminados. Visual Studio optimiza el comando de compilación para compilar solo los archivos que han cambiado desde la última compilación.

Compruebe el orden de preferencia de idioma. Asegúrese de que el idioma que desea probar aparece en la parte superior de la lista de preferencias de idioma en **configuración** .

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>El kit de herramientas informa de errores 0x80004004 en la salida de la compilación

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

Este mensaje se puede mostrar cuando el formato de región entra en conflicto con la operación de compilación del kit de herramientas. La solución alternativa es cambiar el idioma en **configuración** a en-US durante la compilación.


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>El kit de herramientas informa del error 0x80004005 en la salida de la compilación

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

Este mensaje se puede mostrar cuando el archivo. xlf contiene un idioma de destino no compatible. Por ejemplo, "ZH-CHT" es incorrecto (cámbielo a "ZH-hant") y "ZH-CHS" es incorrecto (cámbielo a "ZH-Hans").

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>¿Hay alguna manera de obtener más información acerca de los errores que veo?

Sí, puede activar el registro detallado en Visual Studio. Haga clic en **herramientas**  >  **Opciones**  >  **proyectos y soluciones**  >  **compilar y ejecutar** . Cambiar el detalle de la salida de la **compilación del proyecto de MSBuild** de mínima a normal o superior.

Ejecutar MSBuild desde la línea de comandos también puede generar mensajes adicionales.

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>Error al importar traducción

El proceso de importación realiza la validación básica antes de la importación. Esto garantiza que la información de la referencia cultural de destino de los archivos que se van a importar coincide con la de los archivos. xlf existentes. Abra los archivos. xlf en el editor multilingüe y asegúrese de que la información de la referencia cultural coincide.

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>¿Qué ocurre si mi traductor no tiene instalado Windows 10 o Visual Studio o el kit de herramientas de aplicaciones multilingües?

Al seleccionar **salida: destinatario de correo** en el cuadro de diálogo exportar recursos de cadena, el correo electrónico incluye un vínculo para descargar e instalar el kit de herramientas de aplicaciones multilingües (paspartú) 4,0. El traductor todavía puede instalar la herramienta de editor multilingüe independiente del PASPARTÚ 4,0 incluso sin Windows 10 ni Visual Studio.

Para obtener más información, consulte [uso del kit de herramientas de aplicaciones multilingüe 4,0](use-mat.md).

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>¿Qué ha ocurrido con los `MarkupRules.xml` `ResourcesLocks.xml` archivos y?

El kit de herramientas de aplicaciones multilingües 4,0 no usa archivos de bloqueo de recursos de propietario. En su lugar, se agrega la etiqueta XLIFF 1,2 `<mrk>` directamente al archivo. xlf para identificar las cadenas que no se modifican durante la traducción del equipo. Esto permite que el archivo XLIFF sea independiente y permite el bloqueo de recursos por archivo.

Estos archivos de compatibilidad adicionales ya no son necesarios y puede eliminarlos de forma segura si los tiene.

## <a name="what-happened-to-the-tpx-file"></a>¿Qué ha ocurrido con el archivo. TPX?

El archivo. TPX proporcionó una manera fácil de incluir `MarkupRules.xml` los `ResourcesLocks.xml` archivos y cuando el archivo. xlf se envió para su traducción. Esta funcionalidad ya no es necesaria.

Si tiene traducciones en un archivo. TPX que necesita recuperar, simplemente cambie el nombre de la extensión de archivo. TPX a. zip. Esto le permitirá abrir y extraer el contenido con el explorador de archivos o con cualquier herramienta compatible con. zip.

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>Creo que he hecho todo bien, pero sigue sin funcionar

Siga estos pasos.

1. Agregue traducciones con uno de los métodos ya descritos.
2. Vuelque el archivo. PRI (consulte [MakePri.exe opciones de línea de comandos](../../app-resources/makepri-exe-command-options.md)) para ver si las traducciones están en el archivo. PRI. Las traducciones aparecerán con el código de idioma y el valor traducido de este modo.
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. Compilar desde un símbolo del sistema; el error resultante puede tener más detalles de lo que se muestra en la salida de la compilación.

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>La aplicación no pudo obtener la certificación del Microsoft Store

Antes de iniciar el proceso de certificación de Microsoft Store, debe excluir el `<project-name>.qps-ploc.xlf` archivo del proyecto. Pseudo Language se usa para detectar posibles problemas de localizabilidad o errores, pero no es un lenguaje Microsoft Store válido. Si no se quita, se producirá un error en la aplicación durante el proceso de certificación de Microsoft Store.

## <a name="related-topics"></a>Temas relacionados

* [Usar el Kit de herramientas para aplicaciones multilingües 4.0](use-mat.md)
* [Microsoft Translator](https://www.microsofttranslator.com/)
* [Opciones de línea de comandos de MakePri.exe](../../app-resources/makepri-exe-command-options.md)
