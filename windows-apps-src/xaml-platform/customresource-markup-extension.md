---
description: Proporciona un valor para cualquier atributo XAML evaluando una referencia a un recurso que proceda de una implementación de búsqueda de recursos personalizada. La búsqueda de recursos la realiza una implementación de clase CustomXamlResourceLoader.
title: Extensión de marcado CustomResource
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7eabcb188aa1687d36d4b4e6f432783aa68969de
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8202559"
---
# <a name="customresource-markup-extension"></a>Extensión de marcado {CustomResource}


Proporciona un valor para cualquier atributo XAML evaluando una referencia a un recurso que proceda de una implementación de búsqueda de recursos personalizada. La búsqueda de recursos la realiza una implementación de clase [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327).

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| key | La clave del recurso solicitado. La forma en que se asigna inicialmente la clave es propio de la implementación de la clase [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) actualmente registrada para su uso. |

## <a name="remarks"></a>Comentarios

**CustomResource** es una técnica para obtener valores definidos en otras partes de un repositorio de recursos. Esta técnica es relativamente avanzada y no se usa en la mayoría de escenarios de aplicaciones de Windows Runtime.

En este tema no se describe cómo se resuelve **CustomResource** en un diccionario de recursos, porque eso puede variar en función de cómo se implemente [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327).

El analizador de XAML de Windows Runtime llama al método [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) de la implementación de [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) siempre que detecte un uso de `{CustomResource}` en el marcado. El *resourceId* que se pasa a **GetResource** procede del argumento *key* y los otros parámetros de entrada proceden del contexto, como por ejemplo, a qué propiedad se aplica el uso.

Un uso de `{CustomResource}` no funciona de forma predeterminada (la implementación básica de [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) está incompleta). Para hacer una referencia válida de `{CustomResource}` debes realizar todos estos pasos:

1.  Derivar una clase personalizada desde [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) e invalidar el método [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340). No llames a la base en la implementación.
2.  Establecer [**CustomXamlResourceLoader.Current**](https://msdn.microsoft.com/library/windows/apps/br243328) en una referencia a tu clase en la lógica de inicialización. Esto debe suceder antes de que se cargue cualquier código XAML de nivel de página que incluya el uso de la extensión `{CustomResource}`. Un lugar para establecer **CustomXamlResourceLoader.Current** es en el constructor de la subclase [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) que se genera automáticamente en las plantillas de código subyacente App.xaml.
3.  Ahora puedes usar las extensiones `{CustomResource}` en el código XAML que tu aplicación carga como páginas, o desde dentro de los diccionarios de recursos XAML.

**CustomResource** es una extensión de marcado. Las extensiones de marcado generalmente se implementan cuando es necesario que los valores de atributo de escape no sean valores literales o nombres de controlador y el requisito sea más global que simplemente colocar convertidores de tipos en ciertos tipos o propiedades. Todas las extensiones de marcado en XAML usan los caracteres "\{" y "\}" en su sintaxis de atributo, que es la convención mediante la cual un procesador XAML reconoce que una extensión de marcado debe procesar el atributo.

## <a name="related-topics"></a>Temas relacionados

* [Referencias a ResourceDictionary a recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)
* [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340)

