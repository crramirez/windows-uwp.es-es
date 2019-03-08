---
title: Novedades para el SDK de Xbox Live, agosto de 2015
description: Novedades para el SDK de Xbox Live, agosto de 2015
ms.assetid: a034867b-7cc0-4b97-89d5-3486e95a80b4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a454b7339035475416934c2f921dae65283c340c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639820"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2015"></a>Novedades para el SDK de Xbox Live, agosto de 2015

Consulte la [What's New - junio de 2015](1506-whats-new.md) artículo para lo que se agregó en la versión de junio de 2015.

La versión de agosto de Xbox Live SDK incluye las siguientes actualizaciones:

## <a name="os-and-tool-support"></a>Sistema operativo y la herramienta de soporte técnico
El SDK de Xbox Live ahora es compatible con Windows 10 RTM [versión 10.0.10240] y Visual Studio 2015 RTM [versión 14.0.23107.0].

## <a name="multiplayer-manager-winrt-apis"></a>API de WinRT Manager participan varios jugadores
El Administrador de varios jugadores (en el espacio de nombres experimental) ahora admite APIs WinRT (además de APIs C++)

## <a name="submit-batch-feedback-from-a-title"></a>Enviar comentarios de batch en un título
Envía a un número de elementos de comentarios a la vez desde un título.

## <a name="newupdated-documentation"></a>Documentación de nuevo o actualizado
El paquete del SDK de Xbox Live ahora incluye una carpeta "Docs", contiene las referencias de API actualizadas y la nueva "Xbox Live Guía de programación".

Correcciones de errores:

* Se bloquean mientras quitar las suscripciones de servicio de la actividad en tiempo Real
* Bloquear inicio de sesión con una cuenta de invitado
* Infracción de acceso antes de desconectar el cable de red
* Errores de túnel ahora proporcionan un código de error en las APIs C++
* Problema de ETag con TitleStorageService::DownloadBlobAsync
* Varias correcciones de errores para aplicaciones de ejemplo.
