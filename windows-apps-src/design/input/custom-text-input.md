---
author: Karl-Bridge-Microsoft
Description: The core text APIs in the Windows.UI.Text.Core namespace enable a Universal Windows Platform (UWP) app to receive text input from any text service supported on Windows devices.
title: Introducción a la entrada de texto personalizado
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: teclado, texto, texto principal, texto personalizado, Text Services Framework, entrada, interacciones del usuario
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 14a2811f59b8de33db51b255aee8892abf553198
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "6450160"
---
# <a name="custom-text-input"></a>Entrada de texto personalizado



Las API de texto principales en el espacio de nombres [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) habilitan una aplicación de la Plataforma universal de Windows (UWP) para recibir una entrada de texto desde cualquier servicio de texto compatible con dispositivos Windows. Las API son similares a las API del [Text Services Framework](https://msdn.microsoft.com/library/windows/desktop/ms629032) en que no es necesario que la aplicación tenga información detallada sobre los servicios de texto. Esto permite que la aplicación reciba texto en cualquier idioma y de cualquier tipo de entrada, como el teclado, voz o lápiz.

> **API importantes**: [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238), [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)

## <a name="why-use-core-text-apis"></a>¿Por qué usar API de texto principales?


Para muchas aplicaciones, basta con los controles de cuadro de texto XAML o HTML para la entrada y edición de texto. Sin embargo, si la aplicación controla escenarios de texto complejos, como una aplicación de procesamiento de texto, es posible que necesites la flexibilidad de un control de edición de texto personalizado. Puedes usar las API de teclado de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) para crear tus controles de edición de texto, pero estas no proporcionan una forma de recibir una entrada de texto basado en la composición, necesaria para admitir los idiomas de Asia oriental.

