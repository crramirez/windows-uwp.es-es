---
title: Shell habilitado para voz (VES) en Xbox
description: Obtenga información sobre cómo agregar compatibilidad de control de voz a las aplicaciones para UWP en Xbox.
ms.date: 10/19/2017
ms.topic: article
keywords: Shell de Windows 10, UWP, Xbox, voz y voz habilitada
ms.openlocfilehash: f51ec2c93a904893dc337545f634d04affde10fd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685179"
---
# <a name="using-speech-to-invoke-ui-elements"></a>Usar voz para invocar elementos de la interfaz de usuario

El shell habilitado para voz (VES) es una extensión de la plataforma de voz de Windows que permite una experiencia de voz de primera clase dentro de las aplicaciones, lo que permite a los usuarios usar la voz para invocar controles en pantalla e insertar texto mediante el dictado. VES se esfuerza por proporcionar una experiencia de TI de un extremo a otro, como por ejemplo, en todos los dispositivos y shells de Windows, con el mínimo esfuerzo requerido por los desarrolladores de aplicaciones.  Para lograrlo, aprovecha la plataforma de Microsoft Speech y el marco de automatización de la interfaz de usuario (UIA).

## <a name="user-experience-walkthrough"></a>Tutorial de la experiencia del usuario ##
A continuación se ofrece información general sobre lo que un usuario experimentaría al usar VES en Xbox y debería ayudar a establecer el contexto antes de profundizar en los detalles de cómo funciona VES.

- El usuario activa la consola Xbox y desea examinar sus aplicaciones para encontrar algo de interés:

        User: "Hey Cortana, open My Games and Apps"

- El usuario se deja en modo de escucha activo (ALM), lo que significa que la consola está escuchando ahora que el usuario invoca un control que está visible en la pantalla, sin necesidad de decir "Hola Cortana" cada vez.  Ahora el usuario puede cambiar a ver aplicaciones y desplazarse por la lista de aplicaciones:

        User: "applications"

- Para desplazar la vista, el usuario puede simplemente decir:

        User: "scroll down"

- El usuario ve el material gráfico de la aplicación en la que está interesado pero olvidó el nombre.  El usuario solicita que se muestren las etiquetas de las sugerencias de voz:

        User: "show labels"

- Ahora que está claro qué decir, se puede iniciar la aplicación:

        User: "movies and TV"

- Para salir del modo de escucha activa, el usuario indica a Xbox que detenga la escucha:

        User: "stop listening"

- Más adelante, se puede iniciar una nueva sesión de escucha activa con:

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>Dependencia de UI Automation ##
VES es un cliente de automatización de la interfaz de usuario y se basa en la información expuesta por la aplicación a través de los proveedores de automatización de la interfaz de usuario. Se trata de la misma infraestructura ya utilizada por la característica de narrador en plataformas Windows.  UI Automation permite el acceso mediante programación a los elementos de la interfaz de usuario, incluido el nombre del control, su tipo y los patrones de control que implementa.  A medida que cambia la interfaz de usuario en la aplicación, VES reaccionará a los eventos de actualización de UIA y volverá a analizar el árbol de automatización de la interfaz de usuario actualizado para encontrar todos los elementos procesables, para lo que usará esta información para crear una gramática de reconocimiento de voz. 

Todas las aplicaciones UWP tienen acceso al marco de automatización de la interfaz de usuario y pueden exponer información sobre la interfaz de usuario de forma independiente del marco de gráficos en el que se compilan (XAML, DirectX/Direct3D, Xamarin, etc.).  En algunos casos, como XAML, la mayor parte del trabajo se realiza mediante el marco, lo que reduce considerablemente el trabajo necesario para admitir narrador y VES.

