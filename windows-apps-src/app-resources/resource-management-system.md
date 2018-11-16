---
author: stevewhims
Description: At build time, the Resource Management System creates an index of all the different variants of the resources that are packaged up with your app. At run-time, the system detects the user and machine settings that are in effect and loads the resources that are the best match for those settings.
title: Sistema de administración de recursos
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: b80eda57ff700d732ba2402582ed6402acca4fc6
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6985801"
---
# <a name="resource-management-system"></a>Sistema de administración de recursos
El sistema de administración de recursos tiene funciones de tiempo de compilación y de tiempo de ejecución. Al compilar, el sistema crea un índice de todas las diferentes variantes de los recursos que se empaquetan con tu aplicación. Este índice es el índice de recursos del paquete, o PRI, y también se incluye en el paquete de la aplicación. En tiempo de ejecución, el sistema detecta el usuario y la configuración de la máquina que están en vigor, consulta la información del PRI y carga automáticamente los recursos que son la mejor coincidencia para esa configuración.

## <a name="package-resource-index-pri-file"></a>Archivo de índice de recursos del paquete (PRI)
Cada paquete de la aplicación debería contener un índice binario de los recursos de la aplicación. Este índice se crea durante la compilación y está incluido en uno o más archivos de índice de recursos del paquete (PRI).

- Un archivo PRI contiene recursos de cadena reales y un conjunto indexado de rutas de archivo que hacen referencia a varios archivos del paquete.
- Un paquetes contiene normalmente un solo archivo PRI por idioma, denominado resources.pri.
- El archivo resources.pri de la raíz de cada paquete se carga automáticamente cuando se crea una instancia del [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live).
- Los archivos PRI se pueden crear y volcar con la herramienta [MakePRI.exe](compile-resources-manually-with-makepri.md).
- Para el desarrollo habitual de aplicaciones no necesitarás MakePRI.exe, porque ya está integrado en el flujo de trabajo de compilación de Visual Studio. Además, Visual Studio admite la edición de archivos PRI en una interfaz de usuario dedicada. Sin embargo, es posible que los localizadores y las herramientas que usen se basen en MakePRI.exe.
- Cada archivo PRI contiene una colección de recursos con nombre, denominada mapa de recursos. Cuando se carga un archivo PRI desde un paquete, se comprueba si el nombre del mapa de recursos coincide con el nombre de identidad del paquete.
- Los archivos PRI solo contienen datos, por lo que no usan el formato portable ejecutable (PE). Están diseñados específicamente para que solo contengan datos como el formato de recursos de Windows. Reemplazan a los recursos contenidos en las DLL del modelo de aplicaciones de Win32.
- El límite de tamaño de un archivo PRI es de 64kilobytes.

## <a name="uwp-api-access-to-app-resources"></a>Acceso a las API de UWP para recursos de la aplicación

### <a name="basic-functionality-resourceloader"></a>Funcionalidad básica (ResourceLoader)
La forma más sencilla de acceder a los recursos de la aplicación mediante programación es mediante el espacio de nombres [**Windows.ApplicationModel.Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) y la clase [**ResourceLoader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live). **ResourceLoader** proporciona acceso básico a los recursos de cadena del conjunto de archivos de recursos, bibliotecas de referencia u otros paquetes.

### <a name="advanced-functionality-resourcemanager"></a>Funcionalidad avanzada (ResourceManager)
La clase  [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) (del espacio de nombres [**Windows.ApplicationModel.Resources.Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live)) proporciona información adicional sobre recursos, como la enumeración y la inspección. Esto va más allá de lo que la clase **ResourceLoader** proporciona.

Un objeto [**NamedResource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) representa un recurso lógico individual con varios idiomas u otras variantes calificadas. Describe la vista lógica del activo o recurso, con un identificador de recursos de cadena como `Header1` o un nombre de archivo de recursos como `logo.jpg`.

Un objeto [**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) representa un valor único de recurso concreto y sus calificadores, como la cadena "Hello World" para inglés o "logo.scale-100.jpg" como recurso de imagen calificado que es específico de la resolución **scale-100**.

Los recursos disponibles para una aplicación se almacenan en colecciones jerárquicas, a las que puedes tener acceso con un objeto [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live). La clase **ResourceManager** proporciona acceso a las diversas instancias **ResourceMap** de nivel superior que usa la aplicación, que se corresponden con los diversos paquetes para la aplicación. El valor [**MainResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap) corresponde al mapa de recursos del paquete de la aplicación actual y excluye cualquier paquete de marcos de referencia. Cada **ResourceMap** se denomina según el nombre de paquete que se especifica en el manifiesto del paquete. Dentro de un **ResourceMap** existen subárboles (consulta [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)), que contienen además objetos **NamedResource**. Los subárboles suelen corresponder a los archivos de recursos que contienen el recurso. Para obtener más información, consulta [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md) y [Cargar imágenes y activos adaptados a la escala, tema, contraste alto y otros](images-tailored-for-scale-theme-contrast.md).

Aquí tienes un ejemplo.

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**Nota** El identificador del recurso se trata como un fragmento de identificador uniforme de recursos (URI) y está sujeto a la semántica de los URI. Por ejemplo, `GetValue("Caption%20")` se trata como `GetValue("Caption ")`. No uses "?" ni "#" en los identificadores de recursos, ya que terminan la evaluación del trazado del recurso. Por ejemplo, "MyResource?3" se trata como "MyResource".

El **ResourceManager** no solo admite el acceso a los recursos de cadena de una aplicación, sino que también mantiene la capacidad de enumerar e inspeccionar los distintos recursos de archivo. Para evitar colisiones entre archivos y otros recursos que se originan desde dentro de un archivo, todas las rutas de archivos indexados residen dentro de un subárbol de **ResourceMap** "Archivos". Por ejemplo, el archivo `\Images\logo.png` corresponde al nombre de recurso `Files/images/logo.png`.

Las API [**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live) controlan de forma transparente las referencias a archivos como recursos y son adecuadas para escenarios de uso típicos. El **ResourceManager** solo debe usarse para escenarios avanzados, por ejemplo, cuando quieras invalidar el contexto actual.

### <a name="resourcecontext"></a>ResourceContext
Los candidatos de recursos se eligen en función de un [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live) concreto, que es una colección de valores de calificadores de recursos (idioma, escala, contraste, etc.). Un contexto predeterminado usa la configuración actual de la aplicación para cada valor de calificador, a menos que se sobrescriba. Por ejemplo, los recursos como las imágenes pueden calificarse para escala, lo que varía de un monitor a otro y, por lo tanto, de una vista de la aplicación a otra. Por este motivo, cada vista de la aplicación tiene un contexto predeterminado distinto. El contexto predeterminado de una determinada vista puede obtenerse mediante [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView). Siempre que recuperes un candidato de recurso, deberías pasar una instancia de **ResourceContext** para obtener el valor más apropiado para una determinada vista.

## <a name="important-apis"></a>API importantes
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>Temas relacionados
* [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md)
* [Cargar imágenes y activos adaptados a la escala, el tema, el contraste alto y otros](images-tailored-for-scale-theme-contrast.md)
