---
author: stevewhims
description: Respuestas a preguntas que probablemente tengas acerca de la creación y consumo de las API de Windows Runtime con C++/WinRT.
title: Preguntas más frecuentes sobre C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, preguntas más frecuentes, P+F
ms.localizationpriority: medium
ms.openlocfilehash: aad5c5ed2123af39ebb6aff0c9098586ce958196
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832042"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Preguntas más frecuentes sobre [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Respuestas a preguntas que probablemente tengas acerca de la creación y consumo de las API de Windows Runtime con C++/WinRT.

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>¿Cuáles son los requisitos para la [extensión de Visual Studio (VSIX)](https://aka.ms/cppwinrt/vsix) C++/WinRT?
La extensión [VSIX](https://aka.ms/cppwinrt/vsix) exige una versión de destino de Windows SDK mínima de 10.0.17134.0 (Windows 10, versión 1803). También deberás contar con la versión 15.6 de Visual Studio 2017 o posterior. Puedes identificar un proyecto que usa la extensión VSIX mediante la presencia de `<CppWinRTEnabled>true</CppWinRTEnabled>` en `<PropertyGroup Label="Globals">` en el archivo `.vcxproj`. Para obtener más información, consulta [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="whats-a-runtime-class"></a>¿Qué es una *clase en tiempo de ejecución*?
Una clase en tiempo de ejecución es un tipo que puede activarse y consumirse a través de interfaces de COM modernas, normalmente a través de límites de archivo ejecutables. Sin embargo, una clase en tiempo de ejecución también puede usarse dentro de la unidad de compilación que la implementa. Al declarar una clase en tiempo de ejecución en el lenguaje de definición de interfaz (IDL), puedes implementarla en C++ estándar con C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>¿Qué significa *el tipo proyectado* y *el tipo de implementación*?
Si solo vas a *consumir* una clase de Windows Runtime (clase en tiempo de ejecución), tratarás exclusivamente con *tipos proyectados*. C++/WinRT es una *proyección de lenguaje*, de modo que los tipos proyectados forman parte de la superficie de Windows Runtime que se *proyecta* en C++ con C++/WinRT. Para obtener más información, consulta [Consumir API con C++/WinRT](consume-apis.md).

El *tipo de implementación* contiene la implementación de una clase en tiempo de ejecución, por lo que solo está disponible en el proyecto que implementa la clase en tiempo de ejecución. Cuando estés trabajando en un proyecto que implemente clases en tiempo de ejecución (un proyecto de componente de Windows Runtime o un proyecto que use interfaz de usuario XAML), es importante que te sientas cómodo con la distinción entre tu tipo de implementación para una clase en tiempo de ejecución y el tipo proyectado que representa la clase en tiempo de ejecución proyectada en C++/WinRT. Para obtener más información, consulta [Crear API con C++/WinRT](author-apis.md).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>¿Debo implementar [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)? Y, si es así, ¿cómo?
Si tienes una clase en tiempo de ejecución que libera recursos en su destructor, y si dicha clase en tiempo de ejecución se ha diseñado para usarse desde fuera de su unidad de compilación de implementación (es un componente de Windows Runtime destinado al consumo general por aplicaciones cliente Windows Runtime), te recomendamos que también implementes **IClosable** para poder admitir el consumo de tu clase en tiempo de ejecución con lenguajes que carecen de finalización determinista. Asegúrate de que tus recursos se liberan si se llama al constructor [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close) o a ambos. **IClosable::Close** puede llamarse un número arbitrario de veces.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>¿Tengo que llamar a [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) en las clases en tiempo de ejecución que consumo?
**IClosable** existe para admitir los lenguajes que carecen de finalización determinista. Por lo tanto, no debes llamar a **IClosable::Close** desde C++/WinRT, excepto en casos muy raros que impliquen carreras de apagado o interbloqueos. Si vas a usar tipos **Windows.UI.Composition**, por ejemplo, puede que te encuentres con casos en los que quieras deshacerte de objetos en una secuencia definida, como una alternativa para permitir que la destrucción del contenedor C++/WinRT haga el trabajo por ti.

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>¿Tengo que declarar un constructor en el archivo IDL de mi clase en tiempo de ejecución?
Solo si la clase en tiempo de ejecución se ha diseñado para consumirse desde fuera de su unidad de compilación de implementación (es un componente de Windows Runtime destinado al consumo general por aplicaciones cliente de Windows Runtime). Para obtener información completa sobre el propósito y las consecuencias de declarar constructores en IDL, consulta [Constructores de clases en tiempo de ejecución](author-apis.md#runtime-class-constructors).