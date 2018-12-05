---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: En este artículo se muestra cómo acceder a la luz de un dispositivo y cómo usarla, si la hay. La funcionalidad de luz se administra por separado de la cámara del dispositivo y la funcionalidad de flash de la cámara.
title: Linterna independiente de la cámara
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5fd69900995a51af806c99b25aae43149d6e30a7
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8736585"
---
# <a name="camera-independent-flashlight"></a>Linterna independiente de la cámara



Este artículo muestra cómo acceder y usar la luz de un dispositivo, si está presente. La funcionalidad de luz se administra por separado de la funcionalidad de cámara y flash de cámara del dispositivo. Además de adquirir una referencia a la luz y ajustar su configuración, este artículo también muestra cómo liberar correctamente el recurso de luz cuando no está en uso y cómo detectar cuándo cambia la disponibilidad de la luz en caso de que la esté usando otra aplicación.

## <a name="get-the-devices-default-lamp"></a>Obtener la luz predeterminada del dispositivo

Para obtener el dispositivo de luz predeterminado de un dispositivo, realiza una llamada a [**Lamp.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/dn894327). Las API de luz se encuentran en el espacio de nombres [**Windows.Devices.Lights**](https://msdn.microsoft.com/library/windows/apps/dn894331). Asegúrate de agregar una directiva using a este espacio de nombres antes de intentar acceder a estas API.

[!code-cs[LightsNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetLightsNamespace)]


[!code-cs[DeclareLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDeclareLamp)]


[!code-cs[GetDefaultLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetDefaultLamp)]

Si el objeto devuelto es **null**, la API **Lamp** no es compatible con el dispositivo. Puede que algunos dispositivos no admitan la API **Lamp** incluso si hay una luz físicamente presente en el dispositivo.

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>Obtener una luz específica con la cadena de selector de luz

Algunos dispositivos pueden tener más de una luz. Para obtener una lista de las luces disponibles en el dispositivo, obtén la cadena de selector de dispositivos mediante una llamada a [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/dn894328). A continuación, se puede pasar esta cadena de selector en [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432). Este método se usa para enumerar los distintos tipos de dispositivos y la cadena de selector permite que el método sepa cómo devolver solo los dispositivos de luz. El objeto [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) devuelto por **FindAllAsync** es una colección de objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) que representan las luces disponibles en el dispositivo. Selecciona uno de los objetos en la lista y, a continuación, pasa la propiedad [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) a [**Lamp.FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/dn894326) para obtener una referencia a la luz solicitada. Este ejemplo usa el método de extensión **GetFirstOrDefault** desde el espacio de nombres **System.Linq** para seleccionar el objeto **DeviceInformation** donde la propiedad [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) tiene un valor de **Back**, que selecciona una luz que se encuentra en la parte posterior del dispositivo, si existe una.

Ten en cuenta que las API [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) se encuentran en el espacio de nombres [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459).

[!code-cs[EnumerationNamespace](./code/Lamp/cs/MainPage.xaml.cs#SnippetEnumerationNamespace)]

[!code-cs[GetLampWithSelectionString](./code/Lamp/cs/MainPage.xaml.cs#SnippetGetLampWithSelectionString)]

## <a name="adjust-lamp-settings"></a>Ajustar la configuración de la luz

Una vez que tengas una instancia de la clase [**Lamp**](https://msdn.microsoft.com/library/windows/apps/dn894310), establece la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) en **true** para activar la luz.

[!code-cs[LampSettingsOn](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOn)]

Desactiva la lámpara estableciendo la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/dn894330) en **false**.

[!code-cs[LampSettingsOff](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsOff)]

Algunos dispositivos tienen luces que admiten valores de color. Comprueba si una luz admite color comprobando la propiedad [**IsColorSettable**](https://msdn.microsoft.com/library/windows/apps/dn894329). Si este valor es **true**, puedes definir el color de la luz con la propiedad [**Color**](https://msdn.microsoft.com/library/windows/apps/dn894322).

[!code-cs[LampSettingsColor](./code/Lamp/cs/MainPage.xaml.cs#SnippetLampSettingsColor)]

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>Registrarse para recibir notificaciones cuando cambie la disponibilidad de luz

El acceso a la luz se concede a la aplicación que solicitó acceso más recientemente. Por lo tanto, si otra aplicación se inicia y solicita un recurso de luz que tu aplicación está usando actualmente, la aplicación ya no podrá controlar la luz hasta que la otra aplicación haya liberado el recurso. Para recibir una notificación cuando cambie la disponibilidad de la luz, registra un controlador para el evento [**Lamp.AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317).

[!code-cs[AvailabilityChanged](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChanged)]

En el controlador para el evento, comprueba la propiedad [**LampAvailabilityChanged.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/dn894315) para determinar si la luz está disponible. En este ejemplo, se habilita o deshabilita un modificador de alternancia para activar y desactivar la luz en función de la disponibilidad de luz.

[!code-cs[AvailabilityChangedHandler](./code/Lamp/cs/MainPage.xaml.cs#SnippetAvailabilityChangedHandler)]

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>Desechar correctamente el recurso de luz cuando no esté en uso

Cuando ya no uses la luz, debes deshabilitarla y llamar a [**Lamp.Close**](https://msdn.microsoft.com/library/windows/apps/dn894320) para liberar el recurso y permitir que otras aplicaciones puedan acceder a la luz. Esta propiedad se asigna al método **Dispose** si estás usando C#. Si registraste la aplicación para [**AvailabilityChanged**](https://msdn.microsoft.com/library/windows/apps/dn894317), debes anular el registro del controlador cuando deseches el recurso de luz. El lugar adecuado en el código para desechar el recurso de luz depende de la aplicación. Para limitar el ámbito de acceso a la luz a una sola página, libera el recurso en el evento [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509).

[!code-cs[DisposeLamp](./code/Lamp/cs/MainPage.xaml.cs#SnippetDisposeLamp)]

## <a name="related-topics"></a>Temas relacionados
- [Reproducción de contenido multimedia](media-playback.md)

 




