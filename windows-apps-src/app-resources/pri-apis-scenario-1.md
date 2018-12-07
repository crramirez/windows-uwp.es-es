---
Description: In this scenario, we'll make a new app to represent our custom build system. We'll create a resource indexer and add strings and other kinds of resources to it. Then we'll generate and dump a PRI file.
title: Escenario 1 Generar un archivo PRI de los recursos de cadena y los archivos de activos
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: 9b14e413a5629dfb5447750e32c42c4efafef8fa
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8799382"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>Escenario 1: Generar un archivo PRI de los recursos de cadena y los archivos de activos
En este escenario, usaremos las [API de indexación de recursos de paquetes (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690) para que una aplicación nueva represente nuestro sistema de compilación personalizado. Recuerda que el propósito de este sistema de compilación personalizado consiste en crear archivos PRI para una aplicación para UWP de destino. Por lo tanto, como parte de este tutorial, vamos a crear algunos archivos de recursos de muestra (con cadenas y otros tipos de recursos) para representar los recursos de esa aplicación para UWP de destino.

## <a name="new-project"></a>Nuevo proyecto
Comienza creando un proyecto nuevo en Microsoft Visual Studio Crea un proyecto de **aplicación de consola de Windows de Visual C++** y asígnale el nombre de *CBSConsoleApp* (para "aplicación de consola de sistema de compilación personalizado").

Elige *x64* en la lista desplegable **Plataformas de solución**.

## <a name="headers-static-library-and-dll"></a>Encabezados, biblioteca estática y dll
Las API de PRI se declaran en el archivo de encabezado MrmResourceIndexer.h (que se instala en `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\`). Abre el archivo `CBSConsoleApp.cpp` e incluye el encabezado junto con algunos otros encabezados que necesitarás.

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

Las API se implementan en MrmSupport.dll, al que puedes obtener acceso mediante un vínculo a la biblioteca estática MrmSupport.lib. Abre **Propiedades** del proyecto, haz clic en **Enlazador** > **Entrada**, edita **AdditionalDependencies** y agrega `MrmSupport.lib`.

Compila la solución y, a continuación, copia `MrmSupport.dll` desde `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` a tu carpeta de salida de compilación (probablemente `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\`).

Agrega la siguiente función de aplicación auxiliar a `CBSConsoleApp.cpp` puesto que la necesitarás.

```cppwinrt
inline void ThrowIfFailed(HRESULT hr)
{
    if (FAILED(hr))
    {
        // Set a breakpoint on this line to catch Win32 API errors.
        throw new std::exception();
    }
}
```

En la función `main()`, agrega llamadas para inicializar y anular la inicialización de COM.

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>Archivos de recursos que pertenecen a la aplicación para UWP de destino
Ahora necesitaremos algunos archivos de recursos de muestra (con cadenas y otros tipos de recursos) para representar los recursos de la aplicación para UWP de destino. Estos, por supuesto, pueden encontrarse en cualquier lugar del sistema de archivos. Sin embargo, para este tutorial te resultará cómodo colocarlos en la carpeta de proyecto de CBSConsoleApp para que todo esté en un solo lugar. Solo debes agregar estos archivos de recursos al sistema de archivos; no los agregues al proyecto CBSConsoleApp.

Dentro de la misma carpeta que contiene `CBSConsoleApp.vcxproj`, agrega una subcarpeta denominada `UWPAppProjectRootFolder`. En esa subcarpeta, crea estos archivos de recursos de muestra.

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
Este archivo puede contener cualquier imagen PNG.

### <a name="uwpappprojectrootfolderresourcesresw"></a>\UWPAppProjectRootFolder\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-neutral</value>
    </data>
    <data name="LocalizedString2">
        <value>LocalizedString2-neutral</value>
    </data>
    <data name="NeutralOnlyString">
        <value>NeutralOnlyString-neutral</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderde-deresourcesresw"></a>\UWPAppProjectRootFolder\de-DE\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString2">
        <value>LocalizedString2-de-DE</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderen-usresourcesresw"></a>\UWPAppProjectRootFolder\en-US\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-en-US</value>
    </data>
    <data name="EnOnlyString">
        <value>EnOnlyString-en-US</value>
    </data>
</root>
```

## <a name="index-the-resources-and-create-a-pri-file"></a>Indizar los recursos y crear un archivo PRI
En la función `main()`, antes de llamar para inicializar COM, declara algunas cadenas que necesitaremos y también crea la carpeta de salida en la que generaremos nuestro archivo PRI.

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

Inmediatamente después de llamar a Inicializar COM, declara un manipulador de indizador de recursos y después llama a [**MrmCreateResourceIndexer**]() para crear un indizador de recursos.

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

Esta es una explicación de los argumentos que se pasan a **MrmCreateResourceIndexer**.

- El nombre de familia de paquete de nuestra aplicación para UWP de destino, que se usará como el nombre del mapa de recursos cuando más adelante generamos un archivo PRI de este indizador de recursos.
- La raíz del proyecto de nuestra aplicación para UWP de destino. Es decir, la ruta de acceso a nuestros archivos de recursos. Especificamos estos para poder especificar rutas de acceso relativas a esa raíz en llamadas API posteriores al mismo indizador de recursos.
- La versión de Windows que queremos tener como destino.
- Lista de calificadores de recursos predeterminados.
- Puntero a nuestro manipulador de indizador de recursos para que la función puede establecerlo.

El siguiente paso es agregar nuestros recursos al indizador de recursos que acabamos de crear. `resources.resw` es un archivo de recursos (.resw) que contiene las cadenas neutrales para nuestra aplicación para UWP de destino. Desplázate hacia arriba (en este tema) si quieres ver su contenido. `de-DE\resources.resw` contiene nuestras cadenas de alemán y `en-US\resources.resw`, nuestras cadenas en inglés. Para agregar los recursos de cadena dentro de un archivo de recursos a un indizador de recursos, llama a [**MrmIndexResourceContainerAutoQualifiers**](). En tercer lugar, llama a la función [**MrmIndexFile**]() en un archivo que contenga un recurso de imagen independiente para el indizador de recursos.

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

En la llamada a **MrmIndexFile**, el valor L L"ms-resource:///Files/sample-image.png" es el URI del recurso. El primer segmento de ruta de acceso es "Archivos" y eso es lo que se usará como el nombre del subárbol del mapa de recursos cuando más adelante generamos un archivo PRI de este indizador de recursos.

Habiendo informado del indizador de recursos acerca de nuestros archivos de recursos, es hora de que nos genere un archivo PRI en disco mediante una llamada a la función [**MrmCreateResourceFile**]().

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

En este punto, se ha creado un archivo PRI llamado `resources.pri` dentro de una carpeta denominada `Generated PRIs`. Ahora que hemos terminado con el indizador de recursos, llamamos a [**MrmDestroyIndexerAndMessages**]() para destruir su manipulador y liberar los recursos de máquina que asignó.

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

Dado que un archivo PRI es binario, va a ser más fácil ver lo que acabamos de generar si volcamos el archivo PRI binario en su XML equivalente. Una llamada a [**MrmDumpPriFile**]() hace precisamente eso.

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

Esta es una explicación de los argumentos que se pasan a **MrmDumpPriFile**.

- Ruta de acceso al archivo PRI para el volcado. No estamos usando el indizador de recursos en esta llamada (solo lo hemos destruido), por lo que necesitamos especificar una ruta de acceso completa al archivo.
- Ningún archivo de esquema. Hablaremos de lo que es un esquema más adelante en el tema.
- Solo la información básica.
- La ruta de acceso de un archivo XML que se va a crear.

Esto es lo que contiene el archivo PRI, volcado en XML aquí.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <ResourceMap name="OurUWPApp" version="1.0" primary="true">
        <Qualifiers>
            <Language>en-US,de-DE</Language>
        </Qualifiers>
        <ResourceMapSubtree name="Files">
            <NamedResource name="sample-image.png" uri="ms-resource://OurUWPApp/Files/sample-image.png">
                <Candidate type="Path">
                    <Value>sample-image.png</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="resources">
            <NamedResource name="EnOnlyString" uri="ms-resource://OurUWPApp/resources/EnOnlyString">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>EnOnlyString-en-US</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString1" uri="ms-resource://OurUWPApp/resources/LocalizedString1">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>LocalizedString1-en-US</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString1-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString2" uri="ms-resource://OurUWPApp/resources/LocalizedString2">
                <Candidate qualifiers="Language-de-DE" type="String">
                    <Value>LocalizedString2-de-DE</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString2-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="NeutralOnlyString" uri="ms-resource://OurUWPApp/resources/NeutralOnlyString">
                <Candidate type="String">
                    <Value>NeutralOnlyString-neutral</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

La información comienza con un mapa de recursos, que tiene el nombre de la familia de paquete de nuestra aplicación para UWP de destino. Hay dos subárboles de mapa de recursos incluidos por el mapa de recursos: uno para los recursos de archivos que hemos indizado y otro para nuestros recursos de cadena. Observa cómo el nombre de la familia de paquete se ha insertado en todos los URI de recursos.

El primer recurso de cadena es *EnOnlyString* de `en-US\resources.resw` y solo tiene un candidato (que coincide con el calificador *language-en-US*). A continuación, viene *LocalizedString1* tanto de `resources.resw` como de `en-US\resources.resw`. En consecuencia, tiene dos candidatos: una coincidencia *language-en-US*y un candidato independiente de reserva que coincide con cualquier contexto. Del mismo modo, *LocalizedString2* tiene dos candidatos: *language-de-DE* e independiente. Y, finalmente, *NeutralOnlyString* solo existe en forma independiente. Le doy ese nombre para dejar claro que no se ha diseñado para localizarse.

## <a name="summary"></a>Resumen
En este escenario, te mostramos cómo usar las [API de indexación de recursos de paquetes (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690) para crear un indizador de recursos. Hemos agregado archivos de activos y recursos de cadena para el indizador de recursos. A continuación, hemos usado el indizador de recursos para generar un archivo binario PRI. Y finalmente hemos volcado el archivo PRI binario en formato XML, de modo que podríamos confirmamos que contiene la información que esperábamos.

## <a name="important-apis"></a>API importantes
* [Referencia de la indexación de recursos de paquetes (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>Temas relacionados
* [API de indexación de recursos de paquetes (PRI) y sistemas de compilación personalizados](pri-apis-custom-build-systems.md)
