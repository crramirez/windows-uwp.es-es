---
description: Si la aplicación no tiene recursos que coincidan con la configuración concreta de un dispositivo de cliente, se usan los recursos predeterminados de la aplicación. En este tema se explica cómo especificar cuáles son esos recursos predeterminados.
title: Especificar los recursos predeterminados que la aplicación usa
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 4db2fbce788bac38a0f3a54a108f91c8293de6ba
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031558"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>Especificar los recursos predeterminados que la aplicación usa

Si la aplicación no tiene recursos que coincidan con la configuración concreta de un dispositivo de cliente, se usan los recursos predeterminados de la aplicación. En este tema se explica cómo especificar cuáles son esos recursos predeterminados.

Cuando un cliente instala la aplicación desde el Microsoft Store, la configuración en el dispositivo del cliente se compara con los recursos disponibles de la aplicación. Esta búsqueda de coincidencias se realiza para que solo sea necesario descargar e instalar los recursos adecuados para ese usuario. Por ejemplo, se usan las cadenas e imágenes más adecuadas para las preferencias de idioma del usuario, así como la resolución del dispositivo y la configuración de PPP. Por ejemplo, `200` es el valor predeterminado para `scale` , pero puede invalidar ese valor predeterminado si lo desea.

Incluso en el caso de los recursos que no entran en sus propios paquetes de recursos (como imágenes adaptadas para la configuración de contraste alto), puede especificar los recursos predeterminados que la aplicación debe usar en tiempo de ejecución si no se encuentra un recurso que coincida con la configuración del usuario. Por ejemplo, `standard` es el valor predeterminado para `contrast` , pero puede invalidar ese valor predeterminado si lo desea.

Estos valores predeterminados se especifican en forma de valores predeterminados del calificador de recursos. Para obtener una explicación de los calificadores de recursos, su uso y el propósito, consulte [adaptar los recursos para el idioma, la escala, el contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md).

Puede configurar los valores predeterminados de una de estas dos maneras. Puede Agregar un archivo de configuración al proyecto o puede editar el archivo de proyecto directamente. Use cualquiera de estas opciones con las que esté más familiarizado, o lo que mejor se adapte a su sistema de compilación.

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>Opción 1. Usar priconfig.default.xml para especificar los valores predeterminados del calificador

1. En Visual Studio, agregue un nuevo elemento al proyecto. Elija archivo XML y asigne al archivo el nombre `priconfig.default.xml` .
2. En Explorador de soluciones, seleccione `priconfig.default.xml` y active la ventana Propiedades. La acción de compilación del archivo debe establecerse en ninguno y el directorio de resultados debe estar establecido en no copiar.
3. Reemplace el contenido del archivo por este código XML.
   ```xml
   <default>
      <qualifier name="Language" value="LANGUAGE-TAG(S)" />
      <qualifier name="Contrast" value="standard" />
      <qualifier name="Scale" value="200" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
   
   **Nota:** El valor `LANGUAGE-TAG(S)` debe sincronizarse con el idioma predeterminado de la aplicación. Si se trata de una sola [etiqueta de lenguaje BCP-47](https://tools.ietf.org/html/bcp47), el idioma predeterminado de la aplicación debe ser la misma etiqueta. Si se trata de una lista separada por comas de etiquetas de idioma, el idioma predeterminado de la aplicación debe ser la primera etiqueta de la lista. El idioma predeterminado de la aplicación se establece en el campo **idioma predeterminado** en la pestaña **aplicación** del archivo de origen del manifiesto del paquete de la aplicación ( `Package.appxmanifest` ).

4. Cada `<qualifier>` elemento indica a Visual Studio qué valor se va a usar como valor predeterminado para cada nombre de calificador. Con el contenido del archivo que tiene hasta ahora, no ha cambiado el comportamiento de Visual Studio. En otras palabras, Visual Studio *ya se comportaba como si* este archivo estuviese presente con estos contenidos, ya que estos son los valores predeterminados predeterminados. Por lo tanto, para invalidar un valor predeterminado con su propio valor predeterminado, tendrá que cambiar un valor en el archivo. Este es un ejemplo de cómo puede ser el archivo si editó los tres primeros valores.
   ```xml
   <default>
      <qualifier name="Language" value="de-DE" />
      <qualifier name="Contrast" value="black" />
      <qualifier name="Scale" value="400" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
