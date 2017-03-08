---
author: awkoren
Description: "Además de las API normales disponibles para todas las aplicaciones para UWP, hay algunas extensiones y API disponibles únicamente para las aplicaciones de escritorio convertidas. En este artículo se describen dichas extensiones y cómo usarlas."
Search.Product: eADQiWindows 10XVcnh
title: Extensiones para aplicaciones de escritorio convertidas
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 66b208909e48392046c92e9fd2e689e28af7e88f
ms.lasthandoff: 02/08/2017

---

# <a name="desktop-bridge-app-extensions"></a>Extensiones para aplicaciones del Puente de dispositivo de escritorio

Una aplicación de escritorio convertida puede mejorarse con una amplia gama de API para la Plataforma universal de Windows (UWP). Sin embargo, además de las API normales disponibles para todas las aplicaciones para UWP, hay algunas extensiones y API disponibles únicamente para las aplicaciones de escritorio convertidas. Estas características se centran en escenarios como el inicio de un proceso cuando el usuario inicia sesión y la integración del Explorador de archivos y se han diseñado para suavizar la transición entre la aplicación de escritorio original y el paquete de la aplicación convertida.

En este artículo se describen dichas extensiones y cómo usarlas. La mayoría requieren una modificación manual del archivo de manifiesto de la aplicación convertida, que contiene declaraciones sobre las extensiones que usa la aplicación. Para editar el manifiesto, haz clic en el archivo **Package.appxmanifest** en la solución de Visual Studio y selecciona *Ver código*. 

## <a name="manifest-xml-namespaces"></a>Espacios de nombres XML del manifiesto 

Las extensiones de aplicaciones para el Puente de dispositivo de escritorio requieren varios espacios de nombres XML distintos. Estos espacios de nombres deben definirse en el elemento `<Package>` de la raíz del archivo de manifiesto, como este: 

```xml
<Package
  ...
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  ...
  IgnorableNamespaces="uap uap2 uap3 mp rescap desktop">
```

Si el archivo de manifiesto emite errores del compilador, comprueba que hayas incluido todos los espacios de nombres necesarios y que los elementos secundarios XML lleven el prefijo del espacio de nombres correcto. También puedes consultar un archivo de manifiesto de trabajo completo en el [Repositorio de muestras del Puente de dispositivo de escritorio en GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples). 

## <a name="startup-tasks"></a>Tareas de inicio

Las tareas de inicio permiten que tu aplicación ejecute un archivo ejecutable de forma automática cuando un usuario inicia sesión. 

Para declarar una tarea de inicio, agrega lo siguiente al manifiesto de la aplicación: 

```XML
<desktop:Extension Category="windows.startupTask" Executable="bin\MyStartupTask.exe" EntryPoint="Windows.FullTrustApplication">
    <desktop:StartupTask TaskId="MyStartupTask" Enabled="true" DisplayName="My App Service" />
</desktop:Extension>
```
- *Extension Category* siempre debe tener el valor "windows.startupTask".
- *Extension Executable* es la ruta de acceso relativa al archivo .exe que iniciar.
- *Extensión EntryPoint* siempre debe tener el valor "Windows.FullTrustApplication".
- *StartupTask TaskId* es un identificador exclusivo para la tarea. Con este identificador, la aplicación puede llamar a las API de la clase **Windows.ApplicationModel.StartupTask** para habilitar o deshabilitar una tarea de inicio mediante programación.
- *StartupTask Enabled* indica si la tarea se inicia por primera vez habilitada o deshabilitada. Las tareas habilitadas se ejecutarán la próxima vez que el usuario inicie sesión (a menos que el usuario las deshabilite). 
- *StartupTask DisplayName* es el nombre de la tarea que aparece en el Administrador de tareas. Esta cadena se puede traducir mediante ```ms-resource```. 

Las aplicaciones pueden declarar varias tareas de inicio; cada una se desencadenará y se ejecutará de forma independiente. Todas las tareas de inicio aparecerán en el Administrador de tareas en la pestaña **Inicio** con el nombre especificado en el manifiesto de la aplicación y el icono de la aplicación. El Administrador de tareas analizará automáticamente el impacto en el inicio de las tareas. Los usuarios pueden optar por deshabilitar manualmente la tarea de inicio de la aplicación mediante el Administrador de tareas; si el usuario deshabilita una tarea, no puedes volver a habilitarla mediante programación.

## <a name="app-execution-alias"></a>Alias de ejecución de la aplicación

