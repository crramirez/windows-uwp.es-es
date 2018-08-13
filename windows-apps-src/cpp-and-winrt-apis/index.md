---
author: stevewhims
description: C++/WinRT es una proyección del lenguaje C++ 17 moderna y totalmente estándar para API de Windows Runtime (WinRT), implementada como biblioteca basada en archivos de encabezado.
title: C++/WinRT
ms.author: stwhi
ms.date: 05/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección
ms.localizationpriority: medium
ms.openlocfilehash: 515ac1f9a079e3791be8835f1a33c16198e27362
ms.sourcegitcommit: 929fa4b3273862dcdc76b083bf6c3b2c872dd590
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1935641"
---
# [<a name="cwinrt"></a>C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime, implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows. Con C++/WinRT, puedes crear y consumir API de Windows Runtime usando cualquier compilador de C ++17 compatible con estándares. Windows SDK incluye C++/WinRT. Se introdujo en la versión 10.0.17134.0 (Windows 10, versión 1803).

C++/WinRT está destinado a todo desarrollador interesado en la escritura de código atractivo y rápido para Windows. El motivo es el siguiente.

## <a name="the-case-for-cwinrt"></a>El caso para C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

El lenguaje de programación C++ se usa tanto en empresas *como* en segmentos de fabricantes de software independientes (ISV) para aplicaciones donde se evalúan altos niveles de corrección, calidad y rendimiento. Por ejemplo: sistemas de programación; sistemas insertados y móviles con recursos limitados; juegos y gráficos; controladores de dispositivos; y aplicaciones industriales, científicas y médicas, por nombrar algunas.

Desde un punto de vista de lenguaje, C++ siempre ha tratado la creación y el consumo de abstracciones de tipo enriquecido y ligero. Pero el idioma ha cambiado radicalmente desde los punteros sin procesar, bucles sin procesar y la meticulosa asignación y liberación de memoria de C++98. El lenguaje C++ moderno (desde C++11 en adelante) consiste en una expresión clara de ideas, simplicidad, legibilidad y una probabilidad mucho menor de introducir errores.

Para crear y consumir API de Windows Runtime usando C++, existe C++/WinRT. Este es el reemplazo recomendado de Microsoft para la [biblioteca de plantillas C++ de Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live) y [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live).

Usas tipos de datos de C++ estándar, algoritmos y palabras clave cuando usas C++/WinRT. La proyección tiene sus propios tipos de datos personalizados, pero en la mayoría de los casos no es necesario obtener información sobre ellos ya que ofrecen adecuadas conversiones hacia y desde tipos estándares. De este modo, puedes seguir usando las características del lenguaje C++ estándar al que estás acostumbrado a usar y el código fuente que ya tienes. C++/WinRT hace que sea sumamente fácil llamar a las API de Windows Runtime en cualquier aplicación de C++, desde Win32 hasta UWP.

C++/WinRT funciona mejor y produce archivos binarios más pequeños que cualquier otra opción de lenguaje de Windows Runtime. Incluso supera al código manuscrito usando las interfaces ABI directamente. Esto se debe a que las abstracciones usan modernos giros C++ para los que se ha diseñado el compilador Visual C++ para optimizarlos. Esto incluye estática mágica, clases base vacías, elisión **strlen**, así como muchas optimizaciones más recientes en la última versión de Visual C++ destinada específicamente a mejorar el rendimiento de C++/WinRT.

