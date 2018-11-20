---
title: Cómo funciona la emulación x86 y ARM32 en ARM
author: msatranjr
description: Una descripción general de emulación de aplicaciones x86 en ARM.
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, siempre conectado, emulación x86 en ARM
ms.localizationpriority: medium
ms.openlocfilehash: 6b596ab9abd31fa10d0ca07dec973082b495262e
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7302257"
---
# <a name="how-x86-emulation-works-on-arm"></a>Cómo funciona la emulación x86 en ARM
La emulación de aplicaciones x86 enriquece el ecosistema de aplicaciones de Win32 disponibles en ARM. Esto proporciona al usuario la experiencia mágica de ejecutar una aplicación x86 win32 existente sin modificaciones en la aplicación. La aplicación no sabe incluso que se está ejecutando en Windows en un PC ARM, a menos que llame a API específicas ([IsWoW64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318.aspx)).

El nivel [WOW64](https://msdn.microsoft.com/en-us/library/windows/desktop/aa384249(v=vs.85).aspx) de Windows 10 permite ejecutar el código x86 en la versión ARM64 de Windows 10. La emulación x86 funciona compilando bloques de instrucciones x86 en instrucciones ARM64 con optimizaciones para mejorar el rendimiento. Un servicio almacena en caché estos bloques de código convertidos para reducir la sobrecarga de conversión de instrucciones y permitir la optimización cuando el código se ejecute de nuevo. Las memorias caché se producen para cada módulo para que otras aplicaciones puedan usarlas en el primer inicio. 

Para obtener más información acerca de estas tecnologías, consulta el vídeo de Channel9 [Windows 10 en ARM](https://channel9.msdn.com/Events/Build/2017/P4171). 