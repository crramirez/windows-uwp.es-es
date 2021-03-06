---
title: Audio para juegos
description: Aprende a desarrollar e incorporar música y sonidos en un juego DirectX, y a procesar las señales de audio para crear sonidos dinámicos y posicionales.
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, audio, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 5601a3fc2c4aa1d0aa39295a220bd04c6156343f
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216558"
---
# <a name="audio-for-games"></a>Audio para juegos

Aprende a desarrollar e incorporar música y sonidos en un juego DirectX, y a procesar las señales de audio para crear sonidos dinámicos y posicionales.

Para la programación de audio, se recomienda usar la biblioteca de [XAudio2](/windows/win32/xaudio2/xaudio2-apis-portal) en DirectX o las API de [gráficos de audio](../audio-video-camera/audio-graphs.md) de Windows Runtime. Aquí usamos XAudio2. XAudio2 es una biblioteca de audio de bajo nivel que proporciona una base para el procesamiento y la mezcla de señales para juegos, y que es compatible con varios formatos.

También puedes implementar una reproducción sencilla de sonidos y música con [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk). Microsoft Media Foundation está diseñado para reproducir archivos multimedia y secuencias, tanto de audio como de vídeo, pero también se puede usar en juegos, y es particularmente útil para escenas cinematográficas o componentes no interactivos de tu juego.

## <a name="concepts-at-a-glance"></a>Conceptos de un vistazo

Aquí presentamos unos pocos conceptos de programación de audio que usaremos en esta sección.

-   Las señales son la unidad básica de programación de sonido, análogas a los píxeles en los gráficos. Los procesadores digitales de señales (DSP) que las procesan son los equivalentes de sombreadores de píxeles en el audio para juegos. Pueden transformar señales, combinarlas o filtrarlas. Al programar para los DSP, es posible alterar los efectos sonoros y la música de tu juego con el grado de complejidad que necesites.
-   Las voces son composiciones submezcladas de dos o más señales. Hay 3 tipos de objetos de voz XAudio2: voces de origen, de submezcla y de procesamiento. Las voces de origen operan en datos de audio proporcionados por el cliente. Las voces de origen y de submezcla envían su resultado a una o más voces de submezcla o de procesamiento. Las voces de submezcla y de procesamiento mezclan el audio de todas las voces que les alimentan y trabajan con el resultado. Las voces de procesamiento escriben datos de audio en un dispositivo de audio.
-   Mezclar es el proceso de combinar varias voces discretas, como los efectos sonoros y el audio de fondo que se reproducen en una escena, en una sola secuencia. Submezclar es el proceso de combinar varias señales discretas, como los sonidos de componente del ruido de un motor, y crear una voz.
-   Formatos de audio. La música y los efectos de sonido pueden almacenarse en diferentes formatos digitales para tu juego. Existen formatos no comprimidos, como WAV, y formatos comprimidos como MP3 y OGG. Cuanto más comprimida esté una muestra (normalmente designada por su velocidad de bits; a menor velocidad de bits, más pérdidas de información tendrá la compresión), peor fidelidad tendrá. La fidelidad puede variar entre esquemas de compresión y velocidades de bits, así que experimenta con ellas para averiguar qué es lo que funciona mejor para tu juego.
-   Velocidad de muestra y calidad. Los sonidos se pueden muestrear a diferentes velocidades, y los sonidos muestreados a una velocidad menor tienen una fidelidad mucho más pobre. La velocidad de muestra para una calidad de CD es de 44,1 Khz (44100 Hz). Si no necesitas alta fidelidad para un sonido, puedes elegir una menor velocidad de muestra. Las velocidades más altas pueden ser apropiadas para aplicaciones de audio profesionales, pero probablemente no las necesites a menos que tu juego requiera un sonido con fidelidad profesional.
-   Emisores de sonido (u orígenes). En XAudio2, los emisores de sonido son ubicaciones que emiten un sonido, bien sea el mero pitido de un ruido de fondo o una ruidosa pista de rock reproducida en una máquina de discos del juego. Los emisores se especifican por coordenadas globales.
-   Escuchas de sonido. Una escucha de sonido suele ser el jugador, o quizás una entidad de IA en un juego más avanzado, que procesa los sonidos recibidos por una escucha. Puedes submezclar ese sonido en la secuencia de audio para reproducción para el jugador, o usarlo para llevar una acción específica dentro del juego, como despertar a un guardia de IA marcado como escucha.

