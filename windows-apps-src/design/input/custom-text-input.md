---
description: Las API de texto principales del espacio de nombres Windows. UI. Text. Core permiten a una aplicación de Windows recibir entradas de texto de cualquier servicio de texto compatible con dispositivos Windows.
title: Introducción a la entrada de texto personalizado
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: teclado, texto, texto principal, texto personalizado, Text Services Framework, entrada, interacciones del usuario
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 95dbd6de78cb6670ea7e904252bbc1f9f14edb77
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829639"
---
# <a name="custom-text-input"></a>Entrada de texto personalizado



Las API de texto principales del espacio de nombres [**Windows. UI. Text. Core**](/uwp/api/Windows.UI.Text.Core) permiten a una aplicación de Windows recibir entradas de texto de cualquier servicio de texto compatible con dispositivos Windows. Las API son similares a las API del [Text Services Framework](/windows/desktop/TSF/text-services-framework) en que no es necesario que la aplicación tenga información detallada sobre los servicios de texto. Esto permite que la aplicación reciba texto en cualquier idioma y de cualquier tipo de entrada, como el teclado, voz o lápiz.

> **API importantes**: [**Windows. UI. Text. Core**](/uwp/api/Windows.UI.Text.Core), [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext)

## <a name="why-use-core-text-apis"></a>¿Por qué usar API de texto principales?


Para muchas aplicaciones, basta con los controles de cuadro de texto XAML o HTML para la entrada y edición de texto. Sin embargo, si la aplicación controla escenarios de texto complejos, como una aplicación de procesamiento de texto, es posible que necesites la flexibilidad de un control de edición de texto personalizado. Puedes usar las API de teclado de [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) para crear tus controles de edición de texto, pero estas no proporcionan una forma de recibir una entrada de texto basado en la composición, necesaria para admitir los idiomas de Asia oriental.

En su lugar, usa las API [**Windows.UI.Text.Core**](/uwp/api/Windows.UI.Text.Core) cuando necesites crear un control de edición de texto personalizado. Estas API están diseñadas para ofrecerte mucha flexibilidad al procesar la entrada de texto en cualquier idioma y dejar que proporciones la experiencia de texto que mejor se adapte a tu aplicación. La entrada de texto y los controles de edición integrados con las API de texto principales pueden recibir entrada de texto de todos los métodos de entrada existentes en los dispositivos de Windows, desde los editores de métodos de entrada (IME) y escritura a mano en PC basados en [Text Services Framework](/windows/desktop/TSF/text-services-framework) hasta el teclado Word Flow en dispositivos móviles, que proporciona corrección automática, predicción y dictado.

## <a name="architecture"></a>Architecture


A continuación te mostramos una representación sencilla del sistema de entrada de texto.

-   "Application" representa una aplicación de Windows que hospeda un control de edición personalizado creado mediante las API de texto principales.
-   La API [**Windows.UI.Text.Core**](/uwp/api/Windows.UI.Text.Core) facilitan la comunicación con los servicios de texto a través de Windows. La comunicación entre el control de edición de texto y los servicios de texto se controla principalmente mediante un objeto [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) que proporciona los métodos y eventos para facilitar dicha comunicación.

