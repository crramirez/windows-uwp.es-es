---
title: Componentes de Windows Runtime
description: Los componentes de Windows Runtime son objetos independientes que permiten crear una instancia y se pueden usar desde cualquier lenguaje, como C#, Visual Basic, JavaScript y C++.
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
---

# Componentes de Windows Runtime


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Los componentes de Windows Runtime son objetos independientes que permiten crear una instancia y se pueden usar desde cualquier lenguaje, como C#, Visual Basic, JavaScript y C++.

Puedes usar Visual Studio y C#, Visual Basic o C++ para crear componentes de Windows Runtime que puedan usarse en aplicaciones de la Plataforma universal de Windows (UWP).

| Tema | Descripción |
|-------|-------------|
| [Crear componentes de Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) | En este artículo se muestra cómo usar C++ para crear un componente de Windows Runtime, es decir, un archivo DLL al que se puede llamar desde una aplicación universal de Windows compilada con JavaScript, o C#, Visual Basic o C++. |
| [Tutorial: Crear en C++ un componente básico de Windows Runtime y llamarlo desde JavaScript o C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | En este tutorial se muestra cómo crear un archivo DLL básico del componente de Windows Runtime que se pueda llamar desde JavaScript, C# o Visual Basic. Antes de comenzar este tutorial, asegúrate de que conoces conceptos como la interfaz binaria abstracta (ABI), las clases de referencia y las extensiones del componente de Visual C++, que facilitan el trabajo con clases de referencia. Para obtener más información, consulta [Crear componentes de Windows Runtime en C++](creating-windows-runtime-components-in-cpp.md) y [Referencia del lenguaje de Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx). |
| [Crear componentes de Windows Runtime en C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | A partir de .NET Framework 4.5, puedes usar código administrado para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime. Puedes usar tu componente en aplicaciones de la Plataforma universal de Windows (UWP) con C++, JavaScript, Visual Basic o C#. En este artículo se describen las reglas de creación de un componente y se describen algunos aspectos de la compatibilidad de .NET Framework para Windows Runtime. En general, esa compatibilidad está diseñada para ser transparente para los programadores de .NET Framework. Sin embargo, cuando creas un componente para su uso con JavaScript o C++, debes tener en cuenta las diferencias en la forma en que esos lenguajes son compatibles con Windows Runtime. |
| [Tutorial: Creación de un componente simple de Windows Runtime y llamada al mismo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | En este tutorial se muestra cómo puedes usar .NET Framework con Visual Basic o C# para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime, y cómo llamar al componente de tu aplicación universal de Windows creada para Windows usando JavaScript. |
| [Generación de eventos en componentes de Windows Runtime](raising-events-in-windows-runtime-components.md) | Si tu componente de Windows Runtime genera un evento de un tipo de delegado definido por el usuario en un subproceso en segundo plano (subproceso de trabajo) y quieres que JavaScript pueda recibir el evento, puedes implementarlo o generarlo mediante uno de estos métodos: |
 

 

 





<!--HONumber=Mar16_HO1-->


