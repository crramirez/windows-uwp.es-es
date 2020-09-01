---
description: El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este.
title: Trasladar Windows Phone Silverlight a UWP para la e/s, el dispositivo y el modelo de aplicación '
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 09b192f38a5bedaad491ade322df252d4b9e5971
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162189"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>Migración de modelo de E/S, dispositivos y aplicaciones de Windows Phone Silverlight a UWP


El tema anterior era [Migración de XAML y la interfaz de usuario](wpsl-to-uwp-porting-xaml-and-ui.md).

El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este. También puede implicar el procesamiento de datos. No obstante, este código no se considera generalmente como la capa de interfaz de usuario o la capa de datos. Este código incluye la integración con el controlador de vibración, el acelerómetro, el giroscopio, el micrófono y los altavoces (que se relacionan con el reconocimiento y la síntesis de voz), la localización (geográfica) y las modalidades de entrada, como, por ejemplo, entrada táctil, mouse, teclado y lápiz.

## <a name="application-lifecycle-process-lifetime-management"></a>Ciclo de vida de la aplicación (administración de la duración de los procesos)

La aplicación Windows Phone Silverlight contiene código para guardar y restaurar el estado de la aplicación y el estado de visualización con el fin de permitir su exclusión y posterior reactivación. El ciclo de vida de las aplicaciones de Plataforma universal de Windows (UWP) presenta grandes paralelismos con el de las aplicaciones Windows Phone Silverlight, ya que están diseñadas con el mismo objetivo de maximizar los recursos disponibles para las aplicaciones que el usuario elige que estén en primer plano en cualquier momento. Verás que tu código se adaptará al nuevo sistema con razonable facilidad.

**Nota:**    Al presionar el botón **atrás** de hardware, se finaliza automáticamente una aplicación de Windows Phone Silverlight. Al presionar el botón **Atrás** del hardware en un dispositivo móvil *no* finaliza automáticamente una aplicación para UWP. En su lugar, se suspende y, después, se puede finalizar. Pero esos detalles son transparentes para una aplicación que responde adecuadamente a los eventos de ciclo de vida de la aplicación.

Una "ventana de espera" es el período de tiempo entre que la aplicación se queda inactiva y que el sistema genera el evento de suspensión. Para una aplicación para UWP no hay ninguna ventana de espera; el evento de suspensión se genera en cuanto una aplicación pasa a estar inactiva.

Para obtener más información, consulte [ciclo de vida](../launch-resume/app-lifecycle.md)de la aplicación.

## <a name="camera"></a>Cámara

El código de captura de cámara de Windows Phone Silverlight usa las clases **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** o **Microsoft.Phone.Tasks.CameraCaptureTask**. Para migrar el código a la Plataforma universal de Windows (UWP), puedes usar la clase [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture). En el tema [**CapturePhotoToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostoragefileasync) se ofrece un ejemplo de código. Ese método permite capturar una foto en un archivo de almacenamiento y requiere las funcionalidades de **micrófono** y **cámara web** [**funcionalidades de dispositivos**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability) estén establecidas en el manifiesto del paquete de la aplicación.

Otra opción es la clase [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI), que también necesita el **micrófono** y la **cámara web** [**funcionalidades de dispositivos**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability).

No se admiten las aplicaciones de modos para las aplicaciones para UWP.

## <a name="detecting-the-platform-your-app-is-running-on"></a>Detección de la plataforma en la que se está ejecutando la aplicación

La forma de concebir la selección de destinos de aplicación cambia con Windows 10. El nuevo modelo conceptual consiste en que una aplicación tiene como objetivo la Plataforma universal de Windows (UWP) y se ejecuta en todos los dispositivos Windows. A continuación, puede optar por dar a conocer funciones exclusivas de familias de dispositivos en particular. Si es necesario, la aplicación también tiene la opción de limitarse a orientarse a una o más familias de dispositivos específicamente. Para obtener más información sobre qué son las familias de dispositivos —y cómo decidir qué familia de dispositivos establecer como destino— consulta la [Guía de aplicaciones para UWP](../get-started/universal-application-platform-guide.md).

**Nota:**    Se recomienda no usar el sistema operativo o la familia de dispositivos para detectar la presencia de características. Identificar el sistema operativo actual o la familia de dispositivos normalmente no es la mejor manera de determinar si una función concreta de la familia de dispositivos o del sistema operativo está presente. En lugar de detectar el sistema operativo o la familia de dispositivos (y el número de versión), prueba la presencia de la propia funcion (consulta [Compilación condicional y código adaptable](wpsl-to-uwp-porting-to-a-uwp-project.md)). Si debes requerir un sistema operativo determinado o una familia de dispositivos, asegúrate de usarlo como una versión compatible mínima, en lugar de diseñar la prueba solamente para esa versión.

Para adaptar la interfaz de usuario de la aplicación a diferentes dispositivos, hay varias técnicas que recomendamos. Continúa usando elementos de tamaño automático y paneles de diseño dinámico como siempre. En el marcado XAML, sigue usando tamaños en píxeles efectivos (anteriormente denominados píxeles de visualización) para que la interfaz de usuario se adapte a diferentes resoluciones y factores de escala (consulta [Píxeles de visualización o efectivos, distancia de visualización y factores de escala](wpsl-to-uwp-porting-xaml-and-ui.md)). Y usa los desencadenadores y establecedores adaptables de Visual State Manager para adaptar la interfaz de usuario al tamaño de la ventana (consulta [Guía para aplicaciones para UWP](../get-started/universal-application-platform-guide.md)).

