---
author: msatranjr
title: Componentes de Windows Runtime
description: Los componentes de Windows Runtime son objetos independientes que permiten crear una instancia y se pueden usar desde cualquier lenguaje, como C#, Visual Basic, JavaScript y C++.
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cc6d955ac77409f37b7701bef8c1fa4d93c97e3c
ms.sourcegitcommit: 54c2cd58fde08af889093a0c85e7297e33e6a0eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2018
ms.locfileid: "1664938"
---
# <a name="windows-runtime-components"></a>Componentes de Windows Runtime



Los componentes de Windows Runtime son objetos independientes que permiten crear una instancia y se pueden usar desde cualquier lenguaje, como C#, Visual Basic, JavaScript y C++.

Puedes usar Visual Studio y C#, Visual Basic o C++ para crear componentes de Windows Runtime que puedan usarse en aplicaciones de la Plataforma universal de Windows (UWP).

| Tema | Descripción |
|-------|-------------|
| [Crear componentes de Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) | En este artículo se muestra cómo usar C++ para crear un componente de Windows Runtime, es decir, un archivo DLL al que se puede llamar desde una aplicación universal de Windows compilada con JavaScript, o C#, Visual Basic o C++. |
| [Tutorial: crear un componente básico de Windows Runtime en C++ y llamarlo desde JavaScript o C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | En este tutorial se muestra cómo crear un archivo DLL básico del componente de Windows Runtime que se pueda llamar desde JavaScript, C# o Visual Basic. Antes de comenzar este tutorial, asegúrate de que conoces conceptos como la interfaz binaria abstracta (ABI), las clases de referencia y las extensiones del componente de Visual C++, que facilitan el trabajo con clases de referencia. Para obtener más información, consulta [Crear componentes de Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) y [Referencia del lenguaje de Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx). |
| [Crear componentes de Windows Runtime en C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | A partir de .NET Framework 4.5, puedes usar código administrado para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime. Puedes usar tu componente en aplicaciones de la Plataforma universal de Windows (UWP) con C++, JavaScript, Visual Basic o C#. En este artículo se describen las reglas de creación de un componente y se describen algunos aspectos de la compatibilidad de .NET Framework para Windows Runtime. En general, esa compatibilidad está diseñada para ser transparente para los programadores de .NET Framework. Sin embargo, cuando creas un componente para su uso con JavaScript o C++, debes tener en cuenta las diferencias en la forma en que esos lenguajes son compatibles con Windows Runtime. |
| [Tutorial: Creación de un componente simple de Windows Runtime y llamada al mismo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | En este tutorial se muestra cómo puedes usar .NET Framework con Visual Basic o C# para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime, y cómo llamar al componente desde tu aplicación universal de Windows creada para Windows mediante JavaScript. |
| [Generación de eventos en componentes de Windows Runtime](raising-events-in-windows-runtime-components.md) | Si tu componente de Windows Runtime genera un evento de un tipo de delegado definido por el usuario en un subproceso en segundo plano (subproceso de trabajo) y deseas que JavaScript pueda recibir el evento, puedes implementarlo o generarlo mediante uno de estos métodos: | 
| [Componentes negociados de Windows Runtime para aplicaciones de prueba para UWP](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | En este tema se describe la función orientada a empresas que cuentan con asistencia de Windows 10 Update y superior, que permite a las aplicaciones táctiles de .NET usar el código existente responsable de importantes operaciones fundamentales de la empresa. |
