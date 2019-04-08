---
Description: Algunos tipos de aplicaciones (diccionarios multilingües, herramientas de traducción, etc.) necesitan reemplazar el comportamiento predeterminado de una recopilación de aplicación y crear recursos en el lote de aplicaciones en lugar de tenerlos en paquetes de recursos independientes. En este tema se explica cómo hacerlo.
title: Crear recursos en el paquete de aplicación, en lugar de en un paquete de recursos
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: 8bf2d34bc3dae20750f66c9116499a17444b798c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627290"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>Crear recursos en el paquete de aplicación, en lugar de en un paquete de recursos

Algunos tipos de aplicaciones (diccionarios multilingües, herramientas de traducción, etc.) necesitan reemplazar el comportamiento predeterminado de un lote de aplicaciones y crear recursos en el paquete de la aplicación en lugar de tenerlos en paquetes de recursos independientes. En este tema se explica cómo hacerlo.

De manera predeterminada, al compilar una [lote de aplicaciones (.appxbundle)](../packaging/packaging-uwp-apps.md), solo tus recursos predeterminados para idioma, escala y nivel de característica de DirectX están integrados en el paquete de la aplicación. Los recursos traducidos, y los recursos adaptados para escalas no predeterminadas y/o niveles de características DirectX, están integrados en paquetes de recursos y solo se descargan en los dispositivos que necesitan. Si un cliente compra tu aplicación en la Microsoft Store usando un dispositivo con una preferencia de idioma establecida en español, solo se descargará e instalará tu aplicación además del paquete de recursos de español. Si ese mismo usuario más adelante cambia su preferencia de idioma a francés en **Configuración**, se descargará e instalará el paquete de recursos en francés de tu aplicación. Suceden cosas similares con tus recursos cualificados para escala y para nivel de característica de DirectX. Para la mayoría de las aplicaciones, este comportamiento constituye una valiosa eficiencia y es exactamente lo que el cliente *quieres* y tú quieren que suceda.

Sin embargo, si la aplicación permite al usuario cambiar el idioma sobre la marcha desde dentro de la aplicación (en lugar de a través de **Configuración**), ese comportamiento predeterminado no es apropiado. Realmente quieres todos los recursos de idioma se descarguen e instalen incondicionalmente junto con la aplicación una vez y, a continuación, permanezcan en el dispositivo. Quieres crear todos esos recursos en el paquete de la aplicación en lugar de en los paquetes de recursos independientes.

**Nota** Al incluir recursos en un paquete de la aplicación básicamente se aumenta el tamaño de la aplicación. Ese es el motivo por el que solo vale la pena hacerlo si la naturaleza de la aplicación lo exige. De lo contrario, a continuación, no tienes que hacer nada excepto compilar un lote de aplicaciones normal como de costumbre.

Puedes configurar Visual Studio para compilar recursos en el paquete de la aplicación de una de dos maneras posibles. Puedes agregar un archivo de configuración al proyecto o bien, puedes editar el archivo de proyecto directamente. Usa cualquiera de estas opciones con las que te sientas más cómodo o cualquiera que funcione mejor con tu sistema de compilación.

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>Opción 1. Usar priconfig.packaging.xml para compilar recursos en el paquete de la aplicación

1. En Visual Studio, agrega un elemento nuevo a tu proyecto. Elige Archivo XML y asígnale el nombre `priconfig.packaging.xml`.
2. En el Explorador de soluciones, selecciona `priconfig.packaging.xml` y comprueba la ventana Propiedades. La acción de compilación del archivo debería establecerse en Ninguna y Copiar en el directorio de salida debería establecerse en No copiar.
3. Reemplaza el contenido del archivo con este XML.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. Cada elemento `<autoResourcePackage>` indica a Visual Studio que divida los recursos automáticamente para el nombre de calificador determinado en paquetes de recursos independientes. Esto se denomina *división automática*. Con el contenido del archivo que tiene hasta el momento, realmente no ha cambiado el comportamiento de Visual Studio. Es decir, Visual Studio *ya se comportó como si* este archivo estuviera presente con este contenido, ya que estos son los valores predeterminados. Si no deseas que Visual Studio divida automáticamente en un nombre de calificador, elimina ese elemento `<autoResourcePackage>` del archivo. Este es el aspecto que tendría el archivo si quisieras que todos tus recursos de idiomas se integraran en el paquete de la aplicación en lugar de dividirse automáticamente en paquetes de recursos independientes.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. Guarda y cierra el archivo y recompila el proyecto.

