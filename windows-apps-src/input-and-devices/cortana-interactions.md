---
author: Karl-Bridge-Microsoft
Description: "Amplía la funcionalidad básica de Cortana con comandos de voz que inician y ejecutan una acción única en una aplicación externa."
title: Interacciones de Cortana
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: d55ece0112e5360c1de4e194c6dd326c15404f9e

---

# Interacciones de Cortana en las aplicaciones para UWP




Amplía la funcionalidad básica de **Cortana** con comandos de voz que inician y ejecutan una única acción en una aplicación externa. 


**Otros componentes de voz**

-   Consulta las [Directrices para el diseño de voz](speech-interactions.md) si quieres integrar el reconocimiento de voz y texto a voz (denominado también TTS o síntesis de voz) directamente en la aplicación.

> **Nota**  
> Un comando de voz es una única expresión con un determinado propósito, definida en un archivo de definición de comando de voz (VCD), dirigida a una aplicación instalada mediante **Cortana**.

> Un archivo VCD define uno o más comandos de voz, cada uno con un único propósito.

> Una definición de comando de voz puede variar en complejidad. Puede admitir desde una única expresión restringida a una colección de expresiones más flexibles y de lenguaje natural, que denoten el mismo propósito.


Se puede iniciar la aplicación de destino en primer plano (la aplicación toma el foco y se descarta **Cortana**) o activarla en segundo plano (**Cortana** conserva el foco, pero proporciona los resultados de la aplicación), según el nivel y la complejidad de la interacción. Por ejemplo, los comandos de voz que requieren más contexto o la entrada del usuario se administran mejor en una aplicación en primer plano, mientras que los comandos básicos se pueden controlar en **Cortana** a través de una aplicación en segundo plano.

 

Al integrar la funcionalidad básica de tu aplicación y ofrecer un punto de entrada central para que el usuario realice la mayoría de las tareas sin tener que abrir la aplicación directamente, se permite que **Cortana** actúe como enlace entre tu aplicación y el usuario. Proporcionar este acceso directo a la funcionalidad de la aplicación y reducir la necesidad de cambiar las aplicaciones permite que el usuario ahorre tiempo y esfuerzo significativos.


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Artículo</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Directrices de diseño](cortana-design-guidelines.md)</p></td>
<td align="left"><p>Estas directrices y recomendaciones describen cómo la aplicación puede usar mejor **Cortana** para interactuar con el usuario, ayudarle a realizar una tarea y comunicar claramente lo que está sucediendo.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Iniciar una aplicación en primer plano con comandos de voz](launch-a-foreground-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Además de usar los comandos de voz en <strong>Cortana</strong> para acceder a las funciones del sistema, también puedes usar los comandos de voz a través de <strong>Cortana</strong> para iniciar una aplicación en primer plano y especificar una acción o un comando para que se ejecute dentro de la aplicación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Modificar listas de frases de VCD de forma dinámica](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)</p></td>
<td align="left"><p>Aprende a obtener acceso y actualizar la lista de frases admitidas (elementos <strong>PhraseList</strong>) en un archivo VCD mediante el resultado de reconocimiento de voz en tiempo de ejecución.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Iniciar una aplicación en segundo plano con comandos de voz](launch-a-background-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Además de usar los comandos de voz en <strong>Cortana</strong> para tener acceso a las características del sistema, también puedes ampliar <strong>Cortana</strong> con características y funcionalidades desde una aplicación en segundo plano con comandos de voz que especifiquen una acción o un comando que se debe ejecutar dentro de la aplicación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Interactuar con una aplicación en segundo plano](interact-with-a-background-app-in-cortana.md)</p></td>
<td align="left"><p>Obtén información sobre cómo puede interactuar un usuario con una aplicación en segundo plano mediante el lienzo y la voz de <strong>Cortana</strong> durante la ejecución de un comando de voz.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Vínculo profundo para una aplicación en segundo plano](deep-link-into-your-app-from-cortana.md)</p></td>
<td align="left"><p>Proporciona vínculos profundos desde el servicio de la aplicación en segundo plano en <strong>Cortana</strong> para iniciar la aplicación en primer plano en un contexto o un estado determinado.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Admitir comandos de voz en lenguaje natural](support-natural-language-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Aprende cómo ampliar <strong>Cortana</strong> con comandos de voz más flexibles y naturales, de modo que el usuario pueda decir el nombre de la aplicación en cualquier lugar del comando.</p></td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>Artículos relacionados


* [**VCD elements and attributes v1.2 (Elementos y atributos de VCD v1.2)**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Diseñadores**
* [Directrices para el diseño de voz](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Directrices para el diseño de Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)

**Muestras**
* [Muestra de comando de voz de Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


