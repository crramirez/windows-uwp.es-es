---
author: TerryWarwick
title: Objetos de dispositivo PointOfService
description: Obtener información sobre la creación de objetos de dispositivo PointOfService
ms.author: jken
ms.date: 06/19/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 31af943ab4a9231f58fb2e3d5489e9ae80d8d565
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5937704"
---
# <a name="pointofservice-device-objects"></a>Objetos de dispositivo PointOfService

## <a name="creating-a-device-object"></a>Crear un objeto de dispositivo
Una vez que has identificado el dispositivo PointOfService que deseas usar, desde una enumeración nueva o un DeviceID almacenado, solo tienes que llamar a [**FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) con el [**DeviceID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) que has elegido mediante programación o que el usuario ha seleccionado para crear un nuevo objeto de dispositivo de Punto de servicio.

Esta muestra intenta crear un nuevo objeto BarcodeScanner con FromIdAsync mediante un DeviceID. Si hay un error al crear el objeto, se escribe un mensaje de depuración.

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

Una vez que tengas un objeto de dispositivo, puedes tener acceso a los métodos del dispositivo, a las propiedades y a los eventos.  

## <a name="device-object-lifecycle"></a>Ciclo de vida de objetos de dispositivos
Antes de Windows8, las aplicaciones tenían un ciclo de vida simple. Las aplicaciones Win32 y. NET se están ejecutando o no y los periféricos PointOfService se solían reclamar para el ciclo de vida de aplicación completo. Cuando un usuario las minimiza o sale de ellas, continúan ejecutándose. Esto funcionó bien hasta que los dispositivos portátiles y la administración de la energía empezaron a cobrar cada vez más importancia.

Windows8 introdujo un nuevo modelo de aplicación con las aplicaciones para UWP. En un nivel alto, se agregó un nuevo estado, el estado suspendido. Una aplicación para UWP pasa a estar suspendida poco después de que el usuario la minimice o cambie a otra aplicación. Esto significa que se detienen los subprocesos de la aplicación, la aplicación permanece en memoria a menos que el sistema operativo necesite reclamar recursos y los objetos de dispositivo que representan periféricos de PointOfService se cierran automáticamente para permitir que otras aplicaciones obtengan acceso a los periféricos. Cuando el usuario vuelve a la aplicación, se puede restaurar rápidamente en un estado de ejecución y restaurar las conexiones de periféricos de PointOfService siempre que sigan estando disponibles durante la reanudación.

Puedes detectar cuándo se cierra un objeto por cualquier motivo con un controlador de eventos <DeviceObject>.Closed. A continuación, anota el id. de dispositivo para volver a establecer la conexión en el futuro.   Como alternativa, puedes desear controlar esto en una notificación de suspensión de aplicaciones para guardar el id. de dispositivo para volver a establecer las conexiones del dispositivo en la notificación de la reanudación de la aplicación.  Asegúrate de que no duplicas en los controladores de eventos ni duplicas acciones para el objeto de dispositivo tanto en <DeviceObject>.Closed como en suspensión de aplicaciones.

> [!TIP]
> Consulta los temas siguientes para obtener más información acerca del ciclo de vida de la aplicación para la Plataforma universal de Windows (UWP) de Windows 10:
> - [Ciclo de vida de aplicación para la Plataforma universal de Windows (UWP) de Windows10](../launch-resume/app-lifecycle.md)
> - [Controlar la suspensión de la aplicación](../launch-resume/suspend-an-app.md)
> - [Controlar la reanudación de aplicaciones](../launch-resume/resume-an-app.md)
