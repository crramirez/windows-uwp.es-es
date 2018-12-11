---
Description: This topic provides answers to frequently-asked questions and issues related to the Multilingual App Toolkit (MAT) 4.0.
title: Solución de problemas y preguntas más frecuentes sobre el kit de herramientas para aplicaciones multilingües
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, globalización, localización
ms.localizationpriority: medium
ms.openlocfilehash: a39d2b3133714ab784309e131a71219beae4e3c0
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8878861"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>Solución de problemas y preguntas más frecuentes sobre el kit de herramientas para aplicaciones multilingües 4.0

Este tema proporciona respuestas a preguntas frecuentes y problemas relacionados con el kit de herramientas para aplicaciones multilingües (MAT) 4.0.

Consulta también [Utilizar el kit de herramientas para aplicaciones multilingües 4.0](use-mat.md).

**Nota** El kit de herramientas admite archivos .resw (XAML) y .resjson (JavaScript). Pero en este tema nos referiremos solo a los archivos .resw. Un archivo .resw se conoce como un archivo de recursos. Contiene cadenas, ya sea en el idioma predeterminado o traducidas a otro idioma. La carpeta que contiene un archivo .resw normalmente se denomina según el valor de una etiqueta de idioma.

## <a name="do-i-need-resw-files-in-multiple-languages"></a>¿Necesito archivos .resw en varios idiomas?

No. Una de las ventajas clave del kit de herramientas es que no es necesario contar con los archivos .resw en varios idiomas. El kit de herramientas administra y sincroniza los recursos de la aplicación con archivos .xlf. Esto elimina las dificultades relacionadas con la sincronización del contenido en varios archivos .resw.

Los proyectos que contienen archivos .resw y .xlf coincidentes hacen que se omitan las traducciones del archivo .xlf. Cuando esto sucede, se muestra una advertencia durante la compilación para informar de que las traducciones del archivo .xlf no están incluidas en la aplicación final. Un archivo .resw y un archivo .xlf coinciden cuando tienen un idioma de destino con el mismo código de idioma. Un ejemplo de un par coincidente sería `Strings\de-DE\Resources.resw` y un archivo `<project-name>.de-DE.xlf` (que contiene `target-language="de-DE"`).

## <a name="can-i-have-resw-files-in-multiple-languages"></a>¿Puedo tener archivos .resw en varios idiomas?

Sí, pero no lo recomendamos. Si deseas incluir archivos .resw en varios idiomas en tu proyecto y utilizar el kit de herramientas, asegúrate de que no haya archivos .resw y .xlf coincidentes.

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>No veo ninguna opción en el menú Herramientas que permita habilitar el kit de herramientas para aplicaciones multilingües.

Intenta hacerlo siguiendo estos pasos.

- Asegúrate de seleccionar el nodo del proyecto, no el de la solución, antes de abrir el menú **Herramientas**.
- Confirma que esté instalada la extensión del kit de herramientas con el Administrador de extensiones de Visual Studio.
- Confirma que tu proyecto sea un proyecto de UWP.

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>Al compilar mi proyecto, no se ve un mensaje que indique que se ha iniciado una compilación del kit de herramientas para aplicaciones multilingües

Confirma que has habilitado la MAT para el proyecto. En el menú **Herramientas**, selecciona **Kit de herramientas para aplicaciones multilingües** > **Habilitar selección**. Si tu proyecto se habilitó con una versión anterior, deshabilita y vuelve a habilitar la MAT usando el menú **Herramientas**. De este modo, se actualiza el proyecto para que puedas trabajar con la nueva versión del kit de herramientas.

Asegúrate de que esté instalado el componente "Tarea de compilación para todas las ediciones de Visual Studio". El componente de compilación se instala con la extensión, pero se puede anular su selección manualmente durante la instalación. Este componente es necesario para actualizar los archivos .xlf y agregar la traducción al archivo PRI. Cuando este componente esté instalado y funcione correctamente, verás estos mensajes de compilación.

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>El kit de herramientas notifica que no encontró ningún archivo de idioma XLIFF durante la compilación

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

Este mensaje se muestra cuando el kit de herramientas no encuentra ningún archivo con la extensión .xlf en el proyecto. El kit de herramientas genera y mantiene estos archivos en la carpeta `MultilingualResources` predeterminada. Se pueden mover, pero lo más conveniente es dejarlos en esa carpeta porque eso permite al editor multilingüe encontrar los archivos de metadatos relacionados.

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>Mi archivo .xlf no está incluido en la lista de archivos procesados por el kit de herramientas durante la compilación

