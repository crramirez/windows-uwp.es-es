---
author: Xansky
Description: Lists the practices to avoid if you want to create an accessible Universal Windows Platform (UWP) app.
ms.assetid: 024A9B70-9821-45BB-93F1-61C0B2ECF53E
title: Procedimientos de accesibilidad que deben evitarse
label: Accessibility practices to avoid
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9505dfc666042c22e6f77ed02ffca7c5973d4fba
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5594802"
---
# <a name="accessibility-practices-to-avoid"></a>Procedimientos de accesibilidad que deben evitarse

Si quieres crear una aplicación accesible para Plataforma universal de Windows (UWP), consulta la siguiente lista de procedimientos que deben evitarse: 

* **Evita compilar elementos de la interfaz de usuario personalizados si puedes usar los controles predeterminados de Windows** o los controles que ya hayan implementado la compatibilidad con la automatización de la interfaz de usuario de Microsoft. Se puede obtener acceso de forma predeterminada a los controles estándar de Windows y, por lo general, solo es necesario agregar unos pocos atributos de accesibilidad específicos de la aplicación. Por el contrario, implementar la compatibilidad de la clase [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) con un control personalizado te resultará un poco más complicado (consulta el tema [Personalizar sistemas de automatización del mismo nivel](custom-automation-peers.md)).
* **No coloques texto estático u otros elementos no interactivos en el orden de tabulación** (por ejemplo, no establezcas la propiedad [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) de un elemento que no sea interactivo). Tener elementos no interactivos en el orden de tabulación no es compatible con las directrices de accesibilidad del teclado, ya que reduce la eficacia de navegación de los usuarios que usan el teclado. Muchas tecnologías de ayuda usan el orden de tabulación y la capacidad de enfocar un elemento como parte de su lógica en cuanto al modo de presentar la interfaz de una aplicación al usuario de la tecnología de asistencia. Los elementos de solo texto en el orden de tabulación pueden confundir a los usuarios, ya que solo esperan elementos interactivos en el orden de tabulación (botones, casillas, campos de entrada de texto, cuadros combinados, listas, etc.).
* **No uses el posicionamiento absoluto de los elementos de la interfaz de usuario** (por ejemplo, en un elemento [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267)), ya que el orden de presentación suele ser distinto al orden de la declaración de elementos secundarios (que es el orden lógico real). Cuando sea posible, organiza los elementos de la interfaz de usuario en un orden lógico o de documento para asegurarte de que los lectores de pantalla puedan leer dichos elementos en el orden correcto. Si el orden visible de los elementos de la interfaz de usuario difiere del orden lógico o de documento, puedes usar valores de índice de tabulación explícitos (puedes establecer [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461)) para definir el orden de lectura correcto.
* **No uses los colores como el único modo de transmitir información.** Los usuarios que sufren de daltonismo no pueden recibir información que solo se transmite mediante el color, como un indicador de estado de color. Incluye otras indicaciones visuales, preferentemente texto, para asegurarte de que pueda obtenerse acceso a la información.
* **No actualices automáticamente todo el lienzo de la aplicación**, a menos que sea realmente necesario para su funcionalidad. Si necesitas actualizar automáticamente el contenido de la página, actualiza solo ciertas áreas. Las tecnologías de asistencia deben tener en cuenta que el lienzo actualizado de una aplicación es una estructura totalmente nueva, incluso si los cambios que se realizaron en el mismo son mínimos. Esto afecta al usuario de la tecnología de asistencia, ya que se debe volver a crear y presentar al usuario cualquier descripción o vista de documento de la aplicación actualizada.
  
  Una navegación de páginas intencional iniciada por el usuario es un caso legítimo para actualizar la estructura de la aplicación. Pero asegúrate de que el elemento de la interfaz de usuario que inicia la navegación se denomine o esté identificado correctamente para dar alguna indicación de que su invocación generará un cambio de contexto y la recarga de la página.

  > [!NOTE]
  > Si actualizas el contenido dentro de una región, te recomendamos que establezcas la propiedad de accesibilidad [**AccessibilityProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516) de ese elemento en uno de los parámetros **Polite** o **Assertive** no predeterminados. Algunas tecnologías de asistencia pueden asignar este parámetro al concepto Aplicaciones de Internet enriquecidas y accesibles (ARIA) de las regiones activas y, de esta forma, pueden informar al usuario de que una región de contenido ha cambiado.

* **No uses elementos de la interfaz de usuario que parpadeen más de tres veces por segundo.** Este tipo de elementos puede ocasionar convulsiones en algunas personas. Es mejor evitarlos.
* **No cambies el contexto de usuario ni actives una funcionalidad de forma automática.** Los cambios de activación y el contexto solamente deben producirse cuando el usuario realiza una acción directa en un elemento de la interfaz de usuario que tiene enfoque. Los cambios en el contexto del usuario incluyen cambiar el enfoque, mostrar el contenido nuevo y navegar a una página diferente. La realización de cambios al contexto sin que el usuario participe puede desorientar a aquellos usuarios que tienen discapacidades. Las excepciones a este requisitos son, entre otras, mostrar submenús, validar formularios, mostrar texto de ayuda en otro control y cambiar el contexto en respuesta a un evento asincrónico.

<span id="related_topics"/>

## <a name="related-topics"></a>Temas relacionados  
* [Accesibilidad](accessibility.md)
* [Accesibilidad en la Tienda](accessibility-in-the-store.md)
* [Lista de comprobación de accesibilidad](accessibility-checklist.md)
