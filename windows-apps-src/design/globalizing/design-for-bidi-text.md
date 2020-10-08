---
description: Diseñe la aplicación para proporcionar compatibilidad de texto bidireccional (BiDi) para poder combinar el script de los sistemas de escritura de izquierda a derecha (LTR) y de derecha a izquierda (RTL), que generalmente contienen distintos tipos de alfabetos.
title: Diseñar la aplicación para texto bidireccional
template: detail.hbs
ms.date: 11/10/2017
ms.topic: article
keywords: Windows 10, UWP, globalización, localizabilidad, localización, RTL, LTR
ms.localizationpriority: medium
ms.openlocfilehash: f09aa1ac2b56c83b502e54ce631e46d2f4054943
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829602"
---
# <a name="design-your-app-for-bidirectional-text"></a>Diseñar la aplicación para texto bidireccional

Diseñe la aplicación para proporcionar compatibilidad de texto bidireccional (BiDi) para poder combinar el script de derecha a izquierda (RTL) y los sistemas de escritura de izquierda a derecha (LTR), que normalmente contienen distintos tipos de alfabetos.

Los sistemas de escritura de derecha a izquierda, como los que se usan en Oriente Medio, centro y sur de Asia, y en África, tienen requisitos de diseño únicos. Estos sistemas de escritura requieren compatibilidad de texto bidireccional (BiDi). La compatibilidad bidireccional es la capacidad de escribir y mostrar el diseño de texto de derecha a izquierda (RTL) o de izquierda a derecha (LTR).

Windows incluye un total de nueve idiomas bidireccionales.
- Dos idiomas totalmente localizados. Árabe y hebreo.
- Siete paquetes de interfaz de idioma para los mercados emergentes. Persa, Urdu, dari, central kurdo, Sindhi, punjabí (Pakistán) y uigur.

Este tema contiene la filosofía de diseño BiDi de Windows y casos prácticos que muestran las consideraciones de diseño bidireccionales.

## <a name="bidi-design-elements"></a>Elementos de diseño bidi

Cuatro elementos influyen en decisiones de diseño bidireccionales en Windows.

- **Creación de reflejo de la interfaz de usuario (IU)**. El flujo de la interfaz de usuario permite presentar el contenido de derecha a izquierda en su diseño nativo. El diseño de la interfaz de usuario se siente en mercados bidireccionales.
- **Coherencia en la experiencia del usuario**. El diseño resulta natural en la orientación de derecha a izquierda. Los elementos de interfaz de usuario comparten una dirección de diseño coherente y aparecen cuando el usuario los espera.
- **Optimización táctil**. Similar a la interfaz de usuario táctil en la interfaz de usuario no reflejada, los elementos son fáciles de alcanzar y son naturales para la interacción táctil.
- **Compatibilidad con texto mixto**. La compatibilidad con la direccionalidad de texto permite una gran presentación de texto mixto (texto en inglés en compilaciones bidireccionales y viceversa).

## <a name="feature-design-overview"></a>Información general sobre el diseño de características

Windows admite los cuatro elementos de diseño bidireccionales. Echemos un vistazo a algunas de las principales características importantes de Windows y proporcione algún contexto sobre cómo afectan a la aplicación.

### <a name="navigate-in-the-direction-that-feels-natural"></a>Navegue en la dirección que se sienta natural

Windows ajusta la dirección de la cuadrícula tipográfica para que fluya de derecha a izquierda, lo que significa que el primer mosaico de la cuadrícula se coloca en la esquina superior derecha y el último mosaico en la parte inferior izquierda. Esto coincide con el patrón RTL de publicaciones impresas como libros y revistas, donde el patrón de lectura siempre comienza en la esquina superior derecha y progresa hacia la izquierda.

![Menú de inicio bidireccional menú ](images/56283_BIDI_01_startscreen_resized.png)
 ![ Inicio bidireccional con accesos](images/56283_BIDI_02_startscreen_charm_resized.png)

Para conservar un flujo de interfaz de usuario coherente, el contenido de los mosaicos conserva un diseño de derecha a izquierda, lo que significa que el nombre y el logotipo de la aplicación se colocan en la esquina inferior derecha del icono, independientemente del idioma de la interfaz de usuario de la aplicación.

#### <a name="bidi-tile"></a>Icono BiDi

