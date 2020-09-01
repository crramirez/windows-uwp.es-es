---
title: Agregar una interfaz de usuario
description: Obtenga información acerca de cómo usar las API de Direct2D para agregar una superposición de interfaz de usuario 2D con los menús de visualización y estado del juego a un juego UWP UWP.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, interfaz de usuario, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: eca248887985fc6d33ca6d6b552a0b61a98ce428
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173109"
---
# <a name="add-a-user-interface"></a>Agregar una interfaz de usuario

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

Ahora que el juego tiene sus objetos visuales 3D, es el momento de centrarse en agregar algunos elementos 2D para que el juego pueda proporcionar comentarios sobre el estado del juego al jugador. Esto puede realizarse mediante la adición de opciones de menú simples y componentes de visualización de inicio en la parte superior de la salida de la canalización de gráficos 3D.

>[!Note]
>Si no ha descargado el código de juego más reciente para este ejemplo, vaya a [juego de ejemplo de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este ejemplo forma parte de una gran colección de ejemplos de características de UWP. Para obtener instrucciones sobre cómo descargar el ejemplo, consulte [obtener los ejemplos de UWP en github](../get-started/get-app-samples.md).

## <a name="objective"></a>Objetivo

Con Direct2D, agregue un número de controles y gráficos de interfaz de usuario a nuestro juego DirectX de UWP, incluidos:
- Presentación en cabeza, incluidos los rectángulos delimitadores [del controlador de movimiento y aspecto](tutorial--adding-controls.md)
- Menús de estado de juego


## <a name="the-user-interface-overlay"></a>La superposición de la interfaz de usuario


Aunque hay muchas maneras de mostrar los elementos de texto y de la interfaz de usuario en un juego de DirectX, vamos a centrarnos en el uso de [Direct2D](/windows/desktop/Direct2D/direct2d-portal). También usaremos [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) para los elementos de texto.


Direct2D es un conjunto de API de dibujo 2D que se usa para dibujar elementos primitivos y efectos basados en píxeles. Al comenzar con Direct2D, es mejor simplificar las cosas. Los diseños y los comportamientos de interfaz complejos requieren tiempo y planificación. Si el juego requiere una interfaz de usuario compleja, como la que se encuentra en los juegos de simulación y estrategia, considere la posibilidad de usar XAML en su lugar.

> [!NOTE]
> Para obtener información sobre el desarrollo de una interfaz de usuario con XAML en un juego de DirectX para UWP, consulte [ampliación del juego de ejemplo](tutorial-resources.md).

Direct2D no está diseñado específicamente para interfaces de usuario ni diseños como HTML y XAML. No proporciona componentes de interfaz de usuario como listas, cuadros o botones. Tampoco proporciona componentes de diseño como divs, tablas o cuadrículas.


Para este juego de ejemplo tenemos dos componentes principales de la interfaz de usuario.
1. Una pantalla emergente para la puntuación y los controles en juego.
2. Una superposición usada para mostrar el texto del estado del juego y las opciones, como pausar información y opciones de inicio de nivel.

### <a name="using-direct2d-for-a-heads-up-display"></a>Usar Direct2D para una pantalla de visualización frontal

En la imagen siguiente se muestra la pantalla de los cabezales en el juego para el ejemplo. Es sencillo y despejado, lo que permite que el jugador se Centre en navegar por el mundo 3D y alcanzar los objetivos. Una buena interfaz o visualización de cabezales nunca debe complicar la capacidad del reproductor para procesar y reaccionar a los eventos del juego.

![captura de pantalla de la superposición del juego](images/simple-dx-game-ui-overlay.png)

La superposición consta de los siguientes primitivos básicos.
- Texto de [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal) en la esquina superior derecha que informa al reproductor de 
    - Aciertos correctos
    - Número de capturas realizadas por el jugador
    - Tiempo restante en el nivel
    - Número de nivel actual 
- Dos segmentos de línea de intersección que se usan para formar un Cruz
- Dos rectángulos en las esquinas inferiores del controlador límites de [movimiento y aspecto](tutorial--adding-controls.md) . 


El estado de presentación de la superposición en el juego se dibuja en el método [**GameHud:: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) de la clase [**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) . Dentro de este método, la superposición de Direct2D que representa la interfaz de usuario se actualiza para reflejar los cambios en el número de aciertos, el tiempo restante y el número de nivel.

Si se ha inicializado el juego, se agregan `TotalHits()` , `TotalShots()` y `TimeRemaining()` a un [**swprintf_s**](/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) búfer y se especifica el formato de impresión. A continuación, podemos dibujarlo mediante el método [**DrawText**](/windows/desktop/Direct2D/id2d1rendertarget-drawtext) . Hacemos lo mismo para el indicador de nivel actual, dibujamos números vacíos para mostrar niveles incompletos como ➀ y números rellenos como ➊ para mostrar que se ha completado el nivel específico.


En el siguiente fragmento de código se recorre el proceso del método **GameHud:: Render** para 
- Crear un mapa de bits con [* * ID2D1RenderTarget::D rawbitmap * *](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_))
- Secciones de las áreas de la interfaz de usuario en rectángulos mediante [ **D2D1:: RectF**](/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
- Usar **DrawText** para crear elementos de texto

```cppwinrt
void GameHud::Render(_In_ std::shared_ptr<Simple3DGame> const& game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.get(),
            D2D1::RectF(
                GameUIConstants::Margin,
                GameUIConstants::Margin,
                m_logoSize.width + GameUIConstants::Margin,
                m_logoSize.height + GameUIConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameUIConstants::Margin, GameUIConstants::Margin),
            m_titleHeaderLayout.get(),
            m_textBrush.get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameUIConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.get(),
            m_textBrush.get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static wchar_t wsbuffer[bufferLength];
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
            m_textFormatBody.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3
                ),
            m_textBrush.get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32_t levelCharacter[6];
        for (uint32_t i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32_t>(game->LevelCompleted()) == i) ? 10 : 0);
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
            m_textFormatBodySymbol.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3 + GameUIConstants::Margin,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 4
                ),
            m_textBrush.get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            ...
            // Draw the crosshairs
            ...
        }
    }
}
```

Al bajar el método, esta parte del método [**GameHud:: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) dibuja nuestros rectángulos de movimiento y activación con [**ID2D1RenderTarget::D rawrectangle**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))y con la Cruz usando dos llamadas a [**ID2D1RenderTarget::D rawline**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline).