## <a name="design-considerations"></a>Consideraciones de diseño

El audio es una parte tremendamente importante del diseño y desarrollo de juegos. Muchos jugadores recuerdan un juego mediocre elevado a la categoría de leyenda debido simplemente a una banda sonora memorable, o un gran trabajo de voz y mezcla de sonido, o a una producción de audio estelar. La música y el sonido definen la personalidad de un juego y establecen el motivo principal que lo define y lo hace destacar frente a juegos similares. El esfuerzo que dediques a diseñar y desarrollar el perfil de audio de tu juego merecerá la pena.

El audio 3D posicional puede agregar un nivel de envolvimiento que va más allá del que proporcionan los gráficos en 3D. Si estás desarrollando un juego complejo que simula un mundo, o que requiere un estilo cinematográfico, piensa en la posibilidad de usar técnicas de audio posicional 3D para atraer realmente al jugador.

## <a name="directx-audio-development-roadmap"></a>Guía básica para el desarrollo de audio en DirectX

### <a name="xaudio2-conceptual-resources"></a>Recursos generales de XAudio2

XAudio2 es la biblioteca de mezcla de audio para DirectX y su objetivo principal es desarrollar motores de audio de alto rendimiento para juegos. Para los desarrolladores de juegos que quieren agregar efectos de sonido y música de fondo a sus juegos modernos, XAudio2 ofrece un motor de mezcla de gráfico de audio con baja latencia y compatibilidad con búferes dinámicos, reproducción sincrónica fiel a la muestra y conversión implícita de velocidad de origen.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/xaudio2-introduction">Introducción a XAudio2</a></p></td>
<td align="left"><p>En este tema, proporcionamos una lista de las características de programación de audio de XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/getting-started">Guía rápida de XAudio2</a></p></td>
<td align="left"><p>En este tema, explicamos los conceptos clave de XAudio2, las versiones de XAudio2 y el formato de audio RIFF.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/common-audio-concepts">Conceptos comunes de la programación de audio</a></p></td>
<td align="left"><p>En este tema se proporciona una descripción general de los conceptos comunes de audio que un desarrollador de audio debería conocer.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/xaudio2-voices">Voces de XAudio2</a></p></td>
<td align="left"><p>En este tema, incluimos una descripción general de las voces de XAudio2, que se usan para submezclar, operar y procesar datos de audio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/xaudio2-callbacks">Devoluciones de llamadas de XAudio2</a></p></td>
<td align="left"><p>En este tema, explicamos las devoluciones de llamadas de XAudio2, que se usan para prevenir interrupciones en la reproducción de audio.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/audio-graphs">Gráficos de audio de XAudio2</a></p></td>
<td align="left"><p>En este tema se explican los gráficos de procesamiento de audio de XAudio2, que toman un conjunto de secuencias de audio del cliente como entrada, las procesan y entregan el resultado final en un dispositivo de audio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/xaudio2-audio-effects">Efectos de audio de XAudio2</a></p></td>
<td align="left"><p>En este tema, describimos los efectos de audio de XAudio2, que toman datos de audio entrantes y realizan operaciones en esos datos (como un efecto de reverberación) antes de pasarlos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/xaudio2-streaming-audio-data">Transmitir datos de audio con XAudio2</a></p></td>
<td align="left"><p>En este tema, abordamos la transmisión de audio con XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/x3daudio">X3DAudio</a></p></td>
<td align="left"><p>En este tema, hablamos sobre X3DAudio, una API usada en conjunto con XAudio2 para crear una ilusión de sonido desde un punto en el espacio 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/programming-reference">Referencia de programación de XAudio2</a></p></td>
<td align="left"><p>Esta sección contiene la referencia completa para las API de XAudio2.</p></td>
</tr>
</tbody>
</table>

