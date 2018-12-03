---
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: Emparejar dispositivos
description: Algunos dispositivos deben estar emparejados para que puedan usarse. El espacio de nombres Windows.Devices.Enumeration admite tres modos de emparejar dispositivos.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ccff9d892fbedc62cf1b54e374a0071877805731
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458446"
---
# <a name="pair-devices"></a>Emparejar dispositivos



**API importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

Algunos dispositivos deben estar emparejados para que puedan usarse. El espacio de nombres [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) admite tres modos de emparejar dispositivos.

-   Emparejamiento automático
-   Emparejamiento básico
-   Emparejamiento personalizado

**Sugerencia**algunos dispositivos no necesitan emparejarse con el fin de usarse. Esto se explica en la sección de emparejamiento automático.

 

## <a name="automatic-pairing"></a>Emparejamiento automático


A veces quieres usar un dispositivo en tu aplicación, pero no importa si el dispositivo está emparejado o no. Simplemente quieres poder usar la funcionalidad asociada con un dispositivo. Por ejemplo, si deseas que la aplicación simplemente capture una imagen desde una cámara web, no estás necesariamente interesado en el dispositivo en sí, sino simplemente en la captura de la imagen. Si hay API de dispositivos disponibles para el dispositivo que te interesa, este escenario estaría incluido en el emparejamiento automático.

En este caso, simplemente usa las API asociadas al dispositivo, haciendo las llamadas que sean necesarias y confiando en que el sistema controle el emparejamiento que pueda ser necesario. Algunos dispositivos no necesitan emparejarse para que puedas usar su funcionalidad. Si el dispositivo no necesita emparejarse, las API del dispositivo controlarán la acción de emparejamiento de forma silenciosa para que no tengas que integrar esa funcionalidad en la aplicación. Tu aplicación no tendrá ningún conocimiento sobre si un determinado dispositivo está emparejado o debe estarlo, pero seguirás pudiendo obtener acceso al dispositivo y usando su funcionalidad.

## <a name="basic-pairing"></a>Emparejamiento básico


El emparejamiento básico se produce cuando la aplicación usa las API del espacio de nombres [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) para intentar emparejar el dispositivo. En este escenario, le permites a Windows que intente el proceso de emparejamiento y lo administre. Si se necesita la intervención del usuario, esto lo gestionará Windows. Usarías el emparejamiento básico necesitas emparejar un dispositivo y no hay una API de dispositivo pertinente que intentará realizar el emparejamiento automático. Simplemente quieres poder usar el dispositivo y necesitas emparejarlo primero.

Para intentar el emparejamiento básico, primero debes obtener el objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) del dispositivo que te interesa. Una vez que recibas ese objeto, tendrás que interactuar con la propiedad [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx), la cual es un objeto [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx). Para intentar realizar el emparejamiento, llama a [**DeviceInformationPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/mt608800). Necesitarás **esperar** al resultado para dar tiempo a tu aplicación para que intente completar la acción de emparejamiento. Se devolverá el resultado de la acción del emparejamiento y, siempre y cuando no se devuelva ningún error, se emparejará el dispositivo.

Si estás usando el emparejamiento básico, tienes acceso a información adicional sobre el estado de emparejamiento del dispositivo. Por ejemplo, puedes saber el estado del emparejamiento ([**IsPaired**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.IsPaired)) y si se puede emparejar el dispositivo ([**CanPair**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.CanPair)). Ambas son propiedades del objeto [**DeviceInformationPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx). Si usas el emparejamiento automático, puede que no tengas acceso a esta información a menos que obtengas los objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) pertinentes.

## <a name="custom-pairing"></a>Emparejamiento personalizado


