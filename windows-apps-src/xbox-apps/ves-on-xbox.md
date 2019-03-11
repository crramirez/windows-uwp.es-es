---
title: Voz habilitada Shell (VES) en Xbox
description: Obtenga información sobre cómo agregar compatibilidad con control de voz a sus aplicaciones para UWP en Xbox.
ms.date: 10/19/2017
ms.topic: article
keywords: Windows 10, uwp, xbox, voz, el shell de voz habilitada
ms.openlocfilehash: ea51216c804754e98c3bac459b79fb75dd9369cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596060"
---
# <a name="using-speech-to-invoke-ui-elements"></a>Uso de voz para invocar los elementos de interfaz de usuario

Shell habilitado por voz (VES) es una extensión a la plataforma de voz Windows que ofrece una experiencia de primera clase de voz dentro de las aplicaciones, lo que permite a los usuarios utilizar la voz para invocar en pantalla los controles y para insertar texto a través de dictado. VES se esfuerza por proporcionar una experiencia de vea-it-say-it-to-end común en todos los Shells de Windows y dispositivos, con un esfuerzo mínimo necesario de desarrolladores de aplicaciones.  Para lograr esto, aprovecha la plataforma de voz de Microsoft y el marco de UI Automation (UIA).

## <a name="user-experience-walkthrough"></a>Tutorial de la experiencia de usuario ##
El siguiente es una visión general de lo que experimentaría un usuario cuando se usa VES en Xbox y debería ayudar a establecer el contexto antes de profundizar en los detalles del funcionamiento de VES.

- Usuario enciende la consola Xbox y desea examinar a través de sus aplicaciones para encontrar algo interesante:

        User: "Hey Cortana, open My Games and Apps"

- Usuario se deja en Active escuchando modo (ALM), lo que significa que la consola está escuchando ahora el usuario puede llamar a un control que está visible en la pantalla, sin necesidad de decir, "Hola Cortana" cada vez.  Ahora puede cambiar el usuario para ver las aplicaciones y desplácese por la lista de aplicaciones:

        User: "applications"

- Para desplazar la vista, el usuario puede decir simplemente:

        User: "scroll down"

- Ve el material de la aplicación que están interesado en pero ¿ha olvidado el nombre de usuario.  El usuario solicita las etiquetas de la sugerencia de voz que se mostrará:

        User: "show labels"

- Ahora que quede claro qué decir, se puede iniciar la aplicación:

        User: "movies and TV"

- Para salir del modo de escucha activa, el usuario le indica a Xbox para dejar de escuchar:

        User: "stop listening"

- Más adelante se puede iniciar una nueva sesión de escucha activada con:

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>Dependencia de automatización de interfaz de usuario ##
VES es un cliente de automatización de interfaz de usuario y se basa en información expuesta por la aplicación a través de sus proveedores de automatización de interfaz de usuario. Se trata de la misma infraestructura que ya está usa la característica Narrador en plataformas de Windows.  Automatización de interfaz de usuario permite el acceso mediante programación a los elementos de interfaz de usuario, incluido el nombre del control, su tipo y qué patrones de control implementa.  La interfaz de usuario que los cambios en la aplicación, se reaccionar ante eventos de actualización de UIA VES y volver a analizar el árbol de automatización de interfaz de usuario actualizado para encontrar todos los elementos que requieren acción, con esta información para crear una gramática de reconocimiento de voz. 

Todas las aplicaciones UWP tienen acceso para el marco de automatización de interfaz de usuario y pueden exponer la información acerca de la interfaz de usuario independientemente de qué marco de gráficos que se basan en (XAML, DirectX y Direct3D, Xamarin, etcetera).  En algunos casos, como XAML, la mayoría del trabajo pesado se realiza mediante el marco de trabajo, reduce considerablemente el trabajo necesario para admitir el Narrador y VES.

Para obtener más información sobre la automatización de interfaz de usuario vea [Fundamentos de UI Automation](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "Fundamentos de UI Automation").

## <a name="control-invocation-name"></a>Nombre de la invocación de control ##
VES emplea la heurística siguiente para determinar qué frase para registrar con el reconocimiento de voz como el nombre del control (es decir. lo que el usuario debe comunicar para invocar el control).  También es la frase que se mostrará en la etiqueta de sugerencia de voz.

Origen de nombre en orden de prioridad:

