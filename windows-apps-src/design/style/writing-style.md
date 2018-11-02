---
author: QuinnRadich
title: Estilo de escritura
description: Usar un tono y voz adecuados es fundamental para que el texto de la aplicación parezca una parte natural de su diseño.
keywords: UWP, Windows 10, texto, escritura, voz, tono, diseño, IU, UX
ms.author: quradic
ms.date: 5/7/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8022b3bb5ca312be259c554f46dc9f432ea3caeb
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5944239"
---
# <a name="writing-style"></a>Estilo de escritura

![imagen de encabezado](images/header-writing-style.gif)

La forma en la que elaboras un mensaje de error, la manera en que escribes la documentación de ayuda e incluso el texto que eliges para un botón tienen un gran impacto en la facilidad de uso de la aplicación. El estilo de escritura puede suponer una gran diferencia entre una experiencia de usuario terrible y otra mejor.

## <a name="voice-and-tone-principles"></a>Principios de voz y tono

La investigación muestra que las personas responden mejor a un estilo de escritura descriptivo, útil y conciso. Como parte de esta investigación, Microsoft ha desarrollado tres principios de voz y tono que aplicamos en todo nuestro contenido y que forman parte integral de Fluent Design.

### <a name="be-warm-and-relaxed"></a>Sé amable y distendido

Por encima de todo, no quieres ahuyentar al usuario. Se informal y no usen términos que no se vayan a entender. Incluso cuando las cosas se rompen, no culpes al usuario de ningún problema. En vez de eso, tu aplicación debe asumir la responsabilidad y ofrecer instrucciones amables que sitúen las acciones del usuario en primer lugar.

### <a name="be-ready-to-lend-a-hand"></a>Estar preparado para echar una mano

Muestra siempre empatía al escribir. Céntrate en explicar lo que está sucediendo y en proporcionar la información que el usuario necesita, sin sobrecargarla con información innecesaria. Y si es posible, proporciona siempre una solución cuando haya un problema.

### <a name="be-crisp-and-clear"></a>Sé claro y nítido

La mayoría de las veces, el texto no es el foco de una aplicación. Está ahí para guiar a los usuarios, para enseñarles qué está sucediendo y qué hacer a continuación. No pierdas esto de vista al escribir texto en la aplicación y no des por hecho que los usuarios leen cada palabra. Usa un lenguaje que conozca tu público y asegúrate de que es fácil de comprender de un vistazo.

## <a name="lead-with-whats-important"></a>Comienza con lo que es importante

Los usuarios deben poder leer y comprender el texto de un solo vistazo. No rellenes el texto con introducciones innecesarias. Da la máxima visibilidad a los puntos principales y siempre presenta el fundamento de una idea antes de agregarla.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        Select **filters** to add effects to your image.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        If you want to add visual effects or alterations to your image, select **filters.**
    :::column-end:::
:::row-end:::

## <a name="emphasize-action"></a>Enfatiza la acción

Las aplicaciones se definen mediante acciones. Los usuarios realizan una acción cuando usan la aplicación y esta realiza una acción cuando responde a los usuarios. Asegúrate de que el texto usa la *voz activa* en toda la aplicación. Las personas y las funciones deben describirse usando la forma activa, en lugar de la forma pasiva.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        Restart the app to see your changes.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        The changes will be applied when the app is restarted.
    :::column-end:::
:::row-end:::

## <a name="short-and-sweet"></a>Breve y simple

