---
title: Implementar perfiles de escáner de códigos de barras con MDM
description: Es posible implementar perfiles de escáner de códigos de barras con un servidor MDM.
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dbcaa683e2c7a2bb18d88fcba03e10fa951d4459
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626160"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implementar perfiles de escáner de códigos de barras con MDM

**Tenga en cuenta**  esta característica requiere Windows 10 Mobile o versiones posteriores.

Es posible implementar perfiles de escáner de códigos de barras con un servidor MDM. Para implementar los perfiles, use *OemProfile* en el [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) colocarlos en el \\datos\\SharedData\\OEM\\ Pública\\carpeta de perfil. Luego los fabricantes de controladores pueden usar estos perfiles de escáner para configurar las opciones que no se exponen a través de la superficie de API.

Microsoft no define los detalles de un perfil de escáner o cómo implementarlos.

## <a name="related-topics"></a>Temas relacionados
- [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)
- [Compatibilidad con dispositivos de escáner de código de barras](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-device-support#barcode-scanner)