| Tema | Descripción |
| - | - |
| [Introducción a C++/WinRT](intro-to-using-cpp-with-winrt.md) | Una introducción a C++/WinRT&mdash;una proyección de lenguaje C++ estándar para las API de Windows Runtime. |
| [Introducción a C++/WinRT](get-started.md) | Para ponerte al día con el uso de C++/WinRT, este artículo te guía a través de un ejemplo de código sencillo. |
| [Preguntas más frecuentes](faq.md) | Respuestas a preguntas que probablemente tengas acerca de la creación y consumo de las API de Windows Runtime con C++/WinRT. |
| [Solución de problemas](troubleshooting.md) | La tabla de solución de problemas de síntomas y soluciones de este tema puede resultarte útil si vas a cortar nuevo código o portar una aplicación existente. |
| [Aplicación de ejemplo de C++/WinRT de Photo Editor](photo-editor-sample.md) | Photo Editor es una aplicación de ejemplo para UWP que muestra el desarrollo con proyección de lenguaje de C++/WinRT. La aplicación de ejemplo permite recuperar fotos desde la biblioteca **Imágenes** y, a continuación, modificar la imagen seleccionada con distintos efectos fotográficos. | 
| [Control de cadenas](strings.md) | Con C++/WinRT, puedes llamar a las API de Windows Runtime usando tipos de cadena de caracteres anchos de C++ estándar, o puedes usar el tipo [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). |
| [Tipos de datos C++ estándar y C++/WinRT](std-cpp-data-types.md) | Con C++ / WinRT, puedes llamar a las API de Windows Runtime con tipos de datos C++ estándar. |
| [Conversión boxing y unboxing de valores escalar a IInspectable](boxing.md) | Un valor escalar debe estar encapsulado dentro de un objeto de clase de referencia antes de pasarlo a una función que espera **IInspectable**. Dicho proceso de encapsulación se conoce como la *conversión boxing* del valor. |
| [Consumir API con C++/WinRT](consume-apis.md) | Este tema muestra cómo usar API C++/WinRT, ya se hayan implementado por Windows, por un proveedor de componentes de terceros o por ti mismo. |
| [Crear API con C++/WinRT](author-apis.md) | Este tema muestra cómo crear API C++/WinRT mediante el uso de la estructura base **winrt::implements**, directa o indirectamente. |
| [Gestión de errores con C++/WinRT](error-handling.md) | Este tema describe estrategias para controlar los errores de programación con C++/WinRT. |
| [Controlar eventos usando delegados](handle-events.md) | Este tema muestra cómo registrar y revocar delegados de control de eventos con C++/WinRT. |
| [Crear eventos](author-events.md) | Este tema muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que genera eventos. También muestra una aplicación que consume el componente y controla los eventos. |
| [Operaciones simultáneas y asincrónicas](concurrency.md) | Este tema muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT. |
| [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md) | Una propiedad que se puede enlazar de forma eficaz a un control de elementos XAML se conoce como una propiedad *observable*. Este tema muestra cómo implementar y consumir una propiedad observable y cómo enlazar un control XAML a dicha propiedad. |
| [Controles de elementos XAML; enlazar a una colección C++/WinRT](binding-collection.md) | Una colección que se puede enlazar de forma eficaz a un control de elementos XAML se conoce como una colección *observable*. Este tema muestra cómo implementar y consumir una colección observable y cómo enlazar un control de elementos XAML a dicha colección. |
| [Interoperabilidad entre C++/WinRT y C++/CX](interop-winrt-cx.md) | Este tema muestra dos funciones auxiliares que pueden usarse para convertir entre objetos [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) y C++/WinRT. |
| [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md) | En este tema se muestra cómo migrar código C++/CX a su equivalente en C++/WinRT. |
| [Interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md) | Este tema muestra cómo convertir entre la interfaz binaria de aplicaciones (ABI) y objetos C++/WinRT. |
| [Mover a C++/WinRT desde WRL](move-to-winrt-from-wrl.md) | En este tema se muestra cómo migrar código de la [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) a su equivalente en C++/WinRT. |
| [Referencias débiles](weak-references.md) | El soporte de referencia débil C++/WinRT se basa en el sistema de pago pay-to-play, esto es, no te cuesta nada a menos que se consulte tu objeto para [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609). |
| [Objetos ágiles](agile-objects.md) | Un objeto ágil es aquel al que se puede acceder desde cualquier subproceso. Tus tipos C++/WinRT son ágiles de manera predeterminada, pero puedes optar por rechazarlos. |

## <a name="important-apis"></a>API importantes
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Artículos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)