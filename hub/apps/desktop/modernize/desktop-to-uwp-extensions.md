---
Description: Puedes usar extensiones para integrar tu aplicación de escritorio empaquetada con Windows 10 mediante formas predefinidas.
title: Integrar la aplicación empaquetada de escritorio con Windows 10 y UWP (puente de escritorio)
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 38a5a6fae3e6477321a8879a50dff21e10fd92e2
ms.sourcegitcommit: 48ca9c99b52f93a805d5afeb7045ba4bf6597371
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622944"
---
# <a name="integrate-your-packaged-desktop-app-with-windows-10-and-uwp"></a>Integre su aplicación de escritorio empaquetado con Windows 10 y UWP

Si se [empaquetar su aplicación de escritorio en un contenedor de MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root), puede usar extensiones para integrar la aplicación de escritorio empaquetada con Windows 10 mediante el uso de extensiones predefinidas en el [manifiesto del paquete de aplicación](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Por ejemplo, utilice una extensión para crear una excepción de firewall, que la aplicación predeterminada para un tipo de archivo de la aplicación o elija los iconos de inicio a la versión empaquetada de la aplicación. Para usar una extensión, solo tienes que agregar algunos archivos XML al archivo de manifiesto de paquete de la aplicación. No se requiere ningún tipo de código.

En este artículo se describe estas extensiones y las tareas que puede realizar con ellos.

> [!NOTE]
> Las características descritas en este artículo requieren la creación de un paquete de aplicación de Windows para su aplicación de escritorio. Si aún no ha hecho esto, consulte [empaquetar aplicaciones de escritorio](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root).

## <a name="transition-users-to-your-app"></a>Proceso de transición de usuarios a la aplicación

Ayuda a los usuarios a cambiar a la aplicación empaquetada.

* [Elija los iconos de inicio existentes y los botones de barra de tareas a la aplicación empaquetada](#point)
* [Asegúrese de abrir archivos en lugar de la aplicación de escritorio de la aplicación empaquetada](#make)
* [Asociar la aplicación empaquetada con un conjunto de tipos de archivo](#associate)
* [Agregar opciones a los menús contextuales de los archivos que tienen un tipo de archivo determinado.](#add)
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
|Category |Siempre ``windows.desktopAppMigration``
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

[Visor de imágenes WPF con transición y migración/desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>Asegúrese de abrir archivos en lugar de la aplicación de escritorio de la aplicación empaquetada

Puede asegurarse de que los usuarios abran la aplicación empaquetada nuevo de forma predeterminada para tipos específicos de archivos en lugar de abrir la versión de escritorio de la aplicación.

Para ello, especifica el [identificador de programación (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) de cada aplicación desde la cual quieras heredar asociaciones de archivos.

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

|Name |Descripción |
|-------|-------------|
|Category |Siempre ``windows.fileTypeAssociation``
|Name |Identificador único de la aplicación. Este identificador se usa internamente para crear un [identificador de programación (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) con hash relacionado con la asociación de tipos de archivo. Puede utilizar este identificador para administrar los cambios en versiones futuras de la aplicación. |
|MigrationProgId |El [identificador de programación (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) que describe la aplicación, el componente y la versión de la aplicación de escritorio desde la que desea heredar las asociaciones de archivo.|

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

[Visor de imágenes WPF con transición y migración/desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>Asociar la aplicación empaquetada con un conjunto de tipos de archivo

La aplicación empaquetada puede asociar extensiones de tipo de archivo. Si un usuario hace clic con el botón un archivo y, a continuación, selecciona el **abrir con** opción, la aplicación aparece en la lista de sugerencias.

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

|Name |Descripción |
|-------|-------------|
|Category |Siempre ``windows.fileTypeAssociation``
|Name |Identificador único de la aplicación. Este identificador se usa internamente para crear un [identificador de programación (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) con hash relacionado con la asociación de tipos de archivo. Puede utilizar este identificador para administrar los cambios en versiones futuras de la aplicación.   |
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

[Visor de imágenes WPF con transición y migración/desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

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
|Category | Siempre ``windows.fileTypeAssociation``
|Name |Identificador único de la aplicación. |
|Verb |Es el nombre que aparece en el menú contextual del Explorador de archivos. Esta cadena se puede localizar mediante ```ms-resource```.|
|Id |Es el identificador único del verbo. Si la aplicación es una aplicación para UWP, se pasa a la aplicación como parte de sus argumentos del evento de activación para que pueda procesar correctamente la selección del usuario. Si la aplicación es una aplicación empaquetada de plena confianza, recibe los parámetros en su lugar (consulte la siguiente viñeta). |
|Parámetros |Es la lista de parámetros de argumento y valores asociados con el verbo. Si la aplicación es una aplicación empaquetada de plena confianza, estos parámetros se pasan a la aplicación como argumentos del evento cuando se activa la aplicación. Puede personalizar el comportamiento de la aplicación en función de los verbos de activación diferentes. Si una variable puede contener una ruta de acceso de archivo, escribe el valor del parámetro entre comillas. Así evitarás cualquier problema si la ruta de acceso incluye espacios. Si la aplicación es una aplicación para UWP, no puede pasar parámetros. En su lugar, la aplicación recibirá el identificador (consulta el punto anterior).|
|Extendido |Especifica que el verbo solo debe aparecer si el usuario mantiene presionada la tecla **Mayús** para mostrar el menú contextual, antes de hacer clic con el botón derecho en el archivo. Este atributo es opcional y su valor predeterminado es **False** (por ejemplo, mostrar siempre el verbo) si no se incluye. Este comportamiento se especifica de forma individual para cada verbo (excepto "Abrir", que siempre es **False**).|

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

[Visor de imágenes WPF con transición y migración/desinstalación](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Abrir determinados tipos de archivos mediante una dirección URL

Puede asegurarse de que los usuarios abran la aplicación empaquetada nuevo de forma predeterminada para tipos específicos de archivos en lugar de abrir la versión de escritorio de la aplicación.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* http:\//schemas.microsoft.com/appx/manifest/uap/windows10
* http:\//schemas.microsoft.com/appx/manifest/uap/windows10/3"

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
|Category |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. |
|UseUrl |Indica si se deben abrir archivos directamente desde una dirección URL de destino. Si no establece este valor, trata de la aplicación para abrir un archivo mediante una causa de la dirección URL del sistema para la primera descarga el archivo localmente. |
|Parámetros |parámetros opcionales. |
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

* [Crear la excepción de firewall para la aplicación](#rules)
* [Coloque los archivos DLL en cualquier carpeta del paquete](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>Crear la excepción de firewall de la aplicación

Si la aplicación requiere comunicación a través de un puerto, puede agregar la aplicación a la lista de excepciones de firewall.

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
|Archivo ejecutable |Nombre del archivo ejecutable que quieras agregar a la lista de excepciones del firewall |
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

Usa una extensión para identificar esas carpetas. De este modo, el sistema puede buscar y cargar los archivos que coloques en ellas. Piensa en esta extensión como un sustituto de la variable de entorno _%PATH%_ .

Si no usas esta extensión, el sistema busca el gráfico de dependencia del paquete del proceso, la carpeta raíz del paquete y, a continuación, el directorio del sistema ( _%SystemRoot%\system32_) en ese orden. Para obtener más información, consulta [Orden de búsqueda de aplicaciones Windows](https://docs.microsoft.com/windows/desktop/Dlls/dynamic-link-library-search-order).

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
|Category |Siempre ``windows.loaderSearchPathOverride``
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

* [Definir cómo se comporta la aplicación cuando los usuarios seleccionan y abren varios archivos al mismo tiempo](#define)
* [Mostrar el contenido del archivo en una imagen en miniatura en el Explorador de archivos](#show)
* [Mostrar el contenido del archivo en el panel de vista previa del explorador de archivos](#preview)
* [Permitir que los usuarios para agrupar los archivos mediante el uso de la columna de tipo en el Explorador de archivos](#enable)
* [Propiedades de archivo a disposición búsqueda, índice, cuadros de diálogo Propiedades y el panel de detalles](#make-file-properties)
* [Especifique un controlador del menú contextual para un tipo de archivo](#context-menu)
* [Asegúrese de archivos desde su servicio en la nube aparecen en el Explorador de archivos](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Definir cómo se comporta la aplicación cuando los usuarios seleccionan y abren varios archivos al mismo tiempo

Especifique cómo se comporta la aplicación cuando un usuario abre varios archivos simultáneamente.

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
|Category |Siempre ``windows.fileTypeAssociation``
|Nombre |Identificador único de la aplicación. |
|MultiSelectModel |Consulta la información que tienes a continuación. |
|FileType |Extensiones de archivo relevantes. |

**MultSelectModel**

Las aplicaciones de escritorio empaquetadas tienen las mismas tres opciones que las aplicaciones de escritorio normales.

* ``Player``: La aplicación se activa una vez. Todos los archivos seleccionados se pasan a la aplicación como parámetros de argumento.
* ``Single``: La aplicación se activa una vez para el primer archivo seleccionado. Otros archivos se omiten.
* ``Document``: Se activa una instancia nueva e independiente de la aplicación para cada archivo seleccionado.

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
|Category |Siempre ``windows.fileTypeAssociation``
|Name |Identificador único de la aplicación. |
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

|Name |Descripción |
|-------|-------------|
|Category |Siempre ``windows.fileTypeAssociation``
|Name |Identificador único de la aplicación. |
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

Para obtener más información sobre el campo **Kind** y los valores que puedes usar en este campo, consulta [Using Kind Names (Usar nombres del campo Kind)](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names).

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

|Name |Descripción |
|-------|-------------|
|Category |Siempre ``windows.fileTypeAssociation``
|Name |Identificador único de la aplicación. |
|FileType |Extensiones de archivo relevantes. |
|value |[Valor Kind](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names) válido |

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
|Category |Siempre ``windows.fileTypeAssociation``
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

<a id="context-menu" />

### <a name="specify-a-context-menu-handler-for-a-file-type"></a>Especifique un controlador del menú contextual para un tipo de archivo

Si su aplicación de escritorio define un [controlador del menú contextual](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers), use esta extensión para registrar el controlador de menú.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* http://schemas.microsoft.com/appx/manifest/foundation/windows10
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/4

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extensions>
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="[AppID]" DisplayName="[DisplayName]">
                <com:Class Id="[Clsid]" Path="[Path]" ThreadingModel="[Model]"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type="[Type]">
                <desktop4:Verb Id="[ID]" Clsid="[Clsid]" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
</Extensions>
```

Buscar la referencia de esquema completa aquí: [com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) y [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus).

#### <a name="instructions"></a>Instrucciones

Para registrar el controlador del menú contextual, siga estas instrucciones.

1. En su aplicación de escritorio, implemente un [controlador del menú contextual](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers) implementando la [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) o [IExplorerCommandState](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommandstate) interfaz. Para obtener un ejemplo, vea el [ExplorerCommandVerb](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb) ejemplo de código. Asegúrese de que define un GUID de clase para cada uno de los objetos de implementación. Por ejemplo, el código siguiente define un identificador de clase para una implementación de [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand).

    ```cpp
    class __declspec(uuid("d0c8bceb-28eb-49ae-bc68-454ae84d6264")) CExplorerCommandVerb;
    ```

2. En el manifiesto del paquete, especifique un [com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) extensión de aplicación que se registra un servidor de suplente de COM con el identificador de clase de la implementación del controlador de menú contextual.

    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler">
                <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

2. En el manifiesto del paquete, especifique un [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) extensión de aplicación que registra la implementación del controlador de menú contextual.

    ```xml
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".rar">
                <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
    ```

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  IgnorableNamespaces="desktop4">
  <Applications>
    <Application>
      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler"">
              <com:Class Id="Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
          </com:ComServer>
        </com:Extension>
        <desktop4:Extension Category="windows.fileExplorerContextMenus">
          <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".contoso">
              <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
          </desktop4:FileExplorerContextMenus>
        </desktop4:Extension>
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
|Category |Siempre ``windows.cloudfiles``
|iconResource |El icono que representa tu servicio de proveedor de archivos en la nube. Este icono aparece en el panel Navegación del Explorador de archivos.  Los usuarios eligen este icono para mostrar archivos desde tu servicio en la nube. |
|Clsid CustomStateHandler |El identificador de clase de la aplicación que implementa el CustomStateHandler. El sistema usa este id. de clase para solicitar estados personalizados y columnas para archivos de la nube. |
|Clsid ThumbnailProviderHandler |El identificador de clase de la aplicación que implementa el ThumbnailProviderHandler. El sistema usa este id. de clase para solicitar imágenes en miniatura para archivos de la nube. |
|Clsid ExtendedPropertyHandler |El identificador de clase de la aplicación que implementa el ExtendedPropertyHandler.  El sistema usa este id. de clase para solicitar propiedades ampliadas para un archivo de la nube. |
|Verb |El nombre que aparece en el menú contextual del Explorador de archivos para archivos proporcionados por el servicio en la nube. |
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

## <a name="start-your-application-in-different-ways"></a>Iniciar la aplicación de maneras diferentes

* [Inicie la aplicación mediante un protocolo](#protocol)
* [Inicie la aplicación mediante un alias](#alias)
* [Iniciar un archivo ejecutable cuando los usuarios iniciar sesión en Windows](#executable)
* [Permitir que los usuarios iniciar la aplicación cuando se conecta a un dispositivo a su PC](#autoplay)
* [Reinicie automáticamente después de recibir una actualización de la Microsoft Store](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>Inicie la aplicación mediante un protocolo

Las asociaciones de protocolos permiten que otros programas y componentes del sistema interactúen con la aplicación empaquetada. Cuando se inicia la aplicación empaquetada mediante un protocolo, puede especificar los parámetros específicos para pasar a sus argumentos de evento de activación para que pueda comportarse según corresponda. Los parámetros solo son compatibles con aplicaciones empaquetadas de plena confianza. Las aplicaciones para UWP no pueden usar parámetros.

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
|Category |Siempre ``windows.protocol``
|Nombre |Nombre del protocolo. |
|Parámetros |La lista de parámetros y valores para pasar a la aplicación como argumentos de evento cuando se activa la aplicación. Si una variable puede contener una ruta de acceso de archivo, escribe el valor del parámetro entre comillas. Así evitarás cualquier problema si la ruta de acceso incluye espacios. |

### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
         <uap3:Extension
                Category="windows.appExecutionAlias"
                Executable="exes\launcher.exe"
                EntryPoint="Windows.FullTrustApplication">
            <uap3:AppExecutionAlias>
                <desktop:ExecutionAlias Alias="Contoso.exe" />
            </uap3:AppExecutionAlias>
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>Inicie la aplicación mediante un alias

Los usuarios y otros procesos que pueden usar un alias para iniciar la aplicación sin tener que especificar la ruta de acceso completa a la aplicación. Puedes especificar el nombre de ese alias.

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
|Category |Siempre ``windows.appExecutionAlias``
|Archivo ejecutable |Ruta de acceso relativa al archivo ejecutable que se iniciará cuando se llame al alias. |
|Alias |Nombre corto de la aplicación. Siempre debe acabar con la extensión ".exe". Solo puedes especificar un único alias de ejecución de la aplicación para cada aplicación del paquete. Si varias aplicaciones se registran para el mismo alias, el sistema llamará a la última que se haya registrado, así que asegúrate de elegir un alias exclusivo que sea bastante improbable que otras aplicaciones sustituyan.
|

#### <a name="example"></a>Ejemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
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
 
...
</Package>
```

Puedes encontrar la referencia de esquema completa [aquí](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>Iniciar un archivo ejecutable cuando los usuarios inicien sesión en Windows

Las tareas de inicio que la aplicación pueda ejecutar automáticamente un archivo ejecutable cada vez que un usuario inicia sesión.

> [!NOTE]
> El usuario debe iniciar la aplicación al menos una vez para registrar esta tarea de inicio.

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
|Category |Siempre ``windows.startupTask``|
|Archivo ejecutable |Ruta de acceso relativa para que se inicie el archivo ejecutable. |
|TaskId |Identificador único de la tarea. Con este identificador, la aplicación puede llamar a las API el [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) clase para habilitar o deshabilitar una tarea de inicio mediante programación. |
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

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>Permitir que los usuarios iniciar la aplicación cuando se conecta a un dispositivo a su PC

Reproducción automática puede presentar la aplicación como una opción cuando un usuario conecta un dispositivo a su PC.

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
|Category |Siempre ``windows.autoPlayHandler``
|ActionDisplayName |Una cadena que representa la acción que los usuarios pueden realizar con un dispositivo que se conectan a un equipo (por ejemplo: "Importar archivos" o "Reproducir vídeo"). |
|ProviderDisplayName | Una cadena que representa la aplicación o servicio (por ejemplo: "Contoso Reproductor de vídeo"). |
|ContentEvent |El nombre de un evento de contenido que hace que a los usuarios les aparezca tu ``ActionDisplayName`` y ``ProviderDisplayName``. Se genera un evento de contenido cuando se inserta en el PC un dispositivo de volumen, como una tarjeta de memoria de cámara, una unidad USB o un DVD. Puedes encontrar la lista completa de esos eventos [aquí](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference).  |
|Verb |El valor del verbo identifica un valor que se pasa a la aplicación para la opción seleccionada. Puedes especificar varias acciones de inicio para un evento de Reproducción automática y usar la configuración Verbo para determinar qué opción seleccionó un usuario para tu aplicación. Para saber qué opción seleccionó el usuario, comprueba la propiedad verb de los argumentos del evento de inicio que se pasaron a la aplicación. Puedes usar cualquier valor para la configuración Verbo a excepción de open, que está reservado. |
|DropTargetHandler |El identificador de clase de la aplicación que implementa el [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) interfaz. Los archivos del medio extraíble se pasan al método [Colocar](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) de tu implementación [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017).  |
|Parámetros |No tienes que implementar la interfaz [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) para todos los eventos de contenido. Para cualquiera de los eventos de contenido, podrías proporcionar los parámetros de línea de comandos en lugar de implementar la interfaz [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017). Para estos eventos, reproducción automática iniciará la aplicación mediante el uso de esos parámetros de línea de comandos. Puedes analizar esos parámetros en el código de inicialización de la aplicación para determinar si se inició mediante Reproducción automática y, a continuación, proporcionar tu implementación personalizada. |
|DeviceEvent |El nombre de un evento de dispositivo que hace que a los usuarios les aparezca tu ``ActionDisplayName`` y ``ProviderDisplayName``. Un evento de dispositivo de Reproducción automática se genera cuando se conecta un dispositivo al PC. Los eventos de dispositivo comienzan con la cadena ``WPD`` y puedes encontrarlos enumerados [aquí](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference). |
|HWEventHandler |El identificador de clase de la aplicación que implementa el [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler) interfaz. |
|InitCmdLine |El parámetro de cadena que desees pasar al método [Inicializar](https://docs.microsoft.com/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize) de la interfaz [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler). |

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

Si la aplicación está abierta cuando los usuarios instalen una actualización a él, se cierra la aplicación.

Si desea que esa aplicación reiniciar después de la actualización se complete, llame a la [RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) función en todos los procesos que desea reiniciar.

Cada ventana activa en la aplicación recibe un [WM_QUERYENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-queryendsession) mensaje. En este momento, la aplicación puede llamar a la [RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) función nuevo para actualizar la línea de comandos si es necesario.

Cuando cada ventana activa en la aplicación recibe el [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession) mensaje, la aplicación debe guardar los datos y apagar.

>[!NOTE]
Las ventanas activas, también recibirán el [WM_CLOSE](https://docs.microsoft.com/windows/desktop/winmsg/wm-close) del mensaje en caso de que la aplicación no controla el [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession) mensaje.

En este momento, la aplicación tiene 30 segundos para cerrar los procesos de su propio o la plataforma lo detiene forzosamente.

Una vez completada la actualización, se reinicia la aplicación.

## <a name="work-with-other-applications"></a>Trabajar con otras aplicaciones

Realiza procesos de integración con otras aplicaciones, inicia otros procesos o comparte información.

* [Hacer que la aplicación aparece como el destino de impresión en aplicaciones que admiten la impresión](#printing)
* [Compartir las fuentes con otras aplicaciones de Windows](#fonts)
* [Iniciar un proceso de Win32 desde una aplicación de plataforma Universal de Windows (UWP)](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>Hacer que la aplicación aparece como el destino de impresión en aplicaciones que admiten la impresión

Cuando los usuarios desean imprimir los datos desde otra aplicación como el Bloc de notas, puede hacer que la aplicación aparece como un destino de impresión en la lista de la aplicación de los destinos de impresión disponibles.

Tendrá que modificar la aplicación para que reciba datos de impresión en formato XML Paper Specification (XPS).

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

|Name |Descripción |
|-------|-------------|
|Category |Siempre ``windows.appPrinter``
|DisplayName |Es el nombre que quieres que aparezcan en la lista de destinos de impresión de una aplicación. |
|Parámetros |Los parámetros que requiera la aplicación para controlar correctamente la solicitud. |

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

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts).

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre ``windows.sharedFonts``
|Archivo |Es el archivo que contiene las fuentes que quieres compartir. |

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
|Category |Siempre ``windows.fullTrustProcess``
|GroupID |Cadena que identifica el conjunto de parámetros que quieres pasar al archivo ejecutable. |
|Parámetros |Parámetros que quieres pasar al archivo ejecutable. |

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

Esta extensión puede ser útil si desea crear una interfaz de usuario de plataforma Universal de Windows que se ejecuta en todos los dispositivos, pero desea que los componentes de la aplicación Win32 podrá seguir ejecutándose en plena confianza.

Ahora crearé un paquete de aplicación de Windows para la aplicación de Win32. A continuación, agrega esta extensión al archivo de paquete de la aplicación para UWP. Las extensiones de este indica que desea iniciar un archivo ejecutable en el paquete de aplicación de Windows.  Si quieres comunicarte entre la aplicación para UWP y la aplicación de Win32, puedes configurar uno o más [servicios de la aplicación](/windows/uwp/launch-resume/app-services.md) para poder hacerlo. Puedes leer más acerca de este ejemplo [aquí](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/).

## <a name="next-steps"></a>Pasos siguientes

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Proporcionar comentarios o hacer sugerencias**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
