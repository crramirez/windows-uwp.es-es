---
description: C++/WinRT es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), que se implementa como una biblioteca basada en archivos de encabezado.
title: C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection
ms.localizationpriority: medium
ms.openlocfilehash: b3ad3a204b2eb4b288e3d3c313a0832bea124de0
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219898"
---
# <a name="cwinrt"></a>C++/WinRT

[C++/WinRT](./intro-to-using-cpp-with-winrt.md) es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para darte acceso de primera clase a la API moderna de Windows. Con C++/WinRT, puedes crear y consumir API de Windows Runtime mediante cualquier compilador de C++17 compatible con estándares. Windows SDK incluye C++/WinRT; se introdujo en la versión 10.0.17134.0 (Windows 10, versión 1803).

C++/WinRT está destinado a todo desarrollador interesado en la escritura de código atractivo y rápido para Windows. Y esto es así por las siguientes razones.

## <a name="the-case-for-cwinrt"></a>Caso de C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

El lenguaje de programación C++ se usa tanto en el segmento *empresarial* como en el de los fabricantes de software independientes (ISV) para aplicaciones donde se valoran altos niveles de corrección, calidad y rendimiento. Por ejemplo, programación de sistemas; sistemas móviles e incrustados con restricción de recursos; juegos y gráficos; controladores de dispositivo; y aplicaciones industriales, científicas y médicas, por nombrar algunas.

Desde el punto de vista del lenguaje, C++ siempre ha tenido por objeto la creación y el consumo de abstracciones ligeras y con numerosos tipos. Pero el lenguaje ha cambiado radicalmente desde los punteros sin procesar, los bucles sin procesar y la meticulosa asignación y liberación de memoria de C++98. El lenguaje C++ moderno (desde C++11 en adelante) se caracteriza por una clara expresión de ideas, simplicidad, legibilidad y una probabilidad mucho menor de introducir errores.

Para crear y consumir API de Windows Runtime mediante C++, existe C++/WinRT. Esta es la sustitución recomendada de Microsoft para la proyección de lenguaje [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) y la [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live).

Cuando uses C++/WinRT, utilizarás tipos de datos, algoritmos y palabras clave estándar de C++. La proyección tiene sus propios tipos de datos personalizados, pero en la mayoría de los casos no es necesario conocerlos ya que proporcionan conversiones adecuadas hacia y desde los tipos estándar. De este modo, puedes seguir usando las características estándar del lenguaje C++ al que estás acostumbrado y el código fuente que ya tienes. C++/WinRT hace que sea sumamente fácil llamar a las API de Windows Runtime en cualquier aplicación de C++, desde Win32 hasta UWP.

C++/WinRT funciona mejor y produce archivos binarios más pequeños que cualquier otra opción de lenguaje de Windows Runtime. Incluso supera al código manuscrito mediante el uso de las interfaces ABI directamente. Esto se debe a que las abstracciones usan modernos giros de C++ que el compilador de Visual C++ está diseñado para optimizar. Por ejemplo, estática mágica, clases base vacías, elisión **strlen**, así como muchas optimizaciones más recientes de la última versión de Visual C++ destinada específicamente a mejorar el rendimiento de C++/WinRT.

Hay maneras de introducir C++/WinRT gradualmente en los proyectos. Puede usar [componentes de Windows Runtime](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) o puede interoperar con C++/CX. Para obtener más información, consulte [Interoperabilidad entre C++/WinRT y C++/CX](./interop-winrt-cx.md).

Para obtener información sobre cómo migrar a C++/WinRT, consulte estos recursos.

