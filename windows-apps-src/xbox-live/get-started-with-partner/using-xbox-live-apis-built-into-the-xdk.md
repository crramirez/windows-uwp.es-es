---
title: Mediante las API de Xbox Live integrado en el XDK
description: Obtenga información sobre cómo usar las API integradas de Xbox Live en el proyecto de Xbox Developer Kit (XDK).
ms.assetid: 539caca3-58bc-49d9-8432-ca8e57755be2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c762dd8abbbc80948d232610e4123b6e4893936d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598410"
---
# <a name="using-xbox-live-apis-built-into-the-xdk"></a>Mediante las API de Xbox Live integrado en el XDK

1. Haga clic con el botón derecho en el proyecto en Visual Studio y elija "Referencias".
1. Elija "Agregar nueva referencia"
1. Haga clic en "Durango. <build number>" y "Extensions" en el panel izquierdo
1. En la parte central, elija:
- Si desea usar la API de WinRT XSAPI, elija "API de servicios de Xbox"
- Si desea usar la API de C++ XSAPI, elija "Cpp de API de servicios de Xbox"
1. Haga clic en Aceptar

Nota: Si el sistema de compilación no es compatible con archivos de propiedades, debe agregar manualmente las definiciones de preprocesador y bibliotecas tal como se muestra en `%XboxOneExtensionSDKLatest%\ExtensionSDKs\Xbox.Services.API.Cpp\8.0\DesignTime\CommonConfiguration\Neutral\Xbox.Services.API.Cpp.props`
