---
author: jwmsft
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: "En esta sección se incluyen temas donde se explican los conceptos de programación que generalmente se aplican a cualquier aplicación que escribas."
title: Plataforma XAML
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 3a5a638a2b8caf63cc3353d7bd6a43068ec61119
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xaml-platform"></a>Plataforma XAML

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En esta sección se incluyen temas donde se explican los conceptos de programación que generalmente conciernen a cualquier aplicación que escribas si estás usando C#, MicrosoftVisual Basic o extensiones de componentes de VisualC++ (C++/CX) como lenguaje de programación y XAML para la definición de la interfaz de usuario. Esto incluye conceptos de programación básica, como el uso de propiedades y eventos, y el modo en que estos se aplican a la programación de aplicaciones de la Plataforma universal de Windows (UWP). La Plataforma universal de Windows (UWP) amplía los conceptos de propiedades de C#, Visual Basic o C++/CX y sus valores agregando el sistema de propiedades de dependencias. Los temas de esta sección también documentan el lenguaje XAML tal y como lo usa UWP y abarcan escenarios básicos y temas avanzados sobre el uso de XAML para definir la interfaz de usuario de una aplicación para UWP.
 
| Tema | Descripción |
|-------|-------------|
| [Introducción a XAML](xaml-overview.md) | Se presenta el lenguaje XAML y los conceptos de XAML a la audiencia de desarrolladores de aplicaciones de Windows Runtime y se describen las diversas formas de declarar objetos y establecer atributos en XAML que se usan para crear una aplicación de Windows Runtime. |
| [Introducción a las propiedades de dependencia](dependency-properties-overview.md) | En este tema se explica el sistema de propiedades de dependencia de que dispones cuando escribes una aplicación de Windows Runtime con C++, C# o Visual Basic junto con definiciones XAML para la interfaz de usuario. |
| [Propiedades de dependencia personalizadas](custom-dependency-properties.md) | Se explica cómo definir e implementar las propiedades de dependencia personalizadas para una aplicación de Windows Runtime con C++, C# o Visual Basic. |
| [Introducción a las propiedades adjuntas](attached-properties-overview.md) | Se explica el concepto de propiedad adjunta en XAML y se ofrecen algunos ejemplos. |
| [Propiedades adjuntas personalizadas](custom-attached-properties.md) | Se explica cómo implementar una propiedad adjunta de XAML como una propiedad de dependencia y cómo definir la convención de descriptor de acceso necesaria para que la propiedad adjunta se pueda usar en XAML. |
| [Introducción a eventos y eventos enrutados](events-and-routed-events-overview.md) | Aquí se describe el concepto de programación de eventos en una aplicación de Windows Runtime cuando se usa C#, VisualBasic o C++/CX como lenguaje de programación y XAML para la definición de la interfaz de usuario. Puedes asignar controladores para eventos como parte de las declaraciones de los elementos de la interfaz de usuario en XAML, o puedes agregar los controladores en el código. Windows Runtime admite **eventos enrutados**, lo que implica que ciertos eventos de entrada y eventos de datos puedan ser controlados por otros objetos distintos del objeto que originó el evento. Los eventos enrutados son útiles cuando tienes que definir plantillas de control o usar contenedores de páginas o de diseño. |

 