Para confirmar que se están teniendo en cuenta tus opciones de división automática, busca el archivo `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml` y confirma que su contenido coincide con tus opciones. De ser así, has configurado Visual Studio correctamente para compilar los recursos que elijas en el paquete de la aplicación.

Hay un paso final que debes hacer. **Pero solo si has eliminado el nombre de calificador `Language`**. Debes especificar la unión de todos los idiomas admitidos de tu aplicación como el valor predeterminado de la aplicación para el idioma. Para obtener detalles, consulta [Especificar los recursos predeterminados que la aplicación usa](specify-default-resources-installed.md). Esto es lo que tu `priconfig.default.xml` contendría si estuvieras incluyendo recursos para inglés, español y francés en el paquete de la aplicación.

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>¿Cómo funciona esto?

En segundo plano, Visual Studio inicia una herramienta denominada `MakePri.exe` para generar un archivo conocido como un índice de recursos del paquete, que describe todos los recursos de la aplicación, incluyendo la indicación de en qué nombres de calificador de recursos dividir automáticamente. Para obtener más información acerca de esta herramienta, consulta [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio pasa un archivo de configuración a `MakePri.exe`. El contenido de tu archivo `priconfig.packaging.xml` se usa como el elemento `<packaging>` de ese archivo de configuración, que es la parte que determina la división automática. Por lo tanto, agregar y editar `priconfig.packaging.xml` en última instancia influye en el contenido del archivo de índice de recursos del paquete que Visual Studio genera para tu aplicación, así como el contenido de los paquetes de tu lote de aplicaciones.

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>Usar un nombre diferente a `priconfig.packaging.xml`

Si a tu archivo le asignas el nombre `priconfig.packaging.xml`, Visual Studio lo reconocerá y lo usará automáticamente. Si le asignas un nombre diferente, deberás informar a Visual Studio. En tu archivo de proyecto, entre las etiquetas de apertura y cierre del primer elemento `<PropertyGroup>`, agrega este XML.

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

Reemplaza `FILE-PATH-AND-NAME` por la ruta de acceso al archivo y el nombre del archivo.

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>Opción 2. Usar tu archivo de proyecto para compilar recursos en el paquete de la aplicación

Esta es una alternativa a la Opción 1. Una vez que comprendas cómo funciona la Opción 1, puedes elegir hacer la Opción 2 en su lugar, si es adecuada para tu desarrollo o compilar un mejor flujo de trabajo.

En tu archivo de proyecto, entre las etiquetas de apertura y cierre del primer elemento `<PropertyGroup>`, agrega este XML.

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Este es el aspecto que tiene después de eliminar el primer nombre de calificador.

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Guarda, cierra y recompila el proyecto.

Hay un paso final que debes hacer. **Pero solo si has eliminado el nombre de calificador `Language`**. Debes especificar la unión de todos los idiomas admitidos de tu aplicación como el valor predeterminado de la aplicación para el idioma. Para obtener detalles, consulta [Especificar los recursos predeterminados que la aplicación usa](specify-default-resources-installed.md). Esto es lo que tu archivo de proyecto contendría si estuvieras incluyendo recursos para inglés, español y francés en el paquete de la aplicación.

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>Temas relacionados

* [Empaquetar una aplicación para UWP con Visual Studio](../packaging/packaging-uwp-apps.md)
* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)
* [Especificar los recursos predeterminados que la aplicación usa](specify-default-resources-installed.md)