---
Description: Incorporate speech into your apps using Cortana voice commands, speech recognition, and speech synthesis.
title: Interacciones de Surface Dial
label: Surface Dial interactions
template: detail.hbs
keywords: Surface Dial,rueda de Windows,RadialController,controlador radial,selector radial,interacción del usuario,entrada
ms.date: 02/08/2017
ms.topic: article
ms.assetid: e7deb1d6-feeb-471e-9a83-26386d1aaf37
ms.localizationpriority: medium
ms.openlocfilehash: bcdb8ca6843d126bc245e48f0b50209890740819
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8928572"
---
# <a name="surface-dial-interactions"></a>Interacciones de Surface Dial

![Imagen de Surface Dial con Surface Studio](images/windows-wheel/dial-pen-studio-600px.png)  
*Surface Dial con Surface Studio y Lápiz para Surface* (disponible para su compra en [Microsoft Store](https://aka.ms/purchasesurfacedial)).

## <a name="overview"></a>Introducción

Los dispositivos de rueda de Windows, como Surface Dial, son una nueva categoría de dispositivos de entrada que permiten un abanico de experiencias de interacción del usuario atractivas y únicas para Windows y aplicaciones de Windows. 

> [!IMPORTANT]
> En este tema, nos referimos específicamente a las interacciones de Surface Dial, pero la información es aplicable a todos los dispositivos de rueda de Windows. 

| Vídeos |   |
| --- | --- |
| <iframe src="https://www.youtube-nocookie.com/embed/WMklcdzcNcU" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> |
| *Asociados de aplicaciones de Surface Dial* | *Surface Dial para desarrolladores* |

Con un factor de forma que se basa en una acción (o gesto) de *giro*, Surface Dial se ha diseñado como un dispositivo de entrada secundario multimodal que complementa la entrada de un dispositivo principal. En la mayoría de los casos, el dispositivo se manipula con la mano no dominante del usuario mientras realiza una tarea con la mano dominante (por ejemplo, una entrada manuscrita con un lápiz). No está diseñado como una entrada de puntero de precisión (por ejemplo, entrada táctil, lápiz o mouse). 

Surface Dial también admite una acción de *pulsar y sostener* y una acción de *hacer clic*. Pulsar y sostener tiene una única función: mostrar un menú de comandos. Si el menú está activo, este procesa la entrada de girar y hacer clic. De lo contrario, la entrada se pasa a la aplicación para su procesamiento. 

**Al igual que con todos los dispositivos de entrada de Windows, puedes personalizar y adaptar la experiencia de interacción de Surface Dial para que se ajuste a la funcionalidad de tus aplicaciones.**

> [!TIP]
> Si se usan juntos, Surface Dial y el nuevo Surface Studio pueden proporcionar una experiencia de usuario aún más distintiva.  
>
>Además de la experiencia predeterminada del menú de pulsar y sostener, Surface Dial también puede colocarse directamente sobre la pantalla de Surface Studio. Esto activa un menú especial "en pantalla". 
>
>Mediante la detección del punto de contacto y de los límites de Surface Dial, el sistema usa esta información para controlar la oclusión por el dispositivo y mostrar una versión aún más grande del menú que rodea la parte exterior del Dial. Esta misma información también puede usarla la aplicación para adaptar la interfaz de usuario tanto a la presencia del dispositivo y su uso anticipado, como a la ubicación de la mano y el brazo del usuario.

| Menú fuera de la pantalla de Surface Dial | | Menú en la pantalla de Surface Dial |
| --- | --- | --- |
| ![Menú fuera de la pantalla de Surface Dial](images/windows-wheel/surface-dial-menu-offscreen.png) | | ![Menú en la pantalla de Surface Dial](images/windows-wheel/surface-dial-menu-onscreen.png) |

## <a name="system-integration"></a>Integración del sistema

Surface Dial está estrechamente integrado con Windows y admite un conjunto de herramientas integradas en el menú: volumen del sistema, desplazamiento, acercar/alejar y deshacer/rehacer.

Esta colección de herramientas integradas se adapta al contexto actual del sistema para incluir:
- Una herramienta de brillo del sistema cuando el usuario está en el escritorio de Windows
- Una herramienta de pista anterior/siguiente cuando se están reproduciendo elementos multimedia

Además de esta compatibilidad de plataforma general, Surface Dial también está estrechamente integrado con los controles de plataforma de Windows Ink ([**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) e [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar)).

![Surface Dial con Lápiz para Surface](images/windows-wheel/dial-and-pen-400px.png)  
*Surface Dial con Lápiz para Surface*

Cuando se usa con Surface Dial, estos controles habilitan funciones adicionales que permiten modificar los atributos de entrada de lápiz y controlar la galería de símbolos de regla de la barra de herramientas de entradas de Ink.

Cuando se abre el menú Surface Dial en una aplicación de entrada manuscrita que usa la barra de herramientas de entradas de Ink, el menú ahora incluye herramientas para controlar el tipo de lápiz y el grosor del pincel. Cuando se habilita la regla, se agrega una herramienta correspondiente al menú que permite al dispositivo controlar la posición y el ángulo de la regla.

![Menú de Surface Dial con la herramienta de selección de lápiz para la barra de herramientas de Windows Ink](images/windows-wheel/surface-dial-menu-inktoolbar-pen.png)  
*Menú de Surface Dial con la herramienta de selección de lápiz para la barra de herramientas de Windows Ink*

![Menú de Surface Dial con la herramienta de tamaño de trazo para la barra de herramientas de Windows Ink](images/windows-wheel/surface-dial-menu-inktoolbar-strokesize.png)  
*Menú de Surface Dial con la herramienta de tamaño de trazo para la barra de herramientas de Windows Ink*

![Menú de Surface Dial con la herramienta de regla para la barra de herramientas de Windows Ink](images/windows-wheel/surface-dial-menu-inktoolbar-ruler.png)  
*Menú de Surface Dial con la herramienta de regla para la barra de herramientas de Windows Ink*

## <a name="user-customization"></a>Personalización del usuario

Los usuarios pueden personalizar algunos aspectos de su experiencia con el Dial a través de la página **Configuración de Windows -> Dispositivos -> Rueda**, incluidas las herramientas predeterminadas, la vibración (o comentarios hápticos) y la mano de escribir (o dominante). 

Cuando quieras personalizar la experiencia del usuario de Surface Dial, siempre debes asegurarte de que una determinada función o comportamiento está disponible y habilitado por el usuario.

## <a name="custom-tools"></a>Herramientas personalizadas

En esta sección, analizaremos tanto las directrices sobre la experiencia del usuario como la guía para desarrolladores para personalizar las herramientas que se exponen en el menú de Surface Dial.

### <a name="ux-guidance"></a>Directrices sobre la experiencia de usuario

**Asegurarte de que tus herramientas se corresponden con el contexto actual** Cuando consigues que resulte claro e intuitivo qué es lo que hace una herramienta y cómo funciona la interacción con Surface Dial, ayudas a que los usuarios aprendan rápidamente y se centren en sus tareas.

**Minimizar el número de herramientas de la aplicación tanto como sea posible**  
El menú de Surface Dial tiene espacio para siete elementos. Si hay ocho o más elementos, el usuario deberá girar el Dial para ver qué herramientas están disponibles en un control flotante de desbordamiento, lo que dificultará la navegación por el menú, así como la detección y la selección de las herramientas.

Te recomendamos que proporciones una sola herramienta personalizada para tu aplicación o para el contexto de la aplicación. Esto te permitirá configurar la herramienta en función de lo que está haciendo el usuario sin que tenga que activar el menú de Surface Dial y seleccionar una herramienta. 

**Actualizar dinámicamente la colección de herramientas**  
Como los elementos de menú de Surface Dial no admiten un estado deshabilitado, deberás agregar y quitar dinámicamente las herramientas (incluidas las herramientas integradas predeterminadas) en función del contexto del usuario (vista actual o ventana enfocada). Si una herramienta no es pertinente para la actividad actual o es redundante, quítala.

> [!IMPORTANT]
> Cuando agregues un elemento al menú, asegúrate de que el elemento no existe aún.

**No quitar la herramienta integrada de configuración del volumen del sistema**  
Normalmente, el usuario siempre va a necesitar el control del volumen. Es posible que escuche música mientras usa tu aplicación, así que las herramientas de volumen y de pista siguiente siempre deben estar accesibles desde el menú de Surface Dial. (La herramienta de pista siguiente se agrega automáticamente al menú cuando se reproducen elementos multimedia).

**Ser coherente con la organización del menú**  
Esto ayuda a los usuarios a descubrir y conocer las herramientas que están disponibles al usar la aplicación, y ayuda a mejorar su eficacia al cambiar de herramientas.

**Proporcionar iconos de alta calidad coherentes con los iconos integrados**  
Los iconos pueden transmitir profesionalidad y excelencia, e inspiran confianza a los usuarios.
- Proporcionar una imagen PNG de alta calidad de 64 x 64 píxeles (44 x 44 es el tamaño más pequeño admitido)
- Asegurarse de que el fondo sea transparente
- El icono debería llenar la mayor parte de la imagen
- Un icono en blanco debe tener un contorno negro para que sea visible en el modo de contraste alto

|   |   |   |
| --- | --- | --- |
| ![Icono con fondo alfa](images/windows-wheel/surface-dial-menu-icon1.png) | ![Icono que se muestra en el menú de rueda con el icono de tema predeterminado](images/windows-wheel/surface-dial-menu-icon2.png) | ![Menú en la pantalla de Surface Dial](images/windows-wheel/surface-dial-menu-icon3.png) |
| *Icono con fondo alfa* | *Icono que se muestra en el menú de rueda con el tema predeterminado* | *Icono que se muestra en el menú de rueda con el tema blanco en contraste alto* |

**Usar nombres concisos y descriptivos**  
El nombre de la herramienta se muestra en el menú de la herramienta junto con el icono de la herramienta, y también lo usan los lectores de pantalla. 
- Los nombres deben ser cortos para que quepan dentro del círculo central del menú de rueda.
- Los nombres deben identificar claramente la acción principal (se puede implicar una acción complementaria):
  - El desplazamiento indica el efecto de ambas direcciones de rotación
  - La acción de deshacer especifica una acción principal, sin embargo rehacer (la acción complementaria) se puede inferir y el usuario puede descubrirla fácilmente

### <a name="developer-guidance"></a>Guía para desarrolladores

Puedes personalizar la experiencia de Surface Dial para complementar la funcionalidad de tus aplicaciones a través de un completo conjunto de [API de Windows en tiempo de ejecución](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController). 

Como se mencionó anteriormente, el menú predeterminado de Surface Dial se rellena previamente con un conjunto de herramientas integradas que cubre una amplia gama de características básicas del sistema (volumen del sistema, brillo del sistema, desplazamiento, zoom, deshacer y control multimedia cuando el sistema detecta que se está reproduciendo audio o vídeo). Sin embargo, es posible que estas herramientas predeterminadas no proporcionen la funcionalidad necesaria para la aplicación. 

En las siguientes secciones, describimos cómo agregar una herramienta personalizada al menú de Surface Dial y cómo especificar las herramientas integradas que se exponen.

**Agregar una herramienta personalizada**

En este ejemplo, agregamos una herramienta personalizada básica que pasa los datos de entrada de los eventos de rotación y de clic a algunos controles de UI de XAML.

1. Primero, declaramos nuestra interfaz de usuario (solo un control deslizante y un botón de alternancia) en XAML.

   ![Imagen de la interfaz de usuario de la aplicación de ejemplo](images/windows-wheel/surface-dial-snippet-customtool1.png)  
   *La interfaz de usuario de la aplicación de ejemplo*

    ```Xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
          <TextBlock x:Name="Header"
            Text="RadialController customization sample"
            VerticalAlignment="Center"
            Style="{ThemeResource HeaderTextBlockStyle}"
            Margin="10,0,0,0" />
      </StackPanel>
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center"
        Grid.Row="1">
          <!-- Slider for rotation input -->
          <Slider x:Name="RotationSlider"
            Width="300"
            HorizontalAlignment="Left"/>
          <!-- Switch for click input -->
          <ToggleSwitch x:Name="ButtonToggle"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    ```

2. Luego, en el código subyacente, agregamos una herramienta personalizada al menú de Surface Dial y declaramos los controladores de entrada [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController). 

   Obtenemos una referencia al objeto [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) para Surface Dial (myController) mediante una llamada a [**CreateForCurrentView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.CreateForCurrentView).

   A continuación, creamos una instancia de un [**RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) (myItem) mediante una llamada a [**RadialControllerMenuItem.CreateFromIcon**](https://msdn.microsoft.com/library/windows/apps/mt759255). 

   Después, anexamos ese elemento a la colección de elementos de menú.

   Declaramos los controladores de eventos de entrada ([**ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) y [**RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged)) para el objeto [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController).

   Por último, definimos los controladores de eventos.

    ```csharp
    public sealed partial class MainPage : Page
    {
        RadialController myController;

        public MainPage()
        {
            this.InitializeComponent();
            // Create a reference to the RadialController.
            myController = RadialController.CreateForCurrentView();

            // Create an icon for the custom tool.
            RandomAccessStreamReference icon =
              RandomAccessStreamReference.CreateFromUri(
                new Uri("ms-appx:///Assets/StoreLogo.png"));

            // Create a menu item for the custom tool.
            RadialControllerMenuItem myItem =
              RadialControllerMenuItem.CreateFromIcon("Sample", icon);

            // Add the custom tool to the RadialController menu.
            myController.Menu.Items.Add(myItem);

            // Declare input handlers for the RadialController.
            myController.ButtonClicked += MyController_ButtonClicked;
            myController.RotationChanged += MyController_RotationChanged;
        }

        // Handler for rotation input from the RadialController.
        private void MyController_RotationChanged(RadialController sender,
          RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees > 100)
            {
                RotationSlider.Value = 100;
                return;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < 0)
            {
                RotationSlider.Value = 0;
                return;
            }
            RotationSlider.Value += args.RotationDeltaInDegrees;
        }

        // Handler for click input from the RadialController.
        private void MyController_ButtonClicked(RadialController sender,
          RadialControllerButtonClickedEventArgs args)
        {
            ButtonToggle.IsOn = !ButtonToggle.IsOn;
        }
    }
    ```

Al ejecutar la aplicación, utilizamos Surface Dial para interactuar con ella. En primer lugar, pulsamos y sostenemos para abrir el menú y seleccionar nuestra herramienta personalizada. Una vez activada la herramienta personalizada, podemos girar el Dial para ajustar el control deslizante y hacer clic en él para alternar el modificador.

![Imagen de la interfaz de usuario de la aplicación de ejemplo activada mediante la herramienta personalizada de Surface Dial](images/windows-wheel/surface-dial-snippet-customtool2.png)  
*Interfaz de usuario de la aplicación de ejemplo activada mediante la herramienta personalizada de Surface Dial*

**Especificar las herramientas integradas**

Puedes usar la clase [**RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) para personalizar la colección de elementos de menú integrados de la aplicación.

Por ejemplo, si la aplicación no tiene regiones de desplazamiento o de zoom, y no necesita la funcionalidad de deshacer y rehacer, se pueden quitar estas herramientas del menú. Esto dejará espacio en el menú para agregar herramientas personalizadas para tu aplicación. 

> [!IMPORTANT] 
> El menú de Surface Dial debe tener al menos un elemento de menú. Si se quitan todas las herramientas predeterminadas antes de que agregues una de tus herramientas personalizadas, se restaurarán las herramientas predeterminadas y tu herramienta se anexará a la colección predeterminada.

Por las directrices de diseño, no se recomienda quitar que las herramientas de control multimedia (volumen y pista anterior y siguiente), ya que, con frecuencia, los usuarios tienen música reproduciéndose de fondo mientras realizan otras tareas.

Aquí te mostramos cómo configurar el menú de Surface Dial para incluir solo los controles multimedia solo para volumen y pista siguiente o anterior.

```csharp
public MainPage()
{
  ...
  //Remove a subset of the default system tools
  RadialControllerConfiguration myConfiguration = 
  RadialControllerConfiguration.GetForCurrentView();
  myConfiguration.SetDefaultMenuItems(new[] 
  {
    RadialControllerSystemMenuItemKind.Volume,
      RadialControllerSystemMenuItemKind.NextPreviousTrack
  });
}
```

## <a name="custom-interactions"></a>Interacciones personalizadas

Como ya se ha mencionado, Surface Dial admite tres gestos (pulsar y sostener, girar y hacer clic) con las correspondientes interacciones predeterminadas. 

Asegúrate de que cualquier interacción personalizada basada en estos gestos tenga sentido para la acción o la herramienta seleccionada. 

> [!NOTE]
> La experiencia de interacción depende del estado del menú de Surface Dial. Si el menú está activo, él se encarga de procesar la entrada, de lo contrario, se encarga la aplicación.

### <a name="press-and-hold"></a>Pulsar y sostener

Este gesto activa y muestra el menú de Surface Dial, no hay ninguna funcionalidad de la aplicación asociada con este gesto. 

De manera predeterminada, el menú se muestra en el centro de la pantalla del usuario. Sin embargo, el usuario puede agarrarlo y moverlo a cualquier lugar que quiera.

> [!NOTE]
> Cuando Surface Dial se coloca en la pantalla de Studio Surface, el menú se centra justo en la posición de la pantalla donde está colocado Surface Dial.

### <a name="rotate"></a>Rotar

Surface Dial está diseñado principalmente para admitir la rotación para las interacciones que implican ajustes suaves e incrementales de controles o valores analógicos.

El dispositivo se puede girar hacia la derecha y hacia la izquierda, y también puedes proporcionar comentarios hápticos para indicar distancias discretas.

> [!NOTE]
> El usuario puede deshabilitar los comentarios hápticos en la página **Configuración de Windows -> Dispositivos -> Rueda**.

#### <a name="ux-guidance"></a>Directrices sobre la experiencia de usuario

**Las herramientas con alta sensibilidad rotacional o sensibilidad continua deben deshabilitar los comentarios hápticos**

Los comentarios hápticos coinciden con la sensibilidad rotacional de la herramienta activa. Se recomienda deshabilitar los comentarios hápticos para las herramientas con alta sensibilidad rotacional o sensibilidad continua ya que resulta una experiencia incómoda para el usuario. 

**La mano dominante no debería afectar a las interacciones basadas en la rotación**

Surface Dial no puede detectar qué mano se está usando, pero el usuario puede establecer la escritura manuscrita (o la mano dominante) en **Configuración de Windows -> Dispositivo -> Lápiz y Windows Ink**.

**La configuración regional se debería tener en cuenta para todas las interacciones de rotación**

Maximiza la satisfacción del cliente acomodando y adaptando tus interacciones con la configuración regional y los diseños de derecha a izquierda.

Las herramientas y comandos integrados en el menú del Dial siguen estas directrices para las interacciones basadas en la rotación:

|   |   |   |
| --- | --- | --- |
| Izquierda<br/>Arriba<br/>Alejar | ![Imagen de Surface Dial](images/windows-wheel/surface-dial-rotate.png) | Derecha<br/>Abajo<br/>Acercar |
|   |   |   |

| Dirección conceptual | Asignación a Surface Dial | Rotación en sentido del reloj | Rotación en sentido contrario a las agujas del reloj |
| --- | --- | --- | --- |
| Horizontal | Asignación de izquierda y derecha basada en la parte superior de Surface Dial | Derecha | Izquierda |
| Vertical | Asignación de arriba y abajo basada en el lado izquierdo de Surface Dial | Abajo | Arriba |
| Eje z | Acercar (o más cerca) asignado a arriba/derecha<br/>Alejar (o más lejos) asignado a abajo/izquierda | Acercar | Alejar |

#### <a name="developer-guidance"></a>Guía para desarrolladores

A medida que el usuario gira el dispositivo, se desencadenan los eventos de [**RadialController.RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged) en función de una variación diferencial ([**RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees)) con respecto a la dirección de giro. La sensibilidad (o resolución) de los datos se puede establecer con la propiedad [**RadialController.RotationResolutionInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationResolutionInDegrees).

> [!NOTE]
> De manera predeterminada, un evento de entrada rotacional se entrega a un objeto [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) solo cuando el dispositivo se gira un mínimo de 10 grados. Cada evento de entrada hace que el dispositivo vibre.

En general, se recomienda deshabilitar los comentarios hápticos cuando la resolución de rotación está definida a menos de 5 grados. Esto proporciona una experiencia más uniforme para interacciones continuas. 

Puedes habilitar y deshabilitar los comentarios hápticos para herramientas personalizadas configurando la propiedad [**RadialController.UseAutomaticHapticFeedback**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.UseAutomaticHapticFeedback).

> [!NOTE]
> No puedes invalidar el comportamiento háptico de las herramientas del sistema, como el control de volumen. Para estas herramientas, solo el usuario puede deshabilitar los comentarios hápticos por el usuario desde la página de configuración de la rueda.

Este es un ejemplo de cómo personalizar la resolución de los datos de rotación y habilitar o deshabilitar comentarios hápticos.

```csharp
private void MyController_ButtonClicked(RadialController sender, 
  RadialControllerButtonClickedEventArgs args)
{
  ButtonToggle.IsOn = !ButtonToggle.IsOn;

  if(ButtonToggle.IsOn)
  {
    //high resolution mode
    RotationSlider.LargeChange = 1;
    myController.UseAutomaticHapticFeedback = false;
    myController.RotationResolutionInDegrees = 1;
  }
  else
  {
    //low resolution mode
    RotationSlider.LargeChange = 10;
    myController.UseAutomaticHapticFeedback = true;
    myController.RotationResolutionInDegrees = 10;
  }
}
```

### <a name="click"></a>Hacer clic

Hacer clic en Surface Dial es similar a hacer clic en el botón izquierdo del mouse (el estado de rotación del dispositivo no tiene ningún efecto en esta acción).

#### <a name="ux-guidance"></a>Directrices sobre la experiencia de usuario

**No asignar una acción o comando a este gesto si el usuario no puede recuperarse fácilmente del resultado**

Cualquier acción que realice tu aplicación en base al clic del usuario en Surface Dial debe ser reversible. Siempre debes permitir que el usuario pueda recorrer fácilmente la pila de retroceso de la aplicación y restaurar un estado anterior de la aplicación.

Operaciones binarias como desactivar/activar notificaciones o mostrar/ocultar proporcionar buenas experiencias de usuario con el gesto de hacer clic.

**Las herramientas modales no se deben habilitar o deshabilitar haciendo clic en Surface Dial**

Algunos modos de aplicación/herramienta pueden entrar en conflicto con las interacciones que dependen de la rotación, o deshabilitarlas. Herramientas, como la regla en la barra de herramientas de Windows Ink, deben activarse o desactivarse a través de otras prestaciones de la interfaz de usuario (la barra de herramientas de Windows Ink proporciona un control [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.ToggleButton) integrado).

Para herramientas modales, asigna el elemento de menú de Surface Dial a la herramienta de destino o al elemento de menú seleccionado anteriormente.

#### <a name="developer-guidance"></a>Guía para desarrolladores

Cuando se hace clic en Surface Dial, se desencadena un evento [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked). Los argumentos [**RadialControllerButtonClickedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs) incluyen una propiedad [**Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact) que contiene la ubicación y el área límite del punto de contacto de Surface Dial en la pantalla de Surface Studio. Si Surface Dial no está en contacto con la pantalla, esta propiedad es null. 

### <a name="on-screen"></a>En la pantalla

Tal como se describió anteriormente, Surface Dial se puede usar junto con el Surface Studio para mostrar el menú de Surface Dial en un modo de pantalla especial. 

En este modo, puedes aumentar la integración y la personalización de tus experiencias de interacción del Dial con tus aplicaciones. Estos son algunos ejemplos de experiencias únicas que solo son posibles con Surface Dial y Surface Studio:
- Mostrar herramientas contextuales (como una paleta de colores) en función de la posición de Surface Dial, que hace que sean más fáciles de encontrar y usar.
- Configurar la herramienta activa en función de la interfaz de usuario en la que esté colocado Surface Dial
- Aumentar un área de la pantalla según la ubicación de Surface Dial
- Interacciones únicas con los juegos en base a la ubicación en la pantalla

#### <a name="ux-guidance"></a>Directrices sobre la experiencia de usuario

**Las aplicaciones deben responder cuando se detecta la presencia de Surface Dial sobre la pantalla**

Los comentarios visuales ayudan a indicar al usuario que tu aplicación ha detectado el dispositivo en la pantalla de Surface Studio.

**Ajustar la interfaz de usuario relacionada con Surface Dial en función de la ubicación del dispositivo**

El dispositivo (y el cuerpo del usuario) pueden tapar partes críticas de la interfaz de usuario según donde el usuario lo coloque.

**Ajustar la interfaz de usuario relacionada con Surface Dial en función de la interacción del usuario**

Además de la oclusión de hardware, la mano o el brazo de un usuario pueden ocultar parte de la pantalla al usar el dispositivo. 

El área que queda oculta depende de la mano que se esté usando con el dispositivo. Como el dispositivo está diseñado para usarse principalmente con la mano no dominante, se debe ajustar la interfaz de usuario relacionada con Surface Dial a la mano opuesta a la especificada por el usuario (opción **Configuración de Windows > Dispositivo > Lápiz y Windows Ink > Elige la mano con la que escribes**).

**Las interacciones deben responder a la posición de Surface Dial en lugar de al movimiento**

El pie del dispositivo está diseñado para pegarse a la pantalla y no para deslizarse sobre ella, ya que no es un dispositivo señalador de precisión. Por lo tanto, esperamos que lo habitual sea que los usuarios levanten y coloquen Surface Dial en lugar de arrastrarlo por la pantalla.

**Usar la posición de la pantalla para determinar la intención del usuario**

Configurar la herramienta activa en base al contexto de la interfaz de usuario, como puede ser la proximidad a un control, un lienzo o una ventana, puede mejorar la experiencia del usuario al reducir los pasos necesarios para realizar una tarea.

#### <a name="developer-guidance"></a>Guía para desarrolladores

Cuando Surface Dial se coloca en la superficie digitalizadora de Studio Surface, se desencadena un evento [**RadialController.ScreenContactStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ScreenContactStarted) y se proporciona la información de contacto ([**RadialControllerScreenContactStartedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs.Contact)) a la aplicación.

Del mismo modo, si se hace clic en Surface Dial cuando está en contacto con la superficie del digitalizador de Studio Surface, un [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) se desencadena el evento y la información de contacto ([**RadialControllerButtonClickedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact)) se proporciona a la aplicación. 

La información de contacto ([**RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact)) incluye las coordenadas X/Y del centro de Surface Dial en el espacio de coordenadas de la aplicación ([**RadialControllerScreenContact.Position**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Position)), así como el rectángulo delimitador ([**RadialControllerScreenContact.Bounds**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Bounds)) en píxeles independientes del dispositivo (DIP). Esta información es muy útil para proporcionar contexto a la herramienta activa y proporcionar al usuario comentarios visuales relacionados con el dispositivo.

En el siguiente ejemplo, hemos creado una aplicación básica con cuatro secciones diferentes, cada una de ellas incluye un control deslizante y un botón de alternancia. A continuación, usamos la posición en pantalla de Surface Dial para dictar qué conjunto de controles deslizantes y botones de alternancia está controlado por Surface Dial.

1. Primero, declaramos nuestra interfaz de usuario (cuatro secciones, cada una con un control deslizante y un botón de alternancia) en XAML.

   ![Imagen de la interfaz de usuario de la aplicación de ejemplo](images/windows-wheel/surface-dial-snippet-customtool3.png)  
   *La interfaz de usuario de la aplicación de ejemplo*

   ```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
  <Grid.RowDefinitions>
    <RowDefinition Height="Auto"/>
    <RowDefinition Height="*"/>
  </Grid.RowDefinitions>
  <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
    <TextBlock x:Name="Header"
      Text="RadialController customization sample"
      VerticalAlignment="Center"
      Style="{ThemeResource HeaderTextBlockStyle}"
      Margin="10,0,0,0" />
  </StackPanel>
  <Grid Grid.Row="1" x:Name="RootGrid">
    <Grid.RowDefinitions>
      <RowDefinition Height="*"/>
      <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
      <ColumnDefinition Width="*"/>
      <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid x:Name="Grid0"
      Grid.Row="0"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider0"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle0"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid1"
      Grid.Row="0"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider1"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle1"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid2"
      Grid.Row="1"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider2"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle2"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid3"
      Grid.Row="1"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider3"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle3"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
  </Grid>
</Grid>
   ```

2. Este es el código subyacente con controladores definido para la posición en pantalla de Surface Dial.

```csharp
Slider ActiveSlider;
ToggleSwitch ActiveSwitch;
Grid ActiveGrid;

public MainPage()
{
  ...

  myController.ScreenContactStarted += 
    MyController_ScreenContactStarted;
  myController.ScreenContactContinued += 
    MyController_ScreenContactContinued;
  myController.ScreenContactEnded += 
    MyController_ScreenContactEnded;
  myController.ControlLost += MyController_ControlLost;

  //Set initial grid for Surface Dial input.
  ActiveGrid = Grid0;
  ActiveSlider = RotationSlider0;
  ActiveSwitch = ButtonToggle0;
}

private void MyController_ScreenContactStarted(RadialController sender, 
  RadialControllerScreenContactStartedEventArgs args)
{
  //find grid at contact location, update visuals, selection
  ActivateGridAtLocation(args.Contact.Position);
}

private void MyController_ScreenContactContinued(RadialController sender, 
  RadialControllerScreenContactContinuedEventArgs args)
{
  //if a new grid is under contact location, update visuals, selection
  if (!VisualTreeHelper.FindElementsInHostCoordinates(
    args.Contact.Position, RootGrid).Contains(ActiveGrid))
  {
    ActiveGrid.Background = new 
      SolidColorBrush(Windows.UI.Colors.White);
    ActivateGridAtLocation(args.Contact.Position);
  }
}

private void MyController_ScreenContactEnded(RadialController sender, object args)
{
  //return grid color to normal when contact leaves screen
  ActiveGrid.Background = new 
  SolidColorBrush(Windows.UI.Colors.White);
}

private void MyController_ControlLost(RadialController sender, object args)
{
  //return grid color to normal when focus lost
  ActiveGrid.Background = new 
    SolidColorBrush(Windows.UI.Colors.White);
}

private void ActivateGridAtLocation(Point Location)
{
  var elementsAtContactLocation = 
    VisualTreeHelper.FindElementsInHostCoordinates(Location, 
      RootGrid);

  foreach (UIElement element in elementsAtContactLocation)
  {
    if (element as Grid == Grid0)
    {
      ActiveSlider = RotationSlider0;
      ActiveSwitch = ButtonToggle0;
      ActiveGrid = Grid0;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid1)
    {
      ActiveSlider = RotationSlider1;
      ActiveSwitch = ButtonToggle1;
      ActiveGrid = Grid1;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid2)
    {
      ActiveSlider = RotationSlider2;
      ActiveSwitch = ButtonToggle2;
      ActiveGrid = Grid2;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid3)
    {
      ActiveSlider = RotationSlider3;
      ActiveSwitch = ButtonToggle3;
      ActiveGrid = Grid3;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
  }
}
```

Al ejecutar la aplicación, utilizamos Surface Dial para interactuar con ella. En primer lugar, colocamos el dispositivo en la pantalla de Surface Studio, acción que la aplicación detecta y la asocia a la sección inferior derecha (consulta la imagen). A continuación, pulsamos y sostenemos Surface Dial para abrir el menú y seleccionar la herramienta personalizada. Una vez activada la herramienta personalizada, podemos girar Surface Dial para ajustar el control deslizante o hacer clic en él para alternar el botón de alternancia.

![Imagen de la interfaz de usuario de la aplicación de ejemplo activada mediante la herramienta personalizada de Surface Dial](images/windows-wheel/surface-dial-snippet-customtool4.png)  
*Interfaz de usuario de la aplicación de ejemplo activada mediante la herramienta personalizada de Surface Dial*

## <a name="summary"></a>Resumen

En este tema se ofrece información general sobre el dispositivo de entrada Surface Dial; en él se incluyen directrices sobre la experiencia del usuario y una guía para desarrolladores sobre cómo personalizar la experiencia del usuario para escenarios tanto fuera de la pantalla como encima de la pantalla, cuando se usa con Surface Studio.

## <a name="feedback"></a>Comentarios

Envía tu preguntas, sugerencias y comentarios a [radialcontroller@microsoft.com](mailto:radialcontroller@microsoft.com).

## <a name="related-articles"></a>Artículos relacionados

### <a name="api-reference"></a>Referencia de las API

- [Clase **RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)
- [Clase **RadialControllerButtonClickedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [Clase **RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 
- [Clase **RadialControllerControlAcquiredEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [Clase **RadialControllerMenu**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenu) 
- [Clase **RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) 
- [Clase **RadialControllerRotationChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [Clase **RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact) 
- [Clase **RadialControllerScreenContactContinuedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [Clase **RadialControllerScreenContactStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [Enumeración **RadialControllerMenuKnownIcon**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [Enumeración **RadialControllerSystemMenuItemKind**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>Muestras

[Muestras de la Plataforma universal de Windows (C# y C++)](https://go.microsoft.com/fwlink/?linkid=832713)

[Muestra de escritorio clásico de Windows](https://aka.ms/radialcontrollerclassicsample)