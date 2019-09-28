---
description: En el marcado XAML, especifica un valor null para una propiedad.
title: Extensión de marcado xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d6ee1f4cb1fa0a97c8d1b8a447ccc15cd0b51776
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340455"
---
# <a name="xnull-markup-extension"></a>Extensión de marcado {x:Null}


En el marcado XAML, especifica un valor **null** para una propiedad.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>Comentarios

**null** es la palabra clave de referencia NULL para C# y C++. La palabra clave de Microsoft Visual Basic para una referencia NULL es **Nothing**.

El valor predeterminado inicial puede variar entre las propiedades de dependencia y no es necesariamente **null**. Además, muchas propiedades de dependencia no aceptarán **null** como valor, ya sea a través del marcado o del código, debido a su implementación interna. En estos casos, establecer un valor de atributo XAML con **{x:Null}** puede provocar una excepción del analizador.

Algunos tipos de Windows Runtime aceptan valores NULL. En los casos en los que un tipo que acepta valores NULL no tenga ya un valor **null** como predeterminado, podrías usar **{x:Null}** en XAML para establecer el valor **null**. Si se usan C++ las extensiones deC++componentes de Visual (/CX), los tipos que aceptan valores NULL se representan como [Platform:: IBox @ no__t-4](https://docs.microsoft.com/cpp/cppcx/platform-ibox-interface). Si usas lenguajes de Microsoft .NET, los tipos que aceptan valores NULL se representan como [**Nullable<T>** ](https://docs.microsoft.com/dotnet/api/system.nullable-1).

## <a name="related-topics"></a>Temas relacionados

* [**Nullable @ no__t-2**](https://docs.microsoft.com/dotnet/api/system.nullable-1)
* [**IReference @ no__t-2**](https://docs.microsoft.com/uwp/api/Windows.Foundation.IReference_T_)
 

