---
Description: Presenta los conceptos de accesibilidad relacionados con las aplicaciones de Windows.
ms.assetid: C89D79C2-B830-493D-B020-F3FF8EB5FFDD
title: Accesibilidad
label: Accessibility
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67f98db338d201dd4ef80c7b5f265aba3f6fbfd4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216378"
---
# <a name="accessibility"></a>Accesibilidad  

La accesibilidad es acerca de la creación de experiencias que hacen que la aplicación Windows sea utilizable por personas que usan tecnología en una amplia gama de entornos y se aproximan a la interfaz de usuario con una variedad de necesidades y experiencias. En algunos casos, los requisitos de accesibilidad están impuestos por ley. Sin embargo, resulta buena idea abordar los problemas de accesibilidad independientemente de los requisitos legales, de modo que tus aplicaciones lleguen al mayor público posible.

> También hay una declaración de Microsoft Store con respecto a la accesibilidad de la aplicación.

| Artículo | Descripción |
|---------|-------------|
| [Información general sobre accesibilidad](accessibility-overview.md) | Este artículo es una introducción a los conceptos y tecnologías relacionados con escenarios de accesibilidad para aplicaciones de Windows. |
| [Diseño de software inclusivo](designing-inclusive-software.md) | Obtenga información sobre cómo desarrollar un diseño inclusivo con aplicaciones de Windows para Windows 10.  Diseña y crea software inclusivo teniendo en cuenta la accesibilidad. |
| [Desarrollo de aplicaciones inclusivas de Windows](developing-inclusive-windows-apps.md) | Este artículo es una guía para el desarrollo de aplicaciones de Windows accesibles. |
| [Pruebas de accesibilidad](accessibility-testing.md) | Procedimientos de prueba que deben seguirse para asegurarse de que la aplicación de Windows está accesible. |
| [Accesibilidad en Store](accessibility-in-the-store.md) | Describe los requisitos para declarar la aplicación de Windows como accesible en el Microsoft Store. |
| [Lista de comprobación de accesibilidad](accessibility-checklist.md) | Proporciona una lista de comprobación para ayudarle a asegurarse de que la aplicación de Windows esté accesible. |
| [Exponer información básica de accesibilidad](basic-accessibility-information.md) | La información de accesibilidad básica se suele clasificar en nombre, rol y valor. En este tema se describe el código para ayudar a que tu aplicación exponga la información básica requerida por las tecnologías de asistencia. |
| [Accesibilidad de teclado](keyboard-accessibility.md) | Si tu aplicación no proporciona un buen acceso de teclado, los usuarios invidentes o con problemas de motricidad pueden llegar a tener dificultades para usar tu aplicación o, probablemente, no puedan usarla. |
| [Botones del sistema de hardware y lectores de pantalla](system-button-narration.md) | Los lectores de pantalla, como [narrador](https://support.microsoft.com/en-us/help/22798/windows-10-complete-guide-to-narrator), deben ser capaces de reconocer y controlar los eventos de los botones del sistema de hardware y comunicar su estado a los usuarios. En algunos casos, es posible que el lector de pantalla tenga que controlar los eventos de botón de forma exclusiva y no permitir que se propaguen a otros controladores. |
| [Puntos de referencia y encabezados](landmarks-and-headings.md) | Los puntos de referencia y los encabezados definen secciones de una interfaz de usuario que ayudan a que la navegación sea eficaz para los usuarios de tecnología de asistencia, como son los lectores de pantalla. |
| [Temas de contraste alto](high-contrast-themes.md) | Describe los pasos necesarios para asegurarse de que se puede usar la aplicación de Windows cuando un tema de contraste alto está activo. |
| [Requisitos de texto accesible](accessible-text-requirements.md) | En este tema se describen los procedimientos recomendados sobre accesibilidad de texto en una aplicación mediante la configuración de los colores de texto y fondo, de forma que cumplan con la relación de contraste necesaria. En este tema también se describen los roles de automatización de la interfaz de usuario de Microsoft que pueden tener los elementos de texto en una aplicación de Windows y los procedimientos recomendados para el texto de los gráficos. |
| [Procedimientos de accesibilidad que deben evitarse](practices-to-avoid.md) | Enumera las prácticas que se deben evitar si desea crear una aplicación de Windows accesible. |
| [Automatización del mismo nivel personalizada](custom-automation-peers.md) | Describe el concepto de automatización del mismo nivel para la Automatización de la interfaz de usuario y cómo puedes proporcionar compatibilidad de automatización para tu propia clase de interfaz de usuario personalizada. |
| [Interfaces y patrones de control](control-patterns-and-interfaces.md) | Enumera los patrones de control de Automatización de la interfaz de usuario de Microsoft, las clases que los clientes usan para acceder a ellos y las interfaces que los proveedores usan para implementarlos. |

## <a name="related-topics"></a>Temas relacionados  
* [**Windows. UI. Xaml. Automation**](/uwp/api/Windows.UI.Xaml.Automation) 
* [Introducción a narrador](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
