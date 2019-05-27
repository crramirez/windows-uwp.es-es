---
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: En esta sección se incluyen temas en los que se explica el marco XAML para aplicaciones de la Plataforma universal de Windows (UWP).
title: Plataforma XAML
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e22a10e74a834daf4d256313f0e353a6829911c0
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984240"
---
# <a name="xaml-platform"></a>Plataforma XAML


En esta sección se incluyen temas donde se explican los conceptos de programación que generalmente conciernen a cualquier aplicación que escribas si estás usando C#, Microsoft Visual Basic o extensiones de componentes de Visual C++ (C++/CX) como lenguaje de programación y XAML para la definición de la interfaz de usuario. Esto incluye conceptos de programación básica, como el uso de propiedades y eventos, y el modo en que estos se aplican a la programación de aplicaciones de la Plataforma universal de Windows (UWP). La Plataforma universal de Windows (UWP) amplía los conceptos de propiedades de C#, Visual Basic o C++/CX y sus valores agregando el sistema de propiedades de dependencias. Los temas de esta sección también documentan el lenguaje XAML tal y como lo usa UWP y abarcan escenarios básicos y temas avanzados sobre el uso de XAML para definir la interfaz de usuario de una aplicación para UWP.

| Tema | Descripción |
|-------|-------------|
| [Introducción a XAML](xaml-overview.md) | Presentamos el lenguaje XAML y los conceptos de XAML a los desarrolladores de aplicaciones de Windows Runtime y describimos las diversas formas de declarar objetos y establecer atributos en XAML que se usan para crear una aplicación de Windows Runtime. |
| [Información general sobre las propiedades de dependencia](dependency-properties-overview.md) | En este tema se explica el sistema de propiedades de dependencia de que dispones cuando escribes una aplicación de Windows Runtime con C++, C#, o Visual Basic junto con definiciones XAML para la interfaz de usuario. |
| [Propiedades de dependencia personalizadas](custom-dependency-properties.md) | Se explica cómo definir e implementar las propiedades de dependencia personalizadas para una aplicación de Windows Runtime con C++, C# o Visual Basic. |
| [Introducción a las propiedades adjuntas](attached-properties-overview.md) | Explica el concepto de propiedad adjunta en XAML y proporciona algunos ejemplos. |
| [Propiedades adjuntas personalizadas](custom-attached-properties.md) | Se explica cómo implementar una propiedad adjunta de XAML como una propiedad de dependencia y cómo definir la convención de descriptor de acceso necesaria para que la propiedad adjunta se pueda usar en XAML. |
| [Introducción a eventos y eventos enrutados](events-and-routed-events-overview.md) | Aquí se describe el concepto de programación de eventos en una aplicación de Windows Runtime cuando se usa C#, Visual Basic o C++/CX como lenguaje de programación y XAML para la definición de la interfaz de usuario. Puedes asignar controladores para eventos como parte de las declaraciones de los elementos de la interfaz de usuario en XAML, o puedes agregar los controladores en el código. Windows Runtime admite **eventos enrutados**, lo que implica que ciertos eventos de entrada y eventos de datos puedan ser controlados por otros objetos distintos del objeto que originó el evento. Los eventos enrutados son útiles cuando tienes que definir plantillas de control o usar contenedores de páginas o de diseño. |
|[Controles de UWP en aplicaciones de escritorio (islas XAML)](/windows/apps/desktop/modernize/xaml-islands)| Se explica cómo usar controles de XAML para UWP para mejorar la interfaz de usuario de una aplicación de escritorio de Windows Forms, WPF o Win32.|