Sin embargo, si tienes un escenario donde es inevitable detectar la familia de dispositivos, puedes hacerlo. En este ejemplo, usamos la clase [**AnalyticsVersionInfo**](/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) para navegar a una página adaptada a la familia de dispositivos móviles cuando corresponda y nos aseguramos de volver a una página predeterminada de lo contrario.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

La aplicación también puede determinar la familia de dispositivos en la que se está ejecutando desde los factores de selección de recursos que están en vigor. En el siguiente ejemplo se muestra cómo hacerlo de manera imperativa y en el tema [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) se describe el caso de uso más habitual de la clase en la carga de recursos específicos de familias de dispositivos según el factor de la familia de dispositivos.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Consulta también [Compilación condicional y código adaptable](wpsl-to-uwp-porting-to-a-uwp-project.md).

## <a name="device-status"></a>Estado del dispositivo

Una aplicación de Windows Phone Silverlight puede usar la clase **Microsoft.Phone.Info.DeviceStatus** para obtener información sobre el dispositivo en el que se ejecuta la aplicación. Si bien no hay ningún equivalente de UWP para el espacio de nombres **Microsoft.Phone.Info**, a continuación detallamos algunas de las propiedades y los eventos que puedes usar en una aplicación para UWP en vez de llamadas a los miembros de la clase **DeviceStatus**.

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Propiedades **ApplicationCurrentMemoryUsage** y **ApplicationCurrentMemoryUsageLimit** | Propiedades [**MemoryManager.AppMemoryUsage**](/uwp/api/windows.system.memorymanager.appmemoryusage) y [**AppMemoryUsageLimit**](/uwp/api/windows.system.memorymanager.appmemoryusagelimit)                                                                                                                                    |
| Propiedad **ApplicationPeakMemoryUsage**                                                 | Usa las herramientas de generación de perfiles de memoria en Visual Studio. Para obtener información, consulta [Analizar el uso de memoria](/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).                                                                                                                                                                          |
| Propiedad **DeviceFirmwareVersion**                                                      | Propiedad [**EasClientDeviceInformation.SystemFirmwareVersion**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemfirmwareversion) (familia de dispositivos de escritorio solamente)                                                                                                                                                                             |
| Propiedad **DeviceHardwareVersion**                                                      | Propiedad [**EasClientDeviceInformation.SystemHardwareVersion**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemhardwareversion) (solo familia de dispositivos de escritorio)                                                                                                                                                                             |
| Propiedad **DeviceManufacturer**                                                         | Propiedad [**EasClientDeviceInformation.SystemManufacturer**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemmanufacturer) (familia de dispositivos de escritorio solamente)                                                                                                                                                                                |
| Propiedad **DeviceName**                                                                 | Propiedad [**EasClientDeviceInformation.SystemProductName**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemproductname) (familia de dispositivos de escritorio solamente)                                                                                                                                                                                 |
| Propiedad **DeviceTotalMemory**                                                          | No equivalente                                                                                                                                                                                                                                                                                                                      |
| Propiedad **IsKeyboardDeployed**                                                         | No equivalente. Esta propiedad proporciona información sobre los teclados de hardware para dispositivos móviles, que habitualmente no se usan.                                                                                                                                                                                                        |
| Propiedad **IsKeyboardPresent**                                                          | No equivalente. Esta propiedad proporciona información sobre los teclados de hardware para dispositivos móviles, que habitualmente no se usan.                                                                                                                                                                                                        |
| Evento **KeyboardDeployedChanged**                                                       | No equivalente. Esta propiedad proporciona información sobre los teclados de hardware para dispositivos móviles, que habitualmente no se usan.                                                                                                                                                                                                        |
| Propiedad **PowerSource**                                                                | No equivalente                                                                                                                                                                                                                                                                                                                      |
| Evento **PowerSourceChanged**                                                            | Controla el evento [**RemainingChargePercentChanged**](/uwp/api/windows.phone.devices.power.battery.remainingchargepercentchanged) (solo la familia de dispositivos móviles). El evento se genera cuando el valor de la propiedad [**RemainingChargePercent**](/uwp/api/windows.phone.devices.power.battery.remainingchargepercent) (solo para la familia de dispositivos móviles) disminuye un 1 %. |

## <a name="location"></a>Location

Si una aplicación que declara la funcionalidad de Ubicación en su manifiesto del paquete de la aplicación se usa en Windows 10, el sistema pedirá al usuario final su consentimiento. Por lo tanto, si la aplicación muestra su propia petición de consentimiento personalizado, o si proporciona una alternancia de activar y desactivar, es aconsejable quitarla para que solo se le solicite una vez al usuario final.

## <a name="orientation"></a>Orientación

El equivalente de la aplicación para UWP de las propiedades **PhoneApplicationPage.SupportedOrientations** y **Orientation** es el elemento [**uap:InitialRotationPreference**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-splashscreen) en el manifiesto del paquete de la aplicación. Selecciona la pestaña **Aplicación**, si no lo está, y selecciona una o varias casillas de **Giros admitidos** para registrar tus preferencias.

Sin embargo, es conveniente diseñar la interfaz de usuario de la aplicación para UWP para que tenga un aspecto óptimo independientemente del tamaño de pantalla y la orientación del dispositivo. Encontrarás más información al respecto en el tema [Migración para factores de forma y experiencia de usuario](wpsl-to-uwp-form-factors-and-ux.md), que es el tema posterior al siguiente.

El siguiente tema es [Migración de capas de negocio y de datos](wpsl-to-uwp-business-and-data.md).