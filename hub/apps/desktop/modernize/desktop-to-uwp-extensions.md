---
description: Puedes usar extensiones para integrar tu aplicación de escritorio empaquetada con Windows 10 de formas predefinidas.
title: Modernización de aplicaciones de escritorio existentes con Puente de dispositivo de escritorio
ms.date: 09/11/2020
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 1338b45dd5ccc7558725dd1ad1d210f3c5e7232b
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92132998"
---
# <a name="integrate-your-desktop-app-with-windows-10-and-uwp"></a>Integración de la aplicación de escritorio con Windows 10 y UWP

Si la aplicación de escritorio tiene [identidad de paquete](modernize-packaged-apps.md), puedes usar extensiones para integrar la aplicación con Windows 10 mediante las extensiones predefinidas en el [manifiesto del paquete](/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Por ejemplo, usa una extensión para crear una excepción de firewall, hacer que la aplicación sea la aplicación predeterminada para un tipo de archivo o incluir iconos de inicio de la aplicación. Para usar una extensión, solo tienes que agregar XML al archivo de manifiesto de paquete de la aplicación. No se requiere ningún tipo de código.

En este artículo se describen estas extensiones y las tareas que puedes realizar al usarlas.

> [!NOTE]
> En las características que se describen en este artículo se requiere que la aplicación de escritorio tenga [identidad de paquete](modernize-packaged-apps.md), bien mediante el [empaquetado de la aplicación de escritorio en un paquete MSIX](/windows/msix/desktop/desktop-to-uwp-root) o mediante la [concesión de la identidad de la aplicación con un paquete disperso](grant-identity-to-nonpackaged-apps.md).

## <a name="transition-users-to-your-app"></a>Proceso de transición de usuarios a la aplicación

Para ayudar a los usuarios con la transición a tu aplicación empaquetada, puedes realizar los pasos siguientes:

* [Incluir los iconos de inicio y los botones de la barra de tareas existentes en la aplicación empaquetada](#point)
* [Hacer que abra los archivos la aplicación empaquetada en lugar de la aplicación de escritorio](#make)
* [Asociar la aplicación empaquetada con un conjunto de tipos de archivo](#associate)
* [Agregar opciones en los menús contextuales de los archivos que tienen un tipo de archivo específico](#add)
* [Abrir determinados tipos de archivos directamente con una dirección URL](#open)

<a id="point"></a>

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>Incluir los iconos de inicio y los botones de la barra de tareas existentes en la aplicación empaquetada

Es posible que los usuarios hayan anclado la aplicación de escritorio en la barra de tareas o el menú Inicio. Puedes incluir esos accesos directos en la nueva aplicación empaquetada.

#### <a name="xml-namespace"></a>Espacio de nombres XML

`http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration).

|Nombre | Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.desktopAppMigration``.
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

<a id="make"></a>

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>Hacer que abra los archivos la aplicación empaquetada en lugar de la aplicación de escritorio

Puedes asegurarte de que los usuarios abren de forma predeterminada la nueva aplicación empaquetada y no la versión de escritorio, cuando abren ciertos tipos de archivos.

Para ello, especifica el [identificador de programación (ProgID)](/windows/desktop/shell/fa-progids) de cada aplicación de la cual quieras heredar asociaciones de archivos.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``.
|Nombre |Nombre de la asociación de tipo de archivo. Puedes usar este nombre para organizar y agrupar tipos de archivo. El nombre debe contener todos los caracteres en minúsculas y sin espacios. |
|MigrationProgId |[Identificador de programación (ProgID)](/windows/desktop/shell/fa-progids) que describe la aplicación, el componente y la versión de la aplicación de escritorio de la que quieres heredar asociaciones de archivos.|

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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

<a id="associate"></a>

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>Asociar la aplicación empaquetada con un conjunto de tipos de archivo

Puedes asociar la aplicación empaquetada con extensiones de tipo de archivo. Si un usuario hace clic con el botón derecho y selecciona la opción **Abrir con**, la aplicación aparece en la lista de sugerencias.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``.
|Nombre | Nombre de la asociación de tipo de archivo. Puedes usar este nombre para organizar y agrupar tipos de archivo. El nombre debe contener todos los caracteres en minúsculas y sin espacios.   |
|FileType |Extensión de archivo compatible con la aplicación. |

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
          <uap3:FileTypeAssociation Name="mediafiles">
            <uap:SupportedFileTypes>
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

<a id="add"></a>

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>Agregar opciones en los menús contextuales de los archivos que tienen un tipo de archivo específico

En la mayoría de los casos, los usuarios hacen doble clic en los archivos para abrirlos. Si hacen clic con el botón derecho en un archivo, aparecen varias opciones.

Puedes agregar opciones a ese menú. Estas opciones ofrecen a los usuarios diferentes formas de interactuar con el archivo como, por ejemplo, imprimirlo, editarlo u obtener una vista previa.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría | Siempre ``windows.fileTypeAssociation``.
|Nombre |Nombre de la asociación de tipo de archivo. Puedes usar este nombre para organizar y agrupar tipos de archivo. El nombre debe contener todos los caracteres en minúsculas y sin espacios. |
|Verb |Nombre que aparece en el menú contextual del Explorador de archivos. Esta cadena se puede localizar mediante ```ms-resource```.|
|Id |Identificador único del verbo. Si la aplicación es una aplicación para UWP, este se pasa a la aplicación como parte de los argumentos del evento de activación para poder controlar la elección del usuario correctamente. Si la aplicación es una aplicación empaquetada de plena confianza, en su lugar recibe parámetros (consulta el siguiente punto). |
|Parámetros |Lista de valores y parámetros de argumento asociados con el verbo. Si la aplicación es una aplicación empaquetada de plena confianza, estos parámetros se pasan a la aplicación como argumentos de evento cuando esta se activa. Puedes personalizar el comportamiento de la aplicación en función de los distintos verbos de activación. Si una variable puede contener una ruta de acceso de archivo, escribe el valor del parámetro entre comillas. Así evitarás cualquier problema si la ruta de acceso incluye espacios. Si la aplicación es una aplicación para UWP, no se pueden pasar parámetros. En su lugar, la aplicación recibe el identificador (consulta el punto anterior).|
|Extendido |Especifica que el verbo solo aparece si el usuario mantiene presionada la tecla **Mayús** para mostrar el menú contextual, antes de hacer clic con el botón derecho en el archivo. Este atributo es opcional y su valor predeterminado es **False** (por ejemplo, mostrar siempre el verbo) si no se incluye. Este comportamiento se especifica de forma individual para cada verbo (excepto "Abrir", que siempre es **False**).|

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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

<a id="open"></a>

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Abrir determinados tipos de archivos directamente con una dirección URL

Puedes asegurarte de que los usuarios abren de forma predeterminada la nueva aplicación empaquetada y no la versión de escritorio, cuando abren ciertos tipos de archivos.

#### <a name="xml-namespaces"></a>Espacios de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.fileTypeAssociation``.
|Nombre |Nombre de la asociación de tipo de archivo. Puedes usar este nombre para organizar y agrupar tipos de archivo. El nombre debe contener todos los caracteres en minúsculas y sin espacios. |
|UseUrl |Indica si se deben abrir archivos directamente desde una dirección URL de destino. Si no estableces este valor, cualquier intento que realice la aplicación para abrir un archivo mediante una URL, provocará que el sistema descargue el archivo de forma local. |
|Parámetros | Parámetros opcionales. |
|FileType |Extensiones de archivo pertinentes. |

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
            <uap3:FileTypeAssociation Name="myfiletypes" UseUrl="true" Parameters="%1">
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

## <a name="perform-setup-tasks"></a>Realización de tareas de configuración

* [Crear la excepción de firewall de la aplicación](#rules)
* [Colocar los archivos DLL en cualquier carpeta del paquete](#load-paths)

<a id="rules"></a>

### <a name="create-firewall-exception-for-your-app"></a>Crear la excepción de firewall de la aplicación

Si la aplicación debe comunicarse a través de un puerto, puedes agregarla a la lista de excepciones del firewall.

#### <a name="xml-namespace"></a>Espacio de nombres XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

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

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules).

|Nombre |Descripción |
|-------|-------------|
|Categoría |Siempre ``windows.firewallRules``.|
|Ejecutable |Nombre del archivo ejecutable que quieres agregar a la lista de excepciones del firewall. |
|Dirección |Indica si la regla es entrante o saliente. |
|IPProtocol |Protocolo de comunicación. |
|LocalPortMin |Número de puerto más bajo en un intervalo de números de puerto local. |
|LocalPortMax |Número de puerto más alto de un intervalo de números de puerto local. |
|RemotePortMax |Número de puerto más bajo en un intervalo de números de puerto remoto. |
|RemotePortMax |Número de puerto más alto en un intervalo de números de puerto remoto. |
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

<a id="load-paths"></a>

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>Colocación de archivos DLL en cualquier carpeta del paquete

Use la extensión [uap6:LoaderSearchPathOverride](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride) para declarar hasta cinco rutas de acceso de carpeta en el paquete de la aplicación, relacionadas con la ruta de acceso raíz del paquete de la aplicación, que se utilizarán en la ruta de búsqueda del cargador para los procesos de la aplicación.

El [orden de búsqueda de DLL](/windows/win32/dlls/dynamic-link-library-search-order) para aplicaciones de Windows incluye paquetes en el gráfico de dependencias de paquete si los paquetes tienen derechos de ejecución. De forma predeterminada, incluye los paquetes principales, opcionales y de marco, aunque el elemento [uap6:AllowExecution](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution) del manifiesto del paquete puede sobrescribirlo.

De forma predeterminada, un paquete que se incluye en el orden de búsqueda de DLL incluirá su *ruta de acceso efectiva*. Para obtener más información sobre las rutas de acceso efectivas, consulte la propiedad [EffectivePath](/uwp/api/windows.applicationmodel.package.effectivepath) (WinRT) y la enumeración [PackagePathType](/windows/win32/api/appmodel/ne-appmodel-packagepathtype) (Win32).

Si un paquete especifica [uap6:LoaderSearchPathOverride](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride), esta información se utiliza en lugar de la ruta de acceso efectiva del paquete.

Cada paquete puede contener solo una extensión [uap6:LoaderSearchPathOverride](/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-loadersearchpathoverride). Significa que puedes agregar una de estas al paquete principal y, a continuación, agregar una a cada uno de tus [paquetes opcionales y conjuntos relacionados](/windows/msix/package/optional-packages).

#### <a name="xml-namespace"></a>espacio de nombres XML

`http://schemas.microsoft.com/appx/manifest/uap/windows10/6`

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
|Category |Siempre es ``windows.loaderSearchPathOverride``.
|FolderPath | Ruta de acceso de la carpeta que contiene sus archivos DLL. Especifica una ruta de acceso relativa a la carpeta raíz del paquete. En una extensión puedes especificar hasta cinco rutas de acceso. Si quieres que el sistema busque archivos en la carpeta raíz del paquete, usa una cadena vacía para una de estas rutas de acceso. No incluya rutas de acceso duplicadas y asegúrese de que las rutas de acceso no contengan barras diagonales iniciales o finales ni barras diagonales inversas. <br><br> El sistema no buscará en subcarpetas; por tanto, asegúrate de indicar explícitamente cada carpeta que contenga archivos DLL y que quieras que cargue el sistema.|

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

## <a name="integrate-with-file-explorer"></a>Integración con el Explorador de archivos

Ayuda a los usuarios a organizar tus archivos y a interactuar con estos de la forma habitual.

* [Definir el comportamiento de la aplicación cuando los usuarios seleccionan y abren varios archivos al mismo tiempo](#define)
* [Mostrar el contenido del archivo en una imagen en miniatura en el Explorador de archivos](#show)
* [Mostrar el contenido del archivo en un panel de vista previa del Explorador de archivos](#preview)
* [Permitir que los usuarios agrupen los archivos mediante la columna Tipo del Explorador de archivos](#enable)
* [Hacer que las propiedades de archivo tengan disponibles las opciones de búsqueda, índice, diálogos de propiedad y panel de detalles](#make-file-properties)
* [Especificar un controlador de menú contextual para un tipo de archivo](#context-menu)
* [Hacer que los archivos del servicio en la nube aparezcan en el Explorador de archivos](#cloud-files)

<a id="define"></a>

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Definir el comportamiento de la aplicación cuando los usuarios seleccionan y abren varios archivos al mismo tiempo

Especifica cómo se debe comportar la aplicación cuando el usuario abra varios archivos al mismo tiempo.

#### <a name="xml-namespaces"></a>espacios de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre es ``windows.fileTypeAssociation``.
|Nombre |Nombre de la asociación de tipo de archivo. Puedes usar este nombre para organizar y agrupar tipos de archivo. El nombre debe contener todos los caracteres en minúsculas y sin espacios. |
|MultiSelectModel |Consulte a continuación |
|FileType |Extensiones de archivo pertinentes. |

**MultiSelectModel**

Las aplicaciones de escritorio empaquetadas tienen las mismas tres opciones que las aplicaciones de escritorio normales.

* ``Player``: la aplicación se activa una vez. Todos los archivos seleccionados se pasan a la aplicación como parámetros de argumento.
* ``Single``: la aplicación se activa una vez para el primer archivo seleccionado. Otros archivos se omiten.
* ``Document``: se activa una nueva instancia independiente de la aplicación para cada archivo seleccionado.

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
          <uap3:FileTypeAssociation Name="myfiletypes" MultiSelectModel="Document">
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

Si el usuario abre 15 archivos o menos, la opción predeterminada del atributo **MultiSelectModel** es *Player*. De lo contrario, el valor predeterminado es *Document*. Las aplicaciones para UWP siempre se inician como *Player*.

<a id="show"></a>

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>Mostrar el contenido del archivo en una imagen en miniatura en el Explorador de archivos

Permite que los usuarios vean una imagen en miniatura del contenido del archivo cuando aparece el icono del archivo con un tamaño medio, grande o extra grande.

#### <a name="xml-namespace"></a>espacio de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre es ``windows.fileTypeAssociation``.
|Nombre |Nombre de la asociación de tipo de archivo. Puedes usar este nombre para organizar y agrupar tipos de archivo. El nombre debe contener todos los caracteres en minúsculas y sin espacios. |
|FileType |Extensiones de archivo pertinentes. |
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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

<a id="preview"></a>

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>Mostrar el contenido del archivo en el panel de vista previa del Explorador de archivos

Permite que los usuarios obtengan una vista previa del contenido de un archivo en el panel de vista previa del Explorador de archivos.

#### <a name="xml-namespace"></a>espacio de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre es ``windows.fileTypeAssociation``.
|Nombre |Nombre de la asociación de tipo de archivo. Puedes usar este nombre para organizar y agrupar tipos de archivo. El nombre debe contener todos los caracteres en minúsculas y sin espacios. |
|FileType |Extensiones de archivo pertinentes. |
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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

<a id="enable"></a>

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>Permitir que los usuarios agrupen los archivos mediante la columna Tipo del Explorador de archivos

Puedes asociar uno o más valores predefinidos de los tipos de archivo mediante el campo **Kind**.

En el Explorador de archivos, los usuarios pueden agrupar los archivos mediante ese campo. Los componentes del sistema también usan este campo para diversos fines, como la indexación.

Para obtener más información sobre el campo **Kind** y los valores que puedes usar en este campo, consulta [Usar nombres del campo Kind](/windows/desktop/properties/building-property-handlers-user-friendly-kind-names).

#### <a name="xml-namespaces"></a>espacios de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre es ``windows.fileTypeAssociation``.
|Nombre |Nombre de la asociación de tipo de archivo. Puedes usar este nombre para organizar y agrupar tipos de archivo. El nombre debe contener todos los caracteres en minúsculas y sin espacios. |
|FileType |Extensiones de archivo pertinentes. |
|value |[Valor de Kind](/windows/desktop/properties/building-property-handlers-user-friendly-kind-names) válido. |

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
           <uap:FileTypeAssociation Name="mediafiles">
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

<a id="make-file-properties"></a>

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>Hacer que las propiedades de archivo tengan disponibles las opciones de búsqueda, índice, diálogos de propiedad y panel de detalles

#### <a name="xml-namespace"></a>espacio de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre es ``windows.fileTypeAssociation``.
|Nombre |Nombre de la asociación de tipo de archivo. Puedes usar este nombre para organizar y agrupar tipos de archivo. El nombre debe contener todos los caracteres en minúsculas y sin espacios. |
|FileType |Extensiones de archivo pertinentes. |
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
          <uap3:FileTypeAssociation Name="myfiletypes">
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

<a id="context-menu"></a>

### <a name="specify-a-context-menu-handler-for-a-file-type"></a>Especificar un controlador de menú contextual para un tipo de archivo

Si la aplicación de escritorio define un [controlador de menú contextual](/windows/desktop/shell/context-menu-handlers), utiliza esta extensión para registrar el controlador de menú.

#### <a name="xml-namespaces"></a>espacios de nombres XML

* `http://schemas.microsoft.com/appx/manifest/foundation/windows10`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/4`

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

Busca la referencia de esquema completa aquí: [com:COMServer](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) y [desktop4:FileExplorerContextMenus](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus).

#### <a name="instructions"></a>Instructions

Para registrar el controlador de menú contextual, sigue estas instrucciones.

1. En la aplicación de escritorio, implementa un [controlador de menú contextual](/windows/desktop/shell/context-menu-handlers) mediante la implementación de la interfaz [IExplorerCommand](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) o [IExplorerCommandState](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommandstate). Para obtener un ejemplo, consulta el ejemplo de código [ExplorerCommandVerb](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb). Asegúrate de definir un GUID de clase para cada uno de los objetos de implementación. Por ejemplo, el código siguiente define un identificador de clase para una implementación de [IExplorerCommand](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand).

    ```cpp
    class __declspec(uuid("d0c8bceb-28eb-49ae-bc68-454ae84d6264")) CExplorerCommandVerb;
    ```

2. En el manifiesto del paquete, especifica una extensión de aplicación [com:ComServer](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) que registre un servidor suplente COM con el identificador de clase de la implementación del controlador del menú contextual.

    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler">
                <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

2. En el manifiesto del paquete, especifica una extensión de aplicación [desktop4:FileExplorerContextMenus](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) que registre la implementación del controlador del menú contextual.

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

<a id="cloud-files"></a>

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>Hacer que los archivos del servicio en la nube aparezcan en el Explorador de archivos

Registra los controladores que implementas en la aplicación. También puedes agregar opciones de menú contextual que aparezcan cuando los usuarios hagan clic con el botón derecho en los archivos en la nube en el Explorador de archivos.

#### <a name="xml-namespace"></a>espacio de nombres XML

* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

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
|Category |Siempre es ``windows.cloudfiles``.
|iconResource |El icono que representa tu servicio de proveedor de archivos en la nube. Este icono aparece en el panel Navegación del Explorador de archivos.  Los usuarios eligen este icono para mostrar archivos desde tu servicio en la nube. |
|Clsid CustomStateHandler |Identificador de clase de la aplicación que implementa el objeto CustomStateHandler. El sistema usa este identificador de clase para solicitar estados personalizados y columnas para archivos de la nube. |
|Clsid ThumbnailProviderHandler |Identificador de clase de la aplicación que implementa el objeto ThumbnailProviderHandler. El sistema usa este identificador de clase para solicitar imágenes en miniatura para archivos de la nube. |
|Clsid ExtendedPropertyHandler |Identificador de clase de la aplicación que implementa el objeto ExtendedPropertyHandler.  El sistema usa este identificador de clase para solicitar propiedades ampliadas para un archivo de la nube. |
|Verbo |Nombre que aparece en el menú contextual del Explorador de archivos para los archivos que proporciona el servicio en la nube. |
|Identificador |Identificador único del verbo. |

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

<a id="start"></a>

## <a name="start-your-application-in-different-ways"></a>Iniciar la aplicación de formas diferentes

* [Iniciar la aplicación mediante un protocolo](#protocol)
* [Iniciar la aplicación mediante un protocolo](#alias)
* [Iniciar un archivo ejecutable cuando los usuarios inicien sesión en Windows](#executable)
* [Permitir que los usuarios inicien la aplicación cuando conecten un dispositivo a su PC](#autoplay)
* [Reiniciar automáticamente después de recibir una actualización de Microsoft Store](#updates)

<a id="protocol"></a>

### <a name="start-your-application-by-using-a-protocol"></a>Iniciar la aplicación mediante un protocolo

Las asociaciones de protocolos permiten que otros programas y componentes del sistema interactúen con la aplicación empaquetada. Si la aplicación empaquetada se inicia mediante un protocolo, puedes especificar parámetros específicos para pasarlos a sus argumentos de evento de activación, de forma que se comporte en consecuencia. Los parámetros solo son compatibles con aplicaciones empaquetadas de plena confianza. Las aplicaciones para UWP no pueden usar parámetros.

#### <a name="xml-namespace"></a>espacio de nombres XML

`http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol).

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre es ``windows.protocol``.
|Nombre |El nombre del protocolo |
|Parámetros |Lista de parámetros y valores para pasar a la aplicación como argumentos del evento cuando esta se activa. Si una variable puede contener una ruta de acceso de archivo, escribe el valor del parámetro entre comillas. Así evitarás cualquier problema si la ruta de acceso incluye espacios. |

### <a name="example"></a>Ejemplo

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
```

<a id="alias"></a>

### <a name="start-your-application-by-using-an-alias"></a>Iniciar la aplicación mediante un alias

Tanto los usuarios como otros procesos pueden usar un alias para iniciar la aplicación sin tener que especificar su ruta de acceso completa. Puedes especificar el nombre de ese alias.

#### <a name="xml-namespaces"></a>espacios de nombres XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<uap3:Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="[AliasName]" />
    </uap3:AppExecutionAlias>
</uap3:Extension>
```

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre es ``windows.appExecutionAlias``.
|Executable |Ruta de acceso relativa al archivo ejecutable que se iniciará al invocar el alias. |
|Alias |Nombre corto de la aplicación. Siempre debe acabar con la extensión ".exe". Solo puedes especificar un único alias de ejecución de la aplicación para cada aplicación del paquete. Si varias aplicaciones se registran para el mismo alias, el sistema llamará a la última que se haya registrado, así que asegúrate de elegir un alias exclusivo que sea bastante improbable que otras aplicaciones sustituyan.
|

#### <a name="example"></a>Ejemplo

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

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

<a id="executable"></a>

### <a name="start-an-executable-file-when-users-log-into-windows"></a>Iniciar un archivo ejecutable cuando los usuarios inicien sesión en Windows

Las tareas de inicio permiten que tu aplicación ejecute un archivo ejecutable de forma automática cuando un usuario inicia sesión.

> [!NOTE]
> El usuario tiene que iniciar la aplicación al menos una vez para registrar esta tarea de inicio.

La aplicación puede declarar varias tareas de inicio. Cada tarea se inicia de forma independiente. Todas las tareas de inicio aparecerán en el Administrador de tareas bajo la pestaña **Inicio** con el nombre especificado en el manifiesto de la aplicación y el icono de la aplicación. El Administrador de tareas analizará automáticamente el impacto en el inicio de las tareas.

Los usuarios pueden deshabilitar manualmente la tarea de inicio de la aplicación mediante el Administrador de tareas. Si un usuario deshabilita una tarea, no podrá volver a habilitarla mediante programación.

#### <a name="xml-namespace"></a>espacio de nombres XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

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
|Category |Siempre es ``windows.startupTask``.|
|Executable |Ruta de acceso relativa del archivo ejecutable que se va a iniciar. |
|TaskId |Identificador único de la tarea. Con este identificador, la aplicación puede llamar a las API de la clase [Windows.ApplicationModel.StartupTask](/uwp/api/Windows.ApplicationModel.StartupTask) para habilitar o deshabilitar una tarea de inicio mediante programación. |
|Habilitado |Indica si la tarea se inicia habilitada o deshabilitada. Las tareas habilitadas se ejecutarán la próxima vez que el usuario inicie sesión (a menos que el usuario las deshabilite). |
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

<a id="autoplay"></a>

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>Permitir que los usuarios inicien la aplicación cuando conecten un dispositivo a su PC

Reproducción automática puede presentar tu aplicación como una opción cuando un usuario conecte un dispositivo a su PC.

#### <a name="xml-namespace"></a>espacio de nombres XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/3`

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
|Category |Siempre es ``windows.autoPlayHandler``.
|ActionDisplayName |Cadena que representa la acción que los usuarios pueden realizar con un dispositivo que conectan a un equipo (por ejemplo: "Importar archivos" o "Reproducir vídeo"). |
|ProviderDisplayName | Cadena que representa tu aplicación o servicio (por ejemplo: "Reproductor de vídeo Contoso"). |
|ContentEvent |Nombre de un evento de contenido que hace que a los usuarios les aparezca tu ``ActionDisplayName`` y ``ProviderDisplayName``. Se genera un evento de contenido cuando se inserta en el equipo un dispositivo con volumen, como una tarjeta de memoria de cámara, una unidad USB o un DVD. Puedes encontrar la lista completa de esos eventos [aquí](/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference).  |
|Verbo |La opción de configuración Verbo identifica un valor que se pasa a la aplicación para la opción seleccionada. Puedes especificar varias acciones de inicio para un evento de Reproducción automática y usar la configuración Verbo para determinar qué opción seleccionó un usuario para tu aplicación. Para saber qué opción seleccionó el usuario, comprueba la propiedad verb de los argumentos del evento de inicio que se pasaron a la aplicación. Puedes usar cualquier valor para la configuración Verbo a excepción de open, que está reservado. |
|DropTargetHandler |Identificador de clase de la aplicación que implementa la interfaz [IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget). Los archivos del medio extraíble se pasan al método [Colocar](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) de tu implementación [IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget).  |
|Parámetros |No tienes que implementar la interfaz [IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget) para todos los eventos de contenido. Para cualquiera de los eventos de contenido, podrías proporcionar los parámetros de línea de comandos en lugar de implementar la interfaz [IDropTarget](/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget). Para esos eventos, Reproducción automática iniciará tu aplicación utilizando los parámetros de línea de comandos. Puedes analizar esos parámetros en el código de inicialización de la aplicación para determinar si se inició mediante Reproducción automática y, a continuación, proporcionar tu implementación personalizada. |
|DeviceEvent |Nombre de un evento de dispositivo que hace que a los usuarios les aparezca tu ``ActionDisplayName`` y ``ProviderDisplayName``. Se genera un evento de dispositivo cuando se conecta un dispositivo al equipo. Los eventos de dispositivo comienzan con la cadena ``WPD`` y puedes encontrarlos en una lista [aquí](/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference). |
|HWEventHandler |Identificador de clase de la aplicación que implementa la interfaz [IHWEventHandler](/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler). |
|InitCmdLine |Parámetro de cadena que quieres pasar al método [Inicializar](/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize) de la interfaz [IHWEventHandler](/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler). |

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

<a id="updates"></a>

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Reiniciar automáticamente después de recibir una actualización de Microsoft Store

Si la aplicación está abierta cuando los usuarios instalan una actualización de esta, se cerrará.

Si quieres que esa aplicación se reinicie una vez que se complete la actualización, llama a la función [RegisterApplicationRestart](/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) en cada proceso que quieras reiniciar.

Cada ventana activa de la aplicación recibe un mensaje [WM_QUERYENDSESSION](/windows/desktop/Shutdown/wm-queryendsession). En este punto, la aplicación puede llamar a la función [RegisterApplicationRestart](/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) de nuevo para actualizar la línea de comandos si es necesario.

Cuando cada ventana activa de la aplicación recibe el mensaje [WM_ENDSESSION](/windows/desktop/Shutdown/wm-endsession), la aplicación debe guardar los datos y apagarse.

>[!NOTE]
> Las ventanas activas también reciben el mensaje [WM_CLOSE](/windows/desktop/winmsg/wm-close) en caso de que la aplicación no controle el mensaje [WM_ENDSESSION](/windows/desktop/Shutdown/wm-endsession).

En este punto, la aplicación tiene 30 segundos para cerrar sus procesos o la plataforma los cierra a la fuerza.

Una vez completada la actualización, se reinicia la aplicación.

## <a name="work-with-other-applications"></a>Trabajar con otras aplicaciones

Realiza procesos de integración con otras aplicaciones, inicia otros procesos o comparte información.

* [Hacer que la aplicación aparezca como destino de impresión en aplicaciones que admiten funciones de impresión](#printing)
* [Compartir fuentes con otras aplicaciones de Windows](#fonts)
* [Iniciar un proceso de Win32 desde una aplicación para la Plataforma universal de Windows (UWP)](#win32-process)

<a id="printing"></a>

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>Hacer que la aplicación aparezca como destino de impresión en aplicaciones que admiten funciones de impresión

Cuando los usuarios quieran imprimir datos desde otra aplicación como, por ejemplo, el Bloc de notas de Windows, puedes hacer que la aplicación aparezca como destino de impresión en la lista de destinos de impresión disponibles correspondiente.

Tendrás que modificar la aplicación para que reciba datos de impresión en formato XML Paper Specification (XPS).

#### <a name="xml-namespaces"></a>espacios de nombres XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos y atributos de esta extensión

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

Puedes encontrar la referencia de esquema completa [aquí](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter).

|Nombre |Descripción |
|-------|-------------|
|Category |Siempre es ``windows.appPrinter``.
|DisplayName |Nombre que quieres que aparezca en la lista de destinos de impresión de una aplicación. |
|Parámetros |Cualquier parámetro que la aplicación necesite para controlar correctamente la solicitud. |

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

Puedes encontrar un ejemplo que usa esta extensión [aquí](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF).

<a id="fonts"></a>

### <a name="share-fonts-with-other-windows-applications"></a>Compartir fuentes con otras aplicaciones de Windows

Comparte tus fuentes personalizadas con otras aplicaciones de Windows.

> [!NOTE]
> Para poder enviar una aplicación que use esta extensión a Store, primero debe obtener la aprobación del equipo de Store. Para obtenerla, vaya a [https://aka.ms/storesupport](https://aka.ms/storesupport), haga clic en **Contactar con nosotros** y elija las opciones relacionadas con el envío de aplicaciones al panel. Este proceso de aprobación ayuda a garantizar que no haya conflictos entre las fuentes instaladas por la aplicación y las fuentes que se instalan con el sistema operativo. Si no obtiene la aprobación, recibirá un error similar al siguiente al enviar la aplicación: "Error al validar la aceptación del paquete: No se puede usar la extensión windows.sharedFonts con esta cuenta. Póngase en contacto con nuestro equipo de soporte técnico si quiere solicitar permisos para usar esta extensión".

#### <a name="xml-namespaces"></a>espacios de nombres XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

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
|Category |Siempre es ``windows.sharedFonts``.
|Archivo |Archivo que contiene las fuentes que quieres compartir. |

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

<a id="win32-process"></a>

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>Iniciar un proceso de Win32 desde una aplicación para la Plataforma universal de Windows (UWP)

Inicia un proceso de Win32 que se ejecute en plena confianza.

#### <a name="xml-namespaces"></a>espacios de nombres XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

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
|Category |Siempre es ``windows.fullTrustProcess``.
|GroupID |Cadena que identifica un conjunto de parámetros que quieres pasar al archivo ejecutable. |
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

Esta extensión puede resultar útil si quieres crear una interfaz de usuario de la Plataforma universal de Windows que se ejecute en todos los dispositivos y, a su vez, que los componentes de la aplicación de Win32 sigan realizando una ejecución de plena confianza.

Solo tienes que crear un paquete de aplicación de Windows para la aplicación de Win32. A continuación, agrega esta extensión al archivo de paquete de la aplicación para UWP. Estas extensiones indican que quieres iniciar un archivo ejecutable en el paquete de la aplicación de Windows.  Si quieres establecer la comunicación entre la aplicación para UWP y la aplicación de Win32, puedes configurar uno o más [servicios de la aplicación](/windows/uwp/launch-resume/app-services) para poder hacerlo. Puedes obtener más información acerca de este escenario [aquí](/archive/blogs/appconsult/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app).

## <a name="next-steps"></a>Pasos siguientes

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