5. Guarde y cierre el archivo y recompile el proyecto.

Para confirmar que se están teniendo en cuenta los valores predeterminados invalidados, busque el archivo `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` y confirme que su contenido coincide con las invalidaciones. Si lo hacen, habrá configurado correctamente los valores de calificador de los recursos que la aplicación usará de forma predeterminada. Si no se encuentra ninguna coincidencia para la configuración del usuario, se usarán los recursos cuyos nombres de carpeta o archivo contengan los valores predeterminados de qualifer que ha establecido aquí.

### <a name="how-does-this-work"></a>¿Cómo funciona?

En segundo plano, Visual Studio inicia una herramienta denominada `MakePri.exe` para generar un archivo conocido como un índice de recursos del paquete (PRI), que describe todos los recursos de la aplicación, incluido el que indica cuáles son los recursos predeterminados. Para obtener más información acerca de esta herramienta, consulte [compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio pasa un archivo de configuración a `MakePri.exe` . El contenido del `priconfig.default.xml` archivo se utiliza como el `<default>` elemento de ese archivo de configuración, que es la parte que especifica el conjunto de valores de calificador que se consideran predeterminados. Por lo tanto, agregar y editar `priconfig.default.xml` influye en última instancia en el contenido del archivo de índice de recursos del paquete que Visual Studio genera para la aplicación e incluye en su paquete de la aplicación.

**Nota:** Cada vez que cambie el valor del `<qualifier name="Language" ... />` elemento, deberá sincronizar ese cambio con el idioma predeterminado de la aplicación. Esto es para que los recursos de idioma indexados en el archivo PRI de la aplicación coincidan con el idioma predeterminado del manifiesto de la aplicación. El valor del `<qualifier name="Language" ... />` elemento invalida el valor del manifiesto con respecto al contenido de `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` , pero el archivo y el manifiesto de la aplicación deben coincidir.

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>Con un nombre de archivo diferente que `priconfig.default.xml`

Si asigna un nombre al archivo `priconfig.default.xml` , Visual Studio lo reconocerá y lo usará automáticamente. Si le asigna un nombre diferente, deberá dejar que Visual Studio lo sepa. En el archivo de proyecto, entre las etiquetas de apertura y cierre del primer `<PropertyGroup>` elemento, agregue este XML.

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

Reemplace `FILE-PATH-AND-NAME` por la ruta de acceso y el nombre del archivo.

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>Opción 2. Usar el archivo de proyecto para especificar los valores de calificador predeterminados

Esta es una alternativa a la opción 1. Una vez que comprenda cómo funciona la opción 1, puede elegir la opción 2 en su lugar, si esto se adapta mejor a su flujo de trabajo de desarrollo o de compilación.

En el archivo de proyecto, entre las etiquetas de apertura y cierre del primer `<PropertyGroup>` elemento, agregue este XML.

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Este es un ejemplo de cómo podría parecer después de haber editado los tres primeros valores.

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Guarde y cierre el proyecto y vuelva a compilarlo.

**Nota:** Cada vez que cambie el `Language=` valor, deberá sincronizar ese cambio con el idioma predeterminado de la aplicación en el diseñador de manifiestos (abriendo `Package.appxmanifest` ).

## <a name="related-topics"></a>Temas relacionados

* [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](tailor-resources-lang-scale-contrast.md)
* [Etiqueta de idioma de BCP-47](https://tools.ietf.org/html/bcp47)
* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)