1. Si el elemento tiene un `LabeledBy` usará la propiedad adjunta, VES el `AutomationProperties.Name` de esta etiqueta de texto.
2. `AutomationProperties.Name` del elemento.  En XAML, el contenido de texto del control se usará como el valor predeterminado de `AutomationProperties.Name`.
3. Si el control es un botón o ListItem, VES buscará el primer elemento secundario con válido `AutomationProperties.Name`.

## <a name="actionable-controls"></a>Controles que requieren acción ##
VES considera que un control útil si implementa uno de los siguientes patrones de control de automatización:

- **InvokePattern** (p ej. Botón)-representa controles que inician o realizan una única acción inequívoca y no mantienen el estado cuando se activan.

- **TogglePattern** (eg. Casilla de verificación): representa un control que puede recorrer en iteración un conjunto de Estados y mantener un estado una vez establecido.

- **SelectionItemPattern** (p ej. Cuadro combinado) - representa un control que actúa como contenedor para una colección de elementos secundarios seleccionables.

- **ExpandCollapsePattern** (p ej. Cuadro combinado) - representa controles que se expanden visualmente para mostrar el contenido y contraen para ocultarlo.

- **ScrollPattern** (p ej. Lista): representa los controles que actúan como contenedores desplazables para una colección de elementos secundarios.

## <a name="scrollable-containers"></a>Contenedores desplazables ##
Para los contenedores desplazables que admiten que el ScrollPattern, VES realizará escuchas para voz comandos como "desplazamiento izquierda", "Desplazar a la derecha", etc. e invocará desplazamiento con los parámetros adecuados cuando el usuario activa uno de estos comandos.  Comandos de desplazamiento se insertan en función del valor de la `HorizontalScrollPercent` y `VerticalScrollPercent` propiedades.  Por ejemplo, si `HorizontalScrollPercent` es mayor que 0, "desplazarse a la izquierda" se agregarán si es menor que 100, "Desplazar a la derecha" se agregarán y así sucesivamente.

## <a name="narrator-overlap"></a>Superposición de Narrador ##
La aplicación de Narrador también es un cliente de automatización de interfaz de usuario y usa el `AutomationProperties.Name` propiedad como uno de los orígenes para el texto lee del elemento de interfaz de usuario actualmente seleccionado.  Para proporcionar una mejor experiencia de accesibilidad aplicación muchos desarrolladores han recurrido a la sobrecarga del `Name` propiedad con el texto descriptivo largo con el fin de proporcionar más información y contexto Narrador cuando los lee.  Sin embargo, esto provoca un conflicto entre las dos funciones: VES debe frases cortas que coinciden o coinciden estrechamente con el texto visible del control, mientras se beneficia de Narrador de frases más larga y descriptivas para dar un mejor contexto.

Para resolver esto, a partir de Windows 10 Creators Update, se ha actualizado el Narrador para Examine también el `AutomationProperties.HelpText` propiedad.  Si esta propiedad no está vacía, el Narrador hablará de su contenido además `AutomationProperties.Name`.  Si `HelpText` está vacío, el Narrador sólo leerá el contenido del nombre.  Esto permitirá ya cadenas descriptivas que se utilizará cuando sea necesario, pero se mantiene, speech recognition descriptivo frases cortas en la `Name` propiedad.

![](images/ves_narrator.jpg)

Para obtener más información, consulte [propiedades de automatización para la compatibilidad de accesibilidad en la interfaz de usuario](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "propiedades de automatización para la compatibilidad de accesibilidad en la interfaz de usuario").

## <a name="active-listening-mode-alm"></a>Modo de escucha activa (ALM) ##
### <a name="entering-alm"></a>Introducción a ALM ###
En Xbox, VES no está escuchando constantemente para entrada de voz.  El usuario debe entrar en modo de escucha activa explícitamente diciendo:

- "Hola Cortana, seleccione", o
- "Hola Cortana, realice una selección"

Hay varios otros comandos de Cortana que también deja el usuario de escucha activa tras la finalización, por ejemplo "Hola Cortana, inicio de sesión" o "Hola Cortana, ir a Inicio". 

Introducción a ALM tendrá el siguiente efecto:

- Se mostrará la superposición de Cortana en la esquina superior derecha, que indica al usuario que pueden decir lo que ven.  Mientras está hablando el usuario, fragmentos de la frase que son reconocidos por el reconocedor de voz también se mostrará en esta ubicación.
- VES analiza el árbol de UIA, busca todos los controles que requieren acción, registra su texto en la gramática de reconocimiento de voz e inicia una sesión de escucha continua.

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>Saliendo de ALM ###
El sistema permanecerá en ALM mientras el usuario está interactuando con la interfaz de usuario mediante voz.  Hay dos maneras de salir de ALM:

- Usuario explícitamente dice, "dejar de escuchar", o
- Se producirá un tiempo de espera si no hay un reconocimiento positivo en segundos 17 de ALM de escribir o desde el último reconocimiento positivo

## <a name="invoking-controls"></a>Invocar controles ##
Cuando se encuentra en ALM que el usuario puede interactuar con la interfaz de usuario mediante voz.  Si la interfaz de usuario está configurado correctamente (con las propiedades de nombre que coincide con el texto visible), el uso de voz para realizar acciones debe ser una experiencia fluida natural.  El usuario debe ser capaz de decir lo ven en la pantalla.

## <a name="overlay-ui-on-xbox"></a>Superposición de la interfaz de usuario en Xbox ##
El nombre VES se deriva de un control puede ser diferente que el texto real de visible en la interfaz de usuario.  Esto puede ser debido a la `Name` propiedad del control o el archivo adjunto `LabeledBy` elemento que se va a establecer explícitamente en otra cadena.  O bien, el control no tiene texto de la interfaz gráfica de usuario, pero solo un elemento de imagen o icono.

En estos casos, los usuarios necesitan una manera de ver lo que debe tener en cuenta para poder invocar este tipo de control.  Por lo tanto, una vez en active escuchando, sugerencias de voz se pueden mostrar diciendo "Mostrar las etiquetas".  Esto hace que la voz que tip etiquetas aparezcan encima de todos los controles que requieren acción.

Hay un límite de 100 etiquetas, por lo que si la interfaz de usuario de la aplicación tiene controles más procesables que 100 habrá algunas que no tendrá que se muestran las etiquetas de sugerencia de voz.  Qué etiquetas se eligen en este caso no es determinista, ya que depende la estructura y la composición de la interfaz de usuario actual como el primero enumerado en el árbol de UIA.

Una vez que se muestran las etiquetas de sugerencia de voz no hay ningún comando para ocultarlas, permanecerán visibles hasta que se produce uno de los siguientes eventos:

- el usuario invoca un control
- usuario navega fuera de la escena actual
- usuario dice: "dejar de escuchar"
- modo de escucha activa el tiempo de espera

## <a name="location-of-voice-tip-labels"></a>Ubicación de las etiquetas de sugerencia de voz ##
Etiquetas de la sugerencia de voz se centran horizontal y verticalmente dentro BoundingRectangle del control.  Cuando los controles son pequeños y estrechamente agrupados, las etiquetas pueden superposición/quedar ocultos por otros usuarios y VES intentará insertar estas etiquetas a una distancia de separar y asegúrese de que estén visibles.  Sin embargo, esto no garantiza que funcione el 100% del tiempo.  Si hay una interfaz de usuario muy atestado, probablemente como resultado algunas etiquetas que se va a ocultado por otros usuarios. Revise la interfaz de usuario con "Mostrar etiquetas" para asegurarse de que hay espacio suficiente para la visibilidad de la sugerencia de voz.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>Cuadros combinados ##
Cuando un cuadro combinado se expande cada elemento individual en el cuadro combinado obtiene su propia etiqueta de sugerencia de voz y a menudo, estos serán encima de los controles existentes detrás de la lista desplegable.  Para evitar que presentar un confunda desorganizado y confuso de etiquetas (donde las etiquetas de elemento de cuadro combinado se combinan con las etiquetas de controles de cuadro combinado) cuando un cuadro combinado se expande únicamente las etiquetas de sus elementos secundarios se mostrarán;  se ocultarán todas las otras etiquetas de la sugerencia de voz.  El usuario puede, a continuación, seleccione uno de los elementos de lista desplegable o "cerrar" el cuadro combinado.

- Etiquetas de los cuadros combinados contraída:

    ![](images/ves_combo_closed.png)

- Etiquetas de cuadro combinado expandido:

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>Controles desplazables ##
Para los controles desplazables, las sugerencias de voz para los comandos de desplazamiento se centrará en cada uno de los bordes del control.  Sugerencias de voz se mostrará solo para las direcciones de desplazamiento que sean útiles, por ejemplo si no está disponible el desplazamiento vertical, "Desplazar hacia arriba" y "Desplácese hacia abajo" no se mostrarán.  Cuando hay varias regiones desplazables VES usará las posiciones ordinales para diferenciar entre ellos (p ej. "Desplazar derecha 1", "Desplazamiento derecha 2", etcetera.).

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>Desambiguación ##
Cuando varios elementos de interfaz de usuario tienen el mismo nombre, o el reconocedor de voz coincide con varios candidatos, VES pasará al modo de anulación de ambigüedades.  En esta sugerencia de voz de modo las etiquetas se mostrarán para los elementos implicadas para que el usuario puede seleccionar el correcto. El usuario puede cancelar fuera del modo de desambiguación diciendo "Cancelar".

