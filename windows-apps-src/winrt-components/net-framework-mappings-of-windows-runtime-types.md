---
author: msatranjr
title: Asignaciones de .NET Framework de tipos de Windows Runtime
description: En la tabla siguiente se enumeran las asignaciones que .NET Framework realiza entre los tipos de Plataforma universal de Windows (UWP) y los tipos de .NET Framework.
ms.assetid: 5317D771-808D-4B97-8063-63492B23292F
ms.sourcegitcommit: 4c32b134c704fa0e4534bc4ba8d045e671c89442
ms.openlocfilehash: 286f479c86c06c9d08b4e36cf9776b590a13cc5f

---

# Asignaciones de .NET Framework de tipos de Windows Runtime


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En la tabla siguiente se enumeran las asignaciones que .NET Framework realiza entre los tipos de Plataforma universal de Windows (UWP) y los tipos de .NET Framework. En una aplicación universal de Windows escrita con código administrado, IntelliSense muestra el tipo de .NET Framework en lugar del tipo UWP. Por ejemplo, si un método Windows Runtime toma un parámetro de tipo IVector&lt;string&gt;, IntelliSense muestra un parámetro de tipo IList&lt;string&gt;. De forma similar, en un componente de Windows Runtime escrito con código administrado, se usa el tipo de .NET Framework en las signaturas de miembro. Cuando la [herramienta de exportación de metadatos de Windows Runtime (Winmdexp.exe)](https://msdn.microsoft.com/library/hh925576.aspx) genera su componente de Windows Runtime, el tipo de .NET Framework se convierte en el tipo UWP correspondiente.

## Tablas de asignación


La mayoría de los tipos que tienen el mismo espacio de nombres y nombre de tipo tanto en UWP como en .NET Framework son estructuras (o tipos asociados con estructuras, como enumeraciones). En la UWP, las estructuras no tienen miembros que no sean campos y requieren tipos de ayuda que .NET Framework oculta. Las versiones de .NET Framework de estas estructuras tienen propiedades y métodos que proporcionan la funcionalidad de los tipos de ayuda escondida.

Para obtener más información sobre la forma en que .NET Framework usa los metadatos de Windows para simplificar la programación con Windows Runtime, descarga la [CLR y el libro blanco de Windows Runtime](http://download.microsoft.com/download/2/3/E/23E1E9BE-41AA-4716-A7B3-82040271394C/CLR%20and%20the%20Windows%20Runtime.docx) del Centro de desarrollo de Windows.

Tabla 1: tipos UWP que se asignan a tipos de .NET Framework con un nombre o un espacio de nombres diferente.

| Tipo UWP/espacio de nombres                                            | Tipo de .NET Framework/espacio de nombres                                          | Ensamblado de .NET Framework                           |
|---------------------------------------------------------------|------------------------------------------------------------------------|---------------------------------------------------|
| AttributeUsageAttribute (Windows.Foundation.Metadata)         | AttributeUsageAttribute (System)                                       | System.Runtime.dll                                |
| AttributeTargets (Windows.Foundation.Metadata)                | AttributeTargets (System)                                              | System.Runtime.dll                                |
| DateTime (Windows.Foundation)                                 | DateTimeOffset (System)                                                | System.Runtime.dll                                |
| EventHandler&lt;T&gt; (Windows.Foundation)                    | EventHandler&lt;T&gt; (System)                                         | System.Runtime.dll                                |
| EventRegistrationToken (Windows.Foundation)                   | EventRegistrationToken (System.Runtime.InteropServices.WindowsRuntime) | System.Runtime.InteropServices.WindowsRuntime.dll |
| HResult (Windows.Foundation)                                  | Exception (System)                                                     | System.Runtime.dll                                |
| IReference&lt;T&gt; (Windows.Foundation)                      | Nullable&lt;T&gt; (System)                                             | System.Runtime.dll                                |
| TimeSpan (Windows.Foundation)                                 | TimeSpan (System)                                                      | System.Runtime.dll                                |
| Uri (Windows.Foundation)                                      | Uri (System)                                                           | System.Runtime.dll                                |
| IClosable (Windows.Foundation)                                | IDisposable (System)                                                   | System.Runtime.dll                                |
| IIterable&lt;T&gt; (Windows.Foundation.Collections)           | IEnumerable&lt;T&gt; (System.Collections.Generic)                      | System.Runtime.dll                                |
| IVector&lt;T&gt; (Windows.Foundation.Collections)             | IList&lt;T&gt; (System.Collections.Generic)                            | System.Runtime.dll                                |
| IVectorView&lt;T&gt; (Windows.Foundation.Collections)         | IReadOnlyList&lt;T&gt; (System.Collections.Generic)                    | System.Runtime.dll                                |
| IMap&lt;K,V&gt; (Windows.Foundation.Collections)              | IDictionary&lt;TKey,TValue&gt; (System.Collections.Generic)            | System.Runtime.dll                                |
| IMapView&lt;K,V&gt; (Windows.Foundation.Collections)          | IReadOnlyDictionary&lt;TKey,TValue&gt; (System.Collections.Generic)    | System.Runtime.dll                                |
| IKeyValuePair&lt;K,V&gt; (Windows.Foundation.Collections)     | KeyValuePair&lt;TKey,TValue&gt; (System.Collections.Generic)           | System.Runtime.dll                                |
| IBindableIterable (Windows.UI.Xaml.Interop)                   | IEnumerable (System.Collections)                                       | System.Runtime.dll                                |
| IBindableVector (Windows.UI.Xaml.Interop)                     | IList (System.Collections)                                             | System.Runtime.dll                                |
| INotifyCollectionChanged (Windows.UI.Xaml.Interop)            | INotifyCollectionChanged (System.Collections.Specialized)              | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventHandler (Windows.UI.Xaml.Interop) | NotifyCollectionChangedEventHandler (System.Collections.Specialized)   | System.ObjectModel.dll                            |
| NotifyCollectionChangedEventArgs (Windows.UI.Xaml.Interop)    | NotifyCollectionChangedEventArgs (System.Collections.Specialized)      | System.ObjectModel.dll                            |
| NotifyCollectionChangedAction (Windows.UI.Xaml.Interop)       | NotifyCollectionChangedAction (System.Collections.Specialized)         | System.ObjectModel.dll                            |
| INotifyPropertyChanged (Windows.UI.Xaml.Data)                 | INotifyPropertyChanged (System.ComponentModel)                         | System.ObjectModel.dll                            |
| PropertyChangedEventHandler (Windows.UI.Xaml.Data)            | PropertyChangedEventHandler (System.ComponentModel)                    | System.ObjectModel.dll                            |
| PropertyChangedEventArgs (Windows.UI.Xaml.Data)               | PropertyChangedEventArgs (System.ComponentModel)                       | System.ObjectModel.dll                            |
| TypeName (Windows.UI.Xaml.Interop)                            | Type (System)                                                          | System.Runtime.dll                                |

 

Tabla 2: tipos UWP que se asignan a tipos de .NET Framework con el mismo nombre y espacio de nombres.

| Espacio de nombres                           | Tipo               | Ensamblado de .NET Framework                   |
|-------------------------------------|--------------------|-------------------------------------------|
| Windows.UI                          | Color              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Punto              | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Rect               | System.Runtime.WindowsRuntime.dll         |
| Windows.Foundation                  | Tamaño               | System.Runtime.WindowsRuntime.dll         |
| Windows.UI.Xaml.Input               | ICommand           | System.ObjectModel.dll                    |
| Windows.UI.Xaml                     | CornerRadius       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Duración           | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | DurationType       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridLength         | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | GridUnitType       | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml                     | Grosor          | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Controls.Primitives | GeneratorPosition  | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media               | Matriz             | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | KeyTime            | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehavior     | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Animation     | RepeatBehaviorType | System.Runtime.WindowsRuntime.UI.Xaml.dll |
| Windows.UI.Xaml.Media.Media3D       | Matrix3D           | System.Runtime.WindowsRuntime.UI.Xaml.dll |

 

## Temas relacionados

* [Crear componentes de Windows Runtime en C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)



<!--HONumber=Jun16_HO5-->


