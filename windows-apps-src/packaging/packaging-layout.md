---
author: laurenhughes
title: Creación del paquete con el diseño del empaquetado
description: El diseño del empaquetado es un solo documento que describe la estructura del empaquetado de la aplicación. Especifica los lotes de una aplicación (principal y opcional), los paquetes de los lotes y los archivos de los paquetes.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, packaging, empaquetado, package layout, distribución de paquete, asset package, paquete de activos
ms.localizationpriority: medium
ms.openlocfilehash: 3f8cbb3989b58b726336b4bd757902bd9ea3f8c0
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4685917"
---
# <a name="package-creation-with-the-packaging-layout"></a>Creación del paquete con el diseño del empaquetado  

Con la introducción de paquetes de activos, los desarrolladores ahora tienen las herramientas para crear más paquetes además de más tipos. Cuanto más grande y compleja es una aplicación, generalmente consta de más paquetes y aumenta la dificultad de administrarlos (especialmente si se crean fuera de Visual Studio y usando archivos de asignación). Para simplificar la administración de la estructura de empaquetado de una aplicación, puedes usar el diseño de empaquetado admitido por MakeAppx.exe. 

El diseño del empaquetado es un solo documento XML que describe la estructura del empaquetado de la aplicación. Especifica los lotes de una aplicación (principal y opcional), los paquetes de los lotes y los archivos de los paquetes. Los archivos se pueden seleccionar desde carpetas, unidades y ubicaciones de red diferentes. Pueden usarse caracteres comodín para seleccionar o excluir archivos.

Después de configurar el diseño de empaquetado de una aplicación, se usa con MakeAppx.exe para crear todos los paquetes de una aplicación en una sola llamada de línea de comandos. El diseño de empaquetado puede modificarse para alterar la estructura del paquete para satisfacer tus necesidades de implementación. 


## <a name="simple-packaging-layout-example"></a>Ejemplo de diseño de empaquetado simple

Este es un ejemplo del aspecto de un diseño de empaquetado simple:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>
    
    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>

  </PackageFamily>
