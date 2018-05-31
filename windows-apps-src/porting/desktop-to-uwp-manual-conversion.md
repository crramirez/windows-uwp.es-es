---
author: normesta
Description: Shows how to manually package a Windows desktop application (like Win32, WPF, and Windows Forms) for Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: Empaquetar una aplicación de forma manual (Puente de dispositivo de escritorio)
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.openlocfilehash: 81d5b9b0b52ef0f7529b277215e7fe0b95683f0a
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
ms.locfileid: "1689771"
---
# <a name="package-an-app-manually-desktop-bridge"></a>Empaquetar una aplicación de forma manual (Puente de dispositivo de escritorio)

En este tema se muestra cómo empaquetar la aplicación sin tener que usar herramientas como Visual Studio o Desktop App Converter (DAC).

Para empaquetar la aplicación de forma manual, tienes que crear un archivo de manifiesto de paquete y ejecutar una herramienta de línea de comandos para generar un paquete de la aplicación de Windows.

Si instalas la aplicación mediante el comando xcopy o si estás familiarizado con los cambios que realiza el instalador de la aplicación en el sistema y quieres un control más detallado del proceso, puedes plantearte la posibilidad de realizar el empaquetado manual.

Si no estás seguro de qué cambios realiza el instalador en el sistema o si prefieres usar herramientas automáticas para crear el manifiesto de paquete, puedes tener en cuenta alguna de estas [opciones](desktop-to-uwp-root.md#convert).

>[!IMPORTANT]
>El Puente de dispositivo de escritorio se introdujo en Windows 10, versión 1607, y solo se puede usar en proyectos destinados a la Actualización de aniversario de Windows 10 (10.0, compilación 14393) o una versión posterior de Visual Studio.

## <a name="first-consider-how-youll-distribute-your-app"></a>En primer lugar, piensa en la manera de distribuir la aplicación.
Si vas a publicar la aplicación en la [Microsoft Store](https://www.microsoft.com/store/apps), comienza rellenando [este formulario](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft se pondrá en contacto contigo para iniciar el proceso de incorporación. Como parte de este proceso, podrás reservar un nombre en la tienda y obtener la información que necesitas para empaquetar la aplicación.

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
> Si has reservado el nombre de la aplicación en la Store Windows, puedes obtener el Nombre y el Publicador mediante el panel del Centro de desarrollo de Windows. Si vas a transferir localmente la aplicación a otros sistemas, puedes proporcionar tus propios nombres a estos elementos, siempre y cuando el nombre del publicador que elijas coincida con el nombre que aparece en el certificado que uses para firmar la aplicación.

### <a name="properties"></a>Propiedades

El elemento [Properties](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) tiene tres elementos secundarios obligatorios. Este es un ejemplo de nodo **Properties** con texto de marcador de posición para los elementos. El elemento **DisplayName** es el nombre de la aplicación que reservas en la tienda, para las aplicaciones que se cargan en la tienda.

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

Para las aplicaciones de Puente de dispositivo de escritorio, establece siempre el atributo ``Name`` en ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>Funcionalidades
En cuanto a las aplicaciones de Puente de dispositivo de escritorio, tendrás que agregar la funcionalidad ``runFullTrust``.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>Rellenar los elementos de nivel de aplicación

Rellena esta plantilla con la información que describe la aplicación.

### <a name="application-element"></a>Elemento de la aplicación

Para las aplicaciones de Puente de dispositivo de escritorio, el atributo ``EntryPoint`` del elemento Application siempre es ``Windows.FullTrustApplication``.

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

Si creas activos basados en el destino, tal y como se describe en la sección anterior, o modificas cualquiera de los activos visuales de la aplicación después de crear el paquete, tendrás que generar un nuevo archivo PRI.

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
> Una aplicación empaquetada se ejecuta siempre como un usuario interactivo, por lo que cualquier unidad en la que instales la aplicación empaquetada debe tener un formato NTFS.

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También nos puede preguntar [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Código paso a paso / encontrar y solucionar problemas**

Consulta [Ejecutar, depurar y probar una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-debug.md)

**Firma y distribuye la aplicación**

Consulta [Distribuir una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-distribute.md)
