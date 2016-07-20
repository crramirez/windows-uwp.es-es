---
author: mcleblanc
description: "El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este."
title: "Migración de modelo de E/S, dispositivos y aplicaciones de Windows Phone Silverlight a UWP"
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 6b29e741c9cad68083502b25445b965fc266ef6e

---

#  Migración de modelo de E/S, dispositivos y aplicaciones de Windows Phone Silverlight a UWP

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

El tema anterior era [Migración de XAML y la interfaz de usuario](wpsl-to-uwp-porting-xaml-and-ui.md).

El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este. También puede implicar el procesamiento de datos. No obstante, este código no se considera generalmente como la capa de interfaz de usuario o la capa de datos. Este código incluye la integración con el controlador de vibración, el acelerómetro, el giroscopio, el micrófono y los altavoces (que se relacionan con el reconocimiento y la síntesis de voz), la localización (geográfica) y las modalidades de entrada, como, por ejemplo, entrada táctil, mouse, teclado y lápiz.

## Ciclo de vida de la aplicación (administración de la duración de los procesos)

La aplicación Windows Phone Silverlight contiene código para guardar y restaurar el estado de la aplicación y el estado de visualización con el fin de permitir su exclusión y posterior reactivación. El ciclo de vida de las aplicaciones de Plataforma universal de Windows (UWP) presenta grandes paralelismos con el de las aplicaciones Windows Phone Silverlight, ya que están diseñadas con el mismo objetivo de maximizar los recursos disponibles para las aplicaciones que el usuario elige que estén en primer plano en cualquier momento. Verás que tu código se adaptará al nuevo sistema con razonable facilidad.


            **Nota** Al presionar el botón **Atrás** del hardware, finaliza automáticamente una aplicación WindowsPhone Silverlight. Al presionar el botón **Atrás** del hardware en un dispositivo móvil *no* finaliza automáticamente una aplicación para UWP. En su lugar, se suspende y, después, se puede finalizar. Pero esos detalles son transparentes para una aplicación que responde adecuadamente a los eventos de ciclo de vida de la aplicación.

Una "ventana de espera" es el período de tiempo entre que la aplicación se queda inactiva y que el sistema genera el evento de suspensión. Para una aplicación para UWP no hay ninguna ventana de espera; el evento de suspensión se genera en cuanto una aplicación pasa a estar inactiva.

