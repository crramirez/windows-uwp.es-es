---
author: lex
Description: The following article describes all of the properties and elements within the toast content XML payload.
title: Esquema XML de contenido del sistema
ms.assetid: AF49EFAC-447E-44C3-93C3-CCBEDCF07D22
label: Toast content XML schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bcfc56264ab3063995fd9f2b06bd93e9406cd37e
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7284241"
---
# <a name="toast-content-xml-schema"></a>Esquema XML de contenido del sistema

 

En el siguiente artículo se describen todas las propiedades y elementos dentro de la carga XML de contenido del sistema.

En los siguientes esquemas XML, el sufijo "?" significa que el atributo es opcional.

## <a name="ltvisualgt-and-ltaudiogt"></a>&lt;visual&gt; y &lt;audio&gt;

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

**Atributos de &lt;toast&gt;**

launch?

-   launch? = string
-   Este atributo es opcional.
-   Una cadena que se pasa a la aplicación cuando la notificación del sistema la activa.
-   Según el valor de activationType, este valor puede recibirlo la aplicación en primer plano, dentro de la tarea en segundo plano, o bien otra aplicación que se inicie con protocolo desde la aplicación original.
-   El formato y el contenido de esta cadena los define la aplicación para su propio uso.
-   Cuando el usuario pulse o haga clic en la notificación del sistema para iniciar la aplicación asociada, la cadena de inicio proporciona el contexto de la aplicación que le permite mostrar al usuario una vista relevante para el contenido de la notificación del sistema, en lugar de iniciarse de forma predeterminada.
-   Si la activación se produjo porque el usuario hizo clic en una acción y no en el cuerpo de la notificación del sistema, el desarrollador recupera los "argumentos" predefinidos en esa etiqueta &lt;action&gt;, en lugar del "inicio" predefinido en la etiqueta &lt;toast&gt;.

duration?

-   duration? = "short|long"
-   Este atributo es opcional. El valor predeterminado es "short".
-   Esto está aquí solo para escenarios específicos y appCompat. Ya no necesitas esto para el escenario de alarma.
-   No recomendamos usar esta propiedad.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   Este atributo es opcional.
-   El valor predeterminado es "foreground".

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   Este es un atributo opcional, el valor predeterminado es "default".
-   No es necesario a menos que en el escenario aparezca una alarma, un recordatorio o una llamada entrante.
-   No usar solo para mantener la notificación de forma persistente en la pantalla.

**Atributos de &lt;visual&gt;**

lang?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230847) para obtener detalles sobre este atributo opcional.

baseUri?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230847) para obtener detalles sobre este atributo opcional.

addImageQuery?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230847) para obtener detalles sobre este atributo opcional.

**Atributos de &lt;binding&gt;**

template?

-   \[Important\] template? = "ToastGeneric"
-   Si estás usando cualquiera de las nuevas características de notificaciones interactivas y adaptables, asegúrate de empezar a usar la plantilla "ToastGeneric" en lugar de la plantilla heredada.
-   Es posible que las plantillas heredadas funcionen con las nuevas acciones ahora, pero este no es su uso previsto y no podemos garantizar que sigan funcionando.

lang?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230847) para obtener detalles sobre este atributo opcional.

baseUri?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230847) para obtener detalles sobre este atributo opcional.

addImageQuery?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230847) para obtener detalles sobre este atributo opcional.

**Atributos de &lt;text&gt;**

lang?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230847) para obtener detalles sobre este atributo opcional.

**Atributos de &lt;image&gt;**

src

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230844) para obtener detalles sobre este atributo obligatorio.

placement?

-   placement? = "inline" | "appLogoOverride"
-   Este atributo es opcional.
-   Especifica dónde se mostrará esta imagen.
-   "inline" significa dentro del cuerpo de la notificación del sistema, debajo el texto; "appLogoOverride" significa reemplazar el icono de la aplicación (que aparece en la esquina superior izquierda de la notificación del sistema).
-   Puedes tener hasta una imagen por cada valor de colocación.

alt?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230844) para obtener detalles sobre este atributo opcional.

addImageQuery?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230844) para obtener detalles sobre este atributo opcional.

hint-crop?

-   hint-crop? = "none" | "circle"
-   Este atributo es opcional.
-   "none" es el valor predeterminado, que significa no recortar.
-   "circle" recorta la imagen en una forma circular. Úsalo para las imágenes de perfil de los contactos, imágenes de personas, etc.

**Atributos de &lt;audio&gt;**

src?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230842) para obtener detalles sobre este atributo opcional.

loop?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230842) para obtener detalles sobre este atributo opcional.

silent?

