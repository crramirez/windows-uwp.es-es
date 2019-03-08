---
title: Herramientas de desarrollo de Xbox Live
description: Obtenga información sobre herramientas que se proporcionan ayudar a desarrollar y probar su Xbox Live habilitado título.
ms.assetid: 380a29bf-41a7-4817-9c57-f48f2b824b52
ms.date: 6/13/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, las herramientas de restablecimiento del Reproductor, live analizador de seguimiento, LTA, herramienta de la cuenta de Windows live de xbox,
ms.localizationpriority: medium
ms.openlocfilehash: ad43ecbd3bfd266d4a237253380bca223302fe54
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623520"
---
# <a name="development-tools-for-xbox-live"></a>Herramientas de desarrollo de Xbox Live

Esta sección describen diversas herramientas que puede usar para facilitarle el desarrollo de Xbox Live. Muchas de las herramientas están disponibles en el [Xbox Live Developer Tools GitHub](https://github.com/Microsoft/xbox-live-developer-tools) repositorio. También puede usar el [biblioteca de herramientas de desarrollo](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools) para crear sus propias herramientas personalizadas. Todas las herramientas de desarrollador independiente pueden descargarse en [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools).

> [!NOTE]
> Solo se pueden usar las herramientas de MatchSim y XboxLiveCompute incluidas en la descarga por socios administrados o asociados inscritos en el [ ID@Xbox ](https://www.xbox.com/Developers/id) programa. Para más información acerca de los programas para desarrolladores disponibles, consulte el [visión general del programa para desarrolladores](https://docs.microsoft.com/windows/uwp/xbox-live/developer-program-overview). 

## <a name="global-storage"></a>Almacenamiento global
Almacenamiento de título global se usa para almacenar los datos que puede leer todo el mundo, como listas, asignaciones, desafíos o recursos de arte. Es un tipo de [título almacenamiento](../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md). La herramienta de almacenamiento Global se utiliza para administrar el almacenamiento de título global en espacios aislados de prueba. Todavía se deben publicar datos en venta directa a través del centro de partners o Xbox Developer Portal (XDP). La herramienta está disponible a través de línea de comandos como parte de la [herramientas de desarrollo](https://aka.ms/xboxliveuwptools) zip. Se pueden crear herramientas personalizadas con el [biblioteca de herramientas de desarrollo](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools).

## <a name="multiplayer-session-history-viewer"></a>Visor del historial de sesión de varios jugadores
Visor del historial de sesión participan varios jugadores le ofrece la capacidad de ver una escala de tiempo histórico de todos los cambios a través del historial de un documento de la sesión de varios jugadores (incluidos los documentos eliminados). Con esta herramienta le proporcionará una comprensión más profunda de lo que sucede con los documentos de la sesión MPSD medida que cambian con el tiempo. Está disponible como una herramienta independiente en el [herramientas de desarrollo](https://aka.ms/xboxliveuwptools) zip.

## <a name="player-data-reset"></a>Restablecimiento de los datos Reproductor
La herramienta de restauración de datos de Reproductor se puede usar para restablecer los datos de un jugador en espacios aislados de prueba. Puede restablecer los datos como; logros, tablas de líderes, estadísticas y el historial de título. La herramienta está disponible a través de línea de comandos como parte de la [herramientas de desarrollo](https://aka.ms/xboxliveuwptools) zip. Se pueden crear herramientas personalizadas con el [biblioteca de herramientas de desarrollo](https://www.nuget.org/packages/Microsoft.Xbox.Services.DevTools).

## <a name="xbox-live-developer-account"></a>Cuenta de Xbox Live Developer
La herramienta de la cuenta de Xbox Live Developer se usa para administrar la autenticación de una cuenta de desarrollador. Se necesita para interactuar con otras herramientas de desarrollo que requieren una credencial para desarrolladores, como restablecer el Reproductor y almacenamiento Global. La herramienta está disponible a través de línea de comandos como parte de la [herramientas de desarrollo](https://aka.ms/xboxliveuwptools) zip.

## <a name="xbox-live-trace-analyzer"></a>Analizador de seguimiento activo de Xbox
Uso de [analizador de seguimiento de Xbox Live](analyze-service-calls.md), puede capturar todas las llamadas de servicio y, a continuación, analizarlos sin conexión para las infracciones en la llamada a patrones. El seguimiento de llamadas de servicio puede activarse mediante la herramienta de línea de comandos xbtrace, o a través de la activación de protocolos para obtener más información de los escenarios avanzado. También se admite la activación de la llamada al servicio de seguimiento directamente desde el código de título. La herramienta está disponible a través de línea de comandos como parte de la [herramientas de desarrollo](https://aka.ms/xboxliveuwptools) zip.

## <a name="xbox-live-account-tool"></a>Herramienta de la cuenta de Windows Live de Xbox  
El [herramienta de cuenta de Xbox Live](xbox-live-account-tool.md) está diseñado para ayudarle a configurar las cuentas de prueba existente para probar escenarios de juego. Por ejemplo, puede usar la herramienta de cuenta de Xbox Live para cambiar el nombre de jugador de la cuenta, o agregar rápidamente los seguidores de 1000 a la lista de amigos de la cuenta. La herramienta está disponible a través de línea de comandos como parte de la [herramientas de desarrollo](https://aka.ms/xboxliveuwptools) zip.

## <a name="config-as-source"></a>Configuración como origen
[Configuración como origen](https://github.com/Microsoft/xbox-live-developer-tools/blob/master/CONFIGASSOURCE.md) es un conjunto de herramientas que Microsoft ha desarrollado para dar cabida a los usuarios avanzados, proporcionando herramientas oficialmente compatibles y API para integrar en nuestros servicios de configuración. Normalmente, estos servicios de Xbox Live se configuran para el título en el centro de partners, incluidos los servicios que van de marcadores, logros, para los servicios web y de confianza. Para muchos desarrolladores de juegos, usando el centro de partners es suficiente. Sin embargo, para los usuarios avanzados, hay un deseo de integrar tareas comunes de configuración en sus propios procesos y herramientas.  Configuración como código fuente está diseñado para admitir estos escenarios, ya que proporciona herramientas de línea de comandos y las API nuevas para admitir la integración personalizada sobre los flujos de trabajo existentes y las canalizaciones. La herramienta está disponible a través de línea de comandos como parte de la [herramientas de desarrollo](https://aka.ms/xboxliveuwptools) zip.
