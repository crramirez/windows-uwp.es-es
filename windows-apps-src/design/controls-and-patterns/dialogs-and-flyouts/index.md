---
Description: Los cuadros de diálogo y los controles flotantes muestran elementos transitorios de la interfaz de usuario que aparecen cuando el usuario los solicita o cuando sucede algo que requiere notificación o aprobación.
title: Cuadros de diálogo y controles flotantes
template: detail.hbs
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d542c9809eb939553a851508886e34265f269263
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57583318"
---
# <a name="dialogs-and-flyouts"></a>Cuadros de diálogo y controles flotantes



Los cuadros de diálogo y los controles flotantes son elementos transitorios de la interfaz de usuario que aparecen cuando sucede algo que requiere notificación, aprobación o información adicional por parte del usuario.

> **API importantes**: [Clase ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [clase Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)


:::row:::
    :::column:::
        **Dialogs**
        
        ![Example of a dialog](../images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](../images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
:::row-end:::


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Los cuadros de diálogo y los controles flotantes garantizan que los usuarios estén al tanto de información importante, pero también interrumpen la experiencia del usuario. Dado que los cuadros de diálogo son modales (bloqueo), interrumpen a los usuarios, de forma que les impiden realizar cualquier otra acción hasta que interactúan con ellos. Los controles flotantes ofrecen una experiencia menos molesta, pero si se muestran en exceso, pueden resultar perturbadores.

Una vez que hayas determinado que quieres usar un cuadro de diálogo o un control flotante, debes elegir cuál quieres usar.

Dado que los cuadros de diálogo bloquean las interacciones y los controles flotantes no, los cuadros de diálogo deben reservarse para situaciones en las que quieres que el usuario centre toda su atención en un dato específico o responda a una pregunta. Los controles flotantes, por el contrario, se pueden usar si quieres llamar la atención sobre algo, pero no hay problemas si el usuario quiere pasarlo por alto.

:::row:::
    :::column:::
   <p><b>Usa un cuadro de diálogo para...</b> <br/>
<ul>
<li>Expresar información importante que el usuario <b>debe</b> leer y confirmar antes de continuar. Algunos ejemplos son:
<ul>
  <li>Los casos en que se puede ver comprometida la seguridad del usuario.</li>
  <li>Los casos en que el usuario está a punto de modificar un activo valioso de manera permanente</li>
  <li>Los casos en que el usuario está a punto de eliminar un activo valioso</li>
  <li>Para confirmar una compra desde la aplicación</li>
</ul>

</li>
<li>Mensajes de error que se aplican al contexto general de la aplicación, como un error de conectividad.</li>
<li>Preguntas cuando la aplicación deba hacer al usuario una pregunta de bloqueo, por ejemplo, cuando la aplicación no puede elegir en nombre del usuario. Una pregunta de bloqueo no se puede ignorar ni posponer, y debe ofrecer al usuario opciones bien definidas.</li>
</ul>
</p>
    :::column-end:::
    :::column:::
   <p><b>Usa un control flotante para...</b> <br/>
<ul>
<li>Recopilar información adicional necesaria para poder completar una acción.</li>
<li>Mostrar información que solo sea pertinente algunas veces. Por ejemplo, en una aplicación de galería fotográfica, cuando el usuario hace clic en una imagen en miniatura, podrías usar un control flotante para mostrar una versión grande de la imagen.</li>
<li>Mostrar más información, como detalles o descripciones más largas de un elemento de la página.</li>
</ul></p>
    :::column-end:::
:::row-end:::


## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>Formas de evitar el uso de cuadros de diálogo y controles flotantes

Ten en cuenta la importancia de la información que quieres compartir: ¿es lo suficientemente importante como para que interrumpa al usuario? Considera también la frecuencia con la que debe mostrarse la información. Si vas a mostrar un cuadro de diálogo o una notificación cada pocos minutos, tal vez quieras asignar espacio para dicha información en la interfaz de usuario principal. Por ejemplo, en un cliente de chat, en lugar de mostrar un control flotante cada vez que un amigo inicie sesión, podrías mostrar una lista de amigos que estén en línea en ese momento y resaltar los amigos a medida que inicien sesión.

Los cuadros de diálogo se usan con frecuencia para confirmar una acción (por ejemplo, eliminar un archivo) antes de ejecutarla. Si esperas que el usuario realice una acción concreta con frecuencia, considera la posibilidad de ofrecerle una forma de deshacer la acción si fuese un error, en lugar de obligarlo a confirmar la acción cada vez.

## <a name="how-to-create-a-dialog"></a>Creación de un cuadro de diálogo

Consulta el [artículo sobre los cuadros de diálogo](dialogs.md). 

## <a name="how-to-create-a-flyout"></a>Creación de un control flotante

Consulta el [artículo sobre los controles flotantes](flyouts.md). 

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para abrirla y ver <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> o <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

