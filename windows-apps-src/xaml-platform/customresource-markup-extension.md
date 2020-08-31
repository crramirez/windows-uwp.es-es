---
description: Proporciona un valor para cualquier atributo XAML evaluando una referencia a un recurso que proceda de una implementación de búsqueda de recursos personalizada. La búsqueda de recursos la realiza una implementación de clase CustomXamlResourceLoader.
title: Extensión de marcado CustomResource
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 094a2a957493787acef8e97b49b61778fc1ec42f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155109"
---
# <a name="customresource-markup-extension"></a>Extensión de marcado {CustomResource}


Proporciona un valor para cualquier atributo XAML evaluando una referencia a un recurso que proceda de una implementación de búsqueda de recursos personalizada. La búsqueda de recursos la realiza una implementación de clase [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader).

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| key | Clave del recurso solicitado. La forma en que se asigna inicialmente la clave es propio de la implementación de la clase [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) actualmente registrada para su uso. |

## <a name="remarks"></a>Observaciones

**CustomResource** es una técnica para obtener valores definidos en otras partes de un repositorio de recursos. Esta técnica es relativamente avanzada y no se usa en la mayoría de escenarios de aplicaciones de Windows Runtime.

En este tema no se describe cómo se resuelve **CustomResource** en un diccionario de recursos, porque eso puede variar en función de cómo se implemente [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader).

El analizador de XAML de Windows Runtime llama al método [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) de la implementación de [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) siempre que detecte un uso de `{CustomResource}` en el marcado. El *resourceId* que se pasa a **GetResource** procede del argumento *key* y los otros parámetros de entrada proceden del contexto, como por ejemplo, a qué propiedad se aplica el uso.

Un uso de `{CustomResource}` no funciona de forma predeterminada (la implementación básica de [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) está incompleta). Para hacer una referencia válida de `{CustomResource}` debes realizar todos estos pasos:

1.  Derivar una clase personalizada desde [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) e invalidar el método [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource). No llames a la base en la implementación.
2.  Establecer [**CustomXamlResourceLoader.Current**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.current) en una referencia a tu clase en la lógica de inicialización. Esto debe suceder antes de que se cargue cualquier código XAML de nivel de página que incluya el uso de la extensión `{CustomResource}`. Un lugar para establecer **CustomXamlResourceLoader.Current** es en el constructor de la subclase [**Application**](/uwp/api/Windows.UI.Xaml.Application) que se genera automáticamente en las plantillas de código subyacente App.xaml.
3.  Ahora puedes usar las extensiones `{CustomResource}` en el código XAML que tu aplicación carga como páginas, o desde dentro de los diccionarios de recursos XAML.

**CustomResource** es una extensión de marcado. Las extensiones de marcado se suelen implementar cuando se necesita que los valores de los atributos de escape no sean valores literales o nombres de controladores, y este requisito es de índole más global que limitarse a colocar los convertidores de tipos en determinados tipos o propiedades. Todas las extensiones de marcado de XAML usan los \{ caracteres "" y " \} " en su sintaxis de atributo, que es la Convención por la que un procesador XAML reconoce que una extensión de marcado debe procesar el atributo.

## <a name="related-topics"></a>Temas relacionados

* [Referencias a ResourceDictionary y a los recursos XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)
* [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)