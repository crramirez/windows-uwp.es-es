---
ms.assetid: F8A741B4-7A6A-4160-8C5D-6B92E267E6EA
title: Emparejar dispositivos
description: Algunos dispositivos deben estar emparejados para que puedan usarse. El espacio de nombres Windows.Devices.Enumeration admite tres modos de emparejar dispositivos.
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: d3fff5a8868cfe29c944336a33c8b6554b74ebf4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175449"
---
# <a name="pair-devices"></a>Emparejar dispositivos



**API importantes**

- [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)

Algunos dispositivos deben estar emparejados para que puedan usarse. El espacio de nombres [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration) admite tres modos de emparejar dispositivos.

-   Emparejamiento automático
-   Emparejamiento básico
-   Emparejamiento personalizado

**Sugerencia**    No es necesario emparejar algunos dispositivos para poder usarlos. Esto se explica en la sección de emparejamiento automático.

 

## <a name="automatic-pairing"></a>Emparejamiento automático


A veces quieres usar un dispositivo en tu aplicación, pero no importa si el dispositivo está emparejado o no. Simplemente quieres poder usar la funcionalidad asociada con un dispositivo. Por ejemplo, si deseas que la aplicación simplemente capture una imagen desde una cámara web, no estás necesariamente interesado en el dispositivo en sí, sino simplemente en la captura de la imagen. Si hay API de dispositivos disponibles para el dispositivo que te interesa, este escenario estaría incluido en el emparejamiento automático.

En este caso, simplemente usa las API asociadas al dispositivo, haciendo las llamadas que sean necesarias y confiando en que el sistema controle el emparejamiento que pueda ser necesario. Algunos dispositivos no necesitan emparejarse para que puedas usar su funcionalidad. Si el dispositivo no necesita emparejarse, las API del dispositivo controlarán la acción de emparejamiento de forma silenciosa para que no tengas que integrar esa funcionalidad en la aplicación. Tu aplicación no tendrá ningún conocimiento sobre si un determinado dispositivo está emparejado o debe estarlo, pero seguirás pudiendo obtener acceso al dispositivo y usando su funcionalidad.

## <a name="basic-pairing"></a>Emparejamiento básico


El emparejamiento básico se produce cuando la aplicación usa las API del espacio de nombres [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration) para intentar emparejar el dispositivo. En este escenario, le permites a Windows que intente el proceso de emparejamiento y lo administre. Si se necesita la intervención del usuario, esto lo gestionará Windows. Usarías el emparejamiento básico necesitas emparejar un dispositivo y no hay una API de dispositivo pertinente que intentará realizar el emparejamiento automático. Simplemente quieres poder usar el dispositivo y necesitas emparejarlo primero.

Para intentar el emparejamiento básico, primero debes obtener el objeto [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) del dispositivo que te interesa. Una vez que recibas ese objeto, tendrás que interactuar con la propiedad [**DeviceInformation.Pairing**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing), la cual es un objeto [**DeviceInformationPairing**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing). Para intentar realizar el emparejamiento, llama a [**DeviceInformationPairing.PairAsync**](/uwp/api/windows.devices.enumeration.deviceinformationpairing.pairasync). Necesitarás **esperar** al resultado para dar tiempo a tu aplicación para que intente completar la acción de emparejamiento. Se devolverá el resultado de la acción del emparejamiento y, siempre y cuando no se devuelva ningún error, se emparejará el dispositivo.

Si estás usando el emparejamiento básico, tienes acceso a información adicional sobre el estado de emparejamiento del dispositivo. Por ejemplo, puedes saber el estado del emparejamiento ([**IsPaired**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.IsPaired)) y si se puede emparejar el dispositivo ([**CanPair**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationPairing.CanPair)). Ambas son propiedades del objeto [**DeviceInformationPairing**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing). Si usas el emparejamiento automático, puede que no tengas acceso a esta información a menos que obtengas los objetos [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) pertinentes.

## <a name="custom-pairing"></a>Emparejamiento personalizado


El emparejamiento personalizado permite a tu aplicación participar en el proceso de emparejamiento. Esto permite a la aplicación especificar los objetos [**DevicePairingKinds**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) que sean compatibles con el proceso de emparejamiento. También tendrás que crear tu propia interfaz de usuario para interactuar con el usuario según sea necesario. Usa el emparejamiento personalizado cuando quieras que tu aplicación tenga un poco más de influencia sobre cómo se realiza el proceso de emparejamiento o para mostrar tu propia interfaz de usuario de emparejamiento.

