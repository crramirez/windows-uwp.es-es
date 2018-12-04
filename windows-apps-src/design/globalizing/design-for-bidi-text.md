---
Description: Design your app to provide bidirectional text support (BiDi) so that you can combine script from left-to-right (LTR) and right-to-left (RTL) writing systems, which generally contain different types of alphabets.
title: Diseña tu aplicación para el texto bidireccional
template: detail.hbs
ms.date: 11/10/2017
ms.topic: article
keywords: windows 10, uwp, globalización, localizabilidad, localización, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: 66a158a96fcab5391030f4517b6420ba4585bf04
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8479196"
---
# <a name="design-your-app-for-bidirectional-text"></a>Diseña tu aplicación para el texto bidireccional

Diseña tu aplicación para proporcionar compatibilidad para texto bidireccional (BiDi), de modo que puedas combinar scripts con sistemas de escritura de derecha a izquierda (RTL) y de izquierda a derecha (LTR), que suelen contener distintos tipos de alfabetos.

Los sistemas de escritura de derecha a izquierda, como los que se usan en Oriente Medio, en el centro y el sur de Asia o en África, tienen requisitos de diseño únicos. Estos sistemas de escritura requieren compatibilidad para texto bidireccional (BiDi). La compatibilidad BiDi es la capacidad de introducir y mostrar diseño de texto en el orden de derecha a izquierda (RTL) o de izquierda a derecha (LTR).

En Windows hay un total de nueve idiomas BiDi incluidos.
- Dos idiomas totalmente localizados. Árabe y hebreo.
- Siete paquetes de interfaz de idiomas para los mercados emergentes. Persa, urdu, darí, kurdo central, sindhi, panyabí (Pakistán) y uigur.

Este artículo contiene la filosofía de diseño BiDi de Windows y casos prácticos que muestran consideraciones de diseño BiDi.

## <a name="bidi-design-elements"></a>Elementos de diseño BiDi

Hay cuatro elementos que determinan las decisiones sobre el diseño BiDi en Windows.

- **Creación de reflejos de la interfaz de usuario (UI)**. El flujo de la interfaz de usuario permite que el contenido de derecha a izquierda se presente en su formato nativo. El diseño de la interfaz de usuario se percibe como local en los mercados BiDi.
- **Coherencia en la experiencia del usuario**. El diseño tiene un aspecto natural en la orientación de derecha a izquierda. Los elementos de la interfaz de usuario comparten un diseño coherente y se muestran tal y como el usuario espera.
- **Optimización para entrada táctil**. Los elementos son fáciles de alcanzar y la interacción táctil es natural, del mismo modo que la experiencia táctil del usuario en las interfaces de usuario no reflejadas.
- **Compatibilidad con texto mixto**. La compatibilidad con la direccionalidad del texto permite una presentación óptima de los textos combinados (textos en inglés en compilaciones BiDi y viceversa).

## <a name="feature-design-overview"></a>Introducción al diseño de características

Windows admite los cuatro elementos de diseño BiDi. Veremos algunas de las características más importantes en Windows y te proporcionaremos el contexto sobre cómo pueden afectar a tu aplicación.

### <a name="navigate-in-the-direction-that-feels-natural"></a>Navega en la dirección que te resulte más natural

Windows ajusta la dirección de la cuadrícula tipográfica para que fluya de derecha a izquierda, lo que significa que el primer icono de la cuadrícula está situado en la esquina superior derecha, y el último, en la esquina inferior izquierda. Esto encaja con el patrón RTL de las publicaciones impresas, como libros y revistas, en las que el patrón de lectura siempre comienza en la esquina superior derecha y continúa hacia la izquierda.

![Menú de inicio BiDi](images/56283_BIDI_01_startscreen_resized.png)
![Menú de inicio BiDi con accesos](images/56283_BIDI_02_startscreen_charm_resized.png)

Para mantener un flujo coherente de interfaz de usuario, el contenido de los iconos mantiene una distribución de derecha a izquierda, lo que significa que el nombre y el logotipo de la aplicación se encuentran en la esquina inferior derecha del icono, independientemente del idioma de la interfaz de usuario de la aplicación.

#### <a name="bidi-tile"></a>Icono en BiDi

