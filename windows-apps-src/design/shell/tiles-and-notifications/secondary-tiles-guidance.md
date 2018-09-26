---
author: andrewleader
Description: Learn about when and where you should use secondary tiles in your UWP app.
title: Iconos secundarios
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, iconos secundarios, instrucciones, directrices, procedimientos recomendados
ms.localizationpriority: medium
ms.openlocfilehash: 1e3d31376b9ac155dab6bffa7739cb880af1cff9
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "4204645"
---
# <a name="secondary-tile-guidance"></a>Instrucciones sobre los iconos secundarios


Un icono secundario es una manera coherente y eficaz que tienen los usuarios para acceder directamente a áreas específicas de una aplicación del menú Inicio. Aunque el usuario elige si quiere "anclar" o no un icono secundario al menú Inicio, es el desarrollador quien determina en qué áreas de una aplicación se puede anclar. Para obtener un resumen más detallado, consulta [Introducción a los iconos secundarios](secondary-tiles.md). Ten en cuenta estas directrices a la hora de habilitar iconos secundarios y diseñar la interfaz de usuario asociada en tu aplicación.

> [!NOTE]
> Solo los usuarios pueden anclar un icono secundario al menú Inicio; las aplicaciones no pueden anclar iconos secundarios mediante programación. Los usuarios también pueden controlar la retirada de iconos y pueden quitar un icono secundario del menú Inicio o de la aplicación principal.


## <a name="recommendations"></a>Recomendaciones

Ten en cuenta las siguientes recomendaciones a la hora de habilitar iconos secundarios en tu aplicación:

* Cuando el contenido que tiene el foco se puede anclar, la barra de la aplicación debe incluir un botón "Anclar a Inicio" para crear un icono secundario para el usuario.
* Cuando el usuario haga clic en "Anclar a Inicio", debes llamar inmediatamente a la API desde el subproceso de interfaz de usuario para [anclar el icono secundario](secondary-tiles-pinning.md).
* Si el contenido que tiene el foco ya está anclado, cambia el botón "Anclar a Inicio" de la barra de la aplicación por el botón "Desanclar de Inicio". El botón "Desanclar de inicio" debería retirar el icono secundario existente.
* Cuando el contenido que tiene el foco no se puede anclar, no muestres un botón "Anclar a Inicio" (o muestra un botón "Anclar a Inicio" deshabilitado).
* Usa los glifos proporcionados por el sistema para tus botones "Anclar a Inicio" y "Desanclar de Inicio" (consulta los miembros de anclaje y desanclaje de en Windows.UI.Xaml.Controls.Symbol o WinJS.UI.AppBarIcon).
* Usa el texto de botones estándar: "Anclar a Inicio" y "Desanclar de Inicio". Tendrás que invalidar el texto predeterminado cuando uses los glifos de anclaje y desanclaje proporcionados por el sistema.
* No uses un icono secundario como un botón de comando virtual para interactuar con la aplicación primaria, por ejemplo, un icono "saltar a la siguiente pista".


## <a name="additional-usage-guidance-for-devs"></a>Instrucciones de uso adicionales para desarrolladores

* Cuando se inicia una aplicación, siempre debe enumerar sus iconos secundarios, por si se han realizado adiciones o eliminaciones de las cuales no se estaba al tanto. Cuando se elimina un icono secundario a través de la barra de la aplicación de la pantalla Inicio, Windows simplemente quita el icono. La propia aplicación es responsable de liberar los recursos usados por el icono secundario. Cuando se copian iconos secundarios a través de la nube, las notificaciones de icono y distintivo actuales en el icono secundario, las notificaciones programadas, los canales de notificación de inserción y los Identificadores uniformes de recursos (URI) usados con las notificaciones periódicas no se copian con el icono secundario y deben volver a configurarse.
* La aplicación debe usar identificadores únicos, significativos y que se puedan volver a crear para iconos secundarios. El uso de identificadores de iconos secundarios predecibles y significativos en una aplicación ayuda a que esta sepa qué debe hacer con estos iconos cuando aparecen en una instalación nueva de un nuevo equipo.
  * En tiempo de ejecución, la aplicación puede consultar si existe un icono específico.
  * Se puede solicitar que la plataforma de iconos secundarios devuelva el conjunto de todos los iconos secundarios que pertenecen a una aplicación específica. El uso de identificadores únicos y significativos para estos iconos puede ayudar a que la aplicación examine el conjunto de iconos secundarios y realice las acciones correspondientes. Por ejemplo, en una aplicación de medios sociales, los identificadores podrían identificar los contactos individuales para los que se crean los iconos.
* Los iconos secundarios, como todos los iconos de la pantalla Inicio, son entradas dinámicas que pueden actualizarse frecuentemente con contenido nuevo. Los iconos secundarios pueden exponer notificaciones y actualizaciones mediante los mismos mecanismos que cualquier otro icono. Para más información, consulta [Elegir un método de entrega de notificaciones](choosing-a-notification-delivery-method.md).


## <a name="related"></a>Relacionados

* [Información general sobre iconos secundarios](secondary-tiles.md)
* [Anclar iconos secundarios](secondary-tiles-pinning.md)
* [Activos de icono](app-assets.md)
* [Documentación del contenido de iconos](create-adaptive-tiles.md)
* [Enviar una notificación de icono local](sending-a-local-tile-notification.md)
