---
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: La clase SystemMediaTransportControls permite que la aplicación use los controles de transporte de medios del sistema que están integrados en Windows y actualice los metadatos que los controles muestran sobre los elementos multimedia que está reproduciendo actualmente la aplicación.
title: Control manual de los controles de transporte de contenido multimedia del sistema
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6be1680d1ce843c1fbe7105dc2027e764095495a
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8737459"
---
# <a name="manual-control-of-the-system-media-transport-controls"></a>Control manual de los controles de transporte de contenido multimedia del sistema


A partir de Windows 10, versión 1607, las aplicaciones para UWP que usan la clase [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) para reproducir elementos multimedia se integran automáticamente con los controles de transporte de contenido multimedia del sistema (SMTC) de manera predeterminada. Esta es la forma recomendada de interactuar con los SMTC para la mayoría de los escenarios. Para obtener más información sobre cómo personalizar la integración predeterminada de los SMTC con **MediaPlayer**, consulta [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md).

Hay algunos escenarios donde es posible que tengas que implementar el control manual de los SMTC. Por ejemplo, si estás usando una clase [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) para controlar la reproducción de uno o más reproductores multimedia. O bien, si usas varios reproductores multimedia y solo quieres tener una instancia de SMTC para tu aplicación. Debes controlar manualmente los SMTC si estás usando el control [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaElement) para reproducir archivos multimedia.

## <a name="set-up-transport-controls"></a>Configurar controles de transporte
Si estás usando el control **MediaPlayer** para reproducir elementos multimedia, puedes obtener una instancia de la clase [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.SystemMediaTransportControls) si accedes a la propiedad [**MediaPlayer.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.SystemMediaTransportControls). Si controlarás manualmente los SMTC, para deshabilitar la integración automática que proporciona el control **MediaPlayer**, establece la propiedad [**CommandManager.IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) en false.

> [!NOTE] 
> Si deshabilitas la clase [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) de [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) al establecer la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) en "false", se interrumpirá el vínculo entre **MediaPlayer** y [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) que proporciona el elemento **MediaPlayerElement**, de modo que los controles de transporte integrados ya no controlarán automáticamente la reproducción del reproductor. En su lugar, debes implementar tus propios controles para controlar la clase **MediaPlayer**.

[!code-cs[InitSMTCMediaPlayer](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaPlayer)]

También puedes obtener una instancia de [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) mediante una llamada a [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708). Debes obtener el objeto con este método si estás usando **MediaElement** para reproducir archivos multimedia.

[!code-cs[InitSMTCMediaElement](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetInitSMTCMediaElement)]

Habilita los botones que la aplicación usará, estableciendo la propiedad "is enabled" correspondiente en el objeto **SystemMediaTransportControls**, como [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714), [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713), [**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) e [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715). Consulta la documentación de referencia de **SystemMediaTransportControls** para ver una lista completa de los controles disponibles.

[!code-cs[EnableContols](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetEnableContols)]

Registra un controlador para el evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) para recibir notificaciones cuando el usuario presione un botón.

[!code-cs[RegisterButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterButtonPressed)]

## <a name="handle-system-media-transport-controls-button-presses"></a>Pulsaciones de botones de los controles de transporte de medios del sistema

Los controles de transporte del sistema generan el evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) cuando se presiona uno de los botones habilitados. La propiedad [**Botón**](https://msdn.microsoft.com/library/windows/apps/dn278685) de la [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683) que pasó al controlador de eventos es parte de [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681) la enumeración que indica cuál de los botones habilitados fue pulsado.

Para actualizar los objetos en el subproceso de la interfaz de usuario desde el controlador de eventos de [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706), como un objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), deberás calcular las llamadas mediante el [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). Esto es porque el controlador de eventos **ButtonPressed** no se llama desde el subproceso de interfaz de usuario y, por tanto, se generará una excepción si intentas modificar directamente la interfaz de usuario.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## <a name="update-the-system-media-transport-controls-with-the-current-media-status"></a>Actualizar los controles de transporte de medios del sistema con el estado actual de medios

Debes notificar a los [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) cuándo ha cambiado el estado de contenido multimedia para que el sistema pueda actualizar los controles para reflejar el estado actual. Para ello, establece la propiedad [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719) para el valor [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665) adecuado desde el evento [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) de la clase [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), que se genera cuando cambia el estado multimedia.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## <a name="update-the-system-media-transport-controls-with-media-info-and-thumbnails"></a>Actualizar los controles de transporte de medios del sistema con información multimedia y miniaturas

Usa la clase [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686) para actualizar la información multimedia mostrada por los controles de transporte, como el título de la canción o la carátula del álbum del elemento multimedia que se esté reproduciendo. Obtén una instancia de esta clase con la propiedad [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707). Para escenarios típicos, la forma recomendada de pasar los metadatos es llamar a [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694) pasando por el archivo multimedia que se está reproduciendo. La actualización de pantalla extrae automáticamente los metadatos y la imagen en miniatura del archivo.

