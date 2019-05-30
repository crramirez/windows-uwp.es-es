---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: La clase SystemMediaTransportControls permite que la aplicación use los controles de transporte de medios del sistema que están integrados en Windows y actualice los metadatos que los controles muestran sobre los elementos multimedia que está reproduciendo actualmente la aplicación.
title: Control manual de los controles de transporte de contenido multimedia del sistema
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fc783d07d8d9bb907c7c23da483d5fbfb8ce63ac
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360691"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>Control manual de los controles de transporte de contenido multimedia del sistema


A partir de Windows 10, versión 1607, las aplicaciones para UWP que usan la clase [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) para reproducir elementos multimedia se integran automáticamente con los controles de transporte de contenido multimedia del sistema (SMTC) de manera predeterminada. Esta es la forma recomendada de interactuar con los SMTC para la mayoría de los escenarios. Para obtener más información sobre cómo personalizar la integración predeterminada de los SMTC con **MediaPlayer**, consulta [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md).

Hay algunos escenarios donde es posible que tengas que implementar el control manual de los SMTC. Por ejemplo, si estás usando una clase [**MediaTimelineController**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaTimelineController) para controlar la reproducción de uno o más reproductores multimedia. O bien, si usas varios reproductores multimedia y solo quieres tener una instancia de SMTC para tu aplicación. Debes controlar manualmente los SMTC si estás usando el control [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) para reproducir archivos multimedia.

## <a name="set-up-transport-controls"></a>Configurar controles de transporte
Si estás usando el control **MediaPlayer** para reproducir elementos multimedia, puedes obtener una instancia de la clase [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) si accedes a la propiedad [**MediaPlayer.SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols). Si controlarás manualmente los SMTC, para deshabilitar la integración automática que proporciona el control **MediaPlayer**, establece la propiedad [**CommandManager.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) en false.

> [!NOTE] 
> Si deshabilitas la clase [**MediaPlaybackCommandManager**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) de [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) al establecer la propiedad [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) en "false", se interrumpirá el vínculo entre **MediaPlayer** y [**TransportControls**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) que proporciona el elemento **MediaPlayerElement**, de modo que los controles de transporte integrados ya no controlarán automáticamente la reproducción del reproductor. En su lugar, debes implementar tus propios controles para controlar la clase **MediaPlayer**.

[!code-cs[InitSMTCMediaPlayer](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaPlayer)]

También puedes obtener una instancia de [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) mediante una llamada a [**GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview). Debes obtener el objeto con este método si estás usando **MediaElement** para reproducir archivos multimedia.

[!code-cs[InitSMTCMediaElement](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaElement)]

Habilita los botones que la aplicación usará, estableciendo la propiedad "is enabled" correspondiente en el objeto **SystemMediaTransportControls**, como [**IsPlayEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled), [**IsPauseEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled), [**IsNextEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.isnextenabled) e [**IsPreviousEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.ispreviousenabled). Consulta la documentación de referencia de **SystemMediaTransportControls** para ver una lista completa de los controles disponibles.

[!code-cs[EnableContols](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetEnableContols)]

Registra un controlador para el evento [**ButtonPressed**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) para recibir notificaciones cuando el usuario presione un botón.

[!code-cs[RegisterButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterButtonPressed)]

## <a name="handle-system-media-transport-controls-button-presses"></a>Pulsaciones de botones de los controles de transporte de medios del sistema

Los controles de transporte del sistema generan el evento [**ButtonPressed**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed) cuando se presiona uno de los botones habilitados. La propiedad [**Botón**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsbuttonpressedeventargs.button) de la [**SystemMediaTransportControlsButtonPressedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControlsButtonPressedEventArgs) que pasó al controlador de eventos es parte de [**SystemMediaTransportControlsButton**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControlsButton) la enumeración que indica cuál de los botones habilitados fue pulsado.

Para actualizar los objetos en el subproceso de la interfaz de usuario desde el controlador de eventos de [**ButtonPressed**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed), como un objeto [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement), deberás calcular las llamadas mediante el [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher). Esto es porque el controlador de eventos **ButtonPressed** no se llama desde el subproceso de interfaz de usuario y, por tanto, se generará una excepción si intentas modificar directamente la interfaz de usuario.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>Actualizar los controles de transporte de medios del sistema con el estado actual de medios

Debes notificar a los [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) cuándo ha cambiado el estado de contenido multimedia para que el sistema pueda actualizar los controles para reflejar el estado actual. Para ello, establece la propiedad [**PlaybackStatus**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackstatus) para el valor [**MediaPlaybackStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaPlaybackStatus) adecuado desde el evento [**CurrentStateChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.currentstatechanged) de la clase [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement), que se genera cuando cambia el estado multimedia.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>Actualizar los controles de transporte de medios del sistema con información multimedia y miniaturas

Usa la clase [**SystemMediaTransportControlsDisplayUpdater**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControlsDisplayUpdater) para actualizar la información multimedia mostrada por los controles de transporte, como el título de la canción o la carátula del álbum del elemento multimedia que se esté reproduciendo. Obtén una instancia de esta clase con la propiedad [**SystemMediaTransportControls.DisplayUpdater**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.displayupdater). Para escenarios típicos, la forma recomendada de pasar los metadatos es llamar a [**CopyFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.copyfromfileasync) pasando por el archivo multimedia que se está reproduciendo. La actualización de pantalla extrae automáticamente los metadatos y la imagen en miniatura del archivo.

