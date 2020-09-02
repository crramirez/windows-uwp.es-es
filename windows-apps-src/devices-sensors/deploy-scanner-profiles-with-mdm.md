---
title: Implementación de perfiles de escáner de código de barras con MDM
description: Obtenga información sobre cómo implementar perfiles de escáner de código de barras con un servidor de administración de dispositivos móviles (MDM) mediante el proveedor de servicios de configuración de EnterpriseExtFileSystem (CSP).
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de3a43386e37c9bb997340c35c8f16977871916d
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304727"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implementación de perfiles de escáner de código de barras con MDM

**Nota:**    Esta característica requiere Windows 10 Mobile o posterior.

Los perfiles de escáner de código de barras se pueden implementar con un servidor MDM. Para implementar los perfiles, use *OemProfile* en el [CSP EnterpriseExtFileSystem](/windows/client-management/mdm/enterpriseextfilessystem-csp) para colocarlos en la \\ \\ carpeta Data SharedData \\ OEM \\ público \\ Profile. Los fabricantes de controladores pueden usar estos perfiles de analizador para configurar las opciones que no se exponen a través de la superficie de la API.

Microsoft no define los detalles de un perfil de analizador o cómo implementarlos.

## <a name="related-topics"></a>Temas relacionados
- [EnterpriseExtFileSystem CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [Compatibilidad con dispositivos de escáner de código de barras](./pos-device-support.md#barcode-scanner)