-   Consulta [este artículo de esquema de elemento](https://msdn.microsoft.com/library/windows/apps/br230842) para obtener detalles sobre este atributo opcional.

## <a name="schemas-ltactiongt"></a>Esquemas: &lt;action&gt;


En los siguientes esquemas XML, el sufijo "?" significa que el atributo es opcional.

```
<toast>
  <visual>
  </visual>
  <audio />
  <actions>
    <input id type title? placeHolderContent? defaultInput? >
      <selection id content />
    </input>
    <action content arguments activationType? imageUri? hint-inputId />
  </actions>
</toast>
```

**Atributos de &lt;input&gt;**

id

-   id = string
-   Este atributo es obligatorio.
-   El atributo id es necesario y los desarrolladores lo usan para recuperar las entradas de usuario una vez activada la aplicación (en primer o en segundo plano).

type

-   type = "text | selection"
-   Este atributo es obligatorio.
-   Se usa para especificar una entrada de texto o una entrada de una lista de selecciones predefinidas.
-   En dispositivos móviles y de escritorio, sirve para especificar si quieres una entrada de cuadro de texto o una entrada de cuadro de lista.

title?

-   title? = string
-   El atributo title es opcional y es para que los desarrolladores especifiquen un título para la entrada que los shells representarán cuando haya prestación.
-   En dispositivos móviles y de escritorio, este título aparecerá encima de la entrada.

placeHolderContent?

-   placeHolderContent? = string
-   El atributo placeHolderContent es opcional y es el texto de sugerencia en gris para el tipo de entrada de texto. Este atributo se omite cuando el tipo de entrada no es "text".

defaultInput?

-   defaultInput? = string
-   El atributo defaultInput es opcional y se usa para proporcionar un valor de entrada predeterminado.
-   Si el tipo de entrada es "text", se tratará como una entrada de cadena.
-   Si el tipo de entrada es "selection", se espera que sea el identificador de una de las opciones disponibles dentro de los elementos de esta entrada.

**Atributos de &lt;selection&gt;**

id

-   Este atributo es obligatorio. Se usa para identificar las selecciones del usuario. El identificador se devuelve a la aplicación.

content

-   Este atributo es obligatorio. Proporciona la cadena que se mostrará en este elemento de la selección.

**Atributos de &lt;action&gt;**

content

-   content = string
-   El atributo content es obligatorio. Proporciona la cadena de texto que se muestra en el botón.

arguments

-   arguments = string
-   El atributo arguments es obligatorio. Describe los datos definidos por la aplicación que la aplicación puede recuperar más adelante cuando se active al realizar el usuario esta acción.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   El atributo activationType es opcional y su valor predeterminado es "foreground".
-   Describe el tipo de activación que provocará esta acción: en primer plano, en segundo plano o el inicio de otra aplicación a través del inicio de protocolo, o bien la invocación de una acción del sistema.

imageUri?

-   imageUri? = string
-   imageUri es opcional y se usa para proporcionar un icono de imagen para que esta acción lo muestre en el botón, únicamente con el contenido del texto.

hint-inputId

-   hint-inputId = string
-   El atributo hint-inpudId es obligatorio. Se usa específicamente para el escenario de respuesta rápida.
-   El valor debe ser el identificador del elemento de entrada con el que se quiere asociar.
-   En dispositivos móviles y de escritorio, colocará el botón justo al lado del cuadro de entrada.

## <a name="attributes-for-system-handled-actions"></a>Atributos para acciones controladas por el sistema


El sistema puede controlar las acciones para posponer y descartar notificaciones si no quieres que la aplicación controle el aplazamiento o la reprogramación de notificaciones como una tarea en segundo plano. Las acciones controladas por el sistema se pueden combinar (o especificarse individualmente), pero no se recomienda implementar una acción para posponer sin una acción para descartar.

Combinación de comandos del sistema: SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

Acciones individuales controladas por el sistema

```
<toast>
  <visual>
  </visual>
  <actions>
  <input id="snoozeTime" type="selection" defaultInput="10">
    <selection id="5" content="5 minutes" />
    <selection id="10" content="10 minutes" />
    <selection id="20" content="20 minutes" />
    <selection id="30" content="30 minutes" />
    <selection id="60" content="1 hour" />
  </input>
  <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
  <action activationType="system" arguments="dismiss" content=""/>
  </actions>
</toast>
```

Para crear acciones individuales para posponer y descartar, haz lo siguiente:

-   Especifica el activationType = "system"
-   Especificar los argumentos = "snooze" | "dismiss"
-   Especifica el contenido:
    -   Si quieres que las cadenas localizadas de "snooze" y "dismiss" se muestren en las acciones, especifica el contenido para que sea una cadena vacía: &lt;action content = ""/&gt;
    -   Si quieres una cadena personalizada, proporciona solo su valor: &lt;action content="Remind me later" /&gt;
-   Especifica la entrada:
    -   Si no quieres que el usuario seleccione un intervalo de aplazamiento y, en su lugar, solo quieres que la notificación se posponga solo una vez en un intervalo de tiempo definido por el sistema (que es coherente en todo el sistema operativo), no crees ningún &lt;input&gt; en absoluto.
    -   Si quieres proporcionar las selecciones de intervalo de aplazamiento:
        -   Especifica hint-inputId en la acción de posponer
        -   Haz coincidir el identificador de la entrada con el hint-inputId de la acción de posponer: &lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   Especifica el identificador de selección para que sea un nonNegativeInteger que represente el intervalo de aplazamiento en minutos: &lt;selection id="240" /&gt; significa pospuesto 4 horas
        -   Asegúrate de que el valor de defaultInput en &lt;input&gt; coincida con uno de los identificadores de los elementos secundarios de &lt;selection&gt;.
        -   Proporciona hasta (pero no más de) 5 valores &lt;selection&gt;