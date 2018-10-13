---
author: andrewleader
Description: Secondary tiles allow users to pin specific content and deep links from your app onto their Start menu, providing easy future access to the content within your app.
title: Iconos secundarios
label: Secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, iconos secundarios
ms.localizationpriority: medium
ms.openlocfilehash: 7f11ca4d29f22daf953ce03436c3b786c70a9e04
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "4571666"
---
# <a name="secondary-tiles"></a>Iconos secundarios


Los iconos secundarios permiten a los usuarios anclar determinado contenido y vínculos profundos de la aplicación en su menú Inicio, lo que proporciona un fácil acceso en el futuro al contenido de la aplicación.

![Captura de pantalla de iconos secundarios](images/secondarytiles.png)

Por ejemplo, los usuarios pueden anclar la información meteorológica de numerosas ubicaciones específicas en su menú Inicio, lo que les proporciona (1) una información fácilmente visible y en tiempo real sobre el tiempo actual, gracias a Iconos dinámicos y (2) un punto de entrada rápida al tiempo de la ciudad específica que les interesa. Los usuarios también pueden anclar cotizaciones específicas, artículos de noticias y más elementos que son importantes para ellos.

Al agregar iconos secundarios a la aplicación, estás ayudando al usuario a interactuar más rápida y eficazmente con la aplicación, animándoles a volver más a menudo gracias a la facilidad de acceso que proporcionan los iconos secundarios.

**Solo los usuarios pueden anclar un icono secundario; las aplicaciones no pueden anclar iconos secundarios conforme a programación sin la aprobación por parte del usuario**. El usuario debe hacer clic explícitamente en un botón "Anclar" dentro de la aplicación, momento en que usas la API para crear un icono secundario y, a continuación, el sistema muestra un cuadro de diálogo que pide al usuario que confirme si desea anclar el icono.

## <a name="quick-links"></a>Vínculos rápidos

| Artículo | Descripción |
| --- | --- |
| [Instrucciones sobre los iconos secundarios](secondary-tiles-guidance.md) | Obtén información sobre cuándo y dónde debes usar los iconos secundarios. |
| [Anclar iconos secundarios](secondary-tiles-pinning.md) | Obtén información sobre cómo anclar un icono secundario. |
| [Anclar desde una aplicación de escritorio](secondary-tiles-desktop-pinning.md) | Las aplicaciones de escritorio de Windows pueden anclar iconos secundarios gracias al Puente de dispositivo de escritorio. |


## <a name="secondary-tiles-in-relation-to-primary-tiles"></a>Iconos secundarios en relación con los iconos principales

Los iconos secundarios están asociados a una sola aplicación principal. Están anclados al menú Inicio para ofrecer al usuario una manera coherente y eficaz de iniciar la aplicación directamente en un área de uso frecuente de la aplicación principal. Esta área puede ser una subsección general de la aplicación principal que incluya contenido frecuentemente actualizado o un vínculo profundo a un área específica de la aplicación.

Entre los ejemplos de escenarios de iconos secundarios se encuentran los siguientes:

* Actualizaciones del pronóstico del tiempo para una ciudad específica en una aplicación de información meteorológica
* Un resumen de eventos próximos en una aplicación de calendario
* Estados y actualizaciones de un contacto importante en una aplicación social
* Fuentes específicas en un lector RSS
* Una lista de reproducción de música
* Un blog

Cualquier contenido de modificación frecuente que un usuario quiera supervisar es un buen candidato para un icono secundario. Después de anclado el icono secundario, los usuarios pueden recibir actualizaciones de un vistazo a través del icono y usarlo para iniciar directamente en la aplicación principal.

Los iconos secundarios son similares a los iconos primarios en muchos aspectos:

* Usan notificaciones de icono para mostrar contenido enriquecido.
* Deben incluir un logotipo de 150 x 150 píxeles para el contenido predeterminado del icono.
* Opcionalmente, pueden incluir los otros tamaños de logotipo para permitir mayores tamaños de icono.
* Pueden mostrar notificaciones y distintivos.
* Pueden reordenarse en el menú Inicio.
* Se eliminan de forma automática al desinstalar la aplicación.
* Su distintivo y texto de estado detallado de bloqueo se puede mostrar en estado de bloqueo.

Sin embargo, los iconos secundarios difieren de los iconos principales en algunos aspectos a señalar:

* Los usuarios pueden eliminar los iconos secundarios en cualquier momento, sin necesidad de eliminar la aplicación principal.
* Los iconos secundarios se pueden crear en tiempo de ejecución. Los iconos de la aplicación se pueden crear solo durante la instalación.
* Un control flotante solicita al usuario confirmación antes de agregar un icono secundario.
* No pueden seleccionarse mediante programación para la pantalla de bloqueo a través de una solicitud al usuario. El usuario debe agregar manualmente el icono secundario mediante la página Personalizar, en Configuración de PC.

Para enviar notificaciones, se proporcionan métodos específicos para los actualizadores de iconos y notificaciones y los canales de notificaciones de inserción que se usan con los iconos secundarios. Las versiones van paralelas a las usadas con los iconos primarios. Por ejemplo, CreateBadgeUpdaterForApplication frente a CreateBadgeUpdaterForSecondaryTile.


## <a name="guidance-on-secondary-tiles"></a>Instrucciones sobre los iconos secundarios
Para obtener información sobre cuándo y dónde debes usar los iconos secundarios y otras instrucciones de uso, consulta [Instrucciones sobre los iconos secundarios](secondary-tiles-guidance.md)


## <a name="pinning-secondary-tiles"></a>Anclaje de iconos secundarios
Para obtener información sobre cómo anclar iconos secundarios, consulta [Anclar iconos secundarios](secondary-tiles-pinning.md).


## <a name="desktop-applications-and-secondary-tiles"></a>Aplicaciones de escritorio e iconos secundarios
Para obtener información sobre cómo usar los iconos secundarios de la aplicación de escritorio mediante el Puente de dispositivo de escritorio, consulte [Anclar iconos secundarios desde una aplicación de escritorio](secondary-tiles-desktop-pinning.md).
