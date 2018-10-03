---
author: normesta
Description: You can use extensions to integrate your packaged desktop app with Windows 10 in predefined ways.
Search.Product: eADQiWindows 10XVcnh
title: Integrar la aplicación con Windows 10 (Puente de dispositivo de escritorio)
ms.author: normesta
ms.date: 04/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.localizationpriority: medium
ms.openlocfilehash: fadd9c2b6a35a1418a782ab0a6ef419e3f127f42
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "4311825"
---
# <a name="integrate-your-packaged-desktop-application-with-windows-10"></a>Integrar la aplicación de escritorio empaquetada con Windows 10

Usa extensiones para integrar la aplicación de escritorio empaquetada con Windows 10 mediante formas predefinidas.

Por ejemplo, usa una extensión para crear una excepción de firewall, hacer que la aplicación sea la aplicación predeterminada para un tipo de archivo o incluir iconos de inicio en la versión empaquetada de la aplicación. Para usar una extensión, solo tienes que agregar algunos archivos XML al archivo de manifiesto de paquete de la aplicación. No se requiere ningún tipo de código.

En este tema se describen estas extensiones y las tareas que puedes realizar al usarlas.

## <a name="transition-users-to-your-app"></a>Proceso de transición de usuarios a la aplicación

Ayuda a los usuarios a cambiar a la aplicación empaquetada.

