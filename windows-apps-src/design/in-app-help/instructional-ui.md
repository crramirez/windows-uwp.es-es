---
Description: Diseñar una interfaz de usuario con instrucciones (UI) que enseña a los usuarios a trabajar con su aplicación para UWP.
title: Directrices para diseñar una interfaz de usuario informativa
label: Instructional UI
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: c87e2f06-339d-4413-b585-172752964f56
ms.localizationpriority: medium
ms.openlocfilehash: b39507fb1333fdb642601c6b4828c3d160c6ceb5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656440"
---
# <a name="instructional-ui-guidelines"></a>Directrices para una interfaz de usuario informativa



En ocasiones puede resultar útil informar al usuario acerca de las funciones de la aplicación que puedan no resultar obvias para ellos, tales como las interacciones táctiles específicas. En estos casos, debes presentar instrucciones a través de la interfaz de usuario para que puedan usar esas características que posiblemente no conozcan.

## <a name="when-to-use-instructional-ui"></a>Cuándo usar la interfaz de usuario informativa

La interfaz de usuario informativa debe usarse con cuidado. Cuando se abusa de ella, es fácil pasarla por alto o puede resultar molesta para el usuario, lo que hará que pierda su eficacia.

La interfaz de usuario informativa debería usarse para ayudar al usuario a descubrir características importantes de aplicación que no son evidentes, como los gestos táctiles o ajustes que podrían interesarles. También puede usarse para informar a los usuarios sobre características nuevas o cambios en la aplicación que, de lo contrario, pueden pasar desapercibidos.

A menos que la aplicación dependa de los gestos táctiles, la interfaz de usuario informativa no debe usarse para enseñar al usuario las características fundamentales de la aplicación.

## <a name="principles-of-writing-instructional-ui"></a>Principios del diseño de la UI informativa

Una buena interfaz de usuario informativa es pertinente y educativa para el usuario y mejora su experiencia. Debe ser:

-   **Simple:** Los usuarios no desean que su experiencia se interrumpa con información complicada
-   **Fácil de recordar:** Los usuarios no desean ver las mismas instrucciones cada vez que intente realizar una tarea, por lo que deben ser algo que recordará instrucciones.
-   **Inmediatamente relevante:** Si la interfaz de usuario con instrucciones no enseña un usuario sobre algo que desean hacer inmediatamente, no tienen un motivo para prestar atención a él.

Evita el uso excesivo de la interfaz de usuario informativa y asegúrate de elegir los temas correctos. No incluyas la siguiente información:

-   **Características fundamentales:** Si un usuario necesita instrucciones para usar la aplicación, considere la posibilidad de realizar el diseño de aplicaciones más intuitiva.
-   **Características obvias:** Si un usuario puede descubrir una característica por sí solos sin una instrucción, la interfaz de usuario con instrucciones sólo obtendrá de la forma.
-   **Características complejas:** La interfaz de usuario con instrucciones debe ser conciso y los usuarios interesados en características complejas normalmente dispuestos a buscan instrucciones y no es necesario que se les han concedido.

Evita que la interfaz de usuario informativa suponga una molestia para los usuarios. No hagas lo siguiente:

-   **Información importante poco claros:** Nunca debe obtener la interfaz de usuario con instrucciones obstáculo otras características de la aplicación.
-   **Forzar a los usuarios participar:** Usuarios deben ser capaces de ignorar la interfaz de usuario con instrucciones y todavía el progreso a través de la aplicación.
-   **Mostrar información de repetición:** No acosar al usuario con la interfaz de usuario con instrucciones, aunque ignorarla por primera vez. La mejor solución es agregar una opción de configuración para volver a mostrar la interfaz de usuario informativa.

## <a name="examples-of-instructional-ui"></a>Ejemplos de interfaz de usuario informativa

Aquí te mostramos algunos ejemplos en los que la interfaz de usuario informativa puede ayudar a los usuarios:

-   **Ayuda a los usuarios descubrir las interacciones táctiles.** En la siguiente captura de pantalla se muestra la interfaz de usuario informativa en la que se enseña a un jugador a usar los gestos táctiles en el juego Cut the Rope.

    ![Captura de pantalla del juego que muestra un mensaje de la UI informativa, "Slide across to cut the rope" (Desliza el dedo para cortar la cuerda).](images/in-game-controls-3.png)

-   **Realizar una primera impresión excelente.** Cuando la aplicación Momentos especiales se inicia por primera vez, la interfaz de usuario informativa pide al usuario que comience a crear películas sin obstaculizar su experiencia.

    ![Pantalla de inicio de la aplicación Momentos especiales](images/instructional-ui-movie.png)

-   **Guiar a los usuarios para avanzar al paso siguiente en una tarea complicada.** En la aplicación Correo de Windows, una sugerencia en la parte inferior de la bandeja de entrada dirige a los usuarios a **Configuración** para acceder a los mensajes más antiguos.

    ![Captura de pantalla recortada de la aplicación Correo de Windows que muestra un mensaje de la interfaz de usuario informativa](images/instructional-ui-mail-inbox.png)

    Cuando el usuario hace clic en el mensaje, el control flotante **Configuración** de la aplicación aparece en el lado derecho de la pantalla y permite al usuario completar la tarea. En estas imágenes se muestra la aplicación Mail antes y después de que un usuario haga clic en el mensaje de la interfaz de usuario informativa.

    | Antes                                                               | Después                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Captura de pantalla de la aplicación Correo de Windows](images/instructional-ui-mail.png) | ![Captura de pantalla de la aplicación Correo de Windows con un control flotante Configuración extendido](images/instructional-ui-mail-flyout.png) |

## <a name="related-articles"></a>Artículos relacionados

* [Directrices para obtener ayuda de la aplicación](guidelines-for-app-help.md)
