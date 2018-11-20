---
author: jwmsft
ms.assetid: b632a6cc-3503-4ab8-bfd1-dde731bd89ab
description: Esta sección incluye temas en los que se explican el marco XAML para aplicaciones de la Plataforma universal de Windows (UWP).
title: Plataforma XAML
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fe9bad5afabca3ef0b9c782446e581aea61fe4dc
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7289675"
---
# <a name="xaml-platform"></a>Plataforma XAML


Esta sección incluyen temas donde se explican los conceptos de programación que generalmente conciernen a cualquier aplicación que escribas si estás usando las extensiones de componentes de C#, Microsoft Visual Basic o VisualC ++ (C++ / CX) como lenguaje de programación y XAML para la interfaz de usuario definición. Esto incluye conceptos de programación básica, como el uso de propiedades y eventos, y el modo en que estos se aplican a la programación de aplicaciones de la Plataforma universal de Windows (UWP). La Plataforma universal de Windows (UWP) amplía los conceptos de propiedades de C#, Visual Basic o C++/CX y sus valores agregando el sistema de propiedades de dependencias. Los temas de esta sección también documentan el lenguaje XAML tal y como lo usa UWP y abarcan escenarios básicos y temas avanzados sobre el uso de XAML para definir la interfaz de usuario de una aplicación para UWP.

| Tema | Descripción |
|-------|-------------|
| [Introducción a XAML](xaml-overview.md) | Presentamos el lenguaje XAML y los conceptos de XAML a los desarrolladores de aplicaciones de Windows Runtime y describimos las diversas formas de declarar objetos y establecer atributos en XAML que se usan para crear una aplicación de Windows Runtime. |
| [Introducción a las propiedades de dependencia](dependency-properties-overview.md) | En este tema se explica el sistema de propiedades de dependencia de que dispones cuando escribes una aplicación de Windows Runtime con C++, C#, o Visual Basic junto con definiciones XAML para la interfaz de usuario. |
| [Propiedades de dependencia personalizadas](custom-dependency-properties.md) | Se explica cómo definir e implementar las propiedades de dependencia personalizadas para una aplicación de Windows Runtime con C++, C# o Visual Basic. |
| [Introducción a las propiedades adjuntas](attached-properties-overview.md) | Se explica el concepto de propiedad adjunta en XAML y se ofrecen algunos ejemplos. |
| [Propiedades adjuntas personalizadas](custom-attached-properties.md) | Se explica cómo implementar una propiedad adjunta de XAML como una propiedad de dependencia y cómo definir la convención de descriptor de acceso necesaria para que la propiedad adjunta se pueda usar en XAML. |
| [Introducción a eventos y eventos enrutados](events-and-routed-events-overview.md) | Aquí se describe el concepto de programación de eventos en una aplicación de Windows Runtime cuando se usa C#, VisualBasic o C++/CX como lenguaje de programación y XAML para la definición de la interfaz de usuario. Puedes asignar controladores para eventos como parte de las declaraciones de los elementos de la interfaz de usuario en XAML, o puedes agregar los controladores en el código. Windows Runtime admite **eventos enrutados**, lo que implica que ciertos eventos de entrada y eventos de datos puedan ser controlados por otros objetos distintos del objeto que originó el evento. Los eventos enrutados son útiles cuando tienes que definir plantillas de control o usar contenedores de páginas o de diseño. |
|[Hospedar los controles de UWP en aplicaciones de WPF y Windows Forms](xaml-host-controls.md)| Explica cómo usar controles XAML de UWP para mejorar la interfaz de usuario de una aplicación de escritorio de WPF o Windows Forms.|