Si se excluye un archivo .xlf manualmente desde el proyecto y después vuelve a incluirse, es posible que el elemento de tipo de archivo no esté correctamente configurado. Selecciona el archivo en Visual Studio y comprueba la ventana Propiedades. La acción de compilación del archivo debería establecerse en XliffResource y Copiar en el directorio de salida debería establecerse en No copiar. Este es el aspecto que debería tener la referencia en el archivo de proyecto.

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>Agregué idiomas basados en .xlf. ¿Dónde están mis cadenas?

Tu archivo de recursos de idioma predeterminado (.resw) es tu "esquema" canónico de las cadenas que utiliza la aplicación. Los archivos de recursos traducidos pueden contener todas, o un subconjunto de, esas cadenas.

Al compilar el proyecto, los archivos de recursos y los archivos .xlf están sincronizados.

- Los archivos .xlf se actualizan para reflejar cualquier cadena añadida o quitada, o cualquier archivo de recursos añadido o quitado.
- Los archivos de recursos se actualizan para reflejar cualquier cadena traducida en los archivos .xlf.

Esto se explica en detalle en [Usar el kit de herramientas para aplicaciones multilingües 4.0](use-mat.md).

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>Cuando compilo mi proyecto, los archivos .xlf permanecen vacíos

Para poder usar la MAT eficazmente, la aplicación debe localizarse previamente. Esto se explica en detalle en [Usar el kit de herramientas para aplicaciones multilingües 4.0](use-mat.md).

## <a name="what-is-microsoft-translator"></a>¿Qué es Microsoft Tanslator?

Microsoft Translator es un servicio basado en la nube que proporciona traducciones automáticas. La traducción automática es ideal para tener acceso a traducciones cuando no se pueden obtener traducciones realizadas por humanos. Puedes obtener más información en [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220).

El kit de herramientas usa el servicio Microsoft Translator para proporcionarte sugerencias de traducción. Puedes ver los idiomas que admite Microsoft Translator cuando el icono de Microsoft Translator aparece en el diálogo Idiomas de traducción.

Puedes traducir tu aplicación rápidamente con Microsoft Translator en el editor multilingüe seleccionando una cadena y haciendo clic en el botón **Traducir**.

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>¿Qué es pseudoidioma y cuáles son los seguidores de pseudorecursos?

Pseudoidioma es una modificación artificial del producto de software que pretende simular la localización del idioma real. Puedes encontrar más detalles sobre pseudoidiomas y seguimiento de pseudorecursos en [Usar el kit de herramientas para aplicaciones multilingües 4.0](use-mat.md).

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>¿Cómo establezco el idioma de preferencia para pseudoidioma, de manera que pueda probar mis cadenas pseudolocalizadas?

Esto se explica en detalle en [Usar el kit de herramientas para aplicaciones multilingües 4.0](use-mat.md).

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>¿Qué tipo de problemas de localización puedo tener al usar pseudoidioma?

Esto se explica en detalle en [Usar el kit de herramientas para aplicaciones multilingües 4.0](use-mat.md).

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>Cuando inicio la aplicación, no veo ninguna traducción o la aplicación está traducida solo parcialmente

Abre el archivo .xlf en el editor multilingüe para ver si están las traducciones. Cuando cambian las cadenas en el archivo .resw del idioma predeterminado explícitamente, se quita cualquier traducción de los archivos .xlf. Esto se hace para garantizar que las traducciones concuerdan con sus cadenas de origen. Traducir las cadenas de los archivos .xlf y volver a compilar para actualizar los archivos .resw de idioma no predeterminado.

Si las cadenas están traducidas en los archivos .xlf, pero no aparecen en la aplicación, recompila el proyecto para actualizar los archivos .resw de idioma no predeterminado. Visual Studio optimiza el comando Compilar de modo que compile solamente los archivos que se modificaron desde la última vez que se usó Compilar.

Revisa el orden de preferencias de idioma. Asegúrate de que el idioma que quieres probar está el primero en la lista de preferencias de idiomas en **Configuración**.

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>El kit de herramientas informa de un error 0x80004004 en el resultado de la compilación

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