Para obtener más información sobre la automatización de la interfaz de usuario, vea [fundamentos de UI Automation](https://msdn.microsoft.com/library/ms753107(v=vs.110).aspx "Fundamentos de UI Automation").

## <a name="control-invocation-name"></a>Nombre de invocación de control ##
VES emplea la siguiente heurística para determinar qué frase se debe registrar con el reconocedor de voz como el nombre del control (es decir, lo que el usuario debe hablar para invocar el control).  Esta es también la frase que se mostrará en la etiqueta de la sugerencia de voz.

Origen de nombre por orden de prioridad:

1. Si el elemento tiene una `LabeledBy` propiedad adjunta, VES usará el `AutomationProperties.Name` de esta etiqueta de texto.
2. `AutomationProperties.Name` del elemento.  En XAML, el contenido de texto del control se usará como valor predeterminado para `AutomationProperties.Name`.
3. Si el control es un ListItem o un botón, VES buscará el primer elemento secundario con un `AutomationProperties.Name`válido.

## <a name="actionable-controls"></a>Controles accionables ##
VES considera que un control es accionable si implementa uno de los siguientes patrones de control de automatización:

- **InvokePattern** (p. ej., Button): representa controles que inician o realizan una única acción inequívoca y no mantienen el estado cuando se activan.

- **TogglePattern** (p. ej., Casilla): representa un control que puede recorrer en iteración un conjunto de Estados y mantener un estado una vez establecido.

- **SelectionItemPattern** (p. ej., Cuadro combinado): representa un control que actúa como contenedor para una colección de elementos secundarios seleccionables.

- **ExpandCollapsePattern** (p. ej., Cuadro combinado): representa controles que se expanden visualmente para mostrar contenido y se contraen para ocultar el contenido.

- **ScrollPattern** (p. ej., List): representa controles que actúan como contenedores desplazables para una colección de elementos secundarios.

## <a name="scrollable-containers"></a>Contenedores desplazables ##
En el caso de los contenedores desplazables que admiten ScrollPattern, VES escuchará los comandos de voz como "desplazamiento a la izquierda", "desplazamiento a la derecha", etc. y llamará a Scroll con los parámetros adecuados cuando el usuario desencadene uno de estos comandos.  Los comandos de desplazamiento se insertan en función del valor de las propiedades `HorizontalScrollPercent` y `VerticalScrollPercent`.  Por ejemplo, si `HorizontalScrollPercent` es mayor que 0, se agregará "desplazamiento a la izquierda", si es inferior a 100, se agregará "desplazamiento a la derecha", y así sucesivamente.

## <a name="narrator-overlap"></a>Superposición de narrador ##
La aplicación narrador también es un cliente de automatización de la interfaz de usuario y usa la propiedad `AutomationProperties.Name` como uno de los orígenes para el texto que lee para el elemento de la interfaz de usuario seleccionado actualmente.  Para proporcionar una mejor experiencia de accesibilidad, muchos desarrolladores de aplicaciones han recurrido a la sobrecarga de la propiedad `Name` con texto descriptivo largo con el objetivo de proporcionar más información y contexto cuando el narrador los Lee.  Sin embargo, esto provoca un conflicto entre las dos características: VES necesita frases cortas que coinciden o coinciden estrechamente con el texto visible del control, mientras que el narrador se beneficia de frases más largas y más descriptivas para proporcionar un mejor contexto.

Para resolver este error, a partir de Windows 10 Creators Update, el narrador se actualizó para ver también la propiedad `AutomationProperties.HelpText`.  Si esta propiedad no está vacía, el narrador hablará su contenido además de `AutomationProperties.Name`.  Si `HelpText` está vacío, el narrador solo leerá el contenido del nombre.  Esto permitirá que se usen cadenas más largas en el caso de que sea necesario, pero mantenga una frase más corta y sencilla de reconocimiento de voz en la propiedad `Name`.

![](images/ves_narrator.jpg)

Para obtener más información, vea [propiedades de automatización para la compatibilidad con accesibilidad en la interfaz de usuario](https://msdn.microsoft.com/library/ff400332(vs.95).aspx "Propiedades de Automation para la compatibilidad con accesibilidad en la interfaz de usuario").

## <a name="active-listening-mode-alm"></a>Modo de escucha activo (ALM) ##
### <a name="entering-alm"></a>Escribir ALM ###
En Xbox, VES no escucha constantemente la entrada de voz.  El usuario debe especificar explícitamente el modo de escucha activa indicando:

- "Hola Cortana, seleccionar" o
- "Hola Cortana, hacer una selección"

Hay otros comandos de Cortana que también dejan que el usuario esté activo escuchando tras su finalización, por ejemplo "Hola a Cortana, Inicio de sesión" o "Hola a Cortana, ir a Inicio". 

Escribir ALM tendrá el siguiente efecto:

- La superposición de Cortana se mostrará en la esquina superior derecha para indicar al usuario que puede decir lo que ven.  Mientras el usuario está hablando, los fragmentos de frases que reconoce el reconocedor de voz también se mostrarán en esta ubicación.
- VES analiza el árbol de UIA, busca todos los controles accionables, registra su texto en la gramática de reconocimiento de voz e inicia una sesión de escucha continua.

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>Saliendo de ALM ###
El sistema permanecerá en ALM mientras el usuario interactúa con la interfaz de usuario mediante el uso de Voice.  Hay dos maneras de salir de ALM:

- El usuario indica explícitamente "dejar de escuchar", o bien
- Se producirá un tiempo de espera si no hay un reconocimiento positivo en los 17 segundos de escribir ALM o desde el último reconocimiento positivo.

## <a name="invoking-controls"></a>Invocar controles ##
Cuando en ALM el usuario puede interactuar con la interfaz de usuario mediante el uso de Voice.  Si la interfaz de usuario está configurada correctamente (con propiedades de nombre que coinciden con el texto visible), el uso de Voice para realizar acciones debe ser una experiencia natural y perfecta.  El usuario debe poder decir simplemente lo que ve en la pantalla.

## <a name="overlay-ui-on-xbox"></a>Interfaz de usuario de superposición en Xbox ##
El nombre VES se deriva para un control puede ser diferente del texto visible real en la interfaz de usuario.  Esto puede deberse a que la propiedad `Name` del control o al elemento `LabeledBy` asociado se establece explícitamente en una cadena diferente.  O bien, el control no tiene texto GUI sino solo un icono o un elemento de imagen.

En estos casos, los usuarios necesitan una manera de ver lo que debe decirse para invocar este tipo de control.  Por lo tanto, una vez en la escucha activa, las sugerencias de voz se pueden mostrar diciendo "Mostrar etiquetas".  Esto hace que las etiquetas de la sugerencia de voz aparezcan encima de cada control accionable.

Hay un límite de 100 etiquetas, por lo que si la interfaz de usuario de la aplicación tiene más controles procesables que 100, habrá algunos que no tendrán las etiquetas de la sugerencia de voz.  Las etiquetas que se eligen en este caso no son deterministas, ya que dependen de la estructura y la composición de la interfaz de usuario actual, como se enumeran por primera vez en el árbol de UIA.

Una vez que se muestran las etiquetas de las sugerencias de voz, no hay ningún comando para ocultarlas, seguirán estando visibles hasta que se produzca uno de los siguientes eventos:

- el usuario invoca un control
- el usuario navega fuera de la escena actual
- el usuario dice "detener escucha"
- el modo de escucha activo agota el tiempo de espera

## <a name="location-of-voice-tip-labels"></a>Ubicación de las etiquetas de la sugerencia de voz ##
Las etiquetas de las sugerencias de voz se centran horizontal y verticalmente en el BoundingRectangle del control.  Cuando los controles son pequeños y agrupados de manera estrecha, las etiquetas pueden superponerse o quedar ocultas por otros y VES intentará empujarlas por separado para separarlas y asegurarse de que están visibles.  Sin embargo, no se garantiza que funcione el 100% del tiempo.  Si hay una interfaz de usuario muy llena, es probable que el resto de etiquetas oculten algunas. Revise la interfaz de usuario con "Mostrar etiquetas" para asegurarse de que hay espacio suficiente para la visibilidad de la sugerencia de voz.

![](images/ves_labels.png)

## <a name="combo-boxes"></a>Cuadros combinados ##
Cuando se expande un cuadro combinado, cada elemento individual del cuadro combinado obtiene su propia etiqueta de la sugerencia de voz y, a menudo, se encuentra en la parte superior de los controles existentes detrás de la lista desplegable.  Para evitar la presentación de una muddle de etiquetas abarrotada y confusa (donde las etiquetas de los elementos del cuadro combinado se combinan con las etiquetas de los controles situados detrás del cuadro combinado) cuando se expande un cuadro combinado, solo se mostrarán las etiquetas de sus elementos secundarios;  todas las demás etiquetas de la sugerencia de voz estarán ocultas.  A continuación, el usuario puede seleccionar uno de los elementos desplegables o "cerrar" el cuadro combinado.

- Etiquetas en los cuadros combinados contraídos:

    ![](images/ves_combo_closed.png)

- Etiquetas en el cuadro combinado expandido:

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>Controles desplazables ##
En el caso de los controles desplazables, las sugerencias de voz para los comandos de desplazamiento se centrarán en cada uno de los bordes del control.  Las sugerencias de voz solo se mostrarán para las direcciones de desplazamiento que son accionables, por lo que, por ejemplo, si el desplazamiento vertical no está disponible, no se mostrará "desplazar hacia arriba" y "desplazar hacia abajo".  Cuando hay varias regiones desplazables presentes, VES usará los ordinales para diferenciarlas (por ejemplo, "Desplazar a la derecha 1", "desplazar a la derecha 2", etc.).

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>Desambiguación ##
Cuando varios elementos de la interfaz de usuario tienen el mismo nombre o el reconocedor de voz coincide con varios candidatos, VES entrará en modo de desambiguación.  En este modo, las etiquetas de la sugerencia de voz se mostrarán para los elementos implicados de forma que el usuario pueda seleccionar la adecuada. El usuario puede cancelar el modo de desambiguación diciendo "Cancelar".

Por ejemplo:

- En el modo de escucha activa, antes de la anulación de ambigüedades; el usuario dice "AM I ambiguo":

    ![](images/ves_disambig1.png) 

- Ambos botones coinciden; Desambiguación iniciada:

    ![](images/ves_disambig2.png) 

- Mostrando la acción de clic cuando se eligió "seleccionar 2":

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>Ejemplo de interfaz de usuario ##
A continuación se muestra un ejemplo de una interfaz de usuario basada en XAML, que establece AutomationProperties.Name de varias maneras:

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


En el ejemplo anterior se muestra el aspecto que tendrá la interfaz de usuario con y sin etiquetas de la sugerencia de voz.
 
- En el modo de escucha activa, sin las etiquetas mostradas:

    ![](images/ves_alm_nolabels.png) 

- En el modo de escucha activa, después de que el usuario indique "Mostrar etiquetas":

    ![](images/ves_alm_labels.png) 

En el caso de `button1`, XAML rellena automáticamente la propiedad `AutomationProperties.Name` utilizando el texto del contenido de texto visible del control.  Este es el motivo por el que hay una etiqueta de sugerencia de voz, aunque no haya un conjunto de `AutomationProperties.Name` explícito.

Con `button2`, establecemos explícitamente el `AutomationProperties.Name` en un valor distinto del texto del control.

Con `comboBox`, usamos la propiedad `LabeledBy` para hacer referencia a `label1` como el origen de la `Name`de automatización y, en `label1`, establecemos la `AutomationProperties.Name` en una frase más natural que la que se representa en la pantalla ("día de la semana" en lugar de "Seleccionar día de la semana").

Por último, con `button3`, VES toma el `Name` del primer elemento secundario, ya que `button3` mismo no tiene `AutomationProperties.Name` establecido.

## <a name="see-also"></a>Consulta también
- [Aspectos básicos de la automatización de la interfaz de usuario](https://msdn.microsoft.com/library/ms753107(v=vs.110).aspx "Fundamentos de UI Automation")
- [Propiedades de Automation para la compatibilidad con accesibilidad en la interfaz de usuario](https://msdn.microsoft.com/library/ff400332(vs.95).aspx "Propiedades de Automation para la compatibilidad con accesibilidad en la interfaz de usuario")
- [Preguntas más frecuentes](frequently-asked-questions.md)
- [UWP en Xbox One](index.md)
