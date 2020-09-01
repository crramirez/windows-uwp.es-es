---
title: Cómo funciona la emulación de x86 y ARM32 en ARM
description: Obtenga información sobre cómo la emulación de aplicaciones x86 hace que el amplio ecosistema de las aplicaciones Win32 existentes esté disponible en los dispositivos ARM.
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, siempre conectado, emulación x86 en ARM
ms.localizationpriority: medium
ms.openlocfilehash: 61d994a4a022da671b4141ded80c6235ae38bae6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162329"
---
# <a name="how-x86-emulation-works-on-arm"></a>Cómo funciona la emulación de x86 en ARM
La emulación de aplicaciones x86 hace que el amplio ecosistema de aplicaciones Win32 esté disponible en ARM. Esto proporciona al usuario la experiencia mágica de ejecutar una aplicación Win32 de x86 existente sin modificaciones en la aplicación. La aplicación no sabe incluso que se está ejecutando en un equipo con Windows en ARM, a menos que llame a determinadas API ([IsWoW64Process2](/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2)).

El nivel [WOW64](/windows/desktop/WinProg64/running-32-bit-applications) de Windows 10 permite que el código x86 se ejecute en la versión ARM64 de Windows 10. la emulación de x86 funciona mediante la compilación de bloques de instrucciones x86 en instrucciones ARM64 con optimizaciones para mejorar el rendimiento. Un servicio almacena en caché estos bloques de código convertidos para reducir la sobrecarga de la traducción de instrucciones y permitir la optimización cuando el código vuelve a ejecutarse. Las memorias caché se producen para cada módulo, de modo que otras aplicaciones puedan usarlas en el primer inicio. 

Para más información sobre estas tecnologías, consulte el vídeo de [Windows 10 en ARM](https://channel9.msdn.com/Events/Build/2017/P4171) Channel9.