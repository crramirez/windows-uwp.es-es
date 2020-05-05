---
ms.openlocfilehash: 5340c8375be9cf33a8f56fdba7bd36e9743767ab
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80482306"
---
# <a name="contributing-to-uwp-conceptual-documentation"></a>Contribución a la documentación conceptual de UWP

Gracias por tu interés en la documentación de la Plataforma universal de Windows (UWP). Agradecemos tus comentarios, modificaciones y adiciones a nuestros documentos.

## <a name="writing-content"></a>Escritura de contenido

Nuestra documentación se escribe en Markdown, una sintaxis de estilo de texto ligero. Si no estás familiarizado con Markdown, puedes [conocer los conceptos básicos en GitHub](https://guides.github.com/features/mastering-markdown/). Si no estás seguro, siempre puedes copiar el estilo de formato de otras páginas de nuestros documentos.

## <a name="public-contributions"></a>Contribuciones públicas

Si **no** eres empleado de Microsoft, puedes contribuir mediante el [repositorio de contenido público](https://github.com/MicrosoftDocs/windows-uwp). Las contribuciones públicas son adecuadas para realizar cambios y aclaraciones en las páginas existentes.

### <a name="editing-a-file"></a>Edición de un archivo

Si ya estás en el repositorio de contenido público, para comenzar ve hasta el archivo que quieres cambiar. Desde allí, selecciona el icono de lápiz situado encima del contenido para empezar a editarlo.

Si estás viendo una página en [docs.microsoft.com](https://docs.microsoft.com), una alternativa es seleccionar el botón **Editar** de la parte superior derecha de la página. Esta acción te redirigirá al archivo de origen asociado del repositorio.

Al comenzar a editar, GitHub bifurca automáticamente el repositorio oficial en tu cuenta personal de GitHub, donde puedes realizar los cambios. Cuando hayas terminado, envía una solicitud de incorporación de cambios a la rama **docs**.

### <a name="pull-requests"></a>Solicitudes de incorporación de cambios

Después de enviar la solicitud de incorporación de cambios, se evalúa con una lista de comprobación de calidad del contenido para garantizar que cumple nuestros estándares básicos. Si se aprueba, se asigna a un miembro del equipo de documentación de UWP para su revisión. Si no pasa la evaluación, se te indicarán los cambios que debes hacer.

Los revisores asignados pueden aprobar o rechazar la solicitud o trabajar contigo para realizar más cambios.

## <a name="internal-contributions"></a>Contribuciones internas

Si eres empleado de Microsoft, puedes contribuir mediante el [repositorio de contenido privado](https://github.com/microsoftdocs/windows-uwp-pr). Puedes encontrar instrucciones sobre cómo usar este repositorio en la [Guía de creación de Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/?branch=master). Para contribuir a la documentación de próximas características, solo debe usarse el repositorio privado.

### <a name="editing-a-file"></a>Edición de un archivo

Al igual que en el repositorio público, puedes realizar pequeños cambios en el repositorio privado en el explorador, sin necesidad de crear un clon local. **Debes** asegurarte de que tus contribuciones están en la rama correcta. Para más información sobre la creación de una rama personal, consulta las [instrucciones de la Guía de creación de Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/branches?branch=master).

### <a name="making-substantial-changes"></a>Realización de cambios importantes

Para realizar cambios más profundos en un artículo existente, agregar o cambiar imágenes o contribuir a un artículo nuevo, crea un clon local del repositorio de contenido privado. Para más información, sigue las [instrucciones de la Guía de creación de Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/).

### <a name="pull-requests"></a>Solicitudes de incorporación de cambios

Al crear una solicitud de incorporación de cambios en el repositorio interno, asegúrate de combinar la rama personal con la rama desde la que se creó.

Después de enviar la solicitud de incorporación de cambios, se evalúa con la aplicación [PR Merger](https://review.docs.microsoft.com/help/contribute/prmerger-overview?branch=master) para asegurarse de que cumple con nuestros estándares básicos. Si se aprueba, puedes comentar `#sign-off` para pasarla al miembro del equipo de documentación de UWP para su revisión. Si no se aprueba, se te indicarán los cambios se debes realizar antes de poder aprobarla.

Los revisores asignados pueden aprobar o rechazar la solicitud o trabajar contigo para realizar más cambios. Los revisores no combinarán la solicitud de incorporación de cambios hasta que la hayas aprobado tú mismo.

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>Uso de problemas para proporcionar comentarios sobre la documentación conceptual de UWP

Si quieres proporcionar comentarios sobre los documentos en lugar de realizar modificaciones por tu cuenta, puedes [crear un problema en el repositorio público](https://github.com/MicrosoftDocs/windows-uwp/issues). Selecciona la pestaña **Issues** (Problemas) y haz clic en el botón **New issue** (Nuevo problema). Asegúrate de incluir el título del tema y la dirección URL de la página. El problema se asignará a los miembros del equipo de documentación de UWP para que lo revisen.

* Para problemas internos, usa la [herramienta de solicitud de contenido de WDG](http://sesuw2-iis02a/WSCPubRequest/WindowsContentRequestTool.aspx).
