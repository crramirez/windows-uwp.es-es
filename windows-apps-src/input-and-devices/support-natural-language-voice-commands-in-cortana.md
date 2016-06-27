---
author: Karl-Bridge-Microsoft
Description: "Aprende cómo ampliar Cortana con comandos de voz más flexibles y naturales, de modo que el usuario pueda decir el nombre de tu aplicación en cualquier lugar del comando."
title: Admitir comandos de voz en lenguaje natural en Cortana
ms.assetid: 281E068A-336A-4A8D-879A-D8715C817911
label: Support natural language voice commands
template: detail.hbs
ms.sourcegitcommit: 077fcc6ff462a771ed56f875d960e46e6f4420fc
ms.openlocfilehash: c9cd6894c61f3d6cfe770d197317a97a804b4119

---

# Admitir comandos de voz en lenguaje natural en Cortana

Amplía **Cortana** con comandos de voz más flexibles y naturales, de modo que el usuario pueda decir el nombre de tu aplicación en cualquier lugar del comando.

**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Elementos y atributos de Definición de comando de voz (VCD) v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


El uso de comandos de voz para ampliar Cortana con la funcionalidad de la aplicación requiere que el usuario especifique tanto la aplicación como el comando o la función que ejecutar. Por lo general, esto se logra al anunciar el nombre de la aplicación al principio o al final del comando de voz. Por ejemplo, "Adventure Works, agregar un nuevo viaje a Las Vegas".

Sin embargo, especificar el nombre de la aplicación de esta forma puede resultar incómodo, forzado o incluso carecer de sentido. En muchos casos, poder decir el nombre de la aplicación en otro lugar del comando es más cómodo y natural y ayuda a que la interacción sea mucho más intuitiva y atractiva para el usuario. El ejemplo anterior, "Adventure Works, agregar un nuevo viaje a Las Vegas". puede cambiarse a "Agregar un nuevo viaje Adventure Works a Las Vegas". o "Agregar un nuevo viaje a Las Vegas mediante Adventure Works".

Puedes configurar los comandos de voz para admitir el nombre de la aplicación como:

-   Prefijo (antes de la frase de comando)
-   Infijo (dentro de la frase de comando)
-   Sufijo (después de la frase de comando)

**Requisitos previos:  **

Este tema se basa en [Iniciar una aplicación en segundo plano con los comandos de voz en Cortana](launch-a-background-app-with-voice-commands-in-cortana.md). A continuación, se muestran las funciones con una aplicación de administración y planificación de viajes llamada **Adventure Works**.

Si acabas de empezar a desarrollar aplicaciones de la Plataforma Universal Windows (UWP), consulta estos temas para familiarizarte con las tecnologías presentadas aquí.

