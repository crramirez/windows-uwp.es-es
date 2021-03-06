---
title: Pasar matrices a un componente de Windows Runtime
description: En la Plataforma universal de Windows (UWP), los parámetros son o bien para la entrada, o bien para la salida, nunca para ambas. Esto significa que el contenido de una matriz que se pasa a un método, así como la propia matriz, se dispondrá para la entrada o bien para la salida.
ms.assetid: 8DE695AC-CEF2-438C-8F94-FB783EE18EB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f38538d1a77bdeb6a497eda5802f712f23002b54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155189"
---
# <a name="passing-arrays-to-a-windows-runtime-component"></a>Pasar matrices a un componente de Windows Runtime




En la Plataforma universal de Windows (UWP), los parámetros son o bien para la entrada, o bien para la salida, nunca para ambas. Esto significa que el contenido de una matriz que se pasa a un método, así como la propia matriz, se dispondrá para la entrada o bien para la salida. Si el contenido de la matriz es para la entrada, el método lee la matriz pero no escribe en ella. Si el contenido de la matriz es para la salida, el método escribe en la matriz pero no la lee. Esto supone un problema para los parámetros de matriz, ya que las matrices en .NET son tipos de referencia y el contenido de una matriz es mutable incluso cuando la referencia de matriz se pasa por valor (**ByVal** en Visual Basic). La [Herramienta de exportación de metadatos de Windows Runtime (Winmdexp.exe)](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) requiere que especifiques el uso previsto de la matriz si no está claro a partir del contexto, mediante la aplicación del atributo ReadOnlyArrayAttribute o el atributo WriteOnlyArrayAttribute en el parámetro. El uso de matrices se determina de la siguiente manera:

-   Para el valor devuelto o para un parámetro de salida (un parámetro **ByRef** con el atributo [OutAttribute](/dotnet/api/system.runtime.interopservices.outattribute) en Visual Basic), la matriz siempre es solo de salida. No apliques el atributo ReadOnlyArrayAttribute. El atributo WriteOnlyArrayAttribute se permite en los parámetros de salida, pero es redundante.

    > **PRECAUCIÓN**    El compilador Visual Basic no aplica reglas de solo salida. Nunca debes leer desde un parámetro de salida; puesto es posible que no contenga **nada**. Asigna siempre una nueva matriz.
 
-   Los parámetros que tienen el modificador **ref** (**ByRef** en Visual Basic) no están permitidos. Winmdexp.exe genera un error.
-   Para un parámetro que se pasa por valor, debes especificar si el contenido de la matriz es para la entrada o la salida aplicando el atributo [ReadOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute) o el atributo [WriteOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute). Especificar ambos atributos es un error.

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

Te recomendamos que hagas una copia de la matriz de entrada inmediatamente y manipules la copia. Esto ayuda a garantizar que el método se comporta de la misma forma que el código de .NET llama o no al componente.

## <a name="using-components-from-managed-and-unmanaged-code"></a>Usar componentes de código administrado y no administrado


Los parámetros que tienen el atributo ReadOnlyArrayAttribute o el atributo WriteOnlyArrayAttribute se comportan de forma diferente dependiendo de si el llamador está escrito en código nativo o código administrado. Si el llamador está en código nativo (extensiones de componentes de JavaScript o Visual C++), el contenido de la matriz se trata como sigue:

-   ReadOnlyArrayAttribute: la matriz se copia cuando la llamada cruza los límites de la interfaz binaria (ABI) de la aplicación. Si es necesario, se convierten los elementos. Por lo tanto, cualquier cambio accidental que el método realiza en una matriz solo de entrada no es visible para el llamador.
-   WriteOnlyArrayAttribute: el método llamado no puede hacer ninguna suposición sobre el contenido de la matriz original. Por ejemplo, la matriz que recibe el método podría no inicializarse o podría contener valores predeterminados. Se espera que el método establezca los valores de todos los elementos de la matriz.

Si el autor de la llamada es código administrado, la matriz original está disponible para el método llamado, como sería en cualquier llamada al método en .NET. El contenido de la matriz es mutable en el código .NET, por lo que cualquier cambio que realice el método en la matriz es visible para el autor de la llamada. Esto debe recordarse porque afecta a pruebas unitarias escritas para un componente de Windows Runtime. Si las pruebas se escriben en código administrado, el contenido de una matriz parecerá mutable durante la prueba.

## <a name="related-topics"></a>Temas relacionados

* [ReadOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.readonlyarrayattribute)
* [WriteOnlyArrayAttribute](/dotnet/api/system.runtime.interopservices.windowsruntime.writeonlyarrayattribute)
* [Componentes de Windows Runtime con C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)