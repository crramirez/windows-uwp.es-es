---
author: stevewhims
description: Noticias y los cambios a C++ / WinRT.
title: Novedades en C++ / WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, noticias, ¿qué del, la nueva
ms.localizationpriority: medium
ms.openlocfilehash: bc6be28e112dfdd14b3585bd88ba066fbeae382d
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "4509122"
---
# <a name="whats-new-in-cwinrt"></a>Novedades en C++ / WinRT

La tabla siguiente contiene noticias y cambia a [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) en la versión más reciente disponible por lo general, del Windows SDK, que es 10.0.17763.0 (Windows 10, versión 1809). Estos cambios también pueden estar presentes en versiones posteriores de SDK Insider Preview.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Noticias y los cambios, en Windows SDK versión 10.0.17763.0 (Windows 10, versión 1809)

| Característica nuevo o cambiado | Más información |
| - | - |
| **Cambio importante**. Para que se compile, C++ / WinRT no depende de los encabezados de Windows SDK. | Consulta el [aislamiento de archivos de encabezado de Windows SDK](#isolation-from-windows-sdk-header-files), a continuación. |
| Ha cambiado el formato de sistema de proyecto de Visual Studio. | Consulta [cómo redestinar tu C++ / WinRT proyecto a una versión posterior del Windows SDK](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk), más adelante. |
| Hay nuevas funciones y clases base que te ayudarán a pasar un objeto de colección a una función de Windows Runtime, o para implementar tus propias propiedades de colección y los tipos de colección. | Consulta [colecciones con C++ / WinRT](collections.md). |
| Puedes usar la extensión de marcado [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) con tu C++ / WinRT que las clases en tiempo de ejecución. | Para obtener más información y ejemplos de código, consulta la [Introducción al enlace de datos](/windows/uwp/data-binding/data-binding-quickstart). |
| Soporte técnico para cancelar una corrutina te permite registrar una devolución de llamada de cancelación. | Para obtener más información y ejemplos de código, consulta [Cancelar una operación asincrónica y las devoluciones de llamada de cancelación](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Al crear a un delegado que señala a una función miembro, puede establecer una referencia fuerte o débil al objeto actual (en lugar de un puntero sin procesar *este* ) en el punto donde se registra el controlador. | Para obtener más información y ejemplos de código, consulta la sección de secundarias **Si usas una función miembro como un delegado** en la sección de [forma segura acceso a *este* puntero con un delegado de controlador de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Errores son fijos que estaban descubiertos por mayor conformidad con el estándar de C++ de Visual Studio. También mejor aprovechan la cadena de herramientas LLVM y Clang para validar C++ / conformidad con los estándares de WinRT. | ¿Ya no encontrará el problema se describe en [de por qué no mi nuevo proyecto de compilación? Uso Visual Studio 2017 (versión 15.8.0 o superior) y el SDK versión 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Otros cambios.

- **Cambio importante**. [**winrt::get_abi(winrt::hstring const&)**](/uwp/cpp-ref-for-winrt/get-abi) ahora devuelve `void*` en lugar de `HSTRING`. Puedes usar `static_cast<HSTRING>(get_abi(my_hstring));` para obtener una HSTRING.
- **Cambio importante**. [**winrt::put_abi(winrt::hstring&)**](/uwp/cpp-ref-for-winrt/put-abi) ahora devuelve `void**` en lugar de `HSTRING*`. Puedes usar `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` para obtener una HSTRING *.
- **Cambio importante**. HRESULT ahora se proyecta como **HRESULT**. Si necesitas un valor de HRESULT (para la comprobación de tipos, o para admitir características de tipo), a continuación, puedes `static_cast` un **HRESULT**. De lo contrario, **HRESULT** se convierte a HRESULT, siempre que incluyas `unknwn.h` antes de incluir cualquier C++ / WinRT encabezados.
- **Cambio importante**. GUID ahora se proyecta como **winrt::guid**. Las API que implementas, debes usar **winrt::guid** para los parámetros GUID. De lo contrario, **HRESULT** se convierte en GUID, siempre que incluyas `unknwn.h` antes de incluir cualquier C++ / WinRT encabezados.
- **Cambio importante**. El [**constructor de winrt::handle_type**](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) se ha reforzado realizando explícita (ahora es más difícil escribir código incorrecto con él). Si debes asignar un valor de identificador sin procesar, llama a la [**función handle_type::attach**](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) en su lugar.
- **Cambio importante**. Las firmas de **WINRT_CanUnloadNow** y **WINRT_GetActivationFactory** han cambiado. No declarar estas funciones en absoluto. En su lugar, se incluyen `winrt/base.h` (que se incluyen automáticamente Si incluyes cualquier C++ / archivos de encabezado de espacio de nombres de WinRT Windows) para incluir las declaraciones de estas funciones.
- Para la [**estructura de winrt::clock**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** están en desuso en favor de **from_file_time/to_file_time**.
- Se han simplificado las API que esperan **IBuffer** parámetros. Aunque la mayoría de las API prefiere colecciones o matrices, suficiente API dependen de **IBuffer** que necesite para que sea más fácil de usar estas API de C++. Esta actualización proporciona acceso directo a los datos de una implementación de **IBuffer** , usando la misma convención de nomenclatura de datos usada por los contenedores de la biblioteca estándar de C++. Esto también evita entre en conflicto con los nombres de metadatos que suelen ser comienzan con una letra mayúscula.
- Mejora de la generación de código: varias mejoras para reducir el tamaño del código, mejorar la inclusión y optimizar el almacenamiento en caché de fábrica.
- Quita la repetición innecesaria. Cuando la línea de comandos se hace referencia a una carpeta, en lugar de un determinado `.winmd`, el `cppwinrt.exe` herramienta ya no busca de forma recursiva `.winmd` archivos. El `cppwinrt.exe` ahora también controla herramienta duplicados más inteligente, lo que más resistente a errores del usuario y a mal formado `.winmd` archivos.
- Reforzado punteros inteligentes. Anteriormente, el revokers de evento no se pudo revocar al movimiento asignan un nuevo valor. Esto ayudó a descubrir un problema donde clases de puntero inteligente no estaban controlar de forma confiable asignación automática; con la raíz en la [**plantilla de estructura winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr). se ha corregido **winrt:: com_ptr** y el revokers de evento fijadas para controlar mover la semántica correctamente para que revocar al asignarse.

> [!NOTE]
> Con la versión 1.0.181002.2 (o posterior) de la [C++ / extensión de Visual Studio (VSIX) de WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix) instalado, creando un nuevo C++ / WinRT proyecto instala automáticamente el [paquete de Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para ese proyecto. El paquete de Microsoft.Windows.CppWinRT NuGet proporciona mejor C++ / compatibilidad de compilación del proyecto WinRT, hacer que el proyecto portable entre un equipo de desarrollo y un agente de compilación (en el que el paquete de NuGet y no VSIX, se instala).
>
> Para un proyecto existente&mdash;después de que has instalado la versión 1.0.181002.2 (o posterior) de la extensión VSIX&mdash;te recomendamos que abre el proyecto en Visual Studio, haz clic en el **proyecto** \> **Administrar paquetes de NuGet …**  \>  **Examinar**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de búsqueda y, a continuación, haz clic en **instalar** para instalar el paquete de ese proyecto.


## <a name="isolation-from-windows-sdk-header-files"></a>Aislamiento de archivos de encabezado de SDK de Windows

Esto es potencialmente un cambio importante para el código.

Para que se compile, C++ / WinRT ya no depende de los archivos de encabezado desde el SDK de Windows. Archivos de encabezado en la biblioteca de tiempo de ejecución de C (CRT) y la biblioteca de plantillas estándar (STL) de C++ también no incluyen cualquier encabezado de SDK de Windows. Y que mejora el cumplimiento de los estándares, evita dependencias involuntarias y reduce en gran medida el número de macros que se deben protegerse.

Esta independencia significa que C++ / WinRT es ahora más portátil y compatible con los estándares y contribuye a mejorar la posibilidad de que se convierta en una biblioteca de compilador cruzado y multiplataforma. Esto también significa que el C++ / WinRT encabezados no son macros afectadas negativamente.

Si ha dejado anteriormente a C++ / WinRT para incluir cualquier encabezado de Windows en el proyecto, ahora tendrás que incluir tú mismo. Es, en cualquier caso, siempre mejor práctica para incluir explícitamente los encabezados que dependa de y no dejar que otra biblioteca para incluirlos para TI.

Actualmente, la única excepción a aislamiento de archivo de encabezado de SDK de Windows es para intrínsecos y valores numéricos. No existen problemas conocidos con estas dependencias restantes últimos.

En el proyecto, puedes volver a habilitar la interoperabilidad con los encabezados de SDK de Windows si es necesario. Por ejemplo, podría implementar una interfaz COM (con la raíz en [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Por ejemplo, incluye `unknwn.h` antes de incluir cualquier C++ / WinRT encabezados. Al hacerlo así causas C++ / WinRT biblioteca base para permitir que varios enlaces admitir las interfaces de COM clásicas. Para ver un ejemplo de código, [componentes de autor COM con C++ / WinRT](author-coclasses.md). Del mismo modo, incluir cualquier encabezado de SDK de Windows que declaren los tipos y funciones que quieres llamar explícitamente.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Cómo redestinar tu C++ / WinRT proyecto a una versión posterior del Windows SDK

El método para redestinar tu proyecto que es probable que provocar el problema de compilador y enlazador menor también es más intensa. Ese método implica crear un nuevo proyecto (destinados a la versión del SDK de Windows de tu elección) y, a continuación, copiar archivos en tu nuevo proyecto del antiguo. Habrá secciones de la antigua `.vcxproj` y `.vcxproj.filters` archivos que has puede copiar más guardar puedes agregar los archivos en Visual Studio.

Sin embargo, hay dos formas de otras que volver a destinar el proyecto en Visual Studio.

- Ir a la propiedad **General**de proyecto \> **Versión del SDK de Windows**y selecciona **Todas las configuraciones** y **Todas las plataformas**. Establece la **Versión del SDK de Windows** en la versión que quieres como destino.
- En el **Explorador de soluciones**, haz clic en el nodo del proyecto, haz clic en **Proyectos redestinar**, elegir las versiones de que destino y, a continuación, haz clic en **Aceptar**.

Si se producen cualquier compilador o el error de vinculador después de usar cualquiera de estos dos métodos, puedes intentar la limpieza de la solución (**compilación** > **Limpiar solución** o eliminar manualmente todos los archivos y carpetas de temporales) antes de intentar volver a compilar.

Si el compilador de C++ produce "*error C2039: 'IUnknown': no es un miembro de ' espacio de nombres \'global''*", a continuación, agrega `#include <unknwn.h>` a la parte superior de tu `pch.h` archivo (antes de incluir cualquier C++ / WinRT encabezados).

Es posible que también debes agregar `#include <hstring.h>` a continuación.

Si el vinculador C++ produce "*error LNK2019: símbolo externo sin resolver _WINRT_CanUnloadNow@0 hace referencia en función _VSDesignerCanUnloadNow@0 *", a continuación, se puede resolver que agregando `#define _VSDESIGNER_DONT_LOAD_AS_DLL` a tu `pch.h` archivo.
