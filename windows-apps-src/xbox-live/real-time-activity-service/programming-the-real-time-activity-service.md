---
title: El servicio de la actividad en tiempo real de programación
description: Obtenga información sobre la programación de Xbox Live en tiempo real servicio de la actividad con las APIs de C++.
ms.assetid: 98cdcb1f-41d8-43db-98fc-6647755d3b17
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, actividad en tiempo real
ms.localizationpriority: medium
ms.openlocfilehash: f8846d57343f4f7262bbeea2cec03465fa23b2ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630000"
---
# <a name="programming-the-real-time-activity-service-using-c-apis"></a>El servicio de la actividad en tiempo real mediante C++ APIs de programación

En este artículo contiene las siguientes secciones

* Conectar con el servicio de la actividad en tiempo real de Xbox Live
* Desconecta el servicio de la actividad en tiempo real
* Creación de una estadística
* Suscribirse a una estadística de la actividad en tiempo real
* Cancelar la suscripción de una estadística de servicio de la actividad en tiempo real
* Muestra

## <a name="connecting-to-the-real-time-activity-service-from-xbox-live"></a>Conectar con el servicio de la actividad en tiempo real de Xbox Live

Las aplicaciones deben conectar al servicio de actividad en tiempo real (ATR) para obtener información de eventos de Xbox Live. En este tema se muestra cómo realizar esa conexión.

> [!NOTE]
> Los ejemplos usados en este tema indican las llamadas de método para un usuario. Sin embargo, el título debe realizar estas llamadas para todos los usuarios para conectarse a y desconectarse del servicio de la actividad en tiempo real (ATR).

### <a name="connecting-to-the-real-time-activity-service"></a>Conectar con el servicio de la actividad en tiempo real

```cpp
void Example_RealTimeActivity_ConnectAsync()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.

    // Note, only retrieve an XboxLiveContext for a given user *once*.  Otherwise you may encounter unpredictable behavior.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->activate();
}
```

### <a name="creating-a-statistic"></a>Creación de una estadística

Crear estadísticas en XDP si eres un desarrollador XDK o trabajar en un título de entre-play.  Crear estadísticas en el centro de partners si está realizando una UWP pura que se ejecuta en Windows 10.

#### <a name="xdk-developers"></a>Desarrolladores XDK

Para obtener información sobre cómo crear un stat en XDP, consulte el [XDP documentación](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_10_27_15_a.aspx#events).  Una vez haya creado su stat y define los eventos, deberá ejecutar el [XCETool](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/atoc_xce_jun15.aspx) para generar un encabezado de la aplicación.  Este encabezado contiene las funciones que puede llamar para enviar eventos que modifican las estadísticas.

#### <a name="uwp-developers"></a>Desarrolladores de UWP

Si está desarrollando una UWP en Windows 10 que no es un título de entre-play, definir las estadísticas en [centro de partners](https://partner.microsoft.com/dashboard). Leer el [artículo de configuración del centro de partners estadísticas](../leaderboards-and-stats-2017/player-stats-configure-2017.md) para obtener información sobre cómo configurar las estadísticas en el centro de partners.

> [!NOTE]
> Desarrollador de estadísticas 2013 tendrá que ponerse en contacto con su madre para información sobre [estadísticas 2013 configuración](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/windows-configure-stats-2013) en [centro de partners](https://partner.microsoft.com/dashboard).

### <a name="disconnecting-from-the-real-time-activity-service"></a>Desconectar servicio de la actividad en tiempo real

```cpp
void Example_RealTimeActivity_Disconnect()
{
    // Get the list of users from the system, and create an Xbox Live context from the first.
    // This user's authentication token will be used for the service requests.
    std::shared_ptr<xbox_live_context> xboxLiveContext = std::make_shared<xbox_live_context>(User::Users->GetAt(0));

    xboxLiveContext->real_time_activity_service()->deactivate();
}
```

## <a name="subscribing-to-a-statistic-from-the-real-time-activity"></a>Suscribirse a una estadística de la actividad en tiempo real

Las aplicaciones se suscriben a una actividad en tiempo real (ATR) para obtener las actualizaciones cuando cambian las estadísticas configuradas en Xbox Developer Portal (XDP) o el centro de partners.

### <a name="subscribing-to-a-statistic-from-the-real-time-activity-service"></a>Suscribirse a una estadística desde el servicio de la actividad en tiempo real

```cpp
void Example_RealTimeActivity_SubscribeToStatisticChangeAsync()
{
    // Obtain xboxLiveContext as shown in the connect function.  Then add a handler to be called on statistic changes.
    function_context statisticsChangeContext = xboxLiveContext->user_statistics_service().add_statistic_changed_handler(
        [](statistic_change_event_args args )
        {
            // Called on statistic change.  Inspect args to see which one.
            DebugPrint("%S %S", args.latest_statistic().statistic_name().c_str(), args.latest_statistic().value().c_str());
        }
    );

    // Call to subscribe to an individual statistic.  Once the subscription is complete, the handler will be called with the initial value of the statistic.
    auto statisticResults = xboxLiveContext->user_statistics_service().subscribe_to_statistic_change(
        User::Users->GetAt(0)->XboxUserId->Data(),
        L"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx",    // Get SCID from "Product Details" page in XDP or the Xbox Live Setup page in Partner Center
         L"YourStat"
        );

    std::shared_ptr<statistic_change_subscription> statisticsChangeSubscription;

    if(!statisticResults.err())
    {
        statisticsChangeSubscription = statisticResults.payload();
    }
    else
    {
        DebugPrint("Error calling SubscribeToStatistic: %S", statisticResults.err_message().c_str());
    }
}
```

## <a name="unsubscribing-from-a-statistic-from-the-real-time-activity-service"></a>Cancelar la suscripción de una estadística de servicio de la actividad en tiempo real

Aplicaciones suscriben a una estadística desde el servicio de la actividad en tiempo real (ATR) para obtener las actualizaciones cuando cambia la estadística. Cuando estas actualizaciones ya no son necesarios, se puede finalizar la suscripción y el código de este tema muestra cómo hacerlo.

### <a name="unsubscribing-from-a-real-time-services-statistic"></a>Cancelar la suscripción de una estadística de servicios en tiempo real

```cpp
void Example_RealTimeActivity_UnsubscribeFromStatisticChangeAsync()
{
    // statisticsChangeSubscription from the Example_RealTimeActivity_SubscribeToStatisticChangeAsync function.
    xboxLiveContext->user_statistics_service().unsubscribe_from_statistic_change(
        statisticsChangeSubscription
        );
}
```

> [!IMPORTANT]
> La actividad en tiempo real se desconectará el servicio después de dos horas de uso, el código debe ser capaz de detectar esto y volver a establecer una conexión con el servicio de la actividad en tiempo real si sigue siendo necesario. Esto se hace principalmente para garantizar que los tokens de autenticación se actualizan tras la expiración.
> 
> Si un cliente usa ATR para sesiones de varios jugadores y se desconecta de treinta segundos, el multijugador Directory(MPSD) sesión detecta que la sesión de ATR se cierra e inicia el usuario cierre la sesión. Tiene el cliente ATR para detectar cuándo se cierra la conexión e iniciar una reconexión y volver a suscribirse antes MPSD finaliza la sesión.