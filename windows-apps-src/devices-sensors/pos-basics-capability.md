---
author: TerryWarwick
title: Capacidad de dispositivo de PointOfService
description: La funcionalidad PointOfService es necesaria para usar el espacio de nombres Windows.Devices.PointOfService.
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: dbed55af1a9fa3df14f0a7e3e7c6d1f599b40fd3
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5864177"
---
# <a name="pointofservice-device-capability"></a>Capacidad de dispositivo de PointOfService
Solicitas acceso a las API de PointOfService declarando la funcionalidad en el manifiesto del paquete de la aplicación]. Para declarar la mayoría de las funcionalidades, usa el Diseñador de manifiestos, en Microsoft Visual Studio, o puedes agregarlas manualmente.  

> [!Important]
> Recibirás el error **System.UnauthorizedAccessException** al intentar usar una API en el espacio de nombres Winodws.Devices.PointOfService si no declaras la funcionalidad **pointOfService** en el manifiesto de la aplicación. 

## <a name="declare-capability-using-manifest-designer"></a>Declarar la funcionalidad con el Diseñador de manifiestos

1. En el **Explorador de soluciones**, expande el nodo del proyecto de la aplicación para UWP.
2. Haz doble clic en el archivo **package.appxmanifest**.  
*Si el archivo de manifiesto ya está abierto en la vista de código XML, Visual Studio te pedirá que lo cierres.*
3. Haz clic en la pestaña **Capacidades**.
4. Haz clic en la casilla junto a **Punto de servicio** en la lista de las funcionalidades para habilitar la funcionalidad del dispositivo de punto de servicio


## <a name="declare-capability-manually"></a>Declarar la funcionalidad manualmente

1. En el **Explorador de soluciones**, expande el nodo del proyecto de la aplicación para UWP.
2. Haz clic con el botón derecho en el archivo **Package.appxmanifest** y elige **Ver código**.
3. Agrega el elemento PointOfService DeviceCapability a la sección Capacidades de tu manifiesto de la aplicación.  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```
