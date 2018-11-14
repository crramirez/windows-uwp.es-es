---
title: Implementar perfiles de escáner de códigos de barras con MDM
author: PatrickFarley
description: Es posible implementar perfiles de escáner de códigos de barras con un servidor MDM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.author: pafarley
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: cfd9692620273952483ec7da65a69b643cb5bf4f
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6657407"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implementar perfiles de escáner de códigos de barras con MDM

**Nota**esta característica requiere Windows 10 Mobile o una versión posterior.

Es posible implementar perfiles de escáner de códigos de barras con un servidor MDM. Para implementar los perfiles, usa *OemProfile* en [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) para colocarlos en la carpeta \\Data\\SharedData\\OEM\\Public\\Profile. Luego los fabricantes de controladores pueden usar estos perfiles de escáner para configurar las opciones que no se exponen a través de la superficie de API.

Microsoft no define los detalles de un perfil de escáner o cómo implementarlos.

## <a name="related-topics"></a>Temas relacionados
- [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [Soporte técnico de dispositivo de escáner de códigos de barras](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)