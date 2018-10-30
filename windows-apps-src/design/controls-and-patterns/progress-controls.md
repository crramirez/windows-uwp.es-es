---
author: QuinnRadich
Description: A progress control provides feedback to the user that a long-running operation is underway.
title: Directrices sobre los controles de progreso
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 352a4ce0bc37297fb4d53cb5246d18af88f749b7
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5760335"
---
# <a name="progress-controls"></a>Controles de progreso

 

Un control de progreso proporciona información al usuario sobre el hecho de que se está llevando a cabo una operación de ejecución larga. Esto puede significar que el usuario no puede interactuar con la aplicación cuando el indicador de progreso está visible y también puede indicar el tiempo de espera aproximado, según el indicador que usa.

> **API importantes**: [Clase ProgressBar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx), [Propiedad IsIndeterminate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx), [Clase ProgressRing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx), [Propiedad IsActive](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx)

## <a name="types-of-progress"></a>Tipos de progreso

Hay dos controles que muestran al usuario que hay una operación en curso: con una clase ProgressBar o ProgressRing.

-   El estado *determinate* de la clase ProgressBar muestra el porcentaje completado de una tarea. Esto se debe usar durante una operación cuya duración se conoce, pero su progreso no debe bloquear la interacción del usuario con la aplicación.
-   El estado *indeterminate* de la clase ProgressBar muestra que una operación está en curso, no bloquea la interacción del usuario con la aplicación y no se conoce su tiempo de finalización.
-   La clase ProgressRing solo tiene un estado *indeterminate* y se debe usar cuando se bloquee cualquier interacción hasta que finalice la operación.

Además, un control de progreso es de solo lectura y no es interactivo. Esto significa que el usuario no puede invocar o usar estos controles directamente.

![Estados de la clase ProgressBar](images/ProgressBar_TwoStates.png)

*De arriba abajo: clase ProgressBar indeterminada y determinada*

![Estado de la clase ProgressRing](images/ProgressRing_SingleState.png)

*ProgressRing indeterminado*

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para abrir la aplicación y ver <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> o <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>Cuándo usar cada control

No siempre resulta obvio qué control o qué estado (determinado o indeterminado) se debe usar cuando se intenta mostrar que algo está sucediendo. A veces, una tarea es tan obvia que no necesita un control de progreso, y a veces, incluso si se usa un control de progreso, una línea de texto sigue siendo necesaria para explicar al usuario qué operación está en curso.

### <a name="progressbar"></a>ProgressBar
-   **¿El control tiene una duración definida o una finalización predecible?**

    Entonces, usa una clase ProgressBar determinada y actualiza el porcentaje o el valor según corresponda.

-   **¿El usuario puede continuar sin tener que supervisar el progreso de la operación?**

    Cuando se usa una clase ProgressBar, la interacción es no modal, lo que normalmente significa que la finalización de la operación no bloquea al usuario y puede seguir usando la aplicación de otras maneras hasta que haya finalizado ese aspecto.

-   **Palabras clave**

    Si la operación usa estas palabras clave o si aparece texto junto con la operación de progreso que coincida con estas palabras clave, considera la posibilidad de usar una clase ProgressBar:

    - *Cargando...*
    - *Recuperando*
    - *Trabajando...*

### <a name="progressring"></a>ProgressRing

-   **¿La operación hará que el usuario tenga que esperar para continuar?**

    Si una operación necesita que toda la interacción (o una gran parte) con la aplicación espere hasta que se haya completado, la clase ProgressRing es la mejor opción. El control ProgressRing se usa para las interacciones modales, lo que significa que el usuario se bloquea hasta que desaparezca el control ProgressRing.

-   **¿La aplicación espera a que el usuario complete una tarea?**

    Si es así, usa una clase ProgressRing, ya que está pensada para indicar un tiempo de espera desconocido para el usuario.

-   **Palabras clave**

    Si la operación usa estas palabras clave o si aparece texto junto con la operación de progreso que coincida con estas palabras clave; considera la posibilidad de usar una clase ProgressRing:

    - *Actualizando*
    - *Iniciando sesión...*
    - *Conectando...*

### <a name="no-progress-indication-necessary"></a>No es necesaria ninguna indicación de progreso
-   **¿El usuario necesita saber que está ocurriendo algo?**

    Por ejemplo, si la aplicación está descargando algo en segundo plano y el usuario no inició la descarga, no tiene por qué saberlo necesariamente.