Para obtener más información, consulta [Ciclo de vida de la aplicación](https://msdn.microsoft.com/library/windows/apps/mt243287).

## Cámara

El código de captura de cámara de WindowsPhone Silverlight usa las clases **Microsoft.Devices.Camera**, **Microsoft.Devices.PhotoCamera** o **Microsoft.Phone.Tasks.CameraCaptureTask**. Para migrar el código a la Plataforma universal de Windows (UWP), puedes usar la clase [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124). En el tema [**CapturePhotoToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700836) se ofrece un ejemplo de código. Ese método permite capturar una foto en un archivo de almacenamiento y requiere las funcionalidades de **micrófono** y **cámara web**[**funcionalidades de dispositivos**](https://msdn.microsoft.com/library/windows/apps/dn934747) estén establecidas en el manifiesto del paquete de la aplicación.

Otra opción es la clase [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030), que también necesita el **micrófono** y la **cámara web**[**funcionalidades de dispositivos**](https://msdn.microsoft.com/library/windows/apps/dn934747).

No se admiten las aplicaciones de modos para las aplicaciones para UWP.

## Detección de la plataforma en la que se está ejecutando la aplicación

La forma de concebir la selección de destinos de aplicación cambia con Windows 10. El nuevo modelo conceptual consiste en que una aplicación tiene como objetivo la Plataforma universal de Windows (UWP) y se ejecuta en todos los dispositivos Windows. A continuación, puede optar por dar a conocer funciones exclusivas de familias de dispositivos en particular. Si es necesario, la aplicación también tiene la opción de limitarse a orientarse a una o más familias de dispositivos específicamente. Para obtener más información sobre qué son las familias de dispositivos —y cómo decidir qué familia de dispositivos establecer como destino— consulta la [Guía de aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn894631).


            **Nota** Es recomendable que no uses el sistema operativo o la familia de dispositivos para detectar la presencia de funciones. Identificar el sistema operativo actual o la familia de dispositivos normalmente no es la mejor manera de determinar si una función concreta de la familia de dispositivos o del sistema operativo está presente. En lugar de detectar el sistema operativo o la familia de dispositivos (y el número de versión), prueba la presencia de la propia funcion (consulta [Compilación condicional y código adaptable](wpsl-to-uwp-porting-to-a-uwp-project.md#conditional-compilation)). Si debes requerir un sistema operativo determinado o una familia de dispositivos, asegúrate de usarlo como una versión compatible mínima, en lugar de diseñar la prueba solamente para esa versión.

Para adaptar la interfaz de usuario de la aplicación a diferentes dispositivos, hay varias técnicas que recomendamos. Continúa usando elementos de tamaño automático y paneles de diseño dinámico como siempre. En el marcado XAML, sigue usando tamaños en píxeles efectivos (anteriormente denominados píxeles de visualización) para que la interfaz de usuario se adapte a diferentes resoluciones y factores de escala (consulta [Píxeles de visualización o efectivos, distancia de visualización y factores de escala](wpsl-to-uwp-porting-xaml-and-ui.md#effective-pixels)). Y usa los desencadenadores y establecedores adaptables de Visual State Manager para adaptar la interfaz de usuario al tamaño de la ventana (consulta [Guía para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn894631)).

Sin embargo, si tienes un escenario donde es inevitable detectar la familia de dispositivos, puedes hacerlo. En este ejemplo, usamos la clase [**AnalyticsVersionInfo**](https://msdn.microsoft.com/library/windows/apps/dn960165) para navegar a una página adaptada a la familia de dispositivos móviles cuando corresponda y nos aseguramos de volver a una página predeterminada de lo contrario.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

La aplicación también puede determinar la familia de dispositivos en la que se está ejecutando desde los factores de selección de recursos que están en vigor. En el siguiente ejemplo se muestra cómo hacerlo de manera imperativa y en el tema [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/br206071) se describe el caso de uso más habitual de la clase en la carga de recursos específicos de familias de dispositivos según el factor de la familia de dispositivos.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Consulta también [Compilación condicional y código adaptable](wpsl-to-uwp-porting-to-a-uwp-project.md#conditional-compilation).

## Estado del dispositivo

Una aplicación de WindowsPhone Silverlight puede usar la clase **Microsoft.Phone.Info.DeviceStatus** para obtener información sobre el dispositivo en el que se ejecuta la aplicación. Si bien no hay ningún equivalente de UWP para el espacio de nombres **Microsoft.Phone.Info**, a continuación detallamos algunas de las propiedades y los eventos que puedes usar en una aplicación para UWP en vez de llamadas a los miembros de la clase **DeviceStatus**.

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 
            Propiedades **ApplicationCurrentMemoryUsage** y **ApplicationCurrentMemoryUsageLimit** | 
            Propiedades [
              **MemoryManager.AppMemoryUsage**
            ](https://msdn.microsoft.com/library/windows/apps/dn633832) y [**AppMemoryUsageLimit**](https://msdn.microsoft.com/library/windows/apps/dn633836)                                                                                                                                    |
| 
            Propiedad **ApplicationPeakMemoryUsage**                                                 | Usa las herramientas de generación de perfiles de memoria en Visual Studio. Para obtener información, consulta [Analizar el uso de memoria](http://msdn.microsoft.com/library/windows/apps/dn645469.aspx).                                                                                                                                                                          |
| 
            Propiedad **DeviceFirmwareVersion**                                                      | 
            Propiedad [
              **EasClientDeviceInformation.SystemFirmwareVersion**
            ](https://msdn.microsoft.com/library/windows/apps/dn608144) (familia de dispositivos de escritorio solamente)                                                                                                                                                                             |
| 
            Propiedad **DeviceHardwareVersion**                                                      | 
            Propiedad [
              **EasClientDeviceInformation.SystemHardwareVersion**
            ](https://msdn.microsoft.com/library/windows/apps/dn608145) (familia de dispositivos de escritorio solamente)                                                                                                                                                                             |
| 
            Propiedad **DeviceManufacturer**                                                         | 
            Propiedad [
              **EasClientDeviceInformation.SystemManufacturer**
            ](https://msdn.microsoft.com/library/windows/apps/hh701398) (familia de dispositivos de escritorio solamente)                                                                                                                                                                                |
| 
            Propiedad **DeviceName**                                                                 | 
            Propiedad [
              **EasClientDeviceInformation.SystemProductName**
            ](https://msdn.microsoft.com/library/windows/apps/hh701401) (familia de dispositivos de escritorio solamente)                                                                                                                                                                                 |
| 
            Propiedad **DeviceTotalMemory**                                                          | No hay equivalente                                                                                                                                                                                                                                                                                                                      |
| 
            Propiedad **IsKeyboardDeployed**                                                         | No hay equivalente. Esta propiedad proporciona información sobre los teclados de hardware para dispositivos móviles, que habitualmente no se usan.                                                                                                                                                                                                        |
| 
            Propiedad **IsKeyboardPresent**                                                          | No hay equivalente. Esta propiedad proporciona información sobre los teclados de hardware para dispositivos móviles, que habitualmente no se usan.                                                                                                                                                                                                        |
| 
            Evento **KeyboardDeployedChanged**                                                       | No hay equivalente. Esta propiedad proporciona información sobre los teclados de hardware para dispositivos móviles, que habitualmente no se usan.                                                                                                                                                                                                        |
| 
            Propiedad **PowerSource**                                                                | No hay equivalente                                                                                                                                                                                                                                                                                                                      |
| 
            Evento **PowerSourceChanged**                                                            | Controla el evento [**RemainingChargePercentChanged**](https://msdn.microsoft.com/library/windows/apps/jj207240) (solo la familia de dispositivos móviles). El evento se genera cuando el valor de la propiedad [**RemainingChargePercent**](https://msdn.microsoft.com/library/windows/apps/jj207239) (solo para la familia de dispositivos móviles) disminuye un 1%. |

## Ubicación

Si una aplicación que declara la funcionalidad de Ubicación en su manifiesto del paquete de la aplicación se usa en Windows 10, el sistema pedirá al usuario final su consentimiento. Por lo tanto, si la aplicación muestra su propia petición de consentimiento personalizado, o si proporciona una alternancia de activar y desactivar, es aconsejable quitarla para que solo se le solicite una vez al usuario final.

## Orientación

El equivalente de la aplicación para UWP de las propiedades **PhoneApplicationPage.SupportedOrientations** y **Orientation** es el elemento [**uap:InitialRotationPreference**](https://msdn.microsoft.com/library/windows/apps/dn934798) en el manifiesto del paquete de la aplicación. Selecciona la pestaña **Aplicación**, si no lo está, y selecciona una o varias casillas de **Giros admitidos** para registrar tus preferencias.

Sin embargo, es conveniente diseñar la interfaz de usuario de la aplicación para UWP para que tenga un aspecto óptimo independientemente del tamaño de pantalla y la orientación del dispositivo. Encontrarás más información al respecto en el tema [Migración para factores de forma y experiencia de usuario](wpsl-to-uwp-form-factors-and-ux.md), que es el tema posterior al siguiente.

El siguiente tema es [Migración de capas de negocio y de datos](wpsl-to-uwp-business-and-data.md).




<!--HONumber=Jun16_HO4-->


