---
author: normesta
description: Esta guía te ayuda a crear interfaces de usuario de UWP basadas en Fluent directamente en tus aplicaciones de WPF y Windows Forms
title: Hospedar los controles de UWP en aplicaciones de WPF y Windows Forms
ms.author: normesta
ms.date: 05/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 4823654bce3373ace5b04ced8ec14c4b6c1b6f1d
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2018
ms.locfileid: "1862074"
---
# <a name="host-uwp-controls-in-wpf-and-windows-forms-applications"></a>Hospedar los controles de UWP en aplicaciones de WPF y Windows Forms

> [!NOTE]
> Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.

Estamos incorporando controles de UWP en el escritorio para que puedas mejorar el aspecto, la sensación y la funcionalidad de las aplicaciones de Windows o WPF existentes con características de Fluent Design. Hay dos maneras de hacerlo.

En primer lugar, puedes agregar controles en la superficie de diseño de tu proyecto de WPF o Windows Forms y, a continuación, usarlos como cualquier otro control en el diseñador.  Prueba esto hoy con el nuevo control **WebView**. Este control usa el motor de representación de Microsoft Edge y hasta ahora este control solo estaba disponible para aplicaciones para UWP. Encontrarás la **WebView** en la versión más reciente del [Kit de herramientas de Comunidad Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).

Pronto, tendrás acceso a incluso más características de Fluent Design: te proporcionaremos un control que te permite hospedar una variedad de controles de UWP. Busca este control y muchos otros controles en futuras versiones del Kit de herramientas de Comunidad Windows.

Echa un vistazo rápido a la manera en que estos controles se organizan desde el punto de vista de la arquitectura. Los nombres usados en este diagrama están sujetos a cambios.  

![Arquitectura de control de host](images/host-controls.png)

Las API que aparecen en la parte inferior de este diagrama se incluye con el Windows SDK.  Los controles que agregarás en el diseñador se incluyen como paquetes de Nuget en el Kit de herramientas de Comunidad Windows.

Estos nuevos controles tienen limitaciones por lo que, antes de usarlos, dedica un momento a revisar qué no se admite aún y qué funciona solo con soluciones alternativas.

### <a name="whats-supported"></a>Lo que es compatible

En la mayor parte, todo se admite a menos que se mencione de manera explícita en la lista siguiente.

### <a name="whats-supported-only-with-workarounds"></a>Qué se admite solo con soluciones alternativas

:heavy_check_mark: el hospedaje de varios controles de bandeja de entrada dentro de varias ventanas. Tendrás que colocar cada ventana en su propio subproceso.

:heavy_check_mark: uso de ``x:Bind``con controles hospedados. Tendrás que declarar el modelo de datos en una biblioteca .NET Standard.

:heavy_check_mark: controles de terceros basados en C#. Si tienes el código fuente para un control de terceros, puedes compilar con él.

### <a name="whats-not-yet-supported"></a>Lo que no es compatible todavía

:no_entry_sign: herramientas de accesibilidad que funcionan perfectamente en toda la aplicación y los controles hospedados.

:no_entry_sign: contenido localizado en controles que agregas a las aplicaciones que no contienen un paquete de la aplicación de Windows.

:no_entry_sign: referencias a activos realizadas en XAML dentro de las aplicaciones que no contienen un paquete de la aplicación de Windows.

:no_entry_sign: controles que responden correctamente a cambios en PPP y escala.

:no_entry_sign: adición de un control **WebView** a un control de usuario personalizado, (en subproceso, fuera de subproceso o fuera de proceso).

:no_entry_sign: el efecto de Fluent [Mostrar resaltado](https://docs.microsoft.com/windows/uwp/design/style/reveal).

:no_entry_sign: entrada manuscrita en línea, @Places y @People para controles de entrada.

:no_entry_sign: asignación de teclas de aceleración.

:no_entry_sign: controles de terceros basados en C++.

:no_entry_sign: hospedaje de controles de usuario personalizados.

Los elementos de esta lista cambiarán probablemente a medida que seguimos mejorando la experiencia de llevar Fluent al escritorio.  