-   [Crear tu primera aplicación](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Directrices sobre la experiencia del usuario:  **

Consulta [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233) para obtener información sobre cómo integrar tu aplicación con **Cortana** y [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obtener sugerencias prácticas y diseñar una aplicación habilitada para la voz que sea útil y atractiva.

## <span id="Specify_an_AppName_element_in_the_VCD"></span><span id="specify_an_appname_element_in_the_vcd"></span><span id="SPECIFY_AN_APPNAME_ELEMENT_IN_THE_VCD"></span>Especificar un elemento **AppName** en el VCD


El elemento **AppName** se usa para especificar un nombre descriptivo de una aplicación en un comando de voz.
```XML
<AppName>Adventure Works</AppName>
```

## <span id="Specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="SPECIFY_WHERE_THE_APP_NAME_CAN_BE_SPOKEN_IN_THE_VOICE_COMMAND"></span>Especificar dónde se puede decir el nombre de la aplicación en el comando de voz


El elemento **ListenFor** tiene un atributo **RequireAppName** que especifica los lugares en los que el nombre de la aplicación puede aparecer en el comando de voz. Este atributo admite cuatro valores.

1.  **BeforePhrase**

    Predeterminado.

    Indica que los usuarios deben decir el nombre de la aplicación antes de la frase de comando.

    Aquí, Cortana espera escuchar "Adventure Works cuándo es mi viaje a Las Vegas".
```xml
<ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
```

2.  **AfterPhrase**

    Indica que los usuarios deben decir el nombre de la aplicación después de la frase de comando.

    El sistema ofrece una lista de frases localizada de conjunciones preposicionales. Esto incluye frases como "mediante", "con" y "en".

    Aquí, Cortana espera escuchar comandos como "Mostrar mi próximo viaje a Las Vegas en Adventure Works" y "Mostrar mi próximo viaje a Las Vegas mediante Adventure Works".
```xml
<ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
```

3.  **BeforeOrAfterPhrase**

    Indica que los usuarios deben decir el nombre de la aplicación antes o después de la frase de comando.

    En la versión sufijo, el sistema ofrece una lista de frases localizada de conjunciones preposicionales. Esto incluye frases como "mediante", "con" y "en".

    Aquí, Cortana espera escuchar comandos como "Adventure Works, muestra mi próximo viaje a Las Vegas" o "Mostrar mi último viaje a Las Vegas en Adventure Works".
``` xml
<ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
```

4.  **ExplicitlySpecified**

    Indica que los usuarios deben decir el nombre de la aplicación exactamente dónde lo especificas en la frase de comando. No es necesario que el usuario diga el nombre de la aplicación antes o después de la frase.

    Debes hacer referencia explícita al nombre de la aplicación con la etiqueta **{builtin:AppName}**.

    Aquí, Cortana espera escuchar comandos como "Adventure Works, muestra mi próximo viaje a Las Vegas" y "Mostrar mi próximo viaje de Adventure Works a Las Vegas".
```xml
<ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
```

## <span id="Special_cases"></span><span id="special_cases"></span><span id="SPECIAL_CASES"></span>Casos especiales

Al declarar un elemento **ListenFor** donde **RequireAppName** es "AfterPhrase" o "ExplicitlySpecified", debes asegurarte de que se cumplan ciertos requisitos:

1.  **{builtin:AppName}** debe aparecer solo una vez cuando **RequireAppName** es "ExplicitlySpecified".

    Con este valor, el sistema no puede deducir que el nombre de la aplicación puede aparecer en el comando de voz. Debes especificar explícitamente la ubicación.

2.  No es posible tener un comando de voz que comience con un elemento **PhraseTopic**, que suele usarse para el reconocimiento de voz de vocabularios grandes. Al menos una palabra debe precederlo.

    Esto ayuda a minimizar la posibilidad de que **Cortana** inicie la aplicación si un comando contiene el nombre de la aplicación o parte de ella, en cualquier parte de la expresión.

    Esta es una declaración no válida que podría dar lugar a que **Cortana** inicie la aplicación **Adventure Works** si el usuario dice algo como "Mostrar reseñas sobre Kinect Adventure Works".
```xml
<ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
```

3.  Debe haber al menos dos palabras en la cadena **ListenFor** además del nombre de la aplicación y las referencias a los elementos **PhraseTopic**.

    Igual que en el caso 2, deberás asegurarte de que los comandos tienen contenido fonético suficiente para minimizar las posibilidades de que la aplicación se inicie de forma accidental.

    Esto ayuda a configurar la aplicación para que tenga el mejor funcionamiento posible, de forma que la aplicación no se inicie de forma incorrecta cuando el usuario dice, por ejemplo, "Buscar Kinect Adventure works".

    Estas son declaraciones no válidas que podrían dar lugar a que **Cortana** iniciara la aplicación **Adventure Works** si el usuario dice algo como "Hola adventure works" o "Buscar Kinect adventure works".
```xml
<ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
<ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Observaciones

Al admitir más variación en la forma en que un usuario puede decir un comando de voz en **Cortana**, también se aumenta la facilidad de uso general de la aplicación.

Evita tener "Hola \[nombre de la aplicación\]" como tu **AppName** o **CommandPrefix**. Es mucho más probable que los usuarios digan "Hola Cortana" para invocar a Cortana mediante la activación por voz y tener "Hola \[nombre de la aplicación\]" en la expresión no suena natural. Por ejemplo, "Hola Cortana, muéstrame mi próximo viaje a Las Vegas en Hola Adventure Works".

Considera agregar variaciones de infijo y sufijo en los comandos de voz existentes. Como se ha mostrado aquí, no hay que realizar mucho esfuerzo para agregar un atributo adicional a los elementos **ListenFor** existentes y admitir variantes de sufijo. Resulta mucho más natural decir "Hola Cortana, muéstrame mi próximo viaje a Las Vegas en Adventure works" en lugar de "Hola Cortana, Adventure Works, muéstrame mi próximo viaje a Las Vegas".

Considera la posibilidad de usar el nombre de la aplicación como un prefijo en los casos donde el comando de voz entre en conflicto con funcionalidades de **Cortana** existentes (llamadas, mensajería, etc.). Por ejemplo, "Adventure Works, envía un mensaje a \[agente de viajes\] sobre el viaje a Las Vegas".

## <span id="Complete_example"></span><span id="complete_example"></span><span id="COMPLETE_EXAMPLE"></span>Ejemplo completo


Este es un archivo VCD que muestra varias maneras de ofrecer comandos de voz con lenguaje más natural.

**Nota** Está permitido disponer de varios elementos **ListenFor**, cada uno con un valor de atributo **RequireAppName** distinto. 

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <span id="related_topics"></span>Artículos relacionados


**Desarrolladores**
* [Interacciones de Cortana](cortana-interactions.md)
* [Definir restricciones de reconocimiento personalizadas](define-custom-recognition-constraints.md)
* [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Diseñadores**
* [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Directrices para el diseño de Voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Muestras**
* [Muestra de comando de voz de Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


