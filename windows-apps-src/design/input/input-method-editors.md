---
Description: Un editor de métodos de entrada (IME) es un componente de software que permite al usuario escribir texto en un idioma que no se puede representar fácilmente en un teclado QWERTY estándar.
title: Editores de métodos de entrada (IME)
label: Input Method Editors (IME)
template: detail.hbs
keywords: IME, editor de métodos de entrada, entrada, interacción
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 438c53a0f3fbec1fdac0206bde3584c738759de4
ms.sourcegitcommit: 86ce67a03e87fa1282849b2fcb4f89d1cf23a091
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/05/2020
ms.locfileid: "87840054"
---
# <a name="input-method-editors-ime"></a>Editores de métodos de entrada (IME)

Un editor de métodos de entrada (IME) es un componente de software que permite al usuario escribir texto en un idioma que no se puede representar fácilmente en un teclado QWERTY estándar. Esto suele deberse al número de caracteres del idioma escrito del usuario, como los distintos idiomas de Asia oriental.

En lugar de cada carácter individual que aparece en una sola tecla del teclado, un usuario escribe combinaciones de teclas que el IME interpreta. El IME genera el carácter que coincide con el conjunto de trazos de tecla o una lista de caracteres candidatos entre los que elegir. A continuación, se inserta el carácter seleccionado en el control de edición con el que el usuario está interactuando.

> [!NOTE]
> Los IME pueden admitir teclados de hardware y teclados en pantalla o táctiles.

No es necesario que la aplicación interactúe directamente con el IME. El IME está integrado en el sistema, del mismo modo que el teclado táctil. Si la aplicación tiene una entrada de texto y tiene previsto admitir la entrada de texto en idiomas que requieren un IME, debe probar la experiencia del cliente de un extremo a otro para la entrada de texto. Esto le permite corregir cualquier problema, como ajustar la interfaz de usuario para que no se ocluidos por el teclado táctil o la ventana de candidato de IME.

## <a name="creating-an-ime"></a>Crear un IME

Para habilitar una gran experiencia de entrada para todos los usuarios, Microsoft produce los IME que se incluyen en la caja para una variedad de idiomas.

Además de los IME integrados, puede crear sus propios IME personalizados que los usuarios pueden instalar y usar de la misma manera que un IME integrado.

Todos los IME se ejecutan en el sistema Windows, que se protege para detener los IME malintencionados y mejorar la seguridad y la experiencia del usuario de todos los IME.

Los IME personalizados pueden vincularse al teclado táctil predeterminado y usar su diseño para que los usuarios finales puedan usar su IME con el teclado táctil. Sin embargo, no puede proporcionar su propio teclado táctil independiente y ciertas funciones de los IME integrados para los teclados táctiles no están disponibles para los IME personalizados.

## <a name="requirements-for-imes"></a>Requisitos para los IME

Un IME de terceros debe cumplir estos requisitos:

- Debe estar firmado digitalmente
- Debe tener en cuenta el [marco de trabajo de servicios de texto (TSF)](/windows/win32/tsf/text-services-framework) , con las marcas de IME adecuadas establecidas correctamente
- Debe seguir las instrucciones descritas en [requisitos del editor de métodos de entrada (IME)](input-method-editor-requirements.md) y [diseñar y codificar aplicaciones de Windows](/windows/uwp/design/) .

Se bloquea la ejecución de un IME de terceros que no cumple estos requisitos.

> [!NOTE]
> Los IME personalizados heredados se pueden ejecutar en aplicaciones de escritorio, pero están bloqueados en aplicaciones de Windows.

Además, Windows Defender quita los IME malintencionados del sistema. Por este motivo, es importante que se familiarice con los requisitos de codificación del IME. Para obtener más información, consulte [requisitos del editor de métodos de entrada (IME)](input-method-editor-requirements.md).

## <a name="design-guidelines-for-imes"></a>Directrices de diseño para los IME

Lea los [requisitos del editor de métodos de entrada (IME)](input-method-editor-requirements.md) para obtener más detalles sobre los procedimientos recomendados y las directrices de diseño para los IME. En general, todas las IUs de IME deben:

- Siga las directrices de la experiencia del usuario para las aplicaciones Windows Runtime
- Evitar experiencias modales y mostrar únicamente la ventana del IME cuando sea necesario
- incluir iconos solo en blanco y negro

## <a name="related-topics"></a>Temas relacionados

- [Requisitos del editor de métodos de entrada (IME)](input-method-editor-requirements.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx::GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfContextView::GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
