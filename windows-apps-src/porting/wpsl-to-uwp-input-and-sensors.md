---
description: El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este.
title: Trasladar Windows Phone Silverlight a UWP para la e/s, el dispositivo y el modelo de aplicación '
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a62fcb4a208a52fd77be2a9913e265b12bf31f43
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210921"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>Trasladar Windows Phone Silverlight a UWP para el modelo de e/s, dispositivos y aplicaciones


El tema anterior era [Migración de XAML y la interfaz de usuario](wpsl-to-uwp-porting-xaml-and-ui.md).

El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este. También puede implicar el procesamiento de datos. No obstante, este código no se considera generalmente como la capa de interfaz de usuario o la capa de datos. Este código incluye la integración con el controlador de vibración, el acelerómetro, el giroscopio, el micrófono y los altavoces (que se relacionan con el reconocimiento y la síntesis de voz), la localización (geográfica) y las modalidades de entrada, como, por ejemplo, entrada táctil, mouse, teclado y lápiz.

## <a name="application-lifecycle-process-lifetime-management"></a>Ciclo de vida de la aplicación (administración de la duración de los procesos)

La aplicación de Windows Phone Silverlight contiene código para guardar y restaurar el estado de la aplicación y su estado de vista con el fin de admitir la desecho y la reactivación posterior. El ciclo de vida de las aplicaciones Plataforma universal de Windows (UWP) tiene un gran paralelismo con el de Windows Phone aplicaciones de Silverlight, ya que ambos están diseñados con el mismo objetivo de maximizar los recursos disponibles para cualquier aplicación que el usuario haya elegido tener en el primer plano en cualquier momento. Verás que tu código se adaptará al nuevo sistema con razonable facilidad.

**Tenga en cuenta**   al presionar el botón **atrás** de hardware, se finaliza automáticamente una aplicación Windows Phone Silverlight. Al presionar el botón **Atrás** del hardware en un dispositivo móvil *no* finaliza automáticamente una aplicación para UWP. En su lugar, se suspende y, después, se puede finalizar. Pero esos detalles son transparentes para una aplicación que responde adecuadamente a los eventos de ciclo de vida de la aplicación.

Una "ventana de espera" es el período de tiempo entre que la aplicación se queda inactiva y que el sistema genera el evento de suspensión. Para una aplicación para UWP no hay ninguna ventana de espera; el evento de suspensión se genera en cuanto una aplicación pasa a estar inactiva.

Para obtener más información, consulta [Ciclo de vida de la aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle).

## <a name="camera"></a>Cámara

Windows Phone código de captura de cámara de Silverlight utiliza las clases **Microsoft. Devices. Camera**, **Microsoft. Devices. Fotocamera**o **Microsoft. Phone. Tasks. CameraCaptureTask** . Para migrar el código a la Plataforma universal de Windows (UWP), puedes usar la clase [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture). En el tema [**CapturePhotoToStorageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.capturephototostoragefileasync) se ofrece un ejemplo de código. Ese método permite capturar una foto en un archivo de almacenamiento y requiere las funcionalidades de **micrófono** y **cámara web** [**funcionalidades de dispositivos**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability) estén establecidas en el manifiesto del paquete de la aplicación.

Otra opción es la clase [**CameraCaptureUI**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CameraCaptureUI), que también necesita el **micrófono** y la **cámara web** [**funcionalidades de dispositivos**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability).

No se admiten las aplicaciones de modos para las aplicaciones para UWP.

## <a name="detecting-the-platform-your-app-is-running-on"></a>Detección de la plataforma en la que se está ejecutando la aplicación