Los usuarios echan un vistazo al texto y a menudo pasarán por alto bloques más grandes de palabras. No sacrifiques información ni presentaciones necesarias, pero no uses más palabras de las que necesites. En ocasiones, esto significa usar numerosas frases o fragmentos más cortos. Otras veces, esto significa ser muy selectivo a la hora de elegir palabras y estructuras de frases más largas.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        We couldn't upload the picture. If this happens again, try restarting the app. But don't worry — your picture will be waiting when you come back.
    :::column-end:::
    :::column:::
        ![Don't](images/dont.svg)
        An error occured, and we weren't able to upload the picture. Please try again, and if you encounter this problem again, you may need to restart the app. But don't worry — we've saved your work locally, and it'll be waiting for you when you come back.
    :::column-end:::
:::row-end:::

## <a name="style-conventions"></a>Convenciones de estilo

Si no te consideras escritor, puede que te resulte intimidante intentar implementar estos principios y recomendaciones. Pero no te preocupes, usar un lenguaje sencillo y directo es una manera fantástica de proporcionar una buena experiencia de usuario. Y si aun así, no estás seguro de cómo estructurar tus palabras, aquí te indicamos algunas instrucciones útiles. Y si quieres obtener más información, consulta la [Guía de estilo de Microsoft](https://docs.microsoft.com/style-guide/welcome/).

### <a name="addressing-the-user"></a>Tratamiento del usuario

Habla directamente al usuario.
* Trate siempre al usuario de"tú".
* Para hacer referencia a tu propia perspectiva, usa "nosotros". Resulta amable y ayuda al usuario a sentirse parte de la experiencia.
* No uses "yo" para hacer referencia a la perspectiva de la aplicación, incluso si eres el único que la ha creado.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        We couldn't save your file to that location.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="abbreviations"></a>Abreviaturas

 Las abreviaturas pueden ser útiles cuando se necesita hacer referencia a productos, lugares o conceptos técnicos varias veces en toda la aplicación. Pueden ahorrar espacio y ser más naturales, siempre y cuando el usuario los comprenda.
* No asumas que los usuarios ya están familiarizados con las abreviaturas, incluso si crees que son habituales.
* Define siempre lo que significa una abreviatura nueva la primera vez que el usuario la vea.
* No uses abreviaturas muy parecidos entre sí.
* No uses abreviaturas si estás localizando tu aplicación o si los usuarios hablan inglés como segundo idioma.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        The Universal Windows Platform (UWP) design guidance is a resource to help you design and build beautiful, polished apps. With the design features that are included in every UWP app, you can build user interfaces (UI) that scale across a range of devices.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="contractions"></a>Contracciones

Usa un tono informal para dirigirte a los usuarios. Si usas un tono formal, la aplicación parecerá demasiado formal o incluso forzada.
* Usa contracciones cuando resulten naturales en el texto.
* No uses contracciones forzadas solo para ahorrar espacio o cuando parezca que, al usarlas, el texto sea menos informal.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        When you're happy with your image, select **save** to add it to your gallery. From there, you'll be able to share it with friends.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="periods"></a>Puntos

 Finalizar el texto con un punto implica que el texto es una frase completa. Usa un punto para bloques de texto más grandes y evítalos para texto que sea más corto que una frase completa.
* Usa puntos para terminar oraciones completas en información sobre herramientas, mensajes de error y cuadros de diálogo.
* No termines con un punto el texto de botones, botones de radio, etiquetas o casillas.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>You’re not connected.</b>
        * Check that your network cables are plugged in.
        * Make sure you're not in airplane mode.
        * See if your wireless switch is turned on.
        * Restart your router.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="capitalization"></a>Uso de mayúsculas

Aunque las letras mayúsculas son importantes, es fácil caer en un uso excesivo de ellas.
* Escribe en mayúscula los nombres propios.
* Escribe en mayúsculas el comienzo de cada cadena de texto en tu aplicación: el inicio de cada frase, etiqueta y título.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>Which part is giving you trouble?</b>
        * I forgot your password.
        * It won't accept password.
        * Someone else might be using my account.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

## <a name="error-messages"></a>Mensajes de error

Cuando se produce un error en la aplicación, los usuarios prestan atención. Dado que los usuarios pueden estar confusos o sentirse frustrados cuando ven un mensaje de error, se encuentra en una zona donde usar un buen tono puede tener un impacto particularmente importante.

Sobre todo, es importante que el mensaje de error no culpe al usuario. Pero también es importante no abrumarles con información que no comprendes. La mayoría de las veces un usuario que encuentra un mensaje de error solo desea volver a lo que estaba haciendo de la manera más rápida y sencilla posible. Por lo tanto, cualquier mensaje de error que escribas debería:

* **Sé amable y distendido**; usa un tono de conversación y evita términos desconocidos y una jerga técnica.

* **Está preparado para echar una mano**; indica al usuario qué ha ocurrido de la manera más sencilla posible y qué sucederá a continuación, y ofrécele una solución realista que pueda realizar.

* **Sé claro y nítido**; elimina información superflua.

:::row:::
    :::column:::
        ![Do](images/do.svg)
        <b>You’re not connected.</b>
        * Check that your network cables are plugged in.
        * Make sure you're not in airplane mode.
        * See if your wireless switch is turned on.
        * Restart your router.
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end::: 

## <a name="dialogs"></a>Cuadros de diálogo

:::row:::
    :::column:::
        Many of the same advice for writing error messages also applies when creating the text for any dialogs in your app. While dialogs are expected by the user, they still interrupt the normal flow of the app, and need to be helpful and concise so the user can get back to what they were doing.

        But most important is the "call and response" between the title of a dialog and its buttons. Make sure that your buttons are clear answers to the question posed by the title, and that their format is consistent across your app.
    :::column-end:::
    :::column:::
        ![Do](images/do.svg)
        <b>Which part is giving you trouble?</b>
        1. I forgot my password
        2. It won't accept my password
        3. Someone else might be using my account
    :::column-end:::
:::row-end:::

## <a name="buttons"></a>Botones

:::row:::
    :::column:::
        Text on buttons needs to be concise enough that users can read it all at a glance and clear enough that the button's function is immediately obvious. The longest the text on a button should ever be is a couple short words, and many should be shorter than that.
        When writing the text for buttons, remember that every button represents an action. Be sure to use the *active voice* in button text, to use words that represent actions rather than reactions.
    :::column-end:::
    :::column:::
        ![Do](images/do.svg)
        * Install now
        * Share
    :::column-end:::
:::row-end:::

## <a name="spoken-experiences"></a>Experiencias con voz

Se aplican los mismos principios generales y recomendaciones al escribir texto para experiencias con voz, como Cortana. En esas características, los principios de la buena escritura son aún más importantes, porque no se puede proporcionar a los usuarios otros elementos de diseño visual para complementar las palabras habladas.

* **Sé amable y distendido**, interactúa con los usuarios con un tono de conversación. Más que en cualquier otra área, resulta fundamental que una experiencia con voz resulte amable y accesible, y que sea algo a lo que los usuarios no tengan miedo de hablar.

* **Está preparado para echar una mano**; proporciona sugerencias alternativas cuando el usuario pida lo imposible. De manera muy similar a un mensaje de error, si algo salió mal y tu aplicación no puede completar la solicitud, debes dar al usuario una alternativa realista que pueda intentar pedir, en su lugar.

* **Sé claro y nítido**; mantén un lenguaje sencillo. Las experiencias con voz no son adecuados para frases largas o palabras complicadas.

## <a name="accessibility-and-localization"></a>Accesibilidad y localización

Tu aplicación puede llegar a un público más amplio si se crea teniendo presentes la accesibilidad y la localización. Esto es algo que no solo se puede lograr a través del texto, aunque el lenguaje sencillo y amable es un buen comienzo. Para obtener más información, consulta nuestra [información general sobre accesibilidad](https://docs.microsoft.com/windows/uwp/design/accessibility/accessibility-overview) y las [instrucciones para localización](https://docs.microsoft.com/windows/uwp/design/globalizing/globalizing-portal).

* **Está preparado para echar una mano**; ten en cuenta diferentes experiencias. Evita las frases que podrían no tener sentido para un público internacional y no uses palabras que hagan suposiciones sobre lo que el usuario puede y no puede hacer.

* **Sé claro y nítido**; evita palabras inusuales y especializadas cuando no sean necesarias. Cuanto más sencillo sea el texto, más fácil será de localizar.


## <a name="techniques-for-non-writers"></a>Técnicas para no escritores

No es necesario ser un escritor con formación o experiencia para proporcionar a los usuarios una buena experiencia. Elige palabras que te resulten cómodas: el usuario también se sentirá cómodo con ellas. Pero a veces, no es tan sencillo como parece. Si sigues teniendo problemas, estas técnicas pueden ayudarte. 

* Imagina que estás hablando a un amigo acerca de la aplicación. ¿Cómo les explicarías en qué consiste la aplicación? ¿Cómo hablarías de sus características o les darías instrucciones? Aún mejor, explica la aplicación a una persona real que no la haya usado todavía. 

* Imagina cómo describías una aplicación totalmente diferente. Por ejemplo, si vas a crear un juego, piensa en lo que dirías o escribirías para describir una aplicación financiera o de noticias. El contraste en el lenguaje y la estructura que usas puede darte más información sobre las palabras adecuadas para aquello sobre los que escribes realmente.

* Echa un vistazo a aplicaciones similares para obtener inspiración. 

Encontrar las palabras adecuadas es un problema que muchas personas tratan de hacer frente, así que no te sientas mal si no te resulta fácil encontrar algo que suene natural.