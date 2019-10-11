---
title: Diagnosticar condiciones de error del componente de Windows Runtime
description: En este artículo se proporciona información adicional sobre las restricciones en Windows Runtime componentes escritos con código administrado.
ms.assetid: CD0D0E11-E68A-411D-B92E-E9DECFDC9599
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 55bf6360f09ba4ab6c7878543ecfa0c80c4558e3
ms.sourcegitcommit: 74c674c70b86bafeac7c8c749b1662fae838c428
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72252313"
---
# <a name="diagnosing-windows-runtime-component-error-conditions"></a>Diagnosticar condiciones de error del componente de Windows Runtime

En este artículo se proporciona información adicional sobre las restricciones en Windows Runtime componentes escritos con código administrado. Amplía la información que se proporciona en los mensajes de error de [Winmdexp. exe (Windows Runtime herramienta de exportación de metadatos)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)y complementa la información sobre las restricciones que se proporcionan en [los componentes C# de Windows Runtime con y visual Básico](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

En este artículo no se abarcan todos los errores. Los errores que se tratan aquí se agrupan por categoría general, y cada categoría incluye una tabla de mensajes de error relacionados. Busca el texto del mensaje (omitiendo valores específicos de marcadores de posición) o el número de mensaje. Si no encuentras la información que necesitas aquí, por favor, ayúdanos a mejorar la documentación con el botón de comentarios al final de este artículo. Incluye el mensaje del error. Como alternativa, puedes presentar un error en el sitio Web de Microsoft Connect.

## <a name="error-message-for-implementing-async-interface-provides-incorrect-type"></a>El mensaje de error para implementar la interfaz asincrónica proporciona un tipo incorrecto

Los componentes de Windows Runtime administrados no pueden implementar las interfaces de Plataforma universal de Windows (UWP) que representan acciones u operaciones asincrónicas ([IAsyncAction](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction), [IAsyncActionWithProgress @ no__t-2TProgress @ no__t-3](https://docs.microsoft.com/previous-versions/br205784(v=vs.85)), [ IAsyncOperation @ no__t-5TResult @ no__t-6](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperation_TResult_)o [IAsyncOperationWithProgress @ No__t-8TResult, TProgress @ no__t-9](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_)). En su lugar, .NET proporciona la clase [AsyncInfo](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime) para generar operaciones asincrónicas en Windows Runtime componentes. El mensaje de error que Winmdexp.exe muestra cuando intentas implementar una interfaz asincrónica incorrectamente hace referencia a esta clase por su nombre anterior, AsyncInfoFactory. .NET ya no incluye la clase AsyncInfoFactory.

| Número de error | Texto de mensaje|       
|--------------|-------------|
| WME1084      | El tipo ' {0} ' implementa Windows Runtime interfaz asincrónica ' {1} '. Los tipos de Windows Runtime no pueden implementar interfaces asincrónicas. Usa la clase System.Runtime.InteropServices.WindowsRuntime.AsyncInfoFactory para generar las operaciones asincrónicas para exportar a Windows Runtime. |

> **Tenga en cuenta** El mensaje de error que hace referencia al Windows Runtime usar una terminología antigua. Esta se denomina ahora Plataforma universal de Windows (UWP). Por ejemplo, los tipos de Windows Runtime ahora se denominan tipos UWP.

## <a name="missing-references-to-mscorlibdll-or-systemruntimedll"></a>Referencias ausentes a mscorlib.dll o System.Runtime.dll

Este problema se produce únicamente cuando se utiliza Winmdexp.exe desde la línea de comandos. Se recomienda usar la opción/Reference para incluir referencias a mscorlib. dll y System. Runtime. dll desde los ensamblados de referencia de .NET Framework Core, que se encuentran en "% ProgramFiles (x86)% \\Reference assemblies @ no__t-1Microsoft @ no__t-2Framework @ no__t-3. NETCore @ no__t-4V 4.5" ("% ProgramFiles% \\..." en un equipo de 32 bits).

| Número de error | Texto de mensaje                                                                                                                                     |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1009      | No se ha hecho ninguna referencia a mscorlib.dll. Se requiere una referencia a estos metadatos para exportar correctamente.                               |
| WME1090      | No se pudo determinar el ensamblado de referencia principal. Asegúrese de que mscorlib.dll y System.Runtime.dll se mencionan mediante el conmutador /reference. |

## <a name="operator-overloading-is-not-allowed"></a>No se permite la sobrecarga de los operadores

En un componente de Windows Runtime escrito en código administrado, no se pueden exponer operadores sobrecargados en tipos públicos.

> **Tenga en cuenta**@no__t 1in el mensaje de error, el operador se identifica mediante su nombre de metadatos, como OP @ No__t-2Addition, op @ No__t-3Multiply, op @ No__t-4ExclusiveOr, op @ No__t-5Implicit (conversión implícita), etc.

| Número de error | Texto de mensaje                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------------|
| WME1087      | ' {0} ' es una sobrecarga de operador. Los tipos administrados no pueden exponer sobrecargas de operadores en Windows Runtime. |

## <a name="constructors-on-a-class-have-the-same-number-of-parameters"></a>Los constructores en una clase tienen el mismo número de parámetros

En el UWP, una clase puede tener solo un constructor con un determinado número de parámetros. Por ejemplo, no puedes tener un constructor que tenga un único parámetro de tipo **String** y otro que tenga un único parámetro de tipo **int** (**Integer** en Visual Basic). La única solución alternativa es usar un número de parámetros diferente para cada constructor.

| Número de error | Texto de mensaje                                                                                                                                            |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1099      | El tipo ' {0} ' tiene varios constructores con ' {1} ' argumento (s). Los tipos de Windows Runtime no pueden tener varios constructores con el mismo número de argumentos. |

## <a name="must-specify-a-default-for-overloads-that-have-the-same-number-of-parameters"></a>Debes especificar un valor predeterminado para las sobrecargas que tienen el mismo número de parámetros

En la UWP los métodos sobrecargados pueden tener el mismo número de parámetros solo si se especifica una sobrecarga como sobrecarga predeterminada. Consulte "métodos sobrecargados" en [Windows Runtime componentes con C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

| Número de error | Texto de mensaje                                                                                                                                                                      |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1059      | Varias sobrecargas de @no__t -0-Parameter de ' {1}. {2} ' están decoradas con Windows. Foundation. Metadata. DefaultOverloadAttribute.                                                            |
| WME1085      | Las sobrecargas de @no__t -0-Parameter de {1}. @no__t 2 deben tener exactamente un método especificado como sobrecarga predeterminada al decorarlo con Windows. Foundation. Metadata. DefaultOverloadAttribute. |

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>Errores de espacio de nombres y nombres inválidos para el archivo de salida

En la Plataforma universal de Windows, todos los tipos públicos en un archivo de metadatos (.winmd) de Windows deben estar en un espacio de nombres que comparta el nombre del archivo .winmd o en subespacios de nombres del nombre de archivo. Por ejemplo, si el proyecto de Visual Studio se denomina A.B (es decir, el componente de Windows Runtime es A.B.winmd), puede contener las clases públicas A.B.Class1 y A.B.C.Class2, pero no la A.Class3 (WME0006) o D.Class4 (WME1044).

> **Nota**  Estas restricciones se aplican solo a los tipos públicos, no a los tipos privados usados en tu implementación.

En el caso de A.Class3, puedes o bien mover Class3 a otro espacio de nombres o bien cambiar el nombre del componente de Windows Runtime a A.winmd. Aunque WME0006 es una advertencia, debes tratarla como un error. En el ejemplo anterior, el código que llama a A.B.winmd no podrá encontrar A.Class3.

En el caso de D.Class4, ningún nombre de archivo puede contener tanto D.Class4 como las clases en el espacio de nombres A.B, por lo que cambiar el nombre del componente de Windows Runtime no es una opción. Puedes mover D.Class4 a otro espacio de nombres o incluirlo en otro componente de Windows Runtime.

El sistema de archivos no distingue entre mayúsculas y minúsculas, por lo que los espacios de nombres que varían según el caso no están permitidos (WME1067).

El componente debe contener al menos un tipo **public sealed** (**Public NotInheritable** en Visual Basic). De lo contrario, obtendrás WME1042 o WME1043, en función de si tu componente contiene tipos privados.

Un tipo en un componente de Windows Runtime no puede tener un nombre idéntico al de un espacio de nombres (WME1068).

> **Precaución**  Si llamas directamente a Winmdexp.exe y no usas la opción /out para especificar un nombre para el componente de Windows Runtime, Winmdexp.exe intenta generar un nombre que incluya todos los espacios de nombres en el componente. Cambiar el nombre de los espacios de nombres puede alterar el nombre de tu componente.

 

| Número de error | Texto de mensaje                                                                                                                                                                                                                                                                                                                                             |
|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME0006      | ' {0} ' no es un nombre de archivo winmd válido para este ensamblado. Todos los tipos dentro de un archivo de metadatos de Windows deben existir en un subespacio de nombres del espacio de nombres que se determina según el nombre de archivo. Los tipos que no existen en dicho subespacio de nombres no pueden localizarse en el tiempo de ejecución. En este ensamblado, el espacio de nombres comunes más pequeño que puede actuar como un nombre de archivo es '{1}'. |
| WME1042      | El módulo de entrada debe contener al menos un tipo público que se encuentre dentro de un espacio de nombres.                                                                                                                                                                                                                                                                   |
| WME1043      | El módulo de entrada debe contener al menos un tipo público que se encuentre dentro de un espacio de nombres. Los únicos tipos que se encuentran dentro de espacios de nombres son privados.                                                                                                                                                                                                               |
| WME1044      | Un tipo público tiene un espacio de nombres (' {1} ') que no comparte ningún prefijo común con otros espacios de nombres (' {0} '). Todos los tipos dentro de un archivo de metadatos de Windows deben existir en un subespacio de nombres del espacio de nombres que se determina según el nombre de archivo.                                                                                                                              |
| WME1067      | Los nombres de espacios de nombres no pueden diferenciarse solo por el uso de mayúsculas y minúsculas: ' {0} ', ' {1} '.                                                                                                                                                                                                                                                                                                |
| WME1068      | El tipo ' {0} ' no puede tener el mismo nombre que el espacio de nombres ' {1} '.                                                                                                                                                                                                                                                                                                 |

## <a name="exporting-types-that-arent-valid-universal-windows-platform-types"></a>Exportar tipos que no son tipos válidos de la Plataforma universal de Windows

La interfaz pública de tu componente debe exponer solo los tipos UWP. Sin embargo, .NET proporciona asignaciones para varios tipos de uso frecuente que son ligeramente diferentes en .NET y UWP. Esto permite al desarrollador de .NET trabajar con tipos conocidos en lugar de aprender otros nuevos. Puede usar estos tipos .NET asignados en la interfaz pública de su componente. Vea "declarar tipos en componentes de Windows Runtime" y "pasar tipos de Plataforma universal de Windows a código administrado" en [Windows Runtime componentes C# con y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md), y [asignaciones de .net de tipos de Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

Muchas de estas asignaciones son interfaces. Por ejemplo, [IList&lt;T&gt;](https://docs.microsoft.com/dotnet/api/system.collections.generic.ilist-1) se asigna a la interfaz UWP [IVector&lt;T&gt;](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IVector_T_). Si usas List&lt;string&gt; (`List(Of String)` en Visual Basic) en lugar de IList&lt;string&gt; como tipo de parámetro, Winmdexp.exe proporciona una lista de alternativas que incluye todas las interfaces asignadas implementadas por List&lt;T&gt;. Si usas los tipos genéricos anidados, como List&lt;Dictionary&lt;int, string&gt;&gt; (List(Of Dictionary(Of Integer, String)) en Visual Basic), Winmdexp.exe ofrece opciones para cada nivel de anidación. Estas listas pueden ser bastante largas.

En general, la mejor opción es la interfaz que más se asemeje al tipo. Por ejemplo, para Dictionary&lt;int, string&gt;, la mejor opción es probablemente IDictionary&lt;int, string&gt;.

> **Importante**  JavaScript usa la interfaz que aparece en primer lugar en la lista de interfaces que implementa un tipo administrado. Por ejemplo, si devuelves Dictionary&lt;int, string&gt; al código JavaScript, aparece como IDictionary&lt;int, string&gt; independientemente de qué interfaz especifiques como tipo devuelto. Esto significa que si la primera interfaz no incluye a un miembro que aparece en las últimas interfaces, ese miembro no es visible para JavaScript.

> **Precaución**  Evita el uso de las interfaces no genéricas [IList](https://docs.microsoft.com/dotnet/api/system.collections.ilist) o [IEnumerable](https://docs.microsoft.com/dotnet/api/system.collections.ienumerable) si JavaScript usará tu componente. Estas interfaces se asignan a [IBindableVector](https://docs.microsoft.com/uwp/api/windows.ui.xaml.interop.ibindablevector) y [IBindableIterator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.interop.ibindableiterator), respectivamente. Se admiten los enlaces para los controles de XAML y no son visibles para JavaScript. JavaScript emite el error de tiempo de ejecución "La función 'X' tiene una firma no válida y no se puede llamar".

 

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Número de error</th>
<th align="left">Texto de mensaje</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">WME1033</td>
<td align="left">El método ' {0} ' tiene el parámetro ' {1} ' del tipo ' {2} '. '{2}' no es un tipo de parámetro válido de Windows Runtime.</td>
</tr>
<tr class="even">
<td align="left">WME1038</td>
<td align="left">El método ' {0} ' tiene un parámetro de tipo ' {1} ' en su signatura. Aunque este tipo no es un tipo válido de Windows Runtime, implementa interfaces que son tipos válidos de Windows Runtime. Considera la posibilidad de cambiar la firma de método para utilizar uno de los siguientes tipos en su lugar: '{2}'.</td>
</tr>
<tr class="odd">
<td align="left">WME1039</td>
<td align="left"><p>El método ' {0} ' tiene un parámetro de tipo ' {1} ' en su signatura. Aunque este tipo genérico no es un tipo válido de Windows Runtime, el tipo o sus parámetros genéricos implementan interfaces que son tipos válidos de Windows Runtime. [https://doi.org/10.13012/J8PN93H8]({2})</p>
> **Note @ no__t-1 para {2}, Winmdexp. exe anexa una lista de alternativas, como "considere la posibilidad de cambiar el tipo ' System. Collections. Generic. List @ no__t-3T @ no__t-4 ' en la Signatura del método a uno de los siguientes tipos: ' System. Collections. Generic. IList @ no__t-0T @ no__t-1, System. Collections. Generic. IReadOnlyList @ no__t-2T @ no__t-3, System. Collections. Generic. IEnumerable @ no__t-4T @ no__t-5 '. '
</td>
</tr>
<tr class="even">
<td align="left">WME1040</td>
<td align="left">El método ' {0} ' tiene un parámetro de tipo ' {1} ' en su signatura. En lugar de usar un tipo de tarea administrado, usa Windows.Foundation.IAsyncAction, Windows.Foundation.IAsyncOperation o una de las otras interfaces asincrónicas de Windows Runtime. El patrón await de .NET estándar también se aplica a estas interfaces. Por favor, consulta System.Runtime.InteropServices.WindowsRuntime.AsyncInfo para obtener más información acerca de la conversión de objetos de tareas administradas a interfaces asincrónicas de Windows Runtime.</td>
</tr>
</tbody>
</table>

 

## <a name="structures-that-contain-fields-of-disallowed-types"></a>Estructuras que contienen campos de tipos no autorizados


En la UWP una estructura puede contener solo campos, y solo las estructuras pueden contener campos. Estos campos deben ser públicos. Los tipos de campos válidos incluyen tipos primitivos, estructuras y enumeraciones.

| Número de error | Texto de mensaje                                                                                                                                                                                                                                                            |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| WME1060      | La estructura ' {0} ' tiene el campo ' {1} ' del tipo ' {2} '. '{2}' no es un tipo de campo válido de Windows Runtime. Cada campo en una estructura de Windows Runtime solo puede ser UInt8, Int16, UInt16, Int32, UInt32, Int64, UInt64, Single, Double, Boolean, String, Enum o una estructura en sí mismo. |

 

## <a name="restrictions-on-arrays-in-member-signatures"></a>Restricciones en las matrices en signaturas de miembros


En la UWP, las matrices en las signaturas de miembros deben ser unidimensionales con un límite inferior de 0 (cero). Los tipos de matrices anidadas como `myArray[][]` (`myArray()()` en Visual Basic) no están permitidos.

> **Tenga en cuenta**que @no__t restricción 1This no se aplica a las matrices que se usan internamente en su implementación.

 

| Número de error | Texto de mensaje                                                                                                                                                     |
|--------------|--------------------|
| WME1034      | El método ' {0} ' tiene una matriz de tipo ' {1} ' con un límite inferior distinto de cero en la firma. Las matrices de firmas de método de Windows Runtime deben tener un límite inferior de cero. |
| WME1035      | El método ' {0} ' tiene una matriz multidimensional de tipo ' {1} ' en su signatura. Las matrices de firmas de método de Windows Runtime deben ser unidimensionales.                  |
| WME1036      | El método ' {0} ' tiene una matriz anidada de tipo ' {1} ' en su signatura. Las matrices de firmas de método de Windows Runtime no pueden anidarse.                                    |

 

## <a name="array-parameters-must-specify-whether-array-contents-are-readable-or-writable"></a>Los parámetros de matriz deben especificar si el contenido de la matriz es de lectura o escritura.


En la UWP, los parámetros deben ser de solo lectura o de solo escritura. No se pueden marcar parámetros **ref** (**ByRef** sin el atributo [OutAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.outattribute) en Visual Basic). Esto se aplica al contenido de las matrices, por lo que los parámetros de las matrices deben indicar si el contenido de la matriz es de solo lectura o solo escritura. La dirección es clara para los parámetros **out** (parámetro **ByRef** con el atributo OutAttribute en Visual Basic), pero los parámetros de matrices pasados por valor (ByVal en Visual Basic) deben estar marcados. Consulta [Pasar matrices a un componente de Windows Runtime](passing-arrays-to-a-windows-runtime-component.md).

| Número de error | Texto de mensaje         |
|--------------|----------------------|
| WME1101      | El método ' {0} ' tiene el parámetro ' {1} ', que es una matriz y tiene @no__t 2 y {3}. En Windows Runtime, los parámetros de la matriz de contenido deben ser o bien de lectura o bien de escritura. Quita uno de los atributos de '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| WME1102      | El método ' {0} ' tiene un parámetro de salida ' {1} ' que es una matriz, pero que tiene {2}. En Windows Runtime, el contenido de las matrices de salida es modificable. Quita el atributo de '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| WME1103      | El método ' {0} ' tiene el parámetro ' {1} ', que es una matriz y tiene System. Runtime. InteropServices. InAttribute o System. Runtime. InteropServices. OutAttribute. En Windows Runtime, los parámetros de la matriz deben tener {2} o {3}. Quita estos atributos o reemplázalos con el atributo de Windows Runtime apropiado si es necesario.                                                                                                                                                                                                                                                                                                                                                                                          |
| WME1104      | El método ' {0} ' tiene el parámetro ' {1} ', que no es una matriz y tiene un @no__t 2 o un {3}. Windows Runtime no es compatible con parámetros que no son de matriz de marcado con {2} o {3}.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| WME1105      | El método ' {0} ' tiene el parámetro ' {1} ' con System. Runtime. InteropServices. InAttribute o System. Runtime. InteropServices. OutAttribute. Windows Runtime no es compatible con parámetros de marcado con System.Runtime.InteropServices.InAttribute o System.Runtime.InteropServices.OutAttribute. Por favor, considera la posibilidad de quitar System.Runtime.InteropServices.InAttribute y reemplaza System.Runtime.InteropServices.OutAttribute por el modificador 'out' en su lugar. El método ' {0} ' tiene el parámetro ' {1} ' con System. Runtime. InteropServices. InAttribute o System. Runtime. InteropServices. OutAttribute. Windows Runtime solo es compatible con los parámetros ByRef de marcado con System.Runtime.InteropServices.OutAttribute y no es compatible con otros usos de estos atributos. |
| WME1106      | El método ' {0} ' tiene el parámetro ' {1} ', que es una matriz. En Windows Runtime, el contenido de los parámetros de la matriz debe ser o bien de lectura o bien de escritura. Aplica o bien {2} o bien {3} a '{1}'.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |


## <a name="member-with-a-parameter-named-value"></a>Miembro con un parámetro denominado "valor"


En el UWP, los valores devueltos se considera que son parámetros de salida y los nombres de los parámetros deben ser únicos. De forma predeterminada, Winmdexp.exe proporciona al valor devuelto el nombre de "value". Si tu método tiene un parámetro denominado "value", obtendrás el error WME1092. Hay dos formas de corregirlo:

-   Asigna a tu parámetro un nombre distinto a "value" (en descriptores de acceso de propiedad, un nombre distinto a "returnValue").
-   Usa el atributo ReturnValueNameAttribute para cambiar el nombre del valor devuelto, como se muestra aquí:

    > [!div class="tabbedCodeSnippets"]
    > ```cs
    > using System.Runtime.InteropServices;
    > using System.Runtime.InteropServices.WindowsRuntime;
    >
    > [return: ReturnValueName("average")]
    > public int GetAverage(out int lowValue, out int highValue)
    > ```
    > ```vb
    > Imports System.Runtime.InteropServices
    > Imports System.Runtime.InteropServices.WindowsRuntime
    >
    > Public Function GetAverage(<Out> ByRef lowValue As Integer, _
    > <Out> ByRef highValue As Integer) As <ReturnValueName("average")> String
    > ```

> **Nota**  Si cambias el nombre del valor devuelto y el nuevo nombre entra en conflicto con el nombre de otro parámetro, obtendrás el error WME1091.

El código de JavaScript puede acceder a los parámetros de salida de un método por nombre, incluido el valor devuelto. Por ejemplo, consulta el atributo [ReturnValueNameAttribute](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime.returnvaluenameattribute).

| Número de error | Texto de mensaje |
|--------------|--------------|
| WME1091 | El método ' \{0} ' tiene el valor devuelto con el nombre ' \{1} ', que es igual que un nombre de parámetro. Los parámetros de método de Windows Runtime y el valor devuelto deben tener nombres únicos. |
| WME1092 | El método ' \{0} ' tiene un parámetro denominado ' \{1} ', que es igual que el nombre del valor devuelto predeterminado. Considera la posibilidad de usar otro nombre para el parámetro o usa System.Runtime.InteropServices.WindowsRuntime.ReturnValueNameAttribute para especificar explícitamente el nombre del valor devuelto. |

**Nota**  El nombre predeterminado es "returnValue" para los descriptores de acceso de propiedad y "value" para todos los otros métodos.

## <a name="related-topics"></a>Temas relacionados

* [Componentes de Windows Runtime con C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [Winmdexp. exe (herramienta de exportación de metadatos de Windows Runtime)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool)