El emparejamiento personalizado permite a tu aplicación participar en el proceso de emparejamiento. Esto permite a la aplicación especificar los objetos [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) que sean compatibles con el proceso de emparejamiento. También tendrás que crear tu propia interfaz de usuario para interactuar con el usuario según sea necesario. Usa el emparejamiento personalizado cuando quieras que tu aplicación tenga un poco más de influencia sobre cómo se realiza el proceso de emparejamiento o para mostrar tu propia interfaz de usuario de emparejamiento.

Para implementar el emparejamiento personalizado, debes obtener el objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) del dispositivo que te interesa, al igual que con el emparejamiento básico. Sin embargo, la propiedad específica que te interesa está en [**DeviceInformation.Pairing.Custom**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.custom.aspx). Esto te dará un objeto [**DeviceInformationCustomPairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.aspx). Todos los métodos [**DeviceInformationCustomPairing.PairAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.pairasync.aspx) requieren que incluyas un parámetro [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808). Esto indica las acciones que el usuario tendrá que hacer para intentar emparejar el dispositivo. Consulta la página de referencia **DevicePairingKinds** para obtener más información sobre los distintos tipos y acciones que el usuario tendrá que hacer. Al igual que con el emparejamiento básico, tendrás que **esperar** al resultado con el fin de que tu aplicación tenga tiempo para completar la acción de emparejamiento. Se devolverá el resultado de la acción del emparejamiento y, siempre y cuando no se devuelva ningún error, se emparejará el dispositivo.

Para admitir el emparejamiento personalizado, debes crear un controlador para el evento [**PairingRequested**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcustompairing.pairingrequested.aspx). Este controlador debe asegurarse de tener en cuenta todas las enumeraciones [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) distintas que pueden usarse en un escenario de emparejamiento personalizado. La acción apropiada que deberás realizar dependerá de las enumeraciones **DevicePairingKinds** proporcionadas como parte de los argumentos del evento.

Es importante tener en cuenta que el emparejamiento personalizado es siempre una operación de nivel del sistema. Por este motivo, cuando se usa en el escritorio o en Windows Phone, siempre se mostrará un cuadro de diálogo del sistema al usuario cuando se vaya a producir el emparejamiento. Esto es porque ambas plataformas plantean una experiencia de usuario que requiere consentimiento del usuario. Dado que el cuadro de diálogo se genera automáticamente, no necesitarás crear tu propio cuadro de diálogo cuando se opte por una enumeración [**DevicePairingKinds**](https://msdn.microsoft.com/library/windows/apps/Mt608808) de tipo **ConfirmOnly** al operar en estas plataformas. En cuanto a las demás enumeraciones **DevicePairingKinds**, deberás realizar algunos controles especiales en función del valor **DevicePairingKinds** específico. Consulta la muestra para ver ejemplos de cómo controlar el emparejamiento personalizado en diferentes valores **DevicePairingKinds**.

## <a name="unpairing"></a>Desemparejamiento


Desemparejar un dispositivo solo es relevante en los escenarios de emparejamiento básico o personalizado descritos anteriormente. Si estás usando el emparejamiento automático, la aplicación ignora el estado de emparejamiento del dispositivo y no es necesario desemparejarla. Si decides desemparejar un dispositivo, el proceso es idéntico si implementas el emparejamiento básico o el personalizado. Esto es porque no es necesario proporcionar información adicional o interactuar en el proceso de desemparejamiento.

El primer paso para desemparejar un dispositivo es obtener el objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) del dispositivo que desees desemparejar. A continuación, necesitas recuperar la propiedad [**DeviceInformation.Pairing**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.pairing.aspx) y llamar a [**DeviceInformationPairing.UnpairAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationpairing.unpairasync). Al igual que con el emparejamiento, tendrás que **esperar** al resultado. Se devolverá el resultado de la acción del desemparejamiento y, siempre y cuando no haya ningún error, se desemparejará el dispositivo.

## <a name="sample"></a>Muestra


Para descargar una muestra que te indique cómo usar las API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459), haz clic [aquí](http://go.microsoft.com/fwlink/?LinkID=620536).

 

 
