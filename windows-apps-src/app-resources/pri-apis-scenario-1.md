---
description: En este escenario, crearemos una nueva aplicación para representar nuestro sistema de compilación personalizado. Vamos a crear un indexador de recursos y agregarle cadenas y otros tipos de recursos. A continuación, generaremos y volcaremos un archivo PRI.
title: Escenario 1 generación de un archivo PRI a partir de recursos de cadena y archivos de recursos
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 44f4b8297cc1a34a378af137f75babca64e4edf2
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031678"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>Escenario 1: generar un archivo PRI a partir de recursos de cadena y archivos de recursos
En este escenario, usaremos las [API de indexación de recursos de paquetes (PRI)](/windows/desktop/menurc/pri-indexing-reference) para crear una nueva aplicación que represente el sistema de compilación personalizado. El objetivo de este sistema de compilación personalizado es crear archivos PRI para una aplicación de UWP de destino. Por lo tanto, como parte de este tutorial, vamos a crear algunos archivos de recursos de ejemplo (que contienen cadenas y otros tipos de recursos) para representar los recursos de la aplicación UWP de destino.

## <a name="new-project"></a>Nuevo proyecto
Para empezar, crea un proyecto en Microsoft Visual Studio. Cree un proyecto de **aplicación de consola de Windows Visual C++** y asígnele el nombre *CBSConsoleApp* (para "aplicación de consola del sistema de compilación personalizada").

Elija *x64* en la lista desplegable **plataformas de solución** .

