---
Description: Obtén información sobre cómo adaptar la interfaz de usuario de la aplicación al mostrar u ocultar el teclado táctil.
title: Responder a la presencia del teclado táctil
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Responder a la presencia del teclado táctil
template: detail.hbs
---

# Responder a la presencia del teclado táctil


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185)
-   [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255)

Obtén información sobre cómo adaptar la interfaz de usuario de la aplicación al mostrar u ocultar el teclado táctil.

![El teclado táctil en el modo de diseño predeterminado](images/touchkeyboard-standard.png)

<sup>El teclado táctil en el modo de diseño predeterminado</sup>

El teclado táctil permite la entrada de texto para dispositivos que admiten la entrada táctil. Controles de entrada de texto de plataforma de Windows (UWP) universal invocan al teclado táctil de manera predeterminada cuando un usuario pulsa en un campo de entrada editable. El teclado táctil normalmente permanece visible mientras el usuario navega por los controles en cierta forma, pero este comportamiento puede variar en función de los otros tipos de control dentro de la forma.

Para admitir el comportamiento del teclado táctil correspondiente en un control de entrada de texto personalizado que no se deriva de un control de entrada de texto estándar, debes usar la clase [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/br209185) para exponer los controles a la automatización de la interfaz de usuario de Microsoft e implementar los patrones de control de automatización de la interfaz de usuario correctos. Consulta [accesibilidad de teclado](https://msdn.microsoft.com/library/windows/apps/mt244347) y [automatización del mismo nivel personalizado](https://msdn.microsoft.com/library/windows/apps/mt297667).

Una vez que esta compatibilidad se ha agregado a tu control personalizado, puedes responder adecuadamente a la presencia del teclado táctil.

**Requisitos previos:  **

En este tema se basa en [interacciones de teclado](keyboard-interactions.md).

Debes tener un conocimiento básico de las interacciones de teclado estándar, controlar la entrada de teclado y los eventos y automatización de la interfaz de usuario.

Si acabas de empezar a desarrollar aplicaciones de la Plataforma Universal Windows (UWP), consulta estos temas para familiarizarte con las tecnologías presentadas aquí.

-   [Crea tu primera aplicación](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Directrices sobre la experiencia del usuario:  **

Para obtener sugerencias prácticas sobre el diseño de una aplicación optimizada para entrada de teclado que sea útil y atractiva, consulta [Directrices para el diseño de teclado](https://msdn.microsoft.com/library/windows/apps/hh972345).

## <span id="Touch_keyboard_and_a_custom_UI"></span><span id="touch_keyboard_and_a_custom_ui"></span><span id="TOUCH_KEYBOARD_AND_A_CUSTOM_UI"></span>Teclado táctil y una interfaz de usuario personalizada


Estas son algunas recomendaciones básicas para los controles de entrada de texto personalizado.

-   Muestra el teclado táctil manteniendo la plena interacción con el formulario.

-   Asegúrate de que los controles personalizados tengan la automatización de la interfaz de usuario adecuada [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/br209182) para que el teclado se mantenga cuando el foco se mueve desde el campo de entrada de texto estando en el contexto de entrada de texto. Por ejemplo, si tienes un menú que se abre en medio de una situación de entrada de texto y quieres que se mantenga el teclado, el menú debe tener el menú **AutomationControlType**.

-   No manipules las propiedades de automatización de la interfaz de usuario para controlar el teclado táctil. Otras herramientas de accesibilidad dependen de la precisión de las propiedades de automatización de la interfaz de usuario.

-   Asegúrate de que los usuarios siempre puedan ver el campo de entrada con el que están interactuando.

    Dado que el teclado táctil tapa una gran parte de la pantalla, el UWP garantiza que el campo de entrada con el foco se desplace en la vista como un usuario navega por los controles del formulario, incluidos los controles que no estén en la vista.

    Al personalizar la interfaz de usuario, proporciona un comportamiento similar en la apariencia del teclado táctil, controlando los eventos [**Showing**](https://msdn.microsoft.com/library/windows/apps/br242262) y [**Hiding**](https://msdn.microsoft.com/library/windows/apps/br242260) expuestos por el objeto [**InputPane**](https://msdn.microsoft.com/library/windows/apps/br242255).

    ![Formulario con y sin el teclado táctil visible](images/touch-keyboard-pan1.png)

    En algunos casos, algunos elementos de la interfaz de usuario deben permanecer en la pantalla todo el tiempo. Diseña la interfaz de usuario de modo que los controles del formulario estén incluidos en una región de movimiento panorámico y los elementos de interfaz de usuario importantes permanezcan estáticos. Por ejemplo:

    ![Un formulario que contiene áreas que deben permanecer siempre visibles](images/touch-keyboard-pan2.png)

## <span id="handling_events"></span><span id="HANDLING_EVENTS"></span>Control de los eventos Showing y Hiding


Este es un ejemplo de cómo incluir controladores de eventos para los eventos [**showing**](https://msdn.microsoft.com/library/windows/apps/br242262) y [**hiding**](https://msdn.microsoft.com/library/windows/apps/br242260) del teclado táctil.

```CSharp
public class MyApplication
{
    public MyApplication()
    {
        // Grab the input pane for the main application window and attach
        // touch keyboard event handlers.
        Windows.Foundation.Application.InputPane.GetForCurrentView().Showing  
            += new EventHandler(_OnInputPaneShowing);
        Windows.Foundation.Application.InputPane.GetForCurrentView().Hiding 
            += new EventHandler(_OnInputPaneHiding);
    }

    private void _OnInputPaneShowing(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        // If the size of this window is going to be too small, the app uses 
        // the Showing event to begin some element removal animations.
        if (eventArgs.OccludedRect.Top < 400)
        {
            _StartElementRemovalAnimations();

            // Don&#39;t use framework scroll- or visibility-related 
            // animations that might conflict with the app&#39;s logic.
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _OnInputPaneHiding(object sender, IInputPaneVisibilityEventArgs eventArgs)
    {
        if (_ResetToDefaultElements())
        {
            eventArgs.EnsuredFocusedElementInView = true; 
        }
    }

    private void _StartElementRemovalAnimations()
    {
        // This function starts the process of removing elements 
        // and starting the animation.
    }

    private void _ResetToDefaultElements()
    {
        // This function resets the window&#39;s elements to their default state.
    }
}
```

## <span id="related_topics"></span>Artículos relacionados



* [Interacciones de teclado](keyboard-interactions.md)
* [Accesibilidad de teclado](https://msdn.microsoft.com/library/windows/apps/mt244347)
* [Personalizar sistemas de automatización del mismo nivel](https://msdn.microsoft.com/library/windows/apps/mt297667)


**Muestras de archivo**
* [Entrada: muestra de teclado táctil](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [Muestra de respuesta a la apariencia del teclado en pantalla](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [Muestra de edición de texto XAML](http://go.microsoft.com/fwlink/p/?LinkID=251417)
* [Muestra de accesibilidad XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
 

 






<!--HONumber=Mar16_HO1-->


