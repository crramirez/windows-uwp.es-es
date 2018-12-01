---
title: Agregar una interfaz de usuario
description: Obtén información sobre cómo agregar una superposición de interfaz de usuario 2D a un juego DirectX UWP.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, interfaz de usuario, directx
ms.localizationpriority: medium
ms.openlocfilehash: 09005eb12997126a9cad68c388beb0473b19fda3
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8348434"
---
# <a name="add-a-user-interface"></a>Agregar una interfaz de usuario


Ahora que nuestro juego tiene sus vistas en 3D en su lugar, es el momento centrarse en agregar algunos elementos 2D para que el juego puede proporcionar comentarios sobre el estado al jugador. Esto puede realizarlo agregar sencillas opciones de menú y la salida de la canalización de componentes de visualización frontal encima de los gráficos 3D.

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Ten en cuenta que este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Usar Direct2D, agrega un número de elementos gráficos de interfaz de usuario y los comportamientos a nuestro juego de DirectX de UWP incluidos:
- Pantalla de visualización frontal, incluidos los rectángulos de [controlador de movimiento y vista](tutorial--adding-controls.md) de límite
- Menús de estado del juego


## <a name="the-user-interface-overlay"></a>La superposición de la interfaz de usuario


Si bien existen muchas formas de mostrar elementos de interfaz de usuario y texto en un juego DirectX, vamos a foco sobre el uso de [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx). También usaremos [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) para los elementos de texto.


Direct2D es que un conjunto de API de dibujo 2D se usa para dibujar primitivos basados en píxeles y efectos. Al iniciar un vistazo a con Direct2D, es mejor hacerlo más sencillo. Los diseños y los comportamientos de interfaz complejos requieren tiempo y planificación. Si tu juego requiere una interfaz de usuario complejas, como las de simulación y juegos de estrategia, considera la posibilidad de usar XAML en su lugar.

> [!NOTE]
> Para obtener información sobre cómo desarrollar una interfaz de usuario con XAML en un juego DirectX de UWP, vea [Extender la muestra de juego](tutorial-resources.md).

Direct2D no está diseñado específicamente para interfaces de usuario o diseños como HTML y XAML. No proporciona componentes de interfaz de usuario, como botones, cuadros o listas. No proporciona componentes de diseño como etiquetas div, tablas o cuadrículas.


Para esta muestra de juego tenemos dos componentes principales de la interfaz de usuario.
1. Una pantalla de visualización frontal para la puntuación y controles en el juego.
2. Una superposición que se usa para mostrar texto de estado del juego y las opciones, como información de pausa y opciones de inicio de nivel.

### <a name="using-direct2d-for-a-heads-up-display"></a>Usar Direct2D para una pantalla de visualización frontal

La siguiente imagen muestra la pantalla de visualización frontal en el juego para la muestra. Es sencilla y despejada, lo que permite al jugador centrarse en navegar por el mundo en 3D y destinos de disparos. Una buena interfaz o pantalla de visualización frontal nunca debe complicar la posibilidad de que el jugador de procesar y reaccionar a los eventos en el juego.

![captura de pantalla de la superposición del juego](images/simple-dx-game-ui-overlay.png)

La superposición consta de los primitivos básicos siguientes.
- Texto de [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038) en la esquina superior derecha que informa al jugador de 
    - Impactos
    - Número de disparos realizados por el jugador
    - Tiempo restante en el nivel
    - Número de nivel actual 
- Dos intersección segmentos de línea que se utiliza para formar un enlazado de manera cruzada con
- Dos rectángulos en las esquinas de la parte inferior de los límites de [controlador de movimiento y vista](tutorial--adding-controls.md) . 


El estado de visualización frontal en el juego de la superposición se dibuja en el método [**gamehud:: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) de la clase [**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) . Dentro de este método, la superposición Direct2D que representa nuestra interfaz de usuario se actualiza para reflejar los cambios en el número de visitas, número de tiempo restante y nivel.

Si el juego se ha inicializado, agregamos `TotalHits()`, `TotalShots()`, y `TimeRemaining()` a un [**swprintf_s**](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) búfer y especificar el formato de impresión. A continuación, podemos dibujar mediante el método [**DrawText**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848) . Hacemos los mismos para el indicador de nivel actual, números vacíos para mostrar los niveles sin completar como ➀ y números de rellenado como ➊ para mostrar que se ha completado el nivel específico de dibujo.