## <a name="headers-static-library-and-dll"></a>Encabezados, biblioteca estática y dll
Las API de PRI se declaran en el archivo de encabezado MrmResourceIndexer. h (que se instala en `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\` ). Abra el archivo `CBSConsoleApp.cpp` e incluya el encabezado junto con otros encabezados que necesitará.

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

Las API se implementan en MrmSupport.dll, a la que se tiene acceso vinculando a la biblioteca estática MrmSupport. lib. Abra **las propiedades** del proyecto, haga clic en entrada del **vinculador**  >  **Input** , edite **AdditionalDependencies** y agregue `MrmSupport.lib` .

Compile la solución y, a continuación, copie `MrmSupport.dll` desde `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` a la carpeta de salida de la compilación (probablemente `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\` ).

Agregue la siguiente función auxiliar a `CBSConsoleApp.cpp` , ya que la necesitaremos.

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

En la `main()` función, agregue llamadas para inicializar y anular la inicialización de com.

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>Archivos de recursos que pertenecen a la aplicación de UWP de destino
Ahora necesitaremos algunos archivos de recursos de ejemplo (que contienen cadenas y otros tipos de recursos) para representar los recursos de la aplicación UWP de destino. Por supuesto, pueden encontrarse en cualquier lugar del sistema de archivos. Pero para este tutorial, será conveniente colocarlas en la carpeta de proyecto de CBSConsoleApp para que todo esté en un solo lugar. Solo tiene que agregar estos archivos de recursos al sistema de archivos; no las agregue al proyecto CBSConsoleApp.

En la misma carpeta que contiene `CBSConsoleApp.vcxproj` , agregue una nueva subcarpeta denominada `UWPAppProjectRootFolder` . Dentro de esa nueva subcarpeta, cree estos archivos de recursos de ejemplo.

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

## <a name="index-the-resources-and-create-a-pri-file"></a>Indexación de los recursos y creación de un archivo PRI
En la `main()` función, antes de la llamada a Initialize com, declare algunas cadenas que se van a necesitar y también creará la carpeta de salida en la que se va a generar el archivo PRI.

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

Inmediatamente después de la llamada a para inicializar COM, declare un identificador de indexador de recursos y, a continuación, llame a [**MrmCreateResourceIndexer**](/windows/desktop/menurc/mrmcreateresourceindexer) para crear un indizador de recursos.

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

A continuación se muestra una explicación de los argumentos que se pasan a **MrmCreateResourceIndexer** .

- El nombre de familia de paquete de nuestra aplicación de UWP de destino, que se usará como nombre del mapa de recursos cuando se genere posteriormente un archivo PRI a partir de este indexador de recursos.
- La raíz del proyecto de la aplicación de UWP de destino. En otras palabras, la ruta de acceso a los archivos de recursos. Lo especificamos para que podamos especificar las rutas de acceso relativas a esa raíz en las posteriores llamadas API al mismo indexador de recursos.
- La versión de Windows a la que queremos dirigirse.
- Una lista de calificadores de recursos predeterminados.
- Puntero a nuestro identificador de indexador de recursos para que la función pueda establecerlo.

El siguiente paso consiste en agregar nuestros recursos al indizador de recursos que acabamos de crear. `resources.resw` es un archivo de recursos (. resw) que contiene las cadenas neutras para nuestra aplicación de UWP de destino. Desplácese hacia arriba (en este tema) Si desea ver su contenido. `de-DE\resources.resw` contiene nuestras cadenas alemanas y `en-US\resources.resw` nuestras cadenas en inglés. Para agregar los recursos de cadena dentro de un archivo de recursos a un indizador de recursos, llame a [**MrmIndexResourceContainerAutoQualifiers**](/windows/desktop/menurc/mrmindexresourcecontainerautoqualifiers). En tercer lugar, se llama a la función [**MrmIndexFile**](/windows/desktop/menurc/mrmindexfile) en un archivo que contiene un recurso de imagen neutro al indizador de recursos.

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

En la llamada a **MrmIndexFile** , el valor L "MS-Resource:///files/sample-image.png" es el URI del recurso. El primer segmento de la ruta de acceso es "files", y eso es lo que se usará como el nombre del subárbol del mapa de recursos cuando se genere posteriormente un archivo PRI a partir de este indexador de recursos.

Una vez que se ha informado del indexador de recursos sobre nuestros archivos de recursos, es el momento de hacer que se genere un archivo PRI en el disco llamando a la función [**MrmCreateResourceFile**](/windows/desktop/menurc/mrmcreateresourcefile) .

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

En este momento, se ha creado un archivo PRI denominado `resources.pri` en una carpeta denominada `Generated PRIs` . Ahora que hemos terminado con el indexador de recursos, llamamos a [**MrmDestroyIndexerAndMessages**](/windows/desktop/menurc/mrmdestroyindexerandmessages) para destruir su identificador y liberar los recursos de equipo que ha asignado.

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

Dado que un archivo PRI es binario, es más fácil ver lo que se acaba de generar si se vuelca el archivo. PRI binario en su equivalente de XML. Una llamada a [**MrmDumpPriFile**](/windows/desktop/menurc/mrmdumpprifile) hace justamente eso.

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

A continuación se muestra una explicación de los argumentos que se pasan a **MrmDumpPriFile** .

- Ruta de acceso al archivo PRI que se va a volcar. No usamos el indexador de recursos en esta llamada (acabamos de destruirlo), por lo que es necesario especificar una ruta de acceso completa al archivo.
- No hay ningún archivo de esquema. Veremos qué es un esquema más adelante en el tema.
- Solo la información básica.
- Ruta de acceso de un archivo XML que se va a crear.

Esto es lo que el archivo PRI, volcado a XML aquí, contiene.

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

La información comienza con un mapa de recursos, que se denomina con el nombre de familia de paquete de nuestra aplicación de UWP de destino. El mapa de recursos incluye dos subárboles de mapa de recursos: uno para los recursos de archivo indexados y otro para nuestros recursos de cadena. Observe cómo se ha insertado el nombre de familia de paquete en todos los URI de recurso.

El primer recurso de cadena se *EnOnlyString* de `en-US\resources.resw` y tiene un solo candidato (que coincide con el calificador *Language-en-US* ). A continuación se incluye *LocalizedString1* de `resources.resw` y `en-US\resources.resw` . Por lo tanto, tiene dos candidatos: uno que coincide con *-en-US* y un candidato neutro de reserva que coincide con cualquier contexto. Del mismo modo, *LocalizedString2* tiene dos candidatos: *Language-de-de* y neutral. Y, por último, *NeutralOnlyString* solo existe en formato neutro. Le he dado ese nombre para que quede claro que no está diseñado para ser localizado.

## <a name="summary"></a>Resumen
En este escenario, hemos mostrado cómo usar las [API de indexación de recursos de paquetes (PRI)](/windows/desktop/menurc/pri-indexing-reference) para crear un indizador de recursos. Hemos agregado recursos de cadena y archivos de recursos al indexador de recursos. Después, usamos el indexador de recursos para generar un archivo PRI binario. Finalmente, volcamos el archivo binario PRI en forma de XML para que se pueda confirmar que contiene la información que se esperaba.

## <a name="important-apis"></a>API importantes
* [Referencia de indexación de recursos de paquetes (PRI)](/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Temas relacionados
* [API de indexación de recursos de paquetes (PRI) y sistemas de compilación personalizados](pri-apis-custom-build-systems.md)