-   **¿La operación es una actividad en segundo plano que no bloquea la actividad del usuario y tiene poco interés (aunque lo tenga en parte) para el usuario?**

    Usa texto si la aplicación está realizando tareas que no tienen que estar visibles constantemente, pero necesitas mostrar el estado igualmente.

-   **¿Al usuario solo le interesa la finalización de la operación?**

    A veces, es mejor mostrar un aviso solo cuando la operación se completa o proporcionar inmediatamente un elemento visual cuando la operación haya finalizado y ejecutar los toques finales en segundo plano.

## <a name="progress-controls-best-practices"></a>Procedimientos recomendados de los controles de progreso

A veces, es mejor ver algunas representaciones visuales de cuándo y dónde se usarán los distintos controles de progreso:

**ProgressBar: determinada**

![Ejemplo de clase ProgressBar determinada](images/PB_DeterminateExample.png)

El primer ejemplo es una clase ProgressBar determinada. Cuando se conoce la duración de la operación, cuando se instala, descarga, configura, etc.; es mejor usar una clase ProgressBar determinada.

**ProgressBar: indeterminada**

![Ejemplo de clase ProgressBar indeterminada](images/PB_IndeterminateExample.png)

Cuando se desconoce cuánto tardará la operación, usa una clase ProgressBar indeterminada. Las clases ProgressBar indeterminadas también son útiles cuando se debe rellenar una lista virtualizada y crear una transición visual suave de una clase ProgressBar indeterminada a una determinada.

-   **¿La operación está en una colección virtualizada?**

    Si es así, no coloques un indicador de progreso en cada elemento de lista cuando aparezcan. En su lugar, usa una clase ProgressBar y colócala en la parte superior de la colección de elementos en la que se carga para mostrar que se capturan los elementos.

**ProgressRing: Indeterminado**

![Ejemplo de la clase ProgressRing indeterminada](images/PR_IndeterminateExample.png)

La clase ProgressRing indeterminada se usa cuando cualquier interacción del usuario con la aplicación se detiene o la aplicación espera la entrada del usuario para continuar. El ejemplo "Iniciando sesión…" anterior es un escenario perfecto de la clase ProgressRing, ya que el usuario no puede continuar usando la aplicación hasta que se haya completado el inicio de sesión.

## <a name="customizing-a-progress-control"></a>Personalizar un control de progreso

Ambos controles de progreso son bastante simples, pero algunas características visuales de los controles no son fáciles de personalizar.

**Cambiar el tamaño de la clase ProgressRing**

Se puede cambiar el tamaño de la clase ProgressRing y ampliarlo tanto como quieras, pero su tamaño mínimo es 20x20epx. Para cambiar el tamaño de una clase ProgressRing, debes establecer su alto y ancho. Si solo se establece el alto o el ancho, el control supone que se usa el tamaño mínimo (20x20epx). Por el contrario, si se establecen dos tamaños distintos para el alto y el ancho, se aplica el tamaño más pequeño.
Para garantizar que la clase ProgressRing sea la correcta para tus necesidades, configura tanto el alto como el ancho con el mismo valor:

```XAML
<ProgressRing Height="100" Width="100"/>
```

Para hacer la clase ProgressRing visible y animarla, debes establecer la propiedad IsActive en true:

```XAML
<ProgressRing IsActive="True" Height="100" Width="100"/>
```

```C#
progressRing.IsActive = true;
```

**Colores de los controles de progreso**

De manera predeterminada, se establece el color principal de los controles de progreso en el color de énfasis del sistema. Para invalidar este pincel, simplemente, cambia la propiedad Foreground en uno de los controles.

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<ProgressBar Width="100" Foreground="Green"/>
```

Al cambiar el color del primer plano de la clase ProgressRing, cambiarán los colores de los puntos. La propiedad Foreground de la clase ProgressBar cambiará el color de relleno de la barra. Para modificar la parte de la barra sin rellenar, simplemente, invalida la propiedad Background.

**Mostrar un cursor de espera**

En algunas ocasiones, resulta mejor mostrar solo un breve cursor de espera, cuando la operación o la aplicación necesite tiempo para pensar y tengas que indicar al usuario que no debe interactuar con la aplicación o el área donde el cursor de espera está visible hasta que el cursor de espera haya desaparecido.

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase ProgressBar](https://msdn.microsoft.com/library/windows/apps/br227529)
- [Clase ProgressRing](https://msdn.microsoft.com/library/windows/apps/br227538)

**Para desarrolladores (XAML)**
- [Agregar controles de progreso](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651)
- [Cómo crear una barra de progreso indeterminado personalizada para Windows Phone](http://go.microsoft.com/fwlink/p/?LinkID=392426)
