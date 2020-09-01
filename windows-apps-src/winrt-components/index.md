---
title: Componentes de Windows Runtime
description: Los componentes de Windows Runtime son objetos independientes que permiten crear una instancia y se pueden usar desde cualquier lenguaje, como C#, Visual Basic, JavaScript y C++.
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b01aaff3a0a273ccbf5327d54bb01ae67dd3774e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174249"
---
# <a name="windows-runtime-components"></a>Componentes de Windows Runtime

Un componente de Windows Runtime es un módulo de software independiente al que puede hacer referencia y que puede usar con cualquier lenguaje de Windows Runtime (como C#, C++/WinRT, Visual Basic, JavaScript y C++/CX). Puede usar Visual Studio para crear un componente de Windows Runtime que pueda usarse en la aplicación de la Plataforma universal de Windows (UWP).

> [!NOTE]
> Si eres desarrollador de C++, te recomendamos que uses [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para las nuevas aplicaciones. C++/WinRT es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para darte acceso de primera clase a la API moderna de Windows. Para obtener información sobre cómo crear un componente de Windows Runtime con C++/WinRT, consulte [Componentes de Windows Runtime con C++/WinRT](./create-a-windows-runtime-component-in-cppwinrt.md).

| Tema | Descripción |
|-------|-------------|
| [Componentes de Windows Runtime con C++/WinRT](./create-a-windows-runtime-component-in-cppwinrt.md) | En este tema se muestra cómo usar C++/WinRT para crear y consumir un componente de Windows Runtime&mdash;, que es un componente que se puede llamar desde una aplicación universal de Windows compilada mediante el lenguaje de Windows Runtime. |
| [Componentes de Windows Runtime con C++/CX](creating-windows-runtime-components-in-cpp.md) | En este tema se muestra cómo usar C++/CX para crear un componente de Windows Runtime&mdash;, que es un componente que se puede llamar desde una aplicación universal de Windows compilada mediante el lenguaje de Windows Runtime. |
| [Tutorial para crear un componente de Windows Runtime en C++/CX y llamarlo desde JavaScript o C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | En este tutorial se muestra cómo crear un archivo DLL básico del componente de Windows Runtime que se pueda llamar desde JavaScript, C# o Visual Basic. Antes de comenzar este tutorial, asegúrate de que conoces conceptos como la interfaz binaria abstracta (ABI), las clases de referencia y las extensiones del componente de Visual C++, que facilitan el trabajo con clases de referencia. Para obtener más información, consulta [Crear componentes de Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) y [Referencia del lenguaje de Visual C++ (C++/CX)](/cpp/cppcx/visual-c-language-reference-c-cx). |
| [Componentes de Windows Runtime con C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | Puedes usar código administrado para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime. Puedes usar tu componente en aplicaciones de la Plataforma universal de Windows (UWP) con C++, JavaScript, Visual Basic o C#. En este tema se describen las reglas de creación de un componente y se analizan algunos aspectos de la compatibilidad con .NET para Windows Runtime. En general, esa compatibilidad está diseñada para ser transparente para los programadores de .NET. Sin embargo, cuando creas un componente para su uso con JavaScript o C++, debes tener en cuenta las diferencias en la forma en que esos lenguajes son compatibles con Windows Runtime. |
| [Tutorial para crear un componente de Windows Runtime de C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | En este tutorial se muestra cómo puedes usar .NET con Visual Basic o C# para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime, y cómo llamar al componente desde tu aplicación universal de Windows creada para Windows mediante JavaScript. |
| [Generación de eventos en componentes de Windows Runtime](raising-events-in-windows-runtime-components.md) | Si tu componente de Windows Runtime genera un evento de un tipo de delegado definido por el usuario en un subproceso en segundo plano (subproceso de trabajo) y deseas que JavaScript pueda recibir el evento, puedes implementarlo o generarlo mediante uno de estos métodos: | 
| [Componentes asincrónicos de Windows Runtime para aplicaciones para UWP de prueba](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | En este tema se describe la función orientada a empresas que cuentan con asistencia de Windows 10 Update y superior, que permite a las aplicaciones táctiles de .NET usar el código existente responsable de importantes operaciones fundamentales de la empresa. |