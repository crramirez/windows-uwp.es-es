---
title: Implementar perfiles de escáner de códigos de barras con MDM
description: Es posible implementar perfiles de escáner de códigos de barras con un servidor MDM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f537833385582678b215804cac9a16002618c7e4
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684823"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implementar perfiles de escáner de códigos de barras con MDM

**Tenga en cuenta**  esta característica requiere Windows 10 Mobile o posterior.

Es posible implementar perfiles de escáner de códigos de barras con un servidor MDM. Para implementar los perfiles, use *OemProfile* en el [CSP EnterpriseExtFileSystem](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp) para colocarlos en la carpeta \\Data\\SharedData\\OEM\\perfil\\público. Luego los fabricantes de controladores pueden usar estos perfiles de escáner para configurar las opciones que no se exponen a través de la superficie de API.

Microsoft no define los detalles de un perfil de escáner o cómo implementarlos.

## <a name="related-topics"></a>Temas relacionados
- [CSP EnterpriseExtFileSystem](https://docs.microsoft.com/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [Compatibilidad con dispositivos de escáner de código de barras](https://docs.microsoft.com/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)