</PackagingLayout>
```

Desglosemos este ejemplo para comprender cómo funciona.

### <a name="packagefamily"></a>PackageFamily
Este diseño de empaquetado creará un archivo de paquete de aplicación plana único con un x64 paquete de arquitectura y un paquete de activos "Media". 

El elemento **PackageFamily** se usa para definir una recopilación de aplicaciones. Debes usar el atributo **ManifestPath** para proporcionar un **AppxManifest** para la recopilación, el **AppxManifest** debe coincidir con el**AppxManifest** para el paquete de arquitectura de la recopilación. El atributo **ID** también debe proporcionarse. Esto se usa con MakeAppx.exe durante la creación del paquete para que puedas crear solo este paquete si quieres, y este será el nombre de archivo del paquete resultante. El atributo **FlatBundle** se usa para describir qué tipo de recopilación quieres crear, **true** para una recopilación plana (aquí puedes leer más sobre ella), y **false** para una recopilación clásica. El atributo **ResourceManager** se usa para especificar si los paquetes de recursos dentro de esta recopilación usarán MRT para acceder a los archivos. De manera predeterminada es **true**, pero a partir de Windows 10, versión 1803, este aún no está listo, por lo que este atributo debe establecerse en **false**.


### <a name="package-and-assetpackage"></a>Package y AssetPackage
Dentro de **PackageFamily**, se definen los paquetes que contiene o a los que hace referencia. Aquí, el paquete de la arquitectura (también llamado paquete principal) se define con el elemento **Package** y el paquete de activos se define con el elemento **AssetPackage**. El paquete de arquitectura debe especificar para qué arquitectura es el paquete, "x64", "x86", "arm" o "neutral". También puedes (opcionalmente) proporcionar directamente un **AppxManifest** específicamente para este paquete usando de nuevo el atributo **ManifestPath**. Si una **AppxManifest** no siempre se generará automáticamente desde la **AppxManifest** proporcionado para la **PackageFamily **. 

De manera predeterminada y **AppxManifest** se generará para cada paquete dentro de la recopilación. Para el paquete de activos, también puedes establecer el atributo **AllowExecution**. Si se establece en **false** (predeterminado), te ayudará a reducir el tiempo de publicación de tu aplicación ya que los paquetes que no tienen que ejecutarse no tendrán su detección de virus bloqueando el proceso de publicación (puedes aprender más sobre esto en [Introducción a paquetes de activos](asset-packages.md)). 

### <a name="files"></a>Files
Dentro de la definición de cada paquete, puedes usar el elemento **File** para seleccionar archivos que se van a incluir en este paquete. El atributo **SourcePath** es donde están los archivos localmente. Puedes seleccionar archivos desde carpetas diferentes (proporcionando rutas de acceso relativas), unidades diferentes (proporcionando rutas de acceso absolutas) o incluso recursos compartidos de red (proporcionando algo como `\\myshare\myapp\*`). La **DestinationPath** es donde los archivos acabarán dentro del paquete, en relación con la raíz del paquete. **ExcludePath** puede utilizarse (en lugar de los otros dos atributos) para seleccionar archivos que se van a excluir de los seleccionados con otros atributos **SourcePath** de los elementos **File** dentro del mismo paquete.

Cada elemento **File** puede usarse para seleccionar varios archivos utilizando caracteres comodín. En general, un solo comodín (`*`) se puede usar en cualquier parte dentro de la ruta de acceso cualquier número de veces. Sin embargo, un solo carácter comodín solo coincidirá con los archivos dentro de una carpeta y no en las subcarpetas. Por ejemplo, `C:\MyGame\*\*` puede usarse en **SourcePath** para seleccionar los archivos `C:\MyGame\Audios\UI.mp3` y `C:\MyGame\Videos\intro.mp4`, pero no puede seleccionar `C:\MyGame\Audios\Level1\warp.mp3`. El comodín doble (`**`) también puede usarse en lugar de nombres de carpeta o archivo para que coincidan con cualquier cosa de forma recursiva (pero no puede ser junto a nombres parciales). Por ejemplo, `C:\MyGame\**\Level1\**` puede seleccionar `C:\MyGame\Audios\Level1\warp.mp3` y `C:\MyGame\Videos\Bonus\Level1\DLC1\intro.mp4`. También pueden usarse caracteres comodín para cambiar directamente los nombres de archivo como parte del proceso de empaquetado si se usan los caracteres comodín en lugares diferentes entre el origen y el destino. Por ejemplo, tener `C:\MyGame\Audios\*` para **SourcePath** y `Sound\copy_*` para **DestinationPath** puede seleccionar `C:\MyGame\Audios\UI.mp3` y hacer que aparezca en el paquete como `Sound\copy_UI.mp3`. En general, el número de caracteres comodín únicos y comodines doble debe ser el mismo para **SourcePath** y **DestinationPath** de un solo elemento **File**.


## <a name="advanced-packaging-layout-example"></a>Ejemplo de diseño de empaquetado avanzado

Este es un ejemplo de un diseño más complicado de empaquetado:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <!-- Main game -->
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>

    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>
    
    <!-- English resource package -->
    <ResourcePackage ID="en">
      <Files>
        <File DestinationPath="english\**" SourcePath="C:\mygame\english\**"/>
      </Files>
      <Resources Default="true">
        <Resource Language="en"/>
      </Resources>
    </ResourcePackage>

    <!-- French resource package -->
    <ResourcePackage ID="fr">
      <Files>
        <File DestinationPath="french\**" SourcePath="C:\mygame\french\**"/>
      </Files>
      <Resources>
        <Resource Language="fr"/>
      </Resources>
    </ResourcePackage>
  </PackageFamily>

  <!-- DLC in the related set -->
  <PackageFamily ID="DLC" Optional="true" ManifestPath="C:\DLC\appxmanifest.xml">
    <Package ID="DLC.x86" Architecture="x86">
      <Files>
        <File DestinationPath="**" SourcePath="C:\DLC\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- DLC not part of the related set -->
  <PackageFamily ID="Themes" Optional="true" RelatedSet="false" ManifestPath="C:\themes\appxmanifest.xml">
    <Package ID="Themes.main" Architecture="neutral">
      <Files>
        <File DestinationPath="**" SourcePath="C:\themes\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- Existing packages that need to be included/referenced in the bundle -->
  <PrebuiltPackage Path="C:\prebuilt\DLC2.appxbundle" />

</PackagingLayout>
```

Este ejemplo difiere del ejemplo simple con la adición de los elementos **ResourcePackage** y **Optional**.