La forma de pensar en los cambios de destino de aplicaciones con Windows 10. El nuevo modelo conceptual consiste en que una aplicación tiene como objetivo la Plataforma universal de Windows (UWP) y se ejecuta en todos los dispositivos Windows. A continuación, puede optar por dar a conocer funciones exclusivas de familias de dispositivos en particular. Si es necesario, la aplicación también tiene la opción de limitarse a orientarse a una o más familias de dispositivos específicamente. Para obtener más información sobre qué son las familias de dispositivos —y cómo decidir qué familia de dispositivos establecer como destino— consulta la [Guía de aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

**Tenga en cuenta**   se recomienda no usar el sistema operativo o la familia de dispositivos para detectar la presencia de características. Identificar el sistema operativo actual o la familia de dispositivos normalmente no es la mejor manera de determinar si una función concreta de la familia de dispositivos o del sistema operativo está presente. En lugar de detectar el sistema operativo o la familia de dispositivos (y el número de versión), prueba la presencia de la propia funcion (consulta [Compilación condicional y código adaptable](wpsl-to-uwp-porting-to-a-uwp-project.md)). Si debes requerir un sistema operativo determinado o una familia de dispositivos, asegúrate de usarlo como una versión compatible mínima, en lugar de diseñar la prueba solamente para esa versión.

Para adaptar la interfaz de usuario de la aplicación a diferentes dispositivos, hay varias técnicas que recomendamos. Continúa usando elementos de tamaño automático y paneles de diseño dinámico como siempre. En el marcado XAML, sigue usando tamaños en píxeles efectivos (anteriormente denominados píxeles de visualización) para que la interfaz de usuario se adapte a diferentes resoluciones y factores de escala (consulta [Píxeles de visualización o efectivos, distancia de visualización y factores de escala](wpsl-to-uwp-porting-xaml-and-ui.md)). Y usa los desencadenadores y establecedores adaptables de Visual State Manager para adaptar la interfaz de usuario al tamaño de la ventana (consulta [Guía para aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)).

Sin embargo, si tienes un escenario donde es inevitable detectar la familia de dispositivos, puedes hacerlo. En este ejemplo, usamos la clase [**AnalyticsVersionInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.AnalyticsVersionInfo) para navegar a una página adaptada a la familia de dispositivos móviles cuando corresponda y nos aseguramos de volver a una página predeterminada de lo contrario.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

La aplicación también puede determinar la familia de dispositivos en la que se está ejecutando desde los factores de selección de recursos que están en vigor. En el siguiente ejemplo se muestra cómo hacerlo de manera imperativa y en el tema [**ResourceContext.QualifierValues**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) se describe el caso de uso más habitual de la clase en la carga de recursos específicos de familias de dispositivos según el factor de la familia de dispositivos.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Consulta también [Compilación condicional y código adaptable](wpsl-to-uwp-porting-to-a-uwp-project.md).

## <a name="device-status"></a>Estado del dispositivo

Una aplicación de Windows Phone Silverlight puede usar la clase **Microsoft. Phone. info. DeviceStatus** para obtener información sobre el dispositivo en el que se ejecuta la aplicación. Si bien no hay ningún equivalente de UWP para el espacio de nombres **Microsoft.Phone.Info**, a continuación detallamos algunas de las propiedades y los eventos que puedes usar en una aplicación para UWP en vez de llamadas a los miembros de la clase **DeviceStatus**.

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Propiedades **ApplicationCurrentMemoryUsage** y **ApplicationCurrentMemoryUsageLimit** | Propiedades de [**MemoryManager. AppMemoryUsage**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusage) y [**AppMemoryUsageLimit**](https://docs.microsoft.com/uwp/api/windows.system.memorymanager.appmemoryusagelimit)                                                                                                                                    |
| Propiedad **ApplicationPeakMemoryUsage**                                                 | Usa las herramientas de generación de perfiles de memoria en Visual Studio. Para obtener información, consulta [Analizar el uso de memoria](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).                                                                                                                                                                          |
| Propiedad **DeviceFirmwareVersion**                                                      | Propiedad [**EasClientDeviceInformation. SystemFirmwareVersion**](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemfirmwareversion) (solo para la familia de dispositivos de escritorio)                                                                                                                                                                             |
| Propiedad **DeviceHardwareVersion**                                                      | Propiedad [**EasClientDeviceInformation. SystemHardwareVersion**](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemhardwareversion) (solo para la familia de dispositivos de escritorio)                                                                                                                                                                             |
| Propiedad **DeviceManufacturer**                                                         | Propiedad [**EasClientDeviceInformation. SystemManufacturer**](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemmanufacturer) (solo para la familia de dispositivos de escritorio)                                                                                                                                                                                |
| Propiedad **DeviceName**                                                                 | Propiedad [**EasClientDeviceInformation. SystemProductName**](https://docs.microsoft.com/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemproductname) (solo para la familia de dispositivos de escritorio)                                                                                                                                                                                 |
| Propiedad **DeviceTotalMemory**                                                          | No hay equivalente                                                                                                                                                                                                                                                                                                                      |
| Propiedad **IsKeyboardDeployed**                                                         | No hay equivalente. Esta propiedad proporciona información sobre los teclados de hardware para dispositivos móviles, que habitualmente no se usan.                                                                                                                                                                                                        |
| Propiedad **IsKeyboardPresent**                                                          | No hay equivalente. Esta propiedad proporciona información sobre los teclados de hardware para dispositivos móviles, que habitualmente no se usan.                                                                                                                                                                                                        |
| Evento **KeyboardDeployedChanged**                                                       | No hay equivalente. Esta propiedad proporciona información sobre los teclados de hardware para dispositivos móviles, que habitualmente no se usan.                                                                                                                                                                                                        |
| Propiedad **PowerSource**                                                                | No hay equivalente                                                                                                                                                                                                                                                                                                                      |
| Evento **PowerSourceChanged**                                                            | Controla el evento [**RemainingChargePercentChanged**](https://docs.microsoft.com/uwp/api/windows.phone.devices.power.battery.remainingchargepercentchanged) (solo la familia de dispositivos móviles). El evento se genera cuando el valor de la propiedad [**RemainingChargePercent**](https://docs.microsoft.com/uwp/api/windows.phone.devices.power.battery.remainingchargepercent) (solo para la familia de dispositivos móviles) disminuye un 1 %. |

## <a name="location"></a>Ubicación

Cuando una aplicación que declara la funcionalidad de ubicación en el manifiesto del paquete de la aplicación se ejecuta en Windows 10, el sistema solicitará el consentimiento del usuario final. Por lo tanto, si la aplicación muestra su propia petición de consentimiento personalizado, o si proporciona una alternancia de activar y desactivar, es aconsejable quitarla para que solo se le solicite una vez al usuario final.

## <a name="orientation"></a>Orientación

El equivalente de la aplicación para UWP de las propiedades **PhoneApplicationPage.SupportedOrientations** y **Orientation** es el elemento [**uap:InitialRotationPreference**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-splashscreen) en el manifiesto del paquete de la aplicación. Selecciona la pestaña **Aplicación**, si no lo está, y selecciona una o varias casillas de **Giros admitidos** para registrar tus preferencias.

Sin embargo, es conveniente diseñar la interfaz de usuario de la aplicación para UWP para que tenga un aspecto óptimo independientemente del tamaño de pantalla y la orientación del dispositivo. Encontrarás más información al respecto en el tema [Migración para factores de forma y experiencia de usuario](wpsl-to-uwp-form-factors-and-ux.md), que es el tema posterior al siguiente.

El siguiente tema es [Migración de capas de negocio y de datos](wpsl-to-uwp-business-and-data.md).