Para implementar el emparejamiento personalizado, debes obtener el objeto [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) del dispositivo que te interesa, al igual que con el emparejamiento básico. Sin embargo, la propiedad específica que te interesa está en [**DeviceInformation.Pairing.Custom**](/uwp/api/windows.devices.enumeration.deviceinformationpairing.custom). Esto te dará un objeto [**DeviceInformationCustomPairing**](/uwp/api/windows.devices.enumeration.deviceinformationcustompairing). Todos los métodos [**DeviceInformationCustomPairing.PairAsync**](/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairasync) requieren que incluyas un parámetro [**DevicePairingKinds**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds). Esto indica las acciones que el usuario tendrá que hacer para intentar emparejar el dispositivo. Consulta la página de referencia **DevicePairingKinds** para obtener más información sobre los distintos tipos y acciones que el usuario tendrá que hacer. Al igual que con el emparejamiento básico, tendrás que **esperar** al resultado con el fin de que tu aplicación tenga tiempo para completar la acción de emparejamiento. Se devolverá el resultado de la acción del emparejamiento y, siempre y cuando no se devuelva ningún error, se emparejará el dispositivo.

Para admitir el emparejamiento personalizado, debes crear un controlador para el evento [**PairingRequested**](/uwp/api/windows.devices.enumeration.deviceinformationcustompairing.pairingrequested). Este controlador debe asegurarse de tener en cuenta todas las enumeraciones [**DevicePairingKinds**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) distintas que pueden usarse en un escenario de emparejamiento personalizado. La acción apropiada que deberás realizar dependerá de las enumeraciones **DevicePairingKinds** proporcionadas como parte de los argumentos del evento.

Es importante tener en cuenta que el emparejamiento personalizado es siempre una operación de nivel del sistema. Por este motivo, cuando se usa en el escritorio o en Windows Phone, siempre se mostrará un cuadro de diálogo del sistema al usuario cuando se vaya a producir el emparejamiento. Esto es porque ambas plataformas plantean una experiencia de usuario que requiere consentimiento del usuario. Dado que el cuadro de diálogo se genera automáticamente, no necesitarás crear tu propio cuadro de diálogo cuando se opte por una enumeración [**DevicePairingKinds**](/uwp/api/Windows.Devices.Enumeration.DevicePairingKinds) de tipo **ConfirmOnly** al operar en estas plataformas. En cuanto a las demás enumeraciones **DevicePairingKinds**, deberás realizar algunos controles especiales en función del valor **DevicePairingKinds** específico. Consulta la muestra para ver ejemplos de cómo controlar el emparejamiento personalizado en diferentes valores **DevicePairingKinds**.

A partir de Windows 10, versión 1903, se admite un nuevo **DevicePairingKinds** , **ProvidePasswordCredential**. Este valor significa que la aplicación debe solicitar un nombre de usuario y una contraseña al usuario para poder autenticarse con el dispositivo emparejado. Para controlar este caso, llame al método [**AcceptWithPasswordCredential**](/uwp/api/windows.devices.enumeration.devicepairingrequestedeventargs.acceptwithpasswordcredential?branch=release-19h1#Windows_Devices_Enumeration_DevicePairingRequestedEventArgs_AcceptWithPasswordCredential_Windows_Security_Credentials_PasswordCredential_) de los argumentos del evento del controlador de eventos **PairingRequested** para aceptar el emparejamiento. Pase un objeto [**PasswordCredential**](/uwp/api/windows.security.credentials.passwordcredential) que encapsule el nombre de usuario y la contraseña como un parámetro. Tenga en cuenta que el nombre de usuario y la contraseña del dispositivo remoto son distintos y, a menudo, no coinciden con las credenciales del usuario con sesión iniciada localmente.

## <a name="unpairing"></a>Desemparejamiento


Desemparejar un dispositivo solo es relevante en los escenarios de emparejamiento básico o personalizado descritos anteriormente. Si estás usando el emparejamiento automático, la aplicación ignora el estado de emparejamiento del dispositivo y no es necesario desemparejarla. Si decides desemparejar un dispositivo, el proceso es idéntico si implementas el emparejamiento básico o el personalizado. Esto es porque no es necesario proporcionar información adicional o interactuar en el proceso de desemparejamiento.

El primer paso para desemparejar un dispositivo es obtener el objeto [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) del dispositivo que desees desemparejar. A continuación, necesitas recuperar la propiedad [**DeviceInformation.Pairing**](/uwp/api/windows.devices.enumeration.deviceinformation.pairing) y llamar a [**DeviceInformationPairing.UnpairAsync**](/uwp/api/windows.devices.enumeration.deviceinformationpairing.unpairasync). Al igual que con el emparejamiento, tendrás que **esperar** al resultado. Se devolverá el resultado de la acción del desemparejamiento y, siempre y cuando no haya ningún error, se desemparejará el dispositivo.

## <a name="sample"></a>Ejemplo


Para descargar un ejemplo que muestra cómo usar las API de [**Windows. Devices. Enumeration**](/uwp/api/Windows.Devices.Enumeration) , haga clic [aquí](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing).

 

 