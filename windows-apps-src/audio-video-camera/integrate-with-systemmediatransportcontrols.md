---
ms.assetid: eb690f2b-3bf8-4a65-99a4-2a3a8c7760b7
description: En este artículo se muestra cómo interactuar con los controles de transporte multimedia del sistema.
title: Integrar con los controles de transporte de contenido multimedia del sistema
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c89a1901d15d00c7c102157c8f44d6ab96272ef0
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8686440"
---
# <a name="integrate-with-the-system-media-transport-controls"></a>Integrar con los controles de transporte de contenido multimedia del sistema

En este artículo se muestra cómo interactuar con los controles de transporte multimedia del sistema (SMTC). Los SMTC son un conjunto de controles comunes para todos los dispositivos de Windows10 que proporcionan un modo coherente para que los usuarios controlen la reproducción multimedia de todas las aplicaciones en ejecución que usan [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) para la reproducción.

Para obtener una muestra completa que demuestre la integración con los SMTC, consulta la [muestra de controles de transporte multimedia del sistema en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls).
                    
## <a name="automatic-integration-with-smtc"></a>Integración automática con los SMTC
A partir de Windows 10, versión 1607, las aplicaciones para UWP que usan la clase [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) para reproducir contenido multimedia se integran automáticamente con los SMTC de manera predeterminada. Solo tienes que crear una nueva instancia de **MediaPlayer** y asignar un objeto [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) o [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList) a la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) del reproductor. El usuario verá el nombre de tu aplicación en los SMTC y podrá reproducir, pausar y recorrer tus listas de reproducción mediante los controles SMTC. 

La aplicación puede crear y usar varios objetos **MediaPlayer** a la vez. Para cada instancia de **MediaPlayer** activa en la aplicación, se crea una pestaña independiente en los SMTC que permite al usuario cambiar entre los reproductores multimedia activos y los de otras aplicaciones en ejecución. El reproductor multimedia seleccionado actualmente en los SMTC es el que se verá afectado por los controles.

Para obtener más información sobre el uso de **MediaPlayer** en tu aplicación, como el enlace a un objeto [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) en la página XAML, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

Para obtener más información sobre el uso de **MediaSource**, **MediaPlaybackItem** y **MediaPlaybackList**, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

## <a name="add-metadata-to-be-displayed-by-the-smtc"></a>Agregar los metadatos que deben mostrar los SMTC
Si quieres agregar o modificar los metadatos que se muestran para los elementos multimedia en los SMTC, como el título de un vídeo o una canción, deberás actualizar las propiedades de visualización del objeto **MediaPlaybackItem** que representa el elemento multimedia. Primero, obtén una referencia al objeto [**MediaItemDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties) llamando a [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties). Después, establece el tipo de contenido multimedia, música o vídeo, del elemento con la propiedad [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type). A continuación, puedes rellenar los campos de [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) o [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties), según el tipo de contenido multimedia especificado. Por último, actualiza los metadatos del elemento multimedia mediante una llamada a [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923).

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="use-commandmanager-to-modify-or-override-the-default-smtc-commands"></a>Usar CommandManager para modificar o invalidar los comandos de SMTC predeterminados
La aplicación puede modificar o invalidar completamente el comportamiento de los controles SMTC con la clase [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager). Se puede obtener una instancia del administrador de comandos para cada instancia de la clase **MediaPlayer** mediante el acceso a la propiedad [**CommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.CommandManager).

Para cada comando, como el comando *Next* (que salta al siguiente elemento de forma predeterminada en un objeto **MediaPlaybackList**), el administrador de comandos expone un evento recibido, como [**NextReceived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.NextReceived) y un objeto que administra el comportamiento del comando, como [**NextBehavior**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.NextBehavior). 

El siguiente ejemplo registra un controlador para el evento **NextReceived** y para el evento [**IsEnabledChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabledChanged) de **NextBehavior**.

[!code-cs[AddNextHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddNextHandler)]

En el ejemplo siguiente se ilustra un escenario en que la aplicación quiere deshabilitar el comando *Next* cuando el usuario haya hecho clic en cinco elementos de la lista de reproducción y quizás exigir alguna interacción del usuario antes de seguir reproduciendo contenido. Cada ## que se genera el evento **NextReceived**, se incrementa un contador. Cuando el contador alcanza el número de destino, [**EnablingRule**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.EnablingRule) del comando *Next* se establece en [**Never**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaCommandEnablingRule), con lo que se deshabilita el comando. 