- [Migrar a C++/WinRT desde C++/CX](./move-to-winrt-from-cx.md)
- [Migrar a C++/WinRT desde WRL](./move-to-winrt-from-wrl.md)
- [Migrar a C++/WinRT desde C#](./move-to-winrt-from-csharp.md)

Consulte también [¿Dónde puedo encontrar aplicaciones de ejemplo de C++/WinRT?](./faq.md#where-can-i-find-cwinrt-sample-apps).

### <a name="topics-about-cwinrt"></a>Temas sobre C++/WinRT

| Tema | Descripción |
| - | - |
| [Introducción a C++/WinRT](./intro-to-using-cpp-with-winrt.md) | Una introducción a C++/WinRT, una proyección de lenguaje C++ estándar para las API de Windows Runtime. |
| [Introducción a C++/WinRT](./get-started.md) | Para ponerte al día con el uso de C++/WinRT, este tema te guía a través de un ejemplo de código sencillo. |
| [Novedades de C++/WinRT](./news.md) | Noticias y cambios en C++/WinRT. |
| [Preguntas frecuentes](./faq.md) | Respuestas a preguntas que probablemente tengas acerca de la creación y el consumo de las API de Windows Runtime con C++/WinRT. |
| [Solución de problemas](./troubleshooting.md) | La tabla de síntomas y remedios de solución de problemas de este tema puede resultarte útil si vas a cortar nuevo código o a migrar una aplicación existente. |
| [Aplicación de ejemplo de C++/WinRT de Photo Editor](./photo-editor-sample.md) | Photo Editor es una aplicación de ejemplo para UWP que muestra el desarrollo con la proyección de lenguaje de C++/WinRT. La aplicación de ejemplo permite recuperar fotos de la biblioteca **Imágenes** y, a continuación, modificar la imagen seleccionada con distintos efectos fotográficos. | 
| [Control de cadenas](./strings.md) | Con C++/WinRT, puedes llamar a las API de Windows Runtime mediante tipos de cadena anchos de C++ estándar, o puedes usar el tipo [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). |
| [Tipos de datos de C++ estándar y C++/WinRT](./std-cpp-data-types.md) | Con C++/WinRT, puedes llamar a las API de Windows Runtime mediante tipos de datos de C++ estándar. |
| [Conversión boxing y unboxing de valores escalares a IInspectable](./boxing.md) | Un valor escalar debe estar encapsulado dentro de un objeto de clase de referencia antes de pasarlo a una función que espera **IInspectable**. Dicho proceso de encapsulación se conoce como la *conversión boxing* del valor. |
| [Consumir API con C++/WinRT](./consume-apis.md) | En este tema se muestra cómo usar las API de C++/WinRT, tanto si se han implementado mediante Windows, un proveedor de componentes de terceros o por tu cuenta. |
| [Crear API con C++/WinRT](./author-apis.md) | En este tema se muestra cómo crear las API de C++/WinRT mediante la estructura base **winrt::implements**, directa o indirectamente. |
| [Control de errores con C++/WinRT](./error-handling.md) | En este tema se describen estrategias para controlar los errores de programación con C++/WinRT. |
| [Controlar eventos usando delegados](./handle-events.md) | En este tema se muestra cómo registrar y revocar delegados de control de eventos con C++/WinRT. |
| [Crear eventos](./author-events.md) | En este tema se muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que genera eventos. También se muestra una aplicación que consume el componente y controla los eventos. |
| [Colecciones con C++/WinRT](./collections.md) | C++/WinRT proporciona funciones y clases base que te ahorrarán mucho tiempo y esfuerzo cuando quieras implementar o pasar colecciones. |
| [Operaciones simultáneas y asincrónicas](./concurrency.md) | En este tema se muestra cómo puedes crear y consumir objetos asincrónicos de Windows Runtime con C++/WinRT. |
| [Simultaneidad y asincronía más avanzados](./concurrency-2.md) | Escenarios más avanzados con simultaneidad y asincronía en C++/WinRT. |
| [Controles de XAML; enlazar a una propiedad de C++/WinRT](./binding-property.md) | Una propiedad que se puede enlazar de forma eficaz a un control de XAML se conoce como una propiedad *observable*. En este tema se muestra cómo implementar y consumir una propiedad observable y cómo enlazar un control de XAML a dicha propiedad. |
| [Controles de elementos de XAML; enlazar a una colección C++/WinRT](./binding-collection.md) | Una colección que se puede enlazar de forma eficaz a un control de elementos de XAML se conoce como una colección *observable*. En este tema se muestra cómo implementar y consumir una colección observable y cómo enlazar un control de elementos de XAML a dicha colección. |
| [Controles (con plantilla) personalizados de XAML con C++/WinRT](./xaml-cust-ctrl.md) | Este tema te guía por los pasos de creación de un control personalizado sencillo con C++/WinRT. Puedes usar esta información para crear tus propios controles de interfaz de usuario repletos de características y personalizables. |
| [Pasar parámetros a los límites de la ABI](./pass-parms-to-abi.md) | C++/WinRT simplifica el paso de parámetros a los límites de la ABI ya que proporciona conversiones automáticas para casos habituales. |
| [Consumir componentes COM con C++/WinRT](./consume-com.md) | En este tema se usa un ejemplo de código completo de Direct2D para mostrar cómo utilizar C++/WinRT para consumir clases e interfaces COM. |
| [Crear componentes COM con C++/WinRT](./author-coclasses.md) | C++/WinRT puede ayudarte a crear componentes COM clásicos, igual que te ayuda a crear clases de Windows Runtime. |
| [Migrar a C++/WinRT desde C++/CX](./move-to-winrt-from-cx.md) | En este tema se describen los detalles técnicos implicados en la portabilidad del código fuente de un proyecto de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a su equivalente de [C++/WinRT](./intro-to-using-cpp-with-winrt.md). |
| [Interoperabilidad entre C++/WinRT y C++/CX](./interop-winrt-cx.md) | En este tema se muestran dos funciones auxiliares que pueden usarse para realizar la conversión entre objetos de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) y [C++/WinRT](./intro-to-using-cpp-with-winrt.md). |
| [Asincronía e interoperabilidad entre C++/WinRT y C++/CX](./interop-winrt-cx-async.md) | Este es un tema avanzado relacionado con la migración gradual de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a [C++/WinRT](./intro-to-using-cpp-with-winrt.md). Muestra cómo las tareas y las corrutinas de la Biblioteca de patrones paralelos(PPL) pueden existir en paralelo en el mismo proyecto. |
| [Migrar a C++/WinRT desde WRL](./move-to-winrt-from-wrl.md) | En este tema se muestra cómo portar código de la [Biblioteca de plantillas C++ de Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) a su equivalente en [C++/WinRT](./intro-to-using-cpp-with-winrt.md). |
| [Portabilidad del ejemplo del Portapapeles a C++/WinRT desde C#: un caso práctico](./clipboard-to-winrt-from-csharp.md) | En este tema se presenta un caso práctico de portabilidad de las [aplicaciones para la Plataforma universal de Windows (UWP) de ejemplo](https://github.com/microsoft/Windows-universal-samples) de [C#](/visualstudio/get-started/csharp) a [C++/WinRT](./intro-to-using-cpp-with-winrt.md). Para practicar y ganar experiencias de portabilidad, puedes seguir el tutorial y portar el ejemplo por tu cuenta a medida que avanzas. |
| [Migrar a C++/WinRT desde C#](./move-to-winrt-from-csharp.md) | En este tema se catalogan de forma exhaustiva los detalles técnicos implicados en la migración del código fuente de un proyecto de [C#](/visualstudio/get-started/csharp) a su equivalente de [C++/WinRT](./intro-to-using-cpp-with-winrt.md). |
| [Interoperabilidad entre C++/WinRT y la ABI](./interop-winrt-abi.md) | En este tema se muestra cómo realizar la conversión entre la interfaz binaria de aplicaciones (ABI) y objetos de C++/WinRT. |
| [Referencias fuertes y débiles de C++/WinRT](./weak-references.md) | Windows Runtime es un sistema con recuento de referencias y en este tipo de sistemas es importante conocer el significado de referencias fuertes y débiles y la diferencia entre ellas. |
| [Objetos ágiles](./agile-objects.md) | Un objeto ágil es aquel al que se puede acceder desde cualquier subproceso. Los tipos de C++/WinRT son ágiles de manera predeterminada, pero puedes excluirlos. |
| [Diagnóstico de asignaciones directas](./diag-direct-alloc.md) | Este tema analiza en profundidad una característica de C++/WinRT 2.0 que le ayuda a diagnosticar el error que se produce al crear un objeto de tipo de implementación en la pila, en lugar de usar la familia [**winrt::make**](/uwp/cpp-ref-for-winrt/make) de aplicaciones auxiliares como correspondería. |
| [Puntos de extensión para los tipos de implementación](./details-about-destructors.md) | Estos puntos de extensión en C++/WinRT 2.0 permiten aplazar la destrucción de los tipos de implementación, a fin de realizar consultas de forma segura durante la destrucción y enlazar la entrada y salida de los métodos proyectados. |
| [Un ejemplo sencillo de biblioteca de interfaz de usuario de Windows para C++/WinRT](./simple-winui-example.md) | Este tema te ayudará durante el proceso de adición de compatibilidad simple con WinUI en un proyecto de C++/WinRT. |
| [Componentes de Windows Runtime con C++/WinRT](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md) | En este tema se muestra cómo usar C++/WinRT para crear y consumir un componente de Windows Runtime&mdash;, que es un componente que se puede llamar desde una aplicación universal de Windows compilada mediante el lenguaje de Windows Runtime. |

### <a name="topics-about-the-c-language"></a>Temas sobre el lenguaje C++

| Tema | Descripción |
| - | - |
| [Categorías de valor y referencias a ellas](./cpp-value-categories.md) | En este tema se describen las distintas categorías de valores que existen en C++. Sin duda habrás oído hablar de lvalues y rvalues, pero existen también otros tipos. |

## <a name="important-apis"></a>API importantes
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Biblioteca de plantillas de Windows Runtime C++ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)