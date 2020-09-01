---
title: Objetos de dispositivo PointOfService
description: Obtenga información sobre cómo crear un objeto de dispositivo PointOfService y obtener información sobre el ciclo de vida de objetos de dispositivo en el modelo de aplicación Plataforma universal de Windows (UWP).
ms.date: 06/19/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: db6b47a29e302cb962e5b91cfba823eb7f89db81
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156609"
---
# <a name="pointofservice-device-objects"></a>Objetos de dispositivo PointOfService

## <a name="creating-a-device-object"></a>Crear un objeto de dispositivo

Una vez que haya identificado el dispositivo PointOfService que quiere usar, ya sea desde una nueva enumeración o una DeviceID almacenada, solo tiene que llamar a [**FromIdAsync**](/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) con el[**DeviceID**](/uwp/api/windows.devices.enumeration.deviceinformation.id) que haya elegido mediante programación, o bien el usuario ha seleccionado crear un nuevo objeto de dispositivo de punto de servicio.

En este ejemplo se intenta crear un nuevo objeto BarcodeScanner con FromIdAsync mediante DeviceID. Si se produce un error al crear el objeto, se escribe un mensaje de depuración.

```Csharp

    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(DeviceId);

    if(barcodeScanner != null)
    {
        // after successful creation, claim the scanner for exclusive use and enable it to exchange data
    }
    else
    {
        Debug.WriteLine("Failure to create barcodeScanner object");
    }
    
```

Una vez que tenga un objeto de dispositivo, podrá tener acceso a los métodos, propiedades y eventos del dispositivo.  

## <a name="device-object-lifecycle"></a>Ciclo de vida de objetos de dispositivo

Antes de Windows 8, las aplicaciones tenían un ciclo de vida simple. Las aplicaciones Win32 y .NET están en ejecución o no se están ejecutando y, por lo general, los periféricos PointOfService se han reclamado para el ciclo de vida completo de la aplicación. Cuando un usuario las minimiza o sale de ellas, continúan ejecutándose. Esto funcionó bien hasta que los dispositivos portátiles y la administración de la energía empezaron a cobrar cada vez más importancia.

Windows 8 presentó un nuevo modelo de aplicación con aplicaciones de UWP. En un nivel alto, se agregó un nuevo estado, el estado suspendido. Una aplicación para UWP se suspende poco después de que el usuario la minimice o cambie a otra aplicación. Esto significa que los subprocesos de la aplicación se detienen, la aplicación se deja en memoria a menos que el sistema operativo necesite reclamar recursos y que los objetos de dispositivo que representan periféricos de PointOfService se cierren automáticamente para permitir el acceso de otras aplicaciones a los periféricos. Cuando el usuario vuelve a la aplicación, se puede restaurar rápidamente a un estado de ejecución y restaurar las conexiones de periféricos de PointOfService siempre que estén disponibles en el currículo.

Puede detectar cuándo se cierra un objeto por cualquier motivo con \<DeviceObject\> . Después, el controlador de eventos Closed anota el identificador de dispositivo para volver a establecer la conexión en el futuro.   Como alternativa, puede que desee controlar esto en una notificación de suspensión de la aplicación para guardar el identificador del dispositivo para volver a establecer las conexiones del dispositivo en la notificación de reanudación de la aplicación.  Asegúrese de no doblar los controladores de eventos y las acciones duplicadas del objeto de dispositivo en ambos \<DeviceObject\> . Cerrado y suspender aplicación.

> [!TIP]
> Consulte los temas siguientes para obtener más información sobre el ciclo de vida de las aplicaciones de Windows 10 Plataforma universal de Windows (UWP):
> - [Ciclo de vida de la aplicación de Windows 10 Plataforma universal de Windows (UWP)](../launch-resume/app-lifecycle.md)
> - [Controlar la suspensión de la aplicación](../launch-resume/suspend-an-app.md)
> - [Controlar la reanudación de aplicaciones](../launch-resume/resume-an-app.md)