El siguiente fragmento de código que te guiará a través de proceso del método **gamehud:: Render** para 
- Crear un mapa de bits mediante [** ID2D1RenderTarget::DrawBitmap **](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- Las secciones desactivado áreas de la interfaz de usuario en rectángulos con [ **D2D1::RectF**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- Usar **DrawText** para preparar los elementos de texto

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

Interrumpir el método abajo más allá, esta parte del [**gamehud:: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) método dibuja nuestro movimiento y disparo rectángulos con [**ID2D1RenderTarget::DrawRectangle**](https://msdn.microsoft.com/library/windows/desktop/dd371902)y cruces con dos llamadas a [**ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895).

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

En el método **gamehud:: Render** almacenamos el tamaño lógico de la ventana del juego en la `windowBounds` variable. Esto se usa el [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) método de la clase **DeviceResources** . 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 Obtener el tamaño de la ventana del juego es esencial para la programación de la interfaz de usuario. El tamaño de la ventana se proporciona en una medición llamada DIPs (píxeles independientes del dispositivo), donde un DIP se define como 1/96 de una pulgada. Direct2D escala las unidades de dibujo en píxeles reales cuando se produce el dibujo, si lo haces, mediante el uso de los puntos por pulgada (PPP) de Windows. Del mismo modo, cuando dibujas texto usando [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038), especificas DIP en lugar de puntos para el tamaño de la fuente. Los DIP se expresan como números de punto flotante.

 

### <a name="displaying-game-state-info"></a>Mostrar información de estado del juego

Además de la pantalla de visualización frontal, la muestra de juego tiene una superposición que representa seis estados del juego. Todos los Estados de características de un primitivo de gran tamaño rectángulo negro con texto para el jugador lo lea. Los rectángulos de controlador de movimiento y vista y cruz no se dibuja porque no están activos en estos Estados.

La superposición se crea mediante la clase [**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) , lo que nos permite cambiar el texto que se muestra para alinearse con el estado del juego.

![estado y la acción de superposición](images/simple-dx-game-ui-finaloverlay.png)

La superposición se divide en dos secciones: **estado** y la **acción**. La sección de **estado** aún más se desglosa en rectángulos de **título** y **cuerpo** . La sección **acción** solo tiene un rectángulo. Cada rectángulo tiene un propósito diferente.

-   `titleRectangle` contiene el texto del título.
-   `bodyRectangle` contiene el texto del cuerpo.
-   `actionRectangle` contiene el texto que informa al jugador para realizar una acción específica.

El juego tiene seis Estados que se pueden establecer. El estado del juego transmitido con la parte del **estado** de la superposición. Los rectángulos de **estado** se actualizan mediante una serie de métodos correspondientes con los siguientes estados.

- Cargando
- Estadísticas de puntuación de inicio/alto inicial
- Inicio de nivel
- Juego en pausa
- Fin del juego
- Ha ganado el juego


La parte de la **acción** de la superposición se actualiza con el método [**gameinfooverlay:: Setaction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) , lo que permite el texto de acción establecer en uno de los siguientes.
- "Pulsar para volver a jugar …"
- "Carga de nivel, por favor, espere …"
- "Toca para continuar …"
- Ninguno

> [!NOTE]
> Estos dos métodos se describirán más adelante en la sección [que representa el estado del juego](#representing-game-state) .

Según el qué está sucediendo en el juego, el **estado** y la sección **acción** se ajustan los campos de texto.
Echemos un vistazo a cómo inicializamos y dibujamos la superposición para estos seis estados.

### <a name="initializing-and-drawing-the-overlay"></a>Inicialización y dibujo de la superposición

Los seis estados de **estado** tienen algunas cosas en común, hacer que los recursos y los métodos que necesitan muy similar.
    - Todos los usan un rectángulo negro en el centro de la pantalla como su fondo.
    - El texto mostrado es texto de **título** o de **cuerpo** .
    - El texto usa la fuente Segoe UI y se dibuja encima del rectángulo negro. 


La muestra de juego tiene cuatro métodos que entran en juego al crear la superposición.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
El constructor de [**GameInfoOverlay::GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) inicializa la superposición, mantener la superficie de mapa de bits que se usarán para mostrar información al jugador en. El constructor Obtiene una fábrica de pasa, que se usa para crear un [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479) que el propio objeto de superposición puede dibujar en el objeto de [**ID2D1Device**](https://msdn.microsoft.com/library/windows/desktop/hh404478) . [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>Gameinfooverlay:: Createdevicedependentresources
[**Gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) es nuestro método para crear pinceles que se usará para dibujar el texto. Para ello, hemos obtener un objeto [**ID2D1DeviceContext2**](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789) que permite la creación y dibujo de geometría, además de funcionalidad como entrada de lápiz y el degradado malla de representación. A continuación, creamos una serie de color pinceles con [**ID2D1SolidColorBrush**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207) para dibujar los elementos de la interfaz de usuario siguientes.
- Pincel negro para fondos de rectángulo
- Pincel blanco para el texto de estado
- Pincel de color naranja para el texto de acción

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
El método [**DeviceResources::SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) establece los puntos por pulgada de la ventana. Este método se llama cuando se cambia el valor PPP y se deben ser reajuste lo que sucede cuando se cambia el tamaño de la ventana del juego. Después de actualizar los valores de PPP, este método también llama a[**deviceresources:: Createwindowsizedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) para asegurarte de que los recursos necesarios se vuelven a crear cada vez que se cambie el tamaño de la ventana.


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
El método [**GameInfoOverlay::CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) es donde todos los dibujos de nuestro tiene lugar. El siguiente es un esquema de los pasos del método.
- Se crean tres rectángulos a sección desactivar el texto de la interfaz de usuario para el texto de **título**, **cuerpo**y **acción** .
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

- Un mapa de bits se crea con nombre `m_levelBitmap`, tomando el valor de PPP actual en cuenta con **CreateBitmap**.
- `m_levelBitmap` se establece como destino con [**ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533)de representación de nuestro 2D.
- Se borra el mapa de bits con cada píxel realizado negro usando [**ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772).
- [**ID2D1RenderTarget::BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768) se llama para iniciar el proceso de dibujo. 
- **DrawText** se llama para dibujar el texto que se almacenan en `m_titleString`, `m_bodyString`, y `m_actionString` en el rectángulo de adecuado mediante el correspondiente **ID2D1SolidColorBrush**.
- Se llama a [**ID2D1RenderTarget::EndDraw**](ID2D1RenderTarget::EndDraw) para detener todas las operaciones de dibujo en `m_levelBitmap`.
- Mapa de bits de otro se crea mediante **CreateBitmap** denominado `m_tooSmallBitmap` que se usará como reserva, que muestra solo si la configuración de presentación es demasiado pequeña para el juego.
- Repite el proceso para dibujar en `m_levelBitmap` para `m_tooSmallBitmap`, esta vez solo dibujar la cadena `Paused` en el cuerpo.




Ahora, todo lo que necesitamos son seis métodos para rellenar el texto de nuestro Estados de seis superposición!

### <a name="representing-game-state"></a>Que representa el estado del juego


Cada uno de los Estados de seis superposición en el juego tiene un método correspondiente en el objeto **GameInfoOverlay** . Estos métodos dibujan una variación de la superposición para comunicar información explícita al jugador acerca del propio juego. Esta comunicación se representa con una cadena de **título** y **cuerpo** . Dado que la muestra ya configuró los recursos y el diseño para esta información cuando se inicializa y con el método [**gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) , solo debe proporcionar cadenas específicas del estado de la superposición.

La parte del **estado** de la superposición se establece con una llamada a uno de los métodos siguientes.

Estado del juego | Método set de estado | Campos de estado
:----- | :------- | :---------
Cargando | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Title**</br>Carga de recursos </br>**Body**</br> Imprime de forma incremental "." para implica la actividad de carga.
Estadísticas de puntuación de inicio/alto inicial | [Gameinfooverlay:: Setgamestats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Title**</br>Puntuación más alta</br> **Body**</br> Niveles completado # </br>N.º de puntos totales</br>Total capturas #
Inicio de nivel | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Title**</br>N.º de nivel</br>**Body**</br>Descripción del nivel de objetivo.
Juego en pausa | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Title**</br>Juego en pausa</br>**Body**</br>Ninguno
Fin del juego | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>Fin</br> **Body**</br> Niveles completado # </br>N.º de puntos totales</br>Total capturas #</br>Niveles completado #</br>Alta puntuación #
Ha ganado el juego | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>¡Has ganado!</br> **Body**</br> Niveles completado # </br>N.º de puntos totales</br>Total capturas #</br>Niveles completado #</br>Alta puntuación #




Con el método [**GameInfoOverlay::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) , la muestra declaró tres áreas rectangulares que corresponden a regiones específicas de la superposición.



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

Con el contexto de dispositivo Direct2D que inicializó el objeto **GameInfoOverlay** , este método rellena los rectángulos de título y cuerpo con negro usando el pincel de fondo. Dibuja el texto para la cadena "Máxima puntuación" en el rectángulo de título y una cadena que contiene la información de estado del juego actualizada en el rectángulo de cuerpo usando el pincel de texto blanco.


El rectángulo de acción se actualiza con una llamada posterior a [**gameinfooverlay:: Setaction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) desde un método en el objeto **GameMain** , que proporciona la información de estado del juego necesita **gameinfooverlay:: Setaction** para determinar el mensaje adecuado para el Reproductor, por ejemplo, "Toca para continuar".

La superposición para cualquier estado dado se elige en el método [**GameMain::SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) como este:

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

Ahora el juego tiene una forma de comunicar información de texto al jugador basándose en el estado del juego y tenemos una manera de cambiar de lo que se muestra a ellos a lo largo del juego.

### <a name="next-steps"></a>Pasos siguientes

En el siguiente tema, [Agregar controles](tutorial--adding-controls.md), vemos cómo el jugador interactúa con la muestra de juego y cómo la entrada cambia el estado del juego.



 




