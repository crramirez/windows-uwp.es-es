---
Description: El sonido ayuda a completar la experiencia de usuario de una aplicación y ofrece el toque de audio extra que se ajusta a la percepción de Windows en todas las plataformas.
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
ms.openlocfilehash: 6c479a47a53c5f52bab1febf490957355264bfc4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159885"
---
# <a name="sound"></a>Sonido

![imagen principal](images/header-sound.svg)

Hay muchas formas de usar sonido para mejorar tu aplicación. Puedes usar sonido para complementar otros elementos de la interfaz de usuario, lo que permite a los usuarios reconocer eventos de forma audible. El sonido puede ser un elemento de la interfaz de usuario eficaz para personas con discapacidades visuales. Puedes usar sonido para crear una atmósfera envolvente para el usuario; por ejemplo, puedes reproducir una banda sonora divertida en segundo plano en un juego de rompecabezas o usar efectos de sonido inquietantes para un juego de miedo o supervivencia.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Sound">abrir la aplicación y ver Sonido en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="sound-global-api"></a>API global de sonido

La plataforma UWP proporciona un sistema de sonido fácilmente accesible que permite simplemente "invertir un cambio" y obtener una experiencia de sonido envolvente en toda la aplicación.

[**ElementSoundPlayer**](/uwp/api/windows.ui.xaml.elementsoundplayer) es un sistema de sonido integrado dentro de XAML y, cuando está activado en todos los controles predeterminados, se reproducen sonidos automáticamente.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
**ElementSoundPlayer** tiene tres estados distintos: **On** **Off** y **Auto**.

Si se establece en **Off**, independientemente de dónde se ejecute la aplicación, el sonido no se reproducirá nunca. Si se establece en **On**, los sonidos de tu aplicación se reproducirán en todas las plataformas.

Al habilitar ElementSoundPlayer, se habilitará también automáticamente el audio espacial (sonido 3D). Para deshabilitar el sonido 3D (y seguir manteniendo el sonido activado), deshabilita **SpatialAudioMode** del elemento ElementSoundPlayer: 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

La propiedad **SpatialAudioMode** puede aceptar estos valores: 
- **Auto**: el audio espacial se activará cuando el sonido esté activado. 
- **Off**: el audio espacial siempre está desactivado, incluso si el sonido está activado.
- **On**: siempre se reproducirá audio espacial.

