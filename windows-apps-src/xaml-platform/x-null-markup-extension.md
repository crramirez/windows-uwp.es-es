---
description: En el marcado XAML, especifica un valor nulo para una propiedad.
title: Extensión de marcado xNull
ms.assetid: E6A4038E-4ADA-4E82-9824-582FC16AB037
---

# Extensión de marcado {x:Null}

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En el marcado XAML, especifica un valor **null** para una propiedad.

## Uso del atributo XAML

``` syntax
<object property="{x:Null}" .../>
```

## Observaciones

**null** es la palabra clave de referencia NULL para C# y C++. La palabra clave de Microsoft Visual Basic para una referencia NULL es **Nothing**.

El valor predeterminado inicial puede variar entre las propiedades de dependencia y no es necesariamente **null**. Además, muchas propiedades de dependencia no aceptarán **null** como valor, ya sea a través del marcado o del código, debido a su implementación interna. En estos casos, establecer un valor de atributo XAML con **{x:Null}** puede provocar una excepción del analizador.

Algunos tipos de Windows Runtime aceptan valores NULL. En los casos en los que un tipo que acepta valores NULL no tenga ya un valor **null** como predeterminado, podrías usar **{x:Null}** en XAML para establecer el valor **null**. Si usas las extensiones de componentes de Visual C++ (C++/CX), los tipos que aceptan valores NULL se representan como [**Platform::IBox<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/jj606120.aspx). Si usas lenguajes de Microsoft .NET, los tipos que aceptan valores NULL se representan como [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx).

## Temas relacionados

* [**Nullable<T>**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)
* [**IReference<T>**](https://msdn.microsoft.com/library/windows/apps/br225864)
 



<!--HONumber=Mar16_HO1-->


