---
author: Karl-Bridge-Microsoft
Description: "Aprende a obtener acceso y a actualizar la lista de frases admitidas (elementos PhraseList) en un archivo de definición de comando de voz (VCD), mediante el resultado de reconocimiento de voz en tiempo de ejecución."
title: "Modificar listas de frases de VCD de forma dinámica"
ms.assetid: 98024EAC-EC0E-44AA-AEC5-A611BA7C5884
label: Modify VCD phrase lists
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 623243b94cf8ef6b276f8f2971af7bbdbdece81c

---

# Modificar listas de frases de VCD de forma dinámica





**API importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Actualiza la lista de frases admitidas (elementos **PhraseList**) y accede a ella en un archivo de definición de comando de voz (VCD) en tiempo de ejecución mediante el resultado de reconocimiento de voz.

La modificación de forma dinámica de una lista de frases en tiempo de ejecución es útil si el comando de voz es específico para una tarea que implique algún tipo de datos transitorios de aplicaciones o definidos por el usuario. 

Por ejemplo, supongamos que tienes una aplicación de viajes donde los usuarios pueden escribir destinos y quieres que los usuarios puedan iniciar la aplicación diciendo el nombre de la aplicación seguido de "Muéstrame el viaje a &lt;destino&gt;". A continuación, en el mismo elemento **ListenFor**, especificarías algo como: `<ListenFor> Show trip to {destination}  </ListenFor>`, donde "destination" es el valor del atributo **Label** de **PhraseList**.

La actualización de la lista de frases en tiempo de ejecución elimina la necesidad de crear un elemento **ListenFor** para cada destino posible. En su lugar, puedes rellenar dinámicamente **PhraseList** con destinos especificados por el usuario a medida que escriben sus itinerarios. 

Para obtener más información sobre **PhraseList** y otros elementos de VCD, consulta la referencia [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593).

**Requisitos previos:  **

Este tema se basa en la guía [Iniciar una aplicación en primer plano con los comandos de voz en Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md). A continuación, se muestran las funciones con una aplicación de administración y planificación de viajes llamada **Adventure Works**.

Si acabas de empezar a desarrollar aplicaciones de la Plataforma Universal Windows (UWP), consulta estos temas para familiarizarte con las tecnologías presentadas aquí.

-   [Crear tu primera aplicación](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Encontrarás más información acerca de los eventos, en [Introducción a eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Directrices sobre la experiencia del usuario:  **

Consulta [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233) para obtener información sobre cómo integrar tu aplicación con **Cortana** y [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obtener sugerencias prácticas y diseñar una aplicación habilitada para la voz que sea útil y atractiva.

## <span id="Identify_the_command"></span><span id="identify_the_command"></span><span id="IDENTIFY_THE_COMMAND"></span>Identificar el comando y actualizar la lista de frases

A continuación, se incluye un ejemplo del archivo VCD que define un elemento **Command** "showTripToDestination" y un elemento **PhraseList** que define tres opciones para un destino de nuestra aplicación de viajes **Adventure Works**. A medida que el usuario guarda y elimina destinos en la aplicación, esta actualiza las opciones del elemento **PhraseList**.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>

```

Para actualizar un elemento **PhraseList** en el archivo VCD, obtén el elemento **CommandSet** que contiene la lista de frases. Usa el atributo **Name** del elemento **CommandSet** (ten en cuenta que **Name** debe ser único en el archivo VCD) como clave para acceder a la propiedad [**VoiceCommandManager.InstalledCommandSets**](https://msdn.microsoft.com/library/windows/apps/dn653257) y obtener la referencia [**VoiceCommandSet**](https://msdn.microsoft.com/library/windows/apps/dn653258).

Después de identificar el conjunto de comandos, obtén una referencia para la lista de frases que quieres modificar y llama al método [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261); a continuación, usa el atributo **Label** del elemento **PhraseList** y una matriz de cadenas para que sea el nuevo contenido de la lista de frases.

**Nota**  Si modificas una lista de frases, se reemplazará toda la lista de frases. Si quieres insertar nuevos elementos en una lista de frases, debes especificar tanto los elementos existentes como los elementos nuevos cuando llames a [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261).

En este ejemplo, actualizamos el elemento **PhraseList** que se muestra en el ejemplo anterior con un destino adicional a Phoenix.

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommnadDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Comentarios


Si tienes un conjunto relativamente reducido de palabras, te recomendamos usar un elemento **PhraseList** para restringir el reconocimiento. Cuando el conjunto de palabras sea demasiado grande (de cientos de palabras, por ejemplo) o si no debe restringirse de ninguna manera, usa los elementos **PhraseTopic** y **Subject** para refinar la relevancia de los resultados del reconocimiento de voz y mejorar la escalabilidad.

En nuestro ejemplo, tenemos un elemento **PhraseTopic** que consta de una propiedad **Scenario** que indica "Búsqueda" y que se ha refinado aún más gracias al elemento **Subject** que indica "Ciudad\\Estado".

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <span id="related_topics"></span>Artículos relacionados


**Desarrolladores**
* [Interacciones de Cortana](cortana-interactions.md)
* [Iniciar una aplicación en primer plano con los comandos de voz en Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md)
* [Iniciar una aplicación en segundo plano con los comandos de voz en Cortana](launch-a-background-app-with-voice-commands-in-cortana.md)
* [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Diseñadores**
* [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Directrices para el diseño de Voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Muestras**
* [Muestra de comando de voz de Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


