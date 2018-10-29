---
author: stevewhims
description: El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este.
title: Migración de Windows Runtime 8.x a UWP para E/S, dispositivo y modelo de aplicaciones
ms.assetid: bb13fb8f-bdec-46f5-8640-57fb0dd2d85b
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e15014e39ed6d980cbe80daa0a129ff83a021b9
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5751787"
---
# <a name="porting-windows-runtime-8x-to-uwp-for-io-device-and-app-model"></a>Migración de Windows Runtime 8.x a UWP para E/S, dispositivo y modelo de aplicaciones




El tema anterior era [Migración de XAML y la interfaz de usuario](w8x-to-uwp-porting-xaml-and-ui.md).

El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este. También puede implicar el procesamiento de datos. No obstante, este código no se considera generalmente como la capa de interfaz de usuario *o* la capa de datos. Este código incluye la integración con el controlador de vibración, el acelerómetro, el giroscopio, el micrófono y los altavoces (que se relacionan con el reconocimiento y la síntesis de voz), la localización (geográfica) y las modalidades de entrada, como, por ejemplo, entrada táctil, mouse, teclado y lápiz.

## <a name="application-lifecycle-process-lifetime-management"></a>Ciclo de vida de la aplicación (administración de la duración de los procesos)


Para una aplicación Universal 8.1, hay una "ventana de espera" de dos segundos de tiempo entre la aplicación que se está quedando inactiva y el sistema que está generando el evento de suspensión. Usar esta ventana de espera como tiempo adicional del estado de suspensión no resulta seguro, y en el caso de las aplicaciones para la Plataforma universal de Windows (UWP), no hay ninguna ventana de espera; se genera el evento de suspensión tan pronto como una aplicación pasa a estar inactiva.

Para obtener más información, consulta [Ciclo de vida de la aplicación](https://msdn.microsoft.com/library/windows/apps/mt243287).

## <a name="background-audio"></a>Audio en segundo plano


Para la propiedad [**MediaElement.AudioCategory**](https://msdn.microsoft.com/library/windows/apps/br227352) , **ForegroundOnlyMedia** y **BackgroundCapableMedia** están en desuso para las aplicaciones de Windows 10. En su lugar, usa el modelo de aplicaciones de la Tienda de Windows Phone. Para más información, consulta [Audio en segundo plano](https://msdn.microsoft.com/library/windows/apps/mt282140).

## <a name="detecting-the-platform-your-app-is-running-on"></a>Detección de la plataforma en la que se está ejecutando la aplicación


La forma de concebir destinada a la aplicación cambia con Windows 10. El nuevo modelo conceptual consiste en que una aplicación tiene como objetivo la Plataforma universal de Windows (UWP) y se ejecuta en todos los dispositivos Windows. A continuación, puede optar por dar a conocer funciones exclusivas de familias de dispositivos en particular. Si es necesario, la aplicación también tiene la opción de limitarse a orientarse a una o más familias de dispositivos específicamente. Para obtener más información sobre qué son las familias de dispositivos —y cómo decidir qué familia de dispositivos establecer como destino— consulta la [Guía de aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn894631).

Si tienes código en tu aplicación Universal 8.1 que detecta el sistema operativo en el que se está ejecutando, es posible que necesites cambiarlo en función del motivo de la lógica. Si la aplicación está pasando el valor y no actúa en él, podrás continuar recopilando la información del sistema operativo.

**Nota**  te recomendamos que uses no del sistema operativo o la familia de dispositivos para detectar la presencia de características. Identificar el sistema operativo actual o la familia de dispositivos normalmente no es la mejor manera de determinar si una función concreta de la familia de dispositivos o del sistema operativo está presente. En lugar de detectar el sistema operativo o la familia de dispositivos (y el número de versión), prueba la presencia de la propia funcion (consulta [Compilación condicional y código adaptable](w8x-to-uwp-porting-to-a-uwp-project.md)). Si debes requerir un sistema operativo determinado o una familia de dispositivos, asegúrate de usarlo como una versión compatible mínima, en lugar de diseñar la prueba solamente para esa versión.

 

Para adaptar la interfaz de usuario de la aplicación a diferentes dispositivos, hay varias técnicas que recomendamos. Continúa usando elementos de tamaño automático y paneles de diseño dinámico como siempre. En el marcado XAML, sigue usando tamaños en píxeles funcionales (anteriormente denominados píxeles de vista) para que la interfaz de usuario se adapte a diferentes resoluciones y factores de escala (consulta [Píxeles funcionales, distancia de visualización y factores de escala](w8x-to-uwp-porting-xaml-and-ui.md)). Y usa los desencadenadores y establecedores adaptables de Visual State Manager para adaptar la interfaz de usuario al tamaño de la ventana (consulta [Guía para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn894631)).

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

Consulta también [Compilación condicional y código adaptable](w8x-to-uwp-porting-to-a-uwp-project.md).

## <a name="location"></a>Ubicación


Cuando una aplicación que declara la funcionalidad de ubicación en su manifiesto del paquete de aplicación se ejecuta en Windows 10, el sistema pedirá al usuario final su consentimiento. Esto es true si la aplicación es una aplicación de Windows Phone Store o una aplicación de Windows 10. Por lo tanto, si la aplicación muestra su propia petición de consentimiento personalizado o si proporciona una alternancia de activar y desactivar, es aconsejable quitarla para que solo se le solicite una vez al usuario final.

 

 