Un alias de ejecución de la aplicación te permite especificar un nombre de palabra clave para la aplicación. Los usuarios u otros procesos podrán utilizar esta palabra clave para iniciar la aplicación fácilmente como si estuviera en la variable PATH (por ejemplo, desde Ejecutar o desde el símbolo del sistema) sin tener que proporcionar la ruta de acceso completa. Por ejemplo, si declaras el alias "Foo", el usuario puede escribir "Foo Bar.txt" en cmd.exe y la aplicación se activará con la ruta de acceso a "Bar.txt" como parte de los argumentos del evento de activación.

Para especificar un alias de ejecución de la aplicación, agrega lo siguiente al manifiesto de la aplicación: 

```XML 
<uap3:Extension Category="windows.appExecutionAlias" Executable="exes\launcher.exe" EntryPoint="Windows.FullTrustApplication">
    <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="Foo.exe" />
    </uap3:AppExecutionAlias>
</uap3:Extension>
```

- *Extension Category* siempre debe tener el valor "windows.appExecutionAlias".
- *Extensión ejecutable* es la ruta de acceso relativa al archivo ejecutable para iniciar cuando se invoque el alias.
- *Extensión EntryPont* siempre debe tener el valor "Windows.FullTrustApplication".
- *ExecutionAlias Alias* es el nombre corto de la aplicación. Siempre debe acabar con la extensión ".exe". 

Solo puedes especificar un único alias de ejecución de la aplicación para cada aplicación del paquete. Si varias aplicaciones se registran para el mismo alias, el sistema llamará a la última que se haya registrado, así que asegúrate de elegir un alias exclusivo que sea bastante improbable que otras aplicaciones sustituyan.

## <a name="protocol-associations"></a>Asociaciones de protocolos 

Las asociaciones de protocolos permiten escenarios de interoperabilidad entre la aplicación convertida y otros programas o componentes del sistema. Si la aplicación convertida se inicia mediante un protocolo, puedes especificar parámetros específicos que pasar a su argumentos de evento de activación, de forma que se comporte en consecuencia. Ten en cuenta que los parámetros solo se admiten para aplicaciones convertidas de plena confianza; las aplicaciones para UWP no pueden usar parámetros.  

Para declarar una asociación de protocolo, agrega lo siguiente al manifiesto de la aplicación:

```XML
<uap3:Extension Category="windows.protocol">
    <uap3:Protocol Name="myapp-cmd" Parameters="/p &quot;%1&quot;" />
</uap3:Extension>
```

- *Extension Category* siempre debe tener el valor "windows.protocol". 
- *Protocol Name* es el nombre del protocolo. 
- *Protocol Parameters* es la lista de parámetros y valores para pasar a la aplicación como argumentos del evento cuando se activa. Ten en cuenta que si una variable puede contener una ruta de archivo, debes encapsular dicho valor entre comillas para que no se interrumpa si se pasa una ruta de acceso que incluya espacios.

## <a name="files-and-file-explorer-integration"></a>Archivos e integración en el Explorador de archivos

Las aplicaciones convertidas ofrecen diversas opciones de registro para controlar ciertos tipos de archivo y la integración en el Explorador de archivos. Esto permite a los usuarios obtener acceso fácilmente a la aplicación como parte de su flujo de trabajo normal.

Para empezar, agrega lo siguiente al manifiesto de la aplicación: 

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="myapp">
        ... additional elements here ...
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

- *Extension Category* siempre debe tener el valor "windows.fileTypeAssociation". 
- *FileTypeAssociation Name* es un id. exclusivo. Este identificador se usa internamente para generar un ProgID con hash asociado con la asociación de tipos de archivo. Puede utilizar este identificador para administrar los cambios en versiones futuras de la aplicación. Por ejemplo, si deseas cambiar el icono de una extensión de archivo, puedes moverla a un nuevo FileTypeAssociation con un nombre diferente.  

A continuación, agrega elementos secundarios adicionales a esta entrada en función de las características específicas que necesites. A continuación se describen las opciones disponibles.

### <a name="supported-file-types"></a>Tipos de archivo admitidos

La aplicación puede especificar que admite la apertura de determinados tipos de archivos. Si un usuario hace clic con el botón secundario en un archivo y selecciona "Abrir con", la aplicación aparecerá en la lista de sugerencias.

Ejemplo:

```XML
<uap:SupportedFileTypes>
    <uap:FileType>.txt</uap:FileType>
    <uap:FileType>.avi</uap:FileType>
</uap:SupportedFileTypes>
```

- *FileType* es la extensión que la aplicación admite.

### <a name="file-context-menu-verbs"></a>Verbos de menú contextual de archivo 

