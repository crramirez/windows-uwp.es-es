---
title: Instalar un conjunto relacionado con un archivo del Instalador de aplicación
description: En esta sección, revisaremos los pasos que debes tomar para permitir la instalación de un conjunto relacionado a través del Instalador de aplicación. También analizaremos los pasos para crear un archivo *.appinstaller que definirá su conjunto relacionado.
ms.date: 1/4/2018
ms.topic: article
keywords: windows 10, uwp, instalador de aplicación, AppInstaller, instalación de prueba, conjunto relacionado, paquetes opcionales
ms.localizationpriority: medium
ms.openlocfilehash: c90fffeee7003159f58cf2e108286f4fe7732fd6
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8757994"
---
# <a name="install-a-related-set-using-an-app-installer-file"></a>Instalar un conjunto relacionado con un archivo del Instalador de aplicación

Si estás empezando con paquetes opcionales para UWP o conjuntos relacionados, los siguientes artículos son buenos recursos para empezar a trabajar. 

1.  [Ampliar tu aplicación con paquetes opcionales](https://blogs.msdn.microsoft.com/appinstaller/2017/04/05/uwpoptionalpackages/)
2.  [Crear tu primer paquete opcional](https://blogs.msdn.microsoft.com/appinstaller/2017/05/09/build-your-first-optional-package/)
3.  [Cargar código desde un paquete opcional](https://blogs.msdn.microsoft.com/appinstaller/2017/05/11/loading-code-from-an-optional-package/)
4.  [Herramientas para crear un conjunto relacionado](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)
5.  [Paquetes opcionales y creación de conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)

Con el Windows 10 Fall Creators Update, se pueden instalar ahora conjuntos relacionados mediante el Instalador de aplicación. Esto permite la distribución y la implementación de paquetes de aplicaciones de conjuntos relacionados para los usuarios. 

## <a name="how-to-install-a-related-set"></a>Cómo instalar un conjunto relacionado 
Un conjunto relacionado no es una entidad, sino una combinación de un paquete principal y paquetes opcionales. Para poder instalar un conjunto relacionado como una entidad, debemos podremos especificar el paquete principal y un paquete opcional como uno. Para ello, necesitaremos crear un archivo XML con una extensión ".appinstaller" para definir un conjunto relacionado. El Instalador de aplicación consume el archivo *.appinstaller y permite al usuario instalar todos los paquetes definidos con un solo clic. 

Antes de ver todo con más detalle, este es un archivo *.appinstaller de ejemplo completo:

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

Durante la implementación, se valida el archivo del Instalador de aplicación con los paquetes de aplicaciones a los que se hace referencia en el elemento `Uri`. Por lo tanto, los elementos `Name`, `Publisher` y `Version` deben coincidir con el elemento [Paquete/Identidad](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) del manifiesto del paquete de la aplicación. 

## <a name="how-to-create-an-app-installer-file"></a>Cómo crear un archivo del Instalador de aplicación 

Para distribuir tu conjunto relacionado como una entidad, debes crear un archivo del Instalador de la aplicación que contiene los elementos necesarios para ese [esquema de appinstaller](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

### <a name="step-1-create-the-appinstaller-file"></a>Paso 1: Crear el archivo *.appinstaller
Con un editor de texto, crea un archivo (que contendrá XML) y asígnale el nombre &lt;nombre_archivo&gt;.appinstaller. 

### <a name="step-2-add-the-basic-template"></a>Paso 2: Agregar la plantilla básica
La plantilla básica incluye la información del archivo del Instalador de aplicación. 
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>Paso 3: Agregar la información del paquete principal 
Si el paquete de aplicación principal es un archivo .appxbundle o .msixbundle, usa el `<MainBundle>` se muestra a continuación. Si el paquete de aplicación principal es un archivo .appx o .msix, a continuación, usar `<MainPackage>` en lugar de `<MainBundle>` en el fragmento de código. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />

</AppInstaller>
```
La información del atributo `<MainBundle>` o `<MainPackage>` debe coincidir con el elemento [Paquete/identidad](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) del manifiesto del lote de aplicaciones o del manifiesto del paquete de la aplicación, respectivamente. 

### <a name="step-4-add-the-optional-packages"></a>Paso 4: Agregar paquetes opcionales 
De manera similar al atributo del paquete de aplicación principal, si el paquete opcional puede ser un paquete de aplicación o un lote de aplicaciones, el elemento secundario dentro del atributo `<OptionalPackages>` debe ser `<Package>` o `<Bundle>` respectivamente. La información del paquete de los elementos secundarios debe coincidir con el elemento de identidad del manifiesto del paquete o del lote. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>Paso 5: Agregar dependencias. 
En el elemento de dependencias, puedes especificar los paquetes de marco necesarios para el paquete principal o los paquetes opcionales. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>Paso 6: Agregar configuración de actualización 
El archivo del Instalador de aplicación también puede especificar la configuración de actualización para que los conjuntos relacionados se puedan actualizar automáticamente cuando se publica un archivo del Instalador de aplicación más reciente. **<UpdateSettings>** es un elemento opcional. Dentro de **<UpdateSettings>** la opción OnLaunch especifica que se deben realizar comprobaciones de actualización en el inicio de la aplicación y HoursBetweenUpdateChecks = "12" especifica que se debe realizar una comprobación de actualizaciones cada 12 horas. Si no se especifica HoursBetweenUpdateChecks, el intervalo predeterminado que se usa para buscar actualizaciones es 24 horas.
``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017/2"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>
    
    <UpdateSettings>
        <OnLaunch HoursBetweenUpdateChecks="12" />
    </UpdateSettings>

</AppInstaller>
```

Para todos los detalles sobre el esquema XML, consulta [Referencia del archivo del Instalador de aplicación](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file).

> [!NOTE]
> 
> El tipo de archivo del Instalador de aplicación es nuevo en Windows 10 Fall Creators Update. No hay ninguna compatibilidad con la implementación de aplicaciones para UWP con un archivo del Instalador de aplicación en versiones anteriores de Windows 10.
> También debes tener en cuenta que el elemento **HoursBetweenUpdateChecks** es nuevo en la siguiente actualización importante de Windows 10.
