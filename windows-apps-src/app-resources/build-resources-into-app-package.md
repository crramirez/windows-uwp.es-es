---
description: Algunos tipos de aplicaciones (diccionarios multilingües, herramientas de traducción, etc.) necesitan reemplazar el comportamiento predeterminado de un lote de aplicaciones y crear recursos en el paquete de la aplicación en lugar de tenerlos en distintos paquetes de recursos. En este tema se explica cómo hacerlo.
title: Compilar recursos en el paquete de la aplicación
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: ccc5a5eb9ef6dfd5c0e9eda1cac6b4bf4441cf7d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031898"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>Crear recursos en el paquete de la aplicación, en lugar de en un paquete de recursos

Algunos tipos de aplicaciones (diccionarios multilingües, herramientas de traducción, etc.) necesitan invalidar el comportamiento predeterminado de un lote de aplicaciones y compilar recursos en el paquete de la aplicación en lugar de tenerlos en paquetes de recursos independientes (o paquetes de recursos). En este tema se explica cómo hacerlo.

De forma predeterminada, al compilar un [paquete de aplicaciones (. appxbundle)](/windows/msix/package/packaging-uwp-apps), solo los recursos predeterminados de idioma, escala y nivel de característica de DirectX están integrados en el paquete de la aplicación. Los recursos traducidos &mdash; y los recursos adaptados para las escalas y los niveles de características de DirectX no predeterminados &mdash; se integran en los paquetes de recursos y solo se descargan en los dispositivos que los necesiten. Si un cliente compra la aplicación desde el Microsoft Store mediante un dispositivo con una preferencia de idioma establecida en español, solo se descargará e instalará la aplicación más el paquete de recursos de español. Si más adelante el mismo usuario cambia su preferencia de idioma a francés en **configuración** , se descargará e instalará el paquete de recursos en francés de la aplicación. Se producen acciones similares con los recursos calificados para la escala y el nivel de características de DirectX. Para la mayoría de las aplicaciones, este comportamiento constituye una eficacia valiosa y es exactamente lo que usted y el cliente *quieren* que suceda.

Pero si la aplicación permite al usuario cambiar el idioma sobre la marcha desde dentro de la aplicación (en lugar de hacerlo a través de la **configuración** ), ese comportamiento predeterminado no es adecuado. Realmente desea que todos los recursos de idioma se descarguen e instalen de manera incondicional junto con la aplicación una vez y que permanezcan en el dispositivo. Desea compilar todos esos recursos en el paquete de la aplicación en lugar de en paquetes de recursos independientes.

**Nota:** La inclusión de los recursos en un paquete de aplicación aumenta esencialmente el tamaño de la aplicación. Por eso solo merece la pena hacer si la naturaleza de la aplicación lo exige. Si no es así, no es necesario hacer nada, salvo que se crea un lote de aplicaciones normal como de costumbre.

Puede configurar Visual Studio para compilar recursos en el paquete de la aplicación de una de estas dos maneras. Puede Agregar un archivo de configuración al proyecto o puede editar el archivo de proyecto directamente. Use cualquiera de estas opciones con las que esté más familiarizado, o lo que mejor se adapte a su sistema de compilación.

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>Opción 1. Usar priconfig.packaging.xml para compilar recursos en el paquete de la aplicación

1. En Visual Studio, agregue un nuevo elemento al proyecto. Elija archivo XML y asigne al archivo el nombre `priconfig.packaging.xml` .
2. En Explorador de soluciones, seleccione `priconfig.packaging.xml` y active la ventana Propiedades. La acción de compilación del archivo debe establecerse en ninguno y el directorio de resultados debe estar establecido en no copiar.
3. Reemplace el contenido del archivo por este código XML.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. Cada `<autoResourcePackage>` elemento indica a Visual Studio que divida automáticamente los recursos para el nombre del calificador dado en paquetes de recursos independientes. Esto se denomina *División automática* . Con el contenido del archivo que tiene hasta ahora, no ha cambiado el comportamiento de Visual Studio. En otras palabras, Visual Studio *ya se comportaba como si* este archivo estuviese presente con el contenido, ya que estos son los valores predeterminados. Si no desea que Visual Studio se divida automáticamente según un nombre de calificador, elimine `<autoResourcePackage>` el elemento del archivo. Este es el aspecto que tendrá el archivo si desea que todos los recursos de idioma se compilen en el paquete de la aplicación en lugar de dividirse automáticamente en paquetes de recursos independientes.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. Guarde y cierre el archivo y recompile el proyecto.