En su lugar, usa las API [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) cuando necesites crear un control de edición de texto personalizado. Estas API están diseñadas para ofrecerte mucha flexibilidad al procesar la entrada de texto en cualquier idioma y dejar que proporciones la experiencia de texto que mejor se adapte a tu aplicación. La entrada de texto y los controles de edición integrados con las API de texto principales pueden recibir entrada de texto de todos los métodos de entrada existentes en los dispositivos de Windows, desde los editores de métodos de entrada (IME) y escritura a mano en PC basados en [Text Services Framework](https://msdn.microsoft.com/library/windows/desktop/ms629032) hasta el teclado Word Flow en dispositivos móviles, que proporciona corrección automática, predicción y dictado.

## <a name="architecture"></a>Arquitectura


A continuación te mostramos una representación sencilla del sistema de entrada de texto.

-   "Application" representa una aplicación para UWP creada con las API de texto principales y que hospeda un control de edición personalizado.
-   La API [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238) facilitan la comunicación con los servicios de texto a través de Windows. La comunicación entre el control de edición de texto y los servicios de texto se controla principalmente mediante un objeto [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) que proporciona los métodos y eventos para facilitar dicha comunicación.

![diagrama de arquitectura de texto principal](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>Intervalos y selección de texto


Los controles de edición proporcionan espacio para la entrada de texto y los usuarios esperan poder editar texto en cualquier parte de dicho espacio. Aquí explicamos el sistema de posicionamiento de texto usado por las API de texto principales y cómo se representan los intervalos y selecciones de texto en este sistema.

### <a name="application-caret-position"></a>Posición del cursor de inserción de la aplicación

Los intervalos de texto usados con las API de texto principales se expresan en posiciones del cursor de inserción. Una "Application Caret Position (ACP)" es un número de base cero que indica el número de caracteres desde el inicio del flujo de texto justo antes del cursor de inserción, tal y como se muestra aquí.

![diagrama de flujo de texto de ejemplo](images/coretext/stream-1.png)
### <a name="text-ranges-and-selection"></a>Intervalos y selección de texto

Los intervalos y selecciones de texto se representan mediante la estructura [**CoreTextRange**](https://msdn.microsoft.com/library/windows/apps/dn958201) que contiene dos campos:

| Campo                  | Tipo de datos                                                                 | Descripción                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Número** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | La posición de inicio de un intervalo es la ACP inmediatamente anterior al primer carácter. |
| **EndCaretPosition**   | **Número** \[JavaScript\] | **System.Int32** \[.NET\] | **int32** \[C++\] | La posición final del intervalo es la ACP inmediatamente posterior al último carácter.     |

 

Por ejemplo, en el intervalo de texto que se mostró anteriormente, el intervalo \[0, 5\] especifica la palabra "Hello". **StartCaretPosition** debe ser siempre menor o igual que **EndCaretPosition**. El intervalo \[5, 0\] no es válido.

### <a name="insertion-point"></a>Punto de inserción

La posición del cursor de inserción, normalmente denominada punto de inserción, se representa estableciendo la **StartCaretPosition** igual a **EndCaretPosition**.

### <a name="noncontiguous-selection"></a>Selección no contigua

Algunos controles de edición admiten selecciones no contiguas. Por ejemplo, las aplicaciones de Microsoft Office admiten varias selecciones arbitrarias y muchos editores de código fuente admiten la selección de columna. Sin embargo, las API de texto principales no admiten las selecciones no contiguas. Los controles de edición deben notificar una única selección contigua, principalmente el subintervalo activo de las selecciones no contiguas.

Por ejemplo, observa este flujo de texto:

![diagrama de flujo de texto de ejemplo](images/coretext/stream-2.png) Hay dos opciones: \[0, 1\] y \[6, 11\]. El control de edición debe notificar solo uno de ellos; ya sea \[0, 1\] o \[6, 11\].

## <a name="working-with-text"></a>Trabajar con texto


La clase [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158) habilita el flujo de texto entre Windows y los controles de edición a través del evento [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176), el evento [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) y el método [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172).

El control de edición recibe texto mediante los eventos [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) que se generan cuando los usuarios interactúan con métodos de entrada de texto, como los teclados, la voz o el IME.

Cuando modificas el texto en el control de edición, por ejemplo, al pegar texto en el control, tienes que notificar a Windows mediante una llamada a [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172).

Si el servicio de texto requiere el texto nuevo, se genera un evento [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175). Debes proporcionar el texto nuevo en el controlador de eventos **TextRequested**.

### <a name="accepting-text-updates"></a>Aceptar actualizaciones de texto

El control de edición normalmente debe aceptar solicitudes de actualización de texto ya que representan el texto que el usuario quiere escribir. En el controlador de eventos [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) se espera que el control de edición realice las siguientes acciones:

1.  Insertar el texto especificado en [**CoreTextTextUpdatingEventArgs.Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) en la posición especificada en [**CoreTextTextUpdatingEventArgs.Range**](https://msdn.microsoft.com/library/windows/apps/dn958234).
2.  Colocar la selección en la posición especificada en [**CoreTextTextUpdatingEventArgs.NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233).
3.  Notificar al sistema que la actualización se realizó correctamente estableciendo [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) en [**CoreTextTextUpdatingResult.Succeeded**](https://msdn.microsoft.com/library/windows/apps/dn958237).

Por ejemplo, este es el estado de un control de edición antes que el usuario escriba "d". El punto de inserción es en \[10, 10\].

![diagrama de flujo de texto de ejemplo](images/coretext/stream-3.png) Cuando el usuario escribe "d", se genera un evento [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176) con los siguientes datos [**CoreTextTextUpdatingEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958229):

-   [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958234) = \[10, 10\]
-   [**Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) = "d"
-   [**NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) = \[11, 11\]

En el control de edición, aplica los cambios especificados y establece [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) en **Succeeded**. A continuación te mostramos el estado del control después de que se apliquen los cambios.

![diagrama de flujo de texto de ejemplo](images/coretext/stream-4.png)
### <a name="rejecting-text-updates"></a>Rechazar actualizaciones de texto

A veces no es posible aplicar actualizaciones de texto porque el intervalo solicitado está en un área del control de edición que no se debe modificar. En este caso, no debes aplicar ningún cambio. En su lugar, notifica al sistema que la actualización no se pudo realizar estableciendo [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) en [**CoreTextTextUpdatingResult.Failed**](https://msdn.microsoft.com/library/windows/apps/dn958237).

Por ejemplo, imagina que un control de edición solo acepta una dirección de correo electrónico. Se deben rechazar los espacios porque las direcciones de correo electrónico no pueden contener espacios, por tanto, cuando la barra espaciadora genera los eventos [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176), simplemente debes establecer [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) en **Failed** en el control de edición.

### <a name="notifying-text-changes"></a>Notificar los cambios de texto

A veces el control de edición realiza cambios en el texto, como cuando el texto se pega o se corrige de forma automática. En estos casos, debes notificar a los servicios de texto estos cambios mediante una llamada al método [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172).

Por ejemplo, este es el estado de un control de edición antes de que el usuario pegue "World". El punto de inserción es en \[6, 6\].

![diagrama de flujo de texto de ejemplo](images/coretext/stream-5.png) El usuario realiza la acción pegar y el control de edición quedaría con el texto siguiente:

![diagrama de flujo de texto de ejemplo](images/coretext/stream-4.png) Cuando esto ocurre, se debe llamar a [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) con estos argumentos:

-   *modifiedRange* = \[6, 6\]
-   *newLength* = 5
-   *newSelection* = \[11, 11\]

Aparecerán uno o más eventos [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175), que administrarás para actualizar el texto con el que trabajan los servicios de texto.

### <a name="overriding-text-updates"></a>Reemplazar las actualizaciones de texto

Es posible que quieras reemplazar una actualización de texto en el control de edición para proporcionar funciones de corrección automática.

Por ejemplo, imagina un control de edición que proporciona una función de corrección que formaliza las contracciones. A continuación te mostramos el estado del control de edición antes de que el usuario presione la barra espaciadora para desencadenar la corrección. El punto de inserción es en \[3, 3\].

![diagrama de flujo de texto de ejemplo](images/coretext/stream-6.png) El usuario presiona la barra espaciadora y se genera el correspondiente evento [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176). El control de edición acepta la actualización de texto. Este es el estado del control de edición un instante antes de que finalice la corrección. El punto de inserción es en \[4, 4\].

![diagrama de flujo de texto de ejemplo](images/coretext/stream-7.png) Fuera del controlador de eventos [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176), el control de edición realiza la siguiente corrección. Este es el estado del control de edición una vez completada la corrección. El punto de inserción es en \[5, 5\].

![diagrama de flujo de texto de ejemplo](images/coretext/stream-8.png) Cuando esto ocurre, se debe llamar a [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) con estos argumentos:

-   *modifiedRange* = \[1, 2\]
-   *newLength* = 2
-   *newSelection* = \[5, 5\]

Aparecerán uno o más eventos [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175), que administrarás para actualizar el texto con el que trabajan los servicios de texto.

### <a name="providing-requested-text"></a>Proporcionar el texto solicitado

Es importante que los servicios de texto tengan el texto correcto para proporcionar funciones tales como autocorrección o predicción, especialmente para texto existente en el control de edición, por ejemplo, al cargar un documento, o el texto que inserta el control de edición como se explica en las secciones anteriores. Por lo tanto, siempre que se genere un evento [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) debes proporcionar el texto actual del control de edición para el intervalo especificado.

Habrá veces que [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958227) en [**CoreTextTextRequest**](https://msdn.microsoft.com/library/windows/apps/dn958221) especificará un intervalo que el control de edición no puede integrar tal cual. Por ejemplo, el **Range** es mayor que el tamaño del control de edición en el momento del evento [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175) o el final de **Range** sobrepasa los límites. En estos casos, debes volver a cualquier intervalo que tenga sentido, que suele ser un subconjunto del intervalo solicitado.

## <a name="related-articles"></a>Artículos relacionados

**Ejemplos**
* [Muestra de Control de edición personalizado](https://go.microsoft.com/fwlink/?linkid=831024) 
 **Muestras de archivo**
* [Muestra de edición de texto XAML](http://go.microsoft.com/fwlink/p/?LinkID=251417)