Llama a la clase [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701) para hacer que los controles de transporte de medios del sistema actualicen la interfaz de usuario con los nuevos metadatos y la miniatura.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

Si el escenario lo requiere, puedes actualizar los metadatos que se muestran mediante los controles de transporte de medios del sistema manualmente, estableciendo los valores de los objetos [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696), [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695) o [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702) que expuso la clase [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707).

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## <a name="update-the-system-media-transport-controls-timeline-properties"></a>Actualizar las propiedades de escala de tiempo de los controles de transporte de medios del sistema

Los controles de transporte del sistema muestran información acerca de la escala de tiempo del elemento multimedia que se está reproduciendo, incluida la posición de reproducción actual, la hora de inicio y la hora de finalización del elemento multimedia. Para actualizar el sistema de propiedades de escala de tiempo de controles de transporte del sistema, crea un nuevo objeto [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746). Establecer las propiedades del objeto para reflejar el estado actual del elemento multimedia que se está reproduciendo. Llama a [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760) para hacer que los controles actualicen la escala de tiempo.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   Debes proporcionar un valor para los objetos [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751), [**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747) y [**Posición**](https://msdn.microsoft.com/library/windows/apps/mt218755) para que de los controles del sistema muestren una escala de tiempo para el elemento que se reproduce.

-   [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) y [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) te permiten especificar el intervalo de tiempo dentro del cual el usuario puede buscar contenidos. Un escenario típico sobre esto es permitir que los proveedores de contenido incluyan pausas de anuncios en sus elementos multimedia.

    Debes establecer [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) y [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) para que se genere [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755).

-   Se recomienda que sincronices los controles del sistema con la reproducción de multimedia mediante la actualización de estas propiedades aproximadamente cada 5 segundos durante la reproducción y de nuevo cada vez que el estado de reproducción cambie, como cuando se pausa o se busca una nueva posición.

## <a name="respond-to-player-property-changes"></a>Responder a los cambios de propiedad del Reproductor

Hay un conjunto de transporte del sistema de propiedades de controles relacionados con el estado actual del Reproductor multimedia en sí, en lugar del estado del elemento multimedia que se reproduce. Cada una de estas propiedades se asocia a un evento que se genera cuando el usuario ajusta el control asociado. Estas propiedades y eventos son:

| Propiedad                                                                  | Evento                                                                                                   |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| [**AutoRepeatMode**](https://msdn.microsoft.com/library/windows/apps/mt218753) | [**AutoRepeatModeChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218754) |
| [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756)     | [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757)     |
| [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt218758) | [**ShuffleEnabledChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218759) |

 
Para controlar la interacción del usuario con uno de estos controles, registra primero un controlador para el evento asociado.

[!code-cs[RegisterPlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetRegisterPlaybackChangedHandler)]

En el controlador del evento, primero asegúrate de que el valor solicitado esté dentro de un intervalo válido y esperado. Si es así, establece la propiedad correspondiente en la clase [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) y, a continuación, establece la propiedad correspondiente en el objeto [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677).

[!code-cs[PlaybackChangedHandler](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetPlaybackChangedHandler)]

-   Para que uno de estos eventos de propiedad del Reproductor se genere, debes establecer un valor inicial para la propiedad. Por ejemplo, [**PlaybackRateChangeRequested**](https://msdn.microsoft.com/library/windows/apps/mt218757) no se generará hasta que hayas establecido un valor para la propiedad [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/mt218756) al menos una vez.

## <a name="use-the-system-media-transport-controls-for-background-audio"></a>Usar los controles de transporte de medios del sistema de audio en segundo plano

Si no estás usando la integración automática de SMTC que proporciona **MediaPlayer**, debes integrarla manualmente con los SMTC para habilitar el audio en segundo plano. Como mínimo, la aplicación debe habilitar los botones de reproducción y pausa. Para ello, establece las propiedades [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) e [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713) en true. Tu aplicación también debe controlar el evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706). Si la aplicación no cumple estos requisitos, se detendrá la reproducción de audio cuando la aplicación pase a segundo plano.

Las aplicaciones que usan el nuevo modelo de un solo proceso de audio en segundo plano deben obtener una instancia de [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) llamando al método [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708). Las aplicaciones que usan el modelo heredado de dos procesos de audio en segundo plano deben usar [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) para obtener acceso a los SMTC desde su proceso en segundo plano.

Para obtener más información sobre la reproducción de audio en segundo plano, consulta [Audio en segundo plano](background-audio.md).

## <a name="related-topics"></a>Temas relacionados
* [Reproducción de contenido multimedia](media-playback.md)
* [Integrar con los controles de transporte de contenido multimedia del sistema](integrate-with-systemmediatransportcontrols.md) 
* [Muestra de transporte de contenido multimedia del sistema](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls) 

 




