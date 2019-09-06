---
title: Asignaciones de .NET de tipos de Windows Runtime
description: En la tabla siguiente se enumeran las asignaciones que .NET realiza entre los tipos de Plataforma universal de Windows (UWP) y los tipos de .NET.
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c035db58fc6aa484f9d47a9af61176a2b05d55ee
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393656"
---
# <a name="net-mappings-of-windows-runtime-types"></a>Asignaciones de .NET de tipos de Windows Runtime

En la tabla siguiente se enumeran las asignaciones que .NET realiza entre los tipos de Plataforma universal de Windows (UWP) y los tipos de .NET. En una aplicación universal de Windows escrita con código administrado, Visual Studio IntelliSense muestra el tipo .NET en lugar del tipo UWP. Por ejemplo, si un método Windows Runtime toma un parámetro de tipo cadena&lt;&gt;IVector, IntelliSense muestra un parámetro de tipo IList&lt;String&gt;. De forma similar, en un componente de Windows Runtime escrito con código administrado, se usa el tipo .NET en las firmas de miembro. Cuando la [herramienta de exportación de metadatos de Windows Runtime (Winmdexp. exe)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) genera el componente Windows Runtime, el tipo .net se convierte en el tipo de UWP correspondiente.

La mayoría de los tipos que tienen el mismo nombre de espacio de nombres y el mismo nombre de tipo en UWP y .NET son estructuras (o tipos asociados a estructuras, como enumeraciones). En UWP, las estructuras no tienen miembros que no sean campos y requieren tipos de aplicación auxiliar, que .NET oculta. Las versiones de .NET de estas estructuras tienen propiedades y métodos que proporcionan la funcionalidad de los tipos auxiliares ocultos.

## <a name="uwp-types-that-map-to-net-types-with-the-same-name-and-namespace"></a>Tipos de UWP que se asignan a tipos .NET con el mismo nombre y espacio de nombres

### <a name="in-net-assembly-systemobjectmodeldll"></a>En el ensamblado .NET System. ObjectModel. dll

| Espacio de nombres | Type |
|-|-|
| Windows.UI.Xaml.Input | ICommand |

### <a name="in-net-assembly-systemruntimewindowsruntimedll"></a>En .NET Assembly System. Runtime. WindowsRuntime. dll

| Espacio de nombres | Type |
|-|-|
| Windows.Foundation | Point |
| Windows.Foundation | Rect |
| Windows.Foundation | Tamaño |
| Windows.UI | Color |

### <a name="in-net-assembly-systemruntimewindowsruntimeuixamldll"></a>En .NET Assembly System. Runtime. WindowsRuntime. UI. Xaml. dll

| Espacio de nombres | Type |
|-|-|
| Windows.UI.Xaml | CornerRadius |
| Windows.UI.Xaml | Duration |
| Windows.UI.Xaml | DurationType |
| Windows.UI.Xaml | GridLength |
| Windows.UI.Xaml | GridUnitType |
| Windows.UI.Xaml | Thickness |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition |
| Windows.UI.Xaml.Media | Matrix |
| Windows.UI.Xaml.Media.Animation | KeyTime |
| Windows.UI.Xaml.Media.Animation | RepeatBehavior |
| Windows.UI.Xaml.Media.Animation | RepeatBehaviorType |
| Windows.UI.Xaml.Media.Media3D | Matrix3D |

## <a name="uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace"></a>Tipos de UWP que se asignan a tipos .NET con un nombre y/o espacio de nombres diferente

### <a name="in-net-assembly-systemobjectmodeldll"></a>En el ensamblado .NET System. ObjectModel. dll

| Tipo UWP/espacio de nombres | Tipo/espacio de nombres .NET |
|-|-|
| INotifyCollectionChanged (Windows.UI.Xaml.Interop) | INotifyCollectionChanged (System.Collections.Specialized) | 
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized) | 
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventArgs (System.Collections.Specialized) | 
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop) | NotifyCollectionChangedAction (System.Collections.Specialized) | 
| INotifyPropertyChanged (Windows.UI.Xaml.Data) | INotifyPropertyChanged (System.ComponentModel) | 
| PropertyChangedEventHandler (Windows.UI.Xaml.Data) | PropertyChangedEventHandler (System.ComponentModel) | 
| PropertyChangedEventArgs (Windows.UI.Xaml.Data) | PropertyChangedEventArgs (System.ComponentModel) | 

### <a name="in-net-assembly-systemruntimedll"></a>En .NET Assembly System. Runtime. dll

| Tipo UWP/espacio de nombres | Tipo/espacio de nombres .NET |
|-|-|
| AttributeUsageAttribute (Windows.Foundation.Metadata) | AttributeUsageAttribute (System) |
| AttributeTargets (Windows.Foundation.Metadata) | AttributeTargets (System) |
| DateTime (Windows.Foundation) | DateTimeOffset (System) |
| EventHandler&lt;T&gt; (Windows.Foundation) | EventHandler&lt;T&gt; (System) |
| HResult (Windows.Foundation) | Exception (System) |
| IReference&lt;T&gt; (Windows.Foundation) | Nullable&lt;T&gt; (System) |
| TimeSpan (Windows.Foundation) | TimeSpan (System) |
| Uri (Windows.Foundation) | Uri (System) |
| IClosable (Windows.Foundation) | IDisposable (System) |
| IIterable&lt;T&gt; (Windows.Foundation.Collections) | IEnumerable&lt;T&gt; (System.Collections.Generic) |
| IVector&lt;T&gt; (Windows.Foundation.Collections) | IList&lt;T&gt; (System.Collections.Generic) |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections) | IReadOnlyList&lt;T&gt; (System.Collections.Generic) |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections) | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections) | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections) | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic) |
| IBindableIterable (Windows.UI.Xaml.Interop) | IEnumerable (System.Collections) |
| IBindableVector (Windows.UI.Xaml.Interop) | IList (System.Collections) |
| TypeName (Windows.UI.Xaml.Interop) | Type (System) |

### <a name="in-net-assembly-systemruntimeinteropserviceswindowsruntimedll"></a>En .NET Assembly System. Runtime. InteropServices. WindowsRuntime. dll

| Tipo UWP/espacio de nombres | Tipo/espacio de nombres .NET |
|-|-|
| EventRegistrationToken (Windows.Foundation) | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) |

## <a name="related-topics"></a>Temas relacionados

* [Windows Runtime componentes con C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)