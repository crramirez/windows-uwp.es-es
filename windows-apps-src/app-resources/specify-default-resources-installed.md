---
author: stevewhims
Description: If your app doesn't have resources that match the particular settings of a customer device, then the app's default resources are used. This topic explains how to specify what those default resources are.
title: Especificar los recursos predeterminados que la aplicación usa
template: detail.hbs
ms.author: stwhi
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: daa40656c72812e19c7f6f5fa71e50c2206670af
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6985834"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>Especificar los recursos predeterminados que la aplicación usa

Si la aplicación no tiene recursos que coincidan con la configuración concreta de un dispositivo de cliente, se usan recursos predeterminados de la aplicación. Este tema explica cómo especificar cuáles son esos recursos predeterminados.

Cuando un usuario instala tu aplicación desde la Microsoft Store, la configuración del dispositivo del cliente se compara con los recursos disponibles de la aplicación. Esta coincidencia se realiza solo de manera que los recursos adecuados tengan que descargarse e instalarse para ese usuario. Por ejemplo, se usan las cadenas e imágenes más adecuadas para las preferencias de idioma del usuario, además de la configuración de PPP y la resolución del dispositivo. Por ejemplo, `200` es el valor predeterminado de `scale`, pero puedes invalidar ese valor predeterminado si lo deseas.

Incluso para los recursos que entran en sus propios paquetes de recursos (como las imágenes adaptadas para la configuración de contraste alto), puedes especificar qué recursos predeterminados debe usar la aplicación en tiempo de ejecución si no se encuentra un recurso que coincida con la configuración del usuario. Por ejemplo, `standard` es el valor predeterminado de `contrast`, pero puedes invalidar ese valor predeterminado si lo deseas.

Estos valores predeterminados se especifican en forma de valores de calificador de recursos predeterminados. Para ver una explicación de qué son los calificadores de recursos, su uso y finalidad, consulta [Adaptar los recursos al idioma, escala, contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md).

Puedes configurar qué son estos valores predeterminados de una de dos maneras diferentes. Puedes agregar un archivo de configuración al proyecto o bien, puedes editar el archivo de proyecto directamente. Usa cualquiera de estas opciones con las que te sientas más cómodo o cualquiera que funcione mejor con tu sistema de compilación.

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>Opción 1. Usar priconfig.default.xml para especificar valores de calificador predeterminados

1. En Visual Studio, agrega un elemento nuevo a tu proyecto. Elige Archivo XML y asígnale el nombre `priconfig.default.xml`.
2. En el Explorador de soluciones, selecciona `priconfig.default.xml` y comprueba la ventana Propiedades. La acción de compilación del archivo debería establecerse en Ninguna y Copiar en el directorio de salida debería establecerse en No copiar.
3. Reemplaza el contenido del archivo con este XML.
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
   
   **Note** El valor `LANGUAGE-TAG(S)` debe estar sincronizado con el idioma predeterminado de la aplicación. Si es una única [etiqueta de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302), el idioma predeterminado de la aplicación debe ser la misma etiqueta. Si es una lista separada por comas de etiquetas de idioma, el idioma predeterminado de la aplicación debe ser la primera etiqueta de la lista. Establece el idioma predeterminado de la aplicación en el campo **Idioma predeterminado** de la pestaña **Aplicación** del archivo de origen del manifiesto del paquete de la aplicación (`Package.appxmanifest`).

4. Cada elemento `<qualifier>` indica a Visual Studio qué valor usar como el valor predeterminado para cada nombre de calificador. Con el contenido del archivo que tiene hasta el momento, realmente no ha cambiado el comportamiento de Visual Studio. Es decir, Visual Studio *ya se comportó como si* este archivo estuviera presentes con este contenido, ya que estos son los valores predeterminados. Por lo tanto, para invalidar un valor predeterminado con tu propio valor predeterminado, tendrás que cambiar un valor en el archivo. Este es un ejemplo de qué aspecto podría tener el archivo si editara los tres primeros valores.
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
5. Guarda y cierra el archivo y recompila el proyecto.

Para confirmar que se están teniendo en cuenta tus valores predeterminados invalidados, busca el archivo `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` y confirmar que su contenido coincide con tus invalidaciones. Si es así, has configurado correctamente los valores de calificador de los recursos que tu aplicación usará de manera predeterminada. Si no se encuentra una coincidencia para la configuración del usuario, se usarán los recursos cuyos nombres de archivo o carpeta contengan los valores de calificador predeterminados que has establecido aquí.

### <a name="how-does-this-work"></a>¿Cómo funciona esto?

En segundo plano, Visual Studio inicia una herramienta denominada `MakePri.exe` para generar un archivo conocido como un índice de recursos del paquete (PRI), que describe todos los recursos de la aplicación, incluyendo la indicación de cuáles son los recursos predeterminados. Para obtener más información acerca de esta herramienta, consulta [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio pasa un archivo de configuración a `MakePri.exe`. El contenido de tu archivo `priconfig.default.xml` se usa como el elemento `<default>` de ese archivo de configuración, que es la parte que especifica el conjunto de valores de calificador que se consideran predeterminados. Por lo tanto, agregar y editar `priconfig.default.xml` en última instancia influye en el contenido del archivo de índice de recursos del paquete que Visual Studio genera para tu aplicación e incluye en su paquete de la aplicación.

**Nota** En cualquier momento que cambies el valor del elemento `<qualifier name="Language" ... />`, debes sincronizar ese cambio con el idioma predeterminado de la aplicación. Esto es para que los recursos de idioma indexados en el archivo PRI de tu aplicación coincidan con el idioma predeterminado del manifiesto de la aplicación. El valor del elemento `<qualifier name="Language" ... />` invalida el valor del manifiesto con respecto al contenido de `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml`, pero ese archivo y el manifiesto de la aplicación deben coincidir.

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>Usar un nombre diferente a `priconfig.default.xml`

Si a tu archivo le asignas el nombre `priconfig.default.xml`, Visual Studio lo reconocerá y lo usará automáticamente. Si le asignas un nombre diferente, deberás informar a Visual Studio. En tu archivo de proyecto, entre las etiquetas de apertura y cierre del primer elemento `<PropertyGroup>`, agrega este XML.

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

Reemplaza `FILE-PATH-AND-NAME` por la ruta de acceso al archivo y el nombre del archivo.

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>Opción 2. Usar el archivo de proyecto para especificar valores de calificador predeterminados

Esta es una alternativa a la Opción 1. Una vez que comprendas cómo funciona la Opción 1, puedes elegir hacer la Opción 2 en su lugar, si es adecuada para tu desarrollo o compilar un mejor flujo de trabajo.

En tu archivo de proyecto, entre las etiquetas de apertura y cierre del primer elemento `<PropertyGroup>`, agrega este XML.

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Este es un ejemplo de qué aspecto podría tener tras editar los tres primeros valores.

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Guarda, cierra y recompila el proyecto.

**Nota** En cualquier momento que cambies el valor de `Language=`, debes sincronizar ese cambio con el idioma predeterminado de la aplicación en el diseñador del manifiesto (abriendo `Package.appxmanifest`).

## <a name="related-topics"></a>Artículos relacionados

* [Adaptar los recursos al idioma, escala, contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md)
* [Etiqueta de idioma de BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)