![Icono BiDi](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>Icono de inglés

![Icono de inglés](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>Obtención de notificaciones de icono que se leen correctamente

Los mosaicos tienen compatibilidad con texto mixto. La región de notificación tiene flexibilidad integrada para ajustar la alineación del texto en función del lenguaje de notificación.  Cuando una aplicación envía las notificaciones de idioma árabe, hebreo u otra BiDi, el texto se alinea a la derecha. Y cuando llegue una notificación en inglés (u otro LTR), se alineará a la izquierda.

![Notificaciones de icono](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>Una experiencia de usuario de RTL coherente y fácil de tocar

Todos los elementos de la interfaz de usuario de Windows se ajustan a la orientación RTL. Los accesos y controles flotantes se han colocado en el borde izquierdo de la pantalla para que no se superpongan a los resultados de la búsqueda ni se reduzca la optimización táctil. Pueden llegar fácilmente a los Thumbs.

![Captura de pantalla de BiDi en la que se muestra la ](images/56286_BIDI_05_search_flyout_resized.png)
 ![ captura de pantalla de la búsqueda de control flotante cuyo tamaño se ha cambiado](images/56286_BIDI_06_print_flyout_resized.png)

![Captura de pantalla de BiDi que muestra la ](images/56286_BIDI_07_settings_flyout_resized.png)
 ![ captura de pantalla de configuración cuyo tamaño se ha cambiado](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>Entrada de texto en cualquier dirección

Windows ofrece un teclado táctil en pantalla limpio y sin problemas. En el caso de los idiomas bidireccionales, hay una clave de control de dirección del texto para que se pueda cambiar la dirección de entrada de texto según sea necesario.

![Teclado táctil para lenguaje bidireccional](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>Usar cualquier aplicación en cualquier lenguaje

Instale y use sus aplicaciones favoritas en cualquier idioma. Las aplicaciones aparecen y funcionan como lo harían en versiones no bidireccionales de Windows. Los elementos de las aplicaciones siempre se colocan en una posición coherente y predecible.

![Aplicación en inglés que muestra contenido bidireccional](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>Mostrar los paréntesis correctamente

Con la introducción del algoritmo de paréntesis de BiDi (BPA), los paréntesis emparejados siempre se muestran correctamente, independientemente de las propiedades de idioma o de alineación de texto.

#### <a name="incorrect-parentheses"></a>Paréntesis incorrectos

![Aplicación BiDi con paréntesis incorrectos](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>Paréntesis correctos

![Aplicación BiDi con paréntesis correctos](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>Tipografía

Windows usa la fuente Segoe UI para todos los idiomas bidireccionales. Esta fuente se forma y se escala para la interfaz de usuario de Windows.

![Captura de pantalla que muestra la Segoe UI fuente en la captura de pantalla de inicio ](images/56290_BIDI_13_start_screen_segoe.png)
 ![ que muestra la fuente Segoe Arabic en la pantalla Inicio](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>Caso práctico #1: una aplicación de música bidireccional

### <a name="overview"></a>Información general

Las aplicaciones multimedia realizan un desafío de diseño muy interesante, ya que, por lo general, se espera que los controles multimedia tengan un diseño de izquierda a derecha similar al de los idiomas no bidireccionales.

![Controles multimedia de izquierda a derecha](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![Controles multimedia de derecha a izquierda](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>Establecimiento de la direccionalidad de la interfaz de usuario

Conservar el flujo de la interfaz de usuario de derecha a izquierda es importante para un diseño coherente para los mercados bidireccionales. La adición de elementos que tienen un flujo de izquierda a derecha dentro de este contexto es difícil, porque algunos elementos de navegación, como el botón atrás, pueden contradicción de la orientación direccional del botón atrás en los controles de audio.

![Página de seguimiento de la aplicación de música](images/56292_BIDI_16_app_layout_callouts_resized.png)

Esta aplicación de música conserva una cuadrícula orientada de derecha a izquierda. Esto proporciona a la aplicación un aspecto muy natural para los usuarios que ya navegan en esta dirección a través de la interfaz de usuario de Windows. El flujo se conserva asegurándose de que los elementos principales no se acaban de ordenar de derecha a izquierda, sino que también se alinean correctamente en los encabezados de la sección para ayudar a mantener el flujo de la interfaz de usuario.

![Página de álbum de aplicación de música](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>Control de texto

La biografía del artista en la captura de pantalla anterior se alinea a la izquierda, mientras que otras partes de texto relacionadas con el artista como el álbum y los nombres de pista conservan la alineación derecha. El campo bio es un elemento de texto bastante grande, que se lee mal cuando se alinea a la derecha simplemente porque es difícil realizar un seguimiento entre las líneas mientras se lee un bloque de texto más amplio. En general, se deben tener en cuenta cualquier elemento de texto con más de dos o tres líneas que contengan cinco o más palabras para las excepciones de alineación similares, donde la alineación del bloque de texto es opuesta a la del diseño direccional general de la aplicación.

Manipular la alineación en la aplicación puede parecer sencillo, pero a menudo expone algunos de los límites y las limitaciones de los motores de representación en términos de colocación de caracteres neutros en cadenas BiDi. Por ejemplo, la siguiente cadena puede mostrarse de manera diferente en función de la alineación.

| | Cadena en inglés (LTR) | Cadena hebrea (RTL) |
| -------------- | ------------------- | ------------------- |
| **Alineación a la izquierda** | Hola mundo. | בוקר טוב! |
| **Alineación a la derecha** | ! Hola mundo | !בוקר טוב |

Para asegurarse de que la información de intérprete se muestra correctamente en la aplicación de música, las propiedades de diseño de texto separadas por el equipo de desarrollo de la alineación. En otras palabras, la información del artista podría mostrarse alineada a la derecha en muchos de los casos, pero el ajuste del diseño de la cadena se establece en función del procesamiento en segundo plano personalizado. El procesamiento en segundo plano determina la configuración de diseño direccional más adecuada según el contenido de la cadena.

![Página de intérprete de aplicación de música](images/56292_BIDI_18_app_layout_callouts_resized.png)

Por ejemplo, sin procesar el diseño de cadena personalizado, el nombre del intérprete es "la banda de Contoso". aparecería como ". Banda de Contoso ".

### <a name="specialized-string-direction-preprocessing"></a>Preprocesamiento de dirección de cadena especializada

Cuando la aplicación se pone en contacto con el servidor para los metadatos de medios, procesa cada cadena antes de mostrarla al usuario. Durante este preprocesamiento, la aplicación también realiza una transformación para que la dirección del texto sea coherente. Para ello, comprueba si hay caracteres neutros en los extremos de la cadena. Además, si la dirección del texto de la cadena es opuesta a la dirección de la aplicación establecida por la configuración de idioma de Windows, anexa (y a veces antepone) marcadores de dirección Unicode. La función de transformación tiene este aspecto.

```csharp
string NormalizeTextDirection(string data) 
{
    if (data.Length > 0) {
        var lastCharacterDirection = DetectCharacterDirection(data[data.Length - 1]);

        // If the last character has strong directionality (direction is not null), then the text direction for the string is already consistent.
        if (!lastCharacterDirection) {
            // If the last character has no directionality (neutral character, direction is null), then we may need to add a direction marker to
            // ensure that the last character doesn't inherit directionality from the outside context.
            var appTextDirection = GetAppTextDirection(); // checks the <html> element's "dir" attribute.
            var dataTextDirection = DetectStringDirection(data); // Run through the string until a non-neutral character is encountered,
                                                                 // which determines the text direction.

            if (appTextDirection != dataTextDirection) {
                // Add a direction marker only if the data text runs opposite to the directionality of the app as a whole,
                // which would cause the neutral characters at the ends to flip.
                var directionMarkerCharacter =
                    dataTextDirection == TextDirections.RightToLeft ?
                        UnicodeDirectionMarkers.RightToLeftDirectionMarker : // "\u200F"
                        UnicodeDirectionMarkers.LeftToRightDirectionMarker; // "\u200E"

                data += directionMarkerCharacter;

                // Prepend the direction marker if the data text begins with a neutral character.
                var firstCharacterDirection = DetectCharacterDirection(data[0]);
                if (!firstCharacterDirection) {
                    data = directionMarkerCharacter + data;
                }
            }
        }
    }

    return data;
}
```

Los caracteres Unicode agregados son de ancho cero, por lo que no afectan al espaciado de las cadenas. Este código conlleva una posible penalización del rendimiento, ya que la detección de la dirección de una cadena requiere que se ejecute a través de la cadena hasta que se encuentre un carácter no neutro. Cada carácter cuya neutralidad se comprueba se compara primero con varios rangos Unicode, por lo que no es una comprobación trivial.

## <a name="case-study-2-a-bidi-mail-app"></a>Caso práctico #2: una aplicación de correo bidireccional

### <a name="overview"></a>Información general

En cuanto a los requisitos de diseño de la interfaz de usuario, un cliente de correo es bastante sencillo de diseñar. De forma predeterminada, la aplicación de correo electrónico de Windows está reflejada. Desde una perspectiva de control de texto, se requiere que la aplicación de correo cuente con capacidades de composición y presentación de texto más sólidas para acomodar escenarios de texto mixto.

### <a name="establishing-ui-directionality"></a>Establecimiento de la direccionalidad de la interfaz de usuario

El diseño de la interfaz de usuario de la aplicación de correo está reflejado. Los tres paneles se han reorientado para que el panel de carpetas se coloque en el borde derecho de la pantalla, seguido del panel lista de elementos de correo a la izquierda y, a continuación, el panel Composición de correo electrónico.

![Aplicación de correo reflejado](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

Los elementos adicionales se han reorientado para coincidir con el flujo de la interfaz de usuario general y la optimización táctil. Esto incluye la barra de la aplicación y los iconos de redacción, respuesta y eliminación.

![Aplicación de correo reflejada con la barra de la aplicación](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>Control de texto

#### <a name="ui"></a>UI

La alineación del texto a través de la interfaz de usuario suele estar alineada a la derecha. Esto incluye el panel de carpetas y el panel de elementos. El panel elemento está limitado a dos líneas de texto (dirección y título). Esto es importante para conservar la alineación de derecha a izquierda, sin introducir un bloque de texto que sería difícil de leer cuando la dirección del contenido es opuesta al flujo de la dirección de la interfaz de usuario.

#### <a name="text-editing"></a>Edición de texto

La edición de texto requiere la posibilidad de componer en el formulario de derecha a izquierda y de izquierda a derecha. Además, el diseño de composición debe conservarse con un formato &mdash; como texto enriquecido &mdash; que tenga la capacidad de guardar la información direccional.

![Aplicación de correo de izquierda a derecha](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![Aplicación de correo de derecha a izquierda](images/56294_BIDI_22_email_orientation_RtL_resized.png)
