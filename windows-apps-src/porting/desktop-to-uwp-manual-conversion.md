---
author: normesta
Description: Shows how to manually package a Windows desktop application (like Win32, WPF, and Windows Forms) for Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: Empaquetar una aplicación de forma manual (puente de escritorio)
ms.author: normesta
ms.date: 05/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.openlocfilehash: 9f14e7f8747639ef139e774416e09af954211940
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4617704"
---
# <a name="package-a-desktop-application-manually"></a>Empaquetar una aplicación de escritorio de forma manual

Este tema muestra cómo empaquetar la aplicación sin tener que usar herramientas como Visual Studio o Desktop App Converter (DAC).

Para empaquetar la aplicación de forma manual, tienes que crear un archivo de manifiesto de paquete y ejecutar una herramienta de línea de comandos para generar un paquete de la aplicación de Windows.

Considera la posibilidad de empaquetado manual si instala la aplicación mediante el comando xcopy o estás familiarizado con los cambios que realiza el instalador de la aplicación en el sistema y quieres un control más detallado sobre el proceso.

Si no estás seguro de qué cambios realiza el instalador en el sistema o si prefieres usar herramientas automáticas para crear el manifiesto de paquete, puedes tener en cuenta alguna de estas [opciones](desktop-to-uwp-root.md#convert).

>[!IMPORTANT]
>La capacidad para crear un paquete de aplicación de Windows para la aplicación de escritorio (de lo contrario se conoce como el puente de escritorio, se introdujo en Windows 10, versión 1607, y solo se puede usar en proyectos destinados a la actualización de aniversario de Windows 10 (10.0; Compilación 14393) o una versión posterior de Visual Studio.

## <a name="first-prepare-your-application"></a>Primero, prepara tu aplicación

Revisar esta guía antes de empezar a crear un paquete de la aplicación: [Preparar para empaquetar una aplicación de escritorio](desktop-to-uwp-prepare.md).

## <a name="create-a-package-manifest"></a>Crear un manifiesto de paquete

Crea un archivo y denomínalo **appxmanifest.xml**; a continuación, agrégale este código XML.

Esta es una plantilla básica que contiene los elementos y atributos que necesita el paquete. Agregaremos los valores de estos en la siguiente sección.

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>Rellenar los elementos de nivel de paquete del archivo

Rellena esta plantilla con la información que describe el paquete.

### <a name="identity-information"></a>Información de identidad

Este es un ejemplo del elemento **Identity** con texto del marcador de posición de los atributos. Puedes establecer el atributo ``ProcessorArchitecture`` en ``x64`` o ``x86``.

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> Si has reservado el nombre de la aplicación en la tienda Windows, puedes obtener el nombre y el publicador mediante el panel del centro de desarrollo de Windows. Si vas a transferir localmente la aplicación en otros sistemas, puedes proporcionar tus propios nombres a estos siempre que el nombre del publicador que elijas coincida con el nombre en el certificado que usas para firmar la aplicación.

### <a name="properties"></a>Propiedades

El elemento [Properties](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) tiene tres elementos secundarios obligatorios. Este es un ejemplo de nodo **Properties** con texto de marcador de posición para los elementos. **DisplayName** es el nombre de la aplicación que reservas en la tienda, para las aplicaciones que se cargan en la tienda.

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>Recursos

Este es un ejemplo del nodo [Resources](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources).

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>Dependencias

Para aplicaciones de escritorio que crear un paquete, establece siempre el ``Name`` atributo a ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>Funcionalidades
Para las aplicaciones de escritorio que se crea un paquete para, tendrás que agregar la ``runFullTrust`` funcionalidad.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>Rellenar los elementos de nivel de aplicación

Rellena esta plantilla con la información que describe la aplicación.

### <a name="application-element"></a>Elemento de la aplicación

Para las aplicaciones de escritorio que se crea un paquete, el ``EntryPoint`` atributo del elemento de la aplicación es siempre ``Windows.FullTrustApplication``.

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>Elementos visuales

Este es un ejemplo del nodo [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets" />

## <a name="optional-add-target-based-unplated-assets"></a>(Opcional) Agregar los activos sin placa basados en destino

Los recursos basados en el destino se usan para los iconos y las ventanas que se muestran en la barra de tareas de Windows, la vista de tareas, ALT+TAB, el asistente para el espacio restante y la esquina inferior derecha de las ventanas de Inicio. Puedes leer más sobre ellos [aquí](https://docs.microsoft.com/windows/uwp/shell/tiles-and-notifications/app-assets#target-based-assets).

1. Obtén las imágenes de 44 x 44 correctas y cópialas en la carpeta que contiene las imágenes (es decir, los activos).

2. Para cada imagen de 44 x 44, crea una copia en la misma carpeta y anexa **.targetsize-44_altform-unplated** al nombre del archivo. Debe tener 2 copias de cada icono, cada una llamada de forma específica. Por ejemplo, después de completar el proceso, es posible que la carpeta de activos contenga **MYAPP_44x44.png** y **MYAPP_44x44.targetsize-44_altform-unplated.png**.

   > [!NOTE]
   > En este ejemplo, el icono denominado **MYAPP_44x44.png** es el icono al que se hará referencia en el atributo del logotipo ``Square44x44Logo`` del paquete de la aplicación de Windows.

3.  En el paquete de la aplicación de Windows, establece el elemento ``BackgroundColor`` en cada icono que vaya a ser transparente.

4. Continúa con la siguiente subsección para generar un nuevo archivo de índice de recursos del paquete.

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file"></a>Generar un archivo de índice de recursos del paquete (PRI)

Si creas activos basados en destino como se describe en la sección anterior, o modificas cualquiera de los activos visuales de la aplicación después de crear el paquete, tendrás que generar un nuevo archivo PRI.

1.  Abre un **Símbolo del sistema para desarrolladores para VS 2017**.

    ![símbolo del sistema para desarrolladores](images/desktop-to-uwp/developer-command-prompt.png)

2.  Cambia el directorio a la carpeta raíz del paquete y crea un archivo priconfig.xml ejecutando el comando ``makepri createconfig /cf priconfig.xml /dq en-US``.

5.  Crea los archivos resources.pri mediante el comando ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``.

    Por ejemplo, el comando de la aplicación podría tener el siguiente aspecto: ``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.  Empaqueta el paquete de la aplicación de Windows siguiente las instrucciones del paso siguiente.

<a id="make-appx" />

## <a name="generate-a-windows-app-package"></a>Generar un paquete de aplicación de Windows

Usa el archivo **MakeAppx.exe** para crear un paquete de aplicación de Windows para el proyecto. Se incluye en el SDK de Windows 10 y, si tienes instalado Visual Studio, puedes obtener acceso a él fácilmente a través del símbolo del sistema para desarrolladores para tu versión de Visual Studio.

Consulta [Crear un paquete de la aplicación con la herramienta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)

## <a name="run-the-packaged-app"></a>Ejecutar la aplicación empaquetada

Puedes ejecutar la aplicación para probarla de forma local sin tener que obtener un certificado y firmarlo. Solo tienes que ejecutar este cmdlet de PowerShell:

```Add-AppxPackage –Register AppxManifest.xml```

Para actualizar los archivos .exe o .dll de la aplicación, reemplaza los archivos existentes en el paquete con los nuevos, aumenta el número de versión en AppxManifest.xml y, a continuación, vuelve a ejecutar el comando anterior.

> [!NOTE]
> Una aplicación empaquetada siempre se ejecuta como usuario interactivo y cualquier unidad que se instala la aplicación empaquetada en debe tener el formato al formato NTFS.

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También nos puede preguntar [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Código paso a paso / encontrar y solucionar problemas**

Consulta [Ejecutar, depurar y probar una aplicación de escritorio empaquetada](desktop-to-uwp-debug.md)

**Firmar la aplicación y, a continuación, distribuirlo**

Consulta [distribuir una aplicación de escritorio empaquetada](desktop-to-uwp-distribute.md)