Por ejemplo:

- En modo de escucha activo, antes de la anulación de ambigüedades; usuario dice, "Estoy I ambigua":

    ![](images/ves_disambig1.png) 

- Ambos botones coincide; se inició la anulación de ambigüedades:

    ![](images/ves_disambig2.png) 

- Que se muestra cuando haga clic en acción "Select 2" se ha elegido:

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>Ejemplo de interfaz de usuario ##
Este es un ejemplo de un XAML según la configuración de UI, AutomationProperties.Name de varias maneras:

    <Page
        x:Class="VESSampleCSharp.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:VESSampleCSharp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Button x:Name="button1" Content="Hello World" HorizontalAlignment="Left" Margin="44,56,0,0" VerticalAlignment="Top"/>
            <Button x:Name="button2" AutomationProperties.Name="Launch Game" Content="Launch" HorizontalAlignment="Left" Margin="44,106,0,0" VerticalAlignment="Top" Width="99"/>
            <TextBlock AutomationProperties.Name="Day of Week" x:Name="label1" HorizontalAlignment="Left" Height="22" Margin="168,62,0,0" TextWrapping="Wrap" Text="Select Day of Week:" VerticalAlignment="Top" Width="137"/>
            <ComboBox AutomationProperties.LabeledBy="{Binding ElementName=label1}" x:Name="comboBox" HorizontalAlignment="Left" Margin="310,57,0,0" VerticalAlignment="Top" Width="120">
                <ComboBoxItem Content="Monday" IsSelected="True"/>
                <ComboBoxItem Content="Tuesday"/>
                <ComboBoxItem Content="Wednesday"/>
                <ComboBoxItem Content="Thursday"/>
                <ComboBoxItem Content="Friday"/>
                <ComboBoxItem Content="Saturday"/>
                <ComboBoxItem Content="Sunday"/>
            </ComboBox>
            <Button x:Name="button3" HorizontalAlignment="Left" Margin="44,156,0,0" VerticalAlignment="Top" Width="213">
                <Grid>
                    <TextBlock AutomationProperties.Name="Accept">Accept Offer</TextBlock>
                    <TextBlock Margin="0,25,0,0" Foreground="#FF5A5A5A">Exclusive offer just for you</TextBlock>
                </Grid>
            </Button>
        </Grid>
    </Page>


Usar aquí el ejemplo anterior es el aspecto de la interfaz de usuario con y sin las etiquetas de sugerencia de voz.
 
- En la escucha modo activo, sin las etiquetas que se muestra:

    ![](images/ves_alm_nolabels.png) 

- En la escucha modo activo, después de usuario diga "Mostrar las etiquetas":

    ![](images/ves_alm_labels.png) 

En el caso de `button1`, rellena automáticamente XAML el `AutomationProperties.Name` propiedad utilizando el texto de contenido de texto visible del control.  Se trata de por qué hay una etiqueta de sugerencia de voz, aunque no hay explícita `AutomationProperties.Name` establecido.

Con `button2`, hemos configurado explícitamente la `AutomationProperties.Name` a algo que no sea el texto del control.

Con `comboBox`, hemos usado el `LabeledBy` propiedad para hacer referencia a `label1` como origen de la automatización `Name`y en `label1` establecemos el `AutomationProperties.Name` a una frase más natural a lo que se presenta en la pantalla ("Día de la semana" en su lugar a "Select día de semana").

Por último, con `button3`, VES toma el `Name` desde el primer elemento secundario desde `button3` no tiene un `AutomationProperties.Name` establecido.

## <a name="see-also"></a>Consulte también
- [Fundamentos de UI Automation](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "Fundamentos de UI Automation")
- [Propiedades de automatización para la compatibilidad de accesibilidad en la interfaz de usuario](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "propiedades de automatización para la compatibilidad de accesibilidad en la interfaz de usuario")
- [Preguntas más frecuentes](frequently-asked-questions.md)
- [UWP en Xbox One](index.md)
