---
author: msatranjr
Description: This topic describes performance guidelines for apps that require access to a user's location.
title: Directrices para las aplicaciones con reconocimiento de ubicación
ms.assetid: 16294DD6-5D12-4062-850A-DB5837696B4D
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, ubicación, location, mapa, map, ubicación geográfica, geolocation
ms.localizationpriority: medium
ms.openlocfilehash: d0101124febc52da379d2e829e86bdbba7583851
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7162927"
---
# <a name="guidelines-for-location-aware-apps"></a>Directrices para las aplicaciones con reconocimiento de ubicación




**API importantes**

-   [**Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603)
-   [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534)

En este tema se describen las directrices de rendimiento para las aplicaciones que necesitan acceder a la ubicación del usuario.

## <a name="recommendations"></a>Recomendaciones


-   Empieza a usar el objeto de ubicación solo cuando la aplicación necesite datos de ubicación.

    Llama a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) antes de acceder a la ubicación del usuario. En ese momento, la aplicación debe estar en primer plano y se debe llamar a **RequestAccessAsync** desde el subproceso de la interfaz de usuario. La aplicación no puede acceder a los datos de ubicación hasta que el usuario conceda permiso para que la aplicación obtenga su ubicación.

-   Si la ubicación no es fundamental para la aplicación, no accedas a ella hasta que el usuario intente una tarea que así lo exija. Por ejemplo, si una aplicación de redes sociales tiene un botón para la función “Registrar mi ubicación”, la aplicación no debe acceder a la ubicación hasta que el usuario haga clic en el botón. Sin embargo, es aceptable acceder de inmediato a la ubicación si es necesario para el funcionamiento principal de la aplicación.

-   El primer uso del objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) debe realizarse en el subproceso de la interfaz de usuario principal de la aplicación en primer plano, para desencadenar la petición de consentimiento al usuario. El primer uso de **Geolocator** puede ser la primera llamada a [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) o el primer registro de un controlador para el evento [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540).

-   Indícale al usuario cómo se usarán los datos de ubicación.
-   Incluye elementos en la interfaz de usuario para que los usuarios puedan actualizar manualmente su ubicación.
-   Muestra una barra o un círculo de progreso mientras se espera a que se obtengan los datos de ubicación. <!--For info on the available progress controls and how to use them, see [**Guidelines for progress controls**](guidelines-and-checklist-for-progress-controls.md).-->
-   Muestra cuadros de diálogo o mensajes de error cuando los servicios de ubicación estén deshabilitados o no se encuentren disponibles.

    Si la configuración de ubicación no permite que la aplicación acceda a la ubicación del usuario, es recomendable que proporciones un vínculo práctico a la **configuración de privacidad de ubicación** en la aplicación **Configuración**. Por ejemplo, podrías usar un control de hipervínculo o llamar al método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar la aplicación **Configuración** desde el código mediante el URI `ms-settings:privacy-location`. Para más información, consulta [Iniciar la aplicación Configuración de Windows](https://msdn.microsoft.com/library/windows/apps/mt228342).

-   Borra los datos de ubicación almacenados en caché y libera el objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) cuando el usuario deshabilite el acceso a la información de ubicación.

    Libera el objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) si el usuario desactiva el acceso a la información de ubicación a través de Configuración. La aplicación recibirá entonces resultados **ACCESS\_DENIED** para las llamadas a la API de ubicación. Si la aplicación guarda datos de ubicación o los almacena en caché, borra todos los datos de la memoria caché cuando el usuario revoque el acceso a la información de ubicación. Proporciona un modo alternativo para introducir manualmente información de ubicación cuando los datos de ubicación no estén disponibles a través de servicios de localización.

