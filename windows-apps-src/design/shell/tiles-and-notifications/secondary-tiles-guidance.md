---
Description: Obtenga información acerca de Cuándo y dónde debe usar iconos secundarios en la aplicación de Windows.
title: Guía de diseño de mosaicos secundarios
label: Secondary tiles
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, UWP, iconos secundarios, instrucciones, instrucciones, procedimientos recomendados
ms.localizationpriority: medium
ms.openlocfilehash: 83f8a095a4e15c3ec0ebb02eebc183cf4beb01ea
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172339"
---
# <a name="secondary-tile-guidance"></a>Guía de mosaicos secundarios


Un icono secundario proporciona una forma coherente y eficaz de que los usuarios accedan directamente a áreas específicas de una aplicación desde el menú Inicio. Aunque un usuario elige si desea "anclar" un icono secundario en el menú Inicio, el desarrollador determina las áreas ancladas de una aplicación. Para obtener un resumen más detallado, consulte [información general sobre iconos secundarios](secondary-tiles.md). Tenga en cuenta estas directrices al habilitar los iconos secundarios y diseñar la interfaz de usuario asociada en la aplicación.

> [!NOTE]
> Solo los usuarios pueden anclar un icono secundario al menú Inicio; las aplicaciones no pueden anclar iconos secundarios mediante programación. Los usuarios también controlan la eliminación de los mosaicos y pueden quitar un icono secundario del menú Inicio o de la aplicación primaria.


## <a name="recommendations"></a>Recomendaciones

Tenga en cuenta las siguientes recomendaciones al habilitar iconos secundarios en la aplicación:

* Cuando el contenido del foco es anclado, la barra de la aplicación debe contener el botón "anclar a Inicio" para crear un icono secundario para el usuario.
* Cuando el usuario hace clic en "anclar a Inicio", debe llamar inmediatamente a la API desde el subproceso de la interfaz de usuario para [anclar el icono secundario](secondary-tiles-pinning.md).
* Si el contenido del foco ya está anclado, reemplace el botón "anclar a Inicio" de la barra de la aplicación por un botón "desanclar desde Inicio". El botón "desanclar de inicio" debería quitar el icono secundario existente.
* Cuando el contenido del foco no sea anclado, no muestre un botón "anclar a Inicio" (o muestre un botón "anclar al inicio" deshabilitado).
* Use los glifos proporcionados por el sistema para los botones "anclar a Inicio" y "desanclar desde Inicio" (vea el PIN y desanclar miembros en Windows. UI. Xaml. Controls. Symbol o WinJS. UI. AppBarIcon).
* Use el texto del botón estándar: "anclar a Inicio" y "desanclar desde Inicio". Tendrá que reemplazar el texto predeterminado al usar el PIN proporcionado por el sistema y desanclar los glifos.
* No use un icono secundario como un botón de comando virtual para interactuar con la aplicación primaria, como un icono "ir al siguiente seguimiento".


## <a name="additional-usage-guidance-for-devs"></a>Instrucciones de uso adicionales para desarrolladores

* Cuando se inicia una aplicación, siempre debe enumerar sus iconos secundarios, en caso de que se hayan agregado o eliminado de los que no era consciente. Cuando se elimina un icono secundario a través de la barra de la aplicación de pantalla de inicio, Windows simplemente quita el icono. La propia aplicación es responsable de liberar todos los recursos usados por el icono secundario. Cuando los mosaicos secundarios se copian a través de la nube, las notificaciones de icono o de distintivo actual en el icono secundario, las notificaciones programadas, los canales de notificaciones de envío y los identificadores uniformes de recursos (URI) que se usan con las notificaciones periódicas no se copian con el icono secundario y deben volver a configurarse.
* Una aplicación debe usar identificadores únicos significativos y recreables para los iconos secundarios. El uso de identificadores de iconos secundarios predecibles que son significativos para una aplicación ayuda a la aplicación a comprender qué hacer con estos mosaicos cuando se ven en una instalación nueva en un equipo nuevo.
  * En tiempo de ejecución, la aplicación puede consultar si existe un icono específico.
  * Se puede solicitar a la plataforma de mosaicos secundarios que devuelva el conjunto de todos los iconos secundarios que pertenecen a una aplicación específica. El uso de identificadores únicos significativos para estos iconos puede ayudar a la aplicación a examinar el conjunto de iconos secundarios y realizar las acciones adecuadas. Por ejemplo, en el caso de una aplicación de redes sociales, los identificadores pueden identificar contactos individuales para los que se crearon iconos.
* Los mosaicos secundarios, como todos los mosaicos de la pantalla Inicio, son salidas dinámicas que se pueden actualizar con frecuencia con contenido nuevo. Los iconos secundarios pueden exponer notificaciones y actualizaciones mediante los mismos mecanismos que cualquier otro icono. Consulte [elegir un método de entrega de notificaciones](choosing-a-notification-delivery-method.md) para obtener más información.


## <a name="related"></a>Temas relacionados

* [Información general sobre mosaicos secundarios](secondary-tiles.md)
* [Anclar iconos secundarios](secondary-tiles-pinning.md)
* [Activos en mosaico](../../style/app-icons-and-logos.md)
* [Documentación de contenido de mosaico](create-adaptive-tiles.md)
* [Enviar una notificación de icono local](sending-a-local-tile-notification.md)