Llama a la clase [**Update**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.update) para hacer que los controles de transporte de medios del sistema actualicen la interfaz de usuario con los nuevos metadatos y la miniatura.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

Si el escenario lo requiere, puedes actualizar los metadatos que se muestran mediante los controles de transporte de medios del sistema manualmente, estableciendo los valores de los objetos [**MusicProperties**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.musicproperties), [**ImageProperties**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.imageproperties) o [**VideoProperties**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolsdisplayupdater.videoproperties) que expuso la clase [**DisplayUpdater**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.displayupdater).

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## <a name="update-the-system-media-transport-controls-timeline-properties"></a>Actualizar las propiedades de escala de tiempo de los controles de transporte de medios del sistema

Los controles de transporte del sistema muestran información acerca de la escala de tiempo del elemento multimedia que se está reproduciendo, incluida la posición de reproducción actual, la hora de inicio y la hora de finalización del elemento multimedia. Para actualizar el sistema de propiedades de escala de tiempo de controles de transporte del sistema, crea un nuevo objeto [**SystemMediaTransportControlsTimelineProperties**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControlsTimelineProperties). Establecer las propiedades del objeto para reflejar el estado actual del elemento multimedia que se está reproduciendo. Llama a [**SystemMediaTransportControls.UpdateTimelineProperties**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.) para hacer que los controles actualicen la escala de tiempo.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   Debes proporcionar un valor para los objetos [**StartTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.starttime), [**EndTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.endtime) y [**Posición**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested) para que de los controles del sistema muestren una escala de tiempo para el elemento que se reproduce.

-   [**MinSeekTime** ](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) y [ **MaxSeekTime** ](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime) le permiten especificar el intervalo dentro de la escala de tiempo que el usuario puede buscar. Un escenario típico sobre esto es permitir que los proveedores de contenido incluyan pausas de anuncios en sus elementos multimedia.

    Debes establecer [**MinSeekTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.minseektime) y [**MaxSeekTime**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrolstimelineproperties.maxseektime) para que se genere [**PositionChangeRequest**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackpositionchangerequested).

-   Se recomienda que sincronices los controles del sistema con la reproducción de multimedia mediante la actualización de estas propiedades aproximadamente cada 5 segundos durante la reproducción y de nuevo cada vez que el estado de reproducción cambie, como cuando se pausa o se busca una nueva posición.

## <a name="respond-to-player-property-changes"></a>Responder a los cambios de propiedad del Reproductor

Hay un conjunto de transporte del sistema de propiedades de controles relacionados con el estado actual del Reproductor multimedia en sí, en lugar del estado del elemento multimedia que se reproduce. Cada una de estas propiedades se asocia a un evento que se genera cuando el usuario ajusta el control asociado. Estas propiedades y eventos son:

| Property                                                                  | Evento                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmode) | [**AutoRepeatModeChangeRequested**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.autorepeatmodechangerequested) |
| [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackrate)     | [**PlaybackRateChangeRequested**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested)     |
| [**ShuffleEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabled) | [**ShuffleEnabledChangeRequested**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.shuffleenabledchangerequested) |

 
Para controlar la interacción del usuario con uno de estos controles, registra primero un controlador para el evento asociado.

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

En el controlador del evento, primero asegúrate de que el valor solicitado esté dentro de un intervalo válido y esperado. Si es así, establece la propiedad correspondiente en la clase [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) y, a continuación, establece la propiedad correspondiente en el objeto [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls).

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   Para que uno de estos eventos de propiedad del Reproductor se genere, debes establecer un valor inicial para la propiedad. Por ejemplo, [**PlaybackRateChangeRequested**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackratechangerequested) no se generará hasta que hayas establecido un valor para la propiedad [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.playbackrate) al menos una vez.

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>Usar los controles de transporte de medios del sistema de audio en segundo plano

Si no estás usando la integración automática de SMTC que proporciona **MediaPlayer**, debes integrarla manualmente con los SMTC para habilitar el audio en segundo plano. Como mínimo, la aplicación debe habilitar los botones de reproducción y pausa. Para ello, establece las propiedades [**IsPlayEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.isplayenabled) e [**IsPauseEnabled**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.ispauseenabled) en true. Tu aplicación también debe controlar el evento [**ButtonPressed**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.buttonpressed). Si la aplicación no cumple estos requisitos, se detendrá la reproducción de audio cuando la aplicación pase a segundo plano.

Las aplicaciones que usan el nuevo modelo de un solo proceso de audio en segundo plano deben obtener una instancia de [**SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/Windows.Media.SystemMediaTransportControls) llamando al método [**GetForCurrentView**](https://docs.microsoft.com/uwp/api/windows.media.systemmediatransportcontrols.getforcurrentview). Las aplicaciones que usan el modelo heredado de dos procesos de audio en segundo plano deben usar [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.systemmediatransportcontrols) para obtener acceso a los SMTC desde su proceso en segundo plano.

Para obtener más información sobre la reproducción de audio en segundo plano, consulta [Audio en segundo plano](background-audio.md).

## <a name="related-topics"></a>Temas relacionados
* [Reproducción de multimedia](media-playback.md)
* [Integrar con los medios del sistema de controles de transporte](integrate-with-systemmediatransportcontrols.md) 
* [Ejemplo de transporte de medios del sistema](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 




