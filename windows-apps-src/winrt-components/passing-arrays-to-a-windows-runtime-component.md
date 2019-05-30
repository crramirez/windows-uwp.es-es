---
title: Pasar matrices a un componente de Windows Runtime
description: En la Plataforma universal de Windows (UWP), los parámetros son o bien para la entrada, o bien para la salida, nunca para ambas. Esto significa que el contenido de una matriz que se pasa a un método, así como la propia matriz, se dispondrá para la entrada o bien para la salida.
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aedf33b6fab55aeb217a3b41a22b55f1cc5c9c5f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372505"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>Pasar matrices a un componente de Windows Runtime




En la Plataforma universal de Windows (UWP), los parámetros son o bien para la entrada, o bien para la salida, nunca para ambas. Esto significa que el contenido de una matriz que se pasa a un método, así como la propia matriz, se dispondrá para la entrada o bien para la salida. Si el contenido de la matriz es para la entrada, el método lee la matriz pero no escribe en ella. Si el contenido de la matriz es para la salida, el método escribe en la matriz pero no la lee. Esto representa un problema para los parámetros de matriz, porque las matrices de .NET Framework son tipos de referencia y el contenido de una matriz es mutable incluso cuando la referencia de la matriz se pasa por valor (**ByVal** en Visual Basic). La [Herramienta de exportación de metadatos de Windows Runtime (Winmdexp.exe)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) requiere que especifiques el uso previsto de la matriz si no está claro a partir del contexto, mediante la aplicación del atributo ReadOnlyArrayAttribute o el atributo WriteOnlyArrayAttribute en el parámetro. El uso de matrices se determina de la siguiente manera:

-   Para el valor devuelto o para un parámetro de salida (un parámetro **ByRef** con el atributo [OutAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.outattribute?redirectedfrom=MSDN) en Visual Basic), la matriz siempre es solo de salida. No apliques el atributo ReadOnlyArrayAttribute. El atributo WriteOnlyArrayAttribute se permite en los parámetros de salida, pero es redundante.

    > **Precaución**  compilador de Visual Basic no aplica reglas de solo salida. Nunca debes leer desde un parámetro de salida; puesto es posible que no contenga **nada**. Asigna siempre una nueva matriz.
 
-   Los parámetros que tienen el modificador **ref** (**ByRef** en Visual Basic) no están permitidos. Winmdexp.exe genera un error.
-   Para un parámetro que se pasa por valor, debes especificar si el contenido de la matriz es para la entrada o la salida aplicando el atributo [ReadOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute?redirectedfrom=MSDN) o el atributo [WriteOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute?redirectedfrom=MSDN). Especificar ambos atributos es un error.

Si un método debe aceptar una matriz de entrada, modifica el contenido de la matriz y devuelve la matriz al llamador, usa un parámetro de solo lectura para la entrada y un parámetro de solo escritura (o el valor devuelto) para la salida. El siguiente código muestra una manera de implementar este patrón:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public int[] ChangeArray([ReadOnlyArray()] int[] input)
> {
>     int[] output = input.Clone();
>     // Manipulate the copy.
>     //   ...
>     return output;
> }
> ```
> ```vb
> Public Function ChangeArray(<ReadOnlyArray> input() As Integer) As Integer()
>     Dim output() As Integer = input.Clone()
>     ' Manipulate the copy.
>     '   ...
>     Return output
> End Function
> ```

Te recomendamos que hagas una copia de la matriz de entrada inmediatamente y manipules la copia. Esto ayuda a garantizar que el método se comporte del mismo modo independientemente de si tu componente es llamado por el código de .NET Framework.

## <a name="using-components-from-managed-and-unmanaged-code"></a>Usar componentes de código administrado y no administrado


Los parámetros que tienen el atributo ReadOnlyArrayAttribute o el atributo WriteOnlyArrayAttribute se comportan de forma diferente dependiendo de si el llamador está escrito en código nativo o código administrado. Si el llamador está en código nativo (extensiones de componentes de JavaScript o Visual C++), el contenido de la matriz se trata como sigue:

-   ReadOnlyArrayAttribute: La matriz se copia cuando la llamada cruza el límite de la interfaz binaria (ABI) de la aplicación. Si es necesario, se convierten los elementos. Por lo tanto, los cambios accidentales que realiza el método en una matriz solo de entrada no son visibles para el llamador.
-   WriteOnlyArrayAttribute: El método llamado no puede hacer ninguna suposición sobre el contenido de la matriz original. Por ejemplo, la matriz que recibe el método podría no inicializarse o es posible que contenga valores predeterminados. Se espera que el método establezca valores de todos los elementos en la matriz.

Si el llamador es código administrado, la matriz original está disponible para el método llamado, del mismo modo que en cualquier llamada de método en .NET Framework. El contenido de la matriz es mutable en el código .NET Framework, por lo que cualquier cambio que realice el método en la matriz es visible para el llamador. Esto debe recordarse porque afecta a pruebas unitarias escritas para un componente de Windows Runtime. Si las pruebas se escriben en código administrado, el contenido de una matriz aparecerá como mutable durante las pruebas.

## <a name="related-topics"></a>Temas relacionados

* [ReadOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute?redirectedfrom=MSDN)
* [WriteOnlyArrayAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute?redirectedfrom=MSDN)
* [Creación de componentes de Windows Runtime en C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