-   Proporciona una interfaz de usuario para volver a habilitar los servicios de localización. Por ejemplo, proporcionar un botón de actualización que vuelva el objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) e intenta obtener información de ubicación de nuevo.

    La aplicación debe proporcionar una interfaz de usuario que permita volver a habilitar los servicios de ubicación:

    -   Si el usuario vuelve a habilitar el acceso a la ubicación tras deshabilitarlo, la aplicación no recibirá ninguna notificación. La propiedad [**status**](https://msdn.microsoft.com/library/windows/apps/br225601) no cambia y no se produce un evento [**statusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542). La aplicación debe crear un nuevo objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) y llamar a [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) para intentar obtener datos de ubicación actualizados o suscribirse de nuevo a eventos [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540). Si el estado indica que la ubicación se ha vuelto a habilitar, borra todos los elementos de la interfaz de usuario que la aplicación haya usado para informar al usuario de que los servicios de ubicación estaban deshabilitados y responde correctamente al nuevo estado.
    -   La aplicación también debe intentar volver a obtener datos de ubicación al activarse o cuando el usuario intente usar explícitamente una funcionalidad que requiera información de la ubicación, o en cualquier otro momento adecuado para el escenario.

**Rendimiento**

-   Si la aplicación no necesita recibir actualizaciones de la ubicación, haz peticiones de ubicación únicas. Por ejemplo, una aplicación que agrega una etiqueta de ubicación a una foto no necesita recibir eventos de actualización de la ubicación. En lugar de ello, la aplicación debe solicitar la ubicación con el método [**getGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536), como se describe en [Obtener la ubicación actual](https://msdn.microsoft.com/library/windows/apps/mt219698).

    Si se realiza una solicitud única de ubicación, se deben establecer los siguientes valores.

    -   Especifica la precisión que solicita la aplicación estableciendo el valor de [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) o [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271). Consulta las recomendaciones que se indican más adelante sobre el uso de estos parámetros.
    -   Establece el parámetro de edad máxima de [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) para especificar el límite máximo de antigüedad que puede tener una ubicación para que sea útil para la aplicación. Si la aplicación puede usar una posición que se haya obtenido hace unos segundos o minutos, la aplicación puede recibir una posición de manera casi inmediata y contribuir a reducir el consumo de energía del dispositivo.
    -   Establece el parámetro de tiempo de espera de [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536). De esta forma se define el tiempo que la aplicación puede esperar a que se devuelva una posición o un error. Tendrás que buscar el equilibrio entre la capacidad de respuesta al usuario y la precisión que necesita tu aplicación.
-   Usa una sesión de ubicación continua cuando se requieran actualizaciones frecuentes de posición. Usa eventos [**positionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) y [**statusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) para detectar los movimientos que superan un umbral específico o para actualizaciones de ubicación continuas a medida que ocurren.

    Al solicitar actualizaciones de ubicación, es posible que quieras especificar la precisión solicitada por la aplicación estableciendo el valor de [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) o [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271). También deberías establecer la frecuencia a la que se necesitan actualizaciones de ubicación mediante el uso de [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539) o [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541).

    -   Especifica el umbral de movimiento. Algunas aplicaciones solo necesitan actualizaciones de la ubicación cuando el usuario se ha desplazado una gran distancia. Por ejemplo, es posible que una aplicación que proporciona noticias locales o actualizaciones meteorológicas no necesite actualizaciones de la ubicación a menos que la ubicación del usuario haya cambiado a otra ciudad. En este caso, debes ajustar el movimiento mínimo requerido para un evento de actualización de la ubicación. Para ello, establece la propiedad [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539). Esta acción filtra los eventos [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540). Estos eventos solo se generan cuando un cambio de posición supera el umbral de movimiento.

    -   Usa un valor de [**reportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541) que se alinee con la experiencia de la aplicación y reduzca el uso de los recursos del sistema. Por ejemplo, es posible que una aplicación de información meteorológica solo requiera una actualización de los datos cada 15minutos. La mayoría de aplicaciones, excepto las de navegación en tiempo real, no requiere una transmisión constante de alta precisión de actualizaciones de la ubicación. Si la aplicación no requiere el flujo de datos más preciso posible o si rara vez requiere actualizaciones, establece la propiedad **ReportInterval** para indicar la frecuencia mínima de actualizaciones de ubicación que necesita la aplicación. El origen de la ubicación puede ahorrar energía si solo calcula la ubicación cuando es necesario.

        Las aplicaciones que sí necesitan datos en tiempo real deben establecer [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541) en 0, para indicar que no se especifica ningún intervalo mínimo. El intervalo predeterminado de informe es de 1 segundo o de la frecuencia que el hardware admita, lo que sea más corto.

        Los dispositivos que proporcionan datos de ubicación pueden realizar un seguimiento del intervalo de informe solicitado por distintas aplicaciones y proporcionar informes de datos según el intervalo solicitado más pequeño. La aplicación con mayor necesidad de precisión recibirá los datos que necesita. Por tanto, es posible que el proveedor de ubicación genere actualizaciones con una frecuencia superior a la solicitada por la aplicación si otra aplicación ha solicitado actualizaciones más frecuentes.

        **Nota**no se garantiza que el origen de la ubicación atenderá la solicitud para el intervalo de informe indicado. No todos los dispositivos de proveedores de ubicación hacen un seguimiento del intervalo de informe, pero lo debes proporcionar para los que sí lo hacen.

    -   Para ahorrar energía, establece la propiedad [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) para indicar a la plataforma de ubicación si la aplicación necesita datos de alta precisión. En caso de que no haya ninguna aplicación que necesite datos de alta precisión, el sistema puede ahorrar energía si no activa los proveedoresGPS.

        -   Establece [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) en **HIGH** para permitir que el GPS pueda adquirir datos.
        -   Si la aplicación únicamente usa la información de ubicación para personalizar la publicidad, establece [**desiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535) en **Default** y usa solamente un patrón de llamada de un único intento para minimizar el consumo de energía.

        Si la aplicación tiene unos requisitos de precisión determinados, quizá quieras usar la propiedad [**DesiredAccuracyInMeters**](https://msdn.microsoft.com/library/windows/apps/jj635271) en lugar de [**DesiredAccuracy**](https://msdn.microsoft.com/library/windows/apps/br225535). Esto es especialmente útil en Windows Phone, donde la posición se puede obtener habitualmente basándose en repetidores móviles, repetidores Wi-Fi y satélites. Si se elige un valor de precisión más específico, se ayudará al sistema a identificar las tecnologías adecuadas que debe usar para minimizar el consumo de energía al proporcionar una posición.

        Por ejemplo:

        -   Si tu aplicación obtiene la ubicación para mostrar anuncios personalizados, información meteorológica, noticias, etc., por lo general, basta con una precisión de 5000 metros.
        -   Si la aplicación muestra ofertas cercanas en el entorno, es buena por lo general, proporcionar resultados de una precisión de 300 metros.
        -   Si el usuario quiere ver los restaurantes cercanos recomendados, deberíamos obtener una posición en la manzana en la que se encuentre, por lo que una precisión de 100 metros es suficiente.
        -   Si el usuario intenta compartir su posición, la aplicación debería solicitar una precisión de unos 10 metros.
    -   Usa la propiedad [**Geocoordinate.accuracy**](https://msdn.microsoft.com/library/windows/apps/br225526) si la aplicación tiene requisitos de precisión específicos. Por ejemplo, las aplicaciones de navegación deberían usar la propiedad **Geocoordinate.accuracy** para determinar si los datos de ubicación disponibles cumplen los requisitos de la aplicación.

-   Ten en cuenta el retraso de inicio. La primera vez que una aplicación pide datos de ubicación, es posible que haya un breve retraso (1-2 segundos) mientras se inicia el proveedor de ubicación. Tenlo en cuenta a la hora de diseñar la interfaz de usuario de tu aplicación. Por ejemplo, tal vez quieras bloquear otras tareas que estén pendientes de la finalización de la llamada a [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536).

-   Ten en cuenta el comportamiento en segundo plano Si la aplicación no tiene el foco, no recibirá eventos de actualización de la ubicación mientras esté suspendida en segundo plano. Ten esto en cuenta si tu aplicación hace el seguimiento de las actualizaciones de ubicación registrándolas. Cuando la aplicación vuelve a obtener el foco, recibe únicamente los eventos nuevos, No obtiene ninguna actualización que haya ocurrido mientras estaba inactiva.

-   Usa sensores raw y de fusión de forma eficiente. Hay dos tipos de sensores: *raw* y *de fusión*.

    -   Los sensores raw incluyen el acelerómetro, girómetro y magnetómetro.
    -   Los sensores de fusión incluyen orientación, inclinómetro y brújula. Los sensores de fusión obtienen sus datos a partir de combinaciones de sensores raw.

    El RuntimeAPIs de Windows pueden tener acceso a todos estos sensores, excepto al magnetómetro. Los sensores de fusión son más precisos y estables que los físicos, pero consumen más energía. Usa los sensores adecuados para cada propósito. Para obtener información, consulta [Sensores](https://msdn.microsoft.com/library/windows/apps/mt187358).

**Modo de espera conectado**
- Cuando el equipo se encuentra en estado de modo de espera conectado, siempre se puede crear una instancia de los objetos [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534). No obstante, el objeto **Geolocator** no encontrará ningún sensor que pueda agregar, por lo que las llamadas a [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) agotarán el tiempo de espera después de 7 segundos, no se llamará nunca a las escuchas de eventos [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) y se llamará una vez a las escuchas de eventos [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) con el estado **NoData**.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales


### <a name="detecting-changes-in-location-settings"></a>Detección de cambios en la configuración de ubicación

El usuario puede desactivar la funcionalidad de ubicación mediante la **configuración de privacidad de ubicación** de la aplicación **Configuración**.

-   Para detectar si el usuario deshabilita o vuelve a habilitar los servicios de ubicación:
    -   Controla el evento [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542). La propiedad [**Status**](https://msdn.microsoft.com/library/windows/apps/br225601) del argumento para el evento **StatusChanged** tiene el valor **Disabled** si el usuario desactiva los servicios de ubicación.
    -   Comprueba los códigos de error que devuelve [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536). Si el usuario ha deshabilitado los servicios de ubicación, las llamadas a **GetGeopositionAsync** producirán un error **ACCESS\_DENIED** y la propiedad [**LocationStatus**](https://msdn.microsoft.com/library/windows/apps/br225538) tendrá el valor **Disabled**.
-   Si tienes una aplicación para la cual son esenciales los datos de ubicación (por ejemplo, una aplicación de mapas), asegúrate de hacer lo siguiente:
    -   Controla el evento [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) para obtener actualizaciones si la ubicación del usuario cambia.
    -   Controla el evento [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) del modo descrito anteriormente para detectar los cambios en la configuración de ubicación.

Ten en cuenta que el servicio de ubicación devolverá datos en cuanto estén disponibles. Primero podría devolver una ubicación con un mayor radio de error y después actualizar la ubicación con información más precisa en cuanto esté disponible. Las aplicaciones que muestran la ubicación del usuario normalmente actualizan la ubicación a medida que llega una información más precisa.

### <a name="graphical-representations-of-location"></a>Representación gráfica de la ubicación

La aplicación debe usar [**Geocoordinate.accuracy**](https://msdn.microsoft.com/library/windows/apps/br225526) para indicar claramente la ubicación actual del usuario en el mapa. Existen tres bandas principales de precisión: un radio de error de aproximadamente 10 metros, un radio de error de aproximadamente 100 metros y un radio de error superior a 1 kilómetro. Con el uso de la información de precisión, puedes asegurarte de que tu aplicación muestre la ubicación de forma precisa en el contexto de los datos disponibles. Para obtener información general sobre cómo usar el control de mapa, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](https://msdn.microsoft.com/library/windows/apps/mt219695).

-   Para una precisión de aproximadamente 10 metros (resolución de GPS), la ubicación se puede indicar por medio de un punto o un icono de alfiler en el mapa. Con esta precisión, también se pueden mostrar coordenadas con latitud y longitud, y direcciones de calles.

    ![ejemplo de un mapa que se muestra con una precisión gps de aproximadamente 10 metros.](images/10metererrorradius.png)

-   Para una precisión de entre 10 y 500 metros, (aproximadamente 100 metros), la ubicación se suele recibir mediante resolución Wi-Fi. La ubicación obtenida del teléfono móvil tiene una precisión de unos 300 metros. En tal caso, recomendamos que la aplicación muestre un radio de error. Para aplicaciones que muestren direcciones para las que sea necesario un punto para centrar, se puede mostrar tal punto con un radio de error alrededor.

    ![ejemplo de un mapa que se muestra con una precisión wi-fi de aproximadamente 100 metros.](images/100metererrorradius.png)

-   Si la precisión devuelta es superior a 1 kilómetro, es probable que estés recibiendo la información de la ubicación en resolución de nivel IP. Este nivel de precisión suele ser demasiado bajo para resaltar un punto determinado en un mapa. La aplicación debería acercarse hasta el nivel de ciudad en el mapa, o hasta el área adecuada según el radio de error (por ejemplo, el nivel de región).

    ![ejemplo de un mapa que se muestra con precisión wi-fi de aproximadamente 1 kilómetro.](images/1000metererrorradius.png)

Cuando la precisión de la ubicación cambia de una banda de precisión a otra, proporciona una transición suave entre las diferentes representaciones gráficas. Esto se puede hacer de la forma siguiente:

-   Haciendo que la animación de transición sea suave y la transición rápida y fluida.
-   Esperando unos cuantos informes consecutivos para confirmar el cambio en la precisión, para ayudar a evitar zooms no deseados y demasiado frecuentes.

### <a name="textual-representations-of-location"></a>Representación textual de la ubicación

Algunos tipos de aplicaciones (por ejemplo, una aplicación de información meteorológica o de información local) necesitan mecanismos para representar una ubicación de forma textual en las diferentes bandas de precisión. Asegúrate de mostrar la ubicación claramente y solamente en el nivel de precisión proporcionado por los datos.

-   Para una precisión de aproximadamente 10 metros (resolución de GPS), los datos de ubicación recibidos son bastante precisos y se pueden comunicar al nivel del nombre del barrio. También se puede usar el nombre de la ciudad, estado o provincia y país o región.
-   Para una precisión de aproximadamente 100 metros (resolución Wi-Fi), los datos de ubicación recibidos son moderadamente precisos y se recomienda que muestres información del nombre de la ciudad. Evita usar el nombre del barrio.
-   Para una precisión superior a 1 kilómetro (resolución de IP), muestra solo el nombre del estado o provincia, o del país o región.

### <a name="privacy-considerations"></a>Consideraciones de privacidad

La ubicación geográfica de un usuario es información de identificación personal (PII). En el sitio web siguiente se ofrecen directrices para proteger la privacidad de los usuarios.

-   [Privacidad de Microsoft]( http://go.microsoft.com/fwlink/p/?LinkId=259692)

<!--For more info, see [Guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md).-->

## <a name="related-topics"></a>Temas relacionados

* [Configurar una geovalla](https://msdn.microsoft.com/library/windows/apps/mt219702)
* [Obtener la ubicación actual](https://msdn.microsoft.com/library/windows/apps/mt219698)
* [Mostrar mapas con vistas 2D, 3D y Streetside](https://msdn.microsoft.com/library/windows/apps/mt219695)
<!--* [Design guidelines for privacy-aware apps](guidelines-for-enabling-sensitive-devices.md)-->
* [Ejemplo de ubicación de UWP (geolocation)](http://go.microsoft.com/fwlink/p/?linkid=533278)
 

 
