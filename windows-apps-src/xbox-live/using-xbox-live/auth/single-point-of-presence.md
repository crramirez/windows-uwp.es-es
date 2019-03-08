---
title: Único punto de presencia (SPOP)
description: Obtenga información sobre cómo usar Xbox Live único punto de presencia (SPOP) para asegurarse de que un título se reproduce solo en un único dispositivo a la vez.
ms.assetid: 40f19319-9ccc-4d35-85eb-4749c2cf5ef8
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, spop, único punto de presencia
ms.localizationpriority: medium
ms.openlocfilehash: f1503a6168a50175e861e17027e8b642d1b7834d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595010"
---
# <a name="single-point-of-presence-spop"></a>Único punto de presencia (SPOP)

## <a name="overview"></a>Introducción
Único punto de presencia (SPOP), es una condición de Xbox Live exige que un título sólo se puede reproducir en un dispositivo a la vez. Esto es obligatorio para XDK una Xbox y títulos UWP en cualquier dispositivo.
Un título de Xbox Live, independientemente del dispositivo que está activado, puede iniciar un usuario que ha iniciado sesión en un título en otro dispositivo Xbox One o Windows 10.

## <a name="how-to-handle-spop"></a>Cómo controlar SPOP
SPOP debe controlarse el título de la misma manera que cualquier otro tipo de evento de cierre de sesión. Siempre se notificará al usuario a través de la interfaz de usuario cuando lo hacen una acción que se iniciaría una SPOP para comprobar que les gustaría hacer que el título se desconecten en otro dispositivo.

* Para los títulos de XDK el `User::SignOutCompleted` evento se desencadenará cuando esto ocurre.
* Para los títulos UWP, se notificará el cierre de sesión a través de la `sign_out_complete` controlador desde la `xbox_live_user` clase. Consulte [autenticación para los proyectos de UWP](authentication-for-UWP-projects.md) para obtener más detalles