![Diagrama de arquitectura de CoreText](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>Intervalos y selección de texto


Los controles de edición proporcionan espacio para la entrada de texto y los usuarios esperan poder editar texto en cualquier parte de dicho espacio. Aquí explicamos el sistema de posicionamiento de texto usado por las API de texto principales y cómo se representan los intervalos y selecciones de texto en este sistema.

### <a name="application-caret-position"></a>Posición del cursor de inserción de la aplicación

Los intervalos de texto usados con las API de texto principales se expresan en posiciones del cursor de inserción. Una "Application Caret Position (ACP)" es un número de base cero que indica el número de caracteres desde el inicio del flujo de texto justo antes del cursor de inserción, tal y como se muestra aquí.

![Captura de pantalla que muestra el número de caracteres de la posición del símbolo de intercalación (ACP)](images/coretext/stream-1.png)

### <a name="text-ranges-and-selection"></a>Intervalos y selección de texto

Los intervalos y selecciones de texto se representan mediante la estructura [**CoreTextRange**](/uwp/api/Windows.UI.Text.Core.CoreTextRange) que contiene dos campos:

| Campo                  | Tipo de datos                                                                 | Descripción                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Número** \[ de Código\] | **System. Int32** \[ .net\] | **Int32** \[ C\] | La posición de inicio de un intervalo es la ACP inmediatamente anterior al primer carácter. |
| **EndCaretPosition**   | **Número** \[ de Código\] | **System. Int32** \[ .net\] | **Int32** \[ C\] | La posición final del intervalo es la ACP inmediatamente posterior al último carácter.     |

 

Por ejemplo, en el intervalo de texto mostrado anteriormente, el intervalo \[ 0, 5 \] especifica la palabra "Hello". **StartCaretPosition** debe ser siempre menor o igual que **EndCaretPosition**. El intervalo \[ 5, 0 \] no es válido.

### <a name="insertion-point"></a>Punto de inserción

La posición del símbolo de intercalación actual, a la que se hace referencia con frecuencia como punto de inserción, se representa estableciendo **StartCaretPosition** para que sea igual a **EndCaretPosition**.

### <a name="noncontiguous-selection"></a>Selección no contigua

Algunos controles de edición admiten selecciones no contiguas. Por ejemplo, las aplicaciones de Microsoft Office admiten varias selecciones arbitrarias y muchos editores de código fuente admiten la selección de columna. Sin embargo, las API de texto principales no admiten selecciones no contiguas. Los controles de edición deben notificar una única selección contigua, principalmente el subintervalo activo de las selecciones no contiguas.

Por ejemplo, en la imagen siguiente se muestra una secuencia de texto con dos selecciones no contiguas: \[ 0, 1 \] y \[ 6, 11 \] para las que el control de edición debe notificar solo una ( \[ 0, 1 \] o \[ 6, 11 \] ).

![Captura de pantalla que muestra una selección de texto no contiguo, donde se seleccionan el primer carácter y los cinco últimos caracteres.](images/coretext/stream-2.png)

## <a name="working-with-text"></a>Trabajar con texto


La clase [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext) habilita el flujo de texto entre Windows y los controles de edición a través del evento [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating), el evento [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) y el método [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged).

El control de edición recibe texto mediante los eventos [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) que se generan cuando los usuarios interactúan con métodos de entrada de texto, como los teclados, la voz o el IME.

Al cambiar el texto en el control de edición, por ejemplo pegando texto en el control, debe notificar a Windows mediante una llamada a [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged).

Si el servicio de texto requiere el texto nuevo, se genera un evento [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested). Debes proporcionar el texto nuevo en el controlador de eventos **TextRequested**.

### <a name="accepting-text-updates"></a>Aceptar actualizaciones de texto

El control de edición normalmente debe aceptar solicitudes de actualización de texto ya que representan el texto que el usuario quiere escribir. En el controlador de eventos [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) se espera que el control de edición realice las siguientes acciones:

1.  Insertar el texto especificado en [**CoreTextTextUpdatingEventArgs.Text**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) en la posición especificada en [**CoreTextTextUpdatingEventArgs.Range**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range).
2.  Coloca la selección en la posición especificada en [**CoreTextTextUpdatingEventArgs. NewSelection**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection).
3.  Notificar al sistema que la actualización se realizó correctamente estableciendo [**CoreTextTextUpdatingEventArgs.Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) en [**CoreTextTextUpdatingResult.Succeeded**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult).

Por ejemplo, este es el estado de un control de edición antes que el usuario escriba "d". El punto de inserción es de \[ 10 a 10 \] .

![Captura de pantalla de un diagrama de flujo de texto que muestra el punto de inserción en \[ 10, 10 \] antes de una inserción](images/coretext/stream-3.png)

Cuando el usuario escribe "d", se genera un evento [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating) con los siguientes datos [**CoreTextTextUpdatingEventArgs**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingEventArgs):

-   [**Intervalo de**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range)  =  \[ 10, 10\]
-   [**Text**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) = "d"
-   [**NewSelection**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection)  =  NewSelection \[ 11, 11\]

En el control de edición, aplica los cambios especificados y establece [**Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) en **Succeeded**. A continuación te mostramos el estado del control después de que se apliquen los cambios.

:::image type="content" source="images/coretext/stream-4.png" alt-text="Captura de pantalla de un diagrama de flujo de texto que muestra el punto de inserción a las \[ 11, 11 \] , después de una inserción":::

### <a name="rejecting-text-updates"></a>Rechazar actualizaciones de texto

A veces no es posible aplicar actualizaciones de texto porque el intervalo solicitado está en un área del control de edición que no se debe modificar. En este caso, no debes aplicar ningún cambio. En su lugar, notifica al sistema que la actualización no se pudo realizar estableciendo [**CoreTextTextUpdatingEventArgs.Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) en [**CoreTextTextUpdatingResult.Failed**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult).

Por ejemplo, imagina que un control de edición solo acepta una dirección de correo electrónico. Se deben rechazar los espacios porque las direcciones de correo electrónico no pueden contener espacios, por tanto, cuando la barra espaciadora genera los eventos [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating), simplemente debes establecer [**Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) en **Failed** en el control de edición.

### <a name="notifying-text-changes"></a>Notificar los cambios de texto

A veces el control de edición realiza cambios en el texto, como cuando el texto se pega o se corrige de forma automática. En estos casos, debes notificar a los servicios de texto estos cambios mediante una llamada al método [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged).

Por ejemplo, este es el estado de un control de edición antes de que el usuario pegue "World". El punto de inserción se encuentra en \[ 6, 6 \] .

![Captura de pantalla de un diagrama de flujo de texto que muestra el punto de inserción en \[ 6, 6 \] antes de una inserción](images/coretext/stream-5.png)

El usuario realiza la acción de pegar y el control de edición una vez aplicados los cambios:

:::image type="content" source="images/coretext/stream-4.png" alt-text="Captura de pantalla de un diagrama de flujo de texto que muestra el punto de inserción a las \[ 11, 11 \] , después de una inserción":::

Cuando esto sucede, debes llamar a [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) con estos argumentos:

-   *modifiedRange*  =  modifiedRange \[ 6, 6\]
-   *newLength* = 5
-   *newSelection*  =  newSelection \[ 11, 11\]

Aparecerán uno o más eventos [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested), que administrarás para actualizar el texto con el que trabajan los servicios de texto.

### <a name="overriding-text-updates"></a>Reemplazar las actualizaciones de texto

Es posible que quieras reemplazar una actualización de texto en el control de edición para proporcionar funciones de corrección automática.

Por ejemplo, imagina un control de edición que proporciona una función de corrección que formaliza las contracciones. A continuación te mostramos el estado del control de edición antes de que el usuario presione la barra espaciadora para desencadenar la corrección. El punto de inserción es \[ 3, 3 \] .

![Captura de pantalla de un diagrama de flujo de texto que muestra el punto de inserción en \[ 3, 3 \] antes de una inserción](images/coretext/stream-6.png)

El usuario presiona la barra espaciadora y se genera el correspondiente evento [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating). El control de edición acepta la actualización de texto. Este es el estado del control de edición un instante antes de que finalice la corrección. El punto de inserción es \[ 4, 4 \] .

![Captura de pantalla de un diagrama de flujo de texto que muestra el punto de inserción en \[ 4, 4 \] después de una inserción](images/coretext/stream-7.png)

Fuera del controlador de eventos [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating), el control de edición realiza la siguiente corrección. Este es el estado del control de edición una vez completada la corrección. El punto de inserción está en \[ 5, 5 \] .

![Captura de pantalla de un diagrama de flujo de texto que muestra el punto de inserción en \[ 5, 5\]](images/coretext/stream-8.png)

Cuando esto sucede, debes llamar a [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) con estos argumentos:

-   *modifiedRange*  =  modifiedRange \[ 1, 2\]
-   *newLength* = 2
-   *newSelection*  =  newSelection \[ 5, 5\]

Aparecerán uno o más eventos [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested), que administrarás para actualizar el texto con el que trabajan los servicios de texto.

### <a name="providing-requested-text"></a>Proporcionar el texto solicitado

Es importante que los servicios de texto tengan el texto correcto para proporcionar funciones tales como autocorrección o predicción, especialmente para texto existente en el control de edición, por ejemplo, al cargar un documento, o el texto que inserta el control de edición como se explica en las secciones anteriores. Por lo tanto, siempre que se genere un evento [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) debes proporcionar el texto actual del control de edición para el intervalo especificado.

Habrá veces que [**Range**](/uwp/api/windows.ui.text.core.coretexttextrequest.range) en [**CoreTextTextRequest**](/uwp/api/Windows.UI.Text.Core.CoreTextTextRequest) especificará un intervalo que el control de edición no puede integrar tal cual. Por ejemplo, el **Range** es mayor que el tamaño del control de edición en el momento del evento [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested) o el final de **Range** sobrepasa los límites. En estos casos, debes volver a cualquier intervalo que tenga sentido, que suele ser un subconjunto del intervalo solicitado.

## <a name="related-articles"></a>Artículos relacionados

### <a name="samples"></a>Ejemplos

- [Ejemplo de control de edición personalizado](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)

### <a name="archive-samples"></a>Ejemplos de archivo

- [Ejemplo de edición de texto XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
