---
author: drewbatgit
ms.assetid: EFCF84D0-2F4C-454D-97DA-249E9EAA806C
description: La clase SystemMediaTransportControls permite que la aplicación use los controles de transporte de medios del sistema que están integrados en Windows, y actualice los metadatos que los controles muestran sobre los elementos multimedia que está reproduciendo actualmente la aplicación.
title: Controles de transporte de contenido multimedia del sistema
---

# Controles de transporte de contenido multimedia del sistema

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


La clase [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) permite que la aplicación use los controles de transporte de contenido multimedia del sistema que están integrados en Windows y actualice los metadatos que los controles muestran sobre los elementos multimedia que está reproduciendo actualmente la aplicación.

Los controles de transporte del sistema son distintos que los controles de transporte del objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926). Los controles de transporte del sistema son los controles que aparecen cuando se presionan teclas multimedia de hardware, como el control de volumen en unos auriculares o los botones multimedia de los teclados. Si el usuario presiona la tecla de pausa en un teclado y tu aplicación admite [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677), tu aplicación recibe una notificación y realiza la acción apropiada.

La aplicación también puede actualizar la información multimedia, como el título de la canción y la imagen de miniatura que muestran los [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677).

**Nota**  
La [muestra controles de transporte de contenido multimedia del sistema UWP](http://go.microsoft.com/fwlink/?LinkId=619488) implementa el código tratado en esta introducción. Puedes descargar la muestra para ver el código en contexto o para usarla como punto de partida para tu propia aplicación.

## Configurar controles de transporte

En el archivo XAML de la página, define un [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) que se controlará mediante los controles de transporte de contenido multimedia del sistema. Los eventos [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) y [**MediaOpened**](https://msdn.microsoft.com/library/windows/apps/br227394) se usan para actualizar los controles de transporte de contenido multimedia del sistema y se explicarán más adelante en este artículo.

[!code-xml[MediaElementSystemMediaTransportControls](./code/SMTCWin10/cs/MainPage.xaml#SnippetMediaElementSystemMediaTransportControls)]

Agrega un botón al archivo XAML que permita al usuario seleccionar un archivo para reproducir.

[!code-xml[OpenButton](./code/SMTCWin10/cs/MainPage.xaml#SnippetOpenButton)]

En la página de código subyacente, agrega directivas de uso para los siguientes espacios de nombres.

[!code-cs[Espacio de nombres](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetNamespace)]

Agrega un botón controlador que use la clase [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para permitir al usuario seleccionar un archivo; a continuación, llama a [**SetSource**](https://msdn.microsoft.com/library/windows/apps/br244338) para que sea el archivo activo de **MediaElement**.

[!code-cs[OpenMediaFile](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetOpenMediaFile)]

Obtén una instancia de la clase [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) llamando a [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708).

Habilita los botones que la aplicación usará, estableciendo la propiedad "is enabled" correspondiente en el objeto **SystemMediaTransportControls**, como [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714), [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713), [**IsNextEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278712) y [**IsPreviousEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278715). Consulta la documentación de referencia de **SystemMediaTransportControls** para ver una lista completa de los controles disponibles.

Registra un controlador para el evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706), y así recibir notificaciones cuando el usuario presione un botón.

[!code-cs[SystemMediaTransportControlsSetup](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsSetup)]

## Pulsaciones de botones de los controles de transporte de medios del sistema

Los controles de transporte del sistema generan el evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706) cuando se presiona uno de los botones habilitados. La propiedad [**Botón**](https://msdn.microsoft.com/library/windows/apps/dn278685) de la [**SystemMediaTransportControlsButtonPressedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn278683) que pasó al controlador de eventos es parte de [**SystemMediaTransportControlsButton**](https://msdn.microsoft.com/library/windows/apps/dn278681) la enumeración que indica cuál de los botones habilitados fue pulsado.

Para actualizar los objetos en el subproceso de la interfaz de usuario desde el controlador de eventos de [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706), como un objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), deberás calcular las llamadas mediante el [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). Esto es porque el controlador de eventos **ButtonPressed** no se llama desde el subproceso de interfaz de usuario y, por tanto, se generará una excepción si intentas modificar directamente la interfaz de usuario.

[!code-cs[SystemMediaTransportControlsButtonPressed](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsButtonPressed)]

## Actualizar los controles de transporte de medios del sistema con el estado actual de medios

Debes notificar a los [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) cuándo ha cambiado el estado de contenido multimedia para que el sistema pueda actualizar los controles para reflejar el estado actual. Para ello, establece la propiedad [**PlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278719) para el valor [**MediaPlaybackStatus**](https://msdn.microsoft.com/library/windows/apps/dn278665) adecuado desde el evento [**CurrentStateChanged**](https://msdn.microsoft.com/library/windows/apps/br227375) de la clase [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), que se genera cuando cambia el estado multimedia.

[!code-cs[SystemMediaTransportControlsStateChange](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsStateChange)]

## Actualizar los controles de transporte de medios del sistema con información multimedia y miniaturas

Usa la clase [**SystemMediaTransportControlsDisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278686) para actualizar la información multimedia mostrada por los controles de transporte, como el título de la canción o la carátula del álbum del elemento multimedia que se esté reproduciendo. Obtén una instancia de esta clase con la propiedad [**SystemMediaTransportControls.DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707). Para escenarios típicos, la forma recomendada de pasar los metadatos es llamar a [**CopyFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn278694) pasando por el archivo multimedia que se está reproduciendo. La actualización de pantalla extrae automáticamente los metadatos y la imagen en miniatura del archivo.

Llama a la clase [**Update**](https://msdn.microsoft.com/library/windows/apps/dn278701) para hacer que los controles de transporte de medios del sistema actualicen la interfaz de usuario con los nuevos metadatos y la miniatura.

[!code-cs[SystemMediaTransportControlsUpdater](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetSystemMediaTransportControlsUpdater)]

Si el escenario lo requiere, puedes actualizar los metadatos que se muestran mediante los controles de transporte de medios del sistema manualmente, estableciendo los valores de los objetos [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/dn278696), [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/dn278695) o [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/dn278702) que expuso la clase [**DisplayUpdater**](https://msdn.microsoft.com/library/windows/apps/dn278707).

[!code-cs[SystemMediaTransportControlsUpdaterManual](./code/SMTCWin10/cs/MainPage.xaml.cs#SystemMediaTransportControlsUpdaterManual)]

## Actualizar las propiedades de escala de tiempo de los controles de transporte de medios del sistema

Los controles de transporte del sistema muestran información acerca de la escala de tiempo del elemento multimedia que se está reproduciendo, incluida la posición de reproducción actual, la hora de inicio y la hora de finalización del elemento multimedia. Para actualizar el sistema de propiedades de escala de tiempo de controles de transporte del sistema, crea un nuevo objeto [**SystemMediaTransportControlsTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218746). Establecer las propiedades del objeto para reflejar el estado actual del elemento multimedia que se está reproduciendo. Llama a [**SystemMediaTransportControls.UpdateTimelineProperties**](https://msdn.microsoft.com/library/windows/apps/mt218760) para hacer que los controles actualicen la escala de tiempo.

[!code-cs[UpdateTimelineProperties](./code/SMTCWin10/cs/MainPage.xaml.cs#SnippetUpdateTimelineProperties)]

-   Debes proporcionar un valor para los objetos [**StartTime**](https://msdn.microsoft.com/library/windows/apps/mt218751), [**EndTime**](https://msdn.microsoft.com/library/windows/apps/mt218747) y [**Posición**](https://msdn.microsoft.com/library/windows/apps/mt218755) para que de los controles del sistema muestren una escala de tiempo para el elemento que se reproduce.

-   [
              **MinSeekTime**
            ](https://msdn.microsoft.com/library/windows/apps/mt218749) y [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) te permiten especificar el intervalo de tiempo dentro del cual el usuario puede buscar contenidos. Un escenario típico sobre esto es permitir que los proveedores de contenido incluyan pausas de anuncios en sus elementos multimedia.

    Debes establecer [**MinSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218749) y [**MaxSeekTime**](https://msdn.microsoft.com/library/windows/apps/mt218748) para que se genere [**PositionChangeRequest**](https://msdn.microsoft.com/library/windows/apps/mt218755).

-   Se recomienda que sincronices los controles del sistema con la reproducción de multimedia mediante la actualización de estas propiedades aproximadamente cada 5 segundos durante la reproducción y de nuevo cada vez que el estado de reproducción cambie, como cuando se pausa o se busca una nueva posición.

## Responder a los cambios de propiedad del Reproductor

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

## Usar los controles de transporte de medios del sistema de audio en segundo plano

Para usar los controles de transporte de contenido multimedia del sistema de audio en segundo plano, debes habilitar la reproducción y pausar los botones mediante la configuración [**IsPlayEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278714) y [**IsPauseEnabled**](https://msdn.microsoft.com/library/windows/apps/dn278713) en true. Tu aplicación también debe controlar el evento [**ButtonPressed**](https://msdn.microsoft.com/library/windows/apps/dn278706).

Para obtener una instancia de [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) desde dentro de la tarea en segundo plano de la aplicación, debes usar [**BackgroundMediaPlayer.Current.SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn926635) en lugar de [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/dn278708), que pueden usarse solamente desde dentro de la aplicación en primer plano.

Para obtener más información sobre la reproducción de audio en segundo plano, consulta [Audio en segundo plano](background-audio.md).

 

 






<!--HONumber=May16_HO2-->