```cppwinrt
// Check if game is playing
if (game->IsActivePlay())
{
    // Draw a rectangle for the touch input for the move control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            0.0f,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            GameUIConstants::TouchRectangleSize,
            windowBounds.Height
            ),
        m_textBrush.get()
        );
    // Draw a rectangle for the touch input for the fire control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            windowBounds.Width - GameUIConstants::TouchRectangleSize,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            windowBounds.Width,
            windowBounds.Height
            ),
        m_textBrush.get()
        );

    // Draw the cross hairs
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f - GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        D2D1::Point2F(windowBounds.Width / 2.0f + GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        m_textBrush.get(),
        3.0f
        );
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f -
            GameUIConstants::CrossHairHalfSize),
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f +
            GameUIConstants::CrossHairHalfSize),
        m_textBrush.get(),
        3.0f
        );
}
```

En el método **GameHud:: Render** , se almacena el tamaño lógico de la ventana de juego en la `windowBounds` variable. Utiliza el [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) método de la clase **DeviceResources** . 

```cppwinrt
auto windowBounds = m_deviceResources->GetLogicalSize();
```

La obtención del tamaño de la ventana de juego es esencial para la programación de la interfaz de usuario. El tamaño de la ventana se proporciona en una medida denominada DIP (píxeles independientes del dispositivo), donde una DIP se define como 1/96 de una pulgada. Direct2D escala las unidades de dibujo a los píxeles reales cuando se produce el dibujo, para lo que se usa el valor de puntos por pulgada (PPP) de Windows. Del mismo modo, cuando se dibuja texto mediante [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal), se especifican DIP en lugar de puntos para el tamaño de la fuente. Los DIP se expresan como números de punto flotante. 

### <a name="displaying-game-state-info"></a>Mostrar información de estado de juego

Además de la pantalla de cabeza, el juego de ejemplo tiene una superposición que representa seis Estados de juego. Todos los Estados incluyen una gran primitiva de rectángulo negro con texto que el jugador debe leer. Los rectángulos del controlador de movimiento y la Cruz no se dibujan porque no están activos en estos Estados.

La superposición se crea con la clase [**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) , lo que nos permite desactivar el texto que se muestra para alinearlo con el estado del juego.

![Estado y acción de la superposición](images/simple-dx-game-ui-finaloverlay.png)

La superposición se divide en dos secciones: **Estado** y **acción**. El **Estado** intersecton se divide aún más en rectángulos de **título** y **cuerpo** . La sección de **acción** solo tiene un rectángulo. Cada rectángulo tiene un propósito diferente.

