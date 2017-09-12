---
title: "Implementar perfiles de escáner de códigos de barras con MDM"
author: PatrickFarley
description: "Es posible implementar perfiles de escáner de códigos de barras con un servidor MDM."
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.openlocfilehash: a63a09e64b6e2b935963a3f49ed7cbc6b82bdcef
ms.sourcegitcommit: d2ec178103f49b198da2ee486f1681e38dcc8e7b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2017
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>Implementar perfiles de escáner de códigos de barras con MDM

**Nota**  Esta característica requiere Windows 10 Mobile o una versión posterior.

Es posible implementar perfiles de escáner de códigos de barras con un servidor MDM. Para implementar los perfiles, usa *OemProfile* en [EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025) para colocarlos en la carpeta \\Data\\SharedData\\OEM\\Public\\Profile. Luego los fabricantes de controladores pueden usar estos perfiles de escáner para configurar las opciones que no se exponen a través de la superficie de API.

Microsoft no define los detalles de un perfil de escáner o cómo implementarlos.

## <a name="related-topics"></a>Temas relacionados
[Escáner de códigos de barras](barcode-scanner.md)

[EnterpriseExtFileSystem CSP](https://msdn.microsoft.com/library/windows/hardware/mt157025)