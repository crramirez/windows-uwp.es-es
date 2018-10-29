---
author: jwmsft
description: En el marcado XAML, especifica un valor null para una propiedad.
title: Extensión de marcado xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b6033a01ee811977b3a37f820217005fdbd80616
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5741865"
---
# <a name="xnull-markup-extension"></a>Extensión de marcado {x:Null}


En el marcado XAML, especifica un valor **null** para una propiedad.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object property="{x:Null}" .../>
```

## <a name="remarks"></a>Observaciones

**null** es la palabra clave de referencia NULL para C# y C++. La palabra clave de Microsoft Visual Basic para una referencia NULL es **Nothing**.

El valor predeterminado inicial puede variar entre las propiedades de dependencia y no es necesariamente **null**. Además, muchas propiedades de dependencia no aceptarán **null** como valor, ya sea a través del marcado o del código, debido a su implementación interna. En estos casos, establecer un valor de atributo XAML con **{x:Null}** puede provocar una excepción del analizador.

Algunos tipos de Windows Runtime aceptan valores NULL. En los casos en los que un tipo que acepta valores NULL no tenga ya un valor **null** como predeterminado, podrías usar **{x:Null}** en XAML para establecer el valor **null**. Si usas las extensiones de componente de VisualC ++ (C++ / CX), los tipos que aceptan valores NULL se representan como [**Platform:: ibox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx). Si usas lenguajes de Microsoft .NET, los tipos que aceptan valores NULL se representan como [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx).

## <a name="related-topics"></a>Temas relacionados

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 