* [Incluir los iconos de inicio y los botones de la barra de tareas existentes en la aplicación empaquetada](#point)
* [Hacer que la aplicación empaquetada abra archivos en lugar de la aplicación de escritorio](#make)
* [Asociar la aplicación empaquetada con un conjunto de tipos de archivo](#associate)
* [Agregar opciones en los menús contextuales de los archivos que tienen un tipo de archivo específico](#add)
* [Abrir determinados tipos de archivos mediante una dirección URL](#open)

<a id="point" />

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>Incluir los iconos de inicio y los botones de la barra de tareas existentes en la aplicación empaquetada

Es posible que los usuarios hayan anclado la aplicación de escritorio en la barra de tareas o el menú Inicio. Puedes incluir esos accesos directos en la nueva aplicación empaquetada.

#### <a name="xml-namespace"></a>Espacio de nombres XML

http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>

```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration).

|Nombre | Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.desktopAppMigration``
|AumID |Identificador de modelo de usuario de aplicación de la aplicación empaquetada. |
|ShortcutPath |Ruta de acceso a archivos .ink que inician la versión de escritorio de la aplicación. |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="rescap3">
  <Applications>
    <Application>
      <Extensions>
        <rescap3:Extension Category="windows.desktopAppMigration">
          <rescap3:DesktopAppMigration>
            <rescap3:DesktopApp AumId="[your_app_aumid]" />
            <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\[my_app_folder]\[my_app].lnk"/>
         </rescap3:DesktopAppMigration>
        </rescap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```
#### <a name="related-sample"></a>Muestra relacionada

[Visor de imágenes WPF con transición, migración o desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>Hacer que la aplicación empaquetada abra archivos en lugar de la aplicación de escritorio

Puede asegurarse de que los usuarios abran la nueva aplicación empaquetada para determinados tipos de archivos en lugar de abrir la versión de escritorio de la aplicación de forma predeterminada.

Para ello, especifica el [identificador de programación (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) de cada aplicación desde la cual quieras heredar asociaciones de archivos.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
<FileTypeAssociation Name="[AppID]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. Este identificador se usa internamente para crear un [identificador de programación (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) con hash relacionado con la asociación de tipos de archivo. Puedes usar este identificador para administrar los cambios en versiones futuras de la aplicación. |
|MigrationProgId |El [identificador de programación (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) que describe la aplicación, el componente y la versión de la aplicación de escritorio desde el que quieras heredar asociaciones de archivos.|

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <rescap3:MigrationProgIds>
              <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
              <rescap3:MigrationProgId>Foo.Bar.2</rescap3:MigrationProgId>
            </rescap3:MigrationProgIds>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```
#### <a name="related-sample"></a>Muestra relacionada

[Visor de imágenes WPF con transición, migración o desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>Asociar la aplicación empaquetada con un conjunto de tipos de archivo

Puedes asociar la aplicación empaquetada con extensiones de tipo de archivo. Si un usuario hace clic con el botón un archivo y, a continuación, selecciona la opción **Abrir con** , la aplicación aparece en la lista de sugerencias.

#### <a name="xml-namespace"></a>Espacio de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. Este identificador se usa internamente para crear un [identificador de programación (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) con hash relacionado con la asociación de tipos de archivo. Puedes usar este identificador para administrar los cambios en versiones futuras de la aplicación.   |
|FileType |Es el archivo de extensión compatible con la aplicación. |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
              <uap:FileType>.txt</uap:FileType>
              <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Muestra relacionada

[Visor de imágenes WPF con transición, migración o desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add" />

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>Agregar opciones en los menús contextuales de los archivos que tienen un tipo de archivo específico

En la mayoría de los casos, los usuarios hacen doble clic en los archivos para abrirlos. Si estos usuarios hacen clic con el botón derecho en un archivo, aparecerán varias opciones.

Puedes agregar opciones a ese menú. Estas opciones ofrecen a los usuarios diferentes formas de interactuar con el archivo como, por ejemplo, imprimirlo, editarlo u obtener una vista previa del mismo.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3


#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedVerbs>
              <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría | Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. |
|Verb |Es el nombre que aparece en el menú contextual del Explorador de archivos. Esta cadena se puede localizar mediante ```ms-resource```.|
|Id |Es el identificador único del verbo. Si la aplicación es una aplicación para UWP, esto se pasa a la aplicación como parte de los argumentos del evento de activación para que pueda controlar correctamente la selección del usuario. Si la aplicación es una aplicación de plena confianza empaquetada, recibe parámetros en su lugar (consulta el siguiente punto). |
|Parameters |Es la lista de parámetros de argumento y valores asociados con el verbo. Si la aplicación es una aplicación de plena confianza empaquetada, estos parámetros se pasan a la aplicación como argumentos del evento cuando se activa la aplicación. Puedes personalizar el comportamiento de la aplicación en función de los distintos verbos de activación. Si una variable puede contener una ruta de acceso de archivo, escribe el valor del parámetro entre comillas. Así evitarás cualquier problema si la ruta de acceso incluye espacios. Si la aplicación es una aplicación para UWP, no podrás pasar parámetros. En su lugar, la aplicación recibirá el identificador (consulta el punto anterior).|
|Extended |Especifica que el verbo solo debe aparecer si el usuario mantiene presionada la tecla **Mayús** para mostrar el menú contextual, antes de hacer clic con el botón derecho en el archivo. Este atributo es opcional y su valor predeterminado es **False** (por ejemplo, mostrar siempre el verbo) si no se incluye. Este comportamiento se especifica de forma individual para cada verbo (excepto "Abrir", que siempre es **False**).|

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
              <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
            </uap2:SupportedVerbs>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```
#### <a name="related-sample"></a>Muestra relacionada

[Visor de imágenes WPF con transición, migración o desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Abrir determinados tipos de archivos mediante una dirección URL

Puede asegurarse de que los usuarios abran la nueva aplicación empaquetada para determinados tipos de archivos en lugar de abrir la versión de escritorio de la aplicación de forma predeterminada.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes> 
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. |
|UseUrl |Indica si se deben abrir archivos directamente desde una dirección URL de destino. Si no establece este valor, cualquier intento que realice la aplicación para abrir un archivo mediante una URL, provocará que el sistema descargue el archivo localmente. |
|Parameters |parámetros opcionales. |
|FileType |Extensiones de archivo relevantes. |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="documenttypes" UseUrl="true" Parameters="%1">
              <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
                <uap:FileType>.doc</uap:FileType>
              </uap:SupportedFileTypes> 
            </uap3:FileTypeAssociation>
          </uap:Extension>
        </Extensions>
      </Application>
    </Applications>
</Package>
```

## <a name="perform-setup-tasks"></a>Realizar tareas de configuración

* [Crear la excepción de firewall de la aplicación](#rules)
* [Colocar los archivos DLL en cualquier carpeta del paquete](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>Crear la excepción de firewall de la aplicación

Si la aplicación debe comunicarse a través de un puerto, puedes agregar la aplicación a la lista de excepciones del firewall.

#### <a name="xml-namespace"></a>Espacio de nombres XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.firewallRules">  
  <FirewallRules Executable="[executable file name]">  
    <Rule
      Direction="[Direction]"
      IPProtocol="[Protocol]"
      LocalPortMin="[LocalPortMin]"
      LocalPortMax="LocalPortMax"
      RemotePortMin="RemotePortMin"
      RemotePortMax="RemotePortMax"
      Profile="[Profile]"/>  
  </FirewallRules>  
</Extension>
```
Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules).

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre ``windows.firewallRules``|
|Executable |Nombre del archivo ejecutable que quieras agregar a la lista de excepciones del firewall |
|Direction |Indica si la regla es entrante o saliente |
|IPProtocol |Protocolo de comunicación |
|LocalPortMin |El menor número de puerto en un intervalo de números de puerto local. |
|LocalPortMax |El mayor número de puerto de un intervalo de números de puerto local. |
|RemotePortMax |El menor número de puerto en un intervalo de números de puerto remoto. |
|RemotePortMax |El mayor número de puerto de un intervalo de números de puerto remoto. |
|Perfil |Tipo de red |



#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Extensions>
    <desktop2:Extension Category="windows.firewallRules">  
      <desktop2:FirewallRules Executable="Contoso.exe">  
          <desktop2:Rule Direction="in" IPProtocol="TCP" Profile="all"/>  
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="domain"/>  
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="public"/>  
          <desktop2:Rule Direction="out" IPProtocol="UDP" LocalPortMin="1339" LocalPortMax="1340" RemotePortMin="15"
                         RemotePortMax="19" Profile="domainAndPrivate"/>  
          <desktop2:Rule Direction="out" IPProtocol="GRE" Profile="private"/>  
      </desktop2:FirewallRules>  
  </desktop2:Extension>
</Extensions>
</Package>
```

<a id="load-paths" />

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>Colocar los archivos DLL en cualquier carpeta del paquete

Usa una extensión para identificar esas carpetas. De este modo, el sistema puede buscar y cargar los archivos que coloques en ellas. Piensa en esta extensión como un sustituto de la variable de entorno _%PATH%_.

Si no usas esta extensión, el sistema busca el gráfico de dependencia del paquete del proceso, la carpeta raíz del paquete y, a continuación, el directorio del sistema (_%SystemRoot%\system32_) en ese orden. Para obtener más información, consulta [Orden de búsqueda de aplicaciones Windows](https://msdn.microsoft.com/library/windows/desktop/ms682586.aspx#_search_order_for_windows_store_apps).

Cada paquete puede contener solo una de estas extensiones. Esto significa que puedes agregar una de ellas al paquete principal y, a continuación, agregar una a cada uno de tus [paquetes opcionales y conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

#### <a name="xml-namespace"></a>Espacio de nombres XML

http://schemas.microsoft.com/appx/manifest/uap/windows10/6

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión
Declara esta extensión en el nivel de paquete del manifiesto de la aplicación.

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|Nombre | Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.loaderSearchPathOverride``
|FolderPath | La ruta de la carpeta que contiene tus archivos dll. Especifica una ruta de acceso relativa a la carpeta raíz del paquete. En una extensión puedes especificar hasta cinco rutas de acceso. Si quieres que el sistema busque archivos en la carpeta raíz del paquete, usa una cadena vacía para una de estas rutas de acceso. No incluyas rutas de acceso duplicados y asegúrate de que las rutas de acceso no contengan barras diagonales iniciales y finales o barras diagonales inversas. <br><br> El sistema no buscará en subcarpetas, por tanto asegúrate de indicar explícitamente cada carpeta que contenga archivos DLL que quieres que cargue el sistema.|

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"
  IgnorableNamespaces="uap6">
  ...
    <Extensions>
      <uap6:Extension Category="windows.loaderSearchPathOverride">
        <uap6:LoaderSearchPathOverride>
          <uap6:LoaderSearchPathEntry FolderPath=""/>
          <uap6:LoaderSearchPathEntry FolderPath="folder1/subfolder1"/>
          <uap6:LoaderSearchPathEntry FolderPath="folder2/subfolder2"/>
        </uap6:LoaderSearchPathOverride>
      </uap6:Extension>
    </Extensions>
...
</Package>
```

## <a name="integrate-with-file-explorer"></a>Integrar con el Explorador de archivos

Esta opción permite que los usuarios organicen tus archivos e interactúen con ellos de forma común.

* [Definir el comportamiento de la aplicación cuando los usuarios seleccionan y abren varios archivos al mismo tiempo](#define)
* [Mostrar el contenido del archivo en una imagen en miniatura en el Explorador de archivos](#show)
* [Mostrar el contenido del archivo en un panel de vista previa del Explorador de archivos](#preview)
* [Permitir que los usuarios agrupen los archivos mediante la columna Tipo del Explorador de archivos](#enable)
* [Hacer que las propiedades de archivo tengan disponibles las opciones de búsqueda, índice, diálogos de propiedad y panel de detalles](#make-file-properties)
* [Hacer que los archivos del servicio en la nubes aparezcan en el Explorador de archivos](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Definir el comportamiento de la aplicación cuando los usuarios seleccionan y abren varios archivos al mismo tiempo

Especificar cómo se comporta la aplicación cuando el usuario abre varios archivos al mismo tiempo.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
          <SupportedFileTypes>
                <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```
Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. |
|MultiSelectModel |Consulta la información que tienes a continuación. |
|FileType |Extensiones de archivo relevantes. |

**MultSelectModel**

Las aplicaciones de escritorio empaquetadas tienen las mismas tres opciones que las aplicaciones de escritorio normales.

 * ``Player``: La aplicación se activa una vez. Todos los archivos seleccionados se pasan a la aplicación como parámetros de argumento.
 * ``Single``: La aplicación se activa una vez para el primer archivo seleccionado. Otros archivos se omiten.
 * ``Document``: Una nueva instancia independiente de la aplicación se activa para cada archivo seleccionado.

 Puedes establecer preferencias diferentes para distintos tipos de archivo y acciones. Por ejemplo, si deseas abrir *documentos* en el modo *Document* y las *imágenes* en el modo *Player*.

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
              <uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
            </uap2:SupportedVerbs>
            <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
            </uap:SupportedFileTypes>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Si el usuario abre 15 archivos o menos, la opción predeterminada del atributo **MultiSelectModel** atributo es *Player*. De lo contrario, el valor predeterminado es *Document*. Las aplicaciones para UWP siempre se inician como *Player*.

<a id="show" />

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>Mostrar el contenido del archivo en una imagen en miniatura en el Explorador de archivos

Permite que los usuarios vean una imagen en miniatura del contenido del archivo cuando aparezca el icono del archivo con un tamaño medio, grande o extra grande.

#### <a name="xml-namespace"></a>Espacio de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. |
|FileType |Extensiones de archivo relevantes. |
|Clsid   |Identificador de clase de la aplicación. |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap2:SupportedFileTypes>
            <desktop2:ThumbnailHandler
              Clsid  ="20000000-0000-0000-0000-000000000001"  />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="preview" />

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>Mostrar el contenido del archivo en el panel de vista previa del Explorador de archivos

Permite que los usuarios obtengan una vista previa del contenido de un archivo en el panel de vista previa del Explorador de archivos.

#### <a name="xml-namespace"></a>Espacio de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. |
|FileType |Extensiones de archivo relevantes. |
|Clsid   |Identificador de clase de la aplicación. |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap2SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
                </uap2SupportedFileTypes>
              <desktop2:DesktopPreviewHandler Clsid ="20000000-0000-0000-0000-000000000001" />
           </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="enable" />

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>Permitir que los usuarios agrupen los archivos mediante la columna Tipo del Explorador de archivos

Puedes asociar uno o más valores predefinidos de los tipos de archivo mediante el campo **Kind**.

En el Explorador de archivos, los usuarios pueden agrupar estos archivos mediante ese campo. Los componentes del sistema también usan este campo para diversos fines, como la indización.

Para obtener más información sobre el campo **Kind** y los valores que puedes usar en este campo, consulta [Using Kind Names (Usar nombres del campo Kind)](https://msdn.microsoft.com/library/windows/desktop/cc144136.aspx).

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```
Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. |
|FileType |Extensiones de archivo relevantes. |
|value |[Valor Kind](https://msdn.microsoft.com/en-us/library/windows/desktop/cc144136.aspx#kind_hierarchy) válido |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="Contoso">
             <uap:SupportedFileTypes>
               <uap:FileType>.m4a</uap:FileType>
               <uap:FileType>.mta</uap:FileType>
             </uap:SupportedFileTypes>
             <rescap:KindMap>
               <rescap:Kind value="Item">
               <rescap:Kind value="Communications">
               <rescap:Kind value="Task">
             </rescap:KindMap>
          </uap:FileTypeAssociation>
      </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```
<a id="make-file-properties" />

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>Hacer que las propiedades de archivo tengan disponibles las opciones de búsqueda, índice, diálogos de propiedad y panel de detalles

#### <a name="xml-namespace"></a>Espacio de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```
Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. |
|FileType |Extensiones de archivo relevantes. |
|Clsid  |Identificador de clase de la aplicación. |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap:SupportedFileTypes>
            <desktop2:DesktopPropertyHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files" />

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>Hacer que los archivos del servicio en la nubes aparezcan en el Explorador de archivos

Registra los controladores que se implementan en la aplicación. También puedes agregar opciones de menú contextual que aparecen cuando los usuarios hacen clic con el botón derecho en los archivos en la nube en el Explorador de archivos.

#### <a name="xml-namespace"></a>Espacio de nombres XML

* http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.cloudfiles" >
    <CloudFiles IconResource="[Icon]">
        <CustomStateHandler Clsid ="[Clsid]"/>
        <ThumbnailProviderHandler Clsid ="[Clsid]"/>
        <ExtendedPropertyhandler Clsid ="[Clsid]"/>
        <CloudFilesContextMenus>
            <Verb Id ="Command3" Clsid= "[GUID]">[Verb Label]</Verb>
        </CloudFilesContextMenus>
    </CloudFiles>
</Extension>

```

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.cloudfiles``
|iconResource |El icono que representa tu servicio de proveedor de archivos en la nube. Este icono aparece en el panel Navegación del Explorador de archivos.  Los usuarios eligen este icono para mostrar archivos desde tu servicio en la nube. |
|Clsid CustomStateHandler |El identificador de clase de la aplicación que implementa el CustomStateHandler. El sistema usa este id. de clase para solicitar estados personalizados y columnas para archivos de la nube. |
|Clsid ThumbnailProviderHandler |El identificador de clase de la aplicación que implementa el ThumbnailProviderHandler. El sistema usa este id. de clase para solicitar imágenes en miniatura para archivos de la nube. |
|Clsid ExtendedPropertyHandler |El identificador de clase de la aplicación que implementa el ExtendedPropertyHandler.  El sistema usa este id. de clase para solicitar propiedades ampliadas para un archivo de la nube. |
|Verbo |El nombre que aparece en el menú contextual del Explorador de archivos para archivos proporcionados por el servicio en la nube. |
|Id |Es el id. único del verbo. |

#### <a name="example"></a>Ejemplo

```XML
<Package
    xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
    IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <Extension Category="windows.cloudfiles" >
            <CloudFiles IconResource="images\Wide310x150Logo.png">
                <CustomStateHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ThumbnailProviderHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ExtendedPropertyhandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <desktop:CloudFilesContextMenus>
                    <desktop:Verb Id ="keep" Clsid=     
                       "20000000-0000-0000-0000-000000000001">
                       Always keep on this device</desktop:Verb>
                </desktop:CloudFilesContextMenus>
            </CloudFiles>
          </Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="start" />

## <a name="start-your-application-in-different-ways"></a>Iniciar la aplicación de formas diferentes

* [Iniciar la aplicación mediante un protocolo](#protocol)
* [Iniciar la aplicación mediante un alias](#alias)
* [Iniciar un archivo ejecutable cuando los usuarios inicien sesión en Windows](#executable)
* [Permitir que los usuarios inicien la aplicación cuando conecten un dispositivo a su PC](#autoplay)
* [Reiniciar automáticamente después de recibir una actualización de Microsoft Store](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>Iniciar la aplicación mediante un protocolo

Las asociaciones de protocolos permiten que otros programas y componentes del sistema interactúen con la aplicación empaquetada. Cuando la aplicación empaquetada se inicia mediante un protocolo, puedes especificar parámetros específicos que pasar a sus argumentos de evento de activación, de forma que se comporte en consecuencia. Los parámetros solo son compatibles con aplicaciones empaquetadas de plena confianza. Las aplicaciones para UWP no pueden usar parámetros.  

#### <a name="xml-namespace"></a>Espacio de nombres XML

http://schemas.microsoft.com/appx/manifest/uap/windows10/3


#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension
    Category="windows.protocol">
    <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.protocol``
|Name |Nombre del protocolo. |
|Parameters |La lista de parámetros y valores para pasar a la aplicación como argumentos del evento cuando se activa la aplicación. Si una variable puede contener una ruta de acceso de archivo, escribe el valor del parámetro entre comillas. Así evitarás cualquier problema si la ruta de acceso incluye espacios. |

### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap3:Extension
          Category="windows.protocol">
        <uap3:Protocol
          Name="myapp-cmd"
          Parameters="/p &quot;%1&quot;" />
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```
<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>Iniciar la aplicación mediante un alias

Los usuarios y otros procesos pueden usar un alias para iniciar la aplicación sin tener que especificar la ruta de acceso completa a la aplicación. Puedes especificar el nombre de ese alias.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10


#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <AppExecutionAlias>
            <desktop:ExecutionAlias Alias="[AliasName]" />
      </AppExecutionAlias>
</Extension>
```

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.appExecutionAlias``
|Executable |Ruta de acceso relativa al archivo ejecutable que se iniciará cuando se llame al alias. |
|Alias |Nombre corto de la aplicación. Siempre debe acabar con la extensión ".exe". Solo puedes especificar un único alias de ejecución de la aplicación para cada aplicación del paquete. Si varias aplicaciones se registran para el mismo alias, el sistema llamará a la última que se haya registrado, así que asegúrate de elegir un alias exclusivo que sea bastante improbable que otras aplicaciones sustituyan.
|

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  ...
  <uap3:Extension
        Category="windows.appExecutionAlias"
        Executable="exes\launcher.exe"
        EntryPoint="Windows.FullTrustApplication">
        <uap3:AppExecutionAlias>
            <desktop:ExecutionAlias Alias="Contoso.exe" />
        </uap3:AppExecutionAlias>
  </uap3:Extension>
...
</Package>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>Iniciar un archivo ejecutable cuando los usuarios inicien sesión en Windows

Las tareas de inicio que la aplicación pueda ejecutar un archivo ejecutable automáticamente cuando un usuario inicie sesión.

> [!NOTE]
> El usuario tiene que iniciar la aplicación al menos una vez para registrar esta tarea de inicio.

La aplicación puede declarar varias tareas de inicio. Cada tarea se inicia de forma independiente. Todas las tareas de inicio aparecerán en el Administrador de tareas de la pestaña **Inicio** con el nombre especificado en el manifiesto de la aplicación y el icono de la aplicación. El Administrador de tareas analizará automáticamente el impacto en el inicio de las tareas.

Los usuarios pueden deshabilitar manualmente las tareas de inicio de la aplicación mediante el Administrador de tareas. Si un usuario deshabilita una tarea, no podrá volver a habilitarla mediante programación.

#### <a name="xml-namespace"></a>Espacio de nombres XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension
    Category="windows.startupTask"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <StartupTask
      TaskId="[TaskID]"
      Enabled="true"
      DisplayName="[DisplayName]" />
</Extension>
```

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.startupTask``|
|Executable |Ruta de acceso relativa para que se inicie el archivo ejecutable. |
|TaskId |Identificador único de la tarea. Con este identificador, la aplicación puede llamar a las API en la clase [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) para habilitar o deshabilitar una tarea de inicio mediante programación. |
|Enabled |Indica si la tarea que se inicia primero está habilitada o deshabilitada. Las tareas habilitadas se ejecutarán la próxima vez que el usuario inicie sesión (a menos que el usuario las deshabilite). |
|DisplayName |Nombre de la tarea que aparece en el Administrador de tareas. Puedes localizar esta cadena mediante ```ms-resource```. |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <desktop:Extension
          Category="windows.startupTask"
          Executable="bin\MyStartupTask.exe"
          EntryPoint="Windows.FullTrustApplication">
          <desktop:StartupTask
          TaskId="MyStartupTask"
          Enabled="true"
          DisplayName="My App Service" />
        </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
 </Package>
```
<a id="autoplay" />

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>Permitir que los usuarios inicien la aplicación cuando conecten un dispositivo a su PC

Reproducción automática puede presentar tu aplicación como una opción cuando un usuario conecte un dispositivo a su PC.

#### <a name="xml-namespace"></a>Espacio de nombres XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/3


#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.autoPlayHandler">
  <AutoPlayHandler>
    <InvokeAction ActionDisplayName="[action string]" ProviderDisplayName="[name of your app/service]">
      <Content ContentEvent="[Content event]" Verb="[any string]" DropTargetHandler="[Clsid]" />
      <Content ContentEvent="[Content event]" Verb="[any string]" Parameters="[Initialization parameter]"/>
      <Device DeviceEvent="[Device event]" HWEventHandler="[Clsid]" InitCmdLine="[Initialization parameter]"/>
    </InvokeAction>
  </AutoPlayHandler>
```

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.autoPlayHandler``
|ActionDisplayName |Una cadena que representa la acción que los usuarios pueden realizar con un dispositivo que conectan a un PC (por ejemplo: "Importar archivos" o "Reproducir vídeo"). |
|ProviderDisplayName | Una cadena que representa la aplicación o servicio (por ejemplo: "Reproductor de vídeo Contoso"). |
|ContentEvent |El nombre de un evento de contenido que hace que a los usuarios les aparezca tu ``ActionDisplayName`` y ``ProviderDisplayName``. Se genera un evento de contenido cuando se inserta en el PC un dispositivo de volumen, como una tarjeta de memoria de cámara, una unidad USB o un DVD. Puedes encontrar la lista completa de esos eventos [aquí](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference).  |
|Verbo |La configuración verbo identifica un valor que se pasa a la aplicación para la opción seleccionada. Puedes especificar varias acciones de inicio para un evento de Reproducción automática y usar la configuración Verbo para determinar qué opción seleccionó un usuario para tu aplicación. Para saber qué opción seleccionó el usuario, comprueba la propiedad verb de los argumentos del evento de inicio que se pasaron a la aplicación. Puedes usar cualquier valor para la configuración Verbo a excepción de open, que está reservado. |
|DropTargetHandler |El identificador de clase de la aplicación que implementa la interfaz [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) . Los archivos del medio extraíble se pasan al método [Colocar](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) de tu implementación [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017).  |
|Parámetros |No tienes que implementar la interfaz [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) para todos los eventos de contenido. Para cualquiera de los eventos de contenido, podrías proporcionar los parámetros de línea de comandos en lugar de implementar la interfaz [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017). Para esos eventos, reproducción automática inicia tu aplicación utilizando los parámetros de línea de comandos. Puedes analizar esos parámetros en el código de inicialización de la aplicación para determinar si se inició mediante Reproducción automática y, a continuación, proporcionar tu implementación personalizada. |
|DeviceEvent |El nombre de un evento de dispositivo que hace que a los usuarios les aparezca tu ``ActionDisplayName`` y ``ProviderDisplayName``. Un evento de dispositivo de Reproducción automática se genera cuando se conecta un dispositivo al PC. Los eventos de dispositivo comienzan con la cadena ``WPD`` y puedes encontrarlos enumerados [aquí](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference). |
|HWEventHandler |El identificador de clase de la aplicación que implementa la interfaz [IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx) . |
|InitCmdLine |El parámetro de cadena que desees pasar al método [Inicializar](https://msdn.microsoft.com/en-us/library/windows/desktop/bb775495.aspx) de la interfaz [IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx). |

### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:desktop3="http://schemas.microsoft.com/appx/manifest/desktop/windows10/3"
  IgnorableNamespaces="desktop3">
  <Applications>
    <Application>
      <Extensions>
        <desktop3:Extension Category="windows.autoPlayHandler">
          <desktop3:AutoPlayHandler>
            <desktop3:InvokeAction ActionDisplayName="Import my files" ProviderDisplayName="ms-resource:AutoPlayDisplayName">
              <desktop3:Content ContentEvent="ShowPicturesOnArrival" Verb="show" DropTargetHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8"/>
              <desktop3:Content ContentEvent="PlayVideoFilesOnArrival" Verb="play" Parameters="%1" />
              <desktop3:Device DeviceEvent="WPD\ImageSource" HWEventHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8" InitCmdLine="/autoplay"/>
            </desktop3:InvokeAction>
          </desktop3:AutoPlayHandler>
      </Extensions>
    </Application>
  </Applications>
</Package>
```
<a id="updates" />

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Reiniciar automáticamente después de recibir una actualización de Microsoft Store

Si la aplicación está abierta cuando los usuarios instalar una actualización, se cierra la aplicación.

Si quieres que esa aplicación se reinicie una vez completada la actualización, llama a la función [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) en cada proceso que quieras reiniciar.

Cada ventana activa de la aplicación recibe un mensaje [WM_QUERYENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376890.aspx) . En este punto, la aplicación puede llamar a la función [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) nuevo para actualizar la línea de comandos si es necesario.

Cuando cada ventana activa de la aplicación recibe el mensaje [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) , la aplicación debe guardar datos y se cierra.

>[!NOTE]
Las ventanas activas también reciben el mensaje [WM_CLOSE](https://msdn.microsoft.com/library/windows/desktop/ms632617.aspx) en caso de que la aplicación no controle el mensaje [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) .

En este punto, la aplicación tiene 30 segundos para cerrar sus procesos o la plataforma ellos fuerza.

Una vez completada la actualización, se reinicia la aplicación.

## <a name="work-with-other-applications"></a>Trabajar con otras aplicaciones

Realiza procesos de integración con otras aplicaciones, inicia otros procesos o comparte información.

* [Hacer que la aplicación aparezca como destino de impresión en aplicaciones que admitan funciones de impresión](#printing)
* [Compartir fuentes con otras aplicaciones de Windows](#fonts)
* [Iniciar un proceso de Win32 desde una aplicación de la Plataforma universal de Windows (UWP)](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>Hacer que la aplicación aparezca como destino de impresión en aplicaciones que admitan funciones de impresión

Cuando los usuarios quieran imprimir datos desde otra aplicación, como el Bloc de notas, puede hacer que la aplicación aparezca como destino de impresión en la lista de destinos de impresión disponibles.

Tendrás que modificar la aplicación para que reciba datos de impresión en formato XML Paper Specification (XPS).

#### <a name="xml-namespaces"></a>Espacios de nombres XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.appPrinter``
|DisplayName |Es el nombre que quieres que aparezcan en la lista de destinos de impresión de una aplicación. |
|Parameters |Los parámetros que requiere la aplicación para controlar correctamente la solicitud. |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Applications>
  <Application>
    <Extensions>
      <desktop2:Extension Category="windows.appPrinter">
        <desktop2:AppPrinter
          DisplayName="Send to Contoso"
          Parameters="/insertdoc %1" />
      </desktop2:Extension>
    </Extensions>
  </Application>
</Applications>
</Package>
```
Puedes encontrar un ejemplo que use esa extensión [aquí](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF)

<a id="fonts" />

### <a name="share-fonts-with-other-windows-applications"></a>Compartir fuentes con otras aplicaciones de Windows

Comparte tus fuentes personalizadas con otras aplicaciones de Windows.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](https://review.docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts).


|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.sharedFonts``
|File |Es el archivo que contiene las fuentes que quieres compartir. |

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
  IgnorableNamespaces="uap4">
  <Applications>
    <Application>
      <Extensions>
        <uap4:Extension Category="windows.sharedFonts">
          <uap4:SharedFonts>
            <uap4:Font File="Fonts\JustRealize.ttf" />
            <uap4:Font File="Fonts\JustRealizeBold.ttf" />
          </uap4:SharedFonts>
        </uap4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```
<a id="win32-process" />

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>Iniciar un proceso de Win32 desde una aplicación de la Plataforma universal de Windows (UWP)

Inicia un proceso de Win32 que se ejecute en plena confianza.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fullTrustProcess``
|GroupID |Cadena que identifica el conjunto de parámetros que quieres pasar al archivo ejecutable. |
|Parameters |Parámetros que quieres pasar al archivo ejecutable. |

#### <a name="example"></a>Ejemplo

```XML
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap=
"http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10">
  ...
  <Capabilities>
      <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application>
      <Extensions>
          <desktop:Extension Category="windows.fullTrustProcess" Executable="fulltrustprocess.exe">
              <desktop:FullTrustProcess>
                  <desktop:ParameterGroup GroupId="SyncGroup" Parameters="/Sync"/>
                  <desktop:ParameterGroup GroupId="OtherGroup" Parameters="/Other"/>
              </desktop:FullTrustProcess>
           </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```
Esta extensión puede resultar útil si quieres crear una interfaz de usuario de plataforma Universal de Windows que se ejecuta en todos los dispositivos, pero quieres que los componentes de la aplicación de Win32 sigan realizando una ejecución de plena confianza.

Solo tienes que crear un paquete de aplicación de Windows para la aplicación de Win32. A continuación, agrega esta extensión al archivo de paquete de la aplicación para UWP. Este extensiones indica que quieres iniciar un archivo ejecutable en el paquete de aplicación de Windows.  Si quieres comunicarte entre la aplicación para UWP y la aplicación de Win32, puedes configurar uno o más [servicios de la aplicación](../launch-resume/app-services.md) para poder hacerlo. Puedes leer más acerca de este ejemplo [aquí](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/).

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