Para confirmar que se están teniendo en cuenta las opciones de División automática, busque el archivo `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml` y confirme que su contenido coincide con sus elecciones. Si lo hacen, habrá configurado correctamente Visual Studio para compilar los recursos de su elección en el paquete de la aplicación.

Hay un paso final que debe hacer. **Pero solo si eliminó el `Language` nombre del calificador** . Debe especificar la Unión de todo el idioma compatible de la aplicación como valor predeterminado de la aplicación para el idioma. Para obtener más información, consulte [especificación de los recursos predeterminados que usa la aplicación](specify-default-resources-installed.md). Esto es lo que `priconfig.default.xml` incluiría si estuviera incluyendo recursos para inglés, español y francés en el paquete de la aplicación.

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>¿Cómo funciona?

En segundo plano, Visual Studio inicia una herramienta denominada `MakePri.exe` para generar un archivo conocido como índice de recursos de paquete, que describe todos los recursos de la aplicación, incluido el que indica en qué nombres de calificador de recurso se divide automáticamente. Para obtener más información acerca de esta herramienta, consulte [compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio pasa un archivo de configuración a `MakePri.exe` . El contenido del `priconfig.packaging.xml` archivo se utiliza como el `<packaging>` elemento de ese archivo de configuración, que es la parte que determina la División automática. Por lo tanto, agregar y editar `priconfig.packaging.xml` influye en última instancia en el contenido del archivo de índice de recursos del paquete que Visual Studio genera para la aplicación, así como el contenido de los paquetes en el lote de aplicaciones.

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>Con un nombre de archivo diferente que `priconfig.packaging.xml`

Si asigna un nombre al archivo `priconfig.packaging.xml` , Visual Studio lo reconocerá y lo usará automáticamente. Si le asigna un nombre diferente, deberá dejar que Visual Studio lo sepa. En el archivo de proyecto, entre las etiquetas de apertura y cierre del primer `<PropertyGroup>` elemento, agregue este XML.

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

Reemplace `FILE-PATH-AND-NAME` por la ruta de acceso y el nombre del archivo.

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>Opción 2. Usar el archivo de proyecto para compilar recursos en el paquete de la aplicación

Esta es una alternativa a la opción 1. Una vez que comprenda cómo funciona la opción 1, puede elegir la opción 2 en su lugar, si esto se adapta mejor a su flujo de trabajo de desarrollo o de compilación.

En el archivo de proyecto, entre las etiquetas de apertura y cierre del primer `<PropertyGroup>` elemento, agregue este XML.

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Este es el aspecto que tendrá después de eliminar el primer nombre de calificador.

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Guarde y cierre el proyecto y vuelva a compilarlo.

Hay un paso final que debe hacer. **Pero solo si eliminó el `Language` nombre del calificador** . Debe especificar la Unión de todo el idioma compatible de la aplicación como valor predeterminado de la aplicación para el idioma. Para obtener más información, consulte [especificación de los recursos predeterminados que usa la aplicación](specify-default-resources-installed.md). Esto es lo que contendría el archivo del proyecto si estuviera incluyendo recursos para inglés, español y francés en el paquete de la aplicación.

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>Temas relacionados

* [Empaquetar una aplicación para UWP con Visual Studio](/windows/msix/package/packaging-uwp-apps)
* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)
* [Especificar los recursos predeterminados que la aplicación usa](specify-default-resources-installed.md)
