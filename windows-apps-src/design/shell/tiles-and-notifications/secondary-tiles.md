---
description: Los mosaicos secundarios permiten a los usuarios anclar contenido específico y vínculos profundos de la aplicación en el menú Inicio, lo que facilita el acceso futuro al contenido dentro de la aplicación.
title: Iconos secundarios
label: Secondary tiles
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, UWP, iconos secundarios
ms.localizationpriority: medium
ms.openlocfilehash: 066a6dcb3683e2e55f7452b1f09bb834157aee62
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034508"
---
# <a name="secondary-tiles"></a>Iconos secundarios


Los mosaicos secundarios permiten a los usuarios anclar contenido específico y vínculos profundos de la aplicación en el menú Inicio, lo que facilita el acceso futuro al contenido dentro de la aplicación.

![Captura de pantalla de iconos secundarios](images/secondarytiles.png)

Por ejemplo, los usuarios pueden anclar el tiempo de varias ubicaciones específicas en el menú Inicio, que proporciona (1) información sencilla de vista rápida sobre el tiempo actual gracias a los mosaicos dinámicos y (2) un punto de entrada rápido al tiempo de la ciudad específica que les interesa. Los usuarios también pueden anclar acciones específicas, artículos de noticias y más elementos que son importantes para ellos.

Al agregar iconos secundarios a la aplicación, ayuda al usuario a volver a interactuar de forma rápida y eficaz con la aplicación, y anima a que devuelvan más a menudo gracias al sencillo acceso que proporcionan los mosaicos secundarios.

**Solo los usuarios pueden anclar un icono secundario; las aplicaciones no pueden anclar iconos secundarios mediante programación sin la aprobación del usuario** . El usuario debe hacer clic explícitamente en un botón "anclar" dentro de la aplicación, momento en el que se usa la API para solicitar la creación de un icono secundario y, a continuación, el sistema muestra un cuadro de diálogo que pide al usuario que confirme si desea tener el icono anclado.

## <a name="quick-links"></a>Vínculos rápidos

| Artículo | Descripción |
| --- | --- |
| [Guía sobre iconos secundarios](secondary-tiles-guidance.md) | Obtenga información acerca de Cuándo y dónde debe usar iconos secundarios. |
| [Anclar iconos secundarios](secondary-tiles-pinning.md) | Obtenga información sobre cómo anclar un icono secundario. |
| [Anclar desde aplicaciones de escritorio](secondary-tiles-desktop-pinning.md) | Las aplicaciones de escritorio pueden anclar iconos secundarios gracias al puente de escritorio. |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>Mosaicos secundarios con respecto a los iconos principales

Los mosaicos secundarios están asociados a una sola aplicación primaria. Están anclados al menú Inicio para proporcionar a un usuario una manera coherente y eficaz de iniciar directamente en un área de uso frecuente de la aplicación primaria. Puede tratarse de una subsección general de la aplicación primaria que contiene contenido actualizado con frecuencia, o bien un vínculo profundo a un área específica de la aplicación.

Algunos ejemplos de escenarios de mosaicos secundarios son:

* Actualizaciones meteorológicas para una ciudad específica en una aplicación meteorológica
* Un resumen de los próximos eventos en una aplicación de calendario
* Estado y actualizaciones de un contacto importante en una aplicación social
* Fuentes específicas en un lector RSS
* Una lista de reproducción de música
* Un blog

Cualquier contenido que cambie con frecuencia que un usuario desee supervisar es un buen candidato para un icono secundario. Después de anclar el icono secundario, los usuarios pueden recibir actualizaciones de un vistazo a través del icono y usarlo para iniciar directamente en la aplicación primaria.

Los mosaicos secundarios son similares a los iconos principales de muchas maneras:

* Usan notificaciones de icono para mostrar contenido enriquecido.
* Deben incluir un logotipo de 150 x 150 píxeles para el contenido del mosaico predeterminado.
* Opcionalmente, pueden incluir los otros tamaños de logotipo para habilitar tamaños de mosaico más grandes.
* Pueden mostrar notificaciones y distintivos.
* Se pueden reorganizar en el menú Inicio.
* Se eliminan automáticamente cuando se desinstala la aplicación.
* Su distintivo y el texto de estado detallado del bloqueo se pueden mostrar en el bloqueo.

Sin embargo, los mosaicos secundarios difieren de los iconos principales en algunos aspectos perceptibles:

* Los usuarios pueden eliminar sus iconos secundarios en cualquier momento sin eliminar la aplicación primaria.
* Los mosaicos secundarios se pueden crear en tiempo de ejecución. Los iconos de la aplicación solo se pueden crear durante la instalación.
* Un control flotante solicita confirmación al usuario antes de agregar un icono secundario.
* No se pueden seleccionar mediante programación para la pantalla de bloqueo a través de una solicitud al usuario. El usuario debe agregar manualmente el icono secundario a través de la página de personalización en configuración de PC.

Para el envío de notificaciones, se proporcionan métodos específicos para los actualizadores de iconos y de notificaciones y los canales de notificaciones de entrega que se usan con iconos secundarios. Estas versiones son paralelas a las que se usan con los iconos principales. Por ejemplo, CreateBadgeUpdaterForApplication frente a CreateBadgeUpdaterForSecondaryTile.


## <a name="guidance-on-secondary-tiles"></a>Guía sobre iconos secundarios
Para obtener información acerca de Cuándo y dónde se deben usar los iconos secundarios y otras instrucciones de uso, consulte la [Guía sobre los iconos secundarios](secondary-tiles-guidance.md) .


## <a name="pinning-secondary-tiles"></a>Anclar iconos secundarios
Para obtener información sobre cómo anclar iconos secundarios, consulte [iconos secundarios de PIN](secondary-tiles-pinning.md).


## <a name="desktop-applications-and-secondary-tiles"></a>Aplicaciones de escritorio y mosaicos secundarios
Para obtener información sobre cómo usar iconos secundarios desde la aplicación de escritorio a través del puente de escritorio, consulte [anclar iconos secundarios de aplicaciones de escritorio](secondary-tiles-desktop-pinning.md).
