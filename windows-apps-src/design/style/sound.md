---
Description: Sound helps complete an application's user experience, and gives them that extra audio edge they need to match the feel of Windows across all platforms.
label: Sound
title: Sonido
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: mattben
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 74d1d5b04b13795a075e7111ed898243ed59e7b7
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7976244"
---
# <a name="sound"></a>Sonido

![imagen principal](images/header-sound.svg)

Hay muchas formas de usar sonido para mejorar tu aplicación. Puedes usar sonido para complementar otros elementos de interfaz de usuario, esto permitirá que los usuarios puedan reconocer eventos de forma audible. El sonido puede ser un elemento de interfaz de usuario eficaz para personas con discapacidades visuales. Puedes usar sonido para crear una atmósfera que sumerge al usuario; por ejemplo, puedes reproducir una banda sonora divertida en segundo plano en un puzle o usar efectos de sonido inquietantes para un juego de miedo o supervivencia.

## <a name="sound-global-api"></a>API global de sonido

UWP proporciona un sistema de sonido fácilmente accesible que te permite simplemente "invertir un cambio" y obtener una experiencia de sonido envolvente en toda la aplicación.

La clase [**ElementSoundPlayer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer) es un sistema de sonido integrado dentro de XAML y, cuando está activada en todos los controles predeterminados, se reproducen sonidos automáticamente.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
La **ElementSoundPlayer** tiene tres estados distintos: **Activado** **Desactivado** y **Automático**.

Si se establece en **Desactivado**, independientemente de dónde se ejecute la aplicación, el sonido no se reproducirá nunca. Si se establece en **Activado**, los sonidos de tu aplicación se reproducirán en todas las plataformas.

Al habilitar ElementSoundPlayer, se habilitará también automáticamente el audio espacial (sonido 3D). Para deshabilitar el sonido 3D (y seguir manteniendo el sonido activado), deshabilita el **SpatialAudioMode** de la clase ElementSoundPlayer: 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

La propiedad **SpatialAudioMode** puede aceptar estos valores: 
- **Automático**: el audio espacial se activará cuando el sonido esté activado. 
- **Desactivado**: el audio espacial siempre está desactivado, incluso si el sonido está activado.
- **Activado**: siempre se reproducirá audio espacial.

