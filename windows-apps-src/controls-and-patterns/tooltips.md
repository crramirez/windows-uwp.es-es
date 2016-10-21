---
author: Jwmsft
Description: "Usa información sobre herramientas para ofrecer más información sobre un control antes de pedir al usuario que realice una acción."
title: "Información sobre herramientas"
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 4110f902adf01e5e25ac674faf9be8faf61f4ea0


---
# Información sobre herramientas
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 




La información sobre herramientas es una breve descripción que está vinculada con otro control u objeto. Dicha información sobre herramientas ayuda a los usuarios a comprender objetos que no les son familiares y que no están directamente descritos en la UI. Aparece de forma automática cuando el usuario mueve el foco al control, lo mantiene presionado o cuando pasa sobre él con el puntero del mouse. La información sobre herramientas desaparece tras unos segundos o cuando el usuario mueve el dedo, el puntero o el foco del teclado o del controlador para juegos.

![Una información sobre herramientas](images/controls/tool-tip.png)

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/br227608"><strong>Clase ToolTip</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltipservice"><strong>Clase ToolTipService</strong></a></li>
</ul>

</div>
</div>




## ¿Es este el control adecuado?

Usa información sobre herramientas para ofrecer más información sobre un control antes de pedirle al usuario que realice una acción. Los controles de información sobre herramientas deben usarse de vez en cuando y solamente cuando aporten un valor determinado para el usuario que intenta finalizar una tarea. Por regla general, si la información se encuentra disponible en todas partes en la misma experiencia, no es necesaria la información sobre herramientas. Esta información será valiosa cuando explique una acción poco clara.

¿Cuándo deberías usar información sobre herramientas? Para decidirte, intenta responder a estas preguntas:

-   **¿Debe ser visible la información en función de desplazamiento del puntero?**
    Si no es así, usa otro control. Las sugerencias deben aparecer como resultado de la interacción del usuario, y nunca deben mostrarse de forma indiscriminada.

-   **¿Tiene el control una etiqueta de texto?**
    Si no es así, usa la información sobre herramientas para proporcionar la etiqueta que no tiene. Una buena práctica de diseño de la experiencia de usuario consiste en etiquetar la mayoría de los controles en línea, y para esos no necesitarás usar información sobre herramientas. Los controles de la barra de herramientas y los botones de comando que muestran solo iconos necesitan información sobre herramientas.

-   **¿Se beneficiaría el objeto de una descripción o de cierta información adicional?**
    Si es así, usa la información sobre herramientas. Pero el texto debe ser complementario, es decir, no debe ser algo esencial para la tarea principal. Si es un concepto esencial, deberías ponerlo directamente en la UI para que los usuarios no tengan que descubrirlo ni buscarlo.

-   **¿Es la información complementaria un error, una advertencia o un estado?**
    Si es así, usa otro elemento de UI, como un control flotante.

-   **¿Es necesario que los usuarios interactúen con el texto de información?**
    Si es así, usa otro control. Los usuarios no pueden interactuar con el texto de información ya que en cuanto se mueve el mouse el texto desaparece.

-   **¿Necesitan los usuarios imprimir la información complementaria?**
    Si es así, usa otro control.

-   **¿Es posible que el texto de información estorbe o distraiga a los usuarios?**
    Si es así, plantéate la opción de usar otra solución, sin descartar la idea de no hacer nada. Si vas a usar sugerencias en lugares donde pueden resultar una distracción, permite que los usuarios puedan desactivarlas.

## Ejemplo

Una información sobre herramientas de la aplicación Mapas de Bing.

![Una información sobre herramientas de la aplicación Mapas de Bing](images/control-examples/tool-tip-maps.png)

## Recomendaciones

-   Usa información sobre herramientas con moderación (o no la uses en absoluto). La información sobre herramientas es una interrupción. La información sobre herramientas puede distraer tanto como un elemento emergente, así que no la uses a no ser que aporte un valor importante.
-   El texto de la información sobre herramientas debe ser conciso. La información sobre herramientas son perfectas para frases cortas y fragmentos de frases. Los grandes bloques de texto pueden ser abrumadores y la información sobre herramientas puede agotar el tiempo antes de que el usuario haya terminado de leer.
-   Crea texto de información sobre herramientas complementario y de utilidad. El texto de la información sobre herramientas debe ser informativo. Trata de que no sea información obvia ni repetir lo que ya hay en pantalla. Como la información sobre herramientas no está siempre visible, debería ser información complementaria que no sea obligatorio que los usuarios lean. Comunica información importante con etiquetas de control que se expliquen por sí solas o texto complementario en contexto.
-   Usa imágenes cuando sea conveniente. En algunas ocasiones resulta mejor usar una imagen en la información sobre herramientas. Por ejemplo, cuando un usuario mantiene el puntero sobre un hipervínculo puedes usar la información sobre herramientas para mostrar una vista previa de la página vinculada.
-   No uses la información sobre herramientas para mostrar texto que ya aparece en la interfaz de usuario. Por ejemplo, no pongas información sobre herramientas en un botón si muestra el mismo texto del botón.
-   No pongas controles interactivos dentro de la información sobre herramientas.
-   No pongas imágenes que parezcan interactivas dentro de la información sobre herramientas.

Temas relacionados
-----------------------------------------------

* [**Clase ToolTip**](https://msdn.microsoft.com/library/windows/apps/br227608)



<!--HONumber=Aug16_HO3-->


