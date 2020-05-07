---
Description: Obtén información sobre cómo conceder identidades a aplicaciones de escritorio no empaquetadas para poder usar las características modernas de Windows 10 en esas aplicaciones.
title: Concesión de identidad a aplicaciones de escritorio no empaquetadas
ms.date: 04/23/2020
ms.topic: article
keywords: Windows 10, escritorio, paquete, identidad, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d870c82a3e4a8bc6c2ce923026010eff953eead2
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82107718"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>Concesión de identidad a aplicaciones de escritorio no empaquetadas

Muchas de las características de extensibilidad de Windows 10 requieren el uso de la [identidad de paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) desde aplicaciones de escritorio que no son para UWP, que incluyen tareas en segundo plano, notificaciones, iconos dinámicos y destinos de recursos compartidos. En estos casos, el sistema operativo requiere la identidad para poder identificar al autor de llamada de la API correspondiente.

En las versiones del sistema operativo anteriores a Windows 10, versión 2004, la única manera de conceder la identidad a una aplicación de escritorio es [empaquetarla en un paquete MSIX firmado](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Para estas aplicaciones, la identidad se especifica en el manifiesto del paquete, y el registro de identidades se controla mediante la canalización de la implementación de MSIX en función de la información del manifiesto. Todo el contenido al que se hace referencia en el manifiesto del paquete está presente en el paquete MSIX.

A partir de Windows 10, versión 2004, puedes conceder la identidad del paquete a las aplicaciones de escritorio no empaquetadas en un paquete MSIX mediante la creación y el registro de un *paquete disperso* con la aplicación. Esta compatibilidad permite que las aplicaciones de escritorio que todavía no pueden adoptar el empaquetado de MSIX para la implementación usen las características de extensibilidad de Windows 10 que requieren la identidad del paquete. Para obtener más información general, consulta [esta entrada de blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97).

Para crear y registrar un paquete disperso que conceda la identidad del paquete a la aplicación de escritorio, debes realizar los pasos siguientes.

1. [Crear un manifiesto del paquete para el paquete disperso](#create-a-package-manifest-for-the-sparse-package)
2. [Crear y firmar el paquete disperso](#build-and-sign-the-sparse-package)
3. [Agregar los metadatos de identidad del paquete al manifiesto de aplicación de escritorio](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [Registrar el paquete disperso en tiempo de ejecución](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>Conceptos importantes

Las siguientes características permiten a las aplicaciones de escritorio no empaquetadas adquirir la identidad del paquete.

### <a name="sparse-packages"></a>Paquetes dispersos

Un *paquete disperso* contiene un manifiesto del paquete, pero ningún otro archivo binario ni contenido de la aplicación. El manifiesto de un paquete disperso puede hacer referencia a archivos que están fuera del paquete en una ubicación externa predeterminada. Esto permite que las aplicaciones que aún no pueden adoptar el empaquetado de MSIX para toda la aplicación adquieran la identidad del paquete según lo requieran algunas características de extensibilidad de Windows 10.

> [!NOTE]
> Una aplicación de escritorio que usa un paquete disperso no recibe algunas de las ventajas de la implementación completa a través de un paquete MSIX. Estas ventajas incluyen la protección contra alteraciones, la instalación en una ubicación bloqueada y la administración completa de la implementación, el tiempo de ejecución y la desinstalación por parte del sistema operativo.

### <a name="package-external-location"></a>Ubicación externa del paquete

Para admitir paquetes dispersos, el esquema del manifiesto del paquete ahora admite un elemento [**uap10:AllowExternalContent**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-allowexternalcontent) opcional en el elemento [**Properties**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties). Permite que el manifiesto del paquete haga referencia al contenido fuera del paquete, en una ubicación específica en el disco.

Por ejemplo, si tienes una aplicación de escritorio no empaquetada que instala el ejecutable de la aplicación y otro contenido en C:\Program Files\MyDesktopApp\,, puedes crear un paquete disperso que incluya el elemento **uap10:AllowExternalContent** en el manifiesto. Durante el proceso de instalación de la aplicación o el primer inicio de las aplicaciones, puede instalar el paquete disperso y declarar C:\Archivos de programa\MyDesktopApp\ como la ubicación externa que usará la aplicación.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>Crear un manifiesto del paquete para el paquete disperso

Para poder crear un paquete disperso, primero debes crear el [manifiesto del paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) (un archivo denominado AppxManifest.xml) que declare los metadatos de identidad del paquete para la aplicación de escritorio y otros detalles necesarios. La forma más fácil de crear un manifiesto del paquete para el paquete disperso es usar el ejemplo siguiente y personalizarlo para la aplicación mediante la [referencia de esquema](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Asegúrate de que el manifiesto del paquete incluye estos elementos:

* Un elemento [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) que describa los atributos de identidad de la aplicación de escritorio.
* Un elemento [**uap10:AllowExternalContent**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap10-allowexternalcontent) en el elemento [**Properties**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties). A este elemento se le debe asignar el valor `true`, que permite que el manifiesto del paquete haga referencia al contenido fuera del paquete, en una ubicación específica en el disco. En un paso posterior, deberás especificar la ruta de acceso de la ubicación externa al registrar el paquete disperso desde el código que se ejecuta en el instalador o en la aplicación. Cualquier contenido al que se haga referencia en el manifiesto que no se encuentre en el propio paquete debe instalarse en la ubicación externa.
* El atributo **MinVersion** del elemento [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) debe establecerse en `10.0.19000.0` o en una versión posterior.
* Los atributos **TrustLevel=mediumIL** y **RuntimeBehavior=Win32App** del elemento [**Application**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) declaran que la aplicación de escritorio asociada con el paquete disperso se ejecutará de forma similar a una aplicación de escritorio no empaquetada estándar, sin la virtualización del sistema de archivos y el Registro, y con otros cambios en tiempo de ejecución.

En el ejemplo siguiente se muestra el contenido completo de un manifiesto de paquete disperso (AppxManifest.xml). Este manifiesto incluye una extensión `windows.sharetarget`, que requiere la identidad del paquete.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package 
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  IgnorableNamespaces="uap uap2 uap3 rescap desktop uap10">
  <Identity Name="ContosoPhotoStore" ProcessorArchitecture="x64" Publisher="CN=Contoso" Version="1.0.0.0" />
  <Properties>
    <DisplayName>ContosoPhotoStore</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Assets\storelogo.png</Logo>
    <uap10:AllowExternalContent>true</uap10:AllowExternalContent>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19000.0" MaxVersionTested="10.0.19000.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
    <rescap:Capability Name="unvirtualizedResources"/>
  </Capabilities>
  <Applications>
    <Application Id="ContosoPhotoStore" Executable="ContosoPhotoStore.exe" uap10:TrustLevel="mediumIL" uap10:RuntimeBehavior="win32App"> 
      <uap:VisualElements AppListEntry="none" DisplayName="Contoso PhotoStore" Description="Demonstrate photo app" BackgroundColor="transparent" Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png" Square310x310Logo="Assets\LargeTile.png" Square71x71Logo="Assets\SmallTile.png"></uap:DefaultTile>
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
      <Extensions>
        <uap:Extension Category="windows.shareTarget">
          <uap:ShareTarget Description="Send to ContosoPhotoStore">
            <uap:SupportedFileTypes>
              <uap:FileType>.jpg</uap:FileType>
              <uap:FileType>.png</uap:FileType>
              <uap:FileType>.gif</uap:FileType>
            </uap:SupportedFileTypes>
            <uap:DataFormat>StorageItems</uap:DataFormat>
            <uap:DataFormat>Bitmap</uap:DataFormat>
          </uap:ShareTarget>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

## <a name="build-and-sign-the-sparse-package"></a>Crear y firmar el paquete disperso

Después de crear el manifiesto del paquete, crea el paquete disperso con la [herramienta MakeAppx.exe](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool) en Windows SDK. Dado que el paquete disperso no contiene los archivos a los que se hace referencia en el manifiesto, debes especificar la opción `/nv`, que omite la validación semántica del paquete.

En el siguiente ejemplo se muestra cómo crear un paquete disperso desde la línea de comandos.  

```Console
MakeAppx.exe pack /d <path to directory that contains manifest> /p <output path>\MyPackage.msix /nv
```

Para poder instalar correctamente el paquete disperso en un equipo de destino, debes firmarlo con un certificado que sea de confianza en el equipo de destino. Puedes crear un nuevo certificado autofirmado para fines de desarrollo y firmar el paquete disperso mediante [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool), que está disponible en Windows SDK.

En el siguiente ejemplo se muestra cómo firmar un paquete disperso desde la línea de comandos.

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx /p <certificate password> <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>Agregar los metadatos de identidad del paquete al manifiesto de aplicación de escritorio

También debes incluir un [manifiesto de la aplicación en paralelo](https://docs.microsoft.com/windows/win32/sbscs/application-manifests) con la aplicación de escritorio e incluir un elemento [**msix**](https://docs.microsoft.com/windows/win32/sbscs/application-manifests#msix) con atributos que declaren los atributos de identidad de la aplicación. El sistema operativo usa los valores de estos atributos para determinar la identidad de la aplicación cuando se inicia el archivo ejecutable.

En el ejemplo siguiente se muestra un manifiesto de la aplicación en paralelo con un elemento **msix**.

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity version="1.0.0.0" name="Contoso.PhotoStoreApp"/>
  <msix xmlns="urn:schemas-microsoft-com:msix.v1"
          publisher="CN=Contoso"
          packageName="ContosoPhotoStore"
          applicationId="ContosoPhotoStore"
        />
</assembly>
```

Los atributos del elemento **msix** deben coincidir con estos valores en el manifiesto del paquete disperso:

* Los atributos **packageName** y **publisher** deben coincidir con los atributos **Name** y **Publisher** del elemento [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) del manifiesto del paquete, respectivamente.
* El atributo **applicationId** debe coincidir con el atributo **Id** del elemento [**Application**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) del manifiesto del paquete.

El manifiesto de la aplicación en paralelo debe existir en el mismo directorio que el archivo ejecutable de la aplicación de escritorio y, por convención, debe tener el mismo nombre que el archivo ejecutable de la aplicación con la extensión `.manifest`. Por ejemplo, si el nombre del archivo ejecutable de la aplicación es `ContosoPhotoStore`, el nombre de archivo del manifiesto de aplicación debe ser `ContosoPhotoStore.exe.manifest`.

## <a name="register-your-sparse-package-at-run-time"></a>Registrar el paquete disperso en tiempo de ejecución

Para conceder la identidad del paquete a la aplicación de escritorio, la aplicación debe registrar el paquete disperso mediante el método **AddPackageByUriAsync** de la clase [**PackageManager**](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager). Este método está disponible a partir de Windows 10, versión 2004. Puedes agregar código a la aplicación para registrar el paquete disperso cuando la aplicación se ejecuta por primera vez, o bien ejecutar código para registrar el paquete mientras se instala la aplicación de escritorio (por ejemplo, si usas MSI para instalar la aplicación de escritorio, puedes ejecutar este código desde una acción personalizada).

En el siguiente ejemplo se muestra cómo registrar un paquete disperso. Este código crea un objeto **AddPackageOptions** que contiene la ruta de acceso a la ubicación externa donde el manifiesto del paquete puede hacer referencia al contenido fuera del paquete. A continuación, el código pasa este objeto al método **AddPackageByUriAsync** para registrar el paquete disperso. Este método también recibe la ubicación del paquete disperso firmado como un URI. Para obtener un ejemplo más completo, consulta el archivo de código `StartUp.cs` en el [ejemplo](#sample) relacionado.

```csharp
private static bool registerSparsePackage(string externalLocation, string sparsePkgPath)
{
    bool registration = false;
    try
    {
        Uri externalUri = new Uri(externalLocation);
        Uri packageUri = new Uri(sparsePkgPath);

        Console.WriteLine("exe Location {0}", externalLocation);
        Console.WriteLine("msix Address {0}", sparsePkgPath);

        Console.WriteLine("  exe Uri {0}", externalUri);
        Console.WriteLine("  msix Uri {0}", packageUri);

        PackageManager packageManager = new PackageManager();

        // Declare use of an external location
        var options = new AddPackageOptions();
        options.ExternalLocationUri = externalUri;

        Windows.Foundation.IAsyncOperationWithProgress<DeploymentResult, DeploymentProgress> deploymentOperation = packageManager.AddPackageByUriAsync(packageUri, options);

        // Other progress and error-handling code omitted for brevity...
    }
}
```

## <a name="sample"></a>Muestra

Consulta la muestra [SparesePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages) para obtener una aplicación de ejemplo totalmente funcional que muestre cómo conceder la identidad del paquete a una aplicación de escritorio mediante un paquete disperso. En [esta entrada de blog](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97) se proporciona más información sobre la compilación y ejecución del ejemplo.

En este ejemplo se incluye lo siguiente:

* El código fuente de una aplicación de WPF denominada PhotoStoreDemo. Durante el inicio, la aplicación comprueba si se está ejecutando con identidad. Si no se ejecuta con identidad, registra el paquete disperso y, a continuación, reinicia la aplicación. Consulta en `StartUp.cs` el código que realiza estos pasos.
* Un manifiesto de aplicación en paralelo denominado `PhotoStoreDemo.exe.manifest`.
* Un manifiesto de paquete denominado `AppxManifest.xml`.