[!code-cs[NextReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetNextReceived)]

También puedes establecer el comando en **Always** para que siempre esté habilitado, incluso si, para el ejemplo del comando *Next*, no hay más elementos en la lista de reproducción. También puedes establecer el comando en **Auto** para que el sistema determine si el comando debe habilitarse en función del contenido que se reproduce.

Para el escenario descrito anteriormente, la aplicación querrá, en algún momento, volver a habilitar el comando *Next*. Para hacerlo, deberá establecer **EnablingRule** en **Auto**.

[!code-cs[EnableNextButton](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetEnableNextButton)]

Dado que la aplicación puede tener su propia interfaz de usuario para controlar la reproducción mientras está en primer plano, puedes usar los eventos [**IsEnabledChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabledChanged) para actualizar tu propia interfaz de usuario de modo que coincida con los SMTC a medida que se habilitan o deshabilitan los comandos mediante el acceso a [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior.IsEnabled) de la clase [**MediaPlaybackCommandManagerCommandBehavior**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerCommandBehavior) pasada al controlador.

[!code-cs[IsEnabledChanged](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetIsEnabledChanged)]

En algunos casos, es posible que quieras invalidar completamente el comportamiento de un comando de SMTC. En el siguiente ejemplo, se muestra un escenario donde una aplicación usa los comandos *Next* y *Previous* para cambiar entre las emisoras de radio por Internet, en lugar de saltar entre las pistas de la lista de reproducción actual. Al igual que en el ejemplo anterior, se registra un controlador cuando se recibe un comando, que en este caso es el evento [**PreviousReceived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.PreviousReceived).

[!code-cs[AddPreviousHandler](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetAddPreviousHandler)]

En el controlador **PreviousReceived**, primero se obtiene una clase [**Deferral**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Deferral) mediante una llamada a [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs.GetDeferral) de la clase [**MediaPlaybackCommandManagerPreviousReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs) pasada al controlador. Esto indica al sistema que espere hasta que se complete el aplazamiento antes de ejecutar el comando. Esto es muy importante si vas a hacer llamadas asincrónicas en el controlador. En este punto, el ejemplo llama a un método personalizado que devuelve una clase **MediaPlaybackItem** que representa la emisora de radio anterior.

A continuación, se comprueba la propiedad [**Handled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManagerPreviousReceivedEventArgs.Handled) para garantizar que no había otro controlador que ya estaba controlando el evento. Si no es así, la propiedad **Handled** se establece en true. De este modo, los SMTC y cualquier otro controlador suscrito sabrán que no deben realizar ninguna acción para ejecutar este comando porque ya se controla. A continuación, el código establece el nuevo origen del reproductor multimedia e inicia el reproductor.

Por último, se llama a [**Complete**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Deferral.Complete) en el objeto de aplazamiento para que el sistema sepa que terminaste de procesar el comando.

[!code-cs[PreviousReceived](./code/SMTC_RS1/cs/MainPage.xaml.cs#SnippetPreviousReceived)]
                
## <a name="manual-control-of-the-smtc"></a>Control manual de los SMTC
Como se mencionó anteriormente en este artículo, los SMTC detectarán y mostrarán automáticamente la información de todas las instancias de **MediaPlayer** que cree la aplicación. Si quieres usar varias instancias de **MediaPlayer**, pero quieres que los SMTC proporcionen una sola entrada para la aplicación, debes controlar manualmente el comportamiento de los SMTC en lugar de usar la integración automática. Además, si usas [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) para controlar uno o más reproductores multimedia, debes usar la integración de SMTC manual. Asimismo, si la aplicación usa una API distinta de **MediaPlayer**, como la clase [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioGraph), para reproducir contenido multimedia, debes implementar la integración de SMTC manual para que el usuario emplee los SMTC para controlar la aplicación. Para obtener información sobre cómo controlar manualmente los SMTC, consulta [Control manual de los controles de transporte de multimedia del sistema](system-media-transport-controls.md).



## <a name="related-topics"></a>Temas relacionados
* [Reproducción de contenido multimedia](media-playback.md)
* [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Control manual de los controles de transporte de multimedia del sistema](system-media-transport-controls.md)
* [Muestra de controles de transporte multimedia del sistema en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)
 

 