Normalmente, para abrir archivos, los usuarios simplemente hacen doble clic en ellos. Sin embargo, si un usuario hacer clic con el botón secundario en un archivo, el menú contextual le presenta diversas opciones (conocidas como "Verbos") que proporcionan detalles adicionales sobre cómo desea interactuar con dicho archivo, como "Abrir", "Editar", "Vista previa" o "Imprimir". 

Especificar un tipo de archivo admitido agrega automáticamente el verbo "Abrir". Sin embargo, las aplicaciones también pueden agregar verbos personalizados adicionales en el menú contextual del Explorador de archivos. Estos permiten a la aplicación iniciarse de una determinada manera en función de la elección del usuario al abrir un archivo.

Ejemplo: 

```XML
<uap2:SupportedVerbs>
    <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
    <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
</uap2:SupportedVerbs>
```

- *Verb Id* es un id. exclusivo del verbo. Si la aplicación es una aplicación para UWP, este se pasa a la aplicación como parte de los argumentos del evento de activación para que pueda controlar la elección del usuario correctamente. Si la aplicación es una aplicación de plena confianza convertida, en su lugar recibe parámetros (consulta el siguiente punto). 
- *Verb Parameters* es la lista de parámetros de argumento y valores asociados con el verbo. Si la aplicación es una aplicación de plena confianza convertida, estos se pasan a él como argumentos del evento cuando se activa, para que puedas personalizar su comportamiento para verbos de activación distintos. Si una variable puede contener una ruta de archivo, debes encapsular dicho valor entre comillas para que no se interrumpa si se pasa una ruta de acceso que incluya espacios. Ten en cuenta que si la aplicación es una aplicación para UWP, no se pueden pasar parámetros; en su lugar, la aplicación recibe el identificador (consulta el punto anterior). 
- *Verb extended* especifica que el verbo solo debe aparecer si el usuario mantiene presionada la tecla **Mayús** antes de hacer clic con el botón derecho en el archivo para mostrar el menú contextual. Este atributo es opcional y su valor predeterminado es *False* (por ejemplo, mostrar siempre el verbo) si no se incluye. Este comportamiento se especifica de forma individual para cada verbo (excepto "Abrir", que siempre es *False*). 
- *Verb* es el nombre para mostrar en el menú contextual del Explorador de archivos. Esta cadena se puede traducir mediante ```ms-resource```.

### <a name="shell-context-menu-verbs"></a>Verbos de menú contextual de shell

Por el momento, no se admite agregar elementos al menú contextual de carpetas de shell. 

### <a name="multiple-selection-model"></a>Modelo de selección múltiple

Varias selecciones permiten especificar cómo la aplicación controla que un usuario abra simultáneamente varios archivos con ella (por ejemplo, si selecciona 10 archivos en el Explorador de archivos y pulsa "Abrir").

Las aplicaciones de escritorio convertidas tienen las mismas tres opciones que las aplicaciones de escritorio normales. 
- *Player*: La aplicación se activa una sola vez con todos los archivos seleccionados pasados como parámetros de argumento.
- *Single*: La aplicación se activa una vez para el primer archivo seleccionado. Otros archivos se omiten. 
- *Document*: Se activa una nueva instancia independiente de la aplicación para cada archivo seleccionado.

Puedes establecer preferencias diferentes para distintos tipos de archivo y acciones. Por ejemplo, si deseas abrir *documentos* en el modo *Document* y las *imágenes* en el modo *Player*.

Para establecer el comportamiento de la aplicación, agrega el atributo *MultiSelectModel* a los elementos del manifiesto que estén relacionados con los tipos de archivo y el inicio de archivos. 

Establecer un modelo para un tipo de archivo admitido: 

```XML
<uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
    <uap:SupportedFileTypes>
        <uap:FileType>.txt</uap:FileType>
</uap:SupportedFileTypes>
```

Establecer un modelo para verbos:

```XML
<uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
<uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
```

Si la aplicación no especifica una opción para la selección múltiple, el valor predeterminado es *Player* si el usuario abre 15 archivos o menos. En caso de que se trate de una aplicación convertida, el valor predeterminado es *Document*. Las aplicaciones para UWP siempre se inician como *Player*. 

### <a name="complete-example"></a>Ejemplo completo

A continuación mostramos un ejemplo completo que integra muchos de los elementos relacionados con archivos y con el Explorador de archivos descritos anteriormente: 

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
        <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
            <uap:FileType>.foo</uap:FileType>
    </uap:SupportedFileTypes>
    <uap2:SupportedVerbs>
            <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
            <uap3:Verb Id="Print" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
    </uap2:SupportedVerbs>
    <uap:Logo>Assets\MyExtensionLogo.png</uap:Logo>
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

## <a name="see-also"></a>Consulta también

- [Manifiesto del paquete de la aplicación](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)