![Icono en BiDi](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>Icono en inglés

![Icono en inglés](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>Obtén notificaciones de icono que se lean correctamente

Los iconos admiten texto mixto. La región de notificación dispone de flexibilidad integrada para ajustar la alineación del texto en función del idioma de la notificación.  Cuando una aplicación envía notificaciones en árabe, hebreo u otros idiomas BiDi, el texto se alinea a la derecha. Y cuando llega una notificación en inglés (o cualquier otro LTR), se alinea a la izquierda.

![Notificaciones de icono](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>Facilidad táctil en una experiencia de usuario coherente para los idiomas de derecha a izquierda.

Todos los elementos de la interfaz de usuario de Windows se ajustan a la orientación RTL. Los accesos y los controles flotantes se han colocado en el borde izquierdo de la pantalla para que no se superpongan a los resultados de búsqueda ni afecten a la optimización de la entrada táctil. Pueden alcanzarse fácilmente con los pulgares.

![Captura de pantalla BiDi](images/56286_BIDI_05_search_flyout_resized.png)
![Captura de pantalla BiDi](images/56286_BIDI_06_print_flyout_resized.png)

![Captura de pantalla BiDi](images/56286_BIDI_07_settings_flyout_resized.png)
![Captura de pantalla BiDi](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>Entrada de texto en cualquier dirección

Windows ofrece un teclado táctil en pantalla nítido y sencillo. Para los idiomas BiDi, hay una tecla de control de dirección de texto de modo que la dirección de la entrada de texto se pueda alternar según se necesite.

![Teclado táctil para idiomas BiDi](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>Usa cualquier aplicación en cualquier idioma

Instala tus aplicaciones preferidas y úsalas en cualquier idioma. Las aplicaciones tienen el aspecto y la funcionalidad que tendrían en las versiones de Windows que no son BiDi. Los elementos dentro de las aplicaciones siempre se colocan en una posición coherente y predecible.

![Aplicación en inglés que muestra contenido BiDi.](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>Muestra los paréntesis correctamente

Con la introducción del algoritmo de paréntesis BiDi (BPA), los pares de paréntesis siempre se muestran correctamente, independientemente de las propiedades de idioma o de la alineación del texto.

#### <a name="incorrect-parentheses"></a>Paréntesis incorrecto

![Aplicación BiDi con paréntesis incorrecto](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>Paréntesis correcto

![Aplicación BiDi con paréntesis correcto](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>Tipografía

Windows usa la fuente Segoe UI para todos los idiomas BiDi. La forma y la escala de esta fuente están adaptadas a la interfaz de usuario de Windows.

![Fuente Segoe UI para idioma BiDi](images/56290_BIDI_13_start_screen_segoe.png)
![Fuente Segoe UI para idioma BiDi](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>Caso práctico 1: una aplicación de música en idioma BiDi

### <a name="overview"></a>Introducción

Las aplicaciones multimedia presentan un desafío de diseño muy interesante ya que, por lo general, se espera que los controles multimedia tengan un diseño de izquierda a derecha similar al de los idiomas que no son BiDi.

![Controles multimedia de izquierda a derecha](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![Controles multimedia de derecha a izquierda](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>Establecer la direccionalidad de la interfaz de usuario

Conservar el flujo de la interfaz de usuario de derecha a izquierda es importante para un diseño coherente en los mercados con idiomas BiDi. Incluir elementos con un flujo de izquierda a derecha en este contexto es difícil porque algunos elementos de navegación, como el botón Atrás, pueden contradecir la orientación direccional del botón Atrás en los controles de audio.

![Página de la pista en una aplicación de música](images/56292_BIDI_16_app_layout_callouts_resized.png)

Esta aplicación de música conserva una cuadrícula orientada de derecha a izquierda. Esto le da a la aplicación una apariencia muy natural para los usuarios que ya navegan en esta dirección en toda la interfaz de usuario de Windows. El flujo se mantiene garantizando que los elementos principales no estén simplemente ordenados de derecha a izquierda, sino que también estén correctamente alineados en los encabezados de sección para ayudar a mantener el flujo de la interfaz de usuario.

![Página de un álbum en una aplicación de música](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>Control de texto

La biografía del intérprete en la captura de pantalla anterior está alineada a la izquierda, mientras que otros fragmentos de texto relacionados, como los nombres del álbum y de la pista, mantienen la alineación a la derecha. El campo de la biografía es un elemento de texto de tamaño considerable, que resulta difícil de leer cuando está alineado a la derecha porque cuesta seguir las líneas cuando se lee un bloque de texto más ancho. En general, deben considerarse excepciones de alineación como esta en cualquier elemento de texto de dos o tres líneas con cinco o más palabras, cuando la alineación del bloque de texto es opuesta a la del diseño direccional general de la aplicación.

La manipulación de la alineación en toda la aplicación puede parecer sencilla pero, con frecuencia, pone en evidencia las limitaciones de los motores de representación en cuanto a la ubicación del carácter neutro en las cadenas con idiomas BiDi. Por ejemplo, la siguiente cadena puede mostrarse de diferentes maneras según la alineación.

| | Cadena en inglés (LTR) | Cadena en hebreo (RTL) |
| -------------- | ------------------- | ------------------- |
| **Alineación a la izquierda** | Hello, World! | בוקר טוב! |
| **Alineación a la derecha** | !Hello, World | !בוקר טוב |

Para garantizar que la información del intérprete se muestre correctamente en la aplicación de música, el equipo de desarrollo separó las propiedades de diseño de texto de la alineación. En otras palabras, la información del intérprete probablemente se muestra como alineada a la derecha en muchos de los casos, pero el ajuste de diseño de la cadena se establece sobre la base del procesamiento en segundo plano personalizado. El procesamiento en segundo plano determina la mejor configuración de diseño direccional sobre la base del contenido de la cadena.

![Página del intérprete en una aplicación de música](images/56292_BIDI_18_app_layout_callouts_resized.png)

Por ejemplo, sin procesamiento de diseño de cadenas personalizado, el nombre del intérprete "The Contoso Band." aparecería como ".The Contoso Band"

### <a name="specialized-string-direction-preprocessing"></a>Preprocesamiento especializado de la dirección de la cadena

Cuando la aplicación contacta con el servidor en busca de metadatos multimedia, preprocesa cada cadena antes de mostrarla al usuario. Durante este preprocesamiento, la aplicación también realiza una transformación para dar coherencia a la dirección del texto. Para hacer esto, comprueba si existen caracteres neutros en los extremos de la cadena. Además, si la dirección del texto de la cadena es opuesta a la dirección de la aplicación establecida por la configuración de idiomas de Windows, anexa (y en ocasiones antepone) marcadores de dirección de Unicode. La función de transformación tiene este aspecto.

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

Los caracteres Unicode agregados son de ancho cero y, como resultado, no afectan el espaciado de las cadenas. Este código ocasiona una disminución del rendimiento potencial porque, para detectar la dirección de una cadena, es necesario recorrerla hasta encontrar un carácter que no sea neutro. Para detectar la neutralidad de cada carácter, primero se compara con varios intervalos Unicode, de modo que no es una comprobación trivial.

## <a name="case-study-2-a-bidi-mail-app"></a>Caso práctico 2: una aplicación de correo en idioma BiDi

### <a name="overview"></a>Introducción

En cuanto a los requisitos de diseño de la interfaz de usuario, un cliente de correo es relativamente fácil de diseñar. La aplicación Mail en Windows está reflejada de manera predeterminada. Desde una perspectiva de control de texto, la visualización de texto y las funcionalidades de redacción de la aplicación de correo deben ser más sólidas para adaptarse a escenarios de texto mixto.

### <a name="establishing-ui-directionality"></a>Establecer la direccionalidad de la interfaz de usuario

El diseño de la interfaz de usuario de la aplicación Mail está reflejado. Los tres paneles se reorientaron de modo que el panel de carpetas se encontrase en el margen derecho de la pantalla, seguido del panel de lista de elementos de correo a la izquierda y el panel de redacción de correo electrónico.

![Aplicación de correo reflejada](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

Los elementos adicionales se reorientaron para que coincidiesen con la optimización táctil y de flujo de la interfaz de usuario general. Esto incluye la barra de la aplicación y los iconos de eliminación, respuesta y redacción.

![Aplicación de correo reflejada con barra de la aplicación](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>Control de texto

#### <a name="ui"></a>Interfaz de usuario

La alineación de texto a través de la interfaz de usuario generalmente está alineada a la derecha. Esta incluye el panel de elementos y el panel de carpetas. El panel de elementos está limitado a dos líneas de texto (dirección y título). Esto es importante para conservar la alineación de derecha a izquierda sin introducir un bloque de texto que sería difícil de leer cuando la dirección del contenido es la opuesta al flujo de dirección de la interfaz de usuario.

#### <a name="text-editing"></a>Edición de texto

La edición de texto requiere la capacidad de componer tanto de derecha a izquierda como de izquierda a derecha. Además, debe mantenerse el diseño de la redacción mediante un formato&mdash;, como el texto enriquecido&mdash;, que permite guardar información sobre la dirección.

![Aplicación de correo de izquierda a derecha](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![Aplicación de correo de derecha a izquierda](images/56294_BIDI_22_email_orientation_RtL_resized.png)