### <a name="xaudio2-how-to-resources"></a>Recursos "cómo" de XAudio2

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--initialize-xaudio2">Cómo: inicializar XAudio2</a></p></td>
<td align="left"><p>Aprende a inicializar XAudio2 para la reproducción de audio, al crear una instancia del motor XAudio2 y una voz de procesamiento.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2">Cómo: cargar archivos de datos de audio en XAudio2</a></p></td>
<td align="left"><p>Aprende a rellenar las estructuras necesarias para reproducir datos de audio en XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2">Cómo: reproducir un sonido con XAudio2</a></p></td>
<td align="left"><p>Aprende a reproducir datos de audio cargados previamente en XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--use-submix-voices">Cómo: usar voces de submezcla</a></p></td>
<td align="left"><p>Aprende a establecer grupos de voces para enviar su salida a la misma voz de submezcla.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--use-source-voice-callbacks">Cómo: usar devoluciones de llamadas de voces de origen</a></p></td>
<td align="left"><p>Aprende a usar devoluciones de llamadas de voces de origen en XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--use-engine-callbacks">Cómo: usar devoluciones de llamadas de motores</a></p></td>
<td align="left"><p>Aprende a usar devoluciones de llamadas de motores en XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--build-a-basic-audio-processing-graph">Cómo: crear un gráfico de procesamiento de audio básico</a></p></td>
<td align="left"><p>Aprende a crear un gráfico de procesamiento de audio, construido a partir de una sola voz de procesamiento y una sola voz de origen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--dynamically-add-or-remove-voices-from-an-audio-graph">Cómo: agregar o quitar voces de un gráfico de audio dinámicamente</a></p></td>
<td align="left"><p>Aprende a agregar o quitar voces de submezcla de un gráfico que se ha creado siguiendo los pasos de <a href="/windows/desktop/xaudio2/how-to--build-a-basic-audio-processing-graph">Cómo: crear un gráfico de procesamiento de audio básico</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--create-an-effect-chain">Cómo: crear un efecto en cadena</a></p></td>
<td align="left"><p>Aprende a aplicar un efecto en cadena a una voz para permitir el procesamiento personalizado de los datos de audio para esa voz.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--create-an-xapo">Cómo: crear un XAPO</a></p></td>
<td align="left"><p>Obtenga información sobre cómo implementar <a href="/windows/desktop/api/xapo/nn-xapo-ixapo"><strong>IXAPO</strong></a> para crear un objeto de procesamiento de audio (XAPO) de XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--add-run-time-parameter-support-to-an-xapo">Cómo: agregar compatibilidad con parámetros en tiempo de ejecución en un XAPO</a></p></td>
<td align="left"><p>Obtenga información sobre cómo agregar compatibilidad de parámetros en tiempo de ejecución a un XAPO implementando la interfaz <a href="/windows/desktop/api/xapo/nn-xapo-ixapoparameters"><strong>IXAPOParameters</strong></a> .</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--use-an-xapo-in-xaudio2">Cómo: usar un XAPO en XAudio2</a></p></td>
<td align="left"><p>Aprende a usar un efecto implementado como XAPO en una cadena de efectos de XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--use-xapofx-in-xaudio2">Cómo: usar XAPOFX en XAudio2</a></p></td>
<td align="left"><p>Aprende a usar uno de los efectos incluidos en XAPOFX en una cadena de efectos de XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--stream-a-sound-from-disk">Cómo: transmitir un sonido de un disco</a></p></td>
<td align="left"><p>Aprende a transmitir datos de audio en XAudio2, creando un subproceso independiente para leer un búfer de audio, y cómo usar devoluciones de llamadas para controlar ese subproceso.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--integrate-x3daudio-with-xaudio2">Cómo: integrar X3DAudio con XAudio2</a></p></td>
<td align="left"><p>Aprende a usar X3Audio para proporcionar los valores de volumen y tono de las voces de XAudio2, así como los parámetros para el efecto de reverberación integrado en XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/xaudio2/how-to--group-audio-methods-as-an-operation-set">Cómo: agrupar métodos de audio como un conjunto de operaciones</a></p></td>
<td align="left"><p>Aprende a usar conjuntos de operaciones de XAudio2 para hacer que un grupo de llamadas a métodos tengan efecto al mismo tiempo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/xaudio2/debugging-audio-glitches-in-xaudio2">Depurar problemas de audio en XAudio2</a></p></td>
<td align="left"><p>Aprende a establecer el nivel de registro de depuración para XAudio2.</p></td>
</tr>
</tbody>
</table>

### <a name="media-foundation-resources"></a>Recursos de Media Foundation

