---
description: Noticias y cambios en C++/WinRT.
title: Novedades de C / c++ / WinRT
ms.date: 01/29/2019
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, noticias, lo que de, new
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cb624a93a010dfe9784cf8c26beed12c6cf2f77d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639960"
---
# <a name="whats-new-in-cwinrt"></a>Novedades de C / c++ / WinRT

En la tabla siguiente contiene noticias y cambia a [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) en la última versión disponible con carácter general del SDK de Windows, que es 10.0.17763.0 (Windows 10, versión 1809). Estos cambios también pueden estar presentes en las versiones posteriores de SDK Insider Preview.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Noticias y los cambios, en la versión 10.0.17763.0 del SDK de Windows (Windows 10, versión 1809)

| Características nuevas o modificadas | Más información |
| - | - |
| **Cambio importante**. Para que se compile, C++ / c++ / WinRT no depende de los encabezados del SDK de Windows. | Consulte [aislada con respecto a los archivos de encabezado de Windows SDK](#isolation-from-windows-sdk-header-files), a continuación. |
| Ha cambiado el formato de sistema de proyecto de Visual Studio. | Consulte [cómo redestinar C++ / c++ / WinRT proyecto a una versión posterior del SDK de Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk), a continuación. |
| Existen nuevas funciones y clases base para ayudarle a pasar un objeto de colección a una función en tiempo de ejecución de Windows, o para implementar sus propias propiedades de la colección y los tipos de colección. | Consulte [colecciones con C++ / c++ / WinRT](collections.md). |
| Puede usar el [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) extensión de marcado con C++ / c++ / WinRT clases de tiempo de ejecución. | Para obtener más información y ejemplos de código, vea [información general sobre el enlace de datos](/windows/uwp/data-binding/data-binding-quickstart). |
| Soporte técnico para la cancelación de una corrutina permite registrar una devolución de llamada de cancelación. | Para obtener más información y ejemplos de código, vea [cancelar una operación asincrónica y las devoluciones de llamada de cancelación](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Al crear un delegado que apunta a una función miembro, puede establecer un fuerte o una referencia débil al objeto actual (en lugar de sin formato *esto* puntero) en el punto donde se registra el controlador. | Para obtener más información y ejemplos de código, vea el **si usa una función miembro como un delegado** subsección en la sección [con seguridad de acceso a la *esto* puntero con un delegado de control de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Se corrigieron errores que estaban descubiertos por conformidad mejorada de Visual Studio con el estándar de C++. La cadena de herramientas LLVM y Clang se aprovecha mejor también para validar C + + / cumplimiento de estándares de WinRT. | Ya no nos encontramos el problema descrito en [¿por qué no mi nuevo proyecto se compilará? Estoy usando Visual Studio 2017 (versión 15.8.0 o superior) y el SDK versión 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Otros cambios.

- **Cambio importante**. [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) ahora devuelve `void*` en lugar de `HSTRING`. Puede usar `static_cast<HSTRING>(get_abi(my_hstring));` para obtener una cadena HSTRING.
- **Cambio importante**. [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) ahora devuelve `void**` en lugar de `HSTRING*`. Puede usar `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` para obtener una cadena HSTRING *.
- **Cambio importante**. HRESULT ahora se proyecta como **winrt::hresult**. Si necesita un valor HRESULT (para la comprobación de tipos, o para admitir rasgos de tipo), puede `static_cast` un **winrt::hresult**. En caso contrario, **winrt::hresult** convierte a HRESULT, siempre y cuando incluya `unknwn.h` antes de incluir cualquier C++ / c++ / WinRT encabezados.
- **Cambio importante**. GUID ahora se proyecta como **winrt::guid**. Para las API que implementan, debe usar **winrt::guid** para los parámetros GUID. En caso contrario, **winrt::hresult** convierte en GUID, siempre y cuando incluya `unknwn.h` antes de incluir cualquier C++ / c++ / WinRT encabezados.
- **Cambio importante**. El [ **winrt::handle_type constructor** ](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) se ha protegido mediante la realización de explícita (es ahora más difícil para escribir código incorrecto con él). Si tiene que asignar un valor de identificador sin formato, llame a la [ **handle_type::attach función** ](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) en su lugar.
- **Cambio importante**. Las firmas de **WINRT_CanUnloadNow** y **WINRT_GetActivationFactory** han cambiado. No debe declarar estas funciones en absoluto. En su lugar, incluya `winrt/base.h` (que se incluye automáticamente si se incluye C++ / c++ / archivos de encabezado del espacio de nombres de WinRT Windows) para incluir las declaraciones de estas funciones.
- Para el [ **winrt::clock struct**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** están en desuso en favor de **from_file_time/to_file_time**.
- API que esperan **IBuffer** se simplifican los parámetros. Aunque la mayoría de las API prefiere colecciones o matrices, suficiente API se basan en **IBuffer** que debían ser más fácil usar estas API desde C++. Esta actualización proporciona acceso directo a los datos detrás de un **IBuffer** implementación, con la misma convención de nomenclatura de datos utilizada por los contenedores de la biblioteca estándar de C++. Esto evita también chocar con nombres de los metadatos que convencionalmente comienzan con una letra mayúscula.
- Generación de código mejorada: varias mejoras para reducir el tamaño del código, mejorar la inserción y optimizar la caché del generador.
- Quita la recursividad innecesaria. Cuando la hace la línea de comandos de referencia a una carpeta, en lugar de a un determinado `.winmd`, `cppwinrt.exe` herramienta ya no busca de forma recursiva `.winmd` archivos. El `cppwinrt.exe` herramienta también ahora controla los duplicados de manera más inteligente, lo que sea más resistente a errores del usuario y mal formado a `.winmd` archivos.
- Punteros inteligentes reforzados. Anteriormente, el revokers de evento no se pudo revocar cuando move-asignan un nuevo valor. Esto ayudó a descubrir un problema donde las clases de puntero inteligente no estaban Gestión fiable de asignación automática; ha modificado en el [ **winrt::com_ptr struct plantilla**](/uwp/cpp-ref-for-winrt/com-ptr). **winrt::com_ptr** se ha corregido, y el revokers de evento se ha corregido para controlar la semántica de transferencia correctamente para que estos revocarán tras la asignación.

> [!IMPORTANT]
> Se realizaron cambios importantes en el [C++ / c++ / extensión de Visual Studio (VSIX) de WinRT](https://aka.ms/cppwinrt/vsix), tanto en la versión 1.0.181002.2 y, a continuación, más adelante en la versión 1.0.190128.4. Para obtener información detallada de estos cambios y cómo afectan a los proyectos existentes, [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) y [versiones anteriores de la extensión VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="isolation-from-windows-sdk-header-files"></a>Aislamiento de los archivos de encabezado de Windows SDK

Potencialmente, esto es un cambio importante para el código.

Para que se compile, C++ / c++ / WinRT ya no depende de los archivos de encabezado desde el SDK de Windows. Archivos de encabezado de la biblioteca de tiempo de ejecución de C (CRT) y la biblioteca de plantillas estándar (STL) de C++ también no incluyen los encabezados del SDK de Windows. Y que mejora el cumplimiento de estándares, evita la dependencia involuntario y reduce en gran medida el número de macros que se deben protegerse.

Esta independencia significa que C++ / c++ / WinRT es ahora más portátil y con las normas y contribuye a mejorar la posibilidad de que se convierta en una biblioteca multiplataforma y compilador cruzado. También significa que C++ / c++ / WinRT encabezados no están afectadas negativamente macros.

Si previamente ha dejado a C++ / c++ / WinRT para incluir los encabezados de Windows en el proyecto, ahora deberá incluir usted mismo. Es, en cualquier caso, siempre recomendado para incluir explícitamente los encabezados que depende y no lo deja a otra biblioteca para incluirlas en el.

Actualmente, las únicas excepciones a aislamiento de archivo de encabezado de Windows SDK son intrínsecos y valores numéricos. No hay ningún problema conocido con estas últimas dependencias restantes.

En el proyecto, puede habilitar volver a la interoperabilidad con los encabezados del SDK de Windows si necesita. Por ejemplo, podría implementar una interfaz COM (arraigada en [ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Por ejemplo, incluye `unknwn.h` antes de incluir cualquier C++ / c++ / WinRT encabezados. Si lo hace C / c++ / WinRT biblioteca base para habilitar varios enlaces admitir las interfaces COM clásicas. Para obtener un ejemplo de código, vea [componentes COM de autor con C++ / c++ / WinRT](author-coclasses.md). De forma similar, incluir explícitamente los encabezados de Windows SDK que declaran tipos o funciones que desea llamar.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Cómo redirigir C++ / c++ / WinRT proyecto a una versión posterior del SDK de Windows

El método para redestinar el proyecto que es probable que produzca el problema del compilador y vinculador menor también es más trabajosa. Ese método implica crear un nuevo proyecto (como destino la versión del SDK de Windows de su elección) y, a continuación, copiar archivos a través el nuevo proyecto desde el antiguo. Habrá secciones de la antigua `.vcxproj` y `.vcxproj.filters` archivos que basta con que copie en Guardar, agregar archivos en Visual Studio.

Sin embargo, hay otras dos maneras de redirigir el proyecto en Visual Studio.

- Vaya a la propiedad del proyecto **General** \> **Windows SDK versión**y seleccione **todas las configuraciones de** y **todas las plataformas**. Establecer **Windows SDK versión** a la versión que desea tener como destino.
- En **el Explorador de soluciones**, haga clic en el nodo del proyecto, haga clic en **redestinar proyectos**, elija las versiones que desea tener como destino y, a continuación, haga clic en **Aceptar**.

Si aparecen errores del vinculador o compilador después de usar cualquiera de estos dos métodos, puede probar la solución de limpieza (**compilar** > **Limpiar solución** o elimine manualmente todos archivos y carpetas temporales) antes de intentar volver a crearla.

Si genera el compilador de C++ "*error C2039: 'IUnknown': no es un miembro de '\`espacio de nombres global''*", a continuación, agregue `#include <unknwn.h>` a la parte superior de su `pch.h` archivo (antes de incluir cualquier C++ / c++ / WinRT encabezados).

También es posible que deba agregar `#include <hstring.h>` después de eso.

Si genera el vinculador de C++ "*error LNK2019: símbolo externo sin resolver _WINRT_CanUnloadNow@0 hace referencia en la función _VSDesignerCanUnloadNow@0* ", a continuación, se puede resolver mediante la adición de `#define _VSDESIGNER_DONT_LOAD_AS_DLL` a su `pch.h` archivo.
