---
Description: Diseñar una interfaz de usuario informativa que enseñe a los usuarios cómo funciona la aplicación de la Tienda Windows.
title: Directrices para diseñar una interfaz de usuario informativa
label: Instructional UI
template: detail.hbs
---

# Directrices para una interfaz de usuario informativa

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En ocasiones puede resultar útil informar al usuario acerca de las funciones de la aplicación que puedan no resultar obvias para ellos, tales como las interacciones táctiles específicas. En estos casos, debes presentar instrucciones a través de la interfaz de usuario para que puedan usar esas características que posiblemente no conozcan.

## <span id="when_to_use_instructional_ui"></span><span id="WHEN_TO_USE_INSTRUCTIONAL_UI"></span>Cuándo usar la interfaz de usuario informativa

La interfaz de usuario informativa debe usarse con cuidado. Cuando se abusa de ella, es fácil pasarla por alto o puede resultar molesta para el usuario, lo que hará que pierda su eficacia.

La interfaz de usuario informativa debería usarse para ayudar al usuario a descubrir características de aplicación importantes y que no son evidentes, como los gestos táctiles o configuración en la que podrían estar interesados. También puede usarse para informar a los usuarios sobre características nuevas o cambios en la aplicación que, de lo contrario, pueden pasar desapercibidos.

A menos que la aplicación dependa de los gestos táctiles, la interfaz de usuario informativa no debe usarse para enseñar al usuario las características fundamentales de la aplicación.

## <span id="writing_instructional_ui"></span><span id="WRITING_INSTRUCTIONAL_UI"></span>Principios del diseño de la UI informativa

Una buena interfaz de usuario informativa es pertinente y educativa para el usuario y mejora su experiencia. Debe ser:

-   **Simple:** los usuarios no quieren que su experiencia se vea interrumpida con información complicada
-   **Memorable:** los usuarios no desean ver las mismas instrucciones cada vez que intenten realizar una tarea, por lo que deben ser fáciles de recordar.
-   **Pertinente de manera inmediata:** si la interfaz de usuario informativa no informa a los usuarios sobre algo que quieren hacer inmediatamente, estos no tendrán ningún motivo para prestarle atención.

Evita el uso excesivo de la interfaz de usuario informativa y asegúrate de elegir los temas correctos. No incluyas la siguiente información:

-   **Características fundamentales:** si los usuarios necesitan instrucciones para usar la aplicación, considera la posibilidad de crear un diseño de aplicación más intuitivo.
-   **Características obvias:** la interfaz de usuario informativa no debe incluirse en los casos en que los usuarios puedan descubrir el funcionamiento de una característica por sí solos.
-   **Características complejas:** la interfaz de usuario informativa debe ser concisa, además, los usuarios interesados en características complejas normalmente están dispuestos a buscar instrucciones, por lo que no es necesario proporcionarlas directamente.

Evita molestar a los usuarios con la interfaz de usuario informativa. No hagas lo siguiente:

-   **Ocultar información importante:** la interfaz de usuario informativa nunca debería obstaculizar otras características de la aplicación.
-   **Obligar a los usuarios a participar:** los usuarios deben tener la posibilidad de omitir la interfaz de usuario informativa y avanzar en la aplicación.
-   **Mostrar información repetida:** no hostigues al usuario con la interfaz de usuario informativa, incluso si la pasan por alto la primera vez. La mejor solución es agregar una opción de configuración para volver a mostrar la interfaz de usuario informativa.

## <span id="examples_of_instructional_ui"></span><span id="EXAMPLES_OF_INSTRUCTIONAL_UI"></span>Ejemplos de interfaz de usuario informativa

Aquí te mostramos algunos ejemplos en los que la interfaz de usuario informativa puede ayudar a los usuarios:

-   **Ayudar a los usuarios a descubrir interacciones táctiles.** En la siguiente captura de pantalla se muestra la interfaz de usuario informativa en la que se enseña a un jugador a usar los gestos táctiles en el juego Cut the Rope.

    ![Captura de pantalla de juego que muestra un mensaje de la UI informativa, "Slide across to cut the rope" (Desliza el dedo para cortar la cuerda).](images/in-game-controls-3.png)

-   **Causar una excelente primera impresión.** Cuando la aplicación Momentos especiales se inicia por primera vez, la interfaz de usuario informativa pide al usuario que comience a crear películas sin obstaculizar su experiencia.

    ![Pantalla Inicio de la aplicación Momentos especiales](images/instructional-ui-movie.png)

-   **Guiar a los usuarios para que den el siguiente paso en una tarea complicada.** En la aplicación Correo de Windows, una sugerencia en la parte inferior de la bandeja de entrada dirige a los usuarios a **Configuración** para acceder a mensajes más antiguos.

    ![Captura de pantalla recortada de la aplicación Correo de Windows que muestra un mensaje de la interfaz de usuario informativa](images/instructional-ui-mail-inbox.png)

    Cuando el usuario hace clic en el mensaje, el control flotante **Configuración** de la aplicación aparece en el lado derecho de la pantalla y permite al usuario completar la tarea. En estas imágenes se muestra la aplicación Mail antes y después de que un usuario haga clic en el mensaje de la interfaz de usuario informativa.

    | Antes                                                               | Después                                                                                                        |
    |----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
    | ![Captura de pantalla de la aplicación Correo de Windows](images/instructional-ui-mail.png) | ![Captura de pantalla de la aplicación Correo de Windows con un control flotante Configuración extendido](images/instructional-ui-mail-flyout.png) |

## <span id="related_topics"></span>Artículos relacionados

* [Directrices para la ayuda de la aplicación](guidelines-for-app-help.md)


<!--HONumber=Mar16_HO5-->