-   `titleRectangle` contiene el texto del título.
-   `bodyRectangle` contiene el texto del cuerpo.
-   `actionRectangle` contiene el texto que informa al reproductor de que realice una acción específica.

El juego tiene seis Estados que se pueden establecer. El estado del juego se ha transmitido con la parte de **Estado** de la superposición. Los rectángulos de **Estado** se actualizan mediante una serie de métodos que se corresponden con los Estados siguientes.

- Carga
- Estadísticas de inicio y puntuación iniciales
- Inicio de nivel
- Juego en pausa
- Juego terminado
- Juego ganado


La parte de **acción** de la superposición se actualiza mediante el método [**GameInfoOverlay:: SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) , lo que permite que el texto de la acción se establezca en uno de los siguientes.
- "Pulse para reproducirlo de nuevo..."
- "Cargando nivel, espere..."
- "Pulse para continuar..."
- None

> [!NOTE]
> Ambos métodos se tratarán con más detalle en la sección [representación del estado del juego](#representing-game-state) .

En función de lo que esté ocurriendo en el juego, se ajustan los campos de texto de la sección de **acción** y el **Estado** .
Echemos un vistazo a cómo inicializamos y dibujamos la superposición de estos seis Estados.

### <a name="initializing-and-drawing-the-overlay"></a>Inicialización y dibujo de la superposición

Los seis Estados de **Estado** tienen algunas cosas en común, por lo que los recursos y métodos que necesitan son muy similares.
    - Todos usan un rectángulo negro en el centro de la pantalla como fondo.
    - El texto mostrado es el **título** o el texto **del cuerpo** .
    - El texto utiliza la fuente Segoe UI y se dibuja sobre el rectángulo posterior. 


El juego de ejemplo tiene cuatro métodos que entran en juego al crear la superposición.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
El constructor [**GameInfoOverlay:: GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) inicializa la superposición, manteniendo la superficie de mapa de bits que se va a usar para mostrar información al reproductor. El constructor obtiene un generador del objeto [**ID2D1Device**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device) que se le ha pasado, que usa para crear un [**ID2D1DeviceContext**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) que puede dibujar el propio objeto de superposición. [IDWriteFactory::CreateTextFormat](/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay:: CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) es nuestro método para crear pinceles que se usarán para dibujar el texto. Para ello, obtenemos un objeto [**ID2D1DeviceContext2**](/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2) que permite la creación y el dibujo de geometría, además de funciones como la representación de la malla de tinta y el degradado. A continuación, creamos una serie de pinceles de color con [**ID2D1SolidColorBrush**](/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush) para dibujar los elementos de la interfaz de usuario de folling.
- Pincel negro para el fondo del rectángulo
- Pincel blanco para el texto de estado
- Pincel naranja para texto de acción

#### <a name="deviceresourcessetdpi"></a>DeviceResources:: SetDpi

El método [**DeviceResources:: SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) establece los puntos por pulgada de la ventana. Se llama a este método cuando se cambia el PPP y es necesario reajustarlo, lo que sucede cuando se cambia el tamaño de la ventana del juego. Después de actualizar el PPP, este método también llama a[**DeviceResources:: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) para asegurarse de que los recursos necesarios se vuelvan a crear cada vez que se cambie el tamaño de la ventana.

#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources

El método [**GameInfoOverlay:: CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) es el lugar en el que se realiza todo el dibujo. A continuación se explican los pasos del método.
- Se crean tres rectángulos para deshacer la sección del texto de la interfaz de usuario del **título**, el **cuerpo**y el texto de la **acción** .
    ```cppwinrt 
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

- Se crea un mapa de bits con `m_levelBitmap` el nombre, tomando en cuenta el PPP actual con **CreateBitmap**.
- `m_levelBitmap` se establece como el destino de representación 2D mediante [**ID2D1DeviceContext:: SetTarget**](/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget).
- El mapa de bits se borra con todos los píxeles que se han creado en negro mediante [**ID2D1RenderTarget:: Clear**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear).
- Se llama a [**ID2D1RenderTarget:: BeginDraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) para iniciar el dibujo. 
- Se llama a **DrawText** para dibujar el texto almacenado en `m_titleString` , `m_bodyString` y `m_actionString` en el rectángulo approperiate con el **ID2D1SolidColorBrush**correspondiente.
- Se llama a [**ID2D1RenderTarget:: EndDraw**](ID2D1RenderTarget::EndDraw) para detener todas las operaciones de dibujo en `m_levelBitmap` .
- Se crea otro mapa de bits mediante **CreateBitmap** denominado `m_tooSmallBitmap` para usarlo como reserva, que solo se muestra si la configuración de pantalla es demasiado pequeña para el juego.
- Repita el proceso de dibujo en `m_levelBitmap` para `m_tooSmallBitmap` , esta vez solo dibuje la cadena `Paused` en el cuerpo.




Ahora todo lo que necesitamos es seis métodos para rellenar el texto de nuestros seis Estados superpuestos.

### <a name="representing-game-state"></a>Representación del estado del juego


Cada uno de los seis Estados superpuestos del juego tiene un método correspondiente en el objeto **GameInfoOverlay** . Estos métodos dibujan una variación de la superposición para comunicar información explícita al jugador acerca del propio juego. Esta comunicación se representa con una cadena de **título** y **cuerpo** . Dado que el ejemplo ya configuró los recursos y el diseño para esta información cuando se inicializó y con el método [**GameInfoOverlay:: CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) , solo tiene que proporcionar las cadenas específicas del estado de superposición.

La parte de **Estado** de la superposición se establece con una llamada a uno de los métodos siguientes.

Estado del juego | Método de conjunto de estado | Campos de estado
:----- | :------- | :---------
Carga | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Título**</br>Carga de recursos </br>**Cuerpo**</br> Imprime de forma incremental "." para implicar la carga de la actividad.
Estadísticas de inicio y puntuación iniciales | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Título**</br>Puntuación alta</br> **Cuerpo**</br> Niveles completados # </br>Total de puntos #</br>Total de capturas #
Inicio de nivel | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Título**</br>Dosis #</br>**Cuerpo**</br>Descripción del objetivo de nivel.
Juego en pausa | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Título**</br>Juego en pausa</br>**Cuerpo**</br>None
Juego terminado | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Título**</br>Fin</br> **Cuerpo**</br> Niveles completados # </br>Total de puntos #</br>Total de capturas #</br>Niveles completados #</br>Puntuación alta #
Juego ganado | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Título**</br>¡ GANÓ!</br> **Cuerpo**</br> Niveles completados # </br>Total de puntos #</br>Total de capturas #</br>Niveles completados #</br>Puntuación alta #

Con el método [**GameInfoOverlay:: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) , el ejemplo declara tres áreas rectangulares que corresponden a regiones específicas de la superposición.

Teniendo en cuenta estas áreas, veamos uno de los métodos específicos de estado, **GameInfoOverlay::SetGameStats**, y cómo se dibuja la superposición.

```cppwinrt
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.get());
    m_titleString = L"High Score";

    d2dContext->DrawText(
        m_titleString.c_str(),
        m_titleString.size(),
        m_textFormatTitle.get(),
        m_titleRectangle,
        m_textBrush.get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = std::wstring(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString.c_str(),
        m_bodyString.size(),
        m_textFormatBody.get(),
        m_bodyRectangle,
        m_textBrush.get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device. All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call. At that point, the sample will recreate the device
        // and all associated resources. As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        winrt::check_hresult(hr);
    }
}
```

Mediante el contexto de dispositivo de Direct2D en el que se ha inicializado el objeto **GameInfoOverlay** , este método rellena los rectángulos de título y cuerpo con negro mediante el pincel de fondo. Dibuja el texto para la cadena "Máxima puntuación" en el rectángulo de título y una cadena que contiene la información de estado del juego actualizada en el rectángulo de cuerpo usando el pincel de texto blanco.


El rectángulo de acción se actualiza mediante una llamada subsiguiente a [**GameInfoOverlay:: SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) desde un método en el objeto **GameMain** , que proporciona la información de estado de juego que **GameInfoOverlay:: SetAction** necesita para determinar el mensaje correcto al reproductor, como "TAP para continuar".

La superposición de cualquier estado determinado se elige en el método [**GameMain:: SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) de la siguiente manera:

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
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

Ahora el juego tiene una forma de comunicar la información de texto al jugador en función del estado del juego, y tenemos una forma de cambiar lo que se muestra en todo el juego.

### <a name="next-steps"></a>Pasos siguientes

En el siguiente tema, [Agregar controles](tutorial--adding-controls.md), veremos cómo interactúa el jugador con el juego de ejemplo y cómo la entrada cambia el estado del juego.