Para obtener más información acerca del audio espacial y de cómo XAML lo controla, consulta [AudioGraph: audio espacial](/windows/uwp/audio-video-camera/audio-graphs#spatial-audio).

### <a name="sound-for-tv-and-xbox"></a>Sonido para televisión y Xbox

El sonido es una parte fundamental de la experiencia de 10 pies y, de manera predeterminada, el estado de **ElementSoundPlayer** es **Automático**, lo que significa que solo obtendrás sonido cuando la aplicación se ejecute en Xbox.
Para obtener más información sobre el diseño para televisión o Xbox, consulta el artículo [Diseño para Xbox y televisión](http://go.microsoft.com/fwlink/?LinkId=760736).

## <a name="sound-volume-override"></a>Reemplazo del volumen del sonido

Todos los sonidos de la aplicación se pueden atenuar con el control **Volumen**. Sin embargo, los sonidos de la aplicación no se pueden reproducir a un volumen *más alto que el volumen del sistema*.

Para establecer el nivel del volumen de la aplicación, llama a:
```C#
ElementSoundPlayer.Volume = 0.5;
```
Donde el volumen máximo (en relación con el volumen del sistema) es 1.0 y el mínimo 0.0 (esencialmente silencioso).

## <a name="control-level-state"></a>Estado del nivel de control

Si no se desea un sonido predeterminado de un control, se puede deshabilitar. Esto se realiza mediante el **ElementSoundMode** en el control.

El **ElementSoundMode** tiene dos estados: **Desactivado** y **Predeterminado**. Cuando no se establece, es **Predeterminado**. Si se establece en **Desactivado**, cada sonido que reproduzca el control se silenciará *excepto para foco*.

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## <a name="is-this-the-right-sound"></a>¿Es este el control adecuado?

Al crear un control personalizado o al cambiar un sonido existente del control, es importante comprender los usos de todos los sonidos que proporciona el sistema.

Cada sonido se relaciona con una determinada interacción de usuario básica y, aunque los sonidos se pueden personalizar para reproducir en cualquier interacción, esta sección sirve para ilustrar los casos en los que se deberían usar los sonidos para mantener una experiencia coherente en todas las aplicaciones para UWP.

### <a name="invoking-an-element"></a>Invocar un elemento

Actualmente, el sonido activado por el control más común en nuestro sistema es el sonido **Invocar**. Este sonido se reproduce cuando un usuario invoca un control a través de una pulsación, un clic, una introducción o un espacio o si presiona el botón "A" en un mando de juegos.

Por lo general, este sonido solo se reproduce cuando un usuario destina explícitamente un control simple o una parte del control a través de un [dispositivo de entrada](../input/index.md).

<clic de sonido SelectButtonClick.mp3 aquí>

Para reproducir este sonido en cualquier evento de control, solo tienes que llamar al método de reproducción de **ElementSoundPlayer** y pasar **ElementSound.Invoke**:
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>Mostrar y ocultar el contenido

Hay muchos controles flotantes, cuadros de diálogo e interfaces de usuario descartables en XAML y, cualquier acción que active una de estas superposiciones, debe llamar a un sonido **Mostrar** u **Ocultar**.

Cuando una ventana de contenido de superposición se incluye en la vista, debe llamarse al sonido **Mostrar**:

<clip de sonido OverlayIn.mp3 aquí>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
Por el contrario, cuando se cierra una ventana de contenido de superposición (o se cierra el elemento por cambio de foco), debe llamarse al sonido **Ocultar**:

<clip de sonido OverlayOut.mp3 aquí>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>Navegación dentro de una página

Cuando se navega entre paneles o vistas en la página de una aplicación (consulta [Hub](../controls-and-patterns/hub.md) o [Pestañas y tablas dinámicas](../controls-and-patterns/tabs-pivot.md)), normalmente hay un movimiento bidireccional. Esto significa que puedes pasar al siguiente panel o vista, o bien al anterior, sin salir de la página actual de la aplicación en la que te encuentras.

La experiencia de sonido con relación a este concepto de navegación está incluida en los sonidos **MovePrevious** y **MoveNext**.

Al pasar a un vista o panel que se considera el *siguiente elemento* de una lista, llama a:

<clip de sonido PageTransitionRight.mp3 aquí>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
Y al pasar a un vista o panel anterior de una lista que se considera el *elemento anterior*, llama a:

<clip de sonido PageTransitionLeft.mp3 aquí>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>Volver a la navegación

Al navegar desde la página actual a la página anterior dentro de una aplicación, debe llamarse al sonido **GoBack**:

<clip de sonido BackButtonClick.mp3 aquí>

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>Centrarse en un elemento

El sonido de **Foco** es el único sonido implícito en nuestro sistema. Esto significa que, aunque un usuario no interactúe directamente con nada, seguirá escuchando un sonido.

El enfoque ocurre cuando un usuario navega a través de una aplicación, bien con el mando de juegos, el teclado, el control remoto o el kinect. Normalmente, el sonido de **Foco** *no se reproduce en eventos PointerEntered o al pasar el mouse por encima*.

Para configurar un control para que reproduzca el sonido de **Foco** cuando el control recibe el foco, llama a:

<clip de sonido ElementFocus1.mp3 aquí>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>Recorrer sonidos de foco

Como una característica agregada para llamar a **ElementSound.Focus**, el sistema de sonido, de manera predeterminada, recorrerá 4 sonidos diferentes en cada desencadenador de navegación. Esto significa que no se reproducirán dos sonidos de foco exactos uno detrás de otro.

La finalidad de esta característica de recorrido consiste en evitar que los sonidos de foco sean monótonos y que los note el usuario. Los sonidos de foco se reproducirán más a menudo y, por lo tanto, deben ser más sutiles.

## <a name="related-articles"></a>Artículos relacionados

* [Diseño para Xbox y televisión](http://go.microsoft.com/fwlink/?LinkId=760736)
* [Documentación de la clase ElementSoundPlayer](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.elementsoundplayer)