Los paquetes de recursos se pueden especificar con el elemento **ResourcePackage**. Dentro del **ResourcePackage**, el elemento **Resources** debe usarse para especificar los calificadores de recursos del paquete de recursos. Los calificadores de recursos son los recursos que son compatibles con el paquete de recursos, aquí, podemos ver que hay dos paquetes de recursos definidos y pueden contener los archivos específicos de inglés y francés. Un paquete de recursos puede tener más de un calificador, esto puede hacerse agregando otro elemento **Resource** dentro de **Resources**. También se debe especificar un recurso predeterminado para la dimensión de recurso si la dimensión existe (dimensiones que son idioma, escala, dxfl). Aquí, podemos ver que el inglés es el idioma predeterminado, lo que significa que para los usuarios que no tienen un idioma del sistema definido en francés, se reservarán para descargar el paquete de recursos de inglés y mostrar en inglés.


Cada paquete opcional tiene su propio nombre de familia de paquete distinto y se debe definir con elementos **PackageFamily**, mientras se especifica el atributo **Optional** para que sea **true **. El atributo **RelatedSet** se usa para especificar si el paquete opcional está dentro del conjunto relacionado (por defecto es true): si el paquete opcional debe actualizarse con el paquete primario.

El elemento **PrebuiltPackage** se usa para agregar paquetes que no están definidos en el diseño de empaquetado para incluirse o referenciarse en los archivos de paquete de aplicación que se van a. En este caso, otro paquete opcional de DLC se incluye aquí para que pueda hacer referencia a él y tenerlo a formar parte del conjunto relacionado del archivo de recopilación de aplicación principal.


## <a name="build-app-packages-with-a-packaging-layout-and-makeappxexe"></a>Compilar paquetes de aplicación con un diseño de empaquetado y MakeAppx.exe
Una vez que tengas el diseño de empaquetado de tu aplicación, puedes empezar a usar MakeAppx.exe para compilar los paquetes de tu aplicación. Para compilar todos los paquetes definidos en el diseño de empaquetado, usa el comando:

``` example 
MakeAppx.exe build /f PackagingLayout.xml /op OutputPackages\
```

Pero, si vas a actualizar tu aplicación y algunos paquetes no contienen ningún archivo modificado, puedes compilar solo los paquetes que has cambiado. Usando el ejemplo de diseño de empaquetado sencillo de esta página y compilando el paquete de la arquitectura x64, así es cómo aparecería nuestro comando:

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "x64" /ip PreviousVersion\ /op OutputPackages\ /iv
```

La marca `/id` puede usarse para seleccionar los paquetes que se van a compilar a partir del diseño de empaquetado, correspondiente al atributo **ID** en el diseño. La `/ip` se usa para indicar que la versión anterior de los paquetes se encuentran en este caso. Debe proporcionarse la versión anterior porque el archivo de paquete de la aplicación debe hacer referencia a la versión anterior del paquete de **contenido multimedia** . La marca `/iv` se usa para incrementar automáticamente la versión de los paquetes que se están compilando (en lugar de cambiar la versión en el **AppxManifest**). Como alternativa, los modificadores `/pv` y `/bv` pueden usarse para proporcionar directamente una versión del paquete (para todos los paquetes que se van a crear) y una versión de recopilación (para todas las recopilaciones que se van a crear), respectivamente.
Con el ejemplo de diseño de empaquetado avanzado en esta página, si quieres compilar la recopilación opcional **Themes** y el paquete de aplicación **Themes.main** al que se hace referencia, utilizarías este comando:

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "Themes" /op OutputPackages\ /bc /nbp
```

La marca `/bc` se usa para indicar que los elementos secundarios de la recopilación **Themes** también se deben compilar (en este caso se compilará **Themes.main**). La marca `/nbp` se usa para indicar que el elemento principal de la recopilación **Themes** no se debe compilar. El elemento principal de **Themes**, que es una recopilación de aplicación opcional, es la recopilación de aplicación principal: **MyGame**. Por lo general para un paquete opcional en un conjunto relacionado, la recopilación de aplicación debe también compilarse para que el paquete opcional sea instalable, ya que al paquete opcional también se hace referencia en la recopilación de aplicación principal cuando se encuentra en un conjunto relacionado (para garantizar el control de versiones entre los paquetes principal y opcionales). La relación entre principal y secundario entre paquetes se ilustra en el diagrama siguiente:

![Diagrama de diseño de empaquetado](images/packaging-layout.png)
