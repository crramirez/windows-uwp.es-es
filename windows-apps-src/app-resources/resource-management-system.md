---
description: En tiempo de compilación, el sistema de administración de recursos crea un índice de todas las diferentes variantes de los recursos que se empaquetan con la aplicación. En tiempo de ejecución, el sistema detecta la configuración de usuario y máquina que está en vigor y carga los recursos que mejor coinciden con esos valores.
title: Sistema de administración de recursos
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 083f49dd8a269bd3a0277084cadc175271d5e501
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031628"
---
# <a name="resource-management-system"></a>Sistema de administración de recursos
El sistema de administración de recursos tiene características en tiempo de compilación y en tiempo de ejecución. En tiempo de compilación, el sistema crea un índice de todas las variantes diferentes de los recursos que se empaquetan con la aplicación. Este índice es el índice de recursos del paquete, o PRI, y también se incluye en el paquete de la aplicación. En tiempo de ejecución, el sistema detecta la configuración de usuario y equipo que está en vigor, consulta la información de PRI y carga automáticamente los recursos que son la mejor coincidencia para esos valores.

## <a name="package-resource-index-pri-file"></a>Archivo de índice de recursos del paquete (PRI)
Cada paquete de aplicación debe contener un índice binario de los recursos de la aplicación. Este índice se crea en tiempo de compilación y se encuentra en uno o varios archivos de índice de recursos del paquete (PRI).

- Un archivo PRI contiene los recursos de cadena reales y un conjunto indizado de rutas de acceso de archivo que hacen referencia a varios archivos del paquete.
- Un paquete normalmente contiene un solo archivo PRI por idioma, denominado Resources. PRI.
- El archivo resources. PRI situado en la raíz de cada paquete se carga automáticamente cuando se crea una instancia de [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) .
- Los archivos PRI se pueden crear y volcar con la herramienta [MakePRI.exe](compile-resources-manually-with-makepri.md).
- Para el desarrollo de aplicaciones típico, no necesitará MakePRI.exe porque ya está integrado en el flujo de trabajo de compilación de Visual Studio. Y Visual Studio admiten la edición de archivos PRI en una interfaz de usuario dedicada. Sin embargo, los localizadores y las herramientas que usan pueden basarse en MakePRI.exe.
- Cada archivo PRI contiene una colección de recursos con nombre que hace referencia a una asignación de recursos. Cuando se carga un archivo PRI de un paquete, se comprueba el nombre del mapa de recursos para que coincida con el nombre de identidad del paquete.
- Los archivos PRI solo contienen datos, por lo que no usan el formato portable ejecutable (PE). Están diseñados específicamente para ser de solo datos como el formato de los recursos de Windows. Reemplazan los recursos incluidos en los archivos dll en el modelo de aplicación de Win32.

## <a name="uwp-api-access-to-app-resources"></a>Acceso de API de UWP a recursos de aplicaciones

### <a name="basic-functionality-resourceloader"></a>Funcionalidad básica (ResourceLoader)
La manera más sencilla de acceder a los recursos de la aplicación mediante programación consiste en usar el espacio de nombres [**Windows. ApplicationModel. Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) y la clase [**ResourceLoader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live) . **ResourceLoader** proporciona acceso básico a los recursos de cadena del conjunto de archivos de recursos, bibliotecas a las que se hace referencia u otros paquetes.

### <a name="advanced-functionality-resourcemanager"></a>Funcionalidad avanzada (ResourceManager)
La clase [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) (en el espacio de nombres [**Windows. ApplicationModel. Resources. Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live) ) proporciona información adicional sobre los recursos, como la enumeración y la inspección. Esto va más allá de lo que proporciona la clase **ResourceLoader** .

Un objeto [**NamedResource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) representa un recurso lógico individual con varios idiomas u otras variantes calificadas. Describe la vista lógica del activo o recurso, con un identificador de recurso de cadena como `Header1` , o un nombre de archivo de recursos como `logo.jpg` .

Un objeto [**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) representa un único valor de recurso concreto y sus calificadores, como la cadena "Hola mundo" para inglés o "logo.scale-100.jpg" como un recurso de imagen completo específico de la resolución **-100** .

Los recursos disponibles para una aplicación se almacenan en colecciones jerárquicas, a las que se puede tener acceso con un objeto [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) . La clase **ResourceManager** proporciona acceso a las distintas instancias de **ResourceMap** de nivel superior usadas por la aplicación, que corresponden a los distintos paquetes de la aplicación. El valor [**MainResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap) corresponde al mapa de recursos del paquete de la aplicación actual y excluye todos los paquetes de Framework a los que se hace referencia. Cada **ResourceMap** se denomina para el nombre del paquete que se especifica en el manifiesto del paquete. Dentro de un **ResourceMap** , los subárboles (vea [**ResourceMap. GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)), que contienen más objetos de **NamedResource** . Los subárboles suelen corresponder a los archivos de recursos que contienen el recurso. Para obtener más información, consulte [localizar cadenas en la interfaz de usuario y el manifiesto del paquete de la aplicación](localize-strings-ui-manifest.md) y [cargar imágenes y recursos adaptados para escala, tema, contraste alto y otros](images-tailored-for-scale-theme-contrast.md).

Aquí tiene un ejemplo.

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**Nota:** El identificador de recursos se trata como un fragmento de identificador uniforme de recursos (URI), sujeto a la semántica de URI. Por ejemplo, `GetValue("Caption%20")` se trata como `GetValue("Caption ")` . No use "?" o "#" en los identificadores de recursos porque finalizan la evaluación de la ruta de acceso a los recursos. Por ejemplo, "perresource? 3" se trata como "perresource".

**ResourceManager** no solo admite el acceso a los recursos de cadena de una aplicación, sino que también mantiene la capacidad de enumerar e inspeccionar los distintos recursos de archivo. Con el fin de evitar colisiones entre archivos y otros recursos que se originan en un archivo, las rutas de acceso de archivo indizadas residen en un subárbol de **ResourceMap** "files" reservado. Por ejemplo, el archivo `\Images\logo.png` corresponde al nombre del recurso `Files/images/logo.png` .

Las API [**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live) administran de forma transparente las referencias a los archivos como recursos y son adecuadas para escenarios de uso típicos. **ResourceManager** solo se debe usar para escenarios avanzados, como cuando se desea reemplazar el contexto actual.

### <a name="resourcecontext"></a>ResourceContext
Los candidatos de recursos se eligen en función de un [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live)determinado, que es una colección de valores de calificador de recursos (idioma, escala, contraste, etc.). Un contexto predeterminado utiliza la configuración actual de la aplicación para cada valor de calificador, a menos que se invalide. Por ejemplo, los recursos, como las imágenes, se pueden calificar para escalar, que varía de un monitor a otro y, por lo tanto, de una vista de aplicación a otra. Por esta razón, cada vista de aplicación tiene un contexto predeterminado distinto. El contexto predeterminado para una vista determinada puede obtenerse mediante [**ResourceContext. GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView). Siempre que recupere un candidato de recurso, debe pasar una instancia de **ResourceContext** para obtener el valor más apropiado para una vista determinada.

## <a name="important-apis"></a>API importantes
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>Temas relacionados
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md)
* [Cargar imágenes y recursos adaptados a escala, tema, contraste alto y otros](images-tailored-for-scale-theme-contrast.md)
