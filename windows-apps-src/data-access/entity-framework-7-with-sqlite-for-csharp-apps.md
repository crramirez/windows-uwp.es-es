---
author: mcleblanc
ms.assetid: BC7E8130-A28A-443C-8D7E-353E7DA33AE3
description: "Entity Framework (EF) es un asignador relacional de objetos que te permite trabajar con datos relacionales mediante objetos específicos del dominio."
title: Entity Framework 7 con SQLite para aplicaciones de C#
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, SQLite, C#, EF, entity framework
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2ab2a12f6c2bc2f0f8853b404afaf13bf80635b7
ms.lasthandoff: 02/07/2017

---

# <a name="entity-framework-core-1-with-sqlite-for-c-apps"></a>Entity Framework Core 1 con SQLite para aplicaciones de C#

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Entity Framework (EF) es un asignador relacional de objetos que te permite trabajar con datos relacionales mediante objetos específicos del dominio. En este artículo se explica cómo usar Entity Framework Core 1 con una base de datos SQLite en una aplicación universal de Windows.

Entity Framework Core 1, inicialmente concebido para desarrolladores de .NET, puede utilizarse con SQLite en la plataforma universal de Windows (UWP) para almacenar y manipular datos relacionales mediante objetos específicos del dominio. Puedes migrar el código de EF desde una aplicación .NET hasta una aplicación para UWP y esperar que funcione si realizas los cambios adecuados en la cadena de conexión.

Actualmente, EF solo admite SQLite en la UWP. En la página [Getting Started on Universal Windows Platform](http://go.microsoft.com/fwlink/p/?LinkId=735013) encontrarás un tutorial detallado sobre la instalación de Entity Framework Core 1 y la creación de modelos. Incluye los temas siguientes:

-   Requisitos previos.
-   Creación de un proyecto nuevo.
-   Instalación de Entity Framework.
-   Creación de un modelo.
-   Creación de una base de datos.
-   Cómo utilizar el modelo.

