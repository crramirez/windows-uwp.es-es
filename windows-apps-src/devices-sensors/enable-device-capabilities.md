---
ms.assetid: 949D1CE0-DD7D-420E-904D-758FADEBE85A
title: Habilitar funcionalidades de dispositivos
description: Este tutorial describe cómo declarar funcionalidades del dispositivo en Microsoft Visual Studio. Esta opción permite que la aplicación use cámaras, micrófonos, sensores de ubicación y otros dispositivos.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cd8c493333c2c35dee5ead064f9d002701cc1148
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370224"
---
# <a name="enable-device-capabilities"></a>Habilitar funcionalidades de dispositivos



Este tutorial describe cómo declarar funcionalidades del dispositivo en Microsoft Visual Studio. Esta opción permite que la aplicación use cámaras, micrófonos, sensores de ubicación y otros dispositivos.

## <a name="specify-the-device-capabilities-your-app-will-use"></a>Especificar la funcionalidad de los dispositivos que usará la aplicación


Las aplicaciones de Windows requieren que especifiques en el manifiesto del paquete de la aplicación cuándo se usan determinados tipos de dispositivos. En Visual Studio puedes declarar la mayoría de las funcionalidades mediante el [Diseñador de manifiestos](https://docs.microsoft.com/previous-versions/br230259(v=vs.140)), o puedes agregarlas manualmente tal como se describe en [Cómo especificar funcionalidades de dispositivos en un manifiesto del paquete (manualmente)](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest). En este tutorial se supone que vas a usar el Diseñador de manifiestos.

**Tenga en cuenta**    algunos tipos de dispositivos, como impresoras, escáneres y sensores, no necesitan declararse en el manifiesto del paquete de aplicación.

-   En el explorador de soluciones de Visual Studio, haz doble clic en el archivo del manifiesto del paquete **Package.appxmanifest**.
-   Abre la pestaña **Capacidades**.
-   Selecciona las funcionalidades de dispositivos que use tu aplicación. Si no ves la funcionalidad que buscas en el Diseñador de manifiestos, agrégala manualmente. Para más información, consulta [Cómo especificar funcionalidades de dispositivos en un manifiesto del paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest).

| Funcionalidad del dispositivo | Diseñador de manifiestos | Descripción |
|-------------------|-------------------|-------------|    
| AllJoyn | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Permite que los dispositivos y las aplicaciones habilitadas para AllJoyn se detecten e interactúen entre sí. Todas las aplicaciones que tienen acceso a las API del espacio de nombres [**Windows.Devices.AllJoyn**](https://docs.microsoft.com/uwp/api/Windows.Devices.AllJoyn) deben usar esta funcionalidad. |
| Mensajes de chat bloqueados | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Permite que las aplicaciones lean mensajes SMS y MMS bloqueados por la aplicación de filtro de correo no deseado. |
| Acceso a mensajes de chat | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Permite que las aplicaciones lean y eliminen mensajes de texto. Esta funcionalidad también permite a las aplicaciones almacenar los mensajes de chat en el almacén de datos del sistema. |
| Generación de código | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Permite que las aplicaciones generen código de forma dinámica. |
| Autenticación de empresa | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Esta funcionalidad está sujeta a la directiva de Microsoft Store. Proporciona la capacidad de conectarse a recursos de la intranet de empresa que requieran credenciales de dominio. Normalmente, esta funcionalidad no es necesaria para la mayoría de las aplicaciones. | 
| Internet (cliente) | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona acceso saliente a Internet y a redes de lugares públicos, como aeropuertos y cafeterías. Por ejemplo, redes intranet en las que el usuario ha designado la red como pública. La mayoría de las aplicaciones que requieran acceso a Internet deben usar esta funcionalidad. |
| Internet (cliente y servidor) | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona acceso entrante y saliente a Internet y a redes de lugares públicos, como aeropuertos y cafeterías. Esta funcionalidad es un superconjunto de **Internet (cliente)** . No es necesario habilitar **Internet (cliente)** si también se habilita esta funcionalidad. El acceso entrante a puertos críticos siempre está bloqueado. |
| Location| ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona acceso a la ubicación actual. Se obtiene del hardware dedicado, como un sensor GPS en el equipo, o se deriva de la información de red disponible. | 
| Micrófono | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona acceso al audio del micrófono. Permite que la aplicación grabe con los micrófonos conectados. | 
| Biblioteca de música | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona la capacidad de agregar, cambiar o eliminar archivos de la **Biblioteca de música** del equipo local y de equipos de **grupo en el hogar**. | 
| Objetos 3D | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona acceso mediante programación a los **Objetos 3D** del usuario, lo que permite a la aplicación enumerar y tener acceso a todos los archivos de la biblioteca sin que haya interacción del usuario. Esta funcionalidad se usa normalmente en juegos y aplicaciones 3D que necesitan tener acceso a toda la biblioteca de **Objetos 3D**. | 
| Llamada de teléfono | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Permite a las aplicaciones tener acceso a todas las líneas de teléfono del dispositivo y efectuar las siguientes funciones: realizar una llamada con el teléfono y mostrar el marcador del sistema sin pedir confirmación al usuario, obtener acceso a los metadatos relacionados con la línea u obtener acceso a desencadenadores de línea. Permite que la aplicación de filtro de correo no deseado seleccionada por el usuario establezca y compruebe la lista de bloqueados y la información de origen de las llamadas. | 
| Biblioteca de imágenes | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona la capacidad de agregar, cambiar o eliminar archivos de la **Biblioteca de música** del equipo local y de los equipos de **grupo en el hogar**. | 
| Punto de servicio | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona acceso a dispositivos periféricos del Punto de servicio. Esta funcionalidad es necesaria para acceder a las API del espacio de nombres [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService). | 
| Redes privadas (cliente y servidor) | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona acceso entrante y saliente a las redes de intranet que tengan un controlador de dominio autenticado, o que el usuario haya designado como red doméstica o del trabajo. El acceso entrante a puertos críticos siempre está bloqueado. | 
| Proximidad | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona la capacidad de conectarse a dispositivos cercanos al equipo a través de la transmisión de datos en proximidad (NFC). La proximidad puede usarse para enviar archivos o para comunicarse con una aplicación de un dispositivo cercano. | 
| Almacenamiento extraíble | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona la capacidad de agregar, cambiar o eliminar archivos de dispositivos de almacenamiento extraíble. La aplicación solo puede tener acceso a los tipos de archivo de medios de almacenamiento extraíble definidos en el manifiesto mediante la declaración **Asociaciones de tipos de archivo**. La aplicación no puede tener acceso a medios de almacenamiento extraíble de equipos de **grupo en el hogar**. | 
| Certificados de usuario compartidos | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Esta funcionalidad está sujeta a la directiva de Microsoft Store. Proporciona la funcionalidad de tener acceso a los certificados de software y hardware como, por ejemplo, certificados de tarjeta inteligente, para validar la identidad de un usuario. Cuando se invocan API relacionadas en tiempo de ejecución, el usuario debe realizar una acción (insertar la tarjeta, seleccionar el certificado, etc.). Esta funcionalidad no es necesaria si la aplicación incluye un certificado privado mediante una declaración **Certificates**. | 
| Información de cuenta de usuario | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Permite a las aplicaciones tener acceso al nombre y la imagen del usuario. Esta funcionalidad es necesaria para acceder a algunas API del espacio de nombres [**Windows.System.UserProfile**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile). | 
| Biblioteca de vídeos | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona la capacidad de agregar, cambiar o eliminar archivos de la **Biblioteca de vídeos** del equipo local y de los equipos de **grupo en el hogar**. | 
| Llamada VOIP | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Permite a las aplicaciones tener acceso a las API de llamadas VOIP en el espacio de nombres [**Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls). | 
| Cámara web | ![Disponible en el Diseñador de manifiestos](images/ap-tools.png) | Proporciona acceso al vídeo de la cámara integrada o de la cámara web conectada. Permite que la aplicación capture imágenes y películas. | 
| USB | | Proporciona acceso a dispositivos USB personalizados. Esta capacidad requiere elementos secundarios. Esta característica no se admite en Windows Phone. | 
| Dispositivo de interfaz humana (HID) | | Proporciona acceso a dispositivos de interfaz de usuario (HID). Esta capacidad requiere elementos secundarios. Para más información, consulta [Cómo especificar funcionalidades de dispositivos para HID](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-hid). | 
| Bluetooth GATT | | Proporciona acceso a dispositivos Bluetooth LE mediante una colección de servicios primarios, incluidos servicios, características y descriptores. Esta capacidad requiere elementos secundarios. Para más información, consulta [Cómo especificar funcionalidades de dispositivos para Bluetooth](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-bluetooth). | 
| Bluetooth RFCOMM |  | Proporciona acceso a API que admiten el transporte BR/EDR (Basic Rate/Extended Data Rate) y también permite que tu aplicación de UWP pueda acceder a un dispositivo que implemente SPP (Serial Port Profile). Esta capacidad requiere elementos secundarios. Para más información, consulta [Cómo especificar funcionalidades de dispositivos para Bluetooth](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-bluetooth). |

## <a name="use-the-windows-runtime-api-for-communicating-with-your-device"></a>Usa la API de Windows Runtime para comunicarte con tu dispositivo

La siguiente tabla conecta algunas de las funcionalidades a las API de Windows Runtime.

| Funcionalidad del dispositivo        | API             | 
|--------------------------|-----------------|
| AllJoyn                  | [**Windows.Devices.AllJoyn**](https://docs.microsoft.com/uwp/api/Windows.Devices.AllJoyn) | 
| Mensajes de chat bloqueados    | [**Windows.ApplicationModel.CommunicationBlocking**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.CommunicationBlocking) | 
| Location                 | Consulta [Introducción a ubicación y mapas](https://docs.microsoft.com/windows/uwp/maps-and-location/index) para obtener más información. | 
| Llamada de teléfono               | [**Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls) | 
| Información de cuenta de usuario | [**Windows.System.UserProfile**](https://docs.microsoft.com/uwp/api/Windows.System.UserProfile) | 
| Llamada VOIP             | [**Windows.ApplicationModel.Calls**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Calls) | 
| USB                      | [**Windows.Devices.Usb**](https://docs.microsoft.com/uwp/api/Windows.Devices.Usb) | 
| HID                      | [**Windows.Devices.HumanInterfaceDevice**](https://docs.microsoft.com/uwp/api/Windows.Devices.HumanInterfaceDevice) | 
| Bluetooth GATT           | [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) | 
| Bluetooth RFCOMM         | [**Windows.Devices.Bluetooth.Rfcomm**](https://docs.microsoft.com/uwp/api/Windows.Devices.Bluetooth.Rfcomm) | 
| Punto de servicio         | [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) |