Para obtener más información acerca del audio espacial y de cómo XAML lo controla, consulta [AudioGraph: audio espacial](../../audio-video-camera/audio-graphs.md#spatial-audio).

### <a name="sound-for-tv-and-xbox"></a>Sonido para televisión y Xbox

El sonido es una parte fundamental de la experiencia en pantalla de TV y, de manera predeterminada, el estado de **ElementSoundPlayer** es **Auto**, lo que significa que solo obtendrás sonido cuando la aplicación se ejecute en Xbox.
Para obtener más información sobre el diseño para televisión y Xbox, consulta el artículo [Diseño para Xbox y televisión](../devices/designing-for-tv.md).

## <a name="sound-volume-override"></a>Invalidación del volumen del sonido

Todos los sonidos de la aplicación se pueden atenuar con el control **Volumen**. Sin embargo, los sonidos de la aplicación no se pueden reproducir a un volumen *más alto que el volumen del sistema*.

Para establecer el nivel del volumen de la aplicación, llama a:
```C#
ElementSoundPlayer.Volume = 0.5;
```
Donde el volumen máximo (en relación con el volumen del sistema) es 1.0 y el mínimo es 0.0 (esencialmente silencioso).

## <a name="control-level-state"></a>Estado del nivel de control

Si no se desea el sonido predeterminado de un control, se puede deshabilitar. Para ello, se utiliza la propiedad **ElementSoundMode** en el control.

**ElementSoundMode** tiene dos estados: **Desactivado** y **Predeterminado**. Cuando no se establece, es **Default**. Si se establece en **Off**, cada sonido que reproduzca el control se silenciará *excepto para el foco*.

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## <a name="is-this-the-right-sound"></a>¿Es este el sonido adecuado?

Al crear un control personalizado o cambiar el sonido de un control existente, es importante comprender los usos de todos los sonidos que proporciona el sistema.

Cada sonido se relaciona con una determinada interacción de usuario básica y, aunque los sonidos se pueden personalizar para reproducirse en cualquier interacción, esta sección sirve para ilustrar los escenarios en los que se deberían usar sonidos para mantener una experiencia coherente en todas las aplicaciones para UWP.

### <a name="invoking-an-element"></a>Invocar un elemento

Actualmente, el sonido desencadenado por control más común en nuestro sistema es el sonido **Invoke**. Este sonido se reproduce cuando un usuario invoca un control a través de una pulsación, un clic, una entrada o un espacio o si presiona el botón "A" en un controlador para juegos.

Por lo general, este sonido solo se reproduce cuando un usuario selecciona explícitamente un control simple o una parte del control a través de un [dispositivo de entrada](../input/index.md).


Para reproducir este sonido desde cualquier evento de control, solo tienes que llamar al método Play desde **ElementSoundPlayer** y pasar **ElementSound.Invoke**:
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>Mostrar y ocultar el contenido

Hay muchos controles flotantes, cuadros de diálogo e interfaces de usuario descartables en XAML y, cualquier acción que desencadene una de estas superposiciones, debe llamar a un sonido **Show** o **Hide**.

Cuando una ventana de contenido de superposición se incluye en la vista, se debe llamar al sonido **Show**:

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
Por el contrario, cuando se cierra una ventana de contenido de superposición (o se cierra el elemento por cambio de foco), se debe llamar al sonido **Hide**:

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>Navegación dentro de una página

Cuando se navega entre paneles o vistas en la página de una aplicación (consulta [Pestañas y tablas dinámicas](../controls-and-patterns/pivot.md)), normalmente hay un movimiento bidireccional. Esto significa que puedes pasar al siguiente panel o vista, o bien al anterior, sin salir de la página actual de la aplicación en la que te encuentras.

La experiencia de sonido con relación a este concepto de navegación está incluida en los sonidos **MovePrevious** y **MoveNext**.

Al pasar a una vista o un panel que se considere el *siguiente elemento* de una lista, llama a:

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
Y al pasar a una vista o un panel anterior de una lista que se considere el *elemento anterior*, llama a:

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>Navegación hacia atrás

Al navegar de la página actual a la anterior dentro de una aplicación, se debe llamar al sonido **GoBack**:

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>Establecer el foco en un elemento

El sonido **Focus** es el único sonido implícito en nuestro sistema. Esto significa que, aunque un usuario no interactúe directamente con nada, seguirá escuchando un sonido.

El enfoque se produce cuando un usuario navega a través de una aplicación, bien con el controlador para juegos, el teclado, el control remoto o el Kinect. Normalmente, el sonido de **Foco***no se reproduce en eventos PointerEntered o al pasar el mouse por encima*.

Para configurar un control para que reproduzca el sonido **Focus** cuando el control reciba el foco, llama a:

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>Recorrer sonidos Focus

Como una característica adicional para llamar a **ElementSound.Focus**, el sistema de sonido recorrerá cuatro sonidos diferentes en cada desencadenador de navegación. Esto significa que no se reproducirán dos sonidos de foco exactos uno detrás de otro.

La finalidad de esta característica de ciclo consiste en evitar que los sonidos de foco sean monótonos y perceptibles para el usuario. Los sonidos de foco se reproducirán más a menudo y, por lo tanto, deben ser más sutiles.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

* [Diseño para Xbox y televisión](../devices/designing-for-tv.md)
* [Documentación de la clase ElementSoundPlayer](/uwp/api/windows.ui.xaml.elementsoundplayer)