Este mensaje puede mostrarse cuando el formato de región entra en conflicto con la operación de compilación del kit de herramientas. La solución alternativa es cambiar el código de región a en-US durante la compilación en **Configuración**.


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>El kit de herramientas informa de un error 0x80004005 en el resultado de la compilación

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

Este mensaje puede mostrarse cuando el archivo .xlf contiene un idioma de destino no admitido. Por ejemplo, "zh-cht" es incorrecto (cámbialo a "zh-hant") y "zh-chs" es incorrecto (cámbialo a "zh-hans").

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>¿Hay alguna forma de obtener más información sobre los errores que veo?

Sí, puedes activar el registro detallado en Visual Studio. Haz clic en **Herramientas** > **Opciones** > **Proyectos y soluciones** > **Compilar y ejecutar**. Cambia **Contenido de los resultados de compilación del proyecto de MSBuild** de Mínimo a Normal o una opción superior.

Ejecutar MSBuild desde la línea de comandos también puede generar mensajes adicionales.

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>Error en la importación de la traducción

El proceso de importación realiza una validación básica antes de la importación. Esto garantiza que la información de referencia cultural de destino en los archivos que se están importando coincida con la de los archivos .xlf existentes. Abre el archivo .xlf en el editor multilingüe y asegúrate de que la información de referencia cultural coincide.

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>¿Qué sucede si el traductor no tiene instalado Windows10, Visual Studio o el kit de herramientas para aplicaciones multilingües?

Al seleccionar **Salida: destinatario de correo** en el cuadro de diálogo Exportar recursos de cadena, el correo electrónico incluirá un vínculo para descargar e instalar el kit de herramientas para aplicaciones multilingües (MAT) 4.0. El traductor puede instalar la herramienta independiente Editor multilingüe de MAT 4.0 incluso sin Windows 10 ni Visual Studio.

Para obtener más detalles, consulta [Cómo usar el kit de herramientas para aplicaciones multilingües 4.0](use-mat.md).

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>¿Qué ha pasado con los archivos `MarkupRules.xml` y `ResourcesLocks.xml`?

El kit de herramientas para aplicaciones multilingües 4.0 no usa archivos de bloqueo de recursos propietarios. En su lugar, se agrega la etiqueta de XLIFF 1.2 `<mrk>` directamente en el archivo .xlf para identificar las cadenas que no se modifican durante la traducción automática. Esto permite que el archivo XLIFF esté autocontenido, así como el bloqueo de recursos por archivos individuales.

Estos archivos de compatibilidad adicionales ya no son necesarios y puedes eliminarlos de manera segura si los tienes.

## <a name="what-happened-to-the-tpx-file"></a>¿Qué ha pasado con el archivo .tpx?

El archivo .tpx proporcionaba una manera sencilla de incluir los archivos `MarkupRules.xml` y `ResourcesLocks.xml` al enviar el archivo .xlf para su traducción. Esta función ya no es necesaria.

Si tienes traducciones en un archivo .tpx que quieres recuperar, tan solo cambia el nombre de la extensión del archivo .tpx a .zip. Esto permitirá que abras el contenido y lo extraigas con el Explorador de archivos o con cualquier herramienta compatible con .zip.

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>Creo que lo he hecho todo bien, pero sigue sin funcionar

Intenta hacerlo siguiendo estos pasos.

1. Agrega las traducciones con uno de los métodos ya descritos.
2. Vuelca el archivo .pri (consulta [Opciones de la línea de comandos de MakePRI.exe](../../app-resources/makepri-exe-command-options.md)) para ver si las traducciones están en el archivo .pri. Las traducciones aparecerán con código de idioma y valor traducido así.
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. Compila desde el símbolo del sistema; el error que se genera puede contener más detalles que el resultado de la compilación.

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>Mi aplicación no ha obtenido la certificación de Microsoft Store

Antes de iniciar el proceso de certificación de Microsoft Store, debes excluir el archivo `<project-name>.qps-ploc.xlf` del proyecto. Pseudoidioma se usa para detectar posibles problemas o errores de localización, pero no es un idioma válido para Microsoft Store. Si no se quita, la aplicación producirá un error durante el proceso de certificación de Microsoft Store.

## <a name="related-topics"></a>Temas relacionados

* [Usar el Kit de herramientas para aplicaciones multilingües 4.0](use-mat.md)
* [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)
* [Opciones de línea de comandos de MakePri.exe](../../app-resources/makepri-exe-command-options.md)
