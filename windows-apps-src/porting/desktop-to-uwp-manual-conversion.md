---
author: awkoren
Description: "Muestra cómo convertir manualmente una aplicación de escritorio de Windows (por ejemplo, Win32, WPF y Windows Forms) en una aplicación para la Plataforma universal de Windows (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: "Convertir manualmente una aplicación de escritorio de Windows en una aplicación para la Plataforma universal de Windows (UWP)"
translationtype: Human Translation
ms.sourcegitcommit: fe96945759739e9260d0cdfc501e3e59fb915b1e
ms.openlocfilehash: 6ca48fd829b7437fe2db8aa1251f6ed8976919ab

---

# Convertir manualmente la aplicación a UWP mediante el puente de escritorio

El uso de Desktop App Converter (DAC) es práctico y automático, y resulta útil si existe alguna duda sobre lo que hace el instalador. No obstante, si la aplicación se instala mediante xcopy, o si estás familiarizado con los cambios que realiza el instalador de la aplicación en el sistema, puedes optar por crear un paquete de la aplicación y un manifiesto manualmente.

Pasos para crear un paquete de forma manual:

## Crea un manifiesto manualmente.

El archivo _appxmanifest.xml_ debe tener el siguiente contenido (como mínimo). Cambia los marcadores de posición que tengan un formato como \*\*\*THIS\*\*\* a los valores reales de la aplicación.

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Package
       xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
       xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
       xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
      <Identity Name="***YOUR_PACKAGE_NAME_HERE***"
        ProcessorArchitecture="x64"
        Publisher="CN=***COMPANY_NAME***, O=***ORGANIZATION_NAME***, L=***CITY***, S=***STATE***, C=***COUNTRY***"
        Version="***YOUR_PACKAGE_VERSION_HERE***" />
      <Properties>
        <DisplayName>***YOUR_PACKAGE_DISPLAY_NAME_HERE***</DisplayName>
        <PublisherDisplayName>Reserved</PublisherDisplayName>
        <Description>No description entered</Description>
        <Logo>***YOUR_PACKAGE_RELATIVE_DISPLAY_LOGO_PATH_HERE***</Logo>
      </Properties>
      <Resources>
        <Resource Language="en-us" />
      </Resources>
      <Dependencies>
        <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.14316.0" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
      <Applications>
        <Application Id="***YOUR_PRAID_HERE***" Executable="***YOUR_PACKAGE_RELATIVE_EXE_PATH_HERE***" EntryPoint="Windows.FullTrustApplication">
          <uap:VisualElements
           BackgroundColor="#464646"
           DisplayName="***YOUR_APP_DISPLAY_NAME_HERE***"
           Square150x150Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Square44x44Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Description="***YOUR_APP_DESCRIPTION_HERE***" />
        </Application>
      </Applications>
    </Package>
    ```

## Ejecutar la herramienta MakeAppX

Usa el [App packager (MakeAppx.exe) [Empaquetador de aplicaciones (MakeAppx.exe)]](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx) para generar un AppX para el proyecto. MakeAppx.exe se incluye con el SDK de Windows 10. 

Para ejecutar MakeAppx, primero asegúrate de haber creado un archivo de manifiesto como se ha descrito anteriormente. 

A continuación, crea un archivo de asignación. El archivo debe comenzar por **[Files]** y, después, mostrar cada uno de los archivos de origen en el disco seguido de su ruta de acceso de destino en el paquete. A continuación te mostramos un ejemplo: 

```
[Files]
"C:\MyApp\StartPage.htm"     "default.html"
"C:\MyApp\readme.txt"        "doc\readme.txt"
"\\MyServer\path\icon.png"   "icon.png"
"MyCustomManifest.xml"       "AppxManifest.xml"
```

Finalmente, ejecuta el siguiente comando: 

```cmd
MakeAppx.exe pack /f mapping_filepath /p filepath.appx
```

## Firma el paquete AppX

El cmdlet Add-AppxPackage requiere que el paquete de la aplicación (.appx) que se implemente esté firmado. Usa [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx), que se incluye en el SDK de Microsoft Windows 10, para firmar el paquete .appx.

Ejemplo de uso: 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

Cuando ejecutes MakeCert.exe y se te pida que escribas una contraseña, selecciona **ninguna**. Para obtener más información sobre los certificados y las firmas, consulta: 

- [Cómo crear certificados temporales para su uso durante el desarrollo](https://msdn.microsoft.com/library/ms733813.aspx)

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)

- [SignTool.exe (Sign Tool)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)




<!--HONumber=Nov16_HO1-->