Media Foundation (MF) en una plataforma de medios para transmitir reproducciones de audio y vídeo. Puedes usar las API de Media Foundation para transmitir audio y vídeo  codificados y comprimidos con una variedad de algoritmos. No está diseñado para juegos en tiempo real, pero proporciona eficaces herramientas y una amplia compatibilidad con códecs para una captura y presentación más lineales de componentes de audio y vídeo.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/medfound/about-the-media-foundation-sdk">Acerca de Media Foundation</a></p></td>
<td align="left"><p>Esta sección incluye información general sobre las API de Media Foundation y las herramientas disponibles que las admiten.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/medfound/media-foundation-programming--essential-concepts">Media Foundation: conceptos esenciales</a></p></td>
<td align="left"><p>En este tema, introducimos algunos conceptos que necesitas comprender antes de escribir una aplicación de Media Foundation.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/medfound/media-foundation-architecture">Arquitectura de Media Foundation</a></p></td>
<td align="left"><p>Esta sección describe el diseño general de Microsoft Media Foundation, así como los primitivos de medios y canalización de procesamiento que usa.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/medfound/audio-video-capture">Captura de audio y vídeo</a></p></td>
<td align="left"><p>En este tema, describimos cómo usar Microsoft Media Foundation para realizar capturas de audio y vídeo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/medfound/audio-video-playback">Reproducción de audio y vídeo</a></p></td>
<td align="left"><p>En este tema, describimos cómo implementar la reproducción de audio y vídeo en la aplicación.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/medfound/supported-media-formats-in-media-foundation">Formatos de medios admitidos en Media Foundation</a></p></td>
<td align="left"><p>En este tema, mostramos los formatos de medios que Microsoft Media Foundation admite de forma nativa. (Es posible que terceros admitan formatos adicionales si escriben complementos personalizados).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/medfound/encoding-and-file-authoring">Codificación y creación de archivos</a></p></td>
<td align="left"><p>En este tema, describimos cómo usar Microsoft Media Foundation para la codificación de audio y vídeo, y para crear archivos multimedia.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/medfound/windows-media-codecs">Códecs de Windows Media</a></p></td>
<td align="left"><p>En este tema, describimos cómo usar las características de audio y códecs de vídeo de Windows Media para producir y consumir flujos de datos comprimidos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/desktop/medfound/media-foundation-programming-reference">Referencia de programación de Media Foundation</a></p></td>
<td align="left"><p>Esta sección contiene información de referencia para las API de Media Foundation.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/desktop/medfound/media-foundation-sdk-samples">Muestras de SDK de Media Foundation</a></p></td>
<td align="left"><p>Esta sección muestra aplicaciones de muestra del uso de Media Foundation.</p></td>
</tr>
</tbody>
</table>

### <a name="windows-runtime-xaml-media-types"></a>Tipos de medios XAML de Windows Runtime

Si usa la [interoperabilidad de DirectX-XAML](/previous-versions/windows/apps/hh825871(v=win.10)), puede incorporar el Windows Runtime las API multimedia de XAML en las aplicaciones de UWP con DirectX con C++ para escenarios de juegos más sencillos.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="/uwp/api/Windows.UI.Xaml.Controls.MediaElement"><strong>Windows. UI. Xaml. Controls. MediaElement</strong></a></p></td>
<td align="left"><p>Elemento XAML que representa un objeto que contiene audio, vídeo o ambos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/uwp/audio-video-camera/index">Audio, vídeo y cámara</a></p></td>
<td align="left"><p>Aprende a incorporar audio y vídeo básicos en tu aplicación para la Plataforma universal de Windows (UWP).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/uwp/design/controls-and-patterns/media-playback">MediaElement</a></p></td>
<td align="left"><p>Aprende a reproducir archivos multimedia almacenados localmente en tu aplicación para UWP.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/windows/uwp/design/controls-and-patterns/media-playback">MediaElement</a></p></td>
<td align="left"><p>Aprende a transmitir un archivo multimedia con latencia baja en tu aplicación para UWP.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/windows/uwp/audio-video-camera/media-casting">Transmitir contenido multimedia</a></p></td>
<td align="left"><p>Aprende a usar el contrato de Reproducir en para transmitir multimedia de tu aplicación para UWP a otro dispositivo.</p></td>
</tr>
</tbody>
</table>

## <a name="reference"></a>Referencia

-   [Introducción a XAudio2](/windows/desktop/xaudio2/xaudio2-introduction)
-   [Guía de programación de XAudio2](/windows/desktop/xaudio2/programming-guide)
-   [Introducción a Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk)

## <a name="related-topics"></a>Temas relacionados

-   [Guía de programación de XAudio2](/windows/desktop/xaudio2/programming-guide)