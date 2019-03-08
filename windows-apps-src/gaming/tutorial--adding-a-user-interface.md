---
title: Agregar una interfaz de usuario
description: Obtenga información sobre cómo agregar una superposición de la interfaz de usuario 2D a un juego DirectX UWP.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, interfaz de usuario, directx
ms.localizationpriority: medium
ms.openlocfilehash: 09005eb12997126a9cad68c388beb0473b19fda3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609060"
---
# <a name="add-a-user-interface"></a>Agregar una interfaz de usuario


Ahora que nuestro juego tiene sus objetos visuales 3D en su lugar, es el tiempo para centrarse en agregar algunos elementos 2D para que el juego puede proporcionar comentarios sobre el estado del juego al Reproductor. Esto puede realizarse mediante la adición de opciones de menú simple y salida de la canalización de componentes de visualización sobreimpresa encima de los gráficos 3D.

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Con Direct2D, agregue una serie de gráficos de interfaz de usuario y comportamientos a nuestros juegos de DirectX de UWP incluidos:
- Mostrar Heads-Up, incluidos [controlador de movimiento vistazo](tutorial--adding-controls.md) rectángulos de límite
- Menús del estado del juego


## <a name="the-user-interface-overlay"></a>La superposición de la interfaz de usuario


Aunque hay muchas maneras de mostrar los elementos de la interfaz de usuario y el texto en un juego de DirectX, nos vamos a centrar sobre el uso de [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx). También vamos a usar [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) para los elementos de texto.


Direct2D es un conjunto de API de dibujos 2D que se usa para dibujar los efectos y primitivas basados en píxeles. Cuando comience con Direct2D, es mejor simplificar las cosas. Los diseños y los comportamientos de interfaz complejos requieren tiempo y planificación. Si el juego requiere una interfaz de usuario complejas, como los que se encuentran en la simulación y juegos de estrategia, considere la posibilidad de usar de XAML en su lugar.

> [!NOTE]
> Para obtener información sobre cómo desarrollar una interfaz de usuario con XAML en un juego DirectX de UWP, consulte [ampliar el ejemplo de juego](tutorial-resources.md).

Direct2D no está diseñado específicamente para las interfaces de usuario o diseños como HTML y XAML. No proporciona componentes de interfaz de usuario, como listas, cuadros o botones. No proporciona componentes de diseño, como etiquetas div, tablas o cuadrículas.


Para este ejemplo de juego, tenemos dos componentes principales de la interfaz de usuario.
1. Una pantalla de aviso para los controles de juego y puntuación.
2. Una superposición se utiliza para mostrar el texto de estado del juego y opciones como la información de pausa y nivel de opciones de inicio.

### <a name="using-direct2d-for-a-heads-up-display"></a>Usar Direct2D para una pantalla de visualización frontal

La siguiente imagen muestra la pantalla de aviso en el juego para el ejemplo. Es simple y ordenada, lo que permite el Reproductor para centrarse en explorar el mundo 3D y los destinos de la solución. Una buena interfaz o la visualización sobreimpresa nunca debe complicar la capacidad del Reproductor para procesar y reaccionar a los eventos en el juego.

![captura de pantalla de la superposición del juego](images/simple-dx-game-ui-overlay.png)

La superposición consta de los siguientes primitivos básicos.
- [**DirectWrite** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038) texto en la esquina superior derecha que informa el encargado de 
    - Aciertos correcta
    - Número de capturas que se ha realizado el Reproductor
    - Tiempo restante en el nivel
    - Número de nivel actual 
- Dos segmentos de línea que se usa para formar una cruz de intersección
- Dos rectángulos en las esquinas de la parte inferior de la [controlador de movimiento vistazo](tutorial--adding-controls.md) límites. 


El estado de la pantalla de aviso en el juego de la superposición se dibuja en el [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) método de la [ **GameHud** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) clase. Dentro de este método, la superposición de Direct2D que representa la interfaz de usuario se actualiza para reflejar los cambios en el número de visitas, el número de tiempo restante y nivel.

Si se ha inicializado el juego, agregamos `TotalHits()`, `TotalShots()`, y `TimeRemaining()` a un [ **swprintf_s** ](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) almacene en búfer y especificar el formato de impresión. A continuación, podemos dibujar con el [ **DrawText** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848) método. Se lo mismo para el indicador de nivel actual, dibujo números vacíos para mostrar los niveles sin completar como ➀ y rellena los números como ➊ para mostrar que se ha completado el nivel específico.


El siguiente fragmento de código se recorre el **GameHud::Render** proceso del método para 
- Crear un mapa de bits mediante [** ID2D1RenderTarget::DrawBitmap **](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- Separe las áreas de interfaz de usuario en rectángulos con [ **D2D1::RectF**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- Uso de **DrawText** para hacer que los elementos de texto

```cpp
void GameHud::Render(_In_ Simple3DGame^ game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.Get(),
            D2D1::RectF(
                GameConstants::Margin,
                GameConstants::Margin,
                m_logoSize.width + GameConstants::Margin,
                m_logoSize.height + GameConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameConstants::Margin, GameConstants::Margin),
            m_titleHeaderLayout.Get(),
            m_textBrush.Get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.Get(),
            m_textBrush.Get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static char16 wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );
        
        // Draw the upper right portion of the HUD displaying total hits, shots, and time remaining
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3
                ),
            m_textBrush.Get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32 levelCharacter[6];
        for (uint32 i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        // Create a new rectangle and draw the current level info text inside
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3 + GameConstants::Margin,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize+ GameConstants::Margin) * 4
                ),
            m_textBrush.Get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            // Draw the crosshairs
        }
    }
}
```

Interrumpir el método abajo aún más, este fragmento de la [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) método dibuja nuestro cambio y se activan los rectángulos con [ **ID2D1RenderTarget::DrawRectangle** ](https://msdn.microsoft.com/library/windows/desktop/dd371902)y con forma de cruz mediante dos llamadas a [ **ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895).

```cpp
        // Check if game is playing
        if (game->IsActivePlay())
        {
            // Draw a rectangle for the touch input for the move control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    0.0f,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    GameConstants::TouchRectangleSize,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );
            // Draw a rectangle for the touch input of the fire control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    windowBounds.Width - GameConstants::TouchRectangleSize,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    windowBounds.Width,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );

            // Draw two lines to form crosshairs
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f - GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                D2D1::Point2F(windowBounds.Width / 2.0f + GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                m_textBrush.Get(),
                3.0f
                );
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f - GameConstants::CrossHairHalfSize),
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f + GameConstants::CrossHairHalfSize),
                m_textBrush.Get(),
                3.0f
                );
        }
```

En el **GameHud::Render** método almacenamos el tamaño lógico de la ventana del juego en el `windowBounds` variable. Esto utiliza el [ `GetLogicalSize` ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) método de la **DeviceResources** clase. 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 Obtener el tamaño de la ventana del juego es esencial para la programación de la interfaz de usuario. El tamaño de la ventana se proporciona en una medida denominada DIP (píxeles independientes del dispositivo), donde una DIP se define como 1/96 de pulgada. Direct2D se escala el dibujos unidades de píxeles reales cuando se produce el dibujo, si lo hace, mediante el uso de la Windows Configuración de puntos por pulgada (PPP). De forma similar, al dibujar texto mediante [ **DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038), especifique DIP en lugar de puntos para el tamaño de la fuente. Los DIP se expresan como números de punto flotante.

 

### <a name="displaying-game-state-info"></a>Mostrar información de estado del juego

Además de la pantalla de aviso, el ejemplo del juego tiene una superposición que representa el estado del juego seis. Todos los Estados de características con el texto para el Reproductor leer una primitiva de rectángulo negro grande. No se dibujan los rectángulos de controlador de movimiento vistazo y cruces porque no están activas en estos Estados.

La superposición se crea utilizando el [ **GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) (clase), lo que nos permite desactivar qué texto se muestra para alinearse con el estado del juego.

![estado y el funcionamiento de superposición](images/simple-dx-game-ui-finaloverlay.png)

La superposición se divide en dos secciones: **Estado** y **acción**. El **estado** producirlo adicional se divide en **título** y **cuerpo** rectángulos. El **acción** sección tiene solo un rectángulo. Cada rectángulo tiene un propósito diferente.

-   `titleRectangle` contiene el texto del título.
-   `bodyRectangle` contiene el texto del cuerpo.
-   `actionRectangle` contiene el texto que informa el Reproductor para realizar una acción específica.

El juego tiene seis Estados que se pueden establecer. El estado del juego cubre usando el **estado** parte de la superposición. El **estado** rectángulos se actualizan mediante una serie de métodos que se corresponden con los siguientes estados.

- Cargando
- Estadísticas de inicio inicial y alta puntuación
- Inicio de nivel
- Juego en pausa
- Fin del juego
- Ha ganado


El **acción** parte de la superposición se actualiza mediante el [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) método, lo que permite el texto de acción debe establecerse en uno de los siguientes.
- "Tap para reproducirlas de nuevo..."
- "Nivel de carga, por favor, espere..."
- "Pulse para continuar..."
- Ninguno

> [!NOTE]
> Ambos métodos se tratarán con más detalle en la [que representa el estado del juego](#representing-game-state) sección.

Dependiendo de lo que ocurre en el juego, el **estado** y **acción** se ajustan los campos de texto de la sección.
Echemos un vistazo a cómo inicializar y dibujar la superposición para estas seis estados.

### <a name="initializing-and-drawing-the-overlay"></a>Inicialización y dibujo de la superposición

Los seis **estado** estados tienen algunas cosas en común, hacer que los recursos y los métodos que necesitan muy similares.
    - Todos usan un rectángulo negro en el centro de la pantalla que su fondo.
    - El texto mostrado es **título** o **cuerpo** texto.
    - El texto utiliza la fuente Segoe UI y se dibuja en la parte superior del rectángulo de back. 


El ejemplo de juego tiene cuatro métodos que entran en juego cuando se crea la superposición.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
El [ **GameInfoOverlay::GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) constructor inicializa la superposición, mantener la superficie del mapa de bits que se utilizará para mostrar información para el Reproductor en. El constructor Obtiene un generador de la [ **ID2D1Device** ](https://msdn.microsoft.com/library/windows/desktop/hh404478) objeto pasa a él, usa para crear un [ **ID2D1DeviceContext** ](https://msdn.microsoft.com/library/windows/desktop/hh404479) que puede dibujar el propio objeto de superposición a. [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) es nuestro método para crear pinceles que se utilizará para dibujar el texto. Para ello, obtenemos un [ **ID2D1DeviceContext2** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789) objeto que permite la creación y dibujo de geometría, además de funcionalidad como la escritura con lápiz y degradado representación malla. A continuación, creamos una serie de pinceles color con [ **ID2D1SolidColorBrush** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207) para dibujar los elementos de interfaz de usuario siguientes.
- Pincel negro para los fondos de rectángulo
- Pincel para el texto de estado
- Pincel de color naranja para el texto de acción

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
El [ **DeviceResources::SetDpi** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) método establece los puntos por pulgada de la ventana. Este método se llama cuando el valor de PPP se cambia y debe ser reajustará que ocurre cuando se cambia el tamaño de la ventana del juego. Después de actualizar el valor de PPP, este método también llama[**DeviceResources::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) necesarias está seguro de que los recursos se vuelven a crear cada vez que se cambia el tamaño de la ventana.


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
El [ **GameInfoOverlay::CreateWindowsSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) método es donde realiza todos los dibujos de nuestro. El siguiente es un esquema de los pasos del método.
- Se crean tres rectángulos sección desactivar el texto de la interfaz de usuario para el **título**, **cuerpo**, y **acción** texto.
    ```cpp 
    m_titleRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin + GameInfoOverlayConstant::TitleHeight
        );
    m_actionRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        overlaySize.height - (GameInfoOverlayConstant::ActionHeight + GameInfoOverlayConstant::BottomMargin),
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        overlaySize.height - GameInfoOverlayConstant::BottomMargin
        );
    m_bodyRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        m_titleRectangle.bottom + GameInfoOverlayConstant::Separator,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        m_actionRectangle.top - GameInfoOverlayConstant::Separator
        );
    ```

- Un mapa de bits se crea con el nombre `m_levelBitmap`, teniendo el valor de PPP actual en la cuenta mediante **CreateBitmap**.
- `m_levelBitmap` se establece como nuestro destino de representación 2D mediante [ **ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533).
- Se borra el mapa de bits con cada píxel realizado negro mediante [ **ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772).
- [**ID2D1RenderTarget::BeginDraw** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768) llamar para iniciar el dibujo. 
- **DrawText** se llama para dibujar el texto almacenado en `m_titleString`, `m_bodyString`, y `m_actionString` en el rectángulo approperiate con el correspondiente **ID2D1SolidColorBrush**.
- [**ID2D1RenderTarget::EndDraw** ](ID2D1RenderTarget::EndDraw) se llama para detener todas las operaciones de dibujos en `m_levelBitmap`.
- Se crea otro mapa de bits mediante **CreateBitmap** denominado `m_tooSmallBitmap` a usar como reserva, que muestra solo si la configuración de la visualización es demasiado pequeña para el juego.
- Repita el proceso para dibujar en `m_levelBitmap` para `m_tooSmallBitmap`, pero esta vez sólo la cadena de dibujo `Paused` en el cuerpo.




Ahora todo lo que necesitamos son los seis métodos para rellenar el texto de nuestro Estados seis superposición!

### <a name="representing-game-state"></a>Que representa el estado del juego


Cada uno de los Estados de superposición de seis en el juego tiene un método correspondiente en el **GameInfoOverlay** objeto. Estos métodos dibujan una variación de la superposición para comunicar información explícita al jugador acerca del propio juego. Esta comunicación se representa con un **título** y **cuerpo** cadena. Dado que ya ha configurado en el ejemplo de los recursos y el diseño de esta información cuando se ha inicializado y con el [ **GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) método, solo es necesario proporcionar las cadenas específicas del estado de superposición.

El **estado** parte de la superposición se establece con una llamada a uno de los métodos siguientes.

Estado del juego | Estado establecido (método) | Campos de estado
:----- | :------- | :---------
Cargando | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Title**</br>Cargar los recursos </br>**Body**</br> Incrementalmente imprime "." que implique la actividad de carga.
Estadísticas de inicio inicial y alta puntuación | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Title**</br>Puntuación más alta</br> **Body**</br> Los niveles de completado # </br>Total de puntos de #</br>Total de capturas de #
Inicio de nivel | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Title**</br># Nivel</br>**Body**</br>Descripción del nivel de objetivo.
Juego en pausa | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Title**</br>Juego en pausa</br>**Body**</br>Ninguno
Fin del juego | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>Fin</br> **Body**</br> Los niveles de completado # </br>Total de puntos de #</br>Total de capturas de #</br>Los niveles de completado #</br>Alta puntuación #
Ha ganado | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>¡Has ganado!</br> **Body**</br> Los niveles de completado # </br>Total de puntos de #</br>Total de capturas de #</br>Los niveles de completado #</br>Alta puntuación #




Con el [ **GameInfoOverlay::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) método, el ejemplo declara tres áreas rectangulares que se corresponden con regiones específicas de la superposición.



Teniendo en cuenta estas áreas, veamos uno de los métodos específicos de estado, **GameInfoOverlay::SetGameStats**, y cómo se dibuja la superposición.

```cpp
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.Get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.Get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.Get());
    m_titleString = "High Score";

    d2dContext->DrawText(
        m_titleString->Data(),
        m_titleString->Length(),
        m_textFormatTitle.Get(),
        m_titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = ref new Platform::String(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString->Data(),
        m_bodyString->Length(),
        m_textFormatBody.Get(),
        m_bodyRectangle,
        m_textBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
```

Utilizando el contexto de dispositivo Direct2D que la **GameInfoOverlay** objeto inicializado, este método rellena los rectángulos de título y el cuerpo con negro con el pincel del fondo. Dibuja el texto para la cadena "Máxima puntuación" en el rectángulo de título y una cadena que contiene la información de estado del juego actualizada en el rectángulo de cuerpo usando el pincel de texto blanco.


El rectángulo de la acción se actualiza mediante una llamada subsiguiente a [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) desde un método en el **GameMain** object, que proporciona la información de estado del juego necesaria por **GameInfoOverlay::SetAction** para determinar el mensaje correcto para el Reproductor, como "Pulse para continuar".

Se elige la superposición para cualquier estado determinado en el [ **GameMain::SetGameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) método similar al siguiente:

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading();
        break;

    case GameInfoOverlayState::GameStats:
        m_uiControl->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_uiControl->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_uiControl->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_uiControl->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_uiControl->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
```

Ahora el juego tiene una manera de comunicar información de texto con el Reproductor según el estado del juego, y tenemos una manera de cambiar lo que se muestra a ellos a lo largo del juego.

### <a name="next-steps"></a>Pasos siguientes

En el siguiente tema, [Agregar controles](tutorial--adding-controls.md), vemos cómo el jugador interactúa con la muestra de juego y cómo la entrada cambia el estado